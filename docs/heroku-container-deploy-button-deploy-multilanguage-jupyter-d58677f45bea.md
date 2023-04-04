# Heroku 容器部署按钮:部署多语言 Jupyter

> 原文：<https://blog.devgenius.io/heroku-container-deploy-button-deploy-multilanguage-jupyter-d58677f45bea?source=collection_archive---------15----------------------->

创建和配置 Heroku app.json 以部署内容化应用的指南。

我需要一个 web [Jupyter 笔记本](https://jupyter.org/)与 [Java](https://github.com/SpencerPark/IJava) ，我需要这个程序可以被许多其他人以一种简单的方式复制。因此，我创建了一个带有 Heroku deploy 按钮的 git repo:你可以在 Heroku 注册，点击该按钮，按照程序操作，你就拥有了自己的多语言 jupyter 笔记本。

![](img/9312a4f2bd8de73d688932e97f619cc8.png)

Heroku 部署按钮

我从使用 IJava 作为 Jupyter 内核的[多语言容器](https://z-uo.medium.com/jupyter-with-multiple-languages-1ae58e98dc8e)开始。我创建了 Heroku 文件，我想解释配置 Heroku 按钮来部署您的容器化应用程序的过程。

我的结果在 repo[Java _ JUnit _ jupyter](https://github.com/nicolalandro/java_junit_jupyter)witch 中，我放了一些学习 [JUnit](https://junit.org/junit5/) 和 [Mockyto](https://site.mockito.org/) 基础知识的示例笔记本来创建自动化测试。

这个 jupyter 包含: [**Coq**](https://coq.inria.fr/) **，**[**SoS**](https://vatlab.github.io/sos-docs/)**，**[**Java**](https://www.oracle.com/java/)**，**[**python 3**](https://www.python.org/)**。**

## Dockerfile 文件

开始创建 Dockerfile 文件:

```
FROM nicolalandro/multilanguage-jupyter:1.0                                                           ADD notebooks /server/notebook
```

在我的例子中很简单，因为我从一个准备好的容器开始，我添加了笔记本文件夹，但是您可以根据需要创建 Docker 容器。

## Heroku 配置

heroku 配置需要两个东西一个 app.json 和一个 heroku.yml(仅用于容器配置)。

heroku.yml 可以配置我们的容器的名称和运行命令:

```
build:                               
  docker:                                 
   web: Dockerfile                             
  run:                               
   web: jupyter notebook --ip=0.0.0.0 --port=$PORT --allow-root --NotebookApp.token="$JUPYTER_TOKEN" --no-browser
```

记住要指定$PORT 和一个环境变量，因为 heroku 容器会自动选择暴露的端口。我还添加了$JUPYTER_TOKEN，这是 JUPYTER 的密码，每个 pearson 都可以在部署过程中选择它。

添加 app.json 文件，该文件是 heroku 为部署您的应用程序而搜索的文件:

```
{
  "name": "APP NAME",
  "description": "APP DESC",
  "repository": "https://YOUR/REPO/LINK.git",
  "keywords": [
    "productivity",
    "java",
    "jupyter"
  ],
  "stack": "container",
  "formation": {
    "web": {
      "quantity": 1
    }
  },
  "env": {
    "JUPYTER_TOKEN": {
      "description": "A token to put into the url to use jupyter.",
      "value": ""
    }
  }
}
```

名称、描述和报告简单易懂，关键字可以由你自由编写。我们选择容器作为堆栈，并且我们需要将服务器定义到该结构中。我们也可以用 env 字段定义环境变量。

## 部署按钮

使用 github repo 链接(https://YOUR/REPO/LINK.git)将 heroku deploy 按钮添加到 README.md 中:

```
[![Deploy](https://www.herokucdn.com/deploy/button.svg)](https://YOUR/REPO/LINK.git)
```

提交并推送您的 repo 所有这些内容，然后您就可以一键部署到 heroku 上了。

## 部署 Heroku

可以在 repo git 上的[自述文件](https://github.com/nicolalandro/java_junit_jupyter)中找到可视化指南。

*   登录 heroku
*   单击您的部署按钮
*   选择您最喜欢的应用程序名称
*   填充环境
*   点击“部署应用程序”按钮
*   等等…
*   点击“查看”按钮
*   你在线了！

## 注释和“问题”

我不知道为什么，但默认情况下，Scala、Ruby 和 javascript 内核在开始时不工作，但你可以运行这个命令并刷新 jupyter 页面来拥有它:你可以点击文件新建并查看这些语言。

Javascript:

```
ijsinstall
```

红宝石:

```
iruby register --force
```

Scala:

```
cd /server
./almond --install
```

## 结论

Heroku 是一个非常有用的服务，提供免费和付费的潜力，但也有了免费版本，我们可以做很多事情。我认为这个指南足够简单，让每个人都可以使用这个简单的服务，只需点击一下按钮就可以部署开源项目。