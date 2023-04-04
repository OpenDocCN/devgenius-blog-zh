# 链表:Go-lang 和 Python 实现。

> 原文：<https://blog.devgenius.io/linked-lists-go-lang-and-python-implementations-b918b7657f57?source=collection_archive---------11----------------------->

**前奏**

链表为基于数组的序列提供了另一种选择，即链表。基于数组的序列和链表都以特定的顺序保存元素，但是这种特性的实现在两者之间有很大的不同。

数组提供了一种更集中的表示方式，一大块内存能够容纳对许多元素的引用。另一方面，链表依赖于更加分布式的体系结构，其中称为节点的轻量级对象用于表示链表的每个单独元素。

链表中的每个节点维护对其组成元素的引用，以及对相邻节点的一个或多个引用，这取决于它是单链表实现还是双链表实现。这种表示允许我们共同表示序列的线性顺序。

# 单链表

下面我们将给出单链表在 python 和 Go-lang 中的队列实现，现在这可以扩展到栈和树的双链表。

**记忆使用**

与双向链表相比，单向链表占用更少的内存，因为它只需要一个指向其后继链表的指针。每个指针变量保存一个元素的地址，指针占用 4 个字节；因此，指针变量在单链表中占用的内存空间是 4 个字节。所以内存也按照 O(n)扩展，n 表示元素的数量，每个指针的 4 个字节保持不变。

**时间复杂度**

单链表的访问时间为 0(1)，搜索时间为 O(n)，因为我们必须在一个方向上从第一个到最后一个遍历整个链表，所以在最坏情况和平均情况下，它的插入时间为 O(n)，删除时间为 O(n)。

**Go-lang 单链表队列实现**

```
package mainimport "fmt"type Node struct {
    element int
    next    *Node
}
type GoLinkedQue struct {
    head *Node
    tail *Node
    size int
}func (l *GoLinkedQue) Len() int {
    return l.size
}func (l *GoLinkedQue) is_empty() bool {
    if l.size == 0 {
        return true
    }
    return false
}func (l GoLinkedQue) First() (int, error) {
    if l.head == nil {
        return 0, fmt.Errorf("The Queue is empty !")
    }
    return l.head.element, nil
}func (l GoLinkedQue) Last() (int, error) {
    if l.head == nil {
        return 0, fmt.Errorf("The Queue is empty !")
    }
    if l.size == 1 {
        return l.head.element, nil
    }
    return l.tail.element, nil
}func (l *GoLinkedQue) dequeue() int {
    if l.is_empty() {
        fmt.Errorf("The Queue is empty !")
    }
    answer := l.head.element
    l.head = l.head.next
    l.size--
    if l.is_empty() {
        l.tail = nil
    }
    return answer}
func (l *GoLinkedQue) enqueue(e int) *Node {
    newest := &Node{
        element: e,
        next:    nil,
    }
    if l.is_empty() {
        l.head = newest
    } else {
        l.tail.next = newest
    }
    l.tail = newest
    l.size++
    return newest
}func main() {
    queue := GoLinkedQue{}
    queue.enqueue(100)
    queue.enqueue(200)
    queue.enqueue(300)
    queue.enqueue(400)
    queue.enqueue(500)
    queue.enqueue(600)
    firstval, _ := queue.First()
    lastval, _ := queue.Last()
    fmt.Println("Length = ", queue.Len())
    fmt.Println("First Element :", firstval)
    fmt.Println("Last Element  :", lastval)
    fmt.Println("Is Queue empty:", queue.is_empty())
    queue.dequeue()
    queue.dequeue()
    queue.dequeue()
    queue.dequeue()
    queue.dequeue()
    queue.dequeue()
        fmt.Println("Length =", queue.Len())
        fmt.Println("Is Queue empty:", queue.is_empty())    
}
```

如果你在 go-lang-playground 运行上面的代码，你会得到下面的结果

**输出**

```
Length =  6
First Element : 100
Last Element  : 600
Is Queue empty: false
Program exited.
```

**下面用 Python 实现一个队列结构的单链表**

