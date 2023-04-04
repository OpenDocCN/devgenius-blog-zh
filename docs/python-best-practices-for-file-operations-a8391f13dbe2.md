# Python 文件操作的最佳实践

> 原文：<https://blog.devgenius.io/python-best-practices-for-file-operations-a8391f13dbe2?source=collection_archive---------1----------------------->

## 日常一点 Python 知识！

![](img/5fe89d57f63a4d529747f712d550c8cc.png)

照片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

作为一名 Python 开发人员，我们每天使用 Python 完成不同的任务。文件操作是最常见的任务之一。使用 Python，您可以轻松地为他人生成漂亮的报告，并且只需几行代码就可以快速解析和组织数万个数据文件。

当我们编写与文件相关的代码时，我们通常会关注这些事情:我的代码够快吗？我的代码是否事半功倍？在本文中，我将向您推荐一个被低估的 Python 标准库模块，演示读取大文件的最佳方式，最后分享我对函数设计的一些想法。

# 使用 pathlib 模块

如果你需要用 Python 做文件处理，标准库中的`os`和`os.path`模块一定是你无法回避的两个模块。在这两个模块中，有很多与文件路径处理、文件读写、文件状态查看相关的工具功能。

让我用一个例子来说明它们是如何使用的。假设一个目录下安装了很多数据文件，但是后缀不统一，那么既有`.csv`又有`.txt`文件。我们需要把所有的`.txt`文件都改成`.csv`后缀。

您可以像这样快速编写一个函数:

```
import os
import os.path

def unify_ext_with_os_path(path):
    for filename in os.listdir(path):
        basename, ext = os.path.splitext(filename)
        if ext == '.txt':
            abs_filepath = os.path.join(path, filename)
            os.rename(abs_filepath, os.path.join(path, f'{basename}.csv'))
```

让我们看看上面的代码中使用了哪些与文件处理相关的函数:

*   `os.listdir(path)`:列出路径目录下的所有文件
*   `os.path.splitext(filename)`:分割文件名的基本名和后缀部分
*   `os.path.join(path, filename)`:需要操作的组合文件名是绝对路径
*   `os.rename(...)`:重命名文件

虽然上面的函数可以完成需求，但是说实话，即使写了很多年的 Python 代码，我还是觉得这些函数不仅难记，而且最终的产品代码也不是很讨喜。

## 使用 pathlib 模块

为了使文件处理更容易，Python 在 3.4 版本中引入了一个新的标准库模块:`[pathlib](https://docs.python.org/3/library/pathlib.html)`。它基于面向对象的思想设计，封装了大量与文件操作相关的功能。如果用它重写上面的代码，结果会大不一样。

使用`pathlib`模块后的代码:

```
from pathlib import Path

def unify_ext_with_pathlib(path):
    for fpath in Path(path).glob('*.txt'):
        fpath.rename(fpath.with_suffix('.csv'))
```

与旧代码相比，新功能只需要两行代码就可以完成工作。这两行代码主要做以下事情:

1.  首先使用`Path(path)`将字符串路径转换成一个对象
2.  调用`.glob(‘*.txt’)`对路径下的所有东西进行模式匹配，并将其作为生成器返回，结果仍然是一个`Path`对象，所以我们可以继续做下面的操作
3.  使用`.with_suffix(‘.csv’)`直接获取带有新后缀的文件的完整路径
4.  呼叫`.rename(target)`

与`os`和`os.path`相比，`pathlib`模块引入后的代码明显更简单，整体感更强。所有与文件相关的操作都是一站式完成的。

除此之外，`pathlib`模块还提供了许多有趣的用途。例如，使用`/`操作符来组合文件路径:

```
>>> import os.path
>>> os.path.join('/tmp', 'foo.txt')
'/tmp/foo.txt'

>>> from pathlib import Path
>>> Path('/tmp') / 'foo.txt'
PosixPath('/tmp/foo.txt')
```

或者使用`.read_text()`快速读取文件内容:

```
>>> with open('foo.txt') as file:
...     print(file.read())
...
foo

>>> from pathlib import Path
>>> print(Path('foo.txt').read_text())
foo
```

