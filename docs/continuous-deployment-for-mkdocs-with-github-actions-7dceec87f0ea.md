# 使用 GitHub 动作持续部署 MkDocs

> 原文：<https://blog.devgenius.io/continuous-deployment-for-mkdocs-with-github-actions-7dceec87f0ea?source=collection_archive---------9----------------------->

![](img/ef20df4fb526e752911ae93156e23c9a.png)

照片由[rich Great](https://unsplash.com/@richygreat?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

MkDocs 是一个很棒的文档工具，但是每次你做更新的时候，都需要一个部署过程来将你的更新发布到 GitHub 页面上。为了解决这个问题，我决定学习如何使用 GitHub actions 在更新被推送到或合并到主分支时自动更新发布的站点。如果你是 MkDocs 的新手，我当然推荐我以前的博客[MkDocs 入门](https://parkererickson.github.io/portfolio/blog/gettingStartedWithMkdocs/)

# 什么是 GitHub 动作？

GitHub Actions 是一个 CI/CD 工具，直接绑定到 GitHub 的生态系统中。这使您可以设置工作流，这些工作流可以通过存储库中可能发生的许多不同事件来触发，例如合并或推送到特定分支。在这种情况下，我们将在每次推送和合并到主分支时通过 MkDocs 触发文档的生成和部署。这里也有很多预置动作可用[。](https://github.com/marketplace?type=actions)

# 行动

所有 GitHub 动作都是用 YAML 定义的。有两个主要部分需要定义，包括操作应该何时运行，以及当操作被触发时应该发生什么。首先，让我们在存储库的最外层目录中创建一个`.github`目录，然后在`.github`目录中创建一个`workflows`目录。然后创建一个`main.yml`文件，这是定义动作的地方。

# 何时运行操作

我用这个动作来更新我的个人作品集网站以及其他一些只保存文档和 GitHub 页面内容的存储库。因此，我希望这个操作用主分支中的任何 push 或 pull 请求进行更新。我们对此的定义如下:

```
on: 
  push: 
    branches: [master] 
  pull_request:
    branches: [master]
```

# 定义作业和步骤

在我们决定何时让 GitHub 运行动作之后，下一步是定义作业和每个作业中需要发生的步骤。为此，我们将只有一项工作，称为构建。这些任务如下:

1.  签出主分支——这使用了一个内置的动作，所有 GitHub 动作都可以使用这个动作将存储库克隆到新启动的容器中。
2.  setup Python 3.7——这也使用了一个内置动作，可用于所有 GitHub 动作。
3.  安装 pip 和 mkdocs-material-`run`关键字用于您希望使用的终端命令。`pip install mkdocs-material`还会自动安装 mkdocs 和任何其他需要的包依赖项。
4.  运行`mkdocs gh-deploy`——这将构建文件并将它们部署到 GitHub 页面。

完整的结果如下所示:

```
name: Build Documentation using MkDocs 
# Controls when the action will run. Triggers the workflow on push or pull request 
# events but only for the master branch 
on: 
  push: 
    branches: [master] 
  pull_request: 
    branches: [master] 
jobs: 
  build: 
    name: Build and Deploy Documentation 
    runs-on: ubuntu-latest 
    steps: 
      - name: Checkout Master 
        uses: actions/checkout@v2       - name: Set up Python 3.7 
        uses: actions/setup-python@v2 
        with: 
          python-version: '3.x'       - name: Install dependencies 
        run: | 
          python -m pip install --upgrade pip 
          pip install mkdocs-material 
      - name: Deploy 
        run: | 
          git pull mkdocs gh-deploy
```

# 结论

在`.github/workflows`目录下创建了上面的文件之后，应该就可以了！只需在本地提交您的更改并推送到 GitHub 存储库。您可以在存储库的 GitHub 页面的**动作**选项卡上查看构建的状态。

*最初发布于*[*https://Parker Erickson . github . io*](https://parkererickson.github.io/portfolio/blog/MkDocsCD/)*。*