```
1 class PythonLinkedQueue:                                                                                                                                            
  2     """ A python implementation of a queue data structure using a 'singly linked-list' for storage."""
  3 
  4     class _Node:
  5         """"Lightweight, non-public class for storing  a single Nodes attributes."""
  6         __slots__ = "_element", "_next"
  7 
  8         
  9         def __init__(self, element, next):
 10             """"Instantiate a single Node object."""
 11             self._element = element
 12             self._next    = next
 13 
 14     
 15     def __init__(self):
 16         """Create an empty queue."""
 17         self._head = None
 18         self._tail = None
 19         self._size = 0
 20 
 21 
 22     def __len__(self):
 23         """Return the number of elements in the queue."""
 24         return self._size
 25 
 26     def is_empty(self):
 27         """Return True if the queue is empty. """
 28         return self._size == 0
 29 
 30     def first(self):
 31         """Return but do not remove the element that sits at the front of the queue."""
 32         if self.is_empty():
 33             raise Empty("The Queue is Empty!")
 34         return self._head._element
 35 
 36     def last(self):
 37         """ Return butt do not remove the element that sits at the back of  the queue."""
  38         if self.is_empty():
 39             raise Empty("The Queue is Empty!")
 40         elif self._size == 1:
 41             return self._head._element
 42         else:
 43             return  self._tail._element
 44 
 45     def dequeue(self):
 46         """Remove and return the first element of the queue(FIFO)"""
 47         if self.is_empty():
 48             raise Empty('Queue is empty')
 49         result =  self._head._element
 50         self._head = self._head._next
 51         self._size -= 1
 52         if self.is_empty():
 53             self._tail = None
 54         return result
 55 
 56     def enqueue(self, e):
 57         """Add an element to the back of the queue. """
 58         newest = self._Node(e, None)
 59         if self.is_empty():
 60             self._head = newest
 61         else:
 62             self._tail._next = newest
 63         self._tail = newest
 64         self._size += 1
```

# 双向链表

在单个喜欢的列表中，每个节点维护一个对紧随其后的节点的引用。单链表的这种不对称带来了明显的局限性。例如不能有效地从列表的尾部删除一个节点，或者甚至我们不能从内部位置执行一个任意的节点，如果仅仅给定一个对该节点的引用，因为我们不能确定紧接在我们想要删除的节点之前的节点。

这就是双向链表出现的地方。为了提供更大的对称性，我们定义了一个链表，其中每个节点都保持一个对它之前的节点的显式引用和对它之后的节点的引用。

**内存使用量**

这个列表实现在单个节点中保存两个地址，一个地址指向前一个节点，另一个地址指向下一个节点。因此，两个指针变量占用的空间是 8 个字节。这个空间也必然会按照 O(n)扩展，n 代表链表当前拥有的节点数，每个节点占用每个节点 8 个字节的常量。

**时间复杂度**

在平均时间和最坏情况下，该双向链表的插入时间复杂度为 O(1)，删除时间复杂度为 O(1)，搜索时间复杂度为 O(n)，访问时间复杂度为 O(n)。

我们将使用双向链表实现 deque 数据结构。Deque 是一种类似队列的数据结构，支持在队列的前端和后端插入和删除。

**割台和拖车哨兵**

为了避免在双向链表的边界附近操作时的一些特殊情况，在链表的两端添加特殊的节点是有帮助的:在链表的开头添加一个头节点，在链表的结尾添加一个尾节点。这些“虚拟”节点被称为哨兵(或守卫)，它们不存储主
序列的元素。

**用 python 实现**

```
1 class _DoublyLinkedBase:
  2     """"A base class providing a doubly linked list representation."""                                                                                              
  3 
  4     class _Node:
  5         """A lightweight, non public class for storing a doubly linked node"""
  6         __slots__ = "_element","_next","_prev"
  7 
  8         def __init__(self, elment, prev, next):
  9             self._element = element
 10             self._prev    = prev
 11             self._next    = next
 12 
 13     def __init__(self):
 14         """Create ane empty list."""
 15 
 16         self._header  = self._Node(None, None, None)
 17         self._trailer = self._Node(None, None, None)
 18         self._header._next = self._trailer
 19         self._header._prev = self._header
 20         self._size = 0
 21 
 22     def  __len__(self):
 23         """Return the number of elements in the list."""
 24         return self._size
 25 
 26 
 27     def is_empty(self):
 28         """Return True if list is empty."""
 29         return self._size == 0
 30 
 31     def _insert_between(self, e, predecessor, successor):
 32         """Add element e between two existing nodes and return new node."""
 33         newest = self._Node(e, predecessor, successor)
 34         predecessor._next = newest
 35         successor._prev = newest
 36         self._size += 1                                                                                                                                             
 37         return newest 
 38 
 39     def _delete_node(self, node):
 40         """ Delete nonsentinel node from the list and return its element."""
 41         predecessor = node._prev
 42         successor = node._next
 43         predecessor._next = successor
 44         successor._prev = predecessor
 45         self._size -= 1
 46         element = node._element
 47         node._prev = node._next = node._element = None
 48         return element
```

上面的基类由下面的类继承:

