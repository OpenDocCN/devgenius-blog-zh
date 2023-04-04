# 向量数据库语义搜索

> 原文：<https://blog.devgenius.io/semantic-search-with-vector-database-4d80398d1e3f?source=collection_archive---------6----------------------->

最近听到很多“向量数据库”。做了一些研究，发现和“语义搜索”有关。让我们来理解这些概念，以及我们为什么需要它们。

**语义搜索**:不同的人有不同的意思。但是，这里我指的是“按查询意图/查询意义搜索”。目的是实现比简单的关键字搜索更精确的搜索结果。假设你想搜索“谁是世界头号足球运动员”，实际上你的意思是“谁是世界上最好的足球运动员”或“世界顶级足球运动员”，而不是“世界上哪个足球运动员穿着 1 号球衣”。这在任何搜索场景中都非常有用(不仅是自然语言，还有音频、图像等)。)

[](https://lucidworks.com/post/what-is-semantic-search/) [## 什么是语义搜索？- Lucidworks

### 一位同事最近对我说，“我有这样的印象，不同的人说话的时候意思是不一样的……

lucidworks.com](https://lucidworks.com/post/what-is-semantic-search/) 

**Vector** : vector 只是一串数字。对于自然语言处理，可以对单词进行标记，并将句子翻译成单词索引列表。

**嵌入**:如果你的句子很长，而且你有很多单词，向量也会很长，用一键编码，句子可能会变成稀疏向量，有维数灾。嵌入是一个相对低维的空间，您可以将高维向量转换到其中。嵌入使得在大量输入上进行机器学习变得更加容易，比如表示单词的稀疏向量。理想情况下，嵌入通过在嵌入空间中将语义相似的输入放在一起来捕获输入的一些语义。

[](https://towardsdatascience.com/neural-network-embeddings-explained-4d028e6f0526) [## 解释了神经网络嵌入

### 深度学习如何将战争与和平表现为一个向量

towardsdatascience.com](https://towardsdatascience.com/neural-network-embeddings-explained-4d028e6f0526) [](https://www.pinecone.io/learn/vector-embeddings/) [## 什么是向量嵌入？松果

### 向量嵌入是机器学习中最有趣和最有用的概念之一。他们是许多人的核心…

www.pinecone.io](https://www.pinecone.io/learn/vector-embeddings/) 

**向量相似性搜索**:现在当你搜索查询的嵌入编码表示时，你不想搜索精确匹配，而是搜索相似的嵌入向量或邻近的嵌入向量，它们将具有相似的语义。

**矢量数据库**:矢量数据库索引并存储矢量嵌入，用于快速检索和相似性搜索，具有 CRUD 操作、元数据过滤和水平缩放等功能。传统数据库的设计不便于存储、索引和搜索向量。

[](https://www.pinecone.io/learn/vector-database/) [## 什么是矢量数据库？松果

### 复杂数据正以惊人的速度增长。这些是非结构化形式的数据，包括文档、图像…

www.pinecone.io](https://www.pinecone.io/learn/vector-database/) 

松果是一个向量数据库，同样，学习一个新事物最好的事情是一个工作代码示例 repo。

[](https://github.com/pinecone-io/examples/blob/master/semantic_search/light_demo.ipynb) [## 主松果上的 examples/light _ demo . ipynb-io/examples

### 在 GitHub 上创建一个帐户，为 pinecone-io/examples 开发做出贡献。

github.com](https://github.com/pinecone-io/examples/blob/master/semantic_search/light_demo.ipynb) 

这个笔记本展示了如何用松果进行语义搜索。这是相当清楚的，所以我只描述主要步骤，你可以按照笔记本:

1.  使用 Quora 问题重复数据集，它包含成对的问题，这些问题在语法上不相同，但意思相同
2.  连接到 Pinecore 并创建一个索引来存储向量
3.  对于每个 Quora 问题，生成(id、向量、元数据)元组并插入到 Pinecore 索引中
4.  使用 Quora 问题示例在索引中搜索类似的问题

# 附录

[](https://rom1504.medium.com/semantic-search-with-embeddings-index-anything-8fb18556443c) [## 嵌入的语义搜索:索引任何东西

### 从图像、文本、图表和交互数据中构建可扩展的语义检索

rom1504.medium.com](https://rom1504.medium.com/semantic-search-with-embeddings-index-anything-8fb18556443c) [](https://towardsdatascience.com/how-to-build-a-semantic-search-engine-with-transformers-and-faiss-dcbea307a0e8) [## 如何用 Transformers 和 Faiss 构建语义搜索引擎

### 在本教程中，你将学习如何用句子转换器和 Faiss 构建一个基于向量的搜索引擎。

towardsdatascience.com](https://towardsdatascience.com/how-to-build-a-semantic-search-engine-with-transformers-and-faiss-dcbea307a0e8) [](https://github.com/rom1504/awesome-semantic-search) [## GitHub-rom 1504/awesome-Semantic-search:带嵌入的语义搜索:索引任何内容

### 在嵌入的语义搜索中，我描述了如何构建语义搜索系统(也称为神经搜索)。这些…

github.com](https://github.com/rom1504/awesome-semantic-search) [](https://towardsdatascience.com/milvus-pinecone-vespa-weaviate-vald-gsi-what-unites-these-buzz-words-and-what-makes-each-9c65a3bd0696) [## 并不是所有的矢量数据库都是相同的

### Milvus、Pinecone、Vespa、Weaviate、Vald、GSI 和 Qdrant 的详细比较

towardsdatascience.com](https://towardsdatascience.com/milvus-pinecone-vespa-weaviate-vald-gsi-what-unites-these-buzz-words-and-what-makes-each-9c65a3bd0696)