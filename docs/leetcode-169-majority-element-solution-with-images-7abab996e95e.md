# LeetCode 169。多数元素(带图像的解决方案)

> 原文：<https://blog.devgenius.io/leetcode-169-majority-element-solution-with-images-7abab996e95e?source=collection_archive---------5----------------------->

![](img/3eca10c47a553bebe9b00b6c4318244d.png)

迈克尔·泽兹奇在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 问题:→

给定大小为`n`的数组`nums`，返回*多数元素*。

多数元素是出现超过`⌊n / 2⌋`次的元素。您可以假设多数元素总是存在于数组中。

**例 1:**

```
**Input:** nums = [3,2,3]
**Output:** 3
```

**例 2:**

```
**Input:** nums = [2,2,1,1,1,2,2]
**Output:** 2
```

**约束:**

*   `n == nums.length`
*   `1 <= n <= 5 * 104`
*   `-109 <= nums[i] <= 109`

**追问:**你能在线性时间和`O(1)`空间中解决这个问题吗？

# 解决方案:→

让我们看看下图的解决方案，我们可以得到一个想法，如何解决我们可以得到的，

![](img/fb48fe07d069c269e6bc118a7c96a383.png)

让我们用代码来理解，

这里，提供了一个大小为`n`的数组`nums`，我们需要返回*多数元素*。

![](img/57bc51ffb2010ab8eab5d9ac7eea0e91.png)

这里，我们首先取两个变量，

![](img/a059ae81e48093a9a2e6fccd9222cf64.png)

为了获得*多数重复元素* **，我们需要遍历所有元素** *。*

> 对于第一个元素，

**步骤 1.1**

![](img/6a209b5593f2d4ed05748e0ee439659f.png)

**步骤 1.2**

![](img/e84b5ba59ef7e7a3877312d2b1e316a3.png)

> 对于第二个元素

**步骤 2.1**

![](img/7ebb16a894571a6628f408ba1dfd223b.png)

**步骤 2.2**

![](img/8d27ba6aff7b6f731c872383daf165be.png)

**步骤 2.3**

![](img/5fd077f0aaae48eac42173d42f42eddf.png)

> 对于第三个元素

**步骤 3.1**

![](img/1fb743973d3d45a5bcd5f0e6f9d7a572.png)

**步骤 3.2**

![](img/8ed90e6124a6665977d7132ca5b214d2.png)

**步骤 3.3**

![](img/0a09780403decb5d85a2b29d69eec779.png)

> 对于第四个元素

**步骤 4.1**

![](img/31804d2a865e522f0be7fa5d1294e850.png)

**步骤 4.2**

![](img/7c9a51a2ace250a6b98165683faeb867.png)

**步骤 4.3**

![](img/864de572720245332cba97acbde294da.png)

> 对于第五元素

**步骤 5.1**

![](img/87483aa8cff378a3c1b789cd4b746ec3.png)

**第 5.2 步**

![](img/febe451982a5eafc9eb7e36663e9948c.png)

**第 5.3 步**

![](img/bc393f5df23343aee427de75005264bb.png)

> 对于第六个元素

**步骤 6.1**

![](img/8e81991be43a42895ed51519a00762d3.png)

**步骤 6.2**

![](img/0afc421a320342fdf123c56f292bffe6.png)

> 对于第七个元素

**步骤 7.1**

![](img/73bfe8a9e12be88514f187659da78cd9.png)

**步骤 7.2**

![](img/97b7b8ab2998813738a8e7e2de69bcfc.png)

**步骤 7.3**

![](img/647db89fda36cde9922c6665e4269776.png)

最后，我们迭代了所有元素，所以结果是 **RES → 2。**

![](img/8223c94003a782f05aa951e59b783eb0.png)

最后， **2** 是我们的答案。

现在，让我们看看完整的源代码，

# 代码(Java): →

# 代码(Python): →

# 时间复杂度

这里，我们遍历整个数组，所以总的时间复杂度将是 O(n) 。

# 空间复杂性

这里，我们只使用了两个变量，所以总的空间复杂度也将是 **O(1)** 。

![](img/665384e1b5fc8a20659e7ac3edfe3b2e.png)

感谢你阅读这篇文章，❤

如果这篇文章对你有帮助，请鼓掌👏这篇文章。

请在[媒体](https://medium.com/@alexmurphyas8)上关注我，我会像上面一样发布有用的信息。

insta gram→【https://www.instagram.com/alexmurphyas8/ 

推特→[https://twitter.com/AlexMurphyas8](https://twitter.com/AlexMurphyas8)

如果我做错了什么？让我在评论中。我很想进步。