# 循环比较

> 原文：<https://blog.devgenius.io/in-the-c-programming-language-there-are-several-types-of-loops-that-can-be-used-to-execute-a-5f3947bb5a68?source=collection_archive---------22----------------------->

在 C 编程语言中，有几种类型的循环可用于多次执行一个代码块。这些包括`for`、`while`、`do while`和`goto`循环。

`for`循环是一种允许您在一条语句中指定循环变量的初始化、条件和增量/减量的循环。例如:

```
for (int i = 0; i < 10; i++) {
    // Code to be executed
}
```

这个`for`循环将在其块内执行代码 10 次，变量`i`的值被初始化为 0，然后每次循环运行时递增 1。

一个`while`循环类似于一个`for`循环，但是它只包括一个在每次迭代开始时检查的条件。例如:

```
int i = 0;
while (i < 10) {
    // Code to be executed
    i++;
}
```

这个`while`循环也将执行其代码块中的代码 10 次，但是`i`变量的增量必须在循环的代码块中手动完成。

`do while`循环类似于`while`循环，但条件是在每次迭代结束时检查，而不是在开始时。例如:

```
int i = 0;
do {
    // Code to be executed
    i++;
} while (i < 10);
```

这个`do while`循环也将执行其块内的代码 10 次，但是条件是在代码执行之后而不是之前检查的。

`goto`循环是一种使用`goto`语句跳转到代码中特定点的循环。例如:

```
int i = 0;

loop:
    // Code to be executed
    i++;
    if (i < 10) {
        goto loop;
    }
```

这个`goto`循环还将在其块内执行代码 10 次，每次执行代码时使用`goto`语句跳回到`loop:`标签。

就汇编代码而言，C 语言中不同类型的循环将根据具体的处理器体系结构和所使用的指令集以不同的方式实现。下面是一个如何在 x86 汇编代码中实现`for`循环的例子:

```
mov eax, 0 // Initialize loop variable to 0
mov ecx, 10 // Set number of iterations

loop:
    // Code to be executed

    inc eax // Increment loop variable
    dec ecx // Decrement number of iterations
    jnz loop // Jump to loop label if ecx is not 0as
```

此`for`循环使用`mov`、`inc`、`dec`和`jnz`指令来实现循环变量的初始化、条件和递增/递减。

相比之下，下面是一个如何在 x86 汇编代码中实现`while`循环的例子:

```
mov eax, 0 // Initialize loop variable to 0

loop:
    // Code to be executed

    inc eax // Increment loop variable
    cmp eax, 10 // Compare loop variable to 10
    jl loop // Jump to loop label if eax is less than 10
```

这个`while`循环使用`mov`、`inc`、`cmp`和`jl`指令来实现循环变量的条件和增量。

下面是一个如何在 x86 汇编代码中实现`do while`循环的例子:

```
mov eax, 0 // Initialize loop variable to 0

loop:
    // Code to be executed

    inc eax // Increment loop variable
    cmp eax, 10 // Compare loop variable to 10
    jl loop // Jump to loop label if eax is less than 10
```

这个`do while`循环类似于`while`循环示例，但是`cmp`和`jl`指令被移动到循环块的末尾。

最后，下面是一个如何在 x86 汇编代码中实现`goto`循环的例子:

```
mov eax, 0 // Initialize loop variable to 0

loop:
    // Code to be executed

    inc eax // Increment loop variable
    cmp eax, 10 // Compare loop variable to 10
    jl loop // Jump to loop label if eax is less than 10
```

就性能而言，C 中的`for`和`while`循环通常被认为是最有效的，因为它们使用单个条件语句，并且不需要额外的语句来增加或减少循环变量。`do while`和`goto`循环通常被认为效率较低，因为它们需要额外的语句来递增循环变量或跳转到代码中的特定点。

就汇编代码而言，不同类型的循环将以不同方式实现，这取决于所使用的特定处理器架构和指令集。然而，一般来说，`for`和`while`循环是最有效的，因为它们使用少量的指令来实现循环。`do while`和`goto`循环效率较低，因为它们需要额外的指令来实现循环。

总的来说，选择在 C 程序中使用哪种类型的循环将取决于所编写代码的具体要求和需要。