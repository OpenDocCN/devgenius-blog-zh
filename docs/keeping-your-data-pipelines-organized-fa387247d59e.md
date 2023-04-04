# 保持数据管道井井有条

> 原文：<https://blog.devgenius.io/keeping-your-data-pipelines-organized-fa387247d59e?source=collection_archive---------2----------------------->

*展示简单易用的数据工程师项目结构*

![](img/b758d0801de8ac3959d1392c5253f1f9.png)

由[威廉·费尔克](https://unsplash.com/@gndclouds?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 介绍

数据工程师项目可能会在一眨眼的时间里变得一团糟。由于这种项目有几十个(甚至几百个或几千个)文件，包括依赖文件、作业、单元测试、shell 文件，有时甚至还有 Jupyter 笔记本，所以初学者很难构建一个简单且易于维护的项目结构。

本文的目的是帮助数据工程师领域的新手，提出一个易于维护的项目结构，以使他们的生活更轻松。这个示例项目的源代码可以在 [this](https://github.com/flpn/data-engineering-project-structure) GitHub 资源库中找到。

# 管理您的依赖性

首先，我强烈推荐任何项目都要有一个像[诗](https://python-poetry.org/docs/)这样的依赖管理器(对于真正的家伙来说，太牛逼了！！！).所以，在你安装完诗歌并告诉他你应该在你的根目录下有一个依赖文件。

下面是一个用诗歌创建的依赖文件( *pyproject.toml* )的例子:

pyproject.toml

# 单元测试总是很重要

我们都应该知道，在我们的项目中实现测试可以让我们远离麻烦，并且大多数时候可以提高我们正在开发的软件的代码质量。在数据管道中，规则是一样的:总是实现单元测试！！！

因此，为了做到这一点，我们将创建一个名为 test ( *多么原始，嗯？*)并在其中放置一些测试。在这个例子中，我将使用 pytest 库来实现它们。这里有一个例子:

测试 _ 清理 _ 列. py

你现在可能会问自己:

> “等一下？？？那个*转换*模块是哪里来的？他还没有实施其中任何一项……”

没错。我没有！首先，编写一个失败的测试，然后为了通过单元测试而实现这个测试！那就是我们所说的 [TDD](http://www.jamesshore.com/v2/books/aoad1/test_driven_development) ！既然您已经理解了，让我们实现缺失的代码并通过测试吧！

# 将所有数据转换保存在单独的模块中

我们将数据转换逻辑嵌入到工作中，这是一种非常常见的做法。有时我们这样做是因为一天的匆忙，但是相信我，随着更多的代码行开始进入你的主流程，你会发现你的代码将变成一场噩梦。

因此，为了避免这种情况，我们将在项目的根目录下创建一个名为 *transformations* 的新文件夹，并将每个数据转换(python 函数、pyspark 转换…诸如此类的东西)放入其中。

这是在我们的单元测试中丢失的函数的实现。通常，我不喜欢把*类型的提示*放在我的代码中，但是因为这个函数可以在我的项目中到处使用(并且考虑到在一个项目中你通常和很多人一起工作)，我认为让事情尽可能清晰是一个好的实践。另一个改进是给这个函数添加一个*文档字符串*。

clean_data.py

# 实施您的工作

现在是时候实施我们的工作了！为了简化这里的事情，我不会向你们展示我添加的一些代码，但请记住，我创建了一个名为 *run* 的方法，该方法包装了包含在 *clean_data* 包中的所有转换。

同样，如果你想看到完整的代码，你可以检查这个库。这就是我们的工作:

保存 _ 清理 _ 数据. py

# 执行整个事情

因此，为了执行我们的测试和作业，我喜欢创建一些*。sh* 保持事物有序的脚本(*毕竟这是本文的重点，对吗？*)。另外，我已经下载了[这个](https://github.com/flpn/data-engineering-project-structure/tree/main/resources/inputs) CSV 文件，只是为了在一个真实的数据集中应用我的转换。

因此，为了给你一个例子，我将在这里留下两个脚本，它们可以用来测试和执行你的代码。脚本放在一个名为*脚本*的文件夹中。

运行测试. sh

run-save-cleaned-data.sh

# 我们最终的结构

在所有这些旅程之后，我们已经了解了如何构建一个包含依赖管理、单元测试和良好模块化的 Python 和 Shell 脚本的数据工程项目。最终的结构看起来像这样:

```
data-engineering-project-structure
├── pyproject.toml
├── jobs
│   └── save_cleaned_data.py
├── resources
│   ├── inputs
│       └── top_movie_genres.csv
├── scripts
│   ├── run-save-cleaned-data.sh
│   └── run-tests.sh
├── tests
│   └── test_clean_columns.py
└── transformations
    └── clean_data.py
```

就这样，伙计们！我希望这个指南能对那些在这个奇妙的数据工程世界中起步的人有用！如果有任何问题，请告诉我，再见！:)

*更多内容请看*[*blog . dev genius . io*](http://blog.devgenius.io)*。*