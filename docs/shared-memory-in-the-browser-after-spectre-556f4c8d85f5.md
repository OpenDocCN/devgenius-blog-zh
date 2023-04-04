# Spectre 之后浏览器中的共享内存

> 原文：<https://blog.devgenius.io/shared-memory-in-the-browser-after-spectre-556f4c8d85f5?source=collection_archive---------9----------------------->

![](img/63ad810db48dc902e9bddfde0fabe7c2.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com/s/photos/internet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

2018 年 1 月，[一个昵称为 Spectre 的巨大安全漏洞被发现](https://meltdownattack.com/)，它影响了所有现代微处理器。它允许恶意软件读取不相关程序使用的计算机内存，潜在地将有价值的信息暴露给黑客。

它实现这一点的方式是利用了现代处理器的一个叫做推测执行的特性。这是一种优化技术，其中 cpu 进程基本上在后台做额外的工作，在发生之前从其流水线*中的指令预测可能的结果，以避免等待特定指令发生和*然后*执行操作。如果预测不正确，额外的工作将被忽略并废弃。如果预测是正确的，那么指令准备好执行的速度将比它们以完全逐步线性方式执行的速度快得多。通过提前准备好一些工作，CPU 避免了小的延迟，并在单线程进程上获得了显著的速度提升。*

Spectre 发现了一种利用这种技术的非常聪明的方法。它基本上包括运行一个程序，该程序首先创建一系列特定的指令来准备 CPU，“训练”它开始预测未来以某种方式进行的操作。通过以这种非常可控的方式训练推测性执行，Spectre 能够不断地在 CPU 内存缓存的特定部分保持不必要的额外后台工作。缓存位置与攻击目标软件使用的内存地址空间相对应。这种执行指令并监听特定指定内存缓存位置的行为被称为“微体系结构隐蔽通道”最终，缓存中的推测性指令通过通道暴露数据，Spectre 能够记录这些数据并将其提供给攻击者。

虽然这是一种难以执行的攻击，并且必须针对极其特定的应用程序，但它有可能完全破坏给定 CPU 上的任何操作，不管使用传统的安全措施或加密。英特尔、AMD 和 ARM 处理器都容易受到这些攻击的事实在被发现时造成了严重的紧急情况。

CPU 制造商迅速发布了固件更新，以现有芯片的性能为代价修复了攻击，到年底，新的卡设计在硬件层面上阻止了 Spectre。

除此之外，各种操作系统也为 Spectre 修补了补丁，特别容易受到它攻击的软件也创建了自己的解决方案。

Javascript 是这种攻击在网络上使用的特别危险的载体，所以浏览器需要实现自己的修复。 [Chrome 默认实现了一个名为站点隔离的功能](https://developers.google.com/web/updates/2018/07/site-isolation)，它将浏览器中的每个页面和框架移动到它们自己独立的进程中，这个进程是沙箱化的，并以特定的方式限制彼此之间的通信。 [Firefox 实现了类似的变化](https://blog.mozilla.org/security/2018/01/03/mitigations-landing-new-class-timing-attack/)，也消除了页面之间的共享内存，并“模糊”了所有高分辨率计时器(例如，Firefox 将`performance.now()`的时间戳输出四舍五入为 1 毫秒的增量，而不是之前非常精确的“高重复分辨率”结果，因为像这样的高分辨率计时器的推测性执行之前允许 Spectre 不断运行并监视内存缓存。)

随着火狐 79 版于 7 月 29 日发布，Mozilla 开始实施一些非常有趣的新功能，允许开发者自 2018 年反幽灵措施以来首次选择回到共享内存和高分辨率定时器。

两个新的头文件:`Cross-Origin-Opener-Policy`和`Cross-Origin-Embedder-Policy`允许你将你的进程与攻击者隔离，并分别从同意的网站获取资源。

他们还实现了`JSExecutionManager`作为 JS 引擎的内置安全保障，包含一个禁用所有共享内存的“开关”。如果发现新的攻击，这将允许 Mozilla 快速而轻松地关闭新的可利用功能。

看看这些特性是否会被其他浏览器开发者和整个网络社区所采用将会很有趣。这能让网络再次开始使用共享内存和高分辨率定时器吗？JS 和 Webassembly 应用程序将获得多大的性能提升？这会改变什么吗？

无论这些早期变化的结果如何，在 web 架构中看到大胆的设计选择总是好的，尤其是当安全性是首要考虑的问题时。

[](https://en.wikipedia.org/wiki/Spectre_%28security_vulnerability%29) [## Spectre(安全漏洞)

### Spectre 是一个影响执行分支预测的现代微处理器的漏洞。在大多数处理器上…

en.wikipedia.org](https://en.wikipedia.org/wiki/Spectre_%28security_vulnerability%29) [](https://hacks.mozilla.org/2020/07/safely-reviving-shared-memory/?utm_source=dev-newsletter&utm_medium=email&utm_campaign=July30-2020&utm_content=sharedmem) [## 安全地恢复共享内存-Mozilla Hacks-Web 开发者博客

### 在 Mozilla，我们希望网络能够运行高性能的应用程序，以便用户和内容作者…

hacks.mozilla.org](https://hacks.mozilla.org/2020/07/safely-reviving-shared-memory/?utm_source=dev-newsletter&utm_medium=email&utm_campaign=July30-2020&utm_content=sharedmem) [](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) [## 同源政策

### 同源策略是一种关键的安全机制，它限制了文档或脚本如何从一个源加载…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer/Planned_changes) [## 共享内存的计划更改

### 正在进行的标准化工作使开发人员能够再次创建 SharedArrayBuffer 对象，但改变了…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer/Planned_changes)