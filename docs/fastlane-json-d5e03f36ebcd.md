# 浪子 Json {🚀}

> 原文：<https://blog.devgenius.io/fastlane-json-d5e03f36ebcd?source=collection_archive---------3----------------------->

## 用 fastlane 处理 json

![](img/a25b553168e5412415db0ec0263f70b6.png)

好吧，没什么大不了的，只是 fastlane 的一个处理 json 文件的插件，但是看看它有多容易使用😄

# 首先为什么？

fastlane 是一个开源库，旨在自动化 Android 和 iOS 部署，但你可以扩展它并创建出色的插件来简化你自己和他人的开发过程。

在 Ruby 中处理 json 一点也不困难，但是这个插件的目的是让你的生活更轻松，所以我创建了 [fastlane-plugin-json](https://github.com/MartinGonzalez/fastlane-plugin-json) 来集中 json 处理。

# 正在安装

```
fastlane add_plugin json
```

在您的快速文件中，您可以开始使用它。

# 行动

## read_json

我猜你猜到了！这个动作读取一个特定路径的 json 文件，输出是一个 Ruby 散列。简单吧？

好吧好吧！我没有让你眼花缭乱，让我们看看下一个…

## write_json

Annnnd 是的…我猜你已经知道了…你有一个散列和一个文件路径，不管它是否存在，你可以把那个散列作为一个 json 文件保存到磁盘上。

哈！没有吗？一点也不？好吧……难缠的人群。🤡

## merge_jsons

哦，这个…耶！你有 **N** 个 json 文件，你想把它们都合并吗？你想要合并哈希？您想将合并后的散列保存为 json 吗？没问题！

现在你必须承认那很酷。这是一个很酷的特性，我不知道我应该在哪里使用它，但是如果有一天我不得不合并 jsons 我知道怎么做。

## 下载 _json

好吧，如果你说这个行动没有用，我辞职！(请勿评论！)xD

3 行代码，这太疯狂了！

# 摘要

尽管这个插件做的任务很简单，但当我们谈论时间的时候，它真的很强大，简单但实用！

🗳 [储存库](https://github.com/MartinGonzalez/fastlane-plugin-json)🐦[推特](https://twitter.com/martin_g90)🍫 [Github](https://github.com/MartinGonzalez) 🚀 [Bundletool 插件](https://github.com/MartinGonzalez/fastlane-plugin-bundletool)🔗 [LinkedIn](https://www.linkedin.com/in/martin-gonzalez-90/)
◼️ [中型](https://medium.com/@gonzalez.martin90)