# 了解如何确保您的 git 分支名称有效

> 原文：<https://blog.devgenius.io/find-out-how-to-ensure-your-git-branch-name-is-valid-6930639eaaaa?source=collection_archive---------6----------------------->

## 与大型团队合作的必要 git 验证

## 三个简单有效的解决方案

![](img/4cc6aae0a97adbcfbe32505ff8e7e864.png)

扎克·赖纳在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在一个大型团队中使用 git 可能会很困难，即使你遵循了所有的最佳实践或像 git-flow 这样的分支策略，就个人而言，我讨厌我们有一堆具有不同名称或结构的分支，像*修复-按钮-屏幕-确认*、*专长-标签-用户名-欢迎*、*特性-组-视频-共享*等等，过一会儿你就会为找到你的分支名称而发生冲突。

我总是建议使用斜线来分隔三个重要的信息:代码、任务类型、用户名和吉拉入场券或者任务的简短描述。

这里有几个例子，

```
* feat/username/JIRA-123-new-username-label
* fix/username/JIRA-321-login-crash
* ci/username/JIRA-456-ci-branch-validation
```

任务的类型与传统的提交标准有关，如果你想有一个真正伟大的变更日志或者只是组织你的提交消息，这是非常重要的。*如果你想了解更多关于这个标准的信息，请点击此* [*链接*](https://www.conventionalcommits.org/en/v1.0.0/) *。*

有三个有效的解决方案来验证分支名称，我更喜欢第三个，但是如果您使用 CI/CD，第一个和第二个可能是很好的解决方案。

## 第一种选择:正则表达式

如果您使用 javascript 之类的编程语言，这是验证分支名称的最简单方法。

正则表达式是一种搜索模式，用于在文本字符串中查找或替换匹配项。正则表达式可用于验证文本字符串是否满足特定标准，例如只允许特定的字母、数字和特殊字符。

要使用正则表达式来验证 Git 分支名称，首先需要定义想要使用的搜索模式。

例子，

```
/^(master|develop){1}$|^(feature|chore|fix)\/([\/\w\\-\\d]+)\/(JIRA|TIKET)-([\w\-\d]+)$/
```

这个正则表达式接受我之前提到的结构，并且将 ***master*** 或 ***develop*** 作为分支的名称，但是您也可以定义自己的结构， [**Regex101**](https://regex101.com/) 是一个很好的 web，您可以在其中测试自己的正则表达式。

一旦定义了正则表达式，就可以使用正则表达式验证函数来检查分支名称是否与搜索模式匹配。在一些编程语言中，比如 JavaScript，这可以通过使用 *RegExp.test()* 函数来完成。

```
const regex = /^(master|develop){1}$|^(feature|chore|fix)\/([\/\w\\-\\d]+)\/(JIRA|TIKET)-([\w\-\d]+)$/;
const branchName = "feature/francistec/JIRA-123-new-username-label";

if (regex.test(branchName)) {
  console.log("Valid name");
} else {
  console.log("Wrong name");
}
```

在这种情况下，脚本的结果将是“有效名称”。

如果您使用的 CI/CD 系统可以在启动之前运行脚本，比如 TeamCity 或 Jenkins，那么这个选项真的很好。

## 第二个选项:检查参照格式

第二个选项比上一个好，在我看来，如果有机会修改的话~/。gitconfig 文件。

在开始使用这个选项之前，您应该修改文件~/。gitconfig 通过在*【core】*段中添加标志*【refspecFormat】*，就像这个例子 *:*

```
[core]
  refspecFormat = /^(master|develop){1}$|^(feature|chore|fix)\/([\/\w\\-\\d]+)\/(JIRA|TIKET)-([\w\-\d]+)$/
```

一旦您已经修改了这个文件，您将能够运行下一个命令:

```
git check-ref-format --branch feature/francistec/JIRA-123-new-username-label
```

同样，在您的 CI/CD 中，您可以在开始编译或部署之前执行脚本，如下所示:

```
if git check-ref-format --branch feature/francistec/JIRA-123-new-username-label then
  echo "Valid name"
else
  echo "Wrong name"
fi
```

## 强壮的

![](img/ecdc9606b4e0a8a2a40891d1a615413a.png)

照片由[上的](https://unsplash.com?utm_source=medium&utm_medium=referral)[灰](https://unsplash.com/@gxldy?utm_source=medium&utm_medium=referral)拍下

Husky 是一个帮助您增强 git 提交消息和推送操作的库。

Husky 将允许您添加自定义脚本来运行您的存储库，这些脚本可以用作 git 挂钩。

值得一提的是，Husky 只处理带有 package.json 文件的项目。

这个库的安装非常简单，您只需要运行下一个命令:

```
npm install husky --save-dev
```

然后，您需要修改 package.json，在脚本部分添加属性 prepare

```
"scripts" {
    //.....
    "prepare": "husky install",
    //.....
}
```

在展示使用 husky 验证分支名称的最后一步之前，我想向您展示一个 npm 库来验证分支名称。

## 第三个选项:验证分支机构名称

这个库可以与 Husky 一起使用，但也可以直接在命令行中使用，正如我们在第一个选项中看到的那样。

如果您将通过命令行使用该库，您应该在全局上下文中安装该库

```
$ npm i validate-branch-name -g
```

如果您只是将它用作本地库，那么您应该运行

```
$ npm i validate-branch-name -D
```

下面是一些通过命令行运行它的例子

```
npx feature/francistec/JIRA-123-new-username-label

# test target branch name
npx validate-branch-name -t feature/francistec/JIRA-123-new-username-label

# define regexp to test branch name
npx validate-branch-name -r /^(master|develop){1}$|^(feature|chore|fix)\/([\/\w\\-\\d]+)\/(JIRA|TIKET)-([\w\-\d]+)$/ -t feature/francistec/JIRA-123-new-username-label

# use -h for more usage detail
npx validate-branch-name -h
```

## 验证分支机构名称和哈斯基

好了，现在我们可以继续我们的哈士奇配置。正如您所注意到的，Husky 可以在提交消息之前作为钩子运行任何命令，validate-branch-name 是一个可以通过命令行执行的库。

要使用 husky 添加挂钩，您需要运行下一个命令:

```
npx husky add .husky/pre-commit "npm test"
```

不要忘记将这个文件添加到 git 存储库中。

```
git add .husky/pre-commit
```

现在，在每次提交之前，将执行命令“ *npm 测试*”。

因此，如果您想使用 validate branch name，可以将命令“npm test”更改为 validate-branch-name 命令。

如果您还想返回一条消息，告诉开发人员这条消息不正确，那么您也可以通过添加下一节来修改 package.json

```
 "validate-branch-name": {
    "pattern": "^(master|develop){1}$|^(feature|chore|fix)\/([\/\w\\-\\d]+)\/(JIRA|TIKET)-([\w\-\d]+)$",
    "errorMsg": "Please follow standard branch name, rename branch using: git branch -m <oldname> <newname>"
  },
```

在这一节中，我定义了我想要使用的模式，并且通过提到重命名他们的分支的方法来帮助开发人员。

## 结论

有许多方法可以确保您的 git 分支名称遵循您的标准。我真的希望这篇文章能帮助人们有一个更好的组织，今天我向你展示了三种方法，我提出了一个模式，但是，你可以使用任何模式来帮助你组织你的团队。

这里重要的是让我们的工作更容易。