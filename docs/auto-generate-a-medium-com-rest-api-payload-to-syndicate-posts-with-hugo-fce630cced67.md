# 自动生成一个 Medium.com REST API 有效负载，与 Hugo 联合发布帖子

> 原文：<https://blog.devgenius.io/auto-generate-a-medium-com-rest-api-payload-to-syndicate-posts-with-hugo-fce630cced67?source=collection_archive---------24----------------------->

如果你想充分利用你写的每一篇文章，将你的内容整合到 Medium.com 可以帮助你让你的内容经常出现在更多的眼球前。当我写文章的时候，我通常会先把它们贴到我的网站上，然后把成品贴到 Medium.com。然后，我在 Medium.com 文章设置中提供了一个规范的 URL，这样搜索引擎就不会将这篇文章视为重复内容。

Medium 有一个自动导入工具，它会尝试从您的网站读取内容，并根据找到的内容自动创建一个 Medium.com 邮报。虽然这个工具有时有用，但它并不总是最好的。有时它完美地工作，有时它完全失败。正因为如此，我决定创造一种更简单的方法，用最少的努力和更少的错误把我的网站文章放到媒体上。

我的网站由 Hugo 提供支持，这是一个静态网站生成 CMS，它允许你拥有一个没有数据库或复杂托管计划的全功能网站。我的网站在 netlify.com 上免费托管，因为它是由 Hugo 建立的。使用 Hugo 强大的自定义输出格式功能，我能够轻松地创建 Medium.com 需要的准确信息。然后就像使用 Medium API 发布这些信息一样简单，瞧！你的网站帖子现在在 Medium.com 上，只需要很少的额外调整。所以，事不宜迟，让我们直接进入如何更新您的 Hugo 站点来创建这种输出格式并开始节省一些时间吧！

