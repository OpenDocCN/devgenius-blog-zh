# 在 R 中创建你自己的调色板函数

> 原文：<https://blog.devgenius.io/create-your-own-color-palette-function-in-r-64c8f21fc786?source=collection_archive---------12----------------------->

我偏离了我在发展主题中的常规数据，分享了一个迷你激情项目的演练——结合了我最喜欢的三样东西:香港、电影和 R(我的猫这次没有入选)。

首先，如果你看过马丁·斯科塞斯(Martin Scorsese)的《无间道》(T1)(2006)，那么你一定知道它是根据一部划时代的港片《无间道》(T3)(2002)改编的。如果没有，或者你没有看过《T4》和《T5》电影，没关系，除了黄金时代的香港电影和一个关于身份挣扎和背叛的史诗故事，你不会错过任何东西。现在你知道了。

《无间道》只是香港电影中众多有影响力的作品之一。武术序列启发了我们今天所知道的经典(《黑客帝国》T8，以及塔伦蒂诺的大部分作品)。我想找到一种方式，通过色彩来体现这座城市的活力。所以，我根据一些我喜欢的电影的剧照创建了一个 R 调色板函数。

![](img/610c92edd47de4c9194848c09de0586a.png)

地狱的调色板

![](img/9508423fe993608459cf31f52dc0dd07.png)

胭脂的调色板

这篇文章将介绍如何在 R 中创建一个调色板函数。我的目的是帮助你掌握 R 基础，如果你是一个初学者，或者像我一样通过硬编码来学习。了解函数、循环、索引、结构和语法的基础知识将使编码变得更加容易，定制自己的调色板函数是一个很好的起点。结束时，您将学习如何定制自己的调色板，并在图表中调用它们，就像您调用现有调色板一样，如 *RColorBrewer* 或 *viridis* 。

本演练改编了 grDevices()中的代码，并受益于 Libby Benso 的教程。要复制的完整代码在我的 Github 上。

# **步骤一。定义功能**

首先，我们将建立一个带有两个参数的空函数:n =用户输入他们期望使用多少颜色值。Name 是基于五个选项的调色板选择的受限用户输入。

```
library(grDevices)

hkcolors <- function (n, 
name=c("rouge", "happytogether", "infernal", "moodforlove", "chungking")) { 
}
```

# 第二步。创建调色板

我创建了包含五种颜色的五个对象。它们使用红绿蓝(RGB)组合来识别。你可以使用许多在线工具找到你想要使用的颜色的 RGB 值。输出保存为十六进制代码。

每个调色板必须与您在参数中定义的名称完全匹配。这是必要的，以便 R 可以解析来自用户选择的字符串参数。然后，col2rgb()函数将十六进制代码转换为 RGB——我们在下一步中按照这种格式进行插值。

```
hkcolors <- function (n, 
name=c("rouge", "happytogether", "infernal", "moodforlove", "chungking")) {  
  rouge = rgb(c(217,242,217,115,191), c(130,223,153,34,83), c(162,235,130,26,73), maxColorValue = 255)
  happytogether = rgb(c(213,242,242,191,89), c(217,233,154,85,18), c(138,99,46,23,2), maxColorValue = 255)
  infernal = rgb(c(84,63,138,173,8), c(115,82,176,212,13), c(140,89,191,217,11), maxColorValue = 255)
  moodforlove = rgb(c(38,140,217,191,89), c(1,35,115,81, 23), c(4,44,26,17,8), maxColorValue = 255)
  chungking = rgb(c(202,90,106,217,140), c(89,39,131,32,17), c(9,140,171,7,17), maxColorValue = 255)

  name = match.arg(name) # Match possible names with the user-selected one

  orig = eval(parse(text = name)) # parse the string

  rgb = t(col2rgb(orig))  # col2rgb() hex code to rgb, t() saves this to a matrix
}
```

# 第三步。在定义的颜色之间插值

因为我们只为每个调色板设置了五个颜色值，所以当用户绘制连续变量或需要五种以上的颜色时，我们需要光谱中有更多的颜色。创建的 RGB 组合数量将基于用户输入的 *n* 。每个 R-G-b 都在这个过程中循环。

最后，通过索引 R-G-Bs 中的行值来创建一个 palette 对象(参见该函数第一行中的类似格式)，并设置该函数来返回 palette。

