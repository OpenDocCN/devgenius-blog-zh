# 获取抖音数据

> 原文：<https://blog.devgenius.io/get-tiktok-data-d3e2f385365?source=collection_archive---------0----------------------->

![](img/9c2cfce33fe914639f30b36cbb2e6102.png)

照片由[你好我是 Nik🎞](https://unsplash.com/@helloimnik?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)号上[的 Unsplash](https://unsplash.com/s/photos/tiktok?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我将演示如何使用 python 3 轻松获取抖音数据。这是一个从抖音获得大量数据的非常快速和简单的方法，你可以用它来分析用户，趋势，标签等等！

如果你更喜欢视觉学习，看看我为使用这个 API 制作的视频。我将在那个频道发布更多与逆向工程网站相关的内容，如果你对此感兴趣，请订阅。此外，请随意查看我的另一个 [YouTube 频道](https://www.youtube.com/c/davidteather)，这是与编程和工程相关的更有趣的内容:)

**安装依赖关系**

要设置这个包，你只需运行`pip install TikTokApi`来安装本教程的所有依赖项。

要确保软件包正常运行，请运行以下脚本。

```
from TikTokApi import TikTokApi
api = TikTokApi.get_instance()
print(api.by_trending(count=1))
```

如果你把一本字典打印到你的屏幕上，那么它就工作了！

> 如果你遇到任何错误，请查看[抖音 API 的 GitHub](https://github.com/davidteather/tiktok-api) ，最常见的问题是你需要全局安装 [chromedriver](https://chromedriver.chromium.org/downloads)

**示例脚本**

获取热门 TikToks

```
from TikTokApi import TikTokApi
api = TikTokApi()
results = 10 
trending = api.trending(count=results)
```

下载流行歌曲

```
from TikTokApi import TikTokApi
import os
api = TikTokApi()
results = 10
directory = "trending# Creates the new directory
if not os.path.isdir(directory):
  os.mkdir(directory)trending = api.trending(count=results)
for tiktok in trending:
  video_data = api.get_Video_By_TikTok(tiktok)
  save_file = "downloads/{}.mp4".format(tiktok['id'])
  with open(save_file, 'wb') as tiktok_output:
    tiktok_output.write(video_data)
```

通过标签获取 TikToks

```
from TikTokApi import TikTokApi
api = TikTokApi()
results = 10
hashtag = "funny"
trending = api.byHashtag(hashtag, count=results)
```

通过用户名获取 TikToks

```
from TikTokApi import TikTokApi
api = TikTokApi()
results = 10
username = "therock"
trending = api.byUsername(username, count=results)
```

搜索用户

```
from TikTokApi import TikTokApi
api = TikTokApi()search_term = "Joe"
users = api.search_for_users(search_term)
```

搜索标签

```
from TikTokApi import TikTokApi
api = TikTokApi()search_term = "funny"
hashtags = api.search_for_hashtags(search_term)
```

搜索音乐

```
from TikTokApi import TikTokApi
api = TikTokApi()search_term = "Little Bus"
music = api.search_for_music(search_term)
```

用户喜欢的 TikToks

```
from TikTokApi import TikTokApi
api = TikTokApi()username = "jw_anderson"
results = 30
liked_list = userLikedbyUsername(username, count=results)
```

> **注意:**如果用户的喜好设置为私人，该方法将返回长度为 0 的列表

详细的文档可以在[这里](https://github.com/davidteather/TikTok-Api#Detailed-Documentation)找到

如果你更愿意使用这个 API 作为服务，请查看 [RapidAPI](https://rapidapi.com/rapidapideveloper/api/tiktok2) 。这对于首选单个 HTTP 调用的应用程序来说是最理想的。