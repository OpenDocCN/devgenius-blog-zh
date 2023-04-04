# Python 集

> 原文：<https://blog.devgenius.io/python-set-9b21a0f496eb?source=collection_archive---------12----------------------->

# 概观

集合是未排序和未索引的唯一项目的集合。这意味着没有重复项，并且不能像访问列表或字典那样访问项目。一旦您将物品添加到器械包中，就不能对其进行更改，但是您可以删除和添加新的物品。

# 创造

要创建集合，可以使用花括号或 set 构造函数。

```
newset1 = {"one", "two", "three"}
newset2 = set(("one", "two", "three"))print(newset1 == newset2)
>> True
```

# 插入

使用 add()方法添加项。

```
newset = {"one", "two", "three"}
newset.add("four")
print(newset)
>> {'one', 'four', 'three', 'two'}
```

若要添加多个项目，请使用 update()方法。

```
newset = {"one", "two", "three"}
newset.update(["five", "six"])
print(newset)
>> {'five', 'six', 'one', 'two', 'three'}
```

如果您尝试使用 add()或 update()方法添加一个重复项，您不会得到错误消息，但是该重复项不会出现在集合中。

# 阅读

要访问集合中的项目，您可以循环访问集合。

```
newset = {"one", "two", "three"}
for num in newset:
  print(num)
>> one
>> two
>> three
```

若要检查项目是否在集合中，请使用 in 关键字。

```
newset = {"one", "two", "three"}
print("one" in newset)
>> True
print("four" in newset)
>> False
```

# 删除

要从集合中移除特定的项目，可以使用 remove()方法。如果项目不在集合中，它将抛出一个错误。

```
newset = {"one", "two", "three"}
newset.remove("one")
print(newset)
>> {'two', 'three'}
```

同样，您也可以使用 discard()方法删除特定的元素，但是如果它不在集合中，它不会抛出错误。

```
newset = {"one", "two", "three"}
newset.discard("one")
print(newset)
>> {'two', 'three'}
```

要删除集合中的最后一项，可以使用 pop()方法。pop()方法也将返回从集合中删除的项目。但是，器械包是无序的，因此物品可能不是您所期望的。

```
newset = {"one", "two", "three"}
x = newset.pop()
print(x)
>> two
print(newset)
>> {'one', 'three'}
```

要从集合中移除所有项目，可以使用 clear()方法。

```
newset = {"one", "two", "three"}
newset.clear()
print(newset)
>> set()
```

要删除集合本身，您可以使用 del 关键字

```
newset = {"one", "two", "three"}
del newset
# Error will be thrown because newset does not exist anymore
print(newset)
```

# 长度

要获得集合的长度，可以使用 len()方法。

```
newset = {"one", "two", "three"}
print(len(newset))
>> 3
```

# 连接集合

要连接两个集合，可以使用 union()方法，该方法将返回一个新的集合。

```
newset1 = {"one", "two", "three"}
newset2 = {1, 2, 3}
print(newset1.union(newset2))
>> {1, 'one', 2, 3, 'two', 'three'}
```

您可以使用 update()方法将项目从一个集合移到另一个集合。

```
newset1 = {"one", "two", "three"}
newset2 = {1, 2, 3}
newset1.update(newset2)
print(newset1)
>> {1, 2, 3, 'three', 'two', 'one'}
```

# 结论

像其他语言一样，set 是一种无序、无索引、无重复的数据结构。集合中的项目被散列，所以对于插入、读取和移除，它是 O(1)。如果你不在乎重复和顺序，你会希望使用一套。