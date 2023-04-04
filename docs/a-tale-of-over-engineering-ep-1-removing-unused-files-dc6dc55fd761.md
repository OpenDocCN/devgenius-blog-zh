# 重新发明轮子。1:删除未使用的文件

> 原文：<https://blog.devgenius.io/a-tale-of-over-engineering-ep-1-removing-unused-files-dc6dc55fd761?source=collection_archive---------16----------------------->

![](img/7826f059f64df4ec2f0e40f4d9618837.png)

照片由[妮可·德·霍斯](https://burst.shopify.com/@ndekhors?utm_campaign=photo_credit&amp;utm_content=Picture+of+Hands+Typing+Code+On+Laptop+%E2%80%94+Free+Stock+Photo&amp;utm_medium=referral&amp;utm_source=credit)从[爆](https://burst.shopify.com/onlinelearning?utm_campaign=photo_credit&amp;utm_content=Picture+of+Hands+Typing+Code+On+Laptop+%E2%80%94+Free+Stock+Photo&amp;utm_medium=referral&amp;utm_source=credit)

骑了 15 分钟的摩托车到办公室后，没有什么比一杯新鲜的矿泉水更好了。这是一个阳光明媚的星期一早晨，甚至没有一丝即将下雨的迹象。很明显，在我的背包上没有防雨罩的情况下骑车是个不错的天气，但是没有手套和防晒霜就不适合骑车了。

23 GB 免 500 GB。如果不是因为丢失的文档和我 mac 上 documents 文件夹中每个文件旁边的下载图标，我不会注意到这一点。当你在 PlatformIO 上反复使用 Node.js、Next.js、React、Arduino 等技术栈时，磁盘的可用空间量迅速下降就不足为奇了。

一个叫 ncdu 的工具告诉我哪个项目占用的空间超过百兆。它还让我逐个删除`node_modules`或`.pio`目录。

但是如何在打开 ncdu 的同时不用手动按键盘上的箭头键和 D 键就能快速删除全部呢？

作为一名软件工程师，将这一漫长而乏味的过程自动化的冲动高得离谱。但是，等到你有重新发明一种工具的冲动，而这种工具实际上已经存在了很长时间，只是后来才意识到这一点，并问自己“为什么我没有使用它？？?"。

是的，我写了一个 CLI 工具，用来删除当前工作目录下名称与 CLI 标志值相匹配的每个目录。

我有一段时间没用围棋了。我都不记得上次我用围棋做项目是什么时候了。所以，我用`go mod github.com/alwint3r/janithor`命令创建了一个项目。

是的，你没看错，请不要问我为什么选择这个名字。

首先，我需要一种方法:

*   获取当前工作目录
*   获取我想从命令行参数中删除的目录的名称
*   遍历当前目录树，收集我想要删除的目录的绝对路径，并将它们放在一个字符串片(数组)上
*   迭代切片并从文件系统中实际删除每个条目。

为了获得当前的工作目录，我可以使用`os`包中的`Getcwd`函数

```
cwd, err := os.Getcwd()
```

然后，我使用这一行来获取我想要删除的目录名:

```
var folderName = flag.String("dir", "", "The folder that will be removed");
```

我定义了一个函数，可以用来查找每个目录的绝对路径，该目录的名称存储在变量`folderName`中。

```
func walk(base, target string, entry fs.DirEntry, output *[]string) error {// ...}
```

该函数接受`base`作为当前工作目录或当前工作目录中的任何目录。`target`存储我想要删除的目录的名称，而`entry`参数保存当前工作目录的信息。最后，`output`参数是一个指向字符串数组切片的指针，这样我就可以添加我想要删除的目录的每个绝对路径。如果一切正常，那么函数将返回 nil，否则它将返回我使用的标准库包的函数返回的任何错误。

```
var fullPath string
if entry == nil {
    fullPath = base
} else {
    fullPath = path.Join(base, entry.Name())
}
```

接下来，我检查条目是否为零。如果是 nil，那么我假设 base 是当前工作目录或者`cwd`。否则，`fullPath`将被设置为`entry`的绝对路径。它可以是文件或目录。

```
if entry != nil && entry.IsDir() && entry.Name() == target {
    *output = append(*output, fullPath) return nil
}
```

然后，我检查`entry`是否是一个名称与我想要删除的目录的名称相匹配的目录。如果为 true，将绝对路径添加到输出切片并返回 nil，从而中断递归。

```
if entry == nil || entry.IsDir() {
    dirEntry, err := os.ReadDir(fullpath) if err != nil {
        return err
    } for _, dir := range dirEntry {
        walk(fullPath, target, dir, output)
    }
}
```

如果程序当前位于`cwd`或`cwd`内的任何目录，则读取当前目录的条目，并递归遍历每个条目，直到满足之前的停止条件。现在功能完成了。

以下几行位于 main 函数内部

```
deleteList := make([]string, 0)walk(cwd, *folderName, nil, &deleteList);for _, dir := range deleteList {
    err := os.RemoveAll(file)
    // .. error handling
}
```

我需要从`os`包中调用`RemoveAll`函数来删除一个目录及其内容。

就是这样！

为了简洁起见，这里没有列出完整的代码。如果您有兴趣，可以稍后查看 GitHub 资源库中的完整源代码。

它像预期的那样工作，我在 mac 上删除了几十个`.pio`和`node_modules`目录，并设法恢复了超过 10 GB 的可用空间。

是的，它可以工作，但是我知道我可以通过在终端窗口上运行这一行命令来完成同样的事情吗？

```
find . -type d -name .pio -exec rm -rf {} \;
```

我做到了。

在花不到 30 分钟的时间写自己的工具之前，我想到了吗？不，我没有。这种事经常发生。

作为一名软件工程师，我需要被提醒的是，我不必在每次遇到可以自动解决的问题时都制作自己的工具。必须有一个现有的工具来完成这项工作，并且很可能比我自己的工具好得多。

我后悔做了吗？绝对没有。

我设法重新学习如何用 Go 编程语言编码，并利用其丰富的标准库包。而你，一个随机而奇妙的互联网人，你知道我最后一次使用递归是什么时候吗？

没有吗？我也不知道。

感谢阅读！请不要犹豫给我你的反馈，尤其是对我的破烂英语。这对我意义重大。

 [## GitHub - alwint3r/janithor

### 从当前工作目录中删除目录及其所有内容。示例:移除。pio 目录这将…

github.com](http://github.com/alwint3r/janithor)