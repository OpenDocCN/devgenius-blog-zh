# NumPy 如何节省我们的时间？编程变得简单

> 原文：<https://blog.devgenius.io/how-numpy-saves-our-time-programming-made-easy-1ea211e53094?source=collection_archive---------10----------------------->

python 中的 NumPy 是什么？？很困惑也很愤怒想知道 python 中的 NumPy 到底是做什么的…让我们一起探索一下…

从我们用于数学运算的数字到我们寻找的天气预报，都利用了 **NumPy** 。很有趣，对吧？

![](img/81318a679a5276f9e2dc8f55c320fb10.png)

图片来自 https://realpython.com/numpy-tutorial/

# NumPy

Numberical python，在数学运算等许多方面作为标准包处理。

如果有 **__name__，Python 理解为 package。py**

![](img/685c5e0663bd409f73dd3de40cc9fd1c.png)

# packages 包含 Python 文件和一个名为`__init__` .py 的文件的目录。

**实时示例** —将在播放列表中存储歌曲列表，播放列表在这里作为包播放。

# NumPy 的✦应用

*   *数组上的数学运算*
*   *NumPy 内置了线性代数和随机数生成的函数。*
*   *分析形状尺寸。*

# NumPy 的✦安装:

![](img/0ee5ce385cac37356efac11b1ca76fa4.png)

***安装 NumPy 之前需要检查的两个方法，是 Python 和 PIP，***

![](img/10f3ed57fdc5c4be96e7b5608c4bfd61.png)

检查是否安装了 pip

NumPy 的安装如下

![](img/9af0ef5aadd4208eb9021e06d56adf5c.png)

数字安装

NUMPY 中唯一支持的数据结构是数组

# 如何让 NumPy 投入使用？？

**导入 NumPy :** 使用`import`关键字将 NumPy 导入应用程序:

![](img/49bbc4d6466b4d2cae2ee7760759d7a0.png)

**NumPy 中为什么一定要别名？这样做重要吗？**

**NumPy as np :**

然而，对于对 numpy 函数的大量调用，编写 NumPy 可能会变得很乏味。x 一遍又一遍。

Alias 使我们的任务在任何需要的地方都容易使用 NumPy。只是用 np 赋值 **Numpy**

![](img/176720d91920302444c794d1f392b7dd.png)

# 数组-

**相似数据类型的项目集合**。

![](img/cc824ea91370b4255e5a412096b558a4.png)

数组类似于列表

**NumPy ndarray 对象:**

***作为唯一支持 NumPy 的数据结构是数组。要访问数组，我们需要用 ndarray 创建对象。***

ndarray()-表示需要创建数组的维数。

![](img/26de3dd087da1698772fdee64918f30c.png)

恩达拉雷

![](img/0f207267c6e447379f0aa3d77cd61e61.png)

获取的数组并打印它

![](img/dc73767451662b43a4b0c569c37fcf37.png)

当数组包含单个浮点元素时，它将整个数组转换为浮点元素

![](img/e187c7bd5cb7ba75b9e873ff90b82be4.png)

由于 string 具有最高优先级，它将整个数组转换为 String。

**其他方式转换成任何其他形式-**

*   从一种形式转换成另一种形式

![](img/2957f523d682276973c2527672ab6826.png)

铅字铸造

*   **类型()-** 弹出数组类型

![](img/07c6cd3a75cb5ad6598de068d572d808.png)

*   **dtype()-** 给出特定数组的数据类型

![](img/9c9447303f3f8098ee197df48afca8c3.png)

*   **ndim-** 获取数组的维数

![](img/ad8e8f8828728867add4c50b068eee99.png)![](img/afc1c8930b8cd88ad924a2d5b2ed6c92.png)

*   **shape-** 获取数组的当前形状，但也可用于通过给数组分配数组维数来就地改变数组的形状。

形状就是数组中的行和列 **#R，C**

![](img/5ef929434b5ead403eb2a350c9314a64.png)

这里 4 表示列表中元素的编号。

*   **重塑()-** 改变形态

![](img/7344756981f547f7a07f763706088fd7.png)

整形([R，C])

*   **size-** 统计元素个数。

![](img/3070415179ec4a2dd9fcd1dd588ba814.png)![](img/82a9308c2667a8301046b500bed58adb.png)

