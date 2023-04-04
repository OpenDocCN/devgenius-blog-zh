# 为开发人员更新备忘单

> 原文：<https://blog.devgenius.io/update-cheat-sheet-for-developers-87a4a3c1bae?source=collection_archive---------19----------------------->

使用过时的应用程序是一个容易修复的高风险漏洞。本指南为不同的应用程序提供了易于遵循的指导，以修复安全漏洞。

![](img/53100f5ce9794f5b810321ca10f4fe1d.png)

> *⚠️本指南的目标是通过更新应用程序来消除漏洞。有时更新会破坏事物或导致意想不到的行为。在生产环境中使用本备忘单中的命令之前，您需要进行足够的检查和测试。*

让我们看看如何更新不同的环境。

# RHEL/CentOS/甲骨文 Linux

在终端中运行以下命令(ssh)

```
sudo yum update
```

# Debian/Ubuntu Linux

在终端中运行以下命令(ssh)

```
sudo apt update && sudo apt upgrade
```

# OpenSUSE/SUSE Linux

在终端中运行以下命令(ssh)

```
sudo zypper up
```

# 节点(npm)

在 NodeJs 项目目录中运行下面的命令。

```
npm audit fix
```

请注意，有些漏洞无法自动修复，需要手动干预或检查。使用下面的命令可以强制进行一些修复，但是请确保它不会破坏项目中的任何东西。

```
npm audit fix --force
```

# Python (pip)

你必须一个接一个地更新软件包。运行以下命令以获取过期包的列表。

```
pip list --outdated
```

对于每个包，运行下面的命令来更新它。

```
pip install [package_name] --upgrade
```

# 努格特

从命令行，您可以将解决方案中的包更新到 nuget.org 提供的最新版本。

```
nuget update YourSolution.sln
```

请注意，这不会运行任何 NuGet 包中的任何 PowerShell 脚本。

在 Visual Studio 中，您可以使用[包管理器控制台](https://docs.nuget.org/docs/reference/package-manager-console-powershell-reference)来更新包。这样做的好处是，任何 PowerShell 脚本都将作为更新的一部分运行，而使用 NuGet.exe 不会运行它们。以下命令将把每个项目中的所有包更新到 nuget.org 提供的最新版本。

```
Update-Package
```

# PHP(作曲)

导航到您的应用程序的根目录，也就是您的`composer.json`文件所在的位置，然后运行下面的命令。

```
php composer.phar update
```

在 Windows 中:

```
composer update
```

# Go (golang)

要更新您的`GOPATH`中的所有包，运行下面的命令。

```
go get -u all
```

# 红宝石

要更新所有 gem:

```
gem update
```

RubyGems 保留了 Gems 的旧版本。更新后运行清理以删除旧的 gem。

```
gem cleanup
```

# 梅文(mvn)

运行以下命令以强制更新依赖项。

```
mvn clean install -U
```

# 铁锈(货物)

为了更新 Rust 项目的所有依赖项，您需要安装一个第三方机箱。安装`cargo-update`:

```
cargo install cargo-update
```

然后运行下面的命令来检查新版本并更新所有已安装的软件包。

```
cargo install-update -a
```

# wordpress 软件

WordPress 让你点击一个按钮就可以更新。您可以通过单击新版本横幅中的链接(如果有)或转到控制台>更新窗口来启动更新。一旦你进入*“更新 WordPress”*页面，点击*“立即更新”*按钮开始这个过程。你不需要做任何其他事情，一旦完成，你将是最新的。

# Windows 操作系统

在`cmd`上运行以下命令，打开 Windows update 屏幕。

```
control update
```

出什么事了，或者丢了什么？这个文档是 GitHub 项目的一部分，非常欢迎投稿。

您还可以使用 [SmartScanner 在您的应用程序中查找过时的组件](https://TheSmartScanner.com/)。

# 参考

*   [npm 审计](https://docs.npmjs.com/cli/v8/commands/npm-audit)
*   更新 WordPress
*   [stackoverflow.com](https://stackoverflow.com/a/6882750)
*   [stackoverflow.com](https://stackoverflow.com/a/10383783)
*   [stackoverflow.com](https://stackoverflow.com/a/40982333)