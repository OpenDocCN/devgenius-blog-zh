# 使用 React and Go 优化服务器端呈现:第 2 部分—服务器应用程序编程

> 原文：<https://blog.devgenius.io/make-optimized-server-side-rendering-with-react-and-go-part-2-programming-server-application-eae03f834702?source=collection_archive---------3----------------------->

当我们谈到用 React 应用程序完成的 SSR 时，我们通常会考虑像 NextJS 或 NestJS 这样的框架。这种方法非常适合内容丰富的交互式网站。但是如果…

1.React 中已经内置了一个 SPA，只实现客户端渲染？

2.这是用 create-react-app 等第三方 React 模板工具创建的？

3.您不喜欢为服务器构建创建单独的 Webpack 配置吗？

4.你正在寻找一种便捷的渲染优化方法(不同的缓存策略取决于页面等)。)带轻量级运行时？

那么，你不是一个人！在这一系列文章中，我将向您解释我是如何为 handy SSR 找到自己的解决方案的。以下是该系列文章的链接:

链接到第 1 部分—[https://medium . com/@ jzx 777/make-optimized-server-side-rendering-with-react-and-go-Part-1-preparing-spa-68 D1 FBA 9512 e](https://medium.com/@jzx777/make-optimized-server-side-rendering-with-react-and-go-part-1-preparing-spa-68d1fba9512e)

链接至第 2 部分—此处

这部分是通过核心后端的开发过程来渲染网络应用。让我们继续我们的旅程。耶；3

1.  **首先，我们需要指定服务的关键特性**

*   它应该支持静态资源提取(如 CSS，JS，音频，图像等。)就像静态服务器一样；
*   它应该支持静态资源的持久缓存；
*   它应该有回退到基本的 HTML 模板，如果没有所需的页面匹配；
*   呈现的页面可能依赖于一些提取的数据；
*   如果数据获取失败，一些页面仍然应该给客户端:这也将是基本的 HTML 模板；
*   根据页面的种类，它应该支持最少一个或两个缓存策略:静态页面的最低要求是基于时钟的无效。依赖于外部内容的页面的最低要求是:基于时钟和基于 RPC 的失效；

**2。因此，让我们首先编写文件服务功能。真的很简单，长这样:**

```
package filecontroller

import (
 errors "go-frontend-service-template/utils/errors"
 files "go-frontend-service-template/utils/files"
 "io/ioutil"
 "net/http"
 "path/filepath"
)

type TFileController struct {
 uploadRoot string
}

var extensionToMime map[string]string = map[string]string{
 "png":  "image/png",
 "jpg":  "image/jpg",
 "gif":  "image/gif",
 "svg":  "image/svg+xml",
 "ico":  "image/x-icon",
 "mp3":  "audio/mpeg",
 "ogg":  "audio/ogg",
 "html": "text/html; charset=UTF-8",
 "js":   "application/javascript; charset=UTF-8",
 "css":  "text/css; charset=UTF-8",
 "json": "application/json",
 "txt":  "text/plain",
 "ttf":  "application/x-font-ttf",
 "otf":  "application/x-font-opentype",
}

func New(uploadRoot string) TFileController {
 return TFileController{
  uploadRoot: uploadRoot,
 }
}

func (self *TFileController) FetchFile(
 wrt http.ResponseWriter,
 req *http.Request,
) {
 path := req.URL.Path

 filePath := filepath.Join(
  self.uploadRoot,
  path,
 )
 extension := files.GetFileExtension(filePath)
 file, err := ioutil.ReadFile(filePath)
 if err != nil {
  errors.WriteErrToResponse(
   wrt,
   err,
   "FileController",
   "FetchFile",
  )
  return
 }

 mime, hasMime := extensionToMime[extension]
 if !hasMime {
  mime = "application/x-octet-stream"
 }

 wrt.Header().Add("Content-Type", mime)
 wrt.Header().Add("Cache-Control", "max-age=240000")
 wrt.WriteHeader(200)
 wrt.Write(file)
}
```

**3。接下来，我们将为页面渲染编写控制器。**

实现起来更复杂。它主要有两种类型的端点:

*   呈现由 Webpack 编译的默认 HTML 库。如在一些静态服务器中所做的那样；
*   用某种资源呈现我们想要的页面；

第一部分的代码是这样的:

```
package pagerendercontroller

import (
 "net/http"
)

type TPagesRenderController struct {
 htmlBase         string
}

func New(
 htmlBase string,
) *TPagesRenderController {
 return &TPagesRenderController{
  htmlBase:         htmlBase,
 }
}

func (self *TPagesRenderController) renderDefaultPageImpl(
 wrt http.ResponseWriter,
 req *http.Request,
) {
 wrt.Header().Add("Content-Type", "text/html")
 wrt.WriteHeader(200)
 wrt.Write([]byte(self.htmlBase))
}

func (self *TPagesRenderController) RenderDefaultPage(
 wrt http.ResponseWriter,
 req *http.Request,
) {
 self.renderDefaultPageImpl(wrt, req)
}
```

对于第二部分，我们应该考虑几个步骤来执行工作:

*   检查所需页面是否已经在缓存中。如果是这样——就退回去；
*   通过渲染的全过程为 SPA 的 SSR 端点准备参数:语言，然后资源，然后 URL 路径，然后查询参数；
*   为页面提取资源；
*   通过调用之前捆绑的 JS 代码导出的函数到 Go 应用中 JS 解释器的实例，获得前端状态补丁；
*   用和以前一样的方法渲染 HTML 和 CSS 标签(我使用样式化的组件所以我也需要它们)；
*   具有呈现状态补丁、HTML 和 CSS 标签的基于补丁的 HTML 模板；

让我们编写用于呈现的基本函数，该函数将由特定的页面呈现端点使用:

```
package pagerendercontroller

import (
 "encoding/json"
 "errors"
 "fmt"
 "go-frontend-service-template/services/jsinterpreter"
 pagescache "go-frontend-service-template/services/pages-cache"
 rpcservice "go-frontend-service-template/services/rpc-service"
 errorsutils "go-frontend-service-template/utils/errors"
 httpheadersutils "go-frontend-service-template/utils/http-headers"
 "log"
 "net/http"
 "strings"
 "time"
)

type TPagesRenderController struct {
 pagesCache       *pagescache.TPagesCache
 jsinterpreter    *jsinterpreter.TJsInterpreter
 htmlBase         string
 acceptLangParser *httpheadersutils.TAcceptLanguageParser
 rpcService       *rpcservice.TRpcService
}

func New(
 pagesCache *pagescache.TPagesCache,
 jsinterpreter *jsinterpreter.TJsInterpreter,
 htmlBase string,
 rpcService *rpcservice.TRpcService,
) *TPagesRenderController {
 return &TPagesRenderController{
  htmlBase:         htmlBase,
 }
}

func (self *TPagesRenderController) renderPage(
 wrt http.ResponseWriter,
 req *http.Request,
 pageName string,
 pageCacheDuration time.Duration,
 pageGetResource func() (interface{}, error),
) {
 pathWQuery := ""
 if len(req.URL.RawQuery) > 0 {
  pathWQuery = fmt.Sprintf(
   "%v?%v",
   req.URL.Path,
   req.URL.RawQuery,
  )
 } else {
  pathWQuery = req.URL.Path
 }

 cachedPage, hasCachedPage := self.pagesCache.GetPage(pathWQuery)
 if hasCachedPage {
  wrt.Header().Add("Content-Type", "text/html")
  wrt.WriteHeader(200)
  wrt.Write([]byte(cachedPage))
  return
 }

 acceptLanguage := req.Header.Get("Accept-Language")
 language := self.acceptLangParser.GetLanguageCode(acceptLanguage)
 pageSsrArgs := []interface{}{language}

 resource, err := self.getPackedResource(
  pageGetResource,
 )
 if err != nil {
  log.Println(
   fmt.Sprintf(
    "[%v/%v/ERROR-ON-FETCH-RESOURCE] => %v",
    "PageRenderController",
    fmt.Sprintf(
     "Page%vRender",
     pageName,
    ),
    err.Error(),
   ),
  )
  self.renderDefaultPageImpl(wrt, req)
  return
 }
 if len(resource) > 0 {
  pageSsrArgs = append(pageSsrArgs, resource)
 }

 statePatch, err := self.getStatePatch(pageName, pageSsrArgs)
 if err != nil {
  errorsutils.WriteErrToResponse(
   wrt,
   err,
   "PagesRenderController",
   fmt.Sprintf(
    "Render%vPage",
    pageName,
   ),
  )
  return
 }

 pageSsrArgsForPage := make([]interface{}, 0)
 for _, arg := range pageSsrArgs {
  pageSsrArgsForPage = append(pageSsrArgsForPage, arg)
 }
 pageSsrArgsForPage = append(pageSsrArgsForPage, req.URL.Path)
 if len(req.URL.RawQuery) > 0 {
  pageSsrArgsForPage = append(pageSsrArgsForPage, req.URL.RawQuery)
 }
 htmlCss, err := self.getRenderedHtmlCss(pageName, pageSsrArgsForPage)
 if err != nil {
  errorsutils.WriteErrToResponse(
   wrt,
   err,
   "PagesRenderController",
   fmt.Sprintf(
    "Render%vPage",
    pageName,
   ),
  )
  return
 }

 page := self.getRenderedPage(statePatch, htmlCss["html"], htmlCss["cssTags"])
 self.pagesCache.PutPage(
  pathWQuery,
  page,
  pageCacheDuration,
 )

 wrt.Header().Add("Content-Type", "text/html")
 wrt.WriteHeader(200)
 wrt.Write([]byte(page))
}

// ... already declared endpoint for base HTML rendering ...
```

“getPackedResource”的内部方法是这样的:

```
func (self *TPagesRenderController) getPackedResource(
 pageGetResource func() (interface{}, error),
) (string, error) {
 resource, err := pageGetResource()
 if err != nil {
  return "", err
 }
 if resource != nil {
  packedResource, err := json.Marshal(resource)
  if err != nil {
   return "", err
  }
  return string(packedResource), nil
 }

 return "", nil
}
```

(是的，字符串化资源也是处理向 JS 解释器调用的函数传递参数问题的一部分——如果我决定也缓存资源，我也会重构这个函数)

“getStatePatch”和“getRenderedHtmlCss”函数的实现方式如下:

```
func (self *TPagesRenderController) getStatePatch(
 pageName string,
 pageSsrArgs []interface{},
) (string, error) {
 statePatchFn := fmt.Sprintf(
  "ssr%vStatePatch",
  pageName,
 )
 statePatch, err := self.jsinterpreter.ExecExtFunction(
  statePatchFn,
  pageSsrArgs...,
 )
 if err != nil {
  return "", err
 }
 statePatchStr, isStatePatchStr := statePatch.(string)
 if !isStatePatchStr {
  return "", errors.New("state patch is not a string")
 }

 return statePatchStr, nil
}

func (self *TPagesRenderController) getRenderedHtmlCss(
 pageName string,
 pageSsrArgs []interface{},
) (map[string]string, error) {
 pageFn := fmt.Sprintf(
  "ssr%vPage",
  pageName,
 )
 rawPage, err := self.jsinterpreter.ExecExtFunction(
  pageFn,
  pageSsrArgs...,
 )
 if err != nil {
  return nil, err
 }
 page, isPageMap := rawPage.(map[string]interface{})
 if !isPageMap {
  return nil, errors.New("page is not an object with html & cssTags")
 }

 if _, hasHtml := page["html"]; !hasHtml {
  return nil, errors.New("page doesn't have 'html' string")
 }
 if _, isHtmlString := page["html"].(string); !isHtmlString {
  return nil, errors.New("page's 'html' is not a string")
 }

 if _, hasCssTags := page["cssTags"]; !hasCssTags {
  return nil, errors.New("page doesn't have 'cssTags' string")
 }
 if _, isCssTagsString := page["cssTags"].(string); !isCssTagsString {
  return nil, errors.New("page's 'cssTags' is not a string")
 }

 return map[string]string{
  "html":    page["html"].(string),
  "cssTags": page["cssTags"].(string),
 }, nil
}
```

缓存和交付页面之前的最后一步:

```
func (self *TPagesRenderController) getRenderedPage(
 statePatch string,
 renderedHtml string,
 renderedCss string,
) string {
 htmlWithPatch := strings.Replace(
  self.htmlBase,
  "<script id=\"state-patch-root\"></script>",
  statePatch,
  1,
 )
 html := strings.Replace(
  htmlWithPatch,
  "<div id=\"root\"></div>",
  fmt.Sprintf(
   "<div id=\"root\">%v</div>",
   renderedHtml,
  ),
  1,
 )
 fullHtml := strings.Replace(
  html,
  "<style id=\"sc-placeholder-root\"></style>",
  renderedCss,
  1,
 )

 return fullHtml
}
```

就是这样！然后，我们可以像这样利用端点中的核心功能:

```
func (self *TPagesRenderController) RenderExamplePageA(
 wrt http.ResponseWriter,
 req *http.Request,
) {
 getResource := func() (interface{}, error) {
  resource, err := self.rpcService.GetExampleResource()
  return resource, err
 }

 self.renderPage(
  wrt,
  req,
  "Example",
  time.Hour*3,
  getResource,
 )
}
```

**4。无效页面缓存**

如前所述，该服务支持缓存失效的两个关键策略:通过计时器和通过调用外部微服务可以使用的公开端点。要实现第一点，您应该运行无限循环 goroutine 来侦听滴答。要实现第二点，您应该从允许缓存删除的缓存实用程序中公开端点。让我们这样做:

```
package pagescache

import (
 lrucache "go-frontend-service-template/utils/lru_cache"
 "sync"
 "time"
)

type TPagesCacheEntry struct {
 Content   string
 ExpiredAt time.Time
}

type TPagesCache struct {
 pages     *lrucache.LruCache
 pagesLock *sync.Mutex
 timer     *time.Ticker
}

func New() *TPagesCache {
 cache := lrucache.New(10)
 return &TPagesCache{
  pages:     &cache,
  pagesLock: &sync.Mutex{},
  timer:     nil,
 }
}

func (self *TPagesCache) GetPage(pathWQuery string) (string, bool) {
 self.pagesLock.Lock()
 defer self.pagesLock.Unlock()

 page, hasPage := self.pages.Get(pathWQuery)
 if !hasPage {
  return "", false
 }

 pageEntry := page.(TPagesCacheEntry)
 return pageEntry.Content, true
}

func (self *TPagesCache) PutPage(
 pathWQuery string,
 pageContent string,
 pageDuration time.Duration,
) {
 self.pagesLock.Lock()
 defer self.pagesLock.Unlock()

 self.pages.Put(
  pathWQuery,
  TPagesCacheEntry{
   Content:   pageContent,
   ExpiredAt: time.Now().Add(pageDuration),
  },
 )
}

func (self *TPagesCache) InvalidatePage(pathWQuery string) {
 self.pagesLock.Lock()
 self.pages.Delete(pathWQuery)
 self.pagesLock.Unlock()
}

func (self *TPagesCache) InvalidatePages(pathsWQuery []string) {
 self.pagesLock.Lock()
 defer self.pagesLock.Unlock()

 for _, page := range pathsWQuery {
  self.pages.Delete(page)
 }
}

func (self *TPagesCache) RunTrashClock() {
 self.timer = time.NewTicker(time.Minute * 2)

 go func() {
  for {
   <-self.timer.C

   now := time.Now()

   self.pagesLock.Lock()
   pagesIter := self.pages.GetIterator()
   deletedPages := make([]string, 0)
   for pagesIter.HasNext() {
    kv := pagesIter.Next()
    pageEntry := kv.Val.(TPagesCacheEntry)
    if now.After(pageEntry.ExpiredAt) {
     deletedPages = append(deletedPages, kv.Key.(string))
    }
   }
   for _, dPage := range deletedPages {
    self.pages.Delete(dPage)
   }
   self.pagesLock.Unlock()
  }
 }()
}
```

(是的，缓存是基于 LRU 缓存策略的——这在缓存依赖于不同参数(如查询参数)的服务器渲染页面时特别有用)；

基地建成了。让我们为缓存无效编写控制器。这很简单:

```
package cacheinvalidatorcontroller

import (
 pagescache "go-frontend-service-template/services/pages-cache"
 "net/http"
)

type TCacheInvalidatorController struct {
 pagesCache   *pagescache.TPagesCache
 ipcSecretKey string
}

func New(
 pagesCache *pagescache.TPagesCache,
 ipcSecretKey string,
) *TCacheInvalidatorController {
 return &TCacheInvalidatorController{
  pagesCache:   pagesCache,
  ipcSecretKey: ipcSecretKey,
 }
}

func (self *TCacheInvalidatorController) InvalidateExamplePage(
 wrt http.ResponseWriter,
 req *http.Request,
) {
 ipcSecret := req.Header.Get("x-ipc-secret")
 if ipcSecret != self.ipcSecretKey {
  wrt.WriteHeader(403)
  wrt.Write([]byte("Not authorized"))
  return
 }

 self.pagesCache.InvalidatePage("/example")
 wrt.WriteHeader(200)
 wrt.Write([]byte("OK"))
}
```

**5。结合我们之前写的所有东西，在路由:**

```
package initjobs

import (
 // Services
 jsinterpreter "go-frontend-service-template/services/jsinterpreter"
 pagescache "go-frontend-service-template/services/pages-cache"
 rpcservice "go-frontend-service-template/services/rpc-service"

 // Controllers
 cacheinvalidatorcontroller "go-frontend-service-template/controllers/cache-invalidator-controller"
 filecontroller "go-frontend-service-template/controllers/file-controller"
 pagecontroller "go-frontend-service-template/controllers/page-render-controller"

 // Modules
 "net/http"

 mux "github.com/gorilla/mux"
 fileutils "go-frontend-service-template/utils/files"
)

func InitInternalRoutes(
 rootRouter *mux.Router,
 pagesCache *pagescache.TPagesCache,
 ipcSecretKey string,
) {
 cacheInvalidateController := cacheinvalidatorcontroller.New(
  pagesCache,
  ipcSecretKey,
 )

 internalRouter := rootRouter.PathPrefix("/internal").Subrouter()
 internalRouter.
  Methods("POST").
  Headers("x-ipc-secret").
  Path("/invalidate-example").
  HandlerFunc(
   func(rw http.ResponseWriter, r *http.Request) {
    cacheInvalidateController.InvalidateExamplePage(rw, r)
   },
  )
}

func InitSsrRoutes(
 rootRouter *mux.Router,
 frontendBuildPath string,
 pagesCache *pagescache.TPagesCache,
 jsInterpreter *jsinterpreter.TJsInterpreter,
 htmlBase string,
 rpcService *rpcservice.TRpcService,
) {
 fileController := filecontroller.New(frontendBuildPath)
 pageController := pagecontroller.New(
  pagesCache,
  jsInterpreter,
  htmlBase,
  rpcService,
 )

 frontendRouter := rootRouter.
  PathPrefix("/").
  Subrouter()
 frontendRouter.
  Methods("GET").
  Path("/pageA").
  HandlerFunc(
   func(rw http.ResponseWriter, r *http.Request) {
    pageController.RenderExamplePageA(rw, r)
   },
  )
 frontendRouter.
  Methods("GET").
  PathPrefix("/").
  HandlerFunc(
   func(rw http.ResponseWriter, r *http.Request) {
    // If index page is same as pageA
    if r.URL.Path == "/" {
     pageController.RenderExamplePageA(rw, r)
    } else {
     fileExtension := fileutils.GetFileExtension(r.URL.Path)
     if len(fileExtension) > 0 {
      fileController.FetchFile(rw, r)
     } else {
      pageController.RenderDefaultPage(rw, r)
     }
    }
   },
  )
}
```

最后一个前端路径可能看起来有点奇怪，但是它有明确的目的:如果你在索引路径上，你应该总是呈现一些页面。如果路径不是某个静态托管资源的路径，您应该回退到 HTML 基础。否则，您从前端构建发送一些静态资源；

正如我在上一篇文章中承诺的，这里是我开发的模板的源代码:【https://gitlab.com/jbyte777/go-frontent-service-template。还可以查看 FFI 用 JS 是怎么做的；

请根据您的经验等随意留下任何建议。我感谢任何有益的反馈；)

*我的 git lab:*[*https://gitlab.com/jbyte*](https://gitlab.com/john-byte)*777*