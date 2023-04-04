# 如何在 GitHub 页面上部署 React 应用？

> 原文：<https://blog.devgenius.io/how-to-deploy-a-react-application-on-github-pages-7dd7a48f75cd?source=collection_archive---------0----------------------->

![](img/4fa5036bb843908750973bed72012a70.png)

那么，您想免费部署 react 应用程序吗？是的，您可以使用 Github 页面免费部署 react 应用程序。让我们从这个教程开始吧。

先决条件:
1。[安装 Git](https://docs.github.com/en/github/getting-started-with-github/quickstart/set-up-git) ，
2。[制作你的 GitHub 账号](https://github.com)，
3。[安装 npm 和 npx](https://nodejs.org/en/download/)

8 个简单的步骤:

1.  设置 react 应用程序[如果您有 react 应用程序，则可选]
    我们将使用 create-react-app 设置 react 应用程序

```
$ npx create-react-app react-demo
$ cd ./react-demo
```

2.将 gh-pages npm 包作为开发依赖项安装

```
npm intall gh-pages --save-dev
```

3.现在，在 package.json 中添加重新部署和部署脚本

```
"predeploy": "npm run build","deploy": "gh-pages -d build"
```

4.在 package.json 的顶层添加 homepage 属性

```
"homepage": "https://<username>.github.io/<repo-name>
```

完成这一步后，package.json 应该如下所示:

5.初始化应用程序文件夹中的 git 存储库

```
$ git init
```

6.[创建一个空的 GitHub 存储库](https://github.com/new)并将 repo 作为远程文件添加到您的本地 git 存储库中

```
git remote add origin https://github.com/<username>/<repo-name>.git
```

7.最后一步:将优化的生产构建部署到 gh-pages

```
npm run deploy
```

8.运行以下命令将代码推送到 GitHub 存储库:

```
git add .
git commit -m 'publish to gh-pages'
git push --set-upstream origin master
```

嘣！⭐ ⭐️ ⭐️

react 应用程序位于您在 package.json 的 homepage 属性中添加的 URL 上

下面是我的:
直播:[https://jainaayush01.github.io/react-demo](https://jainaayush01.github.io/react-demo)
回购:[https://github.com/jainaayush01/react-demo](https://github.com/jainaayush01/react-demo)

如果您有任何疑问、建议或需要帮助，请随时评论或联系我。

谢谢你的时间

联系人: [Twitter](https://twitter.com/jainaayush01) ， [GitHub](https://github.com/jainaayush01) ， [Linkedin](https://linkedin.com/in/jainaayush01)