```
1 class LinkedDeque(_DoubleLinkedBase):
  2     """ Double-ended queue implemented based on a doubly linked list."""                                                                                            
  3 
  4     def first(self):
  5         """Return but do not remmove the element at the front of th deque."""
  6         if self.is_empty():
  7             raise Empty("Deque is empty")
  8         return self._header._next._element
  9 
 10 
 11     def last(self):
 12         """Return but do not remove the element at the back of the deque."""
 13         if self.is_empty():
 14             raise Empty("Deque is empty")
 15 
 16         return self._trailer._prev._element
 17 
 18 
 19     def insert_first(self, e):
 20         """Add an element to the front of the deque."""
 21         self._insert_between(e, self._header, self._header._next)
 22 
 23     def insert_last(self, e):
 24         """Add an element to the backk of the deque."""
 25         self._insert_between(e, self._trailer._prev, self._trailer)
 26 
 27     def delete_first(self):
 28         """Remove and return the lement from the front of the deque."""
 29         if self.is_empty():
 30             raise Empty("Deque is empty")
 31         return self._delete_node(self._header._next)
 32 
 33     def delete_last(self):
 34         """Remove and return the element from the back of the deque."""
 35         
 36         if self.is_empty():                                                                                                                                         
 37             raise Empty("Deque is empty") 
 38         return self._delete_node(self._trailer._prev)
 39
```

为了在 Go-lang 中使用双向链表实现 deque 数据结构，我们实现如下

**在 Go-lang 实现。**

```
package mainimport "fmt"type Node struct {
    element int
    prev    *Node
    next    *Node
}type GoDoublyLinkedQue struct {
    header  *Node
    trailer *Node
    size int
}func (l *GoDoublyLinkedQue) Len() int {
    return l.size
}func (l *GoDoublyLinkedQue) is_empty() bool {
    if l.size == 0 {
        return true
    }
    return false
}func (l GoDoublyLinkedQue) First() (int, error) {
    if l.header.next == l.trailer {
        return 0, fmt.Errorf("The Queue is empty !")
    }
    return l.header.next.element, nil
}func (l GoDoublyLinkedQue) Last() (int, error) {
    if l.trailer.prev == l.header {
        return 0, fmt.Errorf("The Queue is empty !")
    }
    return l.trailer.prev.element, nil
}func (l *GoDoublyLinkedQue) insert_between(e int, predecessor, sucessor *Node) *Node {
    newest := &Node{
        element: e,
        prev:    predecessor,
        next:    sucessor,
    }
    predecessor.next = newest
    sucessor.prev = newest
    l.size++
    return newest
}func (l *GoDoublyLinkedQue) delete_node(node *Node) int {
    predecessor := node.prev
    sucessor := node.next
    predecessor.next = sucessor
    sucessor.prev = predecessor
    l.size--
    velement := node.element
    node.prev = nil
    node.next = nil
    node.element = 0
    return velement
}func (l *GoDoublyLinkedQue) insert_first(e int) *Node {
    node := l.insert_between(e, l.header, l.header.next)
    return node}func (l *GoDoublyLinkedQue) insert_last(e int) *Node {
    node := l.insert_between(e, l.trailer.prev, l.trailer)
    return node
}func (l *GoDoublyLinkedQue) delete_first() int {
    if l.is_empty() {
        fmt.Errorf("The Deque is empty")
    }
    element := l.delete_node(l.header.next)
    return element
}func (l *GoDoublyLinkedQue) delete_last() int {
    if l.is_empty() {
        fmt.Errorf("The deque is empty")
    }
    element := l.delete_node(l.trailer.prev)
    return element
}func main() {
    deque := GoDoublyLinkedQue{}
    deque.header = &Node{
        element: 0,
        prev:    nil,
        next:    nil,
    }
    deque.trailer = &Node{
        element: 0,
        prev:    nil,
        next:    nil,
    }
    deque.header.next = deque.trailer
    deque.trailer.prev = deque.header
    deque.insert_first(100)
    deque.insert_last(500)
    firstVal, _ := deque.First()
    lastVal, _ := deque.Last()
    fmt.Println("Length = ", deque.Len())
    fmt.Println("Is deque empty: ", deque.is_empty())
    fmt.Println("First Element :", firstVal)
    fmt.Println("Last Element  :", lastVal)
    deque.delete_last()
    deque.delete_first()
    fmt.Println("Is deque empty: ", deque.is_empty())
}
```

上面的代码导致

```
Length =  2
Is deque empty:  false
First Element : 100
Last Element  : 500
Is deque empty:  trueProgram exited.
```

再见，一会儿见。