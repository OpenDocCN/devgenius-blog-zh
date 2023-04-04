# 在 python 中循环遍历列表

> 原文：<https://blog.devgenius.io/loops-through-a-list-in-python-75db1ad72691?source=collection_archive---------15----------------------->

![](img/cdc03593c8f611574621882c55aae61a.png)

由[穆罕默德·拉赫马尼](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/python-programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄的照片

假设我们想对列表中的每一项执行相同的任务。我们不用重复做同样的任务，而是使用循环，因为它可以自动执行重复的任务。循环是一系列不断重复的指令，直到达到某个条件。

python 中有两种类型的循环，for 和 while。

# For 循环

当您想对列表中的每一项执行相同的操作时，可以使用 Python 的 for 循环。

*   按颜色打印每种颜色。

```
Colors=['pink', 'red', 'gray']
for Color in Colors:
 print(Color)
```

**注意**:-对 print()的调用要缩进。

输出:

```
pink
red
gray
```

*   如果我们使用一个没有循环的索引重复做这件事会怎么样:

```
Colors=['pink', 'red', 'gray']
print(Colors[0])
print(Colors[1])
print(Colors[2])
```

输出:

```
pink
red
gray
```

**Python 列表循环带枚举**:

`enumerate`函数允许我们用它们的索引遍历列表元素。

```
Colors=['pink', 'red', 'gray']

for idx, Colors in enumerate(Colors):

    print(f"{idx}: {Colors}")
```

输出:

```
0: pink
1: red
2: gray
```

**列出 3 的倍数:**

```
multiplied_list = []
for value in range(1, 10):
 multiplied_list.append(value * 3)
print(multiplied_list)
```

输出:

```
[3, 6, 9, 12, 15, 18, 21, 24, 27]
```

# while 循环

使用 while 循环，只要条件为真，我们就可以执行一组语句。

打印所有项目，使用一个`while`循环遍历所有的索引号

```
Colors=['pink', 'red', 'gray']
x = 0
while x < len(Colors):
  print(Colors[x])
  x = x + 1
```

输出:

```
pink
red
gray
```

## While 循环中的 Break 语句

使用 break 语句，即使 while 条件为真，我们也可以停止循环:

我们再次做了和上面代码一样的工作，不同的是当 x 的值变成 1 时，循环中断。

```
Colors=['pink', 'red', 'gray']
x = 0
while x < len(Colors):
  print(Colors[x])
  if x == 1:
    break
  x = x + 1
```

输出:

```
pink
red
```

## else 语句

使用 else 语句，我们可以在条件不再为真时运行一次代码块:

```
Colors=['pink', 'red', 'gray', 'white', 'blue']
x = 0
while x < len(Colors):
  print(Colors[x])
  x = x + 1
else:
 print("finish")
```

输出:

```
pink
red
gray
white
blue
finish
```

如果你喜欢这篇文章，请在 medium 上关注我，了解更多关于 python 的故事。