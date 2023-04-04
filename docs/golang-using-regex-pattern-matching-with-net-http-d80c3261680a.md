# Golang:通过 net/http 使用正则表达式模式匹配

> 原文：<https://blog.devgenius.io/golang-using-regex-pattern-matching-with-net-http-d80c3261680a?source=collection_archive---------2----------------------->

众所周知，net/http 包功能强大且易于使用。很容易启动来服务 http urls。然而，URL 模式识别是基本的，因此程序员通常会求助于第三方包，例如 [Gorilla Mux](https://github.com/gorilla/mux) 来满足他们的路由需求。

但是，如果你在一个受约束的环境中，使用第三方依赖会受到限制，或者你只是想让你的代码简单。你能做的就是把它和正则表达式包(即 regex)和 net/http 包混在一起。

![](img/fb5ecbbd954179d301893e5fb1ba188f.png)

照片由 [Remotar 乔布斯](https://unsplash.com/@remotarjobs?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 技术

想法是创建自己的定期 expresssion 网址匹配器。您将确保匹配器(这是一个结构)实现了[处理程序接口](https://pkg.go.dev/net/http#Handler)。然后，这个结构将被用来提供与我们所熟悉的 net/http 设施相匹配的 URL。请继续阅读，了解更多信息。

```
package regexURLimport (
        "fmt"
        "net/http"
        "regexp"
)type RegexURLMatcher struct {
        Patterns map[string]*regexp.Regexp //1
        Handlers map[string]http.HandlerFunc //2
}func NewRegexURLMatcher() *RegexURLMatcher { //3
        return &RegexURLMatcher{
                Patterns: make(map[string]*regexp.Regexp),
                Handlers: make(map[string]http.HandlerFunc),
        }
}func (r *RegexURLMatcher) Add(regex string, handler   
        http.HandlerFunc) error { //4 compiled, err := regexp.Compile(regex) 
        if err != nil {
                return fmt.Errorf("Regex string cannot compile with err: %w", compiled)
        } r.Handlers[regex] = handler
        r.Patterns[regex] = compiled return nil
}func (r *RegexURLMatcher) ServeHTTP(res http.ResponseWriter, req *http.Request) { //5
        toMatchPattern := req.Method + " " + req.URL.Path
        for regexString, handlerFunc := range r.Handlers { //6
                if  
                r.Patterns[regexString].MatchString(toMatchPattern) 
                == true {
                        handlerFunc(res, req)
                        return
                }
        } http.NotFound(res, req)}
```

## 代码解释

RegexurlMatcher 结构有两个字符串映射——一个保存处理函数(//1 ),另一个(//2)存储编译后的正则表达式 URL 模式。NewRegexURLMatcher 函数(//3)将创建一个新的空 RegexURLMatcher 结构。

它有一个 Add 函数(//4)，允许您将一个 regex 字符串模式关联到一个特定的 [http。HandlerFunc](https://pkg.go.dev/net/http#HandlerFunc) 函数(记住 Golang 中的函数是一等公民)。将首先编译 regex 字符串模式，如果 regex 字符串无效，将引发一个错误。否则，如果一切顺利，编译后的模式和处理函数将被添加到 RegexURLMatcher 结构中的两个映射中。

如前所述，RegexURLMatcher struct 实现了 handler 接口，因此它必须具有 ServeHTTP 函数(//5)。在实现中，我们采用请求方法和 URL 路径来形成要测试的字符串。用什么测试？我们将枚举处理程序的映射来获取 regex 字符串(即键)和处理程序函数(//6)。每个正则表达式字符串将用于从模式映射中获取模式。将测试这个编译后的正则表达式模式，以匹配从请求中获得的字符串。如果匹配，将使用处理函数来处理请求。

## 怎么用？

让我们以 main.go 为例来看看如何使用我们新的 RegexURLMatcher 结构。

```
package mainimport (
        "fmt"
        "net/http"
        "regexURL"
        "strings"
)func main() {r := regexURL.NewRegexURLMatcher()
        r.Add("(GET) /greeting(/?[A-Za-z0-9]*)?", greeter)
        http.ListenAndServe(":8080", r) //7}func greeter(res http.ResponseWriter, req *http.Request) {//8 defer req.Body.Close() path := req.URL.Path
        segments := strings.Split(path, "/") name := "" if len(segments) > 2 {
                name = segments[2]
        } if name == "" {
                name = "user"
        } payload := fmt.Sprintf("Hello %s\n", name)
        res.Write([]byte(payload))}
```

在主程序中，我们创建了新的 spanning new matcher struct (/)并添加了我们想要匹配的 URL 正则表达式以及相关的处理函数，在本例中是我们的自定义欢迎函数(/)8。欢迎函数将尝试提取 URL 的第三段，例如[http://localhost:8080/greeting/Arnold governor](http://localhost:8080/greeting/ArnoldGovernor)，并向其问好。

我将把这个技巧留给你。记住 Golang 简洁有力。编码快乐！