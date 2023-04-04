# Go 中的队列

> 原文：<https://blog.devgenius.io/queues-in-go-89b2cc9d7ff9?source=collection_archive---------3----------------------->

欢迎回到*Go*中的数据结构介绍！在本帖中，我们将关注队列。我们在现实生活中经常听到“队列”这个词。买电影票的队伍很长。伙计，这场联赛排队排得太久了。对于我们大多数人来说，队列是一个熟悉的概念。

# 作为数据结构的队列

队列是一个特殊的列表，就像堆栈一样。然而，与堆栈不同，队列是众所周知的 FIFO 数据结构。这意味着先进入的项目先出来。你可以想象一个公共汽车的队列，队列中的第一个人将首先上车，而其他人必须等到轮到他们的时候。队列是一个简单的列表，你只能在尾部(后面)插入，在头部(前面)弹出。

可以对队列执行的常见操作包括:

*   **插入**，又名**推动**或**插入**
*   **出清**，又名**弹出**或**删除**
*   前面
*   IsEmpty 或 IsFull

# 在 Go 中写入

让我们从定义类型开始。

```
type Queue struct {
    items []int
}
```

简单，我们需要做的就是定义某个数据类型的切片。

现在介绍两个最重要的方法。

```
func (q *Queue) Enqueue(data int) {
    q.items = append(q.items, data)
}func (q *Queue) Dequeue() {
    q.items = q.items[1:]
}
```

这些也很简单。通过使用`append`，我们正在向`q.items`的尾端插入新元素。当出列时，我们只是从第一个元素到最后一个元素取一部分`q.items`。请记住，我们在编程中从 0 开始计数，因此第 0 个元素是列表中的第一个元素。

这里有一些有用的帮助方法，会让我们的生活更轻松。

```
func (q *Queue) Front() (int, error) {
    if len(q.items) == 0 {
        return 0, fmt.Errorf("queue is empty")
    } return q.items[0], nil
}func (q *Queue) IsEmpty() bool {
    if len(q.items) == 0 {
        return true
    } else {
        return false
    }
}func (q *Queue) Print() {
    for _, item := range q.items {
        fmt.Print(item, " ")
    }
    fmt.Println()
}
```

如果队列为空，将返回一个错误，否则将返回列表中的第 0 个元素。

`IsEmpty`检查我们的队列长度是否为 0，如果是，返回`true`。

`Print`遍历我们的项目并打印每一项。

我们试试用吧！

```
func main() {
    myQueue := Queue{[]int{}}
    myQueue.Print()
    fmt.Println(myQueue.IsEmpty()) myQueue.Enqueue(1)
    myQueue.Enqueue(2)
    myQueue.Enqueue(3) myQueue.Print()
    myQueue.Dequeue()
    myQueue.Print()
}Output:true
1 2 3
2 3
```

# 优先级队列

这很简单。既然我们已经了解了普通队列是如何工作的，我们就可以讨论优先级队列了。优先级队列是特殊类型的队列，每个项目分配有不同的优先级。想象你在一个机场，等待你的航班。空服员通知人们他们马上就要开始了。你匆忙收拾好行李，排在队伍的第一位。你先登机吗？通常不会。他们会先让老兵和老人进去，然后是头等舱的乘客。你在他们之后登机。

优先级队列的工作方式略有不同。从优先级队列中出列时，优先级最高的项目将首先出列。当你调用`Front`时，你会得到一个优先级最高的物品。

让我们看看如何在 Go 中写这个。

```
type Item struct {
    data     int
    priority int
}type PriorityQueue struct {
    items []Item
}
```

这就是我们如何定义优先级队列和其中的项目。一个条目包含两条信息:实际存储的数据和它的优先级。优先级队列存储一片`Item`对象。

```
func (pq *PriorityQueue) Sort() {
    sort.Slice(pq.items, func(i, j int) bool {
        return pq.items[i].priority < pq.items[j].priority
    })
}
```

`Sort`大概是我们优先级排队最重要的方法。顾名思义，`Sort`按照优先级从高到低的顺序对优先级队列中的项目进行排序。优先级用一个`int`值表示，值越小意味着优先级越高。第一，第二，第三。

`Sort`使用 Go 内置的`sort`包。`sort.Slice`接受任意类型的切片和确定如何排列项目的任意函数。在这种情况下，我们将比较第 I 项和第 j 项的优先级。

```
func (pq *PriorityQueue) Enqueue(data Item) {
    pq.items = append(pq.items, data)
    pq.Sort()
}func (pq *PriorityQueue) Dequeue() {
    pq.Sort()
    pq.items = pq.items[1:]
}func (pq *PriorityQueue) Front() (Item, error) {
    if len(pq.items) == 0 {
        return Item{}, fmt.Errorf("queue is empty")
    } pq.Sort()
    return pq.items[0], nil
}
```

`Enqueue`、`Dequeue`和`Front`基本保持不变，除了它们都调用`Sort`这个事实。

```
func (pq *PriorityQueue) IsEmpty() bool {
    if len(pq.items) == 0 {
        return true
    } else {
        return false
    }
}func (pq *PriorityQueue) Print() {
    for _, item := range pq.items {
        fmt.Print(item, " ")
    }
    fmt.Println()
}
```

`IsEmpty`和`Print`保持不变。

我们试试用吧！

```
func main() {
    myQueue := PriorityQueue{[]Item{}}
    myQueue.Print()
    fmt.Println(myQueue.IsEmpty()) myQueue.Enqueue(Item{0, 1})
    myQueue.Enqueue(Item{1, 4})
    myQueue.Enqueue(Item{2, 3})
    myQueue.Enqueue(Item{3, 0})
    myQueue.Enqueue(Item{4, 2}) myQueue.Print()
    myQueue.Dequeue()
    myQueue.Print()
}Output:true
{3 0} {0 1} {4 2} {2 3} {1 4} 
{0 1} {4 2} {2 3} {1 4}
```

请注意我们是如何按照从 0 到 4 的升序添加项目的，但是之后会按照优先级排序。

# 结论

感谢您的阅读！这是对队列和优先级队列的简单介绍。如果你想了解更高级的概念，网上有更多的资源。希望你在这个过程中学到了一些东西！

你可以在 [Dev.to](https://dev.to/jpoly1219/queues-in-go-22i9) 和 [my personal site](https://jpoly1219.github.io) 上阅读这篇文章。