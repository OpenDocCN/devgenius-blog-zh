# 使用 Fish 脚本下载迷因和 Tiktoks

> 原文：<https://blog.devgenius.io/download-memes-and-tiktoks-using-fish-scripting-e430fbddf744?source=collection_archive---------6----------------------->

问候书呆子👋今天我们将学习如何从网上下载内容，特别是 reddit，因为作为一名开发人员，它非常容易“破解”,所以如果你想下载迷因稍后发布到你的 discord 服务器或观看随机 tiktoks，这个博客是为你准备的

![](img/c69b4af690c05a2baeaeebb0dc21847f.png)

[https://media . istock photo . com/photos/friends-watching-smart phone-content-in-the-street-picture-id 1066622066？k = 20&m = 1066622066&s = 612 x612&w = 0&h = FHqqjEfbu-qbegzwx 6 kz 1 obm r5 b 37 u _ EJ 3 mzcahsavs =](https://media.istockphoto.com/photos/friends-watching-smartphone-content-in-the-street-picture-id1066622066?k=20&m=1066622066&s=612x612&w=0&h=FHqqjEfbu-qbEGZWX6Kz1OBMr5B37U_eJ3mZcaHSAvs=)

***今日菜单上的***

*   使用`curl`从 reddit 下载图片
*   使用惊人的`youtube-dl`下载视频
*   最后使用`crontab`来安排发布到我们的服务器

没有任何进一步的样板文件，让我们开始吧

***使用 curl 和 wget 从网络上下载内容***

在今天的终极一击中🍑我们有一个强大的命令行工具叫做`wget`，我们可以用它从网上下载视频和图像，如果我们给它提供任何 URL 或者一个带有 URL 列表的文件，很酷吧？

如果你有图片链接，你可以用它从 reddit 或任何其他来源下载图片，所以我们只需从 r/memes 下载一些 memes，你会发现这很简单

```
curl -s -A "cute bot" [https://reddit.com/r/memes/top.json](https://reddit.com/r/memes/top.json) | jq '.'
```

这里有一个巧妙的技巧来获得 JSON 格式的任何 subreddit 内容，您也可以使用`/subreddit/hot.json`、`/subreddit/new.json`或使用`?limit=`指定一个限制，最多只返回 101 个结果，接下来我们将输出通过管道传输到`jq`进行漂亮的解析

现在在浏览器中打开链接，查看 JSON 结构，因为我将只提取包含`jpg`、`png`或`gif`的链接，因为它们是我们从响应中看到的唯一链接

```
curl -s -A "handsome boy" [https://www.reddit.com/r/memes/hot.json](https://www.reddit.com/r/memes/hot.json) | jq -r '.data.children[].data.url_overridden_by_dest' | grep -Eoh "https:\/\/\i\.\w+\.\w+\/\w+(\.jpg|\.gif|\.png)"
```

如果对你来说没有任何意义，请看看我关于正则表达式的博客

现在让我们将它包装在一个漂亮的`for`循环中，使用`wget`下载所有提取的链接，所以我们的脚本现在应该看起来像这样

```
#!/usr/bin/env fish
for meme in (curl -s -A "handsome boy" [https://www.reddit.com/r/memes/hot.json](https://www.reddit.com/r/memes/hot.json) | jq -r '.data.children[].data.url_overridden_by_dest' | grep -Eoh "https:\/\/\i\.\w+\.\w+\/\w+(\.jpg|\.gifv|\.gif|\.png)")
   wget $meme
end
```

就这样，我们下载了 r/memes 热门版块中的每一个 meme

***使用 curl 和 youtube 下载视频-dl***

如果你不知道——很不幸，至少可以这么说— [youtube-dl](https://github.com/ytdl-org/youtube-dl) 是一个非常棒且非常受欢迎的命令行实用程序，用于从包括 youtube 在内的多个来源下载视频。当然，结合我们的 shell 脚本，我们将从 r/tiktokcringe 中提取视频链接，并使用 [youtube-dl](https://github.com/ytdl-org/youtube-dl) 实用程序下载它们

首先，让我们看看这些链接是什么样子的，这样我们就可以编写一个合适的正则表达式

```
curl -s -A "handsome bot" [https://reddit.com/r/tiktokcringe/top.json?t=week](https://reddit.com/r/tiktokcringe/top.json?t=week) | jq '.data.children[].data.url_overridden_by_dest'
```

这将为我们带来本周的热门帖子，你可以使用`day`、`month`或`year`

根据输出判断，在这种情况下我们似乎不需要正则表达式，因为我们不需要过滤掉任何内容，所以我们将继续下载这些链接

```
#!/usr/bin/env fishfor video in (curl -s -A "handsome boy" [https://www.reddit.com/r/memes/hot.json](https://www.reddit.com/r/memes/hot.json) | jq -r '.data.children[].data.url_overridden_by_dest'
   wget $video
end
```

现在，您的 tiktoks 就在当前目录中了

*家庭作业:通过提示用户进入输出目录，使脚本更具交互性*

今天的博客到此结束，我希望你们都学到了一些东西，如果你喜欢，请点击👏在底下

快乐编码