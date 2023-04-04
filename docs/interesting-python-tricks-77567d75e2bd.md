# 有趣的 Python 技巧

> 原文：<https://blog.devgenius.io/interesting-python-tricks-77567d75e2bd?source=collection_archive---------13----------------------->

## Python 上人类可读的正则表达式与数据处理加速

分享一些我最近穿越的有趣的 Python 小技巧。

# Python 上人类可读的正则表达式

有时，我们需要编写 r [正则表达式](https://www.w3schools.com/python/python_regex.asp)来处理文本。但是，由于我不经常使用，所以只有基本的规则能快速浮现在我的脑海中。所以我之前的策略是在网上搜索和试错，直到我看到了[这篇文章](https://towardsdatascience.com/pregex-write-human-readable-regular-expressions-in-python-9c87d1b1335)，感谢 [Khuyen Tran](https://medium.com/u/84a02493194a?source=post_page-----77567d75e2bd--------------------------------) 。

相当有趣。让我们赶紧试试 [Google Colab](https://colab.research.google.com/) (在线 Jupyter 笔记本服务)，“!pip install pregex”显示“找不到合适的 pregex 版本”。按照 pypi.org 的说法，“元节”

[](https://pypi.org/project/pregex) [## pregex

### PRegEx 是一个 Python 包，可以用来以一种更加人性化的方式构造正则表达式模式…

pypi.org](https://pypi.org/project/pregex) 

上面明明说需要 Python ≥3.9，默认 Google Colab Python 是 3.7。下面是你如何在 Google Colab 上安装 Python 3.10。

## 更新 Ubuntu apt

```
!sudo apt update && sudo apt upgrade -y
```

## 添加软件存储库

安装添加自定义 PPAs 所需的依赖项，将死蛇 PPA 添加到 APT 软件包管理器源列表中。

```
!sudo apt install software-properties-common -y!sudo add-apt-repository -y ppa:deadsnakes/ppa
```

## 安装 Python 3.10

```
!sudo apt install python3.10 -y
```

## 为 Python 3.10 安装 pip

```
!sudo apt -y install python3-pip
!wget [https://bootstrap.pypa.io/get-pip.py](https://bootstrap.pypa.io/get-pip.py)
!python3.10 get-pip.py
!python3.10 -m pip install --upgrade pip
```

## 检查 Python，pip 版本

```
!python3.10 -V
# Python 3.10.5
!pip --version
# pip 22.2.1 from /usr/local/lib/python3.10/dist-packages/pip (python 3.10)
```

最后，我们最初想要的

```
!pip install pregex==1.0.5
# !python3.10 -m pip install pregex
```

由于 Google Colab 是基于 Jupyter 的，默认内核仍然是 Python 3.7。所以不能用笔记本来测试，需要用 Python3.10 来测试。首先，让我们使用[" % writefile "](https://ipython.readthedocs.io/en/stable/interactive/magics.html#cellmagic-writefile)魔法命令创建一个文件。

```
%%writefile preg.py
# [https://github.com/khuyentran1401/Data-science/blob/master/productive_tools/pregex.ipynb](https://github.com/khuyentran1401/Data-science/blob/master/productive_tools/pregex.ipynb)from pregex.quantifiers import Optional, AtLeastOnce, AtLeastAtMost
from pregex.classes import AnyFrom, AnyDigit, AnyWhitespace, AnyButWhitespace, Any
from pregex.groups import CapturingGroup
from pregex.tokens import Backslash
from pregex.operators import Either
from pregex.pre import Pregeximport retext = "You can find me through GitHub [https://github.com/khuyentran1401](https://github.com/khuyentran1401)"pre = (
    "https://"
    + AtLeastOnce(AnyButWhitespace())
    + Either(".com", ".org")
    + AtLeastOnce(AnyButWhitespace())
)
print(pre)
print(pre.get_matches(text))
```

使用 Python 3.10 运行

```
!python3.10 preg.py
```

它打印生成的正则表达式，并从文本中提取期望的字符串。

```
https\:\/\/[^\s]+(?:\.com|\.org)[^\s]+
['[https://github.com/khuyentran1401'](https://github.com/khuyentran1401')]
```

# 加速 Python 数据处理

如果你使用 Python 做数据处理，可能你会经常听到这个建议:不要使用 for 循环，使用矢量化方法以获得更好的性能。事实证明这太简单了，我们不应该仅仅基于这种假设就使用矢量化方法。根据 [numpy 文档](https://numpy.org/doc/stable/reference/generated/numpy.vectorize.html)，提供矢量化功能主要是为了方便，而不是为了提高性能。该实现本质上是一个 for 循环。还有一篇有趣的文章:

[](https://towardsdatascience.com/dont-assume-numpy-vectorize-is-faster-dd7e455dba2) [## 不要认为 NumPy.vectorize 更快

### 用不同的向量化函数解决生日悖论

towardsdatascience.com](https://towardsdatascience.com/dont-assume-numpy-vectorize-is-faster-dd7e455dba2) 

然而，当涉及到数据时，就算法而言，你应该考虑基于集合的 [1](https://www.codeproject.com/Articles/34142/Understanding-Set-based-and-Procedural-approaches) 、 [2](https://www.sqlshack.com/t-sql-asset-set-based-programming-approach/) 或矢量化方法 [1](https://medium.com/productive-data-science/why-you-should-forget-for-loop-for-data-science-code-and-embrace-vectorization-696632622d5f) 、 [2](https://resources.experfy.com/bigdata-cloud/why-you-should-forget-loops-and-embrace-vectorization-for-data-science/) 、 [3](https://python.plainenglish.io/pandas-how-you-can-speed-up-50x-using-vectorized-operations-d6e829317f30) ，而不是逐行、程序化或 for 循环的方式处理数据。例如，列表理解是一种比 For 循环更好的技术(性能、可读性)。

[](https://switowski.com/blog/for-loop-vs-list-comprehension) [## 对于循环和列表理解

### Python 中许多简单的“for 循环”可以用列表理解来代替。你经常可以听到列表理解…

switowski.com](https://switowski.com/blog/for-loop-vs-list-comprehension) 

对于不同的框架，也有其他的技术，例如 [numba](https://numba.pydata.org)

## numpy

下面的文章讨论了使用 numba 矢量化装饰器来创建高性能的 ufuncs

[](https://towardsdatascience.com/numpy-ufuncs-the-magic-behind-vectorized-functions-8cc3ba56aa2c) [## NumPy ufuncs——矢量化函数背后的魔力

### 了解 NumPy 通用函数(ufuncs)以及如何创建它们。编写自己的矢量化函数。

towardsdatascience.com](https://towardsdatascience.com/numpy-ufuncs-the-magic-behind-vectorized-functions-8cc3ba56aa2c) 

## 熊猫

你也可以使用“numba”来优化熊猫的性能。

[](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html#numba-jit-compilation) [## 增强性能- pandas 1.4.3 文档

### 在教程的这一部分，我们将研究如何加速熊猫数据帧上的某些函数操作…

pandas.pydata.org](https://pandas.pydata.org/pandas-docs/stable/user_guide/enhancingperf.html#numba-jit-compilation) [](https://coderzcolumn.com/tutorials/python/guide-to-speed-up-code-involving-pandas-dataframe-using-numba) [## 如何用 Numba 加速涉及熊猫 DataFrame 的代码？作者桑尼·索兰基

### Numba 是一个非常常用的库，用来加速 Python 代码的计算。它让我们通过…来加速我们的代码

coderzcolumn.com](https://coderzcolumn.com/tutorials/python/guide-to-speed-up-code-involving-pandas-dataframe-using-numba) 

您可以在 select pandas 方法中指定 engine="numba "关键字。就性能而言，第一次使用 Numba 引擎运行函数会很慢，因为 Numba 会有一些函数编译开销。但是，JIT 编译的函数是缓存的，后续调用会很快。

[](https://medium.com/analytics-vidhya/understanding-vectorization-in-numpy-and-pandas-188b6ebc5398) [## 理解 NumPy 和 Pandas 中的矢量化

### 仅仅几天前，我的导师停下来说明了一点。“做你自己一个…

medium.com](https://medium.com/analytics-vidhya/understanding-vectorization-in-numpy-and-pandas-188b6ebc5398) [](https://python.plainenglish.io/pandas-how-you-can-speed-up-50x-using-vectorized-operations-d6e829317f30) [## 如何使用向量化操作加速 Pandas 数据操作

### 使用矢量化运算使 pandas 数据运算速度提高 50 倍的指南。

python .平原英语. io](https://python.plainenglish.io/pandas-how-you-can-speed-up-50x-using-vectorized-operations-d6e829317f30) 

*矢量化方法比系列应用和系列映射更快。当数据变大时，我们总是希望坚持矢量化的操作，序列或数组。我们应该尽可能避免争吵。当没有可用于您的用例的矢量化操作时，请坚持使用地图操作。这是复习各种文章后的指导方针。*

[](https://pythonspeed.com/articles/pandas-vectorization/) [## 熊猫向量化:更快的代码，更慢的代码，臃肿的内存

### 当你用 Pandas 处理数据时，所谓的“矢量化”操作可以显著提高代码速度。或者在…

pythonspeed.com](https://pythonspeed.com/articles/pandas-vectorization/) 

上面文章中的代码显示了字符串矢量化比非矢量化方法慢(因为它不是完全矢量化，而是在某些时候使用了非矢量化方法)

```
import pandas as pd
import time
df = pd.DataFrame({
    'sentences': ['9910 Surrey Avenue','92 N. Bishop Avenue','9910 Golden Star Avenue', '102 Dunbar St.', '17 West Livingston Court']
})# ... Vectorized operation:
start = time.time()
df["sentence_length"] = df["sentences"].str.split().apply(
    len)
end = time.time()
print(end-start)# ... Non-vectorized operation:
def sentence_length(s):
    return len(s.split())start = time.time()
df["sentence_length2"] = df["sentences"].apply(
    sentence_length
)
end = time.time()
print(end-start)
```

结果

```
# vectorized: 0.006185054779052734
# non-vectorized: 0.0014357566833496094
```

用于计算数据帧中句子长度的正确矢量化代码如下。

```
start = time.time()
df["sentences"].str.split().str.len()
end = time.time()
print(end-start)
# 0.0017879009246826172
```

[](https://medium.com/pythoneers/enhance-python-performance-with-cython-numba-and-eval-fdd4b34c777c) [## 使用 Cython、Numba 和 Eval()增强 Python 性能

### 数据科学和软件开发的简单概念

medium.com](https://medium.com/pythoneers/enhance-python-performance-with-cython-numba-and-eval-fdd4b34c777c) 

展示 numba 如何加快熊猫的功能。

 [## 矢量化字符串操作

### Python 的一个优势是它相对容易处理和操纵字符串数据。熊猫以此为基础…

jakevdp.github.io](https://jakevdp.github.io/PythonDataScienceHandbook/03.10-working-with-strings.html) 

列出熊猫支持的一些矢量化字符串操作。

## PySpark

它有 [pandas_udf](https://databricks.com/blog/2017/10/30/introducing-vectorized-udfs-for-pyspark.html) (也称为矢量化 udf)，使用 [Apache Arrow](https://arrow.apache.org/docs/2.0/python/cuda.html#numba-integration) ，在 Python 中定义低开销、高性能的 UDF。

[](https://spark.apache.org/docs/3.0.0/sql-pyspark-pandas-with-arrow.html) [## 配有阿帕奇箭的熊猫 PySpark 使用指南

### Apache Arrow 是内存中的列数据格式，在 Spark 中用于在 JVM 和…

spark.apache.org](https://spark.apache.org/docs/3.0.0/sql-pyspark-pandas-with-arrow.html) 

但是，numba +熊猫 UDF 能进一步提速吗？

[](https://towardsdatascience.com/enhancing-optimized-pyspark-queries-1d2e9685d882) [## 增强优化的 PySpark 查询

### 梦想成真的故事

towardsdatascience.com](https://towardsdatascience.com/enhancing-optimized-pyspark-queries-1d2e9685d882) 

使用以下内容比较熊猫 UDF 和农巴 UDF 的表现:

1.  熊猫 UDF
2.  熊猫 UDF 调用“数字”功能

```
import pandas as pd
from numba import njitfrom datetime import datetime, date
from pyspark.sql import SparkSession
from pyspark.sql.functions import pandas_udfspark = SparkSession.builder.getOrCreate()[@pandas_udf](http://twitter.com/pandas_udf)('long')
def pandas_plus_one(series: pd.Series) -> pd.Series:
    return series + 1[@njit](http://twitter.com/njit)
def numba_plus_one(series):
    return series + 1[@pandas_udf](http://twitter.com/pandas_udf)('long')
def numba_udf_plus_one(series: pd.Series) -> pd.Series:
    return pd.Series(numba_plus_one(series.to_numpy()))df=spark.range(10000)start = time.time()
df.select(pandas_plus_one(df.id))
end = time.time()
print("time for pandas_plus_one(: {:0.3e} sec".format(end - start))start = time.time()
df.select(numba_udf_plus_one(df.id))
end = time.time()
print("time for numba_udf_plus_one: {:0.3e} sec".format(end - start))
```

结果:Numba 没有进一步加速星火熊猫 UDF。

```
time for pandas_plus_one(: 8.108e-03 sec
time for numba_udf_plus_one: 8.293e-03 sec
```

# 奖金

[](https://ismaelaraujo.medium.com/pytrends-a-python-library-that-you-should-know-24764b384bc2) [## PyTrends:您应该知道的 Python 库

### PyTrends 将使你的下一个项目更上一层楼。原因如下。

ismaelaraujo.medium.com](https://ismaelaraujo.medium.com/pytrends-a-python-library-that-you-should-know-24764b384bc2) 

## 附录

[](https://www.linuxcapable.com/how-to-install-python-3-10-on-ubuntu-22-04-lts/#Install_Python_PIP_with_310) [## 如何在 Ubuntu 22.04 LTS 版上安装 Python 3.10 支持 Linux

### Python 是最流行的高级语言之一，专注于高级和面向对象的应用程序，来自…

www.linuxcapable.com](https://www.linuxcapable.com/how-to-install-python-3-10-on-ubuntu-22-04-lts/#Install_Python_PIP_with_310) [](https://towardsdatascience.com/dont-use-apply-in-python-there-are-better-alternatives-dc6364968f44) [## 不要用 Python 中的 Apply，有更好的替代品！

### 应用功能的替代方案，可将性能提高 700 倍

towardsdatascience.com](https://towardsdatascience.com/dont-use-apply-in-python-there-are-better-alternatives-dc6364968f44) [](https://towardsdatascience.com/how-to-skyrocket-your-python-speed-with-numba-89fd16362e47) [## 如何用 Numba 提升你的 Python 速度🚀

### 学习如何使用 Numba Decorators 让你的代码更快

towardsdatascience.com](https://towardsdatascience.com/how-to-skyrocket-your-python-speed-with-numba-89fd16362e47) [](https://thedatafreak.medium.com/spark-optimization-reducing-shuffle-9cb2c109e977) [## 火花优化:减少洗牌

### "洗牌是大自然唯一不能逆转的事情."——阿瑟·爱丁顿

thedatafreak.medium.com](https://thedatafreak.medium.com/spark-optimization-reducing-shuffle-9cb2c109e977) [](https://medium.com/geekculture/right-way-to-test-mock-and-patch-in-python-b02138fc5040) [## 在 Python 中测试、模拟和修补的正确方法

### 取代强迫你首先考虑测试的 TDD(测试驱动开发),你可以实践 TPD(测试…

medium.com](https://medium.com/geekculture/right-way-to-test-mock-and-patch-in-python-b02138fc5040) [](https://towardsdatascience.com/12-python-decorators-to-take-your-code-to-the-next-level-a910a1ab3e99) [## 12 个 Python 装饰器，让你的代码更上一层楼

### 用更少的代码做更多的事情，而不影响质量

towardsdatascience.com](https://towardsdatascience.com/12-python-decorators-to-take-your-code-to-the-next-level-a910a1ab3e99) [](https://findwork.dev/blog/advanced-usage-python-requests-timeouts-retries-hooks) [## Python 请求的高级用法——超时、重试、挂钩

### Python HTTP 库请求可能是我在所有编程语言中最喜欢的 HTTP 工具。很简单…

findwork.dev](https://findwork.dev/blog/advanced-usage-python-requests-timeouts-retries-hooks)