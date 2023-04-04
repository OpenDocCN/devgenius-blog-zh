# Python 读取文件内容

> 原文：<https://blog.devgenius.io/python-read-file-contents-567fbcd64532?source=collection_archive---------2----------------------->

## 如何在 Python 中读取普通和大文件

![](img/cbf54496e2c448d6bd9c8f8e6c3b94d8.png)

在日常 Python 开发工作中，最常见的任务之一可能是读取文件。当您打开文件进行阅读时，有三个常用的功能:

*   `read()`:一次性读取文本的全部内容，并以字符串形式返回结果
*   `readline()`:只读取一行文本，并以字符串形式返回结果
*   `readlines()`:读取文本的所有内容，并以一个`list`的格式返回结果

例如，假设我们有一个文本文件，包含:

```
yellow
pink
black
```

以上三个函数的行为看起来会像

## 阅读()

```
with open("test.txt", "r") as f_in:
    data = f_in.read()
print(data)# Output:
yellow
pink
black
```

## 读取线()

```
with open("test.txt", "r") as f_in:
    data = f_in.readline()
print(data)# Output:
yellow
```

## 读取行()

```
with open("test.txt", "r") as f_in:
    data = f_in.readlines()
print(data)# Output:
['yellow\n', 'pink\n', 'black\n']
```

我很确定你已经使用了上面所有的函数，但是你有没有想过 Python 文件读取背后的原理？什么时候使用`read()`功能最好，什么时候使用`readlines()`最好？如果你有 **1TB** 大小的文件，还能用这些函数读取吗？你会遇到什么问题吗？

在我们深入研究如何在 Python 中处理文件之前，理解文件到底是什么以及现代操作系统如何处理它们的某些方面是很重要的。

# 什么是文件？

文件的核心是一组连续的字节，用于存储数据。这些数据以特定的格式组织，可以是简单的文本文件，也可以是复杂的二进制文件。在大多数文件系统中，文件由三个主要部分组成:

*   **文件头**:关于文件内容的元数据
*   **数据:**文件内容
*   **文件结束(EOF):** 表示文件结束的特殊字符
    该数据表示的内容取决于所使用的格式规范，通常由扩展名表示。比如 text.csv，test.txt 或者 test.txt

注意 Windows 和 Linux 系统使用字符作为行尾。以“test.txt”为例，如果它是在 Windows 系统上创建的，它看起来会像:

```
yellow\r\n
pink\r\n
black\r\nor 
yellow^M
pink^M
black^M
```

在 Linux 系统上:

```
yellow\n
pink\n
black\n
```

# 读取小文件

读取小文件(< 500MB) is pretty straightforward, below are the implementation of three methods:

## read()

```
try:
    with open("big_file.txt", "r") as f_in:
        data = f_in.read()
except IOError as e:
    print(f"Error open files, {e}")
```

## readline()

```
try:
    data = ""
    with open("big_file.txt", "r") as f_in:
        while True:
            temp_data = f_in.readline()
            if not temp_data:
                break
            data += temp_data
except IOError as e:
    print(f"Error open files, {e}")
```

## readlines()

```
try:
    with open("big_file.txt", "r") as f_in:
        data = f_in.readlines()
except IOError as e:
    print(f"Error open files, {e}")
```

# Read Large Size File

Things start getting interesting when reading large files (> 500MB)。在调用`read()`或`readlines()`函数时，你需要格外小心，因为它们会将整个文件加载到内存中，如果你不小心，你的程序就会耗尽内存。

`readline()`函数没问题，因为它每次只读取文件的一行。您可以按如下方式轻松实现它:

```
try:
    with open("read_large_files/bigfile.txt", "r") as f_in:
        while True:
            temp_data = f_in.readline()
            if "testing" in temp_data:
                print("Find word testing", temp_data)
                # Process this line
            if not temp_data:
                break
except IOError as e:
    print(f"Error open files, {e}")
```

然而，在极端的用例中，**如果整个文件只有一行**呢？即使在这种情况下使用`readline()`,您仍然会遇到内存问题。

```
Aug 31 15:29:22 ctcloud-ide kernel: Out of memory: Kill process 11463 (python) score 536 or sacrifice child
Aug 31 15:29:22 ctcloud-ide kernel: Killed process 11463 (python) total-vm:4474120kB, anon-rss:4317876kB, file-rss:716kB, shmem-rss:0kB
```

为了安全地读取大文件，我们仍然可以使用`read()`函数，但是有一个名为 size(字符数)的参数:

```
try:
    with open("read_large_files/bigfile.txt", "r") as f_in:
        while True:
            temp_data = f_in.read(4096)
            if "testing" in temp_data:
                print("Find word testing", temp_data)
                # Process this line
            if not temp_data:
                break
except IOError as e:
    print(f"Error open files, {e}")
```

在上面的代码片段中，我们将整数 4096 (4k)作为`read()`函数的参数传递，这意味着一次读取 4k 个字符，直到不再有数据为止。这样，即使整个文件内容只包含一行，您也不会遇到内存问题。

# 一个完整的例子

```
def read_in_chunks(file_object, chunk_size=1024):
    """Generator to read a file piece by piece.
    Default chunk size: 1k
    """
    try:
        while True:
            data = file_object.read(chunk_size)
            if not data:
                break
            yield data
    except IOError as e:
        print(f"Error open file, {e}")with open('test_file.txt') as f_in:
    for piece in read_in_chunks(f_in):
```

或带分隔符:

```
def read_in_chunks(file_handler, spilt: str):
    temp = ""
    while True:
        chunk = file_handler.read(200)  # Read 200 chars per time
        if not chunk:  # If no more dazta
            yield temp  # Last piece of data
            break # Remain characters + new chunk values
        temp += chunk
        while True:
            if spilt in temp:  # Check if there is a line break
                pos = temp.index(spilt)  # Get line break index
                yield temp[:pos] # Get line break left side data
                temp = temp[pos + len(spilt):]  # Get right data
            else:
                breakwith open("test_file.txt", "r") as f_in:
    for line in read_in_chunks(f_in, spilt="following"):
        print(line)
```

或者用一种更 pythonic 化的方式

```
from functools import partialwith open('test_file.txt', 'r') as f_in:
    block_read = partial(f_in.read, 1024 * 1024)
    block_iterator = iter(block_read, '') for index, block in enumerate(block_iterator, start=1):
        block = process_block(block)  # process your block data with open(f'{index}.txt', 'w') as f_out:
            f_out.write(block)
```