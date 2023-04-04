# 从 Python 字典中获取密钥的 6 种方法

> 原文：<https://blog.devgenius.io/6-ways-you-can-get-keys-from-a-python-dictionary-9487c57a16bf?source=collection_archive---------1----------------------->

![](img/58193545b95094029973de3f7c898049.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在[unsprash](https://unsplash.com?utm_source=medium&utm_medium=referral)上拍摄

一个伟大的开发人员在创建算法时，如果可能的话，总是会尝试在恒定时间 O(1)内插入和检索数据。在一些编程语言中，字典也被称为哈希映射，它可以帮助我们达到这样的效率。我们不会关注大哦，而是关注如何在 python 字典中检索关键字。键可以帮助我们在恒定的时间内从字典中插入和检索数据，至少几乎总是如此(除了[碰撞](https://en.wikipedia.org/wiki/Hash_collision)的情况，这种情况可能很罕见)。

对于下面的每一种情况，我们都将参考下面的字典数据结构。它存储人和他们的标记。

```
dictionary = {"Doe":90, "John": 95}
```

## **1。使用 for 循环和 key()字典方法**

`keys()`方法给了我们一个可以循环使用的变量

要访问一个值，请使用语法`value = dictionary[key]`。如果找不到钥匙，这将返回**钥匙错误**。

安全的方法是使用`get()`字典法，其原型是`get(key, default_value)`。`default_value`参数可选。如果给定，则在找不到密钥时返回。当键不存在并且没有显式给出默认值时，返回`None`作为默认值。

## **2 .使用内置列表()python 方法**

`list()`方法提供了一个简单的键列表，而不使用`keys()`字典方法。它是我最喜欢的，因为它的可读性。

## **3 .使用列表理解**

我们可以通过两种方式使用列表理解从字典中获取关键字。

首先，我们可以使用第 1 点中提到的`keys()`方法。

第二种方法是不使用看起来更短的`keys()`方法。

## 4.拆包

解包是指在一条语句中给变量赋值。

星号`(*)`帮助我们将字典的键解包到变量`my_keys`中。

## **5。`**keys() dictionary**` 使用一个 for 循环而不使用**的方法****

## **6。使用 items()字典法和 for 循环**

`items()`方法返回一个包含键及其值的 *dict_items* 对象。每个键和值都存储在一个元组对中。然后，我们可以在迭代时解包键和值，如上面的代码片段所示。

**注意**:我用 python 3.9.6 实现了所有的例子。

感谢您的阅读。编码快乐！😊😊