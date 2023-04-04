# 保护 react.js 应用程序的 13 种方法

> 原文：<https://blog.devgenius.io/13-ways-to-secure-your-react-js-application-d48da9eacd45?source=collection_archive---------3----------------------->

![](img/27ba760d2c4b2b01cfd3fc7cc10dec6e.png)

React 是一个免费的开源前端 javascript 库，用于构建用户界面。它可以用作单页 web /移动应用程序的样板。React 是一个结构良好的框架，用于在使用 JSX 语法的 HTML 页面中注入 javascript 代码。对于初学者来说，这是一个非常有用的框架，可以毫不费力地开发动态 UI。

今天，React 由于其额外的简单性和灵活性，已经成为一个非常流行的框架。据估计，超过 1，300，000 名开发人员和超过 1，020 万个站点使用 React.js。

如今，随着越来越多的数据被共享，与这些技术相关的风险也在增加。尽管 React 的风险比其他框架要少，但是一个小小的疏忽都会导致应用程序严重复杂化。React 有丰富的开源组件。然而，使用未经许可、很少使用的代码和不可信的来源会使您的应用程序容易出现安全漏洞。

> 预防胜于治疗。最好对常见的错误或漏洞采取预防措施，因为如果没有适当的安全特性，您的应用程序将毫无用处。

# 入门指南

让我们从 react 应用程序最常见的安全威胁开始。

## 1.跨站点脚本(XSS)

XSS 是一个严重的客户端漏洞，黑客能够在您的程序中添加一些恶意代码，这些代码被解释为有效代码，并作为应用程序的一部分执行。

## 2.SQL 注入

SQL 注入是一种代码注入技术，用于通过将恶意 SQL 查询插入输入字段来攻击数据库内容。它允许攻击者修改(读/写)数据，甚至破坏整个内容。

## 3.XML 外部实体攻击(XXE)

XXE 攻击是一种以 XML 解析器为目标的攻击。当使用配置不完善的 XML 解析器处理外部实体引用时，会发生这种情况，这可能会导致机密数据的泄露。

## 4.身份验证被破坏

身份验证在您的应用程序中起着至关重要的作用。尽管我们有双因素身份验证方法，但身份验证绕过或授权不充分/不佳会导致您的应用程序出现安全漏洞。这可能会将整个用户信息暴露给能够操纵这些信息的攻击者。

## 5.拉链条

Zip Slip 是一个存档提取漏洞，使得攻击者能够将任意文件写入系统，从而导致远程命令执行。

## 6.任意代码执行

任意代码执行是攻击者在目标机器上运行任意代码的能力。任意代码执行利用是攻击者运行的程序，使用远程代码执行方法来利用目标机器。

# 13 反应安全最佳实践

