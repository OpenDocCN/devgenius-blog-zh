# 合并 k 个排序列表

> 原文：<https://blog.devgenius.io/merge-k-sorted-lists-a7462bfd102c?source=collection_archive---------1----------------------->

如何合并多个排序链表中所有节点的集合？

> Leetcode 问题 23([https://leetcode.com/problems/merge-k-sorted-lists/](https://leetcode.com/problems/merge-k-sorted-lists/)

![](img/58d8f42c853238941791f209a2cf375f.png)

leet code—[https://leetcode.com/](https://leetcode.com/)

本文是一个新系列的一部分，在这个系列中，我将解决一些 Leetcode 问题，然后详细解释它们的解决方案。
在所有这些文章中，我将遵循一个由四部分组成的策略—

1.  问题的描述
2.  关于如何解决文章的提示
3.  详细的实际解决方案
4.  解的时间复杂度和空间复杂度

我强烈建议每个人先阅读提示，然后尝试自己解决问题。一旦他们尝试解决，他们可以参考我的解决方案，看看他们的方法与我的有何不同。最后，您可以回顾时间和空间复杂性，以真正深入地理解解决方案。

# **问题**

> 给你一个`*k*`链表`*lists*`的数组，每个链表按升序排序。
> 
> *将所有链表合并成一个排序后的链表并返回。*

**例 1:**

```
**Input:** lists = [[1,4,5],[1,3,4],[2,6]]
**Output:** [1,1,2,3,4,4,5,6]
**Explanation:** The linked-lists are:
[
  1->4->5,
  1->3->4,
  2->6
]
merging them into one sorted list:
1->1->2->3->4->4->5->6
```

**例 2:**

```
**Input:** lists = []
**Output:** []
```

**例 3:**

```
**Input:** lists = [[]]
**Output:** []
```

# **提示**

方法一:暴力

您可以遍历所有的链表，将每个值添加到一个数组中，最后将数组的排序版本作为一个链表返回。

**方法 2:使用优先级队列**

优先级队列只不过是一个为每个元素分配了优先级的队列。具有高优先级的元素在具有低优先级的元素之前被服务。当元素从优先级队列中弹出时，所获得的结果或者按升序排序，或者按降序排序。而当从简单队列中弹出元素时，在结果中获得数据的 FIFO 顺序。

# 解决办法

## **方法一:蛮力**

```
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeKLists(self, lists: List[ListNode]) -> ListNode:

        nodes = []
        head = point = ListNode(0)
        for l in lists:
            while l:
                nodes.append(l.val)
                l = l.next

        for k in sorted(nodes):
            point.next = ListNode(k)
            point = point.next
        return head.next
```

我们将每个列表中每个节点的值保存在节点列表中。然后我们遍历一个排序列表，并保持指针指向排序节点列表中的下一个元素。最后，我们返回 `head.next`,它只不过是最后一个链表的开始，这个链表按照升序排列。

## 方法 2:使用优先级队列

```
from heapq import heappush, heappop, heapifyclass Solution(object):
     def mergeKLists(self, lists):
        """
        :type lists: List[ListNode]
        :rtype: ListNode
        """

        pre = cur = ListNode(0)

        heap = []
        for i in range(len(lists)):
            if lists[i]: 
                heapq.heappush(heap, (lists[i].val, i, lists[i]))

        while heap:
            idx, node = heapq.heappop(heap)
            cur.next = node
            cur = cur.next

            if cur.next:
                heapq.heappush(heap, (cur.next.val, idx, cur.next))

        return pre.next
```

我们首先使用`heapq` 作为`heap`声明一个优先级队列，并将所有值放入这个队列中。现在我们迭代这个优先级队列并弹出这些值，这些值将按升序从队列中弹出，并保存在`idx`和`node`中。接下来，我们简单地迭代`cur`并更新`cur` 以指向下一个值。

# 复杂性

## 方法 1:使用暴力

**时间复杂度:**
*O(NlogN)* 其中 *N* 为节点总数。

*   收集所有值花费 *O* ( *N* )时间。
*   一个稳定的排序算法需要花费*O*(*N*log*N*)的时间。
*   创建链表的迭代花费 *O* ( *N* )时间。

**空间复杂度:**
*O*(*N*)

*   排序代价 *O* ( *N* )空间(取决于你选择的算法)。
*   创建一个新的链表需要花费 *O* ( *N* )的空间。

## 方法 2:使用优先级队列

它的运行时间将是 O(nklog(k))。
while 循环将运行 k * n 顺序的列表中的所有节点，其中 n 是列表的长度。每次在 while 循环中，heappop 和 heap replace 花费 O(log(k))时间，因为我们在堆中最多有 k 个元素。所以 while 循环的运行时间是 O(nklog(k))。

我希望你对这篇文章感兴趣，如果你能想出其他方法来解决这个问题，请告诉我。如果您发现我的现有解决方案有任何改进，请联系我。我很想听到你的反馈。
你也可以在 [Twitter](https://twitter.com/RahilSarvaiya) 上关注我，我会在 Twitter 上发布我的软件开发之旅，也会更新我的最新文章。