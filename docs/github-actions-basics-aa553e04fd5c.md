# 使用 GitHub 动作自动化您的开发工作流程

> 原文：<https://blog.devgenius.io/github-actions-basics-aa553e04fd5c?source=collection_archive---------17----------------------->

![](img/604d426892bafc77339cc92b636cc81d.png)

照片由[詹姆斯·庞德](https://unsplash.com/@jamesponddotco)

## 这是什么？你为什么需要它？

您可能熟悉 CI 工具，例如 TeamCity、Jenkins、Concourse、Travis CI 等等。嗯，这次我们在镇上有了一个新邻居。

这个新邻居提供了自动化工作流的新功能: [GitHub Actions](https://github.com/features/actions) 。

GitHub Actions 提供了不同的方法来自动化开发任务。您可以做的一些事情是:

*   构建您的应用程序，并在每次代码签入时运行测试
*   将应用程序部署到生产服务器上
*   创建拉取请求时，执行不同类型的自动化任务
*   以定义的时间间隔监控应用程序的运行状况

这只是一些想法，但是正如你所看到的，可能性是无穷无尽的…

在本文中，我将介绍如何创建工作流来构建您的应用程序。

## 工作流、事件和一杯咖啡！

第一步是在`.github/workflows`下的项目根目录下创建一个 GitHub 工作流文件。您需要在这个路径下创建一个 YAML 文件。

一旦创建了工作流文件，您就可以使用`name`属性添加一个名称。

```
name: my-application
```

GitHub Actions 不是你的常规 CI 工具…它非常强大，因为你的动作可以由不同类型的事件触发。我先说最常见的一个，就是推到分支的时候。

首先，在工作流文件中添加`on`属性。因为我们希望在您推送到一个分支时触发，所以我们将添加`push`属性。

下一步？指定要在哪些分支上触发此操作。您可以提供特定的名称，如`master`，或者提供通配符，如`feature/**`。`feature/**`通配符将覆盖任何以`feature/`开头的分支。

```
name: my-projecton:
  push:
    branches:
      - master
      - 'feature/**'
```

可以触发此工作流的另一个事件可能是当您推送一个拉取请求时。在这种情况下，您将最终使用`pull_request`属性而不是`push`。

```
name: my-projecton:
  pull_request:
    branches:
      - master
      - 'feature/**'
```

您可以在有人评论某个问题、发布新闻稿、打开某个问题时创建操作，甚至可以安排 cronjobs。关于这些和更多活动，您可以访问此[页面](https://docs.github.com/en/actions/reference/events-that-trigger-workflows)。

到目前为止，您的工作流已经有了一个名称和一个事件，现在…还有三件事情需要了解…作业、步骤和跑步者。

## 跑步者和工作

![](img/46d416f6562e2a1d3b0442197bb7539b.png)

照片由[布鲁诺·纳西门托](https://unsplash.com/@bruno_nascimento)拍摄

下一步将是创造所谓的就业机会。作业包含多个步骤，它们在所谓的运行程序中执行。

> “一个运行程序获得一个作业，它运行该作业的操作，并向 GitHub 报告进度、日志和最终结果。”

现在我已经讲了一些必要的术语，让我们来看看我们如何创造就业机会。

为了创建一个作业，您需要添加`job`属性，并为您的作业提供一个名称。一旦你做到了这一点，你将需要指定你需要什么操作系统。为此，您将在您的作业名称后添加`runs-on`属性。您现在需要指定操作系统和版本。这里的一些有效值可以是`ubuntu-latest`、`ubuntu-18.04`、`windows-latest`或`macos-10.15`。

```
name: my-projecton:
  push:
    branches:
      - master
      - 'feature/**'jobs:
  my-application:
    runs-on: ubuntu-18.04
```

您必须检查不同操作系统的定价，因为它们之间可能会有很大差异。您可以点击了解更多价格信息*。*

## 下一站:台阶！

![](img/9ff699a9e9dcd5021abdae6828d02738.png)

照片由 [Fabian Fauth 拍摄](https://unsplash.com/@fabster74)

步骤是对操作运行的单个命令。此操作可能是运行测试或构建应用程序。您可以在作业中添加一个或多个步骤。第一步是将`steps`数组属性添加到您的作业中。你应该在工作中增加一个`name`和一个`id`。运行作业时，该名称将显示在用户界面上。该 id 将帮助您在其他步骤中引用来自该步骤的任何输出。

如果您添加了`run`属性并添加了管道`|`，您将能够在接下来的行中运行所有的命令。在这种情况下

```
name: my-projecton:
  push:
    branches:
      - master
      - 'feature/**'jobs:
  my-application:
    runs-on: ubuntu-18.04
    steps:
      - name: build
        id: build
        run: |
          echo "Building application...!"
          sh scripts/build-app.sh
```

## 最后一个:签出代码并指定运行时

到目前为止，如果你按照上面的步骤，你可能没有一个可行的解决方案。这是因为你需要检查你的代码。要做到这一点，您只需要添加另一个步骤，称为`actions/checkout@v2`。

```
- uses: actions/checkout@v2
```

除此之外，您还需要您正在构建的程序的运行时。让我们假设你的程序需要节点。在这种情况下，在签出代码后，您将需要另一个步骤来引入您想要的节点版本。这样做很简单……只需为步骤提供一个名称并使用`actions/setup-node@v1`,就可以在`node-version`属性中指定节点版本。

```
- name: Node '12.15'
  uses: actions/setup-node@v1
  with:
    node-version: '12.15'
```

如果您要开发一个使用 Java 的应用程序，您可以使用`actions/setup-java@v1`并在`java-version`属性上指定版本来引入 Java。

```
- name: Java Version 11
  uses: actions/setup-java@v1
  with:
    java-version: 11
```

最后，您的工作流文件应该如下所示。

```
name: my-projecton:
  push:
    branches:
      - master
      - 'feature/**'jobs:
  my-application:
    runs-on: ubuntu-18.04
    steps:
      - uses: actions/checkout@v2
      - name: Node '12.15'
        uses: actions/setup-node@v1
        with:
          node-version: '12.15'
      - name: build
        id: build
        run: |
          echo "Building application...!"
          sh scripts/build-app.sh
```

到目前为止，您应该已经在每次将代码签入到项目的主分支或特性分支时构建了您的应用程序。如果你有任何无效的语法问题，请随时使用 YAML 在线 linter，比如[这个](http://www.yamllint.com/)。这将有助于识别您遇到的与格式或间距相关的任何小问题。

我希望这篇文章能够帮助您了解什么是 GitHub Actions，以及如何创建一个非常简单的工作流来构建您的应用程序。尽情享受吧！

## 参考

 [## GitHub 动作的核心概念

### 以下是我们在网站和 GitHub Actions 文档中使用的常用 GitHub Actions 术语列表。GitHub 操作…

docs.github.com](https://docs.github.com/en/actions/getting-started-with-github-actions/core-concepts-for-github-actions)  [## GitHub 操作的工作流语法

### 工作流文件使用 YAML 语法，并且必须具有. yml 或。yaml 文件扩展名。如果你刚到 YAML，想…

docs.github.com](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idruns-on)