1.  [带数据绑定的默认 XSS 保护](#2629)
2.  [危险网址](#a838)
3.  [渲染 HTML](#904d)
4.  [直接 DOM 访问](#2a68)
5.  [服务器端渲染](#9967)
6.  [检测依赖关系中的漏洞](#fa10)
7.  [注入 JSON 状态](#b4e7)
8.  [从不序列化敏感数据](#aef1)
9.  [检测 React 的易受攻击版本](#fd54)
10.  [配置安全棉条](#7319)
11.  [避开危险库码](#a972)
12.  [实施网络应用防火墙(WAF)](#145f)
13.  [数据库连接的最小特权原则](#4825)

## 1.数据绑定的 XSS 保护

使用带花括号`{}`的数据绑定，React 将自动对值进行转义，以防止 XSS 攻击。然而，这种保护只有在呈现`textContent`和非 HTML 属性时才有帮助。

使用 JSX 数据绑定语法`{}`将数据放入元素中。

执行以下操作:

```
<div>{data}</div>
```

不要这样做:

```
<form action={data}> ...
```

## 2.危险的 URL

URL 可能包含动态脚本内容。因此，始终验证 URL，确保链接是`http:`或`https:`，以避免`javascript:`基于 URL 的脚本注入。使用本机 URL 解析功能验证 URL，并将解析的协议属性与允许列表匹配。

Dos:

```
function validateURL(url) {
  const parsed = new URL(url)
  return ['https:', 'http:'].includes(parsed.protocol)
}
<a href={validateURL(url) ? url : ''}>About</a>
```

不要:

```
<a href={url}>About</a>
```

## 3.呈现 HTML

我们可以使用`dangerouslySetInnerHTML`将 HTML 直接插入 DOM。这些物品必须事先消毒。在将这些值放入`dangerouslySetInnerHTML`属性之前，对它们使用净化库，如`dompurify`。

在将原生 HTML 代码注入 react DOM 时，尝试使用`dompurify`:

```
import purify from "dompurify";
<div dangerouslySetInnerHTML={{ __html:purify.sanitize(data) }} />
```

## 4.直接 DOM 访问

如果你必须注入 HTML，那么使用`dangerouslySetInnerHTML`并在注入组件之前使用`dompurify`净化它。使用`refs`、`findDomNode()`和`innerHTML`的直接 DOM 访问也使我们的应用程序易受攻击。因此，尽量避免使用这些方法，而使用`dangerouslySetInnerHTML`来达到这些目的。

Dos:

```
import purify from "dompurify";
const App = ({data}: Props) => {
 <div dangerouslySetInnerHTML={data} />
}<App data={__html:purify.sanitize(data)} />
```

不要:

```
this.myRef.current.innerHTML = "<div>Hacked</div>";
```

## 5.服务器端渲染

使用诸如`ReactDOMServer.renderToString()`和`ReactDOMServer.renderToStaticMarkup()`之类的服务器端呈现函数，在向客户端发送数据时提供内容转义。

在发送数据进行水合之前，将未初始化的数据与`renderToStaticMarkup()`输出结合在一起是不安全的。避免未初始化数据与`renderToStaticMarkup()`输出的串联。

不要:

```
app.get("/", function (req, res) {
  return res.send(
    ReactDOMServer.renderToStaticMarkup(
      React.createElement("h1", null, "Hello World!")
    ) + otherData
  );
});
```

## 6.检测依赖关系中的漏洞

在将依赖项导入到项目中之前，请始终检查依赖项的漏洞指数。它们可能有易受攻击的依赖项。因此，请尝试安装依赖项的稳定版本或漏洞数量较少的最新版本。

可以使用 [Snyk](https://www.npmjs.com/package/snyk) 等工具来分析漏洞。

在终端中使用以下命令在项目中运行 Snyk，

```
$ npx snyk test
```

## 7.注入 JSON 状态

JSON 和 XML 是在网络上传输数据的两种广泛使用的格式。但是，他们中的大多数更喜欢使用 JSON。此外，可以将 JSON 数据与服务器端呈现的 react 页面一起发送。所以，尽量用温和的值(Unicode 值)替换`<`字符，以防止注入攻击。

总是尝试将 JSON 中特定于 HTML 的代码替换为其 Unicode 等效字符:

```
window.__PRELOADED_STATE__ =   ${JSON.stringify(preloadedState).replace( /</g, '\\u003c')}
```

## 8.从不序列化敏感数据

我们经常用 JSON 值设置应用程序的初始状态。既然如此，`JSON.stringify()`是一个将任何数据转换成字符串的函数，即使它是易受攻击的。因此，它给了攻击者注入可以修改有效数据的恶意 JS 对象的自由。

```
<script>window.__STATE__ = ${JSON.stringify({ data })}</script>
```

## 9.检测 React 的易受攻击版本

与现在不同的是，React 在最初的版本中有一些很高的漏洞。因此，最好保持您的 react 版本最新，以避免使用易受攻击的`react`和`react-dom`版本。使用`npm audit`命令验证依赖漏洞。

## 10.配置安全过磅

通过集成 Linter 配置和插件，我们可以自动检测代码中的安全问题。它为我们提供安全问题的建议，并在存在漏洞时自动更新到新版本。使用 [Snyk ESLint 配置](https://github.com/snyk-labs/eslint-config-react-security/)来检测代码中的安全问题。

使用 [Snyk](https://docs.snyk.io/products/snyk-open-source) 查找并修复开源库中的漏洞，并扫描您的项目是否符合许可。

## 11.危险的图书馆代码

这个库代码经常用于执行危险的操作，比如直接将 HTML 插入 DOM。所以，避免使用`innerHTML`、`dangerouslySetInnerHTML` 或未经验证的 URL 的库。此外，配置 Linters 来检测 React 安全机制的不安全使用。

## 12.实施网络应用防火墙(WAF)

WAF 就像我们监控网络流量的 windows 防火墙。它能够通过分析网络流量来检测和阻止恶意内容。

我们可以在您应用程序中主要以三种方式实现 WAF:

1.  基于网络的硬件级防火墙
2.  软件级别的基于主机的防火墙(通过集成到应用程序中)
3.  基于云的

## 13.数据库连接的最小特权原则

将正确的数据库角色分配给应用程序中的不同用户非常重要。缺少用户角色定义可能会让攻击者在没有有效角色的情况下对数据库执行任何 CRUD 操作。

同样重要的是，除非非常重要，否则不要将应用程序数据库的管理权限授予任何人，以保持管理权限分配的最小化。这将保护您的应用程序并减少数据库攻击的机会。

感谢阅读这篇文章。

如果你喜欢这篇文章，请点击拍手按钮👏并且分享出来帮别人找！

【application.pdf 保护 react.js 的 13 种方法—

[https://drive . Google . com/file/d/1 stlo 23 bahv 9 vrllhcrxnaugxiyses 6 XJ/view？usp =分享](https://drive.google.com/file/d/1stlo23bAhV9vrLLhCrxnaugXIySES6Xj/view?usp=sharing)