[PEP-519](https://www.python.org/dev/peps/pep-0519/) 专门为“文件路径”定义了一个新的对象协议，这意味着从 PEP 生效时起，从 Python 3.6 开始，`pathlib`中的路径对象可以与大多数只接受字符串路径的标准库函数兼容使用:

```
>>> p = Path('/tmp')
>>> os.path.join(p, 'foo.txt')
'/tmp/foo.txt'
```

# 流式传输大文件

几乎所有人都知道 Python 中有一个读取文件的“标准做法”:首先使用一个`with open(fine_name)`上下文管理器获取一个 file 对象，然后`for`用一个循环迭代它，逐行获取文件的内容。例如:

```
def count_nine(fname):
    count = 0
    with open(fname) as file:
        for line in file:
            count += line.count('9')
    return count
```

为什么这种读取文件的方式会成为标准？这是因为它有两个好处:

1.  `with`:上下文管理器自动关闭打开的文件描述符
2.  当迭代 file 对象时，内容是逐行返回的，不会占用太多内存

但是这种标准做法也不是没有缺点。**如果正在读取的文件根本不包含任何换行符，那么上面的第二个好处就不成立**。当代码被`for line in file`执行时，line 会变成一个非常大的 string 对象，消耗非常可观的内存量。

## **成块读取**

为了解决这个问题，我们需要暂时把这个“标准实践”放在一边，使用一个更低级的`file.read()`方法。每个调用`file.read(chunk_size)`将直接返回从当前位置读取的大小的`chunk_size`文件内容，而不是在一个循环中迭代文件对象，而不需要等待任何换行符出现。

因此，新的代码片段将如下所示:

```
def count_nine_v2(fname):
    """Count total 9s，read 8kb each time
    """
    count = 0
    block_size = 1024 * 8
    with open(fname) as fp:
        while True:
            chunk = fp.read(block_size)
            # If no more content
            if not chunk:
                break
            count += chunk.count('9')
    return count
```

在新函数中，我们使用了一个`while`循环来读取文件的内容，每次读取的最大大小为 8kb，这样可以避免之前拼接一个巨大字符串的过程，并且减少了很多内存的使用。

## 将代码与发电机解耦

假设我们说的不是 Python，而是其他编程语言。那么可以说上面的代码已经不错了。但如果分析一下`count_nine_v2`函数，会发现循环体内部有两个独立的逻辑:数据生成(读调用和组块判断)和数据消耗。而这两个独立的逻辑是耦合在一起的。

为了提高复用性，我们可以定义一个新的`chunked_file_reader`生成器函数，负责所有与“数据生成”相关的逻辑。这样里面的主循环`count_nine_v3`只需要负责计数。

```
def chunked_file_reader(fp, block_size=1024 * 8):
    """generator：Read file in chunks
    """
    while True:
        chunk = fp.read(block_size)
        # If no more content
        if not chunk:
            break
        yield chunk

def count_nine_v3(fname):
    count = 0
    with open(fname) as fp:
        for chunk in chunked_file_reader(fp):
            count += chunk.count('9')
    return count
```

在这一点上，代码似乎没有优化的余地，但事实并非如此。`iter(iterable)`是一个用于构造迭代器的内置函数，但它还有一个鲜为人知的用法。

当我们用`iter(callable, sentinel)`方法调用它时，它会返回一个特殊的对象，它会继续生成可调用对象 callable 的可调用结果，直到结果为 sentinel，迭代终止。

```
def chunked_file_reader(file, block_size=1024 * 8):
    """Generator：Use iter to read file in chunks
    """
    # Use partial(fp.read, block_size) to construct a new func
    # Read and return fp.read(block_size) until ''
    for chunk in iter(partial(file.read, block_size), ''):
        yield chunk
```

最后，只用两行代码，我们就完成了一个可重用的分块文件读取功能。

# 文件对象的设计

假设我们要统计每个文件中出现了多少个英文元音(aeiou)。只需对之前的代码做一些调整，新的函数就可以立刻编写出来`count_vowels`。

```
def count_vowels(filename):
    """count (aeiou)s
    """
    VOWELS_LETTERS = {'a', 'e', 'i', 'o', 'u'}
    count = 0
    with open(filename, 'r') as fp:
        for line in fp:
            for char in line:
                if char.lower() in VOWELS_LETTERS:
                    count += 1
    return count

# OUTPUT: 16
print(count_vowels('sample_file.txt'))
```

为了保证程序的正确性，我们需要为它编写一些单元测试。但是当我准备写一个测试的时候，发现很麻烦。主要问题如下:

1.  该函数接受文件路径作为参数，所以我们需要传递一个实际的文件
2.  为了准备测试用例，我们需要提供一些样板文件或者编写一些临时文件
3.  文件能否正常打开和读取已经成为我们需要测试的一个边界条件

**一般来说，如果你发现你的功能难以编写单元测试，那通常意味着你应该改进它的设计。**以上功能应该如何改进？答案是:*让函数依赖于“文件对象”而不是文件路径*。

让我们来试试:

```
def count_vowels_v2(fp):
    """Count (aeiou)s
    """
    VOWELS_LETTERS = {'a', 'e', 'i', 'o', 'u'}
    count = 0
    for line in fp:
        for char in line:
            if char.lower() in VOWELS_LETTERS:
                count += 1
    return count

with open('small_file.txt') as fp:
    print(count_vowels_v2(fp))
```

主要变化是提高了函数的适用性。因为 Python 是“鸭式”的，虽然函数需要接受一个 file 对象，但我们实际上可以将任何实现 file 协议的“类文件对象”传入`count_vowels_v2`函数。

而 Python 有很多“类文件对象”。例如，`io`模块中的 StringIO 对象就是其中之一。它是一个特殊的基于内存的对象，与文件对象具有几乎相同的接口设计。

使用 StringIO，我们可以非常方便地为函数编写单元测试。

```
import pytest
from io import StringIO

@pytest.mark.parametrize(
    "content,vowels_count", [
        # Define test cases
        # (content, expected_output)
        ('', 0),
        ('Hello World!', 3),
        ('HELLO WORLD!', 3),
        ('你好，世界', 0),
    ]
)
def test_count_vowels_v2(content, vowels_count):
    # Use StringIO to construct "file"
    file = StringIO(content)
    assert count_vowels_v2(file) == vowels_count
```

将函数参数改为“文件对象”的最大好处是提高了函数的适用性和可组合性。

通过依赖更抽象的“类似文件的对象”而不是文件路径，它为如何使用函数提供了更多的可能性。StringIO、PIPE 和任何其他满足协议的对象都可以是函数的客户端。

# 结论

文件操作是我们日常工作中经常需要接触的领域。使用更方便的模块，使用生成器节省内存，编写更适用的函数，可以让我们写出更高效的代码。