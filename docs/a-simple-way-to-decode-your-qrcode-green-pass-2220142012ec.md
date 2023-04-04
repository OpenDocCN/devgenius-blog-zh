# 解读二维码绿色通行证的简单方法

> 原文：<https://blog.devgenius.io/a-simple-way-to-decode-your-qrcode-green-pass-2220142012ec?source=collection_archive---------1----------------------->

重要信息:
-您的机器上必须安装 Node js。
-用于将图像从。png 到。jpg，如果你想使用脚本，你需要在 Linux 或 Mac 下(bash scripting)，在 windows 上你需要安装 git-bash。

![](img/8c349365c7c8c97d5a44e93bb0fd6165.png)

程序:【https://github.com/rodolphe37/qrcode-decoder】首先去回购地址下载必要的文件夹，在这个地址:

-从 repo 下载 decode-qr.zip 档案。

然后去这个地址的二维码-工具 app 地址:
[https://qr-code-tools.netlify.app/](https://qr-code-tools.netlify.app/)

你准备好了…我们走吧！

一边是“QrCode-tools”浏览器窗口，另一边是您的。zip 文件夹。

解压缩文件夹…

![](img/f5ace6fd304ea492e2ce592afbe266b8.png)

完成后，用代码编辑器(本例中是 VSCode)打开解压后的文件夹。

当你在根文件夹中(即在/decode-qr 文件夹中)时，输入“npm i”或“yarn”为你的计算机安装依赖项。

![](img/6ca035b433e22af65d425b2e849bc5e4.png)

接下来就是扫描二维码复印了。

![](img/ea0071ebef3dda0787d24232dac8044a.png)

在二维码工具应用程序中，点击网络摄像头选项卡，然后扫描您的二维码。

![](img/d29eb3e3f5b83b2cab22ac52017fa799.png)

您将看到一个密文，复制它并点击生成选项卡。
粘贴之前复制的文本，生成副本。

![](img/cc68f56ca1faa0d93c92537dd5ae5faa.png)![](img/6a03aa1e7c32cc520ac623feb311b3eb.png)![](img/50f7d8ccab5cdb46cc159e5c41160646.png)

然后右键点击二维码，将图片保存在 decode-qr/qrcodes 文件夹中(位置很重要，在继续之前确保图片在正确的文件夹中)。

![](img/8407ce0003db792ab328469acfb0933d.png)

在您的控制台中，通过键入“cd qrcodes”进入 qrcodes 文件夹。

![](img/6826bc964d3c553567476961e8c3de8c.png)![](img/61a992533ac92df81d4e4b01dce63993.png)

您现在位于包含新创建图像的文件夹中。
要转换图像(如果您可以在您的机器上运行 bash)，只需输入。/convert.sh(当然要确保你在正确的文件夹里，否则会给你一个错误信息)。

![](img/d930052627dcc4264b407bc13f115ae1.png)![](img/31a47ba6f0e75a046b6bac01772e458e.png)![](img/3aa0220cc70cc4f196db4ed90c56f62a.png)

转换后，文件夹中有两个图像，一个在。png 格式(从现在开始对我们没用了)和一个 in。jpg 格式(这是我们将在本教程的剩余部分使用的格式)。

在那里，您必须通过执行 cd 退出 qrcodes 文件夹..
(在本教程的剩余部分，您必须与 decode.js 文件位于同一个文件夹中，否则在下一阶段(解密内容的阶段)将出现错误消息)。

![](img/03bf8683215007cb6cea095d742d32b3.png)![](img/68a265812306f6af882bb87b9a884e26.png)

最后一步，通过键入“node decode.js”执行 decode.js 文件。

![](img/483b78b036223573ed0791e333c014b4.png)![](img/3369f48773cbabe5c74693929e0d4eff.png)

就这样，你所有的信息都被解密并可读了。

这就是练习的结束，本教程的目的不是给出伪造二维码的方法，它只是为了信息，对应于我们直接查看与我们有关的信息的权利。

享受这个世界。