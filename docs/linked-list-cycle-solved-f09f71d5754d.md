# 链表循环:已解决

> 原文：<https://blog.devgenius.io/linked-list-cycle-solved-f09f71d5754d?source=collection_archive---------5----------------------->

![](img/aae5d6a1960e7fb070e2905f5312d975.png)

匆忙解决 Leetcode 问题，找到了这个:[链表循环](https://leetcode.com/problems/linked-list-cycle/)。在这篇博客中，我将使用两点法来解决这个问题。

我们的任务是确定链表中是否有循环。如果链表中有循环，则返回 true。否则，返回 false。

什么被认为是循环？如果链表中有某个节点可以通过连续跟随下一个指针再次到达，那么链表中就存在循环。看看这个例子。

![](img/220d96c77811c2d879cb6415abb8e69b.png)

leetcode.com

**输入:** head = [3，2，0，-4]，pos = 1(tail 的下一个指针所连接的节点的索引；未作为参数传递)
**输出:**真
**解释:**链表中有一个循环，其尾部连接到第 1 个节点(索引为 0)。

这是我发现的最直观的方法。它被称为具有慢速指针和快速指针的双指针方法。

所以，我们的第一步是创建两个指针。第一个比较慢，因为它只从前一个位置移动了一个节点。第二个是快速的，它从前一个位置移动了两个节点。

然后，我们将创建一个 while 循环，并在每次迭代中检查:

*   快速指针是否到达终点。如果是，返回 false，因为它不是一个循环。
*   快速指针和慢速指针是否在同一个节点。如果是的话，那就有一个循环，因为没有循环就不可能回到同样的位置。

所以，我的代码是:

```
var hasCycle = function(head) { if(head === null) return false; //if there is no head node, there is no cycle let slow = head;
    let fast = head.next; while(slow !== fast && fast !== null) {
        if(fast.next === null) return false
        fast = fast.next.next
        slow = slow.next
    }
    return slow === fast ? true : false
};
```

希望这对你有帮助！