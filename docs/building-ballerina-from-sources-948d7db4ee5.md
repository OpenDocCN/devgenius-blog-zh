# 从源头打造芭蕾舞演员

> 原文：<https://blog.devgenius.io/building-ballerina-from-sources-948d7db4ee5?source=collection_archive---------7----------------------->

![](img/a10fa9fb25f767fe521c26e67727fdc3.png)

在我的[上一篇文章](https://medium.com/ballerina-techblog/ballerina-integration-programming-language-5d8e1b52e582)中，我已经简单介绍了芭蕾舞演员编程语言。但是我不能给你一个关于如何在你当地环境中设置芭蕾舞演员的提示。

为了训练芭蕾舞演员，我们可以尝试几种方法:

1.  [使用特定于操作系统的安装程序](https://ballerina.io/learn/user-guide/getting-started/setting-up-ballerina/installation-options/#installing-ballerina-via-installers)
2.  [使用公共二进制文件发布芭蕾舞演员](https://ballerina.io/learn/user-guide/getting-started/setting-up-ballerina/installation-options/#installing-via-the-ballerina-language-zip-file)
3.  从源头打造芭蕾舞演员

这里，前两种方法非常简单。但是从源头上打造芭蕾舞演员并不简单。因此，在这篇文章中，我们将仔细看看我们如何做到这一点。

# 先决条件

下载并[安装](https://adoptopenjdk.net/installation.html) OpenJDK 11 ( [采用 OpenJDK](https://adoptopenjdk.net/) 或任何其他 OpenJDK 发行版)

*   **信息:**您也可以使用[甲骨文 JDK](https://www.oracle.com/java/technologies/javase-downloads.html) 。

[为您的 GitHub 帐户设置个人访问令牌](https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token)，并配置以下环境变量(访问令牌应具有`read package`权限)。

**Linux / Unix :**

**视窗:**

# 用工具构建芭蕾舞演员运行时

将 [ballerina-lang 存储库](https://github.com/ballerina-platform/ballerina-lang)添加到您的 GitHub 帐户并克隆它。

```
git clone --recursive https://github.com/<GITHUB_USERNAME>/ballerina-lang.git
```

然后导航到`<BALLERINA_LANG_PROJECT>`目录并运行下面的命令来更新 Git 子模块。

```
git submodule update --init
```

然后用下面的命令开始构建过程。

你可以在下面的路径找到芭蕾舞演员的语言分布。

```
<BALLERINA_LANG_PROJECT>/distribution/zip/jballerina-tools/build/distributions/jballerina-tools-<VERSION>.zip
```

将构建解压缩到您的首选位置，并配置以下环境变量。

**Linux / Unix :**

**视窗:**

# 测试版本

因为这个版本只是一个普通的芭蕾舞语言版本，所以我们只有基本的语言特性和 Java (Java Introp) API。因此，我们可能需要编写一个只使用这些功能的小型芭蕾舞程序。

将上述程序保存在`hello_world.bal`中，并执行以下命令来构建和运行程序。

```
bal run hello_world.bal
```

如果构建成功，您应该得到以下输出。

```
Hello, World!
```

# 构建完整的芭蕾舞演员分布

在上面的步骤中，我们只构建了基本的芭蕾舞演员语言分布。但是芭蕾舞剧的主要特色之一是它的[标准库](https://github.com/ballerina-platform/ballerina-standard-library)。对于基本语言版本，我们将无法访问芭蕾舞演员标准库包。

因此，如果你想获得完整的芭蕾舞演员发行版的全部访问权限，你应该建立[芭蕾舞演员发行库](https://github.com/ballerina-platform/ballerina-distribution)。

将[芭蕾舞演员-发行库](https://github.com/ballerina-platform/ballerina-distribution)转到你的 GitHub 账户并克隆它。

```
git clone --recursive                    [https://github.com/](https://github.com/ballerina-platform/ballerina-distribution.git)<GITHUB_USERNAME>[/](https://github.com/ballerina-platform/ballerina-distribution.git)[ballerina-distribution.git](https://github.com/ballerina-platform/ballerina-distribution.git)
```

然后导航到`<BALLERINA_DISTRIBUTION_PROJECT>`目录，运行下面的命令开始构建(这里为了加快构建速度，排除了`tests`)。

你可以在以下路径找到完整的芭蕾舞演员分布。

```
<BALLERINA_DISTRIBUTION_PROJECT>/ballerina/build/distributions/ballerina-<VERSION>.zip
```

将构建解压缩到您的首选位置，并配置以下环境变量。

**Linux / Unix :**

**视窗:**

# 测试版本

由于这是一个完整的芭蕾舞演员发行版本，因此它将包含所有标准的库模块依赖关系。因此，我们可以使用[芭蕾舞演员 IO 模块](https://github.com/ballerina-platform/module-ballerina-io/)编写简单的 **Hello World** 程序。

将上述程序保存在`hello_world_with_io.bal`中，并执行以下命令构建并运行程序。

```
bal run hello_world_with_io.bal
```

如果构建成功，您应该得到以下输出。

```
Hello, World!
```

在本文中，我们了解了如何构建和使用基本芭蕾舞造型和完整芭蕾舞造型。这很容易也很简单。

跟进芭蕾舞女:[你好，世界！芭蕾舞演员的节目](https://ayesh9303.medium.com/hello-world-program-in-ballerina-9678d2c284fa)