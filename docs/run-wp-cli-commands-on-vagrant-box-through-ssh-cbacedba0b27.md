# 通过 SSH 在流浪者盒子上运行 WP-CLI 命令

> 原文：<https://blog.devgenius.io/run-wp-cli-commands-on-vagrant-box-through-ssh-cbacedba0b27?source=collection_archive---------3----------------------->

## 跳过你注册成为流浪者的步骤。

![](img/12ab6bad36c743fa7a837e8368105719.png)

[克里斯托弗·比尔](https://unsplash.com/@umbra_media?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

今晚我很纠结。我不喜欢保持 SSH 连接打开，以防我需要运行命令行工具。我也不喜欢每次想运行命令行工具时都要连接到一个 travel box。

更糟糕的是，我正在开发一个需要使用 WP-CLI 的 WordPress 插件，并且是通过 Laravel Homestead 的流浪者盒子来完成的。我相信你明白我的困境。

当然，我可以很容易地设置一个 SSH 别名，并运行类似于`wp @homestead ...`的命令，但是我不想仅仅因为开发环境而改变我正在运行的命令。

经过几分钟的摆弄，阅读文档，最终找到正确的组合，我让它按照我想要的方式工作。据我所知，它并没有在任何地方被记录，但是你实际上可以为 WP-CLI 设置一个默认配置，它可以包括一个 ssh 字段。

有了两条线，你就可以走了。我们来分解一下。

首先，这都在项目的`wp-cli.yml`中，它存储在项目的根目录中。这样，任何对`wp`命令的调用都会加载这个配置。

接下来，default 告诉命令我们正在设置默认配置。我试着去掉它，把 ssh 放在顶层，但结果是行不通的。事实上，我得到了一个奇怪的密码登录循环。

实际的 ssh 命令与 WP-CLI 文档中的命令相同，但是如果您还没有阅读这些文档，我还是会在这里解释一下。首先，我们在`@`的左边有用户名。因为这是一个流浪儿盒子，用户名是流浪儿。登录后，您可以通过运行`vagrant ssh`然后运行`whoami`找到您的用户名。

在`@`的右边是流浪者盒子的 IP 地址。家园默认是 192.168.10.10，所以我这里用的是这个。紧随其后的是项目 WordPress 安装在 travel box 上的位置。当我同步我的项目时，我总是把它们放在`~/code/`中，所以这就是你在这里看到的。Homestead 的默认 WordPress 配置在项目文件夹中创建一个`wp`文件夹，在根目录中存储一些配置文件，并将`wp-content`文件夹从`wp`中移出并移入项目根目录。

WP-CLI 关心 WordPress 文件在哪里，所以我们一直告诉它到`/wp`目录。

在这种配置下，如果我在项目中运行 wp-cli，它将使用项目的漫游访问——这很重要，原因有很多，可能与在 Homestead 中运行它有关。我还没有在 VVV 尝试过，但是我怀疑只要你的 IP 和目录设置正确，它会同样有效。