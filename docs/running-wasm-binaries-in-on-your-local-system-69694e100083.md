# 在本地系统上运行 WASM 二进制文件

> 原文：<https://blog.devgenius.io/running-wasm-binaries-in-on-your-local-system-69694e100083?source=collection_archive---------4----------------------->

新 WASI 运行时已经过时了。本文解释了如何在本地机器上运行 wasm 二进制文件。

WASI 代表 [WebAssembly 系统接口](https://github.com/CraneStation/wasmtime/blob/master/docs/WASI-overview.md)。

简而言之，WASI 允许你在任何运行 WASI 的平台上运行 wasm 二进制文件。这种技术有一天可能解放多语言体系结构，在这些语言之间共享本地模块和交换功能。

你可以在 [moz://a HACKS](https://hacks.mozilla.org/category/webassembly/) 博客上关注关于 WebAssembly 的最新文章。在那里你可以寻找作者林克拉克来了解更多关于的事情。

在本文中，我想解释在本地机器上运行 Rust 编译的 wasm 的必要步骤。

# 先决条件

# WebAssembly 运行时

在 [wasmtime.dev](https://wasmtime.dev/) 你可以为基于 Linux 的系统安装最新的运行时。

对于 windows，您需要自己编译可执行文件，但这不是问题。

只需从 GitHub[https://github.com/CraneStation/wasmtime](https://github.com/CraneStation/wasmtime)克隆回购并执行

```
> git clone --recurse-submodules https://github.com/CraneStation/wasmtime.git 
> cd wasmtime 
> cargo build --release
```

在命令行中。

(如果遇到任何问题，不要忘了先`rustup update`

完成后，如果你愿意，让可执行文件对命令行可用，或者你必须在本教程的后面提供可执行文件`wasmtime`的完整路径。

# 检查一个 Rust 项目

为了简单起见，我建议你从 GitHub[https://github.com/rusticus-io/hangman](https://github.com/rusticus-io/hangman)克隆 Rust 社区 Stuttgart 的 hangman 教程。

# 安装 WASI 编译目标

```
> rustup target add wasm32-wasi
```

# 如何编译项目

切换到 Rust 项目文件夹并编译源代码。

```
cargo build --target wasm32-wasi
```

你可以在`target/wasm32-wasi/debug`文件夹中找到 wasm。

# 在运行时运行 WASM 二进制文件

```
> wasmtime target/wasm32-wasi/debug/hangman.wasm
```

并且玩游戏；).

如您所见，在本地机器上运行 WASM 非常容易。

WASI 运行时的好处是，所有的程序都运行在一个沙箱中，开箱即用，只提供访问你的文件夹结构向下。出于安全原因，这有助于限制对系统的访问。如果您想要更大的灵活性，可以查找 wasmtime 命令行选项，并且(显然)等待将来的版本。

这意味着 WASI 的发展还没有结束。

我想看看未来会带来什么。

*原载于*[*http://rusticus . io*](http://rusticus.io/blog/2019-09-29-running-hangman-tutorial-in-wasi/)*。*