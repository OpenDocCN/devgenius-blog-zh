# 一个简单的语言检测应用程序

> 原文：<https://blog.devgenius.io/a-simple-language-detection-application-754b95084b14?source=collection_archive---------15----------------------->

这篇博客的目的是展示我如何在 Heroku 中创建和部署了一个简单的语言检测模型。

对于任何一种 ML 应用，数据都是第一步。即数据收集和数据清理，以便用于我们的目的。
这里是 kaggle 的 [**数据源**](https://www.kaggle.com/datasets/basilb2s/language-detection) 。

![](img/042b2c9615edcff4bc78272fa162b8e0.png)

数据集包含 10336 行和 2 列(10337x2)。“**语言**列为目标变量/因变量，“**文本**列为自变量。

![](img/eb87cc4b1459bdce92d25771cee29591.png)

在这里，您可以观察数据集中包含的语言数量。如果你观察的话，印地语只包含 63 条记录，我想再添加两种语言，比如泰卢固语、孟加拉语和印地语记录。所以，拿任何语言，让我们翻译成泰卢固语，孟加拉语和印地语。让我们用马拉雅拉姆语记录并翻译。

**Translators** 是一个使用 Python 将一种语言翻译成另一种语言的 API。你可以从这个博客了解更多: [**使用 Python 语言翻译**](https://towardsdatascience.com/language-translation-using-python-bd8020772ccc#:~:text=To%20do%20our%20translation%20we%20use%20the%20translate-api,APIs%20that%20are%20available.%20pip%20install%20translators%20--upgrade)

![](img/ffbc8bd8ffe0e880f81432d53fd2c439.png)

让我们将马拉雅拉姆语数据转换成泰卢固语、孟加拉语和印地语。

![](img/89eeed5d634ac925de12a4cf1ba5a237.png)

同样，转换泰卢固语和印地语，并将其添加到具有相应标签的原始数据中。

现在最终的数据集准备好了，我们可以从 ML 方法开始。

![](img/6174fed76d3683e0038bdc5c5705bb74.png)

虽然数据集是文本数据，但我们必须清除标点符号。

> def rempunc(text):
> for pun in string .标点:
> text=text.replace(pun，' ')
> text = text . lower()
> return(text)

# **TFIDF:**

在文本数据中，数据应该从字符串转换成矢量进行计算。TF-IDF 是术语频率逆文档频率的缩写。这是一种非常常见的算法，用于将文本转换为有意义的数字表示(向量),以适应预测的机器算法。它被称为规范化 tf-idf 表示的转换。它将计数矩阵转换为规格化或 tf-idf。Tf 称为频率项，而 tf-idf 称为频率时间。

![](img/c04bcc138f797f546c0dfcc928260a5c.png)

# N-Gram:

n 元语法指的是文本切割的粒度。当文本以记号的形式被切割时，它是由单个单词的集合的单个字母组成的。当文本被切割成几个词，两个，例如，这被称为二元等。这里，我们在这个场景中使用了二元模型。

我们可以用“ **ngram_range=(1，2)”**来定义二元模型

让我们为矢量化和学习算法创建一个管道，如逻辑回归(一种与 Rest 方式的多分类)、随机森林、额外树分类器和朴素贝叶斯。

![](img/1961c4bc04040161439d4ecff864c884.png)

因此，这里我们为 4 种分类器创建了 4 个管道。在这里，大多数人认为，由于 Sigmoid 函数的存在，逻辑回归是以二元分类的方式工作的。但是在多分类的情况下，它以一对多的方式工作。

![](img/f5a8d4d560461fb056f4e1f8324a5339.png)![](img/06969970f1fe2b5eeebb2eed29e64eb0.png)

这里，在随机森林、额外树分类器和 Bernoulies 朴素贝叶斯算法中，我们从逻辑回归得到了最好的准确度，达到 98.4%。

![](img/c889a1c27e3bd396858fc31800c6cba4.png)

让我们使用 Pickle 保存模型，并开始使用 **Streamlit 使其可用。** 所以，我们要用 streamlit 库用 python 写一个脚本。

![](img/ba4943747a4c437d8273c88fffd5dbc0.png)

这里，我们使用 4 个 Streamlit 函数。它们是，标题、文本输入、按钮和文本功能。

![](img/12e1b1bdc17e9d5127dee2c8643d636b.png)

最后，我们使用 Streamlit 创建了一个简单的语言检测应用程序。让我们把它部署在 Heroku

对于 Heroku 部署，您需要几个步骤。他们是，

*   **requirements.txt** 文件来安装所有运行应用程序所需的库。

![](img/c93b882ec714884a14780c1a88f61423.png)

*   Procfile 是 heroku 在您的文件夹中唯一识别的文件。

![](img/e6d00622caccc262f620b394c2d1ba5f.png)

*   **Setup.sh** 设置服务器。

![](img/249e776dc225d583c8b9d5049bdddcd6.png)

*   **平台令牌**用于向 heroku 平台认证我们的 github。转到 heroku - >创建新应用程序并创建令牌。
    转到 github 存储库- >设置- >创建两个变量的秘密存储库，令牌作为令牌，应用名称(在 Heroku 中创建)作为名称。
*   **。github/workflows/ci-CD . YAML**用于为来自 github 的每个推送请求在平台中部署您的代码

> 名称:Python 应用程序
> 
> 开:
> 推:
> 分支:
> —主
> 拉 _ 请求:
> 分支:
> —主
> 
> 乔布斯:
> 打造:
> 
> 运行:Ubuntu-最新版
> 
> 步骤:
> —用途:actions/check out @ v2
> with:
> fetch-depth:0
> —名称:设置 Python 3.7
> 用途:actions/setup-Python @ v2
> with:
> Python-version:3.7
> 名称:安装依赖项
> run:|
> Python-m pip Install—升级 pip
> pip Install flake 8 pytest
> if[-f requirements . txt]；然后 pip install-r requirements . txt；fi
> 
> — name:部署到 Heroku
> env:
> Heroku _ API _ TOKEN:$ { { secrets。TOKEN } }
> HEROKU _ APP _ NAME:$ { { secrets。NAME } }
> don tusforce:false
> if:github . ref = = ' refs/heads/main '&&job . status = = ' success '
> run:|
> git 远程添加 HEROKU[https://HEROKU:$ HEROKU _ API _ TOKEN @ git . HEROKU . com/$ HEROKU _ APP _ NAME . git](https://heroku:$HEROKU_API_TOKEN@git.heroku.com/$HEROKU_APP_NAME.git)
> git 推送 heroku HEAD:main -f

git push 命令之后，github 工作流在 heroku 中启动并执行。如果它显示成功，则您的代码部署成功。

下面是我的网址:[https://langdet-1.herokuapp.com/](https://langdet-1.herokuapp.com/)
Github 作品集:[https://github.com/Surya-Teja-Menta/Language_Detection](https://github.com/Surya-Teja-Menta/Language_Detection)

快乐编码:)

我的个人作品集:[https://suryatejamenta.co.in/](https://suryatejamenta.co.in/)
我的 LinkedIn 简介:[https://www.linkedin.com/in/surya-teja-menta/](https://www.linkedin.com/in/surya-teja-menta/)
我的 Github 简介:[https://github.com/Surya-Teja-Menta](https://github.com/Surya-Teja-Menta)