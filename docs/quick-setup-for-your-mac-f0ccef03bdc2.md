# 轻松设置您的 Mac

> 原文：<https://blog.devgenius.io/quick-setup-for-your-mac-f0ccef03bdc2?source=collection_archive---------5----------------------->

## brew |应用程序|维护|安装| ansible

## 不想手动安装所有需要的应用程序？用一个文件来定义你所需要的一切怎么样？

![](img/7e6f0f3cdf72bac30df7e4f877a139ac.png)

Mia Baker 在 [Unsplash](https://unsplash.com/s/photos/quick-mac?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

是的，有很多复杂的软件/包管理解决方案，但是我想要简单的，几分钟内就能开始的，所以我们开始了:

[](https://github.com/DrPsychick/macdev) [## 心理医生/麦克德夫

### 我讨厌当我需要从头开始安装电脑的时候，但有时我需要这样做。为了简化流程…

github.com](https://github.com/DrPsychick/macdev) 

# **既然要快，下面就来说说怎么用**

```
git clone [https://github.com/DrPsychick/macdev.git](https://github.com/DrPsychick/macdev.git)
cd macdev# copy the example file
cp host_vars/localhost-example.yml host_vars/localhost.yml# --> edit host_vars/localhost.yml to your needs
open host_vars/localhost.yml# run it with one simple command
ansible-playbook macdev.yml
```

> **提示**:brew 使用`sudo`时，可能会要求您输入密码

# 先决条件

现在，这可能无法在新的 mac 上开箱即用，所以这里有一些你必须手动完成的步骤…抱歉没有捷径😜

*   安装**家酿** : [https://brew.sh](https://brew.sh)
*   安装 **python** 和 pip `brew install python`
*   安装 **ansible** : `pip3 install -U ansible`

# 所有的木桶…

要找到更多你可以添加的包，只需输入`brew search --casks ide`找到你最喜欢的 IDE 的包…或者任何你可能需要安装的东西。

当然还有 brew 提供给你的所有其他令人敬畏的应用程序、库和工具:`brew search mtr/jq/kubectx/...`

您还可以通读最近发表的一篇 Medium 文章，其中提到了如何构建开发人员可能需要的所有工具:

[](https://medium.com/@davidbrookton/setting-up-a-mac-os-development-environment-834f81a4ea88) [## 设置 Mac OS 开发环境

### 欢迎使用 Mac OS！

medium.com](https://medium.com/@davidbrookton/setting-up-a-mac-os-development-environment-834f81a4ea88) 

当谈到用于特定项目的**数据库和库时，我更喜欢将它们封装在 docker 容器中，尽管这样不会搞乱我的主机系统……本文对此有更多介绍:**

[](https://medium.com/swlh/8-reasons-why-every-developer-should-use-docker-and-you-wont-believe-5-c71f8a58cd83) [## 每个开发人员都应该使用 Docker 的 8 个理由——你不会相信第 5 个！

### 它将节省您的时间，避免问题，帮助构建可扩展的应用程序，并使您和您的团队更加高效。

medium.com](https://medium.com/swlh/8-reasons-why-every-developer-should-use-docker-and-you-wont-believe-5-c71f8a58cd83) 

# 更多关于自制

以下帖子更详细地解释了 brew 的使用:

[](https://medium.com/tech-explained/how-to-get-and-use-the-ultimate-macos-package-manager-f2ba5c9466d4) [## 家酿——让你微笑的 MacOS 软件包管理器

### 它是免费的，而且很容易。

medium.com](https://medium.com/tech-explained/how-to-get-and-use-the-ultimate-macos-package-manager-f2ba5c9466d4) 

## **开放软件通过贡献而生存……**

你喜欢吗？它非常简单，所以任何人都可以添加它！

可以发个 PR！可以支持！可以分享一下。

## 感谢您的阅读！