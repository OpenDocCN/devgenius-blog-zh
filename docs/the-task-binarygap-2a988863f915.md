# 最佳解决方案。第 1 课迭代

> 原文：<https://blog.devgenius.io/the-task-binarygap-2a988863f915?source=collection_archive---------0----------------------->

任务:[二进制缺口](https://app.codility.com/programmers/lessons/1-iterations/binary_gap/)

 [## 二进制编码任务-学习编码-编码能力

### 正整数 N 中的二进制间隙是任何连续零的最大序列，它在…

app.codility.com](https://app.codility.com/programmers/lessons/1-iterations/binary_gap/) 

## 解决方案:

算法是:

1.右移第 0 位，直到遇到第 1 位；

2.当我们在变量中有第 1 位时，将所有位右移并计算第 0 位；

3.每次当我们遇到当前间隙的末端时，如果它比先前的最大间隙长，则将其设置为最大间隙；

4.返回最后一个最大间隙；

该解决方案除了两个计数器之外不使用额外的存储器，即它具有恒定的存储器复杂度 O(1)。该解决方案对所有比特进行一次遍历，即该解决方案具有 O(N)的时间复杂度，其中 N 是给定变量中的比特数。

回到[目录](https://medium.com/@molchevsky/some-time-ago-i-found-on-codility-website-a-wonderful-set-of-training-tasks-which-covers-many-fdda544629ac)。