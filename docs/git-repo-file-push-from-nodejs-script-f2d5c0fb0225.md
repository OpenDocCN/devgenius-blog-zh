# 从 NodeJS 脚本推送 Git Repo 文件

> 原文：<https://blog.devgenius.io/git-repo-file-push-from-nodejs-script-f2d5c0fb0225?source=collection_archive---------3----------------------->

![](img/68b993b91393a0ad777bf86840c0c402.png)

照片由 [Praveen Thirumurugan](https://unsplash.com/es/@praveentcom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/s/photos/github?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄

在本文中，我们将了解如何从运行的 nodejs 代码本身推送 git 存储库中的文件夹或文件。

当我在我的一个项目中工作时，我不得不将文件从代码本身推到 git 库。但是我找不到解决这个问题的好办法，所以我写了这篇文章。

对于我们的案例，我们将使用 **shellJS** 和 **simple-git** 模块。

ShellJS 是在 Node.js API 之上的 Unix shell 命令的可移植的 **(Windows/Linux/OS X)** 实现。您可以使用它来消除您的 shell 脚本对 Unix 的依赖，同时仍然保留其熟悉而强大的命令。您还可以全局安装它，这样您就可以从外部节点项目运行它——告别那些粗糙的 Bash 脚本！

git-simple 是一个轻量级接口，用于在任何 [node.js](https://nodejs.org) 应用程序中运行`git`命令。

了解更多关于 [shelljs](https://www.npmjs.com/package/shelljs) 和 [simpleGit](https://www.npmjs.com/package/simple-git) 的信息。

```
npm i shelljs
npm i simpleGit
```

为了从 GitHub 克隆 git 存储库，我们需要传递用户凭证(用户名和密码)。一种方法是通过 URL 本身传递用户名和密码。

我们可以通过 simple-git 轻松做到这一点

首先创建 simple-git 的实例，名称可以是 git 或您喜欢的任何名称。

```
const simpleGit = require("simple-git");
const git = simpleGit();
```

在此之后，我们需要使用克隆方法来克隆所需的回购。

```
await git.clone(`https://${username}:${password}@github.com/yourepo`);
```

但是，如果我们只想进行克隆，前提是克隆之前没有完成，那么我们将检查存储库的文件夹名称是否存在，或者是否使用 fs 模块(**文件系统模块)**允许在 nodejs 中使用文件系统。如果不存在，那么我们可以克隆。

```
if(!fs.existSync(path.resolve(__dirname, 'cloned folder name')); 
      await git.clone(`https://${username}:${password}@github.com/yourepo`);
```

克隆完成后，我们的任务是首先复制我们想要放入 git 存储库中的文件夹或文件。为此，我们将使用 fs-extra 模块。

`fs-extra`添加了本地`fs`模块中没有的文件系统方法，并为`fs`方法添加了 promise 支持。当我们想用 promise 复制整个文件夹时，它也很有帮助。

要了解更多关于 fs-extra 的信息，请访问[这里](https://www.npmjs.com/package/fs-extra)。

```
const fse = require(fs-extra);
...await fse.copySync(path.resolve(__dirname, 'folder where file is     present'), path.resolve(__dirname, 'git repo folder'), {                                   overwrite : true }, 
    (error)=> {                                      
        if(error)                                         
           throw new error;                                        
         else                                          
           throw new "some internal error";                                    });....
```

到目前为止，我们已经克隆了存储库，并将文件夹复制到克隆的存储库中。但是主要的工作是提交我们在存储库中所做的更改。

因此，我们将使用 shellJS。Shelljs 为我们提供了在 nodejs 脚本中运行终端命令的功能。

我们将使用 exec 命令来执行查询。

使用 exec，我们将首先在 git local 中添加文件夹，然后使用 commit 命令提交，之后我们将拉取更改，以便原始存储库和当前存储库中不会有任何意外的更改，然后我们将从本地推送更改。

所以这一切的全部代码如下

希望你喜欢这篇文章。有许多其他方法可以做到这一点，但我个人认为这是最简单的。

感谢您抽出时间阅读这篇文章。