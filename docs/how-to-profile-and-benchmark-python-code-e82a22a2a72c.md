# 如何评测 Python 代码

> 原文：<https://blog.devgenius.io/how-to-profile-and-benchmark-python-code-e82a22a2a72c?source=collection_archive---------6----------------------->

![](img/e9ca2da90e9b9a01c958de2f16661350.png)

假设我们有两个功能:

```
def multiples_of(x: int, max: int) -> list[int]:
    """
    Iteratively Find all the multiples of `x` between 0 and `max`
    i.e 0 < M <= max.

    :return list[int]: all multiples
    """
    v = max // x
    return [(i + 1) * x for i in range(v)]
```

和

```
def gen_multiples_of(x: int, max:int) -> Generator:
    """
    Find all the multiples of `x` between 0 and `max`
    i.e 0 < M <= max.

    :return Generator: Can be iterated over to return a list of multiples
    """
    v = max // x
    i = 0
    multiple = 0
    while (multiple <= max) and (i < v):
        multiple = (i + 1) * x
        i += 1
        yield multiple
```

他们都做同样的事情。我们如何判断哪个函数优化得更好，效率更高？我们应该在项目中使用哪个？

> 在这种情况下，效率意味着用最少的时间做大量的工作。我们不一定会考虑计算成本。在现实世界中，选择任何功能都需要更多的上下文(可维护性、可用资源等)。)

# Timeit 模块

开始概要分析的最简单方法之一是， **timeit** 模块是 Python 标准库的一部分。

