# 如何用 Python 发送并发 HTTP 请求

> 原文：<https://blog.devgenius.io/how-to-send-concurrent-http-requests-in-python-d9cda284c86a?source=collection_archive---------0----------------------->

![](img/3199750026e6fd97f16294dadced06c0.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

根据一些消息来源，2020 年互联网上的数据总量将达到 40 兆字节。一个齐塔字节大约是一万亿吉字节。你不得不承认那是相当多的。

检索这些数据并利用它们做一些有用的事情的最好方法是发送一个 HTTP 请求。然而，单个请求可能无法获得大量数据。因此，检索数据的实际最佳方式是发送大量 HTTP 请求。但是如果同步发送，发送大量请求会花费相当多的时间，也就是说，如果在发送下一个请求之前等待一个请求完成。解决这个问题的最好方法是利用并发性。

Python 非常适合处理数据。有大量有用的库，它是数据科学的行业标准，也是数据工程的首选语言之一。

在本文中，我将描述发送并发请求的两种不同方法。

1.  **内置并发库**

从技术上讲，Python 是一种多线程语言，然而，由于实践中的 GIL(全局解释器锁)，它实际上并不是。因此，Python 中的线程与并发性的关系比与并行性的关系更大。

并发库有一个名为 ThreadPoolExecutor 的类，我们将使用它来发送并发请求。对于这个例子，我使用的是 Rick 和 Morty API。我们的目标是获得瑞克和莫蒂漫画中各种角色的信息，API 是一个很好的起点。我们来看一些代码，我会逐行解释:

```
import requestsimport concurrentfrom concurrent.futures import ThreadPoolExecutorcharacters = range(1, 100)base_url = 'https://rickandmortyapi.com/api/character'threads = 20def get_character_info(character): r = requests.get(f'{base_url}/{character}') return r.json()with ThreadPoolExecutor(max_workers=threads) as executor: future_to_url = {executor.submit(get_character_info, char) for char in characters} for future in concurrent.futures.as_completed(future_to_url): try: data = future.result() print(data) except Exception as e: print('Looks like something went wrong:', e)
```

第 1–3 行是我们需要的导入库。我们将使用 [**请求**](https://docs.python-requests.org/en/latest/) 库向 API 发送 HTTP 请求，我们将使用 [**并发**](https://docs.python.org/3/library/concurrent.futures.html) 库并发执行它们。

characters 变量是一个从 1 到 99 的整数范围(注意，我使用的是 range 而不是 list，因为这样变量被延迟加载到内存中，这意味着它在内存方面更有效)。

*base_url* 是我们将调用的端点以及字符 id 后缀来获取我们的数据。

*threads* 变量基本上告诉我们的 *ThreadPoolExecutor* 我们想要产生最多 20 个线程(但不是我所说的真正的 OS 线程)。第 13–20 行进行实际的执行。

*future_to_url* 变量是一个字典，它有一个有趣的键值对。关键是一个方法— *executor.submit* 。更有趣的是，该方法接受两个参数。一个是函数名( *get_character_info* )，另一个是传递给那个函数的参数。确保不要混淆这一点，通过在括号中添加 *char* 参数，就像调用 *get_character_info* 函数本身一样。字典的值基本上是一个元组理解，这是我们通常不会用作字典值的东西。这里的要点是迭代所有的角色 id，并对每个角色 id 进行函数调用。

接下来，我们启动一个 for 循环，该循环将遍历*concurrent . futures . as _ completed(future _ to _ URL)*，简单来说，这意味着——将这些调用的结果作为结束。

try/except 块将数据变量声明为 HTTP 请求的结果，希望不会失败。如果是这样，我们将打印一个简单的错误消息，看看哪里出错了。

如果您运行了这段代码，您可能已经看到了它的执行速度。我们在不到一秒钟的时间里获得了 100 个 API 结果。如果我们一个接一个地做这件事，可能要花一分多钟。

1.  **Asyncio 库**

[**Asyncio**](https://docs.python.org/3/library/asyncio.html) 模块也是内置的，但是为了配合 HTTP 调用使用，我们需要安装一个异步 HTTP 库，名为 [**aiohttp**](https://docs.aiohttp.org/en/stable/) 。原因是我们之前使用的请求库不能异步工作，所以它在这里没有任何作用。

Asyncio 的工作方式与 ThreadPoolExecutor 不同，它使用一种叫做*事件循环*的东西。这类似于 NodeJs 的工作方式，所以如果您来自 JavaScript，您可能对这种方法很熟悉。

代码如下:

```
import aiohttpimport asynciocharacters = range(1,100)base_url = 'https://rickandmortyapi.com/api/character'async def get_character_info(character, session): r = await session.request('GET', url=f'{base_url}/{character}') data = await r.json() return dataasync def main(characters): async with aiohttp.ClientSession() as session: tasks = [] for char in characters: tasks.append(get_character_info(character=char, session=session)) results = await asyncio.gather(*tasks, return_exceptions=True) return resultsif __name__ == '__main__': data = asyncio.run(main(characters)) for item in data: print(item)
```

前 10 行代码与 ThreadPoolExecutor 方法相对相似，有两个主要区别。首先，我们正在导入 **aiohttp** 库，而不是**请求**。第二，在我们的函数定义中，我们在所有东西前面使用了 *async* 关键字。这样我们就告诉 Python 解释器，我们将在一个事件循环中运行这个函数。

第 12-18 行是与第一种方法不同的地方，但是正如您可能得出的结论，最重要的调用是 *tasks.append* 调用，它类似于第一种方法中的*executor . submit*call*。下一行中的 *asyncio.gather* 类似于 *futures.as_completed* 方法，因为它在一个集合中收集并发调用的结果。*

*最后，当使用 asyncio 时，我们需要调用 *asyncio.run()* (只有 Python 3.7 及更高版本才有，否则需要多几行代码)。这个函数接受一个参数，这个参数是我们想要添加到事件循环中的异步函数。*

*这种方法可能有点复杂，但它更快、更可靠。一般来说，我会更多地推荐它，尤其是如果您正在进行数百甚至数千个并发调用的话。*

*最终，这两种方法中的任何一种都将在同步调用 HTTP 调用所需时间的一小部分内完成 HTTP 调用。*

*本文到此为止。一如既往，如果你喜欢，请尽情鼓掌:)*

*如果我错过了什么，请留下评论。*

*感谢阅读！*