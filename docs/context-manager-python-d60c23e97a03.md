# 上下文管理器 Python

> 原文：<https://blog.devgenius.io/context-manager-python-d60c23e97a03?source=collection_archive---------5----------------------->

![](img/f2d47cc8aed64d78c78d5d3e8bd88b53.png)

照片由[亚伦·琼斯](https://unsplash.com/@ajonesyyyyy?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/photos/i2MYu4AElsE?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

python 上下文管理器简介

在任何编程语言中，资源处理都很重要。我们需要沿着护栏清理使用过的资源。有时我们有一个如下的清单来确保这一点。

> 打开→关闭
> 锁→释放
> 变化→复位
> 设置→拆卸
> 连接→断开
> 启动→停止

Python 提供了上下文管理器来帮助我们。

在这篇博客中，我们将了解以下内容:

*   with 语句的目的
*   为一个类/函数写一个上下文管理器。
*   异步处理上下文管理器

## 上下文管理器

上下文管理器是简化 Python 中资源管理的特性。上下文管理器定义临时运行时上下文。这些是干净简洁代码的语法糖。

## `WITH`声明

当 python 遇到 WITH 语句时:

*   它调用对象的 __enter__ 方法
*   将 __enter__ 方法的返回分配给“AS”对象
*   执行代码块
*   调用 exit 方法

```
class CustomFiletManager:
    def __init__(self, fileName, mode):
        self.fileName = fileName
        self.modeName = mode def **__enter__**(self):
        self.file = open(self.fileName, self.mode)
        return self.file    def **__exit__**(self,exc_type, exc_value, exc_traceback):
        print(exc_type, exc_val, exc_traceback)
        self.file.close()
```

`__enter__`方法指定`ContextManager`将以名称`fileName`和模式`mode`打开文件(读/写)。此方法的返回可用作 with 语句的“as”对象。

*注意:我们甚至可以使用* `*contextlib*` *库中的上下文管理器来捕获打印语句。*

如果出现异常，`__exit__`方法使用`close`方法关闭文件，并打印类型(`exc_type`)、值(`exc_value`)和回溯(`exc_traceback`)。当对象超出范围时，调用此方法。

该自定义类对象可与`with`一起使用，如下所示(由于上述 dunder 方法):

## 向方法添加上下文

Python 附带了一个名为`[contextlib](https://docs.python.org/3/library/contextlib.html)`的实用模块，它为我们提供了一个助手`@contextmanager` decorator，让我们定义基于函数的上下文管理器。

```
from contextlib import contextmanager
@contextmanager
def my_cool_function(file_name):
    try:
        f = open(file_name, 'w')
        yield f
    finally:
        f.close
```

注意:必须有 yield 语句才能使其成为生成器。

1.  在代码被*管理*之前，`yield`上面的表达式被执行。
2.  托管代码在`yield`运行。无论产生什么，都构成了上下文变量——在我们的例子中是`file object`。
3.  在托管代码运行后，执行`yield`下面的表达式。

在上面的代码中，我们没有将`yield f`放在 try…catch 块中。但这是强烈推荐的。如果在我们到达上下文管理器的退出处理程序之前发生任何错误，我们将拥有悬空对象/文件句柄/数据库连接。为了避免这种情况，我们应该将该语句放在错误处理代码下。

内置`pathlib`模块中的上下文管理器示例

```
from pathlib import path
import sys

src_path = Path(sys.argv[1])
dest_path = Path(sys.argv[1] + '.output')

try:
    with src_path.open() as src, dest_path.open(mode='x') as dest:
        for line in src:
            output = line.replace('o', '00')
            dest.write(out)
except OSError as error:
    print("A file error occured") 
```

线程模块的基本示例

```
import threading

balance_lock = threading.Lock()

with balance_lock:
    # Do updates
```

异步调用中的上下文管理器示例

```
import asyncio
import aiofiles

async def write_files():
    async with aiofiles.open(filename, 'w') as f:
        print(f"Opened file: {filename}.")
        await f.write(f"Hello from the other side")
        print(f"Finished writing to file: {filename}.")

async def main():
    await asyncio.gather(
        write_file("test1.txt"),
        write_file("test2.txt"),
    )

asyncio.run(main())
```

异步上下文管理器实现特殊的方法`__aenter__`和`__aexit__`，它们对应于常规上下文管理器中的`__enter__()`和`__exit__()`。

```
import aiohttp
import asyncio

class AsyncSession:
    def __init__(self, url):
        self._url = url

    async def __aenter__(self):
        self.session = aiohttp.ClientSession()
        response = await self.session.get(self._url)
        return response

    async def __aexit__(self, exc_type, exc_value, exc_tb):
        await self.session.close()

async def check(url):
    async with AsyncSession(url) as response:
        print(f"{url}: status -> {response.status}")
        html = await response.text()
        print(f"{url}: type -> {html[:17].strip()}")

async def main():
    await asyncio.gather(
        check("https://realpython.com"),
        check("https://pycoders.com"),
    )

asyncio.run(main())
```

带有 DB 连接的上下文管理器的另一个例子

```
import asyncio
import logging

import aiosqlite

async def main():
    logging.basicConfig(level=logging.INFO)
    async with aiosqlite.connect("application.db") as db:
        async with db.execute("SELECT * FROM blogs") as cursor:
            logging.info(await cursor.fetchall())

if __name__ == "__main__":
    asyncio.run(main())
```

上下文管理器允许我们确保在运行代码时满足前置条件和后置条件。

参考:[https://realpython.com/python-with-statement/](https://realpython.com/python-with-statement/)

编码快乐！！