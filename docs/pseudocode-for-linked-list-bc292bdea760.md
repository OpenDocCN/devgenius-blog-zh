# 链表的伪代码👨‍💻

> 原文：<https://blog.devgenius.io/pseudocode-for-linked-list-bc292bdea760?source=collection_archive---------1----------------------->

![](img/03e9f6197dd3bc759907238bdc9e5a58.png)

照片由[克莱门特·H](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

对其代码的简短和最佳理解👽|链接列表|代码|程序

这是链表的伪代码，是针对单链表的。如果你不想要 python 代码，那么只关注下面写的文本点。但是如果你试着用代码来理解，那么这将是对你最好的练习。

```
**1\. Create a Class For A Node . Initialise the Properties which are   needed in a Node . (value and link):**class Node:
  def __init__(self,value,link = None):
    self.value = value 
    self.link = link**2\. Create a Class For your linked list which will have an initialised First Node as None and other needed methods.**class LinkedList:
  def __init__(self):
    self.firstNode= None**3\. Now create those method which you have the need (inserting , deleting , displaying) in side the linked list class .
*Inserting at starting:*** Create a New Node .Then check if there is already a node in your list then link your new node to first node and then assign first node into new node . And if there is no nodes then assign first node as new node . def insertAtbegning(self,value):
    newNode = Node(value)
    if self.firstNode != None:
      newNode.link = self.firstNode
      self.firstNode = newNode
    else:
      self.firstNode = newNode
***Inserting at end :*** Create a new node and if there is already nodes in linked list then travel through the nodes . def insertAtEnd(self,value):
   newNode = Node(value)
   if self.firstNode != None:
     current = self.firstNode
     while current.link != None:
       current = current.link
     current.link = newNode
   else:
     self.firstNode = newNode**4\. Create a method which can display all the nodes of your linked list . And check if list is empty then return it and if it present then travel the list and display them**   def display(self):
    if self.firstNode == None:
      print("List is empty")
      return 
    current = self.firstNode
    while current.link != None:
      print(current.value)
      current = current.link**5\. Create a method which can delete the element by its value , and check if the list is empty then return it and if its not empty then check for the first element that , is it the target ? if ys then return . And if first node is not the target then travel the list and find .** def delete(self,value):
    if self.firstNode == None:
      print("list is empty")
      return
    if self.firstNode == value:
      temp = self.firstNode
      self.firstNode = temp.link
      temp = None
      return     
    current = self.firstNode
      while current.link != None:
        if current.link.value == value:
          temp = current.link
          current.link = temp.link
          temp = None 
          return
        current = current.link
      print("Element is not found")
```

# 来源

我的 LinkedIn:-【linkedin.com/in/my-pro-file】

# **我们将感谢你的支持**

**[![](img/fc66aa9e7102ad57aa923f363e138df4.png)](https://www.buymeacoffee.com/mohdYasir03)

[https://www.buymeacoffee.com/mohdYasir03](https://www.buymeacoffee.com/mohdYasir03)** 

# **您可能感兴趣的主题:**

*   ****合并排序**:[https://medium.com/swlh/title-1692d9fb5ced](https://medium.com/swlh/title-1692d9fb5ced)**
*   ****插入排序**:[https://medium . com/dev-genius/Insertion-Sort-program-in-swift-31740 a 454573](https://medium.com/dev-genius/insertion-sort-program-in-swift-31740a454573)**
*   ****计数排序**:[https://medium . com/@ MD code 2021/Counting-Sort-algorithm-c 32d 71 F2 cc 79](https://medium.com/@mdcode2021/counting-sort-algorithm-c32d71f2cc79)**
*   ****选择排序**:[https://medium . com/@ MD code 2021/line-by-line-Selection-Sort-algorithm-explained-in-c-DD 49638 b15e](https://medium.com/@mdcode2021/line-by-line-selection-sort-algorithm-explained-in-c-dd49638b15e)**

**![](img/b617cb67e563f4bf861c1899492acdf1.png)**