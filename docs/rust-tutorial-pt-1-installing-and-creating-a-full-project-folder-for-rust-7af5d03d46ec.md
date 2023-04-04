# Rust 教程第 1 部分:为 Rust 安装和创建完整的项目文件夹

> 原文：<https://blog.devgenius.io/rust-tutorial-pt-1-installing-and-creating-a-full-project-folder-for-rust-7af5d03d46ec?source=collection_archive---------8----------------------->

![](img/2b8403b5c6e11f4e2e3c65f63808d0bf.png)

使用 WebAssembly (WASM)网络进行系统编程和区块链开发的各种用例。

尽管 Rust 相对来说比大多数编程语言都要新，但它在系统编程中的使用吸引了很多关注。除了它的特性，区块链的开发者还使用 Rust 在 Polkadot、Solana、NEAR Protocol、埃尔隆德和其他区块链网络上构建应用程序。今天，我们将介绍如何安装 Rust 并创建一个项目文件夹。

## 安装铁锈

如果您使用 Linux、Mac 或其他 Unix 系统，这非常简单。安装编程语言只需复制“curl—proto ' = https '—tlsv 1.2-sSf[https://sh . rust up . RS](https://sh.rustup.rs)| sh”即可。安装 Rust 后，可以使用“rustc–version”来检查版本，要进行更新，如果以前安装过 rustup，则需要输入“rustup update”。然而，如果你使用的是 Windows，你可能要遵循 [Rust 文档](https://rust-lang.github.io/rustup/installation/windows.html)。

## 安装 IDE

即使很多 ide 都支持 Rust，但只有两个 ide 以其特性吸引眼球。第一个是由 JetBrains 开发的 IntelliJ IDEA，第二个 IDE 是 VS Code。您可以通过他们的网站下载这两个 ide。

IntelliJ IDEA 支持 Rust，它为大多数开发人员创建了一个方便的开发环境。然而，IntelliJ IDEA 在支持 Rust 方面存在问题，这使得它无法在 IDE 中正常工作。另一方面，VS 代码是一个带有 Rust 扩展的优秀编辑器，与其他 ide 相比，它更容易使用。无论是在你的电脑上有一个文件夹还是创建一个文件夹，VS 代码都非常适合处理 Rust。

## 创建项目文件夹

在我们安装了 Rust 并且它是最新的之后，我们可以通过终端创建一个项目文件夹或者创建一个文件夹，你就可以让项目运行了。根据我的经验，通过你的电脑创建一个文件夹要方便得多，因为它更容易跟踪你的文件夹。

要在没有命令的情况下创建文件夹，您可以在桌面上右键单击并创建一个文件。然后，你可以通过 VS 代码打开你的文件，打开终端，它会显示你的项目文件。然后，您可以编写货物构建、货物初始化和货物构建–发布，以获得 rust 文件的所有包。

一些程序员可能更喜欢通过终端创建项目。你可以使用 VS 代码和它的嵌入式终端，或者只是通过一个计算机终端。要通过终端创建一个项目，首先，您应该编写 cargo new。然后你需要打开 VS 代码，然后它的嵌入式终端，你可以写货物建设，货物初始化和货物建设-释放，以保持项目运行。完成这些步骤后，您就可以在 Rust 中开始您的项目了。快乐学锈快乐编码。

你对生锈有什么看法？你发现哪些特征不同？你用 Rust 编程过吗？在下面的评论区分享你的想法和经历。