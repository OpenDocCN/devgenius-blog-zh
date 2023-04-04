# 链表:如何实现

> 原文：<https://blog.devgenius.io/linked-list-how-to-approach-d13fa96c8888?source=collection_archive---------6----------------------->

![](img/fcfbc64656c30f217216b4a287368b43.png)

什么是真正的**链表**？

首先，它是一种非常常见和广泛使用的数据结构。根据[维基百科](https://en.wikipedia.org/wiki/Linked_list)，链表是数据元素的线性集合，其顺序不是由它们在内存中的物理位置给出的。相反，每个元素(或节点)指向下一个，它与其他节点一起代表一个序列。

这个听起来可怕的“顺序不是由它们在内存中的物理位置给出的”是一个非常重要的特性:这意味着，我们可以从一开始就添加和删除元素，成本更低(想想时间和空间的复杂性！)要比我们通过实现数组的 shift()和 unshift()方法来实现更好。同样，有了链表，我们就不会耗尽空间。

下面是一张简单链表的图片:

![](img/49771c0d63f2e5f0dda6f8f88edd284c.png)

geeksforgeeks.org

正如我们所看到的，每个节点都有一些数据和一个将它连接到另一个节点的指针(或链接)。我们称第一个节点为头。列表中的最后一个节点指向 null。如果一个列表是空的，那么头就是空的。

在 JavaScript 中有很多方法可以构建一个链表。我将向您展示我发现的最简单、最整洁的方法。

**第一步:**创建单个节点。为此，我们初始化节点类，并通过使用构造函数定义一个具有两个属性的对象，即数据和一个名为“next”的指针。至于现在，next 等于 null。

```
class Node {
    constructor(data) {
        this.data = data
        this.next = null                
    }
}
```

**第二步:**使用类构造函数创建一个链表。如果头节点没有被传递，那么头节点被赋值为 null。

```
class LinkedList {
    constructor(head = null) {
        this.head = head
    }
}
```

第三步:让我们使用第一步创建的类创建一些节点。

```
let node1 = new Node("hello") //we create the first node with a data equal to "hello"let node2 = new Node("world") //we create the second node with a data equal to "world"node1.next = node2 //we connect these two nodes with a pointer .next
```

**第四步:**是时候把我们的 *node1* 加入列表了！

```
let list = new LinkedList(node1)
```

由于 *node1* 和 *node2* 是用指针连接的，所以我们的 *node2* 也在链表中！所以，我们的**第五步**就是 console.log

```
console.log(list.head.next.data) //returns "world"
```

那是我们的第一个链表！

![](img/00bafe53c1d0ed7e688573722a5ace19.png)

**来源:**

1.  [维基百科](https://en.wikipedia.org/wiki/Linked_list)
2.  [极客 ForGeeks](https://www.geeksforgeeks.org/data-structures/linked-list/)
3.  [自由代码营](https://www.freecodecamp.org/news/implementing-a-linked-list-in-javascript/)
4.  [JS 中的链表](https://codeburst.io/linked-lists-in-javascript-es6-code-part-1-6dd349c3dcc3)
5.  [Udemy JavaScript 算法和数据结构 Masterclass](https://www.udemy.com/course/js-algorithms-and-data-structures-masterclass/)