# 使用队列的队列和堆栈

> 原文：<https://blog.devgenius.io/queues-stacks-using-deque-e155fabfed84?source=collection_archive---------20----------------------->

![](img/0b17ff254147f296d7f4d83ee7eddf3b.png)

[**来源**](https://unsplash.com/photos/jnuiQZixZNg)

> D 队列是栈和队列的概括(名字读作“deck ”,是“双端队列”的简称)。Deques 支持线程安全、内存高效的追加，并从 deques 的任一侧弹出，在两个方向上具有大致相同的`O(1)`性能。

```
from ***collections*** import ***deque***
```

## 创建队列

```
***I/P :*** deque((1, 2, 3, 4))
***O/P :*** deque([1, 2, 3, 4])***I/P :*** deque(range(2,6)
***O/P :*** deque([2, 3, 4, 5])***I/P :*** deque("lmnop")
***O/P :*** deque(['l', 'm', 'n', 'o', 'p'])***I/P :*** q = {'Movie': 'RRR', "Director": 'SS Rajamauli'}
      deque(q.keys())
***O/P :*** deque(['Movie', 'Director'])***I/P :*** q = {'Movie': 'RRR', "Director": 'SS Rajamauli'}
      deque(q.values())
***O/P :*** deque(['RRR', 'SS Rajamauli'])***I/P :*** q = {'Movie': 'RRR', "Director": 'SS Rajamauli'}
      deque(q.items())
***O/P :*** deque([('Movie', 'RRR'), ('Director', 'SS Rajamauli')])
```

## 追加和追加左

```
***I/P :*** x = deque([2, 3, 4, 5])
      x.append(6)
***O/P :*** deque([2, 3, 4, 5, 6])***I/P :*** x = deque([2, 3, 4, 5])
      x.appendleft(1)
***O/P :*** deque([1, 2, 3, 4, 5])
```

## pop & popleft

```
***I/P :*** x = deque([2, 3, 4, 5])
      x.pop()
***O/P :*** 5***I/P :*** x = deque([2, 3, 4, 5])
      x.popleft()
***O/P :*** 2
```

## 延伸和向左延伸

```
***I/P :*** x = deque([2, 3, 4, 5])
      x.extend([6,7,8])
      x
***O/P :*** deque([2, 3, 4, 5, 6, 7, 8])***I/P :*** x = deque([2, 3, 4, 5])
      x.extendleft([1,0,-1])
      x
***O/P :*** deque([-1, 0, 1, 2, 3, 4, 5])
```

## 辐状的

```
***I/P :*** d = deque(['a','b','c','d'])
      d.rotate(1)                   # right rotation
      d
***O/P :*** deque(['d', 'a', 'b', 'c'])***I/P :*** d = deque(['a','b','c','d'])
      d.rotate(-1)                  # left rotation
      d
***O/P :*** deque(['b', 'c', 'd', 'a'])
```

# 参考资料:

## [RealPython](https://realpython.com/python-deque/)

## [**Python 库参考**](https://docs.python.org/2.5/lib/deque-objects.html)