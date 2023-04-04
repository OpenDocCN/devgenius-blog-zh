# Python 字典的 5 种不同排序方式

> 原文：<https://blog.devgenius.io/5-different-ways-to-sort-python-dictionary-e20274fdf304?source=collection_archive---------2----------------------->

## sorted()函数

![](img/31cf7ef99724995c05b5bfb860064c0a.png)

拉胡尔·潘迪特摄于 [Pexels](https://www.pexels.com/photo/assorted-color-crayons-2078147/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

有许多方法可以对 python 字典进行排序。在本文中，让我们看看对 python 字典进行排序的 5 种不同方法。

1.  使用 sorted()函数

2.使用反向参数排序

3.使用 key 参数排序

4.排序函数在词典理解中的应用

5.使用计数器

# 1.使用 sorted()函数

sorted()函数用于像字典一样对 iterable 进行排序，但它将返回一个排序后的列表。

## 示例 1:对字典进行排序

***sorted(D1 . items())***→D1 . items()将根据键对字典进行排序，并将返回包含键-值对的元组列表

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.items())
*#Output:[('apple', 2), ('banana', 3), ('cherry', 1)]*
```

## 示例 2:对字典中的键进行排序

***sorted(D1 . keys())***→它会单独对字典键进行排序，并返回排序后的键的列表。

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.keys())
*#Output:['apple', 'banana', 'cherry']*
```

## 示例 3:对字典中的值进行排序

***sorted(D1 . values())***→将字典值单独排序，返回字典值列表。

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.values())*#Output:[1,2,3]*
```

# 2.使用反向参数排序

要以逆序(降序)对字典进行排序，reverse 参数设置为 True。

## 示例 1:以逆序排序字典

默认情况下，如果我们设置 reverse=True，那么它将根据字典键以相反的顺序(降序)排序

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.items(),reverse=**True**)*#Output: [('cherry', 1), ('banana', 3), ('apple', 2)]*
```

## 示例 2:以逆序排序字典键

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.keys(),reverse=**True**)
*#Output:['cherry', 'banana', 'apple']*
```

## 示例 3:以逆序对字典值进行排序。

```
d1={'cherry':1,'apple':2,'banana':3}
sorted(d1.values(),reverse=**True**)
*#Output:[3,2,1]*
```

# 3.使用 key 参数排序

默认情况下，sorted()函数将根据键对字典进行排序。如果我们希望根据值或任何其他参数对字典进行排序，那么就要设置“key”参数。

## 示例 1:根据值对字典进行排序。

***排序(d1.items()，key=lambda x:x[1]) →*** 这里关键参数设置为 lambda 函数，取第一个索引*(值)*。字典将根据值进行排序。

```
d1={**'cherry'**:1,**'apple'**:12,**'banana'**:3}
sorted(d1.items(),key=**lambda** x:x[1])
*#Output:[('cherry', 1), ('banana', 3), ('apple', 12)]*
```

## 示例 2:根据键的长度对字典进行排序。

***排序(d1.items()，key = lambda x:len(x[0])→***这里 key 参数设置为 lambda 函数，它会取 key 的长度并排序。

```
d1={**'cherry'**:1,**'apple'**:2,**'banana'**:3} 
sorted(d1.items(),key=**lambda** x: len(x[0]))
*#Output:[('apple', 2), ('cherry', 1), ('banana', 3)]*
```

# 4.排序函数在词典理解中的应用

到目前为止，排序函数返回一个列表。如果在 dictionary comprehension 内部使用 sorted()函数，它将返回一个已排序的词典。

## 示例 1:根据关键字对字典进行排序，并返回一个排序后的字典。

***sort_keys={k:v for k，v in sorted(D1 . items())}****→*循环遍历字典中的键值对(items)并根据键进行排序，返回排序后的字典 **k:v**

```
d1={**'cherry'**:1,**'apple'**:2,**'banana'**:3}
*#by default, sorted function, will sort based on keys.* sort_keys={k:v **for** k,v **in** sorted(d1.items())}
print(sort_keys)
*#Output:{'apple': 2, 'banana': 3, 'cherry': 1}*
```

## 示例 2:根据值对字典进行排序，并返回排序后的字典

```
d1={**'cherry'**:1,**'apple'**:12,**'banana'**:3}
*#key parameter is given as second element which is dict values* sort_values={k:v **for** k,v **in** sorted(d1.items(),key=**lambda** x:x[1])}
print (sort_values)
*#Output:{'cherry': 1, 'banana': 3, 'apple': 12}*
```

# 5.使用计数器

***【most _ common()***→排序字典

如果我们创建一个计数器对象，那么***most _ common()***函数将根据值以降序对其进行排序，并返回一个元组列表。

```
**from** collections **import** Counter
d1={**'cherry'**:1,**'apple'**:12,**'banana'**:3}
print(Counter(d1))
*#Output:Counter({'apple': 12, 'banana': 3, 'cherry': 1})* c=Counter(d1)
print(c.most_common())
*#Output: [('apple', 12), ('banana', 3), ('cherry', 1)]*
```

# 结论:

在本文中，我介绍了对 python 字典进行排序的不同方法。sorted()函数将返回一个列表，但是如果我们希望返回的类型是一个字典，我们可以在字典理解中使用 sorted()函数。

感谢阅读！

*如果你喜欢阅读我更多关于 Python 和数据科学的教程，
关注我的* [***中型***](https://medium.com/@IndhumathyChelliah) ***，*** [***推特***](https://twitter.com/IndhuChelliah)

*点击这里成为中等会员:*[*https://indhumathychelliah.medium.com/membership*](https://indhumathychelliah.medium.com/membership)