注意:即使你不使用 Hugo 作为你网站的内容管理系统，你也很有可能在你选择的内容管理系统上得到同样的结果。如果你不使用 Hugo，可以[跳到本文的后半部分](#two)。

## 修改我的 Hugo 站点以生成正确的 JSON 输出

## 步骤 1:在 Hugo 站点的配置文件中，添加新的自定义输出格式。

```
[outputFormats]
  [outputFormats.MediumJSON]
    baseName = 'medium'
    isPlainText = true
    mediaType = 'application/json'

[outputs]
  page = ['HTML', 'MediumJSON']
  section = ['HTML', 'MediumJSON']
```

在我的网站上，你可以在这里找到这个文件:[https://github . com/askcloudarchitech/askcloudarchitech/blob/master/asksitestatic/config . toml](https://github.com/askcloudarchitech/askcloudarchitech/blob/master/asksitestatic/config.toml)。如果你是 Hugo 的新手，这个文件包含了所有站点的全局设置和一些控制主题的设置。

通过添加配置的`[outputFormats]`片段，您可以指示 Hugo site generator 查找站点将用来生成必要输出的`mediumjson.json`布局文件。要了解更多关于 Hugo 自定义输出格式的信息，请访问 Hugo docs 网站[。名为`[outputs]`的第二部分指示 Hugo 为站点的页面和部分生成 mediumjson 输出。这就是我们在设置中所需要的，以使这个工作。](https://gohugo.io/templates/output-formats/)

## 步骤 2:将 ID 字段添加到您的帖子原型中

```
---
title: "{{ replace .Name "-" " " | title }}"
slug: ""
date: {{ .Date }}
ID: {{ .UniqueID }}
```

 [## askcloudarchitech/posts . MD at master askcloudarchitech/askcloudarchitech

### 在 GitHub 上创建一个帐户，为 askcloudarchitech/askcloudarchitech 开发做贡献。

github.com](https://github.com/askcloudarchitech/askcloudarchitech/blob/master/asksitestatic/archetypes/posts.md) 

这是本系列下一篇文章的铺垫。对于我网站上的每一篇文章，我想生成一个惟一的 ID 来永久地标识一篇特定的文章。这样，以后如果我想自动发布我所有的帖子到 Medium.com(提示提示)，程序可以跟踪 id，这样就不会两次发布同一篇文章。你不需要做这一步，但它需要大约 10 秒钟，所以它肯定不会伤害。

## 第 3 步:创建输出媒体文章帖子数据的布局模板

在进入这一步的操作部分之前，重要的是先看一下 [medium REST API 文档](https://github.com/Medium/medium-api-docs#33-posts)。这些文档清楚地概述了在你的请求正文中创建一篇关于 Medium.com 的新文章需要哪些帖子数据。这是文件中的例子:

```
POST /v1/users/5303d74c64f66366f00cb9b2a94f3251bf5/posts HTTP/1.1
Host: api.medium.com
Authorization: Bearer 181d415f34379af07b2c11d144dfbe35d
Content-Type: application/json
Accept: application/json
Accept-Charset: utf-8

{
  "title": "Liverpool FC",
  "contentFormat": "html",
  "content": "<h1>Liverpool FC</h1><p>You’ll never walk alone.</p>",
  "canonicalUrl": "http://jamietalbot.com/posts/liverpool-fc",
  "tags": ["football", "sport", "Liverpool"],
  "publishStatus": "public"
}
```

上面请求中的 JSON 主体是我试图为我网站的每篇文章复制的。要用 Hugo 做到这一点，您需要在`layouts`目录中创建一个新的布局，这并不复杂。如果你愿意，你可以在我网站的 git repo 中看到布局图。该文件的内容如下所示:

```
{
  "title": {{ .Title | jsonify }},
  "contentFormat": "html",
  "content": {{ printf "<h1>%s</h1> %s" .Title .Content | jsonify }},
  "canonicalUrl": {{ .Permalink | jsonify }},
  "tags": {{ .Params.tags | jsonify }}
}
```

如您所见，这与 Medium docs 中的示例具有相同的布局，但是值已经被 Hugo 模板数据所取代。此外，每个变量值都通过`jsonify`函数来确保它们被正确转义。这个文件应该被命名为`single.mediumjson.json`并放在您的`layouts/_default`目录中。这将确保为您网站上创建的每篇文章生成一个`medium.json`文件。

## 步骤 4:创建一个索引布局模板来获得所有文章的完整列表

网站的第二个也是最后一个布局文件是 Hugo `list`模板，它将创建引用每篇文章的索引。现在不会用到它，但是在本系列的第二部分中，当我们想要自动化整个过程时，它将非常重要。

再次在`layouts/_default`目录下，创建一个名为`list.mediumjson.json`的新文件，并将以下内容放入该文件。

```
[
    {{ range $i, $e := .RegularPagesRecursive -}}
    {{- if $i -}}, 
    {{ end -}}
    {
        "url": "{{ .Permalink }}medium.json",
        "id": "{{ if .Params.Id }}{{ .Params.Id }}{{end}}"
    }{{ end }}
]
```

该模板文件将遍历网站上的所有内容，列出所有文章的完整 URL 及其唯一 id。你可以在我网站的 GIT repo 中看到这个文件[。](https://github.com/askcloudarchitech/askcloudarchitech/blob/master/asksitestatic/layouts/_default/list.mediumjson.json)

## 第五步:发布你的改变并尝试它们

好的。剩下的就是试试了。你可以运行`hugo serve`或者将你的修改上传到你的网站上，然后进行测试。以下是我的网站上一些例子的链接。

索引页:[https://askcloudarchitech.com/posts/medium.json](https://askcloudarchitech.com/posts/medium.json)
文章页:[https://ascloudarchitech . com/posts/tutorials/home brew-setup-and-usage/medium . JSON](https://askcloudarchitech.com/posts/tutorials/homebrew-setup-and-usage/medium.json)

## 使用生成的 JSON 输出和 Postman，通过 Medium REST API 发布您的文章

正如本文开头提到的，如果你没有使用 Hugo 作为你的 CMS，你仍然可以使用你的博客平台或者 CMS 来达到同样的效果。只需设置您选择的 CMS 来生成以下文件:

1.  一个你的内容的 JSON 索引文件
    JSON 格式示例:[https://askcloudarchitech.com/posts/medium.json](https://askcloudarchitech.com/posts/medium.json)HTML 格式示例(仅供参考):[https://askcloudarchitech.com/posts](https://askcloudarchitech.com/posts)
2.  你的每篇文章的 JSON 输出为中等 POST 格式
    示例 JSON 格式:[https://askcloudarchitech . com/posts/tutorials/home brew-setup-and-usage/Medium . JSON](https://askcloudarchitech.com/posts/tutorials/homebrew-setup-and-usage/medium.json)
    示例 HTML 文章格式(仅供参考):[https://askcloudarchitech . com/posts/tutorials/home brew-setup-and-usage/](https://askcloudarchitech.com/posts/tutorials/homebrew-setup-and-usage/)

接下来的步骤将解释如何获取生成的数据，并通过几个简单的步骤轻松地将其发布到 medium.com。

## 步骤 1:生成 Medium.com 访问令牌

要使用 Medium.com API，您首先需要一个媒体访问令牌。邮递员将在下一步中使用这个令牌，这样它就可以代表您发送您的文章。只需按照这些步骤来获得您的令牌。

1.  前往[https://medium.com/me/settings](https://medium.com/me/settings)，然后点击“积分代币”
2.  输入令牌的简短描述，以便记住它的用途。像“中型自动发布工具”这样简单的工具就可以了。
3.  单击“获取集成令牌”,令牌将生成并打印在页面上。
4.  突出显示并复制令牌。暂时把它放在安全的地方。注意:不要共享这个令牌。

![](img/380979634d9bfba43b36897efb910028.png)

Medium.com 集成令牌屏幕

## 第二步:下载我预先制作的邮差合集，它可以为你做任何事情。

我没有给你一堆命令行 CURL 命令来完成所有这些工作，而是创建了一个 Postman 集合，它将三个必要的请求联系在一起，只需点击一个按钮就可以完成所有这些工作。

如果你不知道 Postman 是什么，它是一个工具，通常用于测试 API 或其他 web 请求，以查看它们是否符合你的需求。我重新调整了工具的用途，以便按顺序运行必要的 Medium.com API 请求，并收集必要的信息。

可以在这里下载邮差:[https://www.postman.com/downloads/](https://www.postman.com/downloads/)
可以在这里获取我的工作区:[https://www . Postman . com/morse code/workspace/askcloudarchitech](https://www.postman.com/morsecode/workspace/askcloudarchitech)

在我们开始使用它之前，让我先解释一下这个集合是做什么的。

1.  收集的第一步使用您的媒体集成令牌并连接到`User`端点以获得您的唯一用户 ID。它将此存储在一个变量中，以备将来使用。
2.  收集的第二步是连接到您的网站，并下载您想要发布到 Medium.com 的文章的 JSON 有效负载数据。这使用了您在本文前半部分创建的 JSON 信息。然后，它将检索到的数据存储到变量中，以备最后一步使用。
3.  收集的第三步是把所有的碎片放在一起。它使用您的集成令牌和您的 Medium.com 用户 ID 来连接到介质`posts` API 端点。它发送在第二步中收集的信息。如果一切顺利，它会得到一个`201`响应，表明帖子已经创建

注意:我已经设置了这个集合，只在 Medium.com 上创建草稿，所以不要担心自动公开张贴东西！

## 步骤 3:向 Postman 集合变量添加必要的值，并运行该集合

如果您已经成功下载了我的工作空间，您应该在 Postman 中看到如下内容:

![](img/e49a4266c9f798d6f580a1c6861c846e.png)

查看邮递员集合中的请求

要让这一切正常工作，您只需要填充两个变量。单击集合名称，然后单击变量选项卡。您将看到两个要填写的变量。它应该是这样的:

![](img/59246717bf7fec14a8628aed75cd1a6b.png)

要填充的邮递员集合变量

在`mediumToken`行的`Current Value`列，粘贴你的中集成令牌(截图中我已经隐藏了我的)。在`articleURL`行的`Current Value`列中，经过你的一篇文章的 JSON 媒体输出的完整 URL。如有必要，保存您的更改。

接下来，将鼠标悬停在 Postman 右栏中的收藏名称上，并单击三个点。选择“运行收集”选项。

![](img/bfad6ab599fe183d6685698b52bc8b4c.png)

带有运行收集选项的收集操作菜单

一个新的窗口将在左边打开，有一些选项。

![](img/4da246d54aa2cd985b3673e16b2c8169.png)

邮递员跑步收集按钮

在此屏幕上，确保所有三个请求都被选中，如屏幕截图所示，然后单击“Run Medium API POST”按钮。这将启动该流程。

完成后，您应该会看到一个屏幕，显示所有的请求都已运行。我一切顺利，最后一步将显示一个状态代码 201，表明文章已成功发布。

![](img/378014f96e270e6275265411f6c4d6cd.png)

收集运行后的输出

## 第四步:检查 Medium.com 的草稿，做最后的调整

现在这个已经完成了，去 https://medium.com/me/stories/drafts 的[看看你的草稿。您应该会看到类似这样的内容:](https://medium.com/me/stories/drafts)

![](img/995e8a91a9a605130d051086e98c032e.png)

Medium.com 草稿屏幕

有一些东西是 API 不支持的，所以请确保检查以下内容。

1.  如有必要，添加副标题
2.  添加搜索引擎优化标题
3.  检查您的标签是否正确应用
4.  校对你的内容，以确保它都是正确的
5.  发布您的草稿，以便公开发布

## 几个问题和下一步是什么

当我第一次设置这个过程时，我注意到我的图像被破坏了，并且一些链接不起作用。我发现我的 CMS 在网站上使用图像和链接的相对 URL。为了解决这个问题，我更新了我网站上的代码，以确保它总是使用完整的网址。这也可能是你的一个问题，所以要保持警惕。

正如我上面提到的，这是三部分系列的第一部分。在下一部分中，我将不再需要使用 Postman，而是用一个定制的程序来代替它。该计划将:

1.  跟踪已经发布到 Medium.com 的文章
2.  在终端中用一个简单的命令取代所有邮递员的东西
3.  消除了您提供想要发布的文章的 URL 的需要。使用索引列表，它会自动发现新的文章。

当然，我会将这个程序作为一个开源工具分享给所有人使用，并查看所有代码来展示它是如何工作的。