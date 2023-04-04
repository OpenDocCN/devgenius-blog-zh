# 如何知道 Maven 使用的是哪个版本的 Java

> 原文：<https://blog.devgenius.io/how-to-know-what-version-of-java-that-maven-is-using-3e6860c8a3ab?source=collection_archive---------1----------------------->

![](img/5cdf13f9d5bab13f2a0f0c693acf513e.png)

Juan Rumimpunu 在 [Unsplash](https://unsplash.com/s/photos/thinking?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

如果您正在排除故障，并且想知道您的 Maven 安装使用的是哪个版本的 Java，那么您会很高兴知道实际上有一个非常简单的命令可以告诉您。

# 只是答案

非常简单，您可以使用`mvn -v`、`mvn --version`或`mvn -version`不仅输出 Java 版本，还可以输出其他一些细节，比如 Java 安装在哪里以及操作系统信息，根据 [Sonatype](https://books.sonatype.com/mvnref-book/reference/running-sect-options.html#:~:text=To%20display%20Maven%20version%20information,%2Dv%2C%20%2D%2Dversion) :

```
$ mvn -v
Apache Maven 2.2.1 (r801777; 2009-08-06 14:16:01-0500)
Java version: 1.6.0_15
Java home: /System/Library/Frameworks/JavaVM.framework/Versions/1.6.0/Home
Default locale: en_US, platform encoding: MacRoman
OS name: "mac os x" version: "10.6.1" arch: "x86_64" Family: "mac"
```

# 想要显示版本信息而不停止构建吗？

在某些应用程序中(如 CI/CD 管道)，您可能希望在不完全停止构建的情况下显示版本信息。为此，只需运行命令`mvn -V`(因此带有大写的`V`标志)或`mvn --show-version`。

# 需要发行说明还是需要更新？

如果您需要更新您的 Maven 版本以获得某个特性，那么只需在[Apache Maven 官方网站上寻找您可以下载的版本列表](https://maven.apache.org/docs/history.html)。

此外，如果您需要的话，每个版本都有详细的发行说明和参考文档。

这篇文章是因为我需要弄清楚 Maven 使用的是哪个版本的 Java，但是我也看到了确切知道我使用的是哪个版本的 Maven 的效用。我希望通过这篇文章，你也能——如果你还不知道的话——记住这篇文章。

[](https://tremaineeto.medium.com/membership) [## 通过我的推荐链接加入媒体

### 作为一个媒体会员，你的会员费的一部分会给你阅读的作家，你可以完全接触到每一个故事…

tremaineeto.medium.com](https://tremaineeto.medium.com/membership)