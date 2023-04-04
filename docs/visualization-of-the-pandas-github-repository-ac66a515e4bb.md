# 熊猫 GitHub 知识库的可视化

> 原文：<https://blog.devgenius.io/visualization-of-the-pandas-github-repository-ac66a515e4bb?source=collection_archive---------11----------------------->

虽然我更多地从事分析方面的工作，但我会发表一篇关于数据可视化的文章。

# [GOURCE](https://l.workplace.com/l.php?u=https%3A%2F%2Fgource.io%2F&h=AT1-5YNfNPfts_DMyIQXzXziYwOA-M3J85fLdmoUax4CB7MvY_DmsmGEQqFF37nP2d_bFo1DeouPb_T5ILKiyp2UTAhwApBMdYX2oJla7Iqi5UsChXah7ZTp6Vfws2yTl4UR4LmeAtXdewdQUzyoW4sXeiQjyjGlMA) (在 [GITHUB](https://l.workplace.com/l.php?u=https%3A%2F%2Fgithub.com%2Facaudwell%2FGource&h=AT1-5YNfNPfts_DMyIQXzXziYwOA-M3J85fLdmoUax4CB7MvY_DmsmGEQqFF37nP2d_bFo1DeouPb_T5ILKiyp2UTAhwApBMdYX2oJla7Iqi5UsChXah7ZTp6Vfws2yTl4UR4LmeAtXdewdQUzyoW4sXeiQjyjGlMA) 上也有)

* **专业提示** brew 安装 gource 工作正常

* **另一个专业提示**:类似于应用程序 CopyClip，通过按下 **windows 键+ v** 打开剪贴板历史记录，如果你在将帖子或其他东西保存到其他地方之前删除它，这将是一个救命稻草。我得到了惨痛的教训。

Gource 是一个可视化工具，它基于项目不断变化的目录布局构建一个不断发展的力导向图结构。个体开发者的贡献被显示为在文件节点周围操纵的化身/图标，同时发射激光束，导致每个文件修改的布隆效应。

![](img/ba39b8d991ceba7ba43bc6cf0a2d0e14.png)

# 动画

动画显示(添加、修改、删除)文件随时间的交互。单个节点代表文件目录，叶节点代表文件，用编程语言进行颜色编码。

# 洞察力

可视化项目随时间的增长是很有趣的。作为一个团队，工作模式似乎是成员在项目之间跳跃，单独工作或与其他人并行工作。在另一个层面上，理解全部范围是充分学习的一半。能够说你能接住并扔出一个棒球是很棒的，但是你知道这个游戏是如何运作的吗？此外，以最快的方式共享信息变得越来越强大，因为没有人想阅读文档；因此，一个动画可以帮助个人在短短几分钟内了解结构的全貌。

# 创建视频

首先需要提取特定于 gource 的日志文件。请记住，您可以对这个日志文件进行大量的修改，比如删除 gitignore/updates 或 markdowns，但是为了简单起见，对于单个存储库，gource 可以创建这样的日志文件:

```
gource --output-custom-log repo.log my-repo-location
```

Gource 将使用这个日志文件来渲染一个交互式动画，或者生成一系列图像帧。

# 创建此帖子附带的视频

在搜索了一个有趣的库(你几乎可以使用 GitHub 上的任何公共库)和一点脚本来获得正确的日志文件后，我创建了下面的熊猫 python 库的演变动画。

# 步伐

*   我首先克隆了存储库
*   然后，为了执行，我创建了一个 bash 文件，包含该目录中的所有配置(不要评价我，我只是个人)
*   然后我使用 ffmpeg 创建 mp4 转换
*   然后我在文件中添加了音频(不包括在这篇文章中)
*   朋友(爱人)——晚上

**的最终**配置文件(是的，我可以做得更漂亮，但这样做很好)，看起来像这样:

```
gource \
-1280x720 -r 60 -e 0.5 \
--hide progress \
--hide filenames \
--font-size 2 \
--output-ppm-stream "pandas_gource" \
--seconds-per-day .01 --stop-at-end --font-colour 336699 \
--highlight-users \
--highlight-colour 999999 \
--auto-skip-seconds 1 \
--title "Pandas Library Repo" \
-user-scale 1 \
--bloom-multiplier 1.0 \
--bloom-intensity 1.0 \
--key \
--file-idle-time 0 \
--caption-size 10 \
--caption-duration .01 | \
ffmpeg -y -r 60 -f image2pipe -vcodec ppm -i pandas_gource -vcodec \ libx264 -preset fast \
-pix_fmt yuv420p -crf 1 -threads 0 -bf 0 pandas_gource.mp4 \
```

# 配置文件

[古尔斯配置文件](https://l.workplace.com/l.php?u=https%3A%2F%2Fgithub.com%2Facaudwell%2FGource%2Fwiki%2FControls&h=AT1-5YNfNPfts_DMyIQXzXziYwOA-M3J85fLdmoUax4CB7MvY_DmsmGEQqFF37nP2d_bFo1DeouPb_T5ILKiyp2UTAhwApBMdYX2oJla7Iqi5UsChXah7ZTp6Vfws2yTl4UR4LmeAtXdewdQUzyoW4sXeiQjyjGlMA)

# 问/答？

如果你对此感兴趣，请告诉我，我会在未来发布更多这类内容！

# 社交媒体:

*Instagram: @maxbade*

*GitHub: @maxwellbade*