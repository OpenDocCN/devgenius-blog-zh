# 与“哦，我的 ZSH”和“伊特姆”一起进行终端改造。

> 原文：<https://blog.devgenius.io/terminal-makeover-with-oh-my-zsh-and-iterm-fb47e74a8ca9?source=collection_archive---------11----------------------->

## 编码工具

## 用 iTerm2 替换 Mac 默认终端应用程序的可视化分步指南。

![](img/54c7d21612031a782a72110845faa9c3.png)

照片由 Safar Safarov 在 Unsplash 上拍摄

*这个周末，我决定把我的 Macbook Pro 恢复到出厂设置，这样我就可以从头开始设置编程环境了。*

在本帖中，我们将在 Mac 上设置 oh-my-zsh 和 iTerm2。

*这是最终结果的样子:*

![](img/46f9b35b2b5bcef98647561f2a8b4688.png)

我们开始吧！

![](img/efdf223d66628ce0e6263aa74f6ecc42.png)

按`CMD + SPACE`调用 spotlight 服务。

![](img/a213a85dc0c1ae6dab7b68b4774f80e2.png)

开始输入“终端”,你应该会看到下面类似的内容。

![](img/58839daa8d353a12e27a1e3477a91def.png)

按下`ENTER`键(当然是轻轻的)打开终端应用程序。

如果您看到“默认的交互式 shell 现在是 zsh…”这意味着您仍然在使用 bash 作为您的 shell。

![](img/d12b14941a15d856f7ea88a38c759099.png)

我们换成 zsh 吧。

点击“终端”并选择“首选项…”如下所示。

![](img/640311c79dd44f0af73c4c822269ac43.png)

这将打开终端设置窗口。

![](img/e9d330dc4c7cd4ecea0bad5d4c097e37.png)

在“shell 打开方式”部分，单击“默认登录 shell ”,如下所示。

![](img/3a6ce97e274ec9b94958fee78cf884bd.png)

点击左上角的“X”关闭窗口，然后重启终端。您现在应该看到终端使用 zsh，如下图所示。

![](img/d71be300833e10dfb7621180c2a0c496.png)

# 安装电力线字体

主题“不可知论者”将需要一些特殊的字体来正确渲染。让我们现在安装它们。

在终端中键入以下命令:

```
git clone [https://github.com/powerline/fonts.git](https://github.com/powerline/fonts.git) --depth=1
```

![](img/b4833c343b600a2ef3ba17214a3667a7.png)

然后下面要改变的目录:

```
cd fonts
```

![](img/d9cceef93874b9c94ef480a233c33bb0.png)

该目录将更改为`~/fonts`，如下所示。

![](img/08d37d129e31c0a220a1f52ca32ab9f2.png)

键入以下命令将字体安装到您的系统中。

```
./install.sh
```

![](img/a750a8ae7d12442b738863ed52481550.png)

输出应该如下所示。

![](img/756acc80e5d5379a4725a00a58ef08ac.png)

让我们备份到父目录，以便进行一些清理:

```
cd ..
```

![](img/698ba8080a6c50081139fc86172ce274.png)

您应该看到下面的输出，指示主目录。

![](img/31c48cea79721e1c3ba1313f389c2129.png)

让我们使用以下命令删除安装文件夹:

```
rm -rf fonts
```

![](img/187278d2e6d2bd30b9458afd9ca1f621.png)

现在应该删除 fonts 文件夹了。让我们清除控制台输出。

```
clear
```

![](img/2e55e395d51e8790f85986e7fb00cfcb.png)

您应该会在控制台上看到一个清晰的窗口，如下图所示。

![](img/ea65cc93893b39a435cc5566092609df.png)

# 安装我的 ZSH

天啊，ZSH 负责我们的 zsh shell 的配置。让我们现在安装它。

