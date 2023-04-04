# 如何用 Javascript 创建 Youtube 下载器

> 原文：<https://blog.devgenius.io/creating-youtube-downloader-in-javascript-5b7e20215271?source=collection_archive---------0----------------------->

![](img/19a7b5633214b1853c05b20528fa6c2a.png)

由 [CardMapr](https://unsplash.com/@cardmapr?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

Youtube 是当今最著名的视频流媒体网站，甚至世界上 98%的人都在使用 youtube，甚至安卓手机也内置了谷歌安装的 youtube。我认为这是事实，youtube 是谷歌最受欢迎的产品，让我们转移到今天的话题，JavaScript 的 Youtube 下载器。因此，我们将通过如何开发一个 youtube 下载器来下载你的计算机上的任何 youtube 视频或播放列表，我们可以使用任何编程语言来编写这个 python，C++甚至 java。但是我喜欢 JavaScript，所以我会继续使用它，好吧，不浪费时间，让我们直接开始编码。我们需要 NodeJs 来编译 JavaScript 代码，如果您已经安装了 NodeJs，那么您可以转到库安装部分，或者不只是下载最新版本的 NodeJs 并将其安装在您的 windows、Linux 或任何操作系统上。接下来我们需要一个 JavaScript 模块名为 ***youtube-dl，*** 复制下面的命令并粘贴到命令提示符中。

`*npm i youtube-dl*`

# **编码部分**

创建一个 youtube.js 文件并在代码编辑器中打开它，让我们从在脚本中导入 youtube-dl 库开始。

第一行，我们在常量变量 fs 中加载 fs 模块，该模块主要用于读写视频文件，下一行，我们在常量变量 youtube-dl 中加载我们的主模块。加载完所需的模块后，我们只需使用 youtubedl()函数，在该函数中我们传递 youtube 视频的 URL 和格式。注意，我们将格式编号设置为 18，即 320p mp4 格式，如果您想在 youtube 视频中设置高清格式或您喜欢的任何格式，请访问*[***YouTube _ format _ number***](https://gist.github.com/sidneys/7095afe4da4ae58694d128b1034e01e2)**。**让我们移动到程序的另一部分，正在下载视频检查下面的代码。*

*如果你注意到了，在第 4 行我们将 ***youtubedl()*** 保存在常量视频变量中，而在第 10 行我们使用该视频变量来调用函数 info。下一行使用 javascript console.log 函数在命令提示符下输出 youtube 信息。上次我们使用 fs 模块以 mp4 格式编写我们的文件，如果你看到我们使用 ***video.pipe*** 函数和***fs . createwrtierstream***一起下载我们的视频。Video.pipe 只是以编码形式给出视频数据，fs 函数会将视频转换为 mp4 格式并保存到您的 PC 上。*

# *下载播放列表*

*第一部分非常容易开发，我们的下载器将面临的下一个挑战是下载一个带有****YouTube _ dl***的播放列表。让我们开始开发下载整个播放列表的 javascript 代码，我们将再次使用相同的模块来检查下面的代码。**

**正如您看到的代码，我们刚刚创建了一个名为 playlist 的函数，并传递参数“URL”，现在该 URL 将是我们要传递的播放列表 URL，接下来我们使用`*youtubedl()*`函数将其存储在 video 变量中，在下一行中，我们使用 video.on()函数(实际上是 youtubedl 函数)定义了一个错误处理程序。在该函数中，如果我们的程序在运行时出现任何错误或 bug，程序将返回 console.log 函数，说明错误并告知发生的错误类型。从第 14 行到第 19 行，如果你已经看到我们正在使用 video.on()函数，在这个函数中，我们将播放列表的大小信息存储在 size 变量中，然后`***path.join(__dirname + ‘/’, size + ‘.mp4’)***`将定义一个 size.mp4 视频大小，它将被我们从中获取的大小信息所替换。size()和下一行我们使用 fs 流模块在 javascript 文件所在的目录中输出我们的播放列表视频。从第 21 行到第 31 行，我们实际上在命令行上打印了一些东西，没错，我们让移动条与您在一些软件安装中看到的一样，移动条向右移动，告诉他们安装完成的百分比，但在这种情况下，我们使用它来告诉有多少视频数据被下载。我们使用 stdout()来制作这个 bar，它是 javascript 的一个内置函数。**

# ****获取视频信息****

**我们将进一步探索 youtube_dl 库，让我们开发一个 javascript 代码，它将在命令提示符下打印出视频信息。**

**第一件事首先需要(" youtube_dl ")在我们的 js 文件中加载库下一行我们使用可选参数(如果你不想的话可以忽略它)这个参数在 youtube 中保存你的登录细节。下一行是 youtube_dl 的新函数，我们正在使用 getInfo()，该函数接受 URL，选项参数，一个指示错误消息的函数，if(err)throw err JavaScript 条件，我们正在检查如果出现错误，只需抛出它并停止程序，否则移动到 if 条件体，该条件体使用 console.log 函数打印我们的视频信息。**

# ****使用代理:****

**有时，youtube 视频仅限于特定国家，因此为了避免这种情况，我们将在 youtubedl()函数中使用可选的代理参数，只需将代理和端口号作为参数与 youtube 视频 URL 一起传递。如果你检查下面的代码，你会更。**

# **下载字幕**

**当你的母语不是这种特定语言时，字幕是非常有用的，youtube 视频有视频上传者编辑的多个字幕，我们只下载视频而不下载字幕是不好的，youtube_dl 模块有一些下载视频字幕的功能。**

**让我们分解代码，了解其中发生了什么，像往常一样，第一步是在 Js 文件中加载模块接下来我们创建一个 URL 常量变量，在其中传递一个 youtube 视频 URL，然后我们创建 options 变量，保存 youtubedl()函数的可选参数，如果您注意到我在每个参数后添加了注释，如果您选中 format 是 ttml，您可以将其设置为 vtt， 两者都是字幕文件的格式接下来我们可以定义目录名(你可以在这里添加目录路径)，最后，我们调用 youtubedl()函数 getsubs()传递所有参数。**

**我们刚刚创建了一个很棒的 javascript youtube 下载器，你可以在 web 应用程序或任何基于 javascript 的应用程序中使用该代码，也可以在你的网站上使用。希望这篇文章有一天能帮到你。请随意分享你的观点。**