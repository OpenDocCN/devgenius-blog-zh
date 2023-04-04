# 用 HttpCookie 解析 Java 中的 cookie 字符串

> 原文：<https://blog.devgenius.io/parsing-cookie-strings-in-java-with-httpcookie-bb4c83b17142?source=collection_archive---------3----------------------->

![](img/482dea256f7688b53be75d1bae280a0a.png)

前几天，我正在解决一个非常复杂的 bug，涉及一些粘性会话 cookies 和多个反向代理。在解决 bug 的过程中，我发现我需要解析 set-cookie 头字符串，并在我们的一个反向代理中进行一些值过滤。

我的第一个想法是编写某种正则表达式来解析字符串并得到我想要的值。我选择了这样的方式:

```
(.*?)=(.*?)($|;|,(?! ))
```

这里有一个 [regexer 链接](https://regexr.com/62bdp)。

原来这比简单的正则表达式更复杂。一个字符串可以包含多个 cookie、可选参数等，那么就有一个关于 cookie 字符串的多种格式的问题。我需要围绕我的正则表达式写很多逻辑。

自然，我有点懒，所以我开始研究 Java 能提供什么。对此必须有一个现有的解决方案。我找到了一个名为`HttpCookie`的类。

用法非常简单:

```
List<HttpCookie> cookies = HttpCookie.parse(cookie);
```

它将把字符串中的所有 cookies 解析成一个包含所有需要信息的对象集合。

```
private final String name; // NAME= ... "$Name" style is reservedprivate String value; // value of NAME// Attributes encoded in the header's cookie fields.private String comment; // Comment=VALUE ... describes cookie's useprivate String commentURL; // CommentURL="http URL" ... describes cookie's useprivate boolean toDiscard; // Discard ... discard cookie unconditionallyprivate String domain; // Domain=VALUE ... domain that sees cookieprivate long maxAge = MAX_AGE_UNSPECIFIED; // Max-Age=VALUE ... cookies auto-expireprivate String path; // Path=VALUE ... URLs that see the cookieprivate String portlist; // Port[="portlist"] ... the port cookie may be returned toprivate boolean secure; // Secure ... e.g. use SSLprivate boolean httpOnly; // HttpOnly ... i.e. not accessible to scriptsprivate int version = 1; // Version=1 ... RFC 2965 style
```

这节省了我很多时间。

*最初发表于*[](https://ppolivka.com/posts/parsing-cookie-string-in-java-with-httpcookie)**。**