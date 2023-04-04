# 在管道中处理 Latex 文件

> 原文：<https://blog.devgenius.io/process-latex-files-in-your-pipeline-f75ad13fb208?source=collection_archive---------0----------------------->

## latex | PDF | TeX | PDFLatex | Pipeline | artifact

## 如何在不安装 TeX Live 的情况下使用 PDFLatex？

![](img/3f00e378184739fc8f06e8b50762eb64.png)

照片由 [Florian Klauer](https://unsplash.com/@florianklauer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

# 在每台机器上安装 TeX Live？

当你需要安装一个工具，而它没有安装，然后它需要各种依赖时，你不讨厌吗？

TeX Live 或 Latex 对我来说从来都不容易安装或维护——更不用说理解我需要的所有软件包了。所以有一天，我开始了为我和其他人解决这个问题的冒险——永远！使用`docker` …

你想处理一个. tex 文件，但是没有安装 Latex？
(这将下载大约 200MB 大小的图像，但不需要安装其他东西)

`docker run --rm -v $PWD:/data drpsychick/texlive-pdflatex:latest pdflatex myfile.tex`

瞧，如果一切顺利，您应该会看到输出，并在当前目录中找到一个`myfile.pdf`。不需要安装 pdflatex 或其他什么，如果你只是需要标准的 latex 功能。

# 好了，让我们自动化吧！

但是从您的管道中自动生成文档怎么样？

这个`.gitlab-ci.yml`将所有的`.tex`文件处理成`.pdf`文件，并将结果文件作为工件保存一天。

```
image: drpsychick/texlive-pdflatex

build:
  script:
    - find . -name \*.tex -exec pdflatex {} ';'
    # fail if number .tex files does not match number of .pdf files
    - if [ $(find . -name \*.tex |wc -l) -gt $(find . -name \*.pdf |wc -l) ]; then exit 1; fi
  artifacts:
    paths:
      - "*.pdf"
    expire_in: 1 day
```

这个解决方案已经为我服务了几年，没有出现任何问题，并且节省了我很多时间。所以我想是时候分享它了，同时也节省了别人的时间。

# 无限可能…

您还可以将它用作基础映像，并扩展它以包含您需要的任何 TeX Live 软件包:

我很高兴在 github 上收到 pull 请求或讨论您的想法！

https://github.com/DrPsychick/docker-texlive-pdflatex

[](https://github.com/DrPsychick/docker-texlive-pdflatex) [## drpsycick/docker-tex live-pdflatex

### 最小的 texlive 自动构建阿尔卑斯山图像，包括 pdflatex，欧洲标准类和语言使用…

github.com](https://github.com/DrPsychick/docker-texlive-pdflatex) 

https://hub.docker.com/r/drpsychick/texlive-pdflatex/

## 谢谢你

现在把它拿走，尽情享受乳胶吧！