# 如何用 Lua 在 Neovim 中创建自定义键映射

> 原文：<https://blog.devgenius.io/create-custom-keymaps-in-neovim-with-lua-d1167de0f2c2?source=collection_archive---------0----------------------->

## 尼奥维姆

## 学习使用可选的内置 Lua 运行时在 Neovim 中创建自定义键绑定。为了 Lua 的利益，抛弃古老而神秘的 VimScript！

![](img/715edf0516b411b9143d2ae265f01bd4.png)

开始使用 Lua 来配置 Neovim &同时保护您的理智(图片来源:Somraj Saha)

Neovim ( *甚至 Vim* )对于任何开发者来说都是一款优秀的软件。创建自定义按键绑定的能力&几乎可以做任何事情
证明了 Vim 的受欢迎程度。如果您以前使用过 Vim，那么您应该
知道通过自定义 Vim 键绑定可以做些什么。

对于门外汉来说，Vim 对于创建自定义按键绑定的开放性几乎没有竞争对手
。因此，只有你的想象力才能限制你使用自定义键绑定所能创造的东西。

Vim 用户还需要具备 Vimscript ( *这是一种为 Vim 配置而构建的脚本语言*)的工作知识。它不是最优雅的语言，在 Vim 之外也没有任何用处。对于你们中的许多人来说，学习一门多余的编程语言所需的时间投入可能也不会有什么成效。幸运的是， *Neovim v0.5+* 给了社区一个重要的更新，可以用来玩&内置的 Lua 运行时。

