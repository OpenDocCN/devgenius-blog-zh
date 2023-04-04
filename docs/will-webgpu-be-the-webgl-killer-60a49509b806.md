# WebGPU 会成为 WebGL 黑仔吗？

> 原文：<https://blog.devgenius.io/will-webgpu-be-the-webgl-killer-60a49509b806?source=collection_archive---------2----------------------->

## 你知道 WebGPU 吗？

![](img/e9ce9e08a46caa7f2f6c3c5206800fb0.png)

迪克夏·帕哈里亚在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

你可能用过 WebGL，如果没有那你一定用过 Three.js，不是吗？

啊哈，那也行。本文将向您介绍 WebGL 和后起之秀 WebGPU。

# WebGL 是什么？

## WebGL 的起源

说到起源，就不得不说 OpenGL 了。

在个人电脑早期，应用最广泛的 3D 图形渲染技术是 Direct3D 和 OpenGL。Direct3D 是微软 DirectX 技术的一部分，主要用于 Windows 平台。OpenGL 作为一种开源的跨平台技术，赢得了众多开发者的青睐。

然后出现了一个特殊的版本——OpenGL ES。它是为嵌入式计算机、智能手机、家用游戏机和其他设备设计的。它从 OpenGL 中删除了许多旧的和无用的特性，同时添加了新的特性。比如去掉多余的矩形等多边形，只保留点、线、三角形等基本图形。这使得它在保持轻量级的同时，仍然强大到足以渲染美丽的 3D 图形。

而 WebGL 是从 OpenGL ES 衍生出来的。它专注于 Web 的 3D 图形渲染。

下图显示了它们之间的关系:

![](img/87ee33bfdc52b7d8524977b15dffbbba.png)

图片来自 [WebGL 编程指南](https://www.google.com/books/edition/WebGL_Programming_Guide/3c-jmWkLNwUC?hl=en&gbpv=0)

## WebGL 的历史

![](img/d5ba8cb0324118a70db09ca279562cde.png)

图片来自 [YouTube](https://www.youtube.com/watch?v=y2dZYG5YTRU)

从上图可以看出，WebGL 已经相当老了。不仅因为它的长期存在，还因为它的标准是从 OpenGL 继承来的。OpenGL 的设计理念最早可以追溯到 1992 年，这些古老的概念其实和今天 GPU 的工作原理非常不一致。

对于浏览器开发者来说，他们需要适应 GPU 的不同特性，这给他们带来了很多不便。虽然这些是我们上层开发者看不到的。

从上图我们可以看到，2014 年苹果发布了 Metal。史蒂夫·乔布斯是 OpenGL ES 的支持者，他认为这是行业的未来。所以当时苹果设备上的游戏依赖于 OpenGL ES(如愤怒的小鸟，水果忍者)。你玩过吗？

但在乔布斯去世后，苹果放弃了 OpenGL ES，开发了新的图形框架 Metal。

微软也在 2015 年发布了自己的 D3D12 [Direct3D 12]图形框架。其次是 Khronos 集团，你知道是谁吗？

![](img/4ad1dd487cacc46288fc55d532a6a4ea.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Siednji Leon](https://unsplash.com/@siednji?utm_source=medium&utm_medium=referral) 拍摄的照片

其实是图形界的国际组织，类似于前端圈的 W3C 和 TC39。而 WebGL 就是它的标准。甚至他们也逐渐淡化了 WebGL，转而支持现在的 Vulkan。

到目前为止，Metal、D3D12 [Direct3D 12]、Vulkan 并列为现代图形三大框架。这些框架充分释放了 GPU 的可编程能力，允许开发人员最大限度地自由控制 GPU。

同样需要注意的是，今天的主流操作系统不再支持 OpenGL 作为主要支持。这意味着我们今天写的每一行 WebGL 代码都有 90%的几率不会被 OpenGL 绘制出来。在 Windows 电脑上用 DirectX 绘制，在 Mac 电脑上用 Metal 绘制。

可见 OpenGL 已经晚了。但这并不意味着它会消失。它继续在嵌入式和科学研究等特殊领域发挥作用。

WebGL 也是如此，大量的适配工作让它很难前进。于是推出了 WebGPU。

# WebGPU 是什么？

WebGPU 的目的是提供现代 3D 图形和计算能力。它是由前端的老朋友 W3C 组织开发的标准。与 WebGL 不同，WebGPU 不是 OpenGL 的包装器。而是指目前的图形渲染技术，一种新的跨平台高性能图形接口。

其设计更容易被三大图形框架实现，减轻了浏览器开发者的负担。这是一个精确的图形 API，完全开放了整个显卡的功能。不再是 WebGL 那样的上层 API。

更具体的优点如下:

*   减少 CPU 开销
*   良好的多线程支持
*   通过计算着色器将通用计算(GPGPU)的强大功能带到网络中
*   全新的着色器语言— WebGPU 着色语言(WGSL)
*   未来将支持“实时光线追踪”的技术

![](img/c7bdebf7aa964821ec38df4764cf917f.png)

照片由[哈维尔·埃斯特万](https://unsplash.com/@javiestebaan?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

这里我挑出多线程支持来详细讨论一下:

在 WebGL 中，可以使用 OffscreenCanvas 进行多线程处理，但是 Safari 还不支持这个功能。而且它的操作也非常有限，你要把整个画布的所有操作都转移到单个工人身上。

webGPU 上下文可以由多个工作者共享。只要这些 workers 在时间轴上没有互斥操作，都可以正常运行。这无疑带来了很好的开发体验。

这是因为 WebGL 全局状态的设置。它会在设置状态后立即执行，这使得 WebGL 无法支持多线程。

而 WebGPU 使用`Pipeline Object`来设置渲染管道中的相关信息。分为“记录命令”和“提交命令”两个阶段。

“记录命令”是一个纯 CPU 过程。可以在多个 workers(多线程)中分别记录，然后提交到同一个队列。

“提交命令”就是让 GPU 按照队列中的命令顺序执行。

# WebGPU 的发展现状

WebGPU 的 API 仍在开发迭代中，但我们可以在 Chrome Canary 中试用。

![](img/2c85ef5747751a73f8d479ff0006aeae.png)

在目前的前端框架中，Three.js 已经开始实现 WebGPU 的后端渲染器，Babylon.js 计划在`5.x`版本中支持 WebGPU。

# 结论

我认为 WebGPU 取代 WebGL 是大势所趋。我相信它在元宇宙有很大的潜力，这很有趣，不是吗？

你对 WebGL 有什么看法？你看好 WebGPU 吗？

*感谢阅读。如果你喜欢这样的故事，想支持我，请考虑成为* [*中等会员*](https://medium.com/@islizeqiang/membership) *。每月 5 美元，你可以无限制地访问媒体内容。如果你通过* [*我的链接*](https://medium.com/@islizeqiang/membership) *报名，我会得到一点佣金。*

你的支持对我来说非常重要——谢谢。