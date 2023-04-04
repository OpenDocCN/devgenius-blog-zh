# 使用 Java 发送电子邮件

> 原文：<https://blog.devgenius.io/sending-an-email-using-java-b1f6863539ea?source=collection_archive---------6----------------------->

## 一个月前，在 Spring 项目中，作为实习生，我们被要求编写一个发送电子邮件的程序。在尝试了很多不同的方法后，我们终于找到了解决办法。来，让我们看看。

![](img/2a6849af0e5fbe82811b959f516e4f04.png)

好了，在进入实际的编码部分和导入 java 类之前，负责发送电子邮件。我们需要将这些类(JAR / API)添加到项目的类路径中。默认情况下，Java 不附带这些类。

在 **pom.xml** 文件中添加这个依赖项。并更新 maven 项目。

好了，要使用 Java 发送一封电子邮件，第一步是拥有一个发件人电子邮件 id (abraj306@gmail.com，以及一个收件人电子邮件 id (abhijeet.biencaps@gmail.com。

下一步是理解，每当我们通过互联网交换电子邮件时，它们都遵循一套称为 SMTP(简单邮件传输协议)的规则。当发件人点击发送电子邮件按钮。那封邮件先传到***SMTP***服务器，然后从那里到达收件人的收件箱。像 Gmail 或 yahoo 这样的电子邮件服务提供商有他们专属的 smtp 服务器。这些服务器一般格式化为**。(*比如 Gmail 的 *SMTP 服务器*地址是 *smtp* .gmail.com，Twilio SendGrid 的是*SMTP*. send grid . com .*)**

*下一步是将 smtp 主机或服务器的名称/地址存储在一个字符串中。(我们稍后将需要这个字符串)。我们在应用程序中使用 google smtp 服务器。所以每当我们的应用程序发送一封电子邮件。那封邮件会被转到谷歌的 smtp 服务器。此外，我们还应该让应用程序知道，发送电子邮件时应该点击 smtp 服务器的哪个端口。(如果是 google 的 smtp 服务器，应该会点击端口号 587)。*

*在我们发送电子邮件之前，我们必须验证发件人的电子邮件 id 和密码。将这些值存储在一个字符串变量中。*

*现在，Java 应用程序的每个运行实例都有一些[属性](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/Properties.html)。这些属性可以从[系统类](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/lang/System.html#getProperties%28%29)中检索。现在，我们只需设置该应用程序的属性，如 smtp 服务器地址、要访问的 smtp 端口、发送者的用户名和密码。属性 **"mail.smtp.auth"** 表示发送邮件前是否需要认证。在这种情况下，该属性被设置为 true，因为在从 g mail 帐户发送邮件之前，我们必须使用有效的用户名和密码进行身份验证。*

*现在另一个属性**“mail . SMTP . starttls . enable”**被设置为 true。但这意味着什么呢？嗯，当 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) 实现[安全](https://www.google.com/search?q=http+vs+https&client=ubuntu&hs=Exy&channel=fs&sxsrf=ALeKk01WW3TfcmsRTchaEaJcVrvpy6TmPg%3A1617116999804&ei=Rz9jYLDMMMP8rQGR4r2QAg&oq=http+vs+htt&gs_lcp=Cgdnd3Mtd2l6EAMYADIFCAAQsQMyAggAMgIIADICCAAyAggAMgIIADICCAAyAggAMgIIADICCAA6BwgjELADECc6BwgAELADEEM6BwgjEOoCECc6BAgjECc6BAgAEEM6BwgAELEDEEM6CAgAELEDEIMBOgcIABCHAhAUOgoIABCHAhCxAxAUUM4JWPEhYKAnaAJwAngAgAHoAogBgxSSAQcwLjcuNC4xmAEAoAEBqgEHZ3dzLXdperABCsgBCsABAQ&sclient=gws-wiz)的时候。它使用 SSL 或 TLS 作为加密手段。因此，我们基本上是在告诉我们的应用程序，电子邮件在网络上传输的整个过程(以及往返于 smtp 服务器)应该是 TLS 加密的。*

*现在，与 smtp 服务器的每一组通信都可以被视为一个会话。现在我们需要用预设的属性(如用户名、密码、smtp 服务器地址等)初始化一个会话。*

*一旦会话被初始化，我们将需要在其中传递一些消息。但是为什么要模仿信息和我的东西呢？在最初的日子里，邮件只能交换文本数据，没有照片，视频或文件。因此，MIME 是它的扩展，能够发送多媒体，如照片、视频等。(所以我们的消息是 MIME 类型的)。*

*每封电子邮件在通过网络发送之前都附有一些标题。该报头由发送方和接收方的地址组成。所以我们必须将这些头附加到 MimeMessage 实例中。*

*发件人电子邮件地址以字符串格式存在，不能直接附加到邮件头。为此，我们必须解析 [InternetAddress](http://geronimo.apache.org/maven/specs/geronimo-javamail_1.4_spec/1.6/apidocs/javax/mail/internet/InternetAddress.html) 格式的发件人电子邮件地址。然后使用 message.setFrom()方法将解析后的地址附加到头部。*

*类似地，我们将接收者的地址解析为 InternetAddress 格式，并将其传递给(MIME)消息的 addRecipient 方法。我们还可以提到，收件人是否添加在抄送(消息。RecipientType.CC)、密件抄送(Message。RecipientType.BBC)或者使用 TO(Message)将邮件直接发送给他。RecipientType.TO)值。接下来，我们设置电子邮件主题和电子邮件文本。*

*继续，我们最后使用[传输](https://docs.oracle.com/javaee/7/api/javax/mail/Transport.html)类来建立与 smtp 服务器的连接。当我们调用 session . get transport(“SMTP”)时。我们说我们正在使用 SMTP(协议)发送电子邮件。*

*接下来，我们通过将发送者的 smtp 服务器地址、用户名和密码传递给 connect 方法来连接到 smtp 服务器。完成后，我们将消息发送给所有收件人。*