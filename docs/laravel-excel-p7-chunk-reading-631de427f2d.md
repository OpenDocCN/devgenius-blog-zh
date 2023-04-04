# Laravel-Excel — P7:组块读取

> 原文：<https://blog.devgenius.io/laravel-excel-p7-chunk-reading-631de427f2d?source=collection_archive---------9----------------------->

![](img/e2c4d144d358748aa24d3e9231342372.png)

在上一篇文章中，我们讨论了批量插入，一次插入 100 行。块读取处理将一定量的数据拉入内存。如果不读取数据块，我们会将整张表拉入内存。如果 excel 表中有大量数据，这可能会很大。

[](/laravel-excel-p6-batch-imports-261c80e46448) [## laravel-Excel-P6:批量导入

### 到目前为止，当我们将 excel 表导入数据库时，我们所做的是为每一行创建一个导入:一个实际的…

blog.devgenius.io](/laravel-excel-p6-batch-imports-261c80e46448) 

# 如何使用组块阅读？

我希望你能看到一种趋势的出现:使用 Laravel-Excel，大多数事情都非常简单。我们所要做的就是实现`WithChunkReading`关注点并使用`chunkSize`方法。

# 向块读取添加批量插入

这是组块阅读真正开始发光的地方。您可以从文件中读取有限数量的数据，然后将其作为块插入。对于这个例子，我们将一次读取 20 行并批量插入 20 行。我们的文件有 100 行。

让我们在`UserController`类中创建我们的方法并添加我们的路线。

# 你在哪里？

如果你需要知道你在哪一行，你可以`use`这个`RemembersRowNumber`特征，然后调用`$this->getRowNumber()`来获得当前的行号。这适用于整篇或整段阅读的纸张。

运行这段代码，并删除所有`UserController`方法中对`/`的重定向，会产生以下结果:

```
string(6) "Row: 2" 
string(6) "Row: 3" 
string(6) "Row: 4" 
string(6) "Row: 5"
...
string(6) "Row: 101"
```

我们还可以看到带有`RemembersChunkOffset`特征的块偏移量是多少。一旦使用，就调用`$this->getChunkOffset()`方法。

每个`var_dump`的结果如下所示。

```
string(18) "Row: 2 -- Chunk: 2" 
string(18) "Row: 3 -- Chunk: 2" 
string(18) "Row: 4 -- Chunk: 2" 
string(18) "Row: 5 -- Chunk: 2" 
string(18) "Row: 6 -- Chunk: 2"
...
string(20) "Row: 99 -- Chunk: 82" 
string(21) "Row: 100 -- Chunk: 82" 
string(21) "Row: 101 -- Chunk: 82"
```

我们离让导入变得非常棒只有一步之遥，那就是排队导入。有了块读取，排队导入将会很棒。我们接下来会解决这些问题。

![](img/4df5c00d0bca6594f9bf1b397e2a6ae7.png)

Dino Cajic 目前是 [Absolute Biotech](https://www.absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。