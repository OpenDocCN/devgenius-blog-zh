# 在 Python 中缓存函数

> 原文：<https://blog.devgenius.io/cache-your-functions-in-python-95f8591caa07?source=collection_archive---------0----------------------->

## Python 中缓存的简单演示

![](img/f2ce8b9e5a958e0d753aaa01ce000393.png)

照片由[冯阮](https://unsplash.com/@daisy278?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/memory?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

缓存是将经常需要的数据存储在离请求者更近的地方。因此，我们提高了获取信息的速度。

举一个现实生活中的例子，你在一个大图书馆里做研究。你有一门物理相关的课程，你需要经常参考牛顿的数学原理。当你需要这本书的时候，要么你去书所在的书架，打开书，找到相关的地方，拿着你的笔记，回到你的书桌。或者，你可以把这本书拿走，放在你正在工作的桌子上，甚至可以把相关章节影印下来随身携带。第二种方式不是更合理吗？在这里，藏书楼是你放书的包或桌子。

![](img/f8a8843f326dd06ed2e7d450dd6f75cb.png)

来源:[维基百科](https://en.wikipedia.org/wiki/Philosophiæ_Naturalis_Principia_Mathematica)

缓存是一种优化技术。我们将经常使用的数据存储在可以更快更便宜地访问的内存区域。

缓存过程带来了速度，但大小有限。我们不能永远缓存数据。例如，在图书馆的例子中，我们可以放在桌子上的书的数量是有限的。

由于这个限制，在缓存过程中明智地行动是必要的。为此，已经开发了缓存策略。根据情况所需的条件，可以优选任何技术。我们来考察一些策略。

*   FIFO - First-In / First-Out:在 FIFO 逻辑中，当容量变满时，存储的第一个数据将是第一个被丢弃的数据。考虑一个存储从 A 到 g 的字母的数组，让这个数组满负荷。当 H 来的时候，我们丢弃 a，我们得到 H。如果对传入数据的访问更重要，可以使用这种策略。
*   后进先出:像堆栈数据结构一样，后进先出。如果访问最旧的数据更重要，可以使用这种类型。
*   MRU -最近使用的:任何最近使用的数据都会被清除。如果重新访问的可能性随着数据使用日期的变老而增加，则可以使用它。
*   LFU -最不常用:最不常用的数据被驱逐。保留经常访问的数据非常重要。
*   LRU -最近最少使用:最近最少使用的数据被驱逐。最近使用的数据更有可能被重新访问。

## Python 实现

让我们看看如何在 Python 中实现 **LRU 缓存**。

我们有一个简单的函数叫做 *foo* 。我放了一个 print 语句来表示这个函数在被调用时被执行。

```
def foo(x: int, y: int) -> int:
    print(f"executing foo with x: {x} y: {y}")
    return x ** y

foo(3,4) 

#executing foo with x: 3 y: 4
#Out: 81
```

现在，让我们使用 Python 的 *functools* 包中的 *lru_cache* 装饰器。如果你不知道什么是室内设计师，你可以看看下面的文章。

[](https://python.plainenglish.io/how-to-use-python-decorators-8e861f03007b) [## Python Decorators 的内部

### 用简单的例子解释装饰者

python .平原英语. io](https://python.plainenglish.io/how-to-use-python-decorators-8e861f03007b) 

总之…我把装饰器放在了 *foo* 函数的顶部。然后，反复多次调用 *foo* 函数。正如你所注意到的，它只是第一次执行这个函数。该函数被缓存。

```
from functools import lru_cache

@lru_cache
def foo(x: int, y: int) -> int:
    print(f"executing foo with x: {x} y: {y}")
    return x ** y

print(foo(3,4))
print(foo(3,4))
print(foo(3,4))
print(foo(3,4))
print(foo(3,4))
print(foo(3,4))
print(foo(3,4))

"""
executing foo with x: 3 y: 4
81
81
81
81
81
81
81

"""
```

我已经更改了传递的值。它首先用新值执行它，然后缓存它。

```
print(foo(3,4))
print(foo(3,4))

print(foo(3,5))
print(foo(3,5))

"""
executing foo with x: 3 y: 4
81
81

executing foo with x: 3 y: 5
243
243

"""
```

我们可以定义缓存的最大大小。

```
@lru_cache(maxsize=2)
def foo(x: int, y: int) -> int:
    print(f"executing foo with x: {x} y: {y}")
    return x ** y
```

现在，只有 2 个值将被缓存。并且当容量已满时，最近最少使用的值将被清除。

```
print(foo(3,4))
#executing foo with x: 3 y: 4
#81
print(foo(3,5))
#executing foo with x: 3 y: 5
#243

print(foo(3,4))
#81
print(foo(3,5))
#243
```

现在再介绍一个；

```
print(foo(2,4))
#executing foo with x: 2 y: 4
#16
```

最近最少使用的一个被驱逐: *print(foo(3，4))*

```
print(foo(3,5))
#243 <- still from cache

print(foo(3,4))
#executing foo with x: 3 y: 4 <- executed again because it was evicted before
#81
```

考虑一个非常昂贵的操作，它每次都返回与另一个用例相同的输出。可以缓存。

```
@lru_cache(maxsize=1)
def expensive_operation() -> str:
    print("$$$$$$$$$$$")
    return "I am an expensive result"

print(expensive_operation())
print(expensive_operation())
print(expensive_operation())
print(expensive_operation())
print(expensive_operation())

"""
$$$$$$$$$$$
I am an expensive result
I am an expensive result
I am an expensive result
I am an expensive result
I am an expensive result
"""
```

您可以使用 *__wrapped__* 访问缓存的原始函数

```
@lru_cache(maxsize=2)
def foo(x: int, y: int) -> int:
    print(f"executing foo with x: {x} y: {y}")
    return x ** y

print(foo(3,4))

og = foo.__wrapped__
print(og)

print(og(3,4))

"""
executing foo with x: 3 y: 4
81
<function foo at 0x7fec2874aca0>
executing foo with x: 3 y: 4
81
"""
```

一个经典的例子是斐波那契函数:

```
def calculate_fibonacci(n: int) -> int:
    print("Calculating fibonacci value for ", n)
    if n == 0 or n == 1:
        return 1
    return calculate_fibonacci(n-1) + calculate_fibonacci(n-2)

calculate_fibonacci(5)

"""
Calculating fibonacci value for  5
Calculating fibonacci value for  4
Calculating fibonacci value for  3
Calculating fibonacci value for  2
Calculating fibonacci value for  1
Calculating fibonacci value for  0
Calculating fibonacci value for  1
Calculating fibonacci value for  2
Calculating fibonacci value for  1
Calculating fibonacci value for  0
Calculating fibonacci value for  3
Calculating fibonacci value for  2
Calculating fibonacci value for  1
Calculating fibonacci value for  0
Calculating fibonacci value for  1
"""
```

一个递归函数…为了计算 n=5 的值，由于斐波纳契数列的性质，它被反复执行。

```
 @lru_cache(maxsize=16)
def calculate_fibonacci(n: int) -> int:
    print("Calculating fibonacci value for ", n)
    if n == 0 or n == 1:
        return 1
    return calculate_fibonacci(n-1) + calculate_fibonacci(n-2)

calculate_fibonacci(5)

"""
Calculating fibonacci value for  5
Calculating fibonacci value for  4
Calculating fibonacci value for  3
Calculating fibonacci value for  2
Calculating fibonacci value for  1
Calculating fibonacci value for  0
"""
```

让我们比较一个大数字的计算时间:

```
start = time.time()
calculate_fibonacci(35)
print(time.time()-start)

"""
without caching: 31.114999055862427 
with caching: 0.00023484230041503906
"""
```

最后，让我们创建一个自定义函数，为 API 请求缓存 JSON 格式的数据。

```
import json
import requests

def fetch_api_data(url: str, json_path: str, update_cache: bool = False):
    """
    url: request url address
    json_path: the path of the json file
    update_cache: a boolean for update operation
    """
    if update_cache:
        #if we are updating, current will be none
        #so it will create a new json
        cached_data = None
    else:
        try:
            with open(json_path, 'r') as file:
                cached_data = json.load(file)
                print("Data has been collected from local cache!\n")
        except(FileNotFoundError, json.JSONDecodeError) as e:
            print(f"Some error occured when the JSON file is being read: {e}\n")
            cached_data = None

    #if there is not a cached data available
    if not cached_data:
        #fetch request data
        print("getting new data from url\n")
        cached_data = requests.get(url).json()
        with open(json_path, 'w') as file:
            print("Creating a new cache JSON file\n")
            json.dump(cached_data, file)

    return cached_data

url = "https://dummyjson.com/comments"
json_path = "cachefile.json"
data = fetch_api_data(url, json_path)
print(data)

"""
Some error occured when the JSON file is being read: [Errno 2] No such file or directory: 'cachefile.json'

getting new data from url

Creating a new cache JSON file

{'comments': [{'id': 1, 'body': 'This is some awesome thinking!', .......
"""

#SECOND CALL
data = fetch_api_data(url, json_path)
print(data)
"""
Data has been collected from local cache!

{'comments': [{'id': 1, 'body': 'This ........
"""
```

如果有一个存储缓存数据的 JSON 文件，该函数将读取该文件并返回数据。如果 JSON 文件不存在，或者被请求更新，或者读取时出现错误，它会通过从 URL 请求数据来收集数据，并将其写入新的 JSON 文件。

因此，缓存是加速数据检索的一种重要的优化方法。多亏了缓存，我们可以实现更快更高效的应用。

目前就这些。感谢阅读。

## 阅读更多内容…

[](https://python.plainenglish.io/how-to-use-python-decorators-8e861f03007b) [## Python Decorators 的内部

### 用简单的例子解释装饰者

python .平原英语. io](https://python.plainenglish.io/how-to-use-python-decorators-8e861f03007b) [](https://towardsdev.com/keep-loggin-loggin-loggin-loggin-in-python-f7c0c3088795) [## 继续登录，登录，登录，用 Python 登录

### 快速发现 Python 中的日志记录

towardsdev.com](https://towardsdev.com/keep-loggin-loggin-loggin-loggin-in-python-f7c0c3088795) [](https://python.plainenglish.io/test-driven-development-in-python-49fa22cb95d4) [## Python 中的测试驱动开发

### 关于 TDD 和 Python 实现的细节

python .平原英语. io](https://python.plainenglish.io/test-driven-development-in-python-49fa22cb95d4) [](https://levelup.gitconnected.com/unit-test-with-pytest-d6f53919a19a) [## 使用 PyTest 的单元测试

### 使用 PyTest 介绍 Python 中的单元测试

levelup.gitconnected.com](https://levelup.gitconnected.com/unit-test-with-pytest-d6f53919a19a) 

## 来源

[https://realpython.com/lru-cache-python/](https://realpython.com/lru-cache-python/)

[https://www.mathsisfun.com/numbers/fibonacci-sequence.html](https://www.mathsisfun.com/numbers/fibonacci-sequence.html)

[https://www . fortinet . com/resources/cyber glossary/what-is-caching](https://www.fortinet.com/resources/cyberglossary/what-is-caching)

[https://docs.python.org/3/library/functools.html](https://docs.python.org/3/library/functools.html)

https://www.youtube.com/watch?v=K0Q5twtYxWY

https://www.youtube.com/watch?v=0_ievUF0L_E[t = 27s](https://www.youtube.com/watch?v=0_ievUF0L_E&t=27s)

[https://www.youtube.com/watch?v=kErCTr9dwlk](https://www.youtube.com/watch?v=kErCTr9dwlk)