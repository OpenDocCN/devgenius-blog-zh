# 自动版本控制 V2

> 原文：<https://blog.devgenius.io/automatic-versioning-v-2-f45f00c26b66?source=collection_archive---------4----------------------->

![](img/34d3f90c615096ab3a1d53d6e10c94dc.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Carlos Irineu da Costa](https://unsplash.com/@carlosirineu?utm_source=medium&utm_medium=referral) 拍摄的照片

前段时间我发表了关于这个话题的[第一篇故事](http://Automatic versioning)。从那以后有了一些变化。

# TL；博士；医生

*   自动版本化是软件开发中的最佳实践之一，它带来了一些[好处](https://gitversion.net/docs/learn/why)；
*   本文描述了其中的一个具体案例，你需要对一个 **blazor web assembly 解决方案**(客户端和服务器)，使用 Azure DevOps 作为 CI/CD；
*   目标是在应用程序、CI/CD 管道中显示相同的版本信息，同时处理源存储库；
*   工具和技术有 [gitversion](https://gitversion.net/docs/) 、[语义](https://semver.org)版本化、[常规提交](https://www.conventionalcommits.org/en/v1.0.0/#summary)和 [gittools](https://github.com/GitTools/actions)

# 先决条件

*   安装 gittools ( [本地](https://gitversion.net/docs/usage/cli/installation)并作为[扩展](https://marketplace.visualstudio.com/items?itemName=gittools.gittools)到你的 Azure DevOps 组织)
*   该解决方案应该有 3 个项目:客户端(blazor web 程序集)、服务器和共享(可选)。

# 配置

在这种情况下，我们将使用[常规提交](https://www.conventionalcommits.org/en/v1.0.0/#summary)。

常规提交规范是提交消息之上的轻量级约定。它为创建显式提交历史提供了一组简单的规则；这使得在之上编写自动化工具变得更加容易。通过描述提交消息中的特性、修复和重大更改，该约定与[***SemVer***](http://semver.org/)*相吻合。*

## ***GitVersion***

在你的解决方案的根目录下创建***git version . yml***，内容如下:

```
assembly-versioning-scheme: MajorMinorPatchnext-version: 0.0.1mode: MainLine # Only add this if you want every version to be created automatically on your main branch.major-version-bump-message: "^(build|chore|ci|docs|feat|fix|perf|refactor|revert|style|test)(\\([\\w\\s-]*\\))?(!:|:.*\\n\\n((.+\\n)+\\n)?BREAKING CHANGE:\\s.+)"minor-version-bump-message: "^(feat)(\\([\\w\\s-]*\\))?:"patch-version-bump-message: "^(build|chore|ci|docs|fix|perf|refactor|revert|style|test)(\\([\\w\\s-]*\\))?:"
```

## 密码

**步骤 1** 将在 web assembly app 中显示的 assembly 版本属于客户端，而不是服务器，项目。因此，它必须在每次添加适当的提交时进行更新。

在您的两个项目文件中(顺便说一下，这对于客户端项目是必需的)添加以下行:

```
<GenerateAssemblyVersionAttribute>false</GenerateAssemblyVersionAttribute>
    <GenerateAssemblyFileVersionAttribute>false</GenerateAssemblyFileVersionAttribute>
    <GenerateAssemblyInformationalVersionAttribute>false</GenerateAssemblyInformationalVersionAttribute>
```

**第二步**可以使用下面的代码显示 app 版本:

```
typeof(Program).Assembly.GetName().Version
```

**第三步**假设 GitVersion.yaml 位于以上级别，从客户端项目文件夹中生成带有更新版本信息的组装文件:

```
gitversion /updateassemblyinfo Properties/AssemblyInfo.cs /ensureassemblyinfo /config ../../GitVersion.yml
```

这一步将创建如下内容:

```
using System.Reflection;[assembly: AssemblyFileVersion("0.0.1.0")][assembly: AssemblyVersion("0.0.1.0")][assembly: AssemblyInformationalVersion("0.0.1-beta.......")]
```

# 建设

在 CI 阶段，在构建项目之前，您必须从客户端/属性文件夹执行:

```
dotnet-gitversion /updateassemblyinfo AssemblyInfo.cs /ensureassemblyinfo /config ../../GitVersion.yml
```

# 例子

## Azure DevOps 管道

```
- task: gitversion/setup@0 displayName: Install GitVersion inputs: versionSpec: '5.x'- task: gitversion/execute@0 inputs: useConfigFile: true configFilePath: '$(Build.SourcesDirectory)/SolutionFolder/GitVersion.yml'- task: Bash@3 inputs: targetType: 'inline' script: | cd '$(Build.SourcesDirectory)/SolutionFolder/Client/Properties' dotnet-gitversion /updateassemblyinfo AssemblyInfo.cs /ensureassemblyinfo /config ../../GitVersion.yml
```