```
hkcolors <- function (n, 
name=c("rouge", "happytogether", "infernal", "moodforlove", "chungking")) {  
  rouge = rgb(c(217,242,217,115,191), c(130,223,153,34,83), c(162,235,130,26,73), maxColorValue = 255)
  happytogether = rgb(c(213,242,242,191,89), c(217,233,154,85,18), c(138,99,46,23,2), maxColorValue = 255)
  infernal = rgb(c(84,63,138,173,8), c(115,82,176,212,13), c(140,89,191,217,11), maxColorValue = 255)
  moodforlove = rgb(c(38,140,217,191,89), c(1,35,115,81, 23), c(4,44,26,17,8), maxColorValue = 255)
  chungking = rgb(c(202,90,106,217,140), c(89,39,131,32,17), c(9,140,171,7,17), maxColorValue = 255)

  name = match.arg(name) # Match possible names with the user-selected one

  orig = eval(parse(text = name)) # parse the string

  rgb = t(col2rgb(orig))  # col2rgb() hex code to rgb, t() saves this to a matrix
  temp = matrix(NA, ncol = 3, nrow = n) # create an empty matrix based on user input of number of colors
  x = seq(0, 1, , length(orig)) # create an equal range of sequence of numbers (5 total, b/c of the range of the palettes)
  xg = seq(0, 1, , n) # create an equal range of sequence of numbers (n based on user input)
  for (k in 1:3) {
    hold = spline(x, rgb[, k], n = n)$y # interpolate each of r,g, and b numbers in between default range
    hold[hold < 0] = 0 # replace any values less than 0 as 0
    hold[hold > 255] = 255 # replace any values more than 255 as 255
    temp[, k] = round(hold)
  }
  palette = rgb(temp[, 1], temp[, 2], temp[, 3], maxColorValue = 255) # final color palette, where each row is r,g,b
  palette
}
```

# 第四步:测试一下

[*数据集*](https://stat.ethz.ch/R-manual/R-devel/library/datasets/html/00Index.html#B) 包包含一个小样本数据集的集合。它们对于像这样的预演或测试代码非常有用。我用的是橘子树(Orange)生长的样本数据。

![](img/dbeb17c8ac4ef1a73380225b0aca9bfd.png)

样本数据集，橙色

下面的函数为每个调色板创建并保存相同的图表。因为我已经将我的调色板函数定义为 hkcolors()，所以我将该命令保存到 *pal* (调色板的简称)中，并可以像通常在 ggplot()的 scale_color_manual()中那样调用它。

```
plot <- function() {  
  palettes <- c("rouge", "happytogether", "infernal", "moodforlove", "chungking")
  Orange <- Orange # read the sample data
  Orange$Tree <- factor(Orange$Tree, levels=c(1, 2, 3, 4, 5)) # reorder trees
  for (i in palettes) {
    pal <- hkcolors(n=5, name=i)

    plot <- ggplot(Orange) +
       geom_line(aes(x=age, y=circumference, color=Tree)) +
       scale_color_manual(values=pal) +
       theme_minimal() +
       labs(x="Age", y="Circumference")

    print(plot)

    ggsave(paste0(i, ".png"))

    ggsave(paste0(i, ".pdf"))
  }
}
plot()
```

为了简洁起见，我粘贴了调色板的一个示例输出， *happytogether，*基于王家卫 1997 年的爱情剧*。*其他样本输出为[此处为](https://github.com/yining-w/hkcolors/tree/main/sample)。

![](img/f8caa4c7d6557a183c27c66fc11f89ff.png)

快乐在一起(1997)调色板

**图表:**

![](img/1ded47c05df3d0722a0add410a7a9c6a.png)

使用“快乐在一起”调色板，按年龄和树来显示桔子树的生长情况。

就是这样！您能够使用非常少且简单的代码行创建五个自定义调色板。希望这有助于你更好地理解 R 结构。如果你对创建自己的定制调色板感兴趣，我推荐丽莎·夏洛特·穆思的这本[关于数据可视化中颜色决策的简明指南](https://blog.datawrapper.de/colorguide/)，以及辛西娅·布鲁尔的[这个工具](https://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3)来帮助你选择可访问的调色板。

感谢您的阅读，如果这是有用的，或者如果您想在未来阅读类似的内容，请联系我。