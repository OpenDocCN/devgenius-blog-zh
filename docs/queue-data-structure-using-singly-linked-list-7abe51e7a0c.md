# 队列数据结构||使用单链表

> 原文：<https://blog.devgenius.io/queue-data-structure-using-singly-linked-list-7abe51e7a0c?source=collection_archive---------26----------------------->

在本文中，我们将学习队列数据结构，以及如何在 swift 中使用链表对队列进行编码

![](img/74e93fbcd85cae8d6227a752f56d8a64.png)

路易斯·恩古吉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 为什么？又是怎么做到的？

队列基本上遵循 FIFO 组织，FIFO 的意思是先进先出。现在让我们来看一个例子，假设你有一台打印机，有很多人给打印机下命令要打印，现在打印机会把命令排列成队列，这样第一个人的命令会先得到打印。

队列主要执行两个主要动作，第一个是入队和出队。入队意味着在队列中添加元素，出队意味着从队列中移除元素。

将有两个指针头和尾，头将指向插入的第一个元素，尾将是下一个。

我们可以使用数组和链表来执行堆栈数据结构，但是使用数组来实现是困难和奇怪的，而使用链表来实现是非常方便和容易的。所以我们用链表来查找。

# 在 Swift 中排队

**现在让我们看看使用链表的代码:**

```
 **public** **class** Node<T>{
    **var** value : T 
    **var** next : Node<T>? 

    **public** **init**(value:T){  
        **self**.value = value
    }}**public** **class** Queue<T>{
    **var** head : Node<T>!
    **public** **var** isEmpty : Bool { **return** head == **nil** }
    **var** first : Node<T>? { **return** head }
    **var** last : Node<T>? {
        **if** **var** currentNode = **self**.head{
            **while** **case** **let** next? = currentNode.next{
                currentNode = next
            }
            **return** currentNode
        }**else**{
             **return** **nil** }
    }
    **func** enqueue(value : T){
        **let** newItem = Node(value: value)
        **if** **let** lastNode = last{
            lastNode.next = newItem
        }**else**{
            head = newItem
        }
    } **func** dequeue() -> T? {
        **if** **self**.head?.value == **nil** { **return** **nil**} 
        **if** **let** nextItem = **self**.head?.next {
            head = nextItem
        }**else**{
            head = **nil** }
        **return** head?.value
    }
}
```