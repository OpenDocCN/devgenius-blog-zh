# 如何在 Go 中生成 XML 站点地图

> 原文：<https://blog.devgenius.io/how-to-generate-xml-sitemaps-in-go-1d5a6249fc9d?source=collection_archive---------7----------------------->

![](img/b075aa68fae9e1a9016fd540b5c31627.png)

每个网站必不可少的一部分是它的**站点地图**，它看起来像一个简单的 XML 文件，可以使用简单的脚本来构建，虽然在 Wordpress 等内容管理系统中只需点击一下就很容易，但对于大型网站来说，这可能会变得很棘手，在这些网站中，您可能需要将几个站点地图文件合并到一个**站点地图索引**文件中，以获得一个与[站点地图协议](https://www.sitemaps.org/protocol.html)兼容的有组织的输出，该协议定义了要应用的大型站点地图的一些规则。

在这篇文章中，我将详细解释这些规则，以及它们的困难，此外还有使用强大的 **Golang 模块**帮助你轻松生成网站的站点地图的指导。

## 详细的站点地图模式协议限制

与所有其他 SEO 技术一样，Sitemap 有一个在构建过程中必须考虑的合同协议，你可以在上面提到的网站上看到。本文的重点是大型网站的 sitemap 文件规则和限制，这些网站可能有数千个不同类型的链接。

最重要的步骤是使用站点地图索引文件对多个站点地图文件进行分组，其中包括站点地图文件的 URL。主要的限制是每个文件中 *< url >* 标签的数量，这对于小型博客/应用网站来说似乎很高，但是对于大型博客/应用网站来说，这是一个很小的数量，因为在大型博客/应用网站中可能有更多的标签。

另一个限制是未压缩文件的大小必须不超过 50MB，这可能发生在长 URL 以及包含更多信息的带扩展名的网站地图[视频网站地图](https://developers.google.com/search/docs/advanced/sitemaps/video-sitemaps)。

解决方案是将它们分成几个文件，在站点地图索引文件中引用，如下例所示，其中显示了一个列出两个站点地图的站点地图索引:

```
<?xml version="1.0" encoding="UTF-8"?><[sitemapindex](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemapindex) > <[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)> <[loc](https://www.sitemaps.org/protocol.html#sitemapIndex_loc)>http://www.example.com/sitemap1.xml.gz</loc> <[lastmod](https://www.sitemaps.org/protocol.html#sitemapIndex_lastmod)>2004-10-01T18:23:17+00:00</lastmod> </sitemap> <[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)> <[loc](https://www.sitemaps.org/protocol.html#sitemapIndex_loc)>http://www.example.com/sitemap2.xml.gz</loc> <[lastmod](https://www.sitemaps.org/protocol.html#sitemapIndex_lastmod)>2005-01-01</lastmod> </sitemap></sitemapindex>
```

## Go 网站地图-生成器模块

为了忘记所有关于站点地图限制的考虑，以及你需要在你的网站中为不同的内容类型有单独的文件的情况，我建议你使用这个[高性能 Go 模块](https://github.com/sabloger/sitemap-generator)，它透明地处理所有需要的规则。

[](https://github.com/sabloger/sitemap-generator) [## GitHub-sabloger/Sitemap-generator:一个非常棒的 Go 网站地图生成器模块

### 一个高性能的站点地图生成器 Go 模块，它是一个创建和管理站点地图索引的综合工具

github.com](https://github.com/sabloger/sitemap-generator) 

这个库给了你一个简单而漂亮的 API 来使用一个强大的引擎完成你所需要的一切。它控制 URL 项目的数量以及最终的文件大小，并自动将它分成几个文件。

## 如何使用网站地图生成器

根据文档，您可以使用这一行`go get`将它添加到您的 go mod 项目中:

`go get github.com/sabloger/sitemap-generator`

下面的示例代码是一个使用`smg.NewSitemap()`将这个模块用于单个站点地图文件的例子:

```
package main

import (
  "fmt"
  "github.com/sabloger/sitemap-generator/smg"
  "log"
  "time"
)

func main() {
  now := time.Now().UTC()

  sm := smg.NewSitemap(true) // The argument is PrettyPrint which must be set on initializing
  sm.SetName("single_sitemap") // Optional - sets the file name without extension
  sm.SetHostname("https://www.example.com")
  sm.SetOutputPath("./some/path")
  sm.SetLastMod(&now)
  sm.SetCompress(false) // Default is true

  // Adding URL items
  err := sm.Add(&smg.SitemapLoc{
    Loc:        "some/uri.html",
    LastMod:    &now,
    ChangeFreq: smg.Always,
    Priority:   0.4,
  })
  if err != nil {
    log.Fatal("Unable to add SitemapLoc:", err)
  }

  // Save func saves the xml file and returns more than one filename in case of split large files.
  filenames, err := sm.Save()
  if err != nil {
    log.Fatal("Unable to Save Sitemap:", err)
  }
  for i, filename := range filenames {
    fmt.Println("file no.", i+1, filename)
  }
}
```

此外，您可以使用`smg.NewSitemapIndex()`来构建一个站点地图索引实例:

```
package main

import (
  "fmt"
  "github.com/sabloger/sitemap-generator/smg"
  "log"
  "time"
)

func main() {
  now := time.Now().UTC()

  smi := smg.NewSitemapIndex(true)
  smi.SetCompress(false)
  smi.SetSitemapIndexName("an_optional_name_for_sitemap_index")
  smi.SetHostname("https://www.example.com")
  smi.SetOutputPath("./sitemap_index_example/")
  smi.SetServerURI("/sitemaps/") // Optional

  smBlog := smi.NewSitemap()
  smBlog.SetName("blog_sitemap")
  smBlog.SetLastMod(&now)
  err := smBlog.Add(&smg.SitemapLoc{
    Loc:        "blog/post/1231",
    LastMod:    &now,
    ChangeFreq: smg.Weekly,
    Priority:   0.8,
  })
  if err != nil {
    log.Fatal("Unable to add SitemapLoc:", err)
  }

  smNews := smi.NewSitemap()
  smNews.SetLastMod(&now)
  err = smNews.Add(&smg.SitemapLoc{
    Loc:        "news/2021-01-05/a-news-page",
    LastMod:    &now,
    ChangeFreq: smg.Weekly,
    Priority:   1,
  })
  if err != nil {
    log.Fatal("Unable to add SitemapLoc:", err)
  }

  filename, err := smi.Save()
  if err != nil {
    log.Fatal("Unable to Save Sitemap:", err)
  }

  fmt.Println("sitemap_index file:", filename)
}
```

输出目录将如下所示:

```
sitemap_index_example
├── an_optional_name_for_sitemap_index.xml
├── blog_sitemap.xml
└── sitemap2.xml
```

`an_optional_name_for_sitemap_index.xml`将模样:

```
<?xml version="1.0" encoding="UTF-8"?>
<sitemapindex >
  <[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)>
    <loc>https:/www.example.com/sitemaps/blog_sitemap.xml</loc>
    <lastmod>2022-02-12T18:38:06.671183Z</lastmod>
  </[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)>
  <[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)>
    <loc>https:/www.example.com/sitemaps/sitemap2.xml</loc>
    <lastmod>2022-02-12T18:38:06.671183Z</lastmod>
  </[sitemap](https://www.sitemaps.org/protocol.html#sitemapIndex_sitemap)>
</sitemapindex>
```

## 结论

如今，拥有一个网站地图是必要的，但是当你不知道可用的工具时，这可能会很困难。我试图向您介绍一个有助于 Go 开发人员以高效的方式生成站点地图文件的软件包。与所有其他开源项目一样，这个库对贡献者开放，变得比现在有用得多。