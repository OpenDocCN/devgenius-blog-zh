# V8 8.6 有什么新功能？更好的 WebAssembly！

> 原文：<https://blog.devgenius.io/whats-new-in-v8-8-6-better-webassembly-2a67abd766fa?source=collection_archive---------2----------------------->

![](img/7e2ae303926b3111553740d44efabda8.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[谭 Kaninthanond](https://unsplash.com/@12tan34?utm_source=medium&utm_medium=referral) 的照片

本周，由谷歌开发的著名 V8 网络引擎发布了他们最新的开发分支，8.6 版。在这个版本中有大量的新特性，我将谈论那些与 WebAssembly 相关的特性。

# Chrome 起源

当谷歌在 2008 年发布 Chrome 浏览器时，这是一款令人印象深刻的软件，它比当时的竞争对手更快、更轻便，并引入了许多独特的功能，这些功能在我们今天浏览网络时是理所当然的。它很快成为所有平台上最受欢迎的浏览器——今天它拥有超过 65%的活跃浏览器市场份额。

Chrome(及其免费开源的 Chromium 代码库)背后的秘密是它的 V8 javascript 引擎。Chromium 发布一年后，跨平台 Javascript 运行时环境 Node.js 也发布了，它也运行 V8。Node 和它的 NPM 生态系统迅速革新了 web 开发，今天任何有 JS 使用经验的人都知道这一点。

基本上，如果你以任何方式与互联网互动，你可能会依赖 V8 引擎及其跨所有平台的高性能(当然，不包括 iOs)。)

# WebAssembly —另一种 V8 语言

然而，V8 不仅适合运行 Javascript。它还实现了 WebAssembly(或简称 wasm 一种类似于传统汇编的非常低级的语言，为极高性能的应用程序而设计。V8 的基线 Wasm 编译器叫做 Liftoff。

在最新的 V8 版本中，一组特殊的机器指令被称为[单指令，多数据(SIMD)](https://v8.dev/features/simd) 被添加到升空编译器中。这允许 wasm 软件通过同时执行某些重复操作来更有效地利用数据并行化。

这使得 Liftoff 在这类操作中的性能几乎提高了三倍，使得代码更加高效和高性能，从而增加了它的可移植性。通过在基线编译器中实现这些硬件向量指令，wasm 变得更接近本机汇编代码，这是在高性能 web 应用中增加采用 wasm 的一大步。

我期待着在未来几个月看到 webassembly 的一般用途会有什么变化，因为它显然是 V8 开发的一个优先事项。下面链接的是一个非常酷的 github repo，它包含了编译成 wasm 的各种编程语言的列表。这意味着您可以用您选择的语言编写您常用的本机软件，并使用该列表中给定的编译器将其转换为超快速的与平台无关的低级 wasm 代码！

[](https://v8.dev/blog/v8-release-86) [## V8 版本 8.6

### 每六周，我们都会创建一个新的 V8 分支，作为我们发布过程的一部分。每个版本都是从 V8 的 Git 分支而来的…

v8.dev](https://v8.dev/blog/v8-release-86) [](https://github.com/appcypher/awesome-wasm-langs) [## appcypher/awesome-wasm-langs

### 😎直接编译或在 web assembly-appcypher/awesome-wasm-langs 中拥有虚拟机的语言精选列表

github.com](https://github.com/appcypher/awesome-wasm-langs)  [## 简介- WebAssembly 1.1

### WebAssembly(缩写为 Wasm 1)是一种安全、可移植的低级代码格式，旨在高效执行和…

webassembly.github.io](https://webassembly.github.io/spec/core/intro/introduction.html)  [## 使用 WebAssembly SIMD 的快速并行应用程序

### SIMD 代表单指令多数据。SIMD 指令是一类特殊的指令，它利用…

v8.dev](https://v8.dev/features/simd)