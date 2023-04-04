# 用 C 语言编写协程

> 原文：<https://blog.devgenius.io/writing-a-coroutine-in-c-language-6a537275ddc3?source=collection_archive---------2----------------------->

![](img/36966dbe2e60f865a97831995a52fb79.png)

我已经很久没有在博客上发表任何东西了。但那是因为工作变动。我希望你明白，在保持陡峭的技术学习曲线的同时，与新的人重新适应新的环境从来都不是一件容易的事情。相应地调整自己是需要时间的。不管怎样，我写了“C 语言中的协同程序”作为我即将发布的关于 C++20 协同程序的文章的预备。今天我们将看到“协程如何在内部工作？”。

> */！\:这篇文章最初发表在我的博客上。如果你有兴趣接收我的最新文章，* [*请注册我的简讯*](http://eepurl.com/gDNybv) *。*

# 先决条件

如果你是一个绝对的初学者，你可以通过下面的先决条件。如果你不是初学者，你最好知道该跳过什么！

*   [C 程序如何转换成汇编！](http://www.vishalchovatiya.com/how-c-program-convert-into-assembly/)
*   [C 程序的内存布局](http://www.vishalchovatiya.com/how-c-program-stored-in-ram-memory/)

# 协程基础

# 什么是协程？

*   协程是一个可以暂停和恢复的功能/子程序(确切地说是合作子程序)。
*   换句话说，您可以将协程视为普通函数&线程的中间解决方案。因为，一旦函数/子程序被调用，它就会执行到最后。另一方面，线程可能会被同步原语(如互斥、信号量等)阻塞，或者被操作系统调度程序挂起。但是你也不能决定暂停还是恢复。因为它是由 OS 调度程序完成的。
*   而另一方面，协程可以在一个预定义的点暂停&以后由程序员根据需要恢复。因此，程序员将完全控制执行流程。与线程相比，开销也很小。
*   协程也称为本机线程、纤程(在 windows 中)、轻量级线程、绿色线程(在 java 中)等。

# 为什么我们需要协程？

*   正如我通常所做的，在学习任何新东西之前，你应该问自己这个问题。但是，让我来回答一下:
*   协程可以用很少的开销提供很高水平的并发性。因为它不需要操作系统干预调度。在线程环境中，您必须承担操作系统调度开销。
*   协程可以在预先确定的点上挂起，因此也可以避免锁定共享数据结构。因为您永远不会告诉您的代码在临界区中间切换到另一个协程。
*   对于线程来说，每个线程都需要自己的线程本地存储栈和其他东西。因此，内存使用量随着线程数量的增加而线性增长。而对于协同例程，您拥有的例程数量与您的内存使用没有直接关系。
*   对于大多数用例，协程是更好的选择，因为它比线程更快。
*   如果你仍然不相信，那就等着看我的 [C++协程](https://dev.totodo/)帖子吧。

# 点对点上下文切换 API 理论

*   在我们深入研究协程之前，我们需要理解以下用于上下文切换的基础函数/API。当然，正如我们所做的，用更少的、中肯的理论&用更多的代码例子。

1.  `setcontext`
2.  `getcontext`
3.  `makecontext`
4.  `swapcontext`

*   如果你已经熟悉了`[setjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)` [/](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/) `[longjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)`，那么你可能会很容易理解这些功能。你可以把这些函数看作是`[setjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)` [/](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/) `[longjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)`的高级版本。
*   唯一的区别是`[setjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)`[/](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)/`[longjmp](http://www.vishalchovatiya.com/error-handling-setjmp-longjmp/)`只允许单个非局部向上跳栈。然而，这些 API 允许创建多个协作的控制线程，每个线程都有自己的堆栈或入口点。

# 存储执行上下文的数据结构

*   `ucontext_t`如下定义的类型结构用于存储执行上下文。
*   所有四个(`setcontext`、`getcontext`、`makecontext`、&、`swapcontext`)控制流功能都在这个结构上运行。

```
typedef struct {
    ucontext_t *uc_link;    
    stack_t     uc_stack;
    mcontext_t  uc_mcontext;
    sigset_t    uc_sigmask;
    ...
} ucontext_t;
```

*   `uc_link`指向当前上下文退出时将恢复的上下文，如果该上下文是用`makecontext`(二级上下文)创建的。
*   `uc_stack`是上下文使用的堆栈。
*   `uc_mcontext`存储执行状态，包括所有寄存器和 CPU 标志、帧/基指针(即指示当前执行帧)、指令指针(即程序计数器)、链接寄存器(即存储返回地址)和堆栈指针(即指示当前堆栈限制或当前帧结束)。`mcontext_t`是一个[不透明类型](https://en.wikipedia.org/wiki/Opaque_data_type)。
*   `uc_sigmask`用于存储上下文中阻塞的信号集。这不是今天的重点。

# `int setcontext(const ucontext_t *ucp)`

*   该功能将控制转移到`ucp`中的上下文。从上下文存储在`ucp`的点继续执行。`setcontext`不归。

# `int getcontext(ucontext_t *ucp)`

*   将当前上下文保存到`ucp`。该函数在两种可能的情况下返回:

1.  在初始呼叫之后，
2.  或者当线程通过`setcontext`或`swapcontext`切换到`ucp`中的上下文时。

*   `getcontext`函数不提供区分情况的返回值(其返回值仅用于指示错误)，因此程序员必须使用显式标志变量，该变量不能是寄存器变量，并且必须声明为`volatile`以避免常量传播或其他编译器优化。

# `void makecontext(ucontext_t *ucp, void (*func)(), int argc, ...)`

*   `makecontext`功能在`ucp`中设置一个备用控制线程，该线程之前已使用`getcontext`初始化。
*   `ucp.uc_stack`成员应指向适当大小的堆栈；通常使用常量 SIGSTKSZ 或 MINSIGSTKSZ。
*   当使用`setcontext`或`swapcontext`跳转到`ucp`时，将在`func`指向的函数的入口点开始执行，使用指定的`argc`参数。当`func`终止时，控制返回到`ucp.uc_link`中指定的上下文。

# `int swapcontext(ucontext_t *oucp, ucontext_t *ucp)`

*   将当前执行状态保存到`oucp`，然后将执行控制转移到`ucp`。

# 【例 1】:用`setcontext` & `getcontext`函数理解上下文切换

*   我们已经阅读了很多理论。让我们从中创造有意义的东西。
*   考虑下面这个实现每秒打印“Hello world”的普通无限循环的程序。

```
#include <stdio.h>
#include <ucontext.h>
#include <unistd.h>
#include <stdlib.h>int main( ) {
    ucontext_t ctx = {0}; getcontext(&ctx);   // Loop start
    puts("Hello world");
    sleep(1);
    setcontext(&ctx);   // Loop end    return EXIT_SUCCESS;
}
```

*   这里，`getcontext`返回了我们之前提到的两种可能的情况，即:

1.  在初始呼叫之后，
2.  当线程通过`setcontext`切换到上下文时。

*   其余的我认为不言自明。

# 【例 2】:用`makecontext` & `swapcontext`函数理解控制流程

```
#include <stdio.h>
#include <stdint.h>
#include <stdlib.h>
#include <signal.h>
#include <ucontext.h>void assign(uint32_t *var, uint32_t val) { 
    *var = val; 
}int main( ) {
    uint32_t var = 0;
    ucontext_t ctx = {0}, back = {0}; getcontext(&ctx); ctx.uc_stack.ss_sp = calloc(1, MINSIGSTKSZ);
    ctx.uc_stack.ss_size = MINSIGSTKSZ;
    ctx.uc_stack.ss_flags = 0; ctx.uc_link = &back; // Will get back to main as `swapcontext` call will populate `back` with current context
    // ctx.uc_link = 0;  // Will exit directly after `swapcontext` call makecontext(&ctx, (void (*)())assign, 2, &var, 100);
    swapcontext(&back, &ctx);    // Calling `assign` by switching context printf("var = %d\n", var); return EXIT_SUCCESS;
}
```

*   这里，`makecontext`功能在`ctx`中设置一个控制的替代线程。当通过使用`swapcontext`使用`ctx`跳转时，将从`assign`开始执行，指定各自的参数。
*   当`assign`终止时，控制将切换到`ctx.uc_link`。指向`back`的 T33 将在跳转/上下文切换前由`swapcontext`填充。
*   如果`ctx.uc_link`置 0，则当前执行上下文被认为是主上下文，当`assign`上下文结束时线程将退出。
*   在调用`makecontext`之前，应用程序/开发人员需要确保被修改的上下文具有预分配的堆栈。并且`argc`匹配传递给`func`的类型`int`的参数数量。否则，行为是未定义的。

# C 语言中的协同程序

*   最初，我创建了单个文件示例。但后来我意识到，这将是太多的东西塞进一个文件。因此，我将实现和使用示例拆分到不同的文件中，这将使示例更容易理解。##协程实现
*   下面是 c 语言中最简单的协程:

# `**coroutine.h**`

```
#pragma once#include <stdint.h>
#include <stdlib.h>
#include <ucontext.h>
#include <stdbool.h>typedef struct coro_t_ coro_t;
typedef int (*coro_function_t)(coro_t *coro);/* 
    Coroutine handler
*/
struct coro_t_ {
    coro_function_t     function;           // Actual co-routine function
    ucontext_t          suspend_context;    // Stores context previous to coroutine jump
    ucontext_t          resume_context;     // Stores coroutine context
    int                 yield_value;        // Coroutine return/yield value
    bool                is_coro_finished;   // To indicate the current coroutine status
};/* 
    Coroutine APIs for users
*/
coro_t *coro_new(coro_function_t function);
int coro_resume(coro_t *coro);    
void coro_yield(coro_t *coro, int value);
void coro_free(coro_t *coro);
```

*   现在只需忽略协程 API。
*   这里主要关注的是协程处理程序，它有以下字段
*   `function`:保存用户提供的实际协程函数的地址。
*   `suspend_context`:用于暂停协程功能。
*   `resume_context`:保存实际协程函数的上下文。
*   `yield_value`:存储中间暂停点&之间的返回值，也是最终返回值。
*   `is_coro_finished`:检查协程寿命状态的指示器。

# `**coroutine.c**`

```
#include <signal.h>
#include "coroutine.h"static void _coro_entry_point(coro_t *coro) {
    int return_value = coro->function(coro);
    coro->is_coro_finished = true;
    coro_yield(coro, return_value);
}coro_t *coro_new(coro_function_t function) {
    coro_t *coro = calloc(1, sizeof(*coro)); coro->is_coro_finished = false;
    coro->function = function;
    coro->resume_context.uc_stack.ss_sp = calloc(1, MINSIGSTKSZ);
    coro->resume_context.uc_stack.ss_size = MINSIGSTKSZ;
    coro->resume_context.uc_link = 0; getcontext(&coro->resume_context);
    makecontext(&coro->resume_context, (void (*)())_coro_entry_point, 1, coro);
    return coro;
}int coro_resume(coro_t *coro) {    
    if (coro->is_coro_finished) return -1;
    swapcontext(&coro->suspend_context, &coro->resume_context);
    return coro->yield_value;
}void coro_yield(coro_t *coro, int value) {
    coro->yield_value = value;
    swapcontext(&coro->resume_context, &coro->suspend_context);
}void coro_free(coro_t *coro) {
    free(coro->resume_context.uc_stack.ss_sp);
    free(coro);
}
```

*   协程使用最多的 API 是`coro_resume` & `coro_yield`，它拖动挂起&恢复的实际工作。
*   如果你已经有意识地浏览了上面的上下文切换 API 例子，那么我不认为`coro_resume` & `coro_yield`有什么好解释的。它只是`coro_yield`跳到`coro_resume`T25，反之亦然。除了跳转到`_coro_entry_point`的对`coro_resume`的第一次调用。
*   `coro_new`函数为处理程序和堆栈&分配内存，然后填充处理程序成员。再次`getcontext` & `makecontext`这一点应该很清楚。如果没有，那么请重新阅读上面关于上下文切换 API 示例的部分。
*   如果你真正理解上面的协同 API 实现，那么很明显的问题是为什么我们甚至需要`_coro_entry_point`？为什么不能直接跳转到实际的协程函数？。
*   但是我的论点将是“你如何确保协程的生存期？”。
*   这在技术上意味着，调用`coro_resume`的次数应该类似于/有效于调用`coro_yield`的次数加 1(实际返回)。
*   否则你就无法跟踪产量。行为将变得不确定。
*   然而，`_coro_entry_point`函数是必需的，否则你无法推断出 corotine 执行完全完成。并且对`coro_resume`的下一个/后续调用不再有效。

# 协程寿命

*   通过上面的实现，**使用协程处理程序**，在整个程序/应用程序生命周期中，您应该只能完整地执行一次协程函数。
*   如果你想再次调用协程函数，那么你需要创建新的协程处理程序。其余的过程将保持不变。

# 协程用法示例

# `**coroutine_example.c**`

```
#include <stdio.h>
#include <assert.h>
#include "coroutine.h"int hello_world(coro_t *coro) {    
    puts("Hello");
    coro_yield(coro, 1);    // Suspension point that returns the value `1`
    puts("World");
    return 2;
}int main() {
    coro_t *coro = coro_new(hello_world);
    assert(coro_resume(coro) == 1);     // Verifying return value
    assert(coro_resume(coro) == 2);     // Verifying return value
    assert(coro_resume(coro) == -1);    // Invalid call
    coro_free(coro);
    return EXIT_SUCCESS;
}
```

*   用例非常简单:
*   首先，创建一个协程处理程序。
*   然后，在同一个协程处理程序的帮助下，启动/恢复实际的协程函数。
*   而且，每当您的实际协程函数遇到调用`coro_yield`，它将暂停执行&，返回在`coro_yield`的第二个参数中传递的值。
*   以及当实际的协程函数执行完全完成时。对`coro_resume`的调用将返回`-1`以指示协程处理程序对象不再有效&生命周期已到期。
*   所以，你可以看到`coro_resume`是我们的协程`hello_world`的包装器，它部分地执行`hello_world`(显然是通过上下文切换)。

# **编译**

*   我已经用 gcc 9.3.0 和 glibc 2.31 在 WSL 中测试了这个例子。

```
$ gcc -I./ coroutine_example.c coroutine.c  -o myapp && ./myapp 
Hello
World
```

# 离别赠言

如果你理解“CPU 如何执行代码”,你会发现这并不神奇..!"良好的 Glibc 提供了一组丰富的上下文切换 API。而且，从低级开发人员的角度来看，这仅仅是一个精心安排的&难以组织/维护(如果使用原始的)上下文切换函数调用。我在这里的意图是为 C++20 协程打下基础。因为我相信，如果你从 CPU &编译器的角度来看代码，那么在 C++中一切都变得容易推理了。下次在我的 [C++20 协程](https://dev.totodo/)帖子中再见。