# 创建批处理文件以打开应用程序套件

> 原文：<https://blog.devgenius.io/creating-batch-files-to-open-app-suites-ba05911df291?source=collection_archive---------14----------------------->

![](img/793d60a5e2f72c06f0c570112ca0ba4b.png)

Emile Perron 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

为了让一天的代码更容易开始，并减少我打开每个应用程序的点击次数，我创建了一个. bat 文件，可以从我电脑上的不同位置打开几个重要的应用程序。这是我尝试的一个教程，因为我在网上找不到。

## 第一步:

*   *首先，决定您想要用 bat 文件打开的应用程序。*

对于这个例子，我想做几件事。

1.  打开 Semantic-Ui-React 网站，我用它作为参考。

2.启动 Visual Studio 代码

3.在我的编程目录中启动 Git-Bash

4.为歌曲启动 Windows 10 Spotify 应用程序

现在我有了我的应用程序列表，我可以继续第二步。

## 语义 Ui 反应

1.  抓取 [Semantic-React-Ui](https://react.semantic-ui.com/) 网址。

> [" https://react . semantic-ui . com/](https://react.semantic-ui.com/)"

*   注意，这必须有引号

2.打开记事本

3.文件开始于:

```
@echo off
```

*   这可以防止你的。bat 文件在处理时不显示字符

4.在下一行，添加:

```
start "semantic-ui-react" "[https://react.semantic-ui.com/](https://react.semantic-ui.com/)"
```

*   同样，必须有与此相同的引号
*   此命令会在您的浏览器中打开网站。
*   语法是:

```
start "website-name-in-quotes" "website-url-in-quotes"
```

5.最后，补充一句:

```
exit
```

*   这让你的文件知道结束，对于防止循环是很重要的

## 完成后的文件应该是这样的:

```
@echo offstart "semantic-ui-react" "https://react.semantic-ui.com/"exit
```

## 保存您的文件:

```
AppSuite.bat
```

*   您需要在保存前从`Save as Type`下拉菜单中选择`all files`

1.现在转到您的桌面视图。

*   右键单击背景
*   从`New`下拉菜单中选择`New Shortcut`
*   当快捷向导打开时，点击`browse`并选择您新保存的`AppSuites.bat`文件。

(如果您不记得您的文件保存在哪里，只需打开`AppSuites.bat`，点击`File`并选择`Save As`，在点击`cancel`之前记下保存位置)

*   键入快捷方式的名称。我也选择称它为 AppSuites，但是你可以随意命名。没有扩展名，只有名字。

2.一旦创建了快捷方式，在桌面上找到它并右击它。

*   你会看到`Target`被设置为`AppSuites.bat`这是完美的。
*   更改`Start In`选项以反映您通常编码的目录。
*   对我来说，我把我所有的项目保存到一个指定的目录中，所以我使用了这个目录。
*   这一步对于我们以后要添加的文件来说更重要，但是现在最容易实现。未来的我们工作更少！

3.完成后，双击新的 AppSuites 快捷方式。它应该向 Semantic-Ui-React 网站开放。

## Visual Studio 代码

**这一步你首先需要知道 Visual Studio 代码在你的系统中的位置。**

1.  按下键盘上的`Windows`键。
2.  没有点击！！键入 Visual Studio 代码
3.  右键单击 Visual Studio 代码，然后单击`Open file location`
4.  在打开的`File Explorer`窗口中右击 Visual Studio 代码，然后点击`Properties`
5.  从属性窗口中复制`Target`字符串

*   我电脑上的字符串是:

```
"C:\Users\Me\AppData\Local\Programs\Microsoft VS Code\Code.exe"
```

6.有了这个字符串，将这一行添加到`AppSuite`，在 Semantic-Ui 行的下面:

```
cd C:\Users\QuinnCalhoun\AppData\Local\Programs\Microsoft VS Code\ && start “” “Code.exe”
```

*   注意引号！它们必须完全相同
*   第一组是一个空字符串，表示一个名称，并且必须在那里

7.保存你的文件，你就可以开始了。`AppSuite.bat`现在应该是这样的:

## AppSuite.bat

```
@echo offstart "semantic-ui-react" "https://react.semantic-ui.com/"cd C:\Users\Me\AppData\Local\Programs\Microsoft VS Code\ && start "" "Code.exe"exit
```

*   你的`AppSuite.bat`可能有一条不同的路径通向 Vs 代码

# 饭桶狂欢

这一过程与我们刚才所做的相似，但不完全相同。不要急！

1.  因为 Git-Bash 自动在调用它的目录中打开，所以我们不需要目标。相反，导航到
    `C:\Program Files\Git\git-bash.exe`

(如果在`Program Files`中没有`Git`目录，可以在`Program Filesx86`中找到)

2.找到`git-bash.eve`文件后，复制路径并返回`AppSuite.bat`

3.添加这一行:

```
cd C:\Users\QuinnCalhoun\Desktop\’YourCodeDirectory’ && start "" "C:\Program Files\Git\git-bash.exe"
```

*   其中“YourCodeDirectory”是一个目录，没有引号
*   必须是目录/文件夹

4.这应该结束了。这会将目录更改为您的编码目录，并在那里调用 git-bash。

`AppSuite.bat`现在看起来是这样的:

```
@echo offstart "semantic-ui-react" "https://react.semantic-ui.com/"cd C:\Users\QuinnCalhoun\AppData\Local\Programs\Microsoft VS Code\ && start "" "Code.exe"cd C:\Users\QuinnCalhoun\Desktop\QuinnThomasCalhoun && start "" "C:\Program Files\Git\git-bash.exe"exit
```

# Spotify

## 第一阶段:

这是最困难的一项，因为 Spotify 是我电脑上的 windows 10 应用程序，因此驻留在一个名为 WindowsApps 的隐藏文件夹中。不要急！

1.  导航至`C:\Program Files`
2.  你会看到一个名为`WindowsApps`的灰色文件夹
3.  右击`WindowsApps`并点击`Properties`
4.  点击`Security`选项卡
5.  点击`Advanced`
6.  您现在会看到`Advanced Security Settings`窗口
7.  点击显示`Owner`的右侧的`Change`徽章
8.  你现在会看到`Select User or Group`窗口
9.  在显示`Enter the Object Name to Select`的输入框中输入`Users`并点击检查姓名。系统将自动填充该对象。
10.  点击`OK`
11.  确保您选中了标记为`Replace owner on subcontainers and objects`的框，否则您将能够打开`WindowsApps`，但除此之外无法打开任何文件夹或文件
12.  点击`Apply`然后点击`Ok`关闭窗口
13.  现在，您应该能够导航到 WindowsApps 目录。
    干得好！

## 第二阶段:

1.  在`WindowsApps`的底部，你会看到一个名为
    `SpotifyAB.SpotifyMusic_1.134.694.0_x86__zpdnekdrzrea0`的目录
2.  单击此目录，从窗口顶部的导航栏复制路径。它应该是这样的:

```
C:\Program Files\WindowsApps\SpotifyAB.SpotifyMusic_1.134.694.0_x86__zpdnekdrzrea0
```

*   很抱歉这一行的格式很奇怪，但那是一行

3.现在回到你的`AppSuite.bat`文件

4.添加这一行:

```
cd C:\Program Files\WindowsApps\SpotifyAB.SpotifyMusic_1.134.694.0_x86__zpdnekdrzrea0 && start "" "Spotify.exe"
```

*   再次，注意引号，这是一个严格的格式
*   不要忽略表示名称的空字符串

5.**搞定**！您的整个`AppSuite.bat`文件应该如下所示:

编辑:我开始意识到，每当 Spotify 应用程序修补到新版本时，它都会改变本例中使用的路径，因此当您的 bat 文件在 Spotify 上中断时，只需重复 Spotify 步骤即可获得新路径。

```
@echo offstart "semantic-ui-react" "https://react.semantic-ui.com/"cd C:\Users\QuinnCalhoun\AppData\Local\Programs\Microsoft VS Code\ && start "" "Code.exe"cd C:\Users\QuinnCalhoun\Desktop\QuinnThomasCalhoun && start "" "C:\Program Files\Git\git-bash.exe"cd C:\Program Files\WindowsApps\SpotifyAB.SpotifyMusic_1.134.694.0_x86__zpdnekdrzrea0 && start "" "Spotify.exe"exit
```

只需双击我们之前创建的快捷方式，即可启动所有程序。

*   您应该看到 git-bash 窗口在所选的
*   您应该在默认浏览器中打开 semantic-ui-react 网站。(如果您的浏览器已经打开，它将在浏览器窗口中创建一个新选项卡)
*   您应该打开 Visual Studio 代码
*   你应该打开 Spotify

## 感谢尝试我的教程，第一次为我做这样的东西，所以任何关于可读性或清晰性的指示或提示都是非常有价值的！