可选的 Lua 运行时也是向后兼容的，这意味着您仍然可以在 Vimscript 中尝试可选的运行时。我们将不讨论如何在 Vimscript 中编写 Lua 代码，因为这超出了本文的范围。但是，请随意参考 GitHub 上的这个令人惊叹的 [Neovim-Lua 指南](https://github.com/nanotee/nvim-lua-guide)以获得快速参考。

如果这激起了你的兴趣&你想继续了解在 Neovim 中创建自定义键绑定是多么愉快的体验，那么请继续阅读
。本文的其余部分首先简要介绍可选的 Lua
运行时，然后创建一个 Lua 函数来映射定制的键绑定。在文章的最后，我们会推荐更多的资源，你可能想看一看，以便更好地了解 Neovim 的 Lua 运行时。

## 介绍可选的 Lua 运行时

在我们深入本文之前，让我们简要介绍一下 Neovim 中的 Lua
运行时。对它有所了解将有助于更好地理解如何创造什么。

也就是说，Neovim 发布时提供了一些有用的功能。使 Neovim 脱颖而出的一个这样的
特性是它的[内置 API](https://neovim.io/doc/user/api.html) 。Lua 运行时对 API &的编程访问意味着如果你愿意，你可以尽情发挥你的想象力。

正如你所知道的，像任何其他用于攻击 Neovim/Vim 的配置文件一样，Lua 代码也需要放在 *runtimepath* (更多信息*参见* ***:h rtp*** *)。这些配置文件(*带有. lua 扩展名*)被放在一个
目录中，这个目录被恰当地命名为 *lua* 。Neovim 将在调用时获取该
目录中的所有内容。*

请注意，Neovim 运行时路径的位置因您选择的操作系统而异。所以，对于 Linux 用户来说，检查你的操作系统是否遵循了
[XDG 基本目录](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html)规范，那么 Lua 文件通常可以在: ***$HOME/获得。config/nvim/Lua*。我的 Windows 用户伙伴们，你们应该在***% local appdata % \ nvim \ Lua***查看 Lua 文件。**

关于在哪里放置配置文件的更多信息，请查看 GitHub 上 Neovim-Lua 指南的
[放置 Lua 文件](https://github.com/nanotee/nvim-lua-guide#where-to-put-lua-files)部分。

鉴于我们对可选 Lua 运行时的简单介绍，让
看看如何用 Lua 配置 Neovim 键绑定。在接下来的几个
小节中，我们将看一看，编写一个示例 Lua 函数，后面跟着一个
Lua 中的实际函数包装器。这个功能包装器将用于创建我们的自定义键绑定，其中&在任何需要的时候。

## 如何为 Neovim 键绑定编写 Lua 函数

回到 Neovim 还不存在的时候，vim 提供了 ***remap*** 命令(*和****no remap****用于非递归重映射*)来定制&重映射键绑定。因此，看到 ***nnoremap*** 命令散落在 ***各处是很常见的场景。vimrc* 文件**文件。这里有一个这样的
[*例子* ***。vimrc***](https://github.com/jessfraz/.vim/blob/master/vimrc) 文件我从网上捡的。文件巨大( *~1200 行代码！*)，很难操作&维护起来简直是一场噩梦。

这是我从 ***中摘录的一小段代码。vimrc* 上面的**文件。

仅使用 Vimscript 配置 Neovim 的示例

幸运的是，Neovim 通过它的内置 API 提供了一个助手功能。
恰如其分地命名为***nvim _ set _ key map()***，用户可以直接使用这个函数
，也可以将其封装在 Lua 代码中使用。推荐使用后一种方法，因为这样可以遵循标准的编码实践。将 Lua 代码与 Neovim API 结合使用也有助于配置的模块化。从而使维护变得更加容易&保持你的理智完好无损。

使用包装函数还可以确保配置是"[](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)**"&"[](https://www.digitalocean.com/community/conceptual_articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design)**"。遵循这样的通用开发标准实践意味着配置看起来也很干净&有条理。****

****那么，当与 Lua 一起使用时，典型的 Neovim 配置是什么样子的呢？
这里有一个例子:****

****创建 Lua 功能，通过遵循干燥和坚固的原则来定制 Neovim 按键****

****乍一看，Lua 代码可能显得过于冗长，但这是一件好事，您很快就会看到。****

****在上面我们分享的 Lua 代码片段中，我们定义了一个名为 ***map()*** 的函数。它接受四个参数，即:****

1.  *******模式*** ( *如同在 Vim 模式中一样* ***正常*** */* ***插入*** *模式*)****
2.  ******lhs** ( *您需要的自定义键位*)****
3.  ******rhs** ( *命令或已有的按键来定制*)****
4.  ******opts** ( *附加选项如* ***<静音>****/***<no remap>***，参见****:h map-arguments****了解更多信息*)****

****默认情况下， ***map()*** 函数的 ***opts*** 参数赋给一个表
***{ nore map = true }***。这样做时，允许映射的嵌套&递归使用(*参考****:h map-commands****了解更多信息*)。您可以根据需要进一步扩展 ***opts*** 表，添加***map-arguments***。包装器的核心是***vim . API . nvim _ set _ key map()***函数，它接受上面提到的参数列表。****

****这个函数可以在任何需要的地方重用。正如您将在
中看到的，接下来我们可以进一步模块化我们的 Neovim 配置！****

## ****在 Neovim 运行时文件中使用 Lua 函数****

****我们在上一节中使用的包装函数确实有助于定制
默认 Vim 重映射。但是定义包装函数&然后在我们的 ***init.lua*** 文件中使用它根本不符合 DRY 和/或 SOLID 原则。为了使配置更符合标准开发实践，我们将使用 [**Lua 模块**](https://www.lua.org/manual/5.1/manual.html#5.3) 。使用 Lua 模块确保了在我们的配置中有清晰的逻辑分离。****

****如果您需要复习如何为 Neovim 创建 Lua 模块，请参考 GitHub 上 Neovim-Lua 指南的
和[模块](https://github.com/nanotee/nvim-lua-guide#modules)部分。****

****也就是说，在 runtimepath 中的"***【Lua***"目录下创建一个"***【utils . Lua****"目录。
如果 Lua 模块包含以下代码行，则它们被识别:*****

*****定义 Lua 模块的基本框架*****

*****解释 Lua 模块也超出了本文的范围，所以可以参考上面链接的 Lua 5.1 文档。*****

*****对于我们的用例，我们定义的 Lua 模块将包含定制键绑定的功能包装器。照这样这就是我们的"***utils . Lua****"*模块的内容会是什么样子:*****

*****一个 Lua 模块的例子，包含了我们的实用 map()函数，用于定制 Neovim 按键绑定*****

*****由于 Neovim 源和加载位于 runtimepath 中“lua”目录下的任何 Lua 文件，我们的 ***map()*** 函数也可以导入到任何其他地方。因此，现在可以简单地将包装器导入到您的“***init . Lua****”*文件&中，用它来创建您的 remaps。因此，你现在可以保持文件干净，可维护的&快速加载。*****

*****下面是如何通过从" ***utils*** "模块导入" ***map()*** "
"函数来重写" ***init.lua*** "文件；*****

*****既然代码已经被适当地模块化了，维护就不应该是一件苦差事&出问题的几率会低得多。此外，如果您希望扩展我们基本的 ***map()*** 函数，只需对“ ***utils*** ”模块进行必要的更改即可！*****

*****也就是说，如果你已经使用 Neovim 有一段时间了，你还需要将你的配置迁移到 Lua 代码。你会很高兴听到 Neovim 开发人员竭尽全力保持向后兼容性。如果对现有配置进行从 Vimscript 到 Lua 代码的迁移更改让您感到不安，那么不要担心。您甚至可以在 Vimscript 中使用 Lua 代码！为此，请参考“ ***:h heredocs*** ”了解更多关于该主题的信息。*****

*****为了帮助您在迁移现有配置时找到合适的方法，
下一节将推荐一些有用的参考资料供您参考。*****

## *****最后的话&值得期待的事情*****

*****Neovim 中可选的 Lua 运行时是天赐之物&这也是使 Neovim 从 vim 中脱颖而出的特性之一。但是由于特性& Neovim 本身相对较新，所以很难找到关于这些特性的资源。因此，如果用 Lua 配置 Neovim 引起了您的兴趣，您可能想看看下面的一些资源。*****

1.  *****[在 Neovim 中使用 Lua 的指南](https://github.com/nanotee/nvim-lua-guide)*****
2.  *****"[***h:lua***](https://neovim.io/doc/user/lua.html)"获得如何在 Neovim 中使用 Lua 的全面指导。*****
3.  *****有点无耻，你也可以用我的博客作为参考。我曾经写过一篇这样的文章，介绍在 [Vim 或 Neovim 中为 Neovim 使用 Lua 的好处？以下是你应该使用后者](https://jarmos.netlify.app/posts/vim-vs-neovim/)的原因。还会有更多这样的文章，所以请保持警惕。*****

*****我错过了其他有用的资源吗？如果我做了，让我知道！*****

*****而且总结一下这篇文章，你怎么看待用 Lua 配置
Neovim？您当前的配置是 Vimscript 还是 Lua？你注意到使用这两种方法有什么不同吗？*****

*****给我发一条短信到推特[推特](https://twitter.com/Jarmosan)或者电子邮件，随你喜欢。*****