在终端中键入以下内容(不要使用任何换行符，应该只有一行):

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)
```

![](img/550c3674a0aa96d79468b18b6024cefc.png)

现在，您应该会看到 oh-my-zsh 安装在您的计算机上。

![](img/d5a0d4787e1a6413dab45908c0a16d7c.png)![](img/1bc98c80759ae022da73ca9aa5f52b22.png)

如果您看到一条消息说“检测到不安全的依赖于完成的目录”，我们需要在。主目录下的 zshrc 文件。

为此，请打开 Finder 窗口并导航到主目录。

![](img/dc391a39603066785b5ea26d107b87d2.png)

按`SHIFT + CMD + .`显示隐藏文件。您现在应该会看到类似下面的内容。

![](img/c4e8c84868974e92e548785044625b91.png)

打开。zshrc 文件使用类似 [Sublime](https://www.sublimetext.com/3) 的文本编辑器。

这就是里面的。zshrc 文件看起来像:

![](img/516a1e65a7e21636ff4f8901e65e8fe3.png)

向下滚动第 73 行。

![](img/83291452e6b0fbe35da37b194fb1f1e4.png)

在 source $ZSH/oh-my-zsh.sh 之前插入下面一行:

```
ZSH_DISABLE_COMPFIX="true"
```

![](img/f595de452e14d029af65a5c359b4a06b.png)

保存并关闭。zshrc 文件，并打开一个新的终端窗口。您应该看到类似下面的内容。

![](img/9c398f2dea8ef56a49a38699b75b1b19.png)

# 替换默认终端

去 https://www.iterm2.com/version3.html 的[下载最新版本。](https://www.iterm2.com/version3.html)

![](img/76df27170bc31c911934620f85e8e1d1.png)

将安装程序保存在“下载”文件夹中，如下所示:

![](img/42afeebca3c4cef8dddacb225e6efa50.png)

打开新的 Finder 窗口并导航到“下载”您应该会在下面看到类似的内容。双击 zip 文件，它应该提取 iTerm 应用程序的一个实例。

![](img/d93159c95e7a497a99d737f6e9f337e0.png)

双击“iTerm.app”

![](img/b0d050ec921cad0201f1ff27f857fdc9.png)

如果提示从互联网下载应用程序，请点按“打开”

![](img/33b711fab43f2a40a6f733f85c745740.png)

如果提示将应用程序移动到应用程序文件夹，请点击“移动到应用程序文件夹”

![](img/f06bafcffbfe97eff54d50034a7bfbd8.png)

关闭所有窗口，按 CMD + SPACE 调出 spotlight 搜索服务，输入“iterm”点击回车键，你现在应该看到 iTerm 应用程序。

![](img/fe84e3d102d7c8db0526322e8c9488e0.png)

打开 Finder 窗口，导航到主目录，然后找到。zshrc 文件。

![](img/811da148eb93a66f6141dad2deced595.png)

打开。使用文本编辑器编辑 zshrc 文件。

![](img/16f735db6475f22a1763f033235af40b.png)

找到 ZSH_THEME="robbyrussell "并将" robbyrussell "替换为" agnoster ",如下所示。

![](img/93317ba36a4be1f34c4d32b5b6bed1ef.png)

保存并关闭文件。按下`CTRL + Q`关闭任何剩余的打开的 iTerm 窗口。

按下`CMD + SPACE`并输入“iterm”重启 iTerm，如下图所示。

![](img/b4a613631396b7d9df4bc4972d315239.png)![](img/4a2eefb1fd2f7ca31560ebeeb5e3981a.png)

点击`ENTER`键，一个新的 iTerm 窗口应该会像下面这样打开。

![](img/628e5efb46f66a6ee2fc409bdfdf5a05.png)

提示看起来有点怪。让我们修理它！

进入 iTerm2 并选择“首选项…”如下所示。

![](img/d33adf2fe594a1a4a16ff0668be8753d.png)

您将看到类似下图的内容。

![](img/5d793741f00589e93589da6fa58ddf33.png)

点击“个人资料”

在“标签>”旁边的配置文件名称区域下方的窗口左下角找到“+”

![](img/873c0f60b0a5a29047573274807ccdfa.png)

点击“+”号。

![](img/7da610ea114c18259443928cd86d594b.png)

在“常规”选项卡的“基本”区域下，将默认的“新配置文件”名称替换为您喜欢的配置文件名称。在下面，我输入了“青铜色”

![](img/cd01e62bae0f9968ccf2ccd137285141.png)

在“标题”中，单击下拉菜单，选中或取消选中您对窗口标题外观的首选项。

![](img/32f504955432ae467103ee219f67ab24.png)

导航到“颜色”选项卡，点击窗口右下角的“颜色预设…”下拉菜单，然后选择“Smooooooth”

![](img/7a3d70fe09eda8b5ed939c44bcd7efb3.png)

在“基本颜色”部分找到“背景”，设置颜色为 R:0 G:50 B:150，如下图所示。

![](img/fe5190a6d656facc375f236bf3a35e0b.png)

导航到“文本”选项卡，找到“字体”部分。选择任何电力线字体。下面，我选择了“Roboto Mono Medium for Powerline”并将字体大小增加到 13。

![](img/4572587d7dd665dedc5e6c0c710e9d7e.png)

在同一个“字体”部分下，选中“对非 ASCII 文本使用不同的字体”,并选择与之前相同的字体。参考下图。

![](img/fc55f3b9b9d8f51d12adbe3302a65962.png)

接下来，导航到“窗口”标签，并设置透明度和模糊如下所示。

![](img/2e147665b72c38914de64f30e0aa26e5.png)

然后，导航到“终端”标签，并选择“无限回滚”

![](img/6542b9647ea325125f2e7efa4130470e.png)

最后，让我们通过单击“Other Actions…”下拉菜单并选择“Set as Default”来将这个新创建的配置文件设置为默认值，如下所示。

![](img/e6a28216f8ac3ee66f2da74e7ec2ed7a.png)

现在，您应该会在新创建的配置文件旁边看到一个星号，表明它是新窗口的默认配置文件。

![](img/b259cdfb50fe229adc0146e97da21c59.png)

重启 iTerm，你应该会看到类似下面的东西。

![](img/274dc173fd68b12f71e2d354ca3ab073.png)

请注意，我们几乎看不到提示符上的目录指示器。还有，用户名@主机名对于喜欢来说有点长。让我们解决这些问题。

再次转到 iTerm 首选项，并导航到“Profiles”选项卡。在“正常”列下的 ANSI 颜色中找到“蓝色”,然后单击彩色框。

![](img/d8293cef15b903f8a743070bfb51ac1e.png)

如下所示，将 RGB 值设置为 R:0 G:200 B:250。

![](img/978606518abcf5ad363157ebcdb88556.png)

按下`CMD + Q`退出 iTerm 并打开一个 Finder 窗口。导航到主目录，用`SHIFT + CMD + .`显示隐藏的文件并双击。哦，我的“文件夹。

![](img/d55243ef62f6998f94d7709aa9710591.png)

导航并点击“主题”文件夹。

![](img/66ad7438f0ee5ae3b1483d7843e2896f.png)

查找“agnoster.zsh-theme”文件，并使用文本编辑器打开它。

![](img/9230fcd633549fc983b7f80f4274596e.png)

这是主题内部的样子:

![](img/75cd1454f382b1b5206801c69108b4e6.png)

在第 92 行，查找“%n@%m”字符串。

![](img/600f9a65fa9ee72e7e848237949694a7.png)

选择“%n@%m ”,并将其替换为您希望在提示上显示的任何内容。

![](img/d061b962ceeb60fe7f568781dad499b1.png)

下面，为了简洁起见，我简单地将“%n@%m”替换为“Dd”。

![](img/1a1cb16b2288d8c46e160058d59978f5.png)

重启 iTerm，你应该得到类似下图的东西。

![](img/061a69cb2f948f4429df27b7ab73d365.png)

如果您导航到一个 git 存储库，您将看到类似下面的内容:

![](img/9a806a774f82887bed3814937998a62b.png)

就是这样！

编码快乐！

*原载于 2020 年 6 月 14 日*[*【https://blog.ednalyn.com】*](https://blog.ednalyn.com/2020/06/13/terminal-makeover/)*。*