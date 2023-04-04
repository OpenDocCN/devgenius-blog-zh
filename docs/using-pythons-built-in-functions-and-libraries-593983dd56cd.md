# 使用 Python 的内置函数和库

> 原文：<https://blog.devgenius.io/using-pythons-built-in-functions-and-libraries-593983dd56cd?source=collection_archive---------14----------------------->

![](img/e5f498f0cae2f02b4fb1e1b5e79f9fea.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 拍摄的照片

我已经用 Python 编码一年多了，我一直在寻找更多的 Python 代码。我想分享一些我学到的有用的函数和库，并开始定期使用它们来编写简洁的 Pythonic 代码。我试图保持文章简短，但提供了更多信息的额外链接。

# 预赛

你可以在 https://www.python.org/dev/peps/pep-0008/[找到有用的风格指南和约定，比如命名约定，但是在这篇文章中，我想把重点放在编写更干净、更快速的代码上。](https://www.python.org/dev/peps/pep-0008/)

编写更多 Pythonic 代码的最典型方式可能是使用列表或字典理解，而不是 for 循环。例如，假设我们想列出前五个 2 的倍数。我们可以做

```
multiples = []
for i in range(1, 6):
    multiples.append(i * 2)
```

但是在 Python 中，有一个使用列表理解的更加优雅和快速的版本

```
multiples = [i * 2 for i in range(1, 6)]
```

类似地，假设我们有一个元组列表，每个元组包含一个人的姓名和年龄

```
data = [('Alice', 20), ('Bob', 30), ('Charles', 40)]
```

我们想把它转换成一个字典，名字作为键，年龄作为值。在 Python 中，我们可以通过字典理解简洁地做到这一点

```
data_dict = {name: age for name, age in data}
```

# Python 的内置函数

让我们先看一些 Python 内置函数的例子。

## zip()

让我们稍微改变一下上面的例子，试着从*数据*中分离出姓名和年龄到一个字典中:

```
data = [('Alice', 20), ('Bob', 30), ('Charles', 40)]
# turn into {'names': ['Alice', 'Bob', 'Charles'], 'ages': [20, 30, 40]}
```

要做到这一点，我们可以做到

```
names, ages = zip(*data)
new_data = {'names': list(names), 'ages': list(ages)}
```

它甚至可以在一行中完成

```
new_data = {k: list(v) for k, v in zip(('names', 'ages'), zip(*data))}
```

尽管在这种情况下可读性差，速度也慢。

那么这是如何工作的呢？*将*数据*中的 3 个元组解包成 3 个独立的参数，传递给 Python 的内置 **zip** 函数。 *zip* 函数获取 iterables 并将它们聚集在一个元组中。下面是一个稍微简单一点的使用 zip()的例子:

```
a = [1, 2, 3]
b = ['a', 'b', 'c']
c = list(zip(a, b))  # gives [(1, 'a'), (2, 'b'), (3, 'c')]
```

当然，我们还利用了 **list** 内置函数，该函数将 *zip* 返回的元组转换为列表。

## 任意()和全部()

假设我们有一个整数列表，想要检查其中是否有偶数。我们可以使用 for 循环，因为我们已经知道列表理解会更快

```
integers = [1, 5, 7, 9, 11]  # some list of integers
if [i for i in integers if i % 2 == 0]:
    # do some stuff
```

如果列表中有内容，它评估为*真*，如果列表为空(即没有偶数)，那么它评估为*假*。另一种选择是使用内置的 **any** 函数，该函数接受一个 iterable，如果至少有一个元素为 *True* ，则返回 *True*

```
integers = [1, 5, 7, 9, 11]
if any(i % 2 == 0 for i in integers):
    # do some stuff
```

这种方法更有效，因为我们不需要创建一个新的列表来检查它是否非空，而且可读性更好。

如果我们想检查所有的整数是否都是奇数，我们可以使用 **all** 函数

```
if all(i % 2 == 1 for i in integers):
    # do some stuff
```

只有当可迭代的所有元素都为*真*时，它才返回*真*。

## 地图()

假设我们有一个整数列表，我们想列出这些整数的平方。例如*【1，2，3，4】*到*【1，4，9，16】*。最简单的方法是

```
integers = [1, 2, 3, 4]
squares = [i ** 2 for i in integers]
```

我们也可以使用 Python 的 **map** 函数，它接受一个 iterable 和一个应用于 iterable 的每个元素的函数。

```
squares = list(map(lambda x: x ** 2, integers))
```

这里我们使用 Python 的 **lambda** 函数，它允许我们在一行中定义一个函数，而不是:

```
def square(x):
    return x ** 2

squares = list(map(square, integers))
```

注意使用 *map* 函数的好处是保持它作为一个迭代器。在这里，我将它包装在 *list* 函数中，这样我们可以很容易地看到方块列表，但是最好使用 *map* 来迭代方块。例如，我们可以使用内置的**求和**函数对它们求和

```
squares_sum = sum(map(lambda x: x ** 2, integers))  # gives 30
```

这样做的好处是，我们不必创建一个新的列表对象来迭代哪一个更节省内存，速度更快。

## 已排序()

回到这个例子，

```
data = [('Alice', 20), ('Bob', 30), ('Charles', 40)]
```

假设我们想按照人们的年龄降序排列元组列表。我们可以用 Python 的**排序**函数做到这一点:

```
sorted_data = sorted(data, key=lambda x: x[1], reverse=True)
```

我们再次看到使用了 *lambda* 函数，它在这里选择每个元组中的第 2 个元素。反向标志用于按降序排序。

内置函数的完整列表可以在 https://docs.python.org/3/library/functions.html[的文档中找到。现在让我们看看一些有用的库！](https://docs.python.org/3/library/functions.html)

# Python 库

## 收集

这个库增加了一些更有用的容器数据结构。我用得最多的是 **defaultdict** 。假设我们有一个包含姓名和 t 恤尺寸的元组列表:

```
data = [('Alice', 'S'), ('Bob', 'L'), ('Charles', 'M'), ('David', 'L'), ...]
```

我们希望将这些名字按大小分组放入字典中。我们通常必须检查每个键是否已经存在于字典中，以避免键错误:

```
names_by_size = {}
for name, size in data:
    if size in names_by_size:
        names_by_size[size].append(name)
    else:
        names_by_size[size] = [name]
```

使用 *defaultdict* 我们可以指定值的数据结构，并忽略键检查

```
from collections import defaultdictnames_by_size = defaultdict(list)
for name, size in data:
    names_by_size[size].append(name)# gives: defaultdict(<class 'list'>, {'S': ['Alice'], 'L': ['Bob', 'David'], 'M': ['Charles']})
```

我们甚至可以在*默认字典*中包含一个*默认字典*，并如下使用它

```
some_dict = defaultdict(lambda: defaultdict(list))
some_dict[key_1][key_2].append(value)
```

对于某些 *key_1，key_2，value* 。

**计数器**是另一种有用的数据类型。假设我们想找出一个字母在列表中出现的次数。我们可以做

```
from collections import Counterletters = ['a', 'b', 'c', 'a', 'b']
letters_count = Counter(letters)  # gives  Counter({'a': 2, 'b': 2, 'c': 1})
```

要从*计数器*中取回所有信件，我们可以这样做

```
letters_count.elements()
```

并获得从最常见到最少的元素的排序列表

```
letters_count.most_common()
```

其他容器类型包括 **namedtuple** 、**dequee**、 **OrderedDict** 以及一个我在撰写本文时才发现的容器 **ChainMap** 。*链式映射*对于存储多个字典很有用，例如，一次从字典中获取所有键或所有值。详见[https://www.geeksforgeeks.org/chainmap-in-python/](https://www.geeksforgeeks.org/chainmap-in-python/)。然而，如果我们只有两个字典 *a* 和 *b* ，我们可以使用

```
combined_dict = {**a, **b}
```

**的行为类似于*，但是它可以解包键和值。

## itertools

正如文档中所描述的，这个库具有“创建迭代器以实现高效循环的函数”。我们来举几个例子。

假设我们有一个分数列表，每个分数对应于一个不同的人，我们想找出有多少种方法可以将两个人配对在一起，使他们的总分数大于 10。我们可以利用 itertools **组合**函数:

```
from itertools import combinationsscores = [2, 4, 3, 6, 4, 5, 7]
matches = 0
for score_1, score_2 in combinations(scores, 2):
    if score_1 + score_2 > 10:
        matches += 1
```

或者使用列表理解和内置的 **sum** 函数以更简洁、更快速的方式实现

```
matches = sum([1 for score_1, score_2 in combinations(scores, 2) if score_1 + score_2 > 10])
```

*组合*函数查看 r 个唯一分数的所有组合(按位置而非值)，其中 r 是函数的第二个参数(在我们的示例中为 2)。例如

```
# list(combinations('ABCD', 2)) -> [('A', 'B'), ('A', 'C'), ('A', 'D'), ('B', 'C'), ('B', 'D'), ('C', 'D')]# list(combinations('ABCD', 3)) -> [('A', 'B', 'C'), ('A', 'B', 'D'), ('A', 'C', 'D'), ('B', 'C', 'D')]
```

如果我们希望单个元素重复多次，我们可以使用**combinations _ with _ replacement**函数，如果我们不在乎顺序，我们也可以使用 **permutations** 函数。

另一个有用的功能是**链条**。它接受 iterables 并返回一个迭代器，该迭代器依次返回每个 iterable 中的元素。例如

```
from itertools import chain
# chain('ABC', 'DEF') -> A B C D E F
```

如果我们有一个可迭代的列表，我们可以用*来解包它们，或者我们可以使用 chain.from_iterable 函数来代替

```
# chain.from_iterable(['ABC', 'DEF']) -> A B C D E F
```

如果我们有一个字典列表，并希望将它们合并成一个字典，这将非常有用。

```
my_dicts = [{1: 2, 2: 3, 4: 5}, {6: 7, 8: 9, 4: 11}]
combined = dict(chain.from_iterable(d.items() for d in my_dicts))
```

与*链图*(前面提到过)不同，如果有重复的键，将使用后面的值。在最终的字典中，键 4 的值为 11。

看看这篇非常详细的文章[https://realpython.com/python-itertools/](https://realpython.com/python-itertools/)来了解更多关于 itertools 的信息。最重要的是这些函数返回的是迭代器而不是列表，这使得内存效率更高，速度更快。所以不要在他们身边放一个 *list()* 除非你想看看他们回馈的是什么！

## 更多 _itertools

该库是 itertools 库的扩展，需要与 pip(*pip install more-ITER tools*)一起安装。

如果我们想把一个列表分割成大小相等的块，我们可以使用**分块**函数

```
from more_itertools import chunkedmy_list = [1, 2, 3, 4, 5, 6, 7, 8]
chunks = list(chunked(my_list, 3))  # gives [[1, 2, 3], [4, 5, 6], [7, 8]]
```

另一个有用的功能是**可窥视**。当我们用迭代器包装一个迭代器时，它允许我们查看迭代器的下一个元素，而不用检索它。例如

```
from more_itertools import peekablep = peekable([1, 2])
p.peek()  # gives 1 without retrieving it from the iterator
next(p)  # gives 1 and retrieves it from the iterator so we are left with peekable([2])
```

如果我们只想在适当的时候检索元素，这是很有用的。我们可以传入一个默认值来返回，而不是在迭代器用尽时引发 **StopIteration** (意味着所有元素都已被检索)。例如

```
p = peekable([])
p.peek()  # Would raise StopIteration
p.peek('iterator is empty')  # returns 'iterator is empty' 
```

在 https://more-itertools.readthedocs.io/en/stable/api.html[你可以找到更多工具中的更多功能。](https://more-itertools.readthedocs.io/en/stable/api.html)

# 结论

如果你做到了这一步，感谢你的阅读，并希望你学到了一些东西或刷新了你的记忆。这篇文章只是触及了 Python 的可能性的表面，但即使只是使用这里提到的函数和库，它也会大大改进您的代码。如果你找到了一个替代的/更好的方法，或者有一些你经常使用的其他函数/库，我没有包括在内，那么请留下评论，这样我们都可以从中受益！