技术上，`**timeit**` **是**用于标杆代码。从 [rbspy 文档](https://rbspy.github.io/)中，

> **基准测试告诉你你的代码有多慢(“做 X Y Z 花了 20 秒”)，剖析器告诉你为什么慢(“35%的时间花在压缩上”)**

下面是我们如何使用它来测试我们的功能:

*   我们将首先创建一个 main/runner 函数，这样我们的生成器也会得到评估

```
def run(f):
    # f will be one of our functions
    for i in f(x=3, max=1000):
        pass

# Example Usage
lambda: run(multiples_of)
lambda: run(gen_multiples_of)
```

*   导入`**timeit**`包并使用它的`**timeit**`函数来测量代码执行时间

```
import timeit 

n = int(1500)
r = timeit.timeit(lambda: run(multiples_of), number=n)
print(f"Iterative Function:, Total Time (for {n} cycles): {r}s, Average time (per cycle) {r/n}s")

r = timeit.timeit(lambda: run(gen_multiples_of), number=n)
print(f"Generator Function:, Total Time (for {n} cycles): {r}s, Average time (per cycle) {r/n}s")
```

样本输出:

```
Iterative Function:, Total Time (for 1500 cycles): 0.15279039996676147s, Average time (per cycle) 0.00010186026664450765s
Generator Function:, Total Time (for 1500 cycles): 0.29554930003359914s, Average time (per cycle) 0.0001970328666890661s
```

看来迭代函数比生成函数快。这可能是因为列表理解和 while 循环中的常量求值吗？我们会和另一个侧写员一起深入调查。

来自`[Time.timeit()](https://docs.python.org/3/library/timeit.html#timeit.Timer.timeit)` [文档](https://docs.python.org/3/library/timeit.html#timeit.Timer.timeit):

> 时间*数量*主语句的执行次数。这将执行 setup 语句一次，然后**返回多次执行 main 语句所花费的时间，以秒为单位，以浮点形式计算**。参数是循环的次数，默认为一百万次。

关于`**timeit**`模块的注意事项:

*   在执行测试时，垃圾收集每次都被 time()函数禁用。
*   `**timeit()**`根据您正在使用的操作系统，内部采用准确的时间。例如，它将为 Windows 操作系统使用 time.clock()，为 mac 和 Linux 使用 time.time()。
*   `**timeit**`还公开了一个[命令行接口](https://docs.python.org/3/library/timeit.html#command-line-interface)。

# cProfile 模块

来自官方文件:

> `[cProfile](https://docs.python.org/3/library/profile.html#module-cProfile)`推荐大多数用户使用；它是一个 C 扩展，具有合理的开销，适合于分析长时间运行的程序。

`cProfile`模块揭示了执行特定函数需要多长时间，以及该函数在程序中被调用了多少次。一个例子:

```
import cProfile
cProfile.run("run(multiples_of)")

print("\n\n")

cProfile.run("run(gen_multiples_of)")
```

我们得到类似如下的输出:

```
6 function calls in 0.119 seconds

Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.119    0.119 <string>:1(<module>)
        1    0.106    0.106    0.106    0.106 euler-1.py:12(<listcomp>)
        1    0.013    0.013    0.119    0.119 euler-1.py:30(run)
        1    0.000    0.000    0.106    0.106 euler-1.py:4(multiples_of)
        1    0.000    0.000    0.119    0.119 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}

333338 function calls in 0.415 seconds

Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.415    0.415 <string>:1(<module>)
   333334    0.266    0.000    0.266    0.000 euler-1.py:15(gen_multiples_of)
        1    0.149    0.149    0.415    0.415 euler-1.py:30(run)
        1    0.000    0.000    0.415    0.415 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
```

这意味着什么(直接来自文档):

*   `*ncalls*`:对于通话次数，
*   `*tottime*`:给定函数花费的总时间(不包括调用子函数的时间)
*   `*percall*`:是`tottime`的商除以`ncalls`
*   `*cumtime*`:该子功能和所有子功能的累计时间(从调用到退出)。这个数字对于递归函数来说是精确的*甚至*。
*   `*percall*` : 是`cumtime`除以原始调用的商
*   `*filename*` : lineno(function)提供各功能各自的数据以供识别。

从我们的结果来看，很明显，生成器函数本身并不需要很长时间来执行，但是因为它被调用了很多次(比`10^5`更多)，所以整体执行相当慢。相比之下，我们只需调用一次迭代函数，就能在 50%的时间内完成。当我们处理相对较大的数据集时，如果我们有太多的值无法放入内存，我们仍然会使用生成器函数。这里的权衡变得很明显。

关于 cProfile 需要注意的事项:

*   您可以根据执行时间、函数名、调用次数等对分析结果进行排序。
*   可以使用`[*pstats*](https://docs.python.org/3/library/profile.html#the-stats-class)` [包](https://docs.python.org/3/library/profile.html#the-stats-class)对执行概要文件进行进一步分析。
*   cProfile 有一个[命令行接口](https://docs.python.org/3/library/profile.html#instant-user-s-manual) (CLI)以方便使用。

# 可视化个人资料统计(SnakeViz)

> SnakeViz 是一个基于浏览器的图形查看器，用于 Python 的 [cProfile](https://docs.python.org/3/library/profile.html#module-cProfile) 模块的输出，是使用标准库`[pstats](https://docs.python.org/3/library/profile.html#module-pstats)` [模块](https://docs.python.org/3/library/profile.html#module-pstats)的替代方案。

开始安装 snakeviz

```
pip install snakeviz
```

剖析您的代码并将统计数据转储到一个文件中

```
import cProfile
pr = cProfile.Profile()
pr.enable()
run(multiples_of)
pr.disable()
pr.dump_stats("multiples_of.prof")

pr = cProfile.Profile()
pr.enable()
run(gen_multiples_of)
pr.disable()
pr.dump_stats("gen_multiples_of.prof")
```

这将在当前目录下创建两个新文件。您现在可以使用`**snakeviz**`来查看结果。从你的终端运行这个

```
snakeviz multiples_of.prof
```

SnakeViz 将生成一个 web 服务器(通常在[http://127 . 0 . 0 . 1:8080/SnakeViz/](http://127.0.0.1:8080/snakeviz/))，以图表的形式显示您的个人资料结果。在函数中花费的时间部分由可视化元素的范围表示，可以是矩形的宽度，也可以是弧的角度范围，具体取决于您选择的可视化样式。

# Yappi:分析并发和多线程程序

标准的分析器不能很好地处理并发和多线程程序。一个示例程序:

```
import asyncio

async def waste_a_lot_time():
    await asyncio.sleep(5)

async def do_something_else():
    print("Hello")

async def main():
    tasks = asyncio.gather(
        waste_a_lot_time(),
        do_something_else()
        )
    await tasks

asyncio.run(main())
```

如果我们用 cProfile 来分析这段代码，它将不能识别出`waste_a_lot_time`是一个瓶颈。没有一个标准的分析器是为了处理并发而构建的，除非开发人员进行一些模拟修补。这就是 Yappi 的用武之地

> yappi 1.2 版引入了`coroutine profiling`的概念。使用`coroutine-profiling`，你应该能够分析正确的墙/cpu 时间和协程的调用次数。(也包括花费在上下文切换上的时间)。你可以在这里看到细节。

安装`**yappi**`与

```
pip install yappi
```

我们将修改之前的脚本:

```
import yappi
import asyncio

async def waste_a_lot_time():
    await asyncio.sleep(5)

async def do_something_else():
    print("Hello")

async def main():
    tasks = asyncio.gather(
        waste_a_lot_time(),
        do_something_else()
        )
    await tasks

yappi.set_clock_type("wall") # Measure the absolute time taken
yappi.start() # Start profiling/watching

asyncio.run(main())

stats = yappi.get_func_stats() # Get the stats for function calls
stats.sort("ttot") # Sort it by total time
stats.print_all() # Print it all out
```

仔细观察，我们可以看到`**main**`占用了很多时间，而`**waste_a_lot_time**`就是负责这个的函数。

这就是这篇文章的全部内容。我们已经能够对 Python 脚本进行基准测试和剖析，并根据我们的结果做出业务决策。

> 感谢阅读！敬请订阅更多🙂

# 额外资源

[https://machinelearningmastery.com/profiling-python-code](https://machinelearningmastery.com/profiling-python-code)

[](https://docs.python.org/3/library/timeit.html) [## time it——测量小代码片段的执行时间

### 源代码:Lib/timeit.py 这个模块提供了一种简单的方法来为小部分 Python 代码计时。它既有…

docs.python.org](https://docs.python.org/3/library/timeit.html) [](https://docs.python.org/3/library/profile.html) [## Python 分析器

### 源代码:Lib/profile.py 和 Lib/pstats.py，并提供 Python 程序的确定性分析。配置文件是一个…

docs.python.org](https://docs.python.org/3/library/profile.html)  [## SnakeViz

### SnakeViz 是一个基于浏览器的图形查看器，用于 Python 的 cProfile 模块的输出，是使用…

jiffyclub.github.io](https://jiffyclub.github.io/snakeviz/) [](https://github.com/sumerc/yappi/) [## GitHub - sumerc/yappi:又一个 Python 分析器，但这次是多线程、asyncio 和…

### 一个支持多线程、asyncio 和 gevent 的跟踪分析器。快:Yappi 快。它完全是用 C 语言写的…

github.com](https://github.com/sumerc/yappi/)