*   **可变-** 具有变化的能力。

![](img/3eca4780873b011dc60146395802a0cd.png)

位置 1 为 4，被更改为 20

# 规模

数字尺寸中的称为**轴**。

用简单的方括号( **[** )表示数量

举个例子——一个( **[** 1，2，3，4 **]** 的数组，这里的括号前后只有一个方括号。所以，它表示 **1D 阵。**

**(**[[**1，2，3，4 **]]** )的数组这里的括号前后有两个方括号。所以，它表示 **2D 阵。****

**我们下面看到的例子属于一维数组…**

**让我们来看看更多关于**二维(2D)阵列****

**![](img/5ec0adc5944756840979cb74fb6057b8.png)**

**2D 阵列**

**![](img/c7bf14144b5d738691e77b13e557664d.png)**

**阵列的尺寸和形状**

*   ****len() —** 函数返回第一个轴的长度。**

**放在[]内的元素将被视为一个元素。**

**![](img/c864167aab5b9135417b5b678229ccec.png)**

**在这种情况下，方括号中包含了三个集合**

*   ****in-** 用于测试数组中是否存在值**

**![](img/c25033dbef239a6062c5e284fe6b2468.png)**

**它用真或假来表示检查**

**![](img/832eafa5b99b2f21c514d88bd3742456.png)**

**类似地，如果元素不是列表，它弹出 False**

****如果数组中有不匹配会发生什么？****

**![](img/8532f7263488c27bebb60df6e41e6230.png)**

**将执行，但带有警告**

**上面的例子执行时，把它当作一个列表，而不把它当作 2D 数组。**

**![](img/5964a871431446e1742faab121786506.png)**

**因此，输出将在 1D 数组中**

# **数组切片**

**![](img/4d0da71f4312784bfa474c9af8aefc0c.png)**

**切片变得简单**

**![](img/f6a044870fcb81da949cb662650b74d4.png)**

**将方括号内的第一个集合视为第 0 个位置**

*   ****如果我们需要访问范围或特定元素怎么办？****

**![](img/4c3b57bd46144574305ddfdcf15bde18.png)**

**第 0 个位置第 0 个元素**

**![](img/acccaea074a9060aae77160881dbf254.png)**

**第 0 个位置第 1 个元素**

*   ****获取特定元素****

**![](img/578efb76e42e6414fa81687fab3cb9df.png)**

**从数组中检索 8，9**

**![](img/d20f54fbd6fc0d6a4fc79435e53a63eb.png)****![](img/ad2d25442c619a405daf81f4b5536c8b.png)**

**0:2 表示行范围，1，4 位于第 0 个位置**

**作为切片流[R，C]**

**![](img/1f30d361fecb18189cac9efdb0d46437.png)**

**选择范围 2，5，6**

*   ****享受更多功能带来的乐趣****

**![](img/135adecbdd8a1d76f80f5d94f69b4c7f.png)**

*   ****当我们需要特定行的平均值时？****

**![](img/4e329cd42a8480f652a5e5fd2b1a7030.png)**

**找出所有列的平均值，并指定轴**

**![](img/990daa3ea3caef68b953fcdf73d5247b.png)**

**在垂直轴上寻找平均值**

# **创建数组的另一种方式**

**![](img/09ae071abde8e39bbf03eb707458e734.png)****![](img/f80918d9d318d3aedaf356e785ef99cd.png)**

**创建数组的另一种方式**

*   ****什么时候我们需要从阵列开始…？？****

**![](img/2ed0c855ceb5da0ee795c529fea23eb2.png)**

**从 1 到 10 的元素**

*   ****reshape(R，C)-** 排列任意维度**

**![](img/310e8f3fc507c276551e57d5cf418e79.png)****![](img/ef950244b52788594ab5af11c6dd9650.png)**

**整形(R，C)**

**![](img/234e216d4b4d2a57db39ee200638008a.png)**

**当试图用非原始设置进行整形时**

# **NUMPY 数组中的串联**

*   **沿现有轴连接一系列数组。**

**![](img/60cd116e5ae177f2056dc501cc984d61.png)****![](img/715f9b1b57457b81b5b1ef42cc9a7f40.png)**

**串联两个不同的数组**

*   **纵轴串联**

**![](img/3a3f17008a6315fc33a775f9e64ec405.png)****![](img/e709e93ed2c132bcd615e794affe4869.png)**

**纵轴串联**

*   **在水平轴上串联**

**![](img/dd4c20306f5e1a58ddf4cc360e2ef522.png)****![](img/c00618957416cff68286f5dd73adf7f1.png)**

**在水平轴上串联**

# **为什么要把数组转换成字符串？？**

*   **何时存储大数组。**
*   **当我们尝试使用字符串函数时。**
*   ****flatten()-** 从更高维度还原为 1D 数组。**
*   **高效地访问数据。**
*   **中性网络。**

****示例** -高维到低维而不丢失数据**

**![](img/e93b997d5824600e3d17c45aaeb10d21.png)****![](img/3b761448e36cdaa2e9a6cc45c119fe08.png)**

**展平()**

****newaxis-** 将 1D 转换为 2D 数组，标注提到的值。**

**![](img/17d9d4838b34b79e7b8c64e945970102.png)****![](img/655b5e635f8be018f4194346ee71fb92.png)**

**改为(3，1)**

**![](img/547d5b7edcd9d862a9e0b446f80c016a.png)**

**已更改为(1，3)**

*   ****范围-类似于范围****

**![](img/de81817813f710ae523cb1046e54bb91.png)****![](img/0cfa2021225f576777fdbd11673012d8.png)**

**我们可以排列阵列**

> *****【开始、停止、步进】*****

**![](img/8bac84c8ca2616aca46c85ad3a81676a.png)**

*   ****单位矩阵-** 确认一个矩阵的逆矩阵，并且始终是一个方阵。**

**![](img/ea12615240b5cf333477a7f0fac64dd1.png)****![](img/0502603b754cf27b63ba17023fabeb9d.png)**

*   ****ones-** 返回具有指定尺寸的一个**

**![](img/942a011125d2697fe1cf493960e3c6b7.png)****![](img/a80c64318f8d822dfcddba2093c15289.png)**

**带整数和浮点**

*   ****零-** 用这些值创建指定维数的新数组。**

**![](img/d8239ccf40218bfaaaa44237f7f7cf17.png)****![](img/e410d9c202e1223cb274b432b5e38bac.png)**

*   ****0 _ like 和 1 _ like-**0 _ like 和 1 _ like 函数创建一个具有相同维数的新数组。**

**![](img/2d044a880659e77afbd316bd9b34bf73.png)****![](img/cd48b60a397e87f35b461058df00f518.png)****![](img/474dd6a7df40035940c3491ab79cb481.png)**

**像某人一样**

*   ****眼睛-** 用对角线上的 1 创建二维数组**

**![](img/b69971ecd58d52d6b02e049d5e43e0f3.png)****![](img/7f3f19944c6347e14a3d3af3cd5cba80.png)**

**k=0 表示第 0 个位置**

**![](img/f63ebcdd566756f809a72cdb30e2881a.png)**

**具有相应的位置**

*   ****广播对象** →数组维数不匹配，会重复较小的数组**

**![](img/54bc71d5e256f7f42890b7ab81f304ac.png)****![](img/46538893a13386cda0d4d5fd18eb4d42.png)**

# **阵列数学**

**![](img/329e508ca99a0713ca6eb1659b88c726.png)****![](img/f4db9a510af36d3f3f65be669c88cc0d.png)**

**如果数组大小不匹配，将引发错误:**

**![](img/3e94b87bbf0a460b3e54b34f7b9f0f6f.png)****![](img/4a47d7ce343f067d86bb40cc832a1393.png)**

*   ****sqrt()-** 平方数**

**![](img/8865ad1db458fa3c28c324b56a497678.png)****![](img/6736bf0e283c0e1e17a75d5efca2afef.png)**

*   ****下限-** 给出较低的整数:**

**![](img/1768ae81271a21e9ea6bd4dbfc6ac470.png)**

*   ****ceil-** 给出上整数:**

**![](img/20faf377b273fde1adf011788e64ed6f.png)**

*   ****rint-** 给出最接近的整数:**

**![](img/306e27bbc14d242e6390e865b7995286.png)**

*   **NumPy 模块中还包括两个重要的数学常数:**

**![](img/d6f7963eea8e8cebf330281578afed65.png)**

# **数组迭代**

*   **以类似于列表的方式迭代数组是可能的**

**![](img/8fdebb84e6deb5b0846705efd10adc8b.png)**

*   **对于多维数组**

**![](img/f4a4718a9cc4f7001cb7a98f3101e5b7.png)**

*   **多重赋值也可以用于数组迭代:**

**![](img/f1ad24cead374ef21e1d3cf9c260cc18.png)**

*   ****拆包-** 估计只需要哪个。**

**![](img/9974e125e6ff0ca90b255c5fe0b50bf2.png)**

# **基本数组操作**

*   **数组的和与积**

**![](img/42fb2e80dc043a74f34244ffd044f4b1.png)**

**加法和乘法数组**

*   **最小和最大元素的位置— **argmin()和 argmax()****

**![](img/071df25384323fd031e9309f9e5bc1b5.png)****![](img/ddace66ce723369332e0d1aa41aea1fd.png)****![](img/5e5b01a4898892843777c7758482acd0.png)**

*   ****clip(start，end)** -给定范围如果数组中的元素小于开始，它将用最小值替换。**

**![](img/8492f845a5b6635a54b391927fef8f8d.png)****![](img/1edd370f157b0d3e7e70c6bf12f41c02.png)**

**这里 6 大于范围，所以在范围内替换**

*   ****unique()** -获取唯一值，不重复值。**

**![](img/d8d8e6d564da1e45abafaa785fdaaa04.png)****![](img/0e8ff320149f727d639dfbaed15d92e4.png)**

*   ****diagonal()** -对于二维数组，可以提取对角线:**

**![](img/279c6f7448d7c64aff6f731bfbd6da2e.png)****![](img/575552bf138dace0560d77502542ac6c.png)**

*   ****sort()-** 默认按升序排列元素。**

**![](img/6d1de95a06213491f2087f4915bfbd27.png)****![](img/32e392f1082a52572212b4edbf342856.png)**

# **比较操作**

**在大小相等的数组中按元素比较成员。返回值是布尔值 True / False 的数组:**

**![](img/32ef3723bae277acd119bf9f866a1c62.png)****![](img/30279677189e3a4009d74bc60ae2f7fa.png)**

*   **使用广播可以将数组与单个值进行比较:**

**![](img/b9b2aa2d035a5e68278c36a3a96ebd7f.png)**

*   **any 和 all 运算符可用于确定布尔数组的任何或所有元素是否为真:**

**![](img/d34589e336af4dee4750ccd05a847114.png)**

*   **where 函数-类似于 if else 语句..如果条件为真，将执行 true，否则终止或执行 else 部分。**

> *****where(boolarray，truearray，false array)*****

**![](img/991e65613a0e769a6d8ec1b96c57fe40.png)****![](img/c65af517685a3a10c1911d5ec6771a65.png)**

**当数字/ 0 是无穷大时，变暖就发生了**

**![](img/d32c4d65de9d18a278de85b10ac1df02.png)**

*   ****非零-** 检查非零值并获取其索引。**

**![](img/da6da68ffac947608f62b6834c06fa33.png)****![](img/7bfeacecad7f4bb87e53522d9dd15555.png)**

*   **检查是否有任何不是数字的元素，如果有，抛出 True。**

**![](img/a6c4ab0ac72ba3eac16684e70f44e8e3.png)**

*   ****infinite()** -检查是否有任何元素是有限的，如果有，则抛出 True。**

**![](img/9b4610b47b705275fe80f17046f82721.png)**

# **数组选择和操作**

**但是，与列表不同，数组还允许使用其他数组进行选择。**

**![](img/99ddb91366817b3f40dc33ce590706d4.png)**

**将检查条件是真还是假**

**![](img/09cbefd45ea677c28e45b90851939353.png)**

**将显示大于或等于条件的数字。**

*   **除了布尔选择之外，还可以使用整数数组进行选择。这里，整数数组包含要从数组中取出的元素的索引。**

**![](img/51a2675b002cf0e4432561620d369089.png)**

*   **对于多维数组，我们必须向选择括号发送多个一维整数数组，每个轴一个。然后，按顺序遍历这些选择数组中的每一个:第一个元素的第一个轴索引取自第一个选择数组的第一个成员，第二个索引取自第二个选择数组的第一个成员，依此类推。**

**![](img/0bc1543e4df2d9d563f7cb4957b5421d.png)**

*   ****take-** 一个特殊的函数 **take** 也可用于执行整数数组的选择。这与括号选择的工作方式相同:**

**![](img/8bc44d3dfe4c113bba556595f2783eb2.png)**

*   ****put-** 与 take 函数相反的是 put 函数，它从一个源数组中取值，并将它们放在调用 put 的数组中指定的索引处。**

**![](img/8a3a58a0488e03ca9368a92460e6e60f.png)****![](img/091ee637ccb4102417bdaf184ac046c6.png)**

**给你们带来新的乐趣…有问题吗？请在评论区给我留言，我会尽快回复你:)**

****更多博客即将推出！！！****

**让我们在 [Linkedin](https://www.linkedin.com/in/shifana-tasneem-954718172/) 上进行更多的讨论**

**干杯**

**希法娜·塔斯尼姆:**

**资源:**

## **资源:**

**[](https://numpy.org/doc/stable/) [## NumPy 文档-NumPy 1.23 版手册

### 版本:1.23 下载文档:PDF 版本|文档的历史版本有用链接:安装|…

numpy.org](https://numpy.org/doc/stable/) 

https://www.w3schools.com 的数字

[](https://towardsdatascience.com/how-to-create-numpy-arrays-from-scratch-3e0341f9ffea) [## 如何从头开始创建 NumPy 数组？

### 本教程是关于理解 NumPy python 包和从头开始创建 NumPy 数组的。

towardsdatascience.com](https://towardsdatascience.com/how-to-create-numpy-arrays-from-scratch-3e0341f9ffea) 

**图像**

[](https://www.w3resource.com/numpy/manipulation/reshape.php) [## NumPy:shape()函数- w3resource

### 函数的作用是:在不改变数组数据的情况下，给数组一个新的形状。语法:numpy . shape(a…

www.w3resource.com](https://www.w3resource.com/numpy/manipulation/reshape.php) [](https://www.java67.com/2015/05/4-ways-to-concatenate-strings-in-java.html) [## Java 中连接字符串的 4 种方法[示例和性能]

### 当我们想到 Java 中的字符串连接时，我们想到的是+运算符，这是最简单的方法之一…

www.java67.com](https://www.java67.com/2015/05/4-ways-to-concatenate-strings-in-java.html) [](https://www.javatpoint.com/numpy-concatenate) [## Python-Java point 中的 numpy.concatenate()

### concatenate()函数是 NumPy 包中的一个函数。这个函数本质上结合了 NumPy 数组…

www.javatpoint.com](https://www.javatpoint.com/numpy-concatenate) [](https://www.tutorialsandyou.com/python/numpy-concatenate-in-python-69.html) [## Python 中的 numpy . concatenate()| Python 中的 np.concatenate()

### 报告此 ad Numpy concatenate()不是数据库联接。它基本上是垂直堆叠 Numpy 数组或…

www.tutorialsandyou.com](https://www.tutorialsandyou.com/python/numpy-concatenate-in-python-69.html) [](https://www.javatpoint.com/numpy-sort) [## Python-Java point 中的 numpy.sort

### Python 中的 numpy.sort，包含 numpy 简介、环境设置、n Array、数据类型、数组创建、属性…

www.javatpoint.com](https://www.javatpoint.com/numpy-sort) [](https://www.w3resource.com/numpy/array-creation/zeros_like.php) [## NumPy: zeros_like()函数- w3resource

### zeros_like()函数用于获取一个由零组成的数组，其形状和类型与给定的数组相同。语法…

www.w3resource.com](https://www.w3resource.com/numpy/array-creation/zeros_like.php) [](https://towardsdatascience.com/5-smart-python-numpy-functions-dfd1072d2cb4) [## 5 个智能 Python NumPy 函数

### 简洁编程的优雅 NumPy 函数

towardsdatascience.com](https://towardsdatascience.com/5-smart-python-numpy-functions-dfd1072d2cb4) [](https://www.javatpoint.com/numpy-matrix-multiplication) [## NumPy 矩阵乘法-Java 点

### 矩阵乘法是以两个矩阵为输入，将两个矩阵相乘，生成一个矩阵的运算

www.javatpoint.com](https://www.javatpoint.com/numpy-matrix-multiplication) 

https://www . splash learn . com/math-vocabulary/geometry/dimensions

geekstocode.com**