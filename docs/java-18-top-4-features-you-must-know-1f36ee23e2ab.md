# Java 18:你必须知道的 4 大特性

> 原文：<https://blog.devgenius.io/java-18-top-4-features-you-must-know-1f36ee23e2ab?source=collection_archive---------5----------------------->

## 软件工程之旅

## Java 18:作为软件工程师你必须知道的 4 大特性

![](img/44ba6c8e77c89907196c806ee931652c.png)

www.techgeeknext.com 的照片

# 概观

Java 18 于 2022 年 3 月 22 日发布，这是继上一个长期支持(LTS)版本 Java 17 之后的第一个版本。你可以看看我的文章 [Java 17: 5 特性你必须知道](https://medium.com/@techisbeautiful/java-17-top-5-features-you-must-know-bbed2afaea3d)如果你错过了它。

在本教程中，我将分享最新的 Java 平台， [Java SE 18](https://www.oracle.com/java/technologies/javase/jdk18-archive-downloads.html) ，包括新的特性和变化。Java 18 于 2022 年 3 月 22 日全面上市。

首先，Java 18 中有许多 JDK 增强建议(jep)。我把它们按照 JEP 数字的顺序排列，你可以看到下面 Java 18 的列表:

*   400: [UTF-8 默认](https://openjdk.java.net/jeps/400)
*   408: [简单网络服务器](https://openjdk.java.net/jeps/408)
*   413:[Java API 文档中的代码片段](https://openjdk.java.net/jeps/413)
*   416: [用方法句柄重新实现核心反射](https://openjdk.java.net/jeps/416)
*   417: [载体 API(第三孵育器)](https://openjdk.java.net/jeps/417)
*   418: [互联网地址解析 SPI](https://openjdk.java.net/jeps/418)
*   419: [外来函数&内存 API(第二孵化器)](https://openjdk.java.net/jeps/419)
*   420: [切换模式匹配(第二次预览)](https://openjdk.java.net/jeps/420)
*   421: [反对最终确定移除](https://openjdk.java.net/jeps/421)

虽然我们在 Java 18 中有很多特性，但我将分享任何软件工程师在 Java 18 版本中必须知道的四大特性。让我们一起探索吧！

# JAVA 18 中你必须知道的 4 大特性

## #1:默认为 UTF8

我相信 Java 开发人员必须处理许多问题，因为 Java 字符集因操作系统(Linux、Window 等)而异。)和语言设置。用于读写文件的标准 Java APIs 作为参数传递的字符集。字符集控制着原始字节和 Java 编程语言的 16 位`char`值之间的转换。支持的字符集包括 US-ASCII、UTF-8 和 ISO-8859-1。

如果没有将字符集参数传递给函数，标准 Java APIs 通常会使用默认的字符集。JDK 在应用程序启动时根据运行时环境选择默认字符集:操作系统、用户的语言环境等。

让我们回顾一下这个例子，在这个例子中，我们有一个函数来编写文本“快乐编码！”日语，并在 Unix (Linux 或 MacOS)中运行该功能:

```
try (FileWriter fileWriter = new FileWriter("techisbeautiful.txt");             BufferedWriter bWriter = new BufferedWriter(fileWriter)) {   
    bWriter.write("ハッピーコーディング！"); 
}
```

如果您在 Windows 中运行下面的阅读功能，您会遇到问题，因此无法阅读您写的文本:

```
try (FileReader fReader = new FileReader("techisbeautiful.txt");     BufferedReader br = new BufferedReader(fReader)) {   
    String line = br.readLine();   
    System.out.println(line); 
}
```

这是因为 Unix 以 UTF-8 格式存储文件，而 Windows 以 Windows-1252 格式读取文件。在 Java 18 的这个新版本中，这个问题得到了解决，因为 UTF-8 现在是默认的。

## #2:简单的网络服务器

在 Java 18 中，它提供了一个命令行工具来启动 web 服务器。尽管不建议在生产中使用该工具，但它有助于实现概念验证、即席编程或测试目的。

要启动 web 服务器，我们可以使用“jwebserver”命令。默认情况下，它在 localhost:8000 上启动服务器。

```
$ jwebserver 
```

您可以使用`-b`参数来指定服务器应该监听的 IP 地址。通过`-p`，你可以改变端口，通过`-d`，你可以改变服务器应该服务的目录。使用-o，您可以配置日志输出。例如:

```
$ jwebserver -b 127.0.0.50 -p 1234 -d /techisbeautiful -o verbose
Serving /techisbeautiful and subdirectories on 50 port 1234
URL [http://127.0.0.50:1234/](http://127.0.0.100:4444/)
```

此外，您还可以获得一个选项列表，其中包含以下解释:

```
jwebserver -h
```

## #3:用方法句柄重新实现核心反射

虽然这种变化很少被软件工程师使用，但是如果你使用 Java 反射，了解这种变化是值得的。如果您使用 java，您会知道使用 Java 反射的方法总是不止一种。例如，通过反射读取字符串的私有`id`字段，有两种方法:

**1。核心反射:**

```
Field field = String.class.getDeclaredField("id");
field.setAccessible(true);
byte[] id = (byte[]) field.get(string);
```

**2。方法句柄**

```
VarHandle handle =
    MethodHandles.privateLookupIn(String.class, MethodHandles.lookup())
        .findVarHandle(String.class, "id", byte[].class);
byte[] id = (byte[]) handle.get(string);
```

在 Java 18 中，决定在“java.lang.invoke”方法句柄之上重新实现反射类“java.lang.reflect.Method”、“Field”和“Constructor”的代码。这将减少“java.lang.reflect”和“Java . lang . invoke”API 的开发工作量。

## #4:改进创建 Java API 文档的方式

在 Java 18 中，它为 JavaDoc 添加了“@snippet”标记。在以前的版本中，如果您需要将多行代码片段集成到 JavaDoc 中，您必须通过

```
…
```

来完成。这有两个缺点:

```
     tag and the code or no line break between the code and 
    ```

    之间没有换行。
*   代码必须直接从星号后面开始

这是一个用

```
开发 JavaDoc 的例子:
```

```
*/**
 * Write a text file with Java 11:
 *
 * <pre><b>try</b> (BufferedWriter writer = Files.<i>newBufferedWriter</i>(path)) {
 *  writer.write("text");
 *}</pre>
 */*
```

这是带有

```
和{@code…}的示例:
```

```
*/**
 * Write a text file with Java 11:
 *
 * <pre>{****@code*** *try (BufferedWriter writer = Files.newBufferedWriter(path)) {
 *  writer.write("text");
 *}}</pre>
 */*
```

有了 Java 18 中的新增强，我们可以像这样编写 Javadoc:

```
*/**
 * {****@snippet*** *:
 * try (BufferedWriter writer = Files.newBufferedWriter(path)) {
 *   writer.write("text");  // @highlight substring="text"
 * }
 * }
 */*
```

你可以看到这要简单得多。

# 摘要

在这篇文章中，我用例子分享了 Java 18 的四个必须知道的新特性。我们可以在这里总结其中的四个:

*   无论语言和区域设置如何，UTF-8 都是默认字符集。
*   使用“jwebserver”命令快速启动 web 服务器。
*   用方法句柄重新实现核心反射。
*   改进创建 Java API 文档的方式。

如果您错过了以前 Java 版本中的“必须知道的特性”,您可以在下面查看:

*   [你必须知道的 Java 17: 5 特性](https://medium.com/@techisbeautiful/java-17-top-5-features-you-must-know-bbed2afaea3d)
*   [你必须知道的 Java 11: 8 特性](https://medium.com/@techisbeautiful/new-features-you-must-know-in-java-11-and-examples-3fda2ad26def)
*   [你必须知道的 Java 8 : 7 特性](/java-8-seven-features-you-must-know-and-examples-1c3964ae7fe8)

如果你喜欢这个故事，请[关注](https://medium.com/@techisbeautiful)，[让我](https://medium.com/subscribe/@techisbeautiful)成为第一个收到我下一个故事邮件的人。

[你可以在这里](https://medium.com/@techisbeautiful/membership)成为媒介会员，可以**无限制访问**媒介平台上的每一个故事。如果你使用上面的链接，它也支持我，因为我有一个来自 Medium 的小佣金。谢谢大家！