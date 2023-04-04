# 如何使用 Bun、Rust 和 Go 运行速度惊人的 JavaScript 代码

> 原文：<https://blog.devgenius.io/how-to-run-blazingly-fast-javascript-code-with-bun-rust-and-go-2e645cf579b5?source=collection_archive---------4----------------------->

![](img/899d38fd874a1a452cfbb32a805f9bd3.png)

[瓦莱丽·布兰切特的图片](https://unsplash.com/@valerieblanchett)

你听说过 Bun 吗？没有吗？好吧，你会有所收获的。Bun 是一个新的、速度惊人的 JavaScript 运行时，它利用了苹果的 JavaScript 引擎 JavaScript Core，而不是 Node.js 今天使用的 V8 引擎。也是用齐格语写的。如果这是 Bun 不可思议的速度的一部分，这已经超出了这个故事的范围。

Bun 使之变得非常简单的是使用 FFI，或外国函数接口。这意味着，我们能够运行用 C/C++、Rust、Go 等编程语言编写的代码。

想象一下，你或者有一个用不同语言编写的库，或者你因为某些原因(速度、底层等等)需要使用不同的语言。您可以从编译语言中导出函数，您可以在 JavaScript 中运行这些函数。如果您追求速度，请记住，如果计算量很小，可能会有一点开销，这意味着您可能会更好地在 JavaScript 中运行它，尽管我自己没有在这方面采取任何措施。

由于 Bun 处于测试阶段，情况可能会有所变化。我建议你去 [https://bun.sh](https://bun.sh) 看看那里的安装选项。

## 要求

*   小圆面包
*   Go 编译器
*   Rust 编译器

为了创建一个新的 Bun 项目，我们将运行`bun init`

```
~/  bun init                                                              ✔  10:54:57
bun init helps you get started with a minimal project and tries to guess sensible defaults. Press ^C anytime to quitpackage name (bun):
entry point (index.ts):Done! A package.json file was saved in the current directory.
 + index.ts
 + .gitignore
 + tsconfig.json (for editor auto-complete)
 + README.mdTo get started, run:
  bun run index.ts
```

Bun 为我们创建了一堆文件。您可能已经注意到了`index`文件是在 typescript 中。默认情况下，Bun 使用 typescript。

## 面包和铁锈 FFI

让我们编写一个快速计算器/加法器函数，它使用 Rust 将两个数相加。在程序开始时初始化二进制函数可能是个好主意，所以我们将在 IIFE 中加载所有内容(立即调用函数表达式)

```
// index.tsimport { dlopen, FFIType, ***suffix*** } from 'bun:ffi'

const FFI = (() => {
    const rustLib = dlopen(`librust_file.${***suffix***}`, {
        add: {
            args: [FFIType.i32, FFIType.i32],
            returns: FFIType.i32,
        }
    })

    return {
        rust: rustLib.symbols,
    }
})()
```

我们使用`dlopen``将二进制库加载到 JavaScript 中。我们还描述了哪些函数将从二进制文件中导出，并导入到我们的 JavaScript 中，以及哪些类型的参数和函数的返回类型。

如果我们试图运行这个程序，我们会得到一个错误，说明它无法找到或打开库。这很好。这意味着代码试图找到生锈的文件。`suffix`将根据您使用的操作系统动态添加。因为我运行的是 Mac，所以后缀会是`dylib`。也支持`so`和`dll`。在编译 rust 应用程序时，请记住这一点。

让我们写一份快速防锈申请。在项目的根目录下，创建一个文件`rust_file.rs`

```
// rust_file.rs#[no_mangle]
pub extern "C" fn add(a: i32, b: i32) -> i32 {
    a + b
}
```

由于 Rust 是一种编译语言，我们不知道一旦编译后文件名会是什么，所以`no_mangle`有助于保持文件名不变。我们还告诉编译器，这个函数将是公共的。

我们可以直接用 rust comiler 来编译。使用适用于您的操作系统的输出文件类型。

```
~/  rustc --crate-type cdylib rust_file.rs
```

如果我们再次运行我们的 Bun 程序，我们会注意到之前看到的错误已经消失了。这意味着 Rust 程序已经成功加载到我们的 JavaScript 程序中。酷！

下面我们的`FFI`功能:

```
***console***.log(FFI.rust.add(1, 2))
```

运行你的程序，输出应该是`3`。非常酷！计算是由 Rust 处理的。

## FFE 面包和 Go

大多数 Rust 示例直接取自 Bun 文档。我是 Go 编程语言的粉丝。Bun 文档没有提到任何关于从 Bun 运行 golang 代码的内容。我们能从 Bun 运行 Go 代码吗？好吧，让我们来看看！

我们可以从扩展我们的`FFI`函数来加载一个 Go 应用程序开始。

在我们的`FFI`中，我们将把 Go 应用程序加载到一个`goLib`变量中，并以与 Rust 应用程序相同的方式返回它:

```
const FFI = (() => {
    ....
    const goLib = dlopen(`goFile.${***suffix***}`, {
        Add: {
            args: [FFIType.i32, FFIType.i32],
            returns: FFIType.i32,
        },
    })

    return {
        rust: rustLib.symbols,
        go: goLib.symbols
    }
})()
```

现在，除了`Add`中的大写`A`外，该代码与 Rust 应用程序相同。让我们看看是否可以创建一个 Go 应用程序来为我们做同样的计算。

在项目的根目录下创建一个文件`goFile.go`

```
// goFile.gopackage main

func main(){}

func Add(x int, y int) int {
return x + y
}
```

我们正在导出一个函数`Add`，它接受两个整型数，并将结果作为一个整型数返回。让我们构建程序:

```
~/  go build -buildmode=c-shared -o goFile.dylib goFile.go
```

如果我们运行 Bun 程序，编译器会告诉我们`Add`函数丢失了。原因可能显而易见，也可能不明显，但是如果您查看您的目录，您会注意到没有 C 头文件。所以我们需要做的是添加足够的信息，这样 Go 编译器也可以生成一个头文件。

我们可以通过导入一个名为`C`的 Go 库来解决这个问题，并在导出的函数上显式添加一个注释:

```
// goFile.gopackage main 

import "C" // <- new 

func main(){}

*//export Add <- new* func Add(x int, y int) int {
return x + y
}
```

现在，如果我们运行相同的构建命令，头文件将被正确创建，并且我们能够成功加载 Go 程序。

让我们编辑我们的`index.ts`文件来记录我们的 Go 结果:

```
***// index.ts
.....******console***.log('Rust:', FFI.rust.add(1, 2))
***console***.log('Go:', FFI.go.Add(3, 4))
```

如果我们运行我们的程序:

```
 bun run index.ts            
Rust: 3
GO: 7
```

现在，我们只讨论了如何处理数字。Go 和 Rust 功能实际上是相同的。弦乐怎么样？

字符串有点难处理，部分原因是 C 语言中不存在我们在更高级的编程语言中习惯使用的字符串类型(但也因为它们的编码不同。([更多信息见 Bun docs](https://github.com/oven-sh/bun#strings-cstring) )

让我们定义如何导入一个处理字符串的 Go 函数。在我们的`FFI`函数中，我们将向`goLib`添加一个新函数

```
const goLib = dlopen(`goFile.${***suffix***}`, {
    ....
    Greet: {
        args: [FFIType.cstring],
        returns : FFIType.cstring,
    }
})
```

所以我们将使用的类型是`cstring`。该函数将接受一个名称作为字符串，并返回一个问候，也是一个字符串。我们将前往我们的 Go 文件并添加我们的新函数

```
// goFile.go....
import (
    "unsafe"
)
....//export Greet
func Greet(name *C.char) *C.char {
    cstr := C.CString("Hey from Go, " + C.GoString(name) + "!")
    defer C.free(unsafe.Pointer(cstr))
    return cstr
}
```

我们从参数中创建并返回一个`C string`。由于这将被编译成 C 库，我们需要记住释放所使用的内存。

如果我们试图在此时构建我们的 Go 程序，我们会得到一个错误:

```
could not determine kind of name for C.free
```

我们如何解决这个问题？事实证明，我们不仅能够构建 C 头文件和库，还能够将它们包含在我们的 Go 代码中！让我们导入 C 标准库。我们就在 C 导入上面做:

```
// goFile.go....*// #include <stdlib.h>* import "C"....
```

完美。构建是成功的。让我们讨论一下如何在我们的 JavaScript 应用程序中使用它。

我们可以尝试像之前使用加法器功能一样使用它:

```
***console***.log('Greeter:', FFI.go.Greet('Stephan'))
```

我们希望它能给我们一个好的欢迎信息。相反，我们得到一个错误，告诉我们做错了。

```
To convert a string to a pointer, encode it as a buffer
```

我们正在处理比我们通常在 JavaScript 中发现的更低级的数据，正如前面提到的，C 和 JavaScript 中的字符串是完全不同的动物。为了解决这个问题，我们可以用字符串创建一个缓冲区

```
// index.ts....const nameBuffer = ***Buffer***.from("John Doe", "utf-8")
```

由于 C 字符串在`utf-8`中，我们确保缓冲区也在。

如果我们运行我们的程序，它不会崩溃！然而，输出不是真的可以理解…

```
Greeter:  O$��
```

结果是，我们的欢迎函数返回一堆东西，而不是我们预期的值。它还返回一个指向我们输出的指针:

```
// index.ts.....const nameBuffer = ***Buffer***.from("John Doe", "utf-8")
const { ptr } = FFI.go.Greet(nameBuffer)
***console***.log(ptr)
```

产出:

```
0.00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000052150168576707
```

一个对我们没有意义的长数字(不应该有意义)。我们可以用指针创建一个新的`Cstring:`

```
const nameBuffer = ***Buffer***.from("John Doe", "utf-8")
const { ptr } = FFI.go.Greet(nameBuffer)
const greeting = new CString(ptr)
***console***.log(greeting)
```

产出:

```
Hey from Go, John Doe!
```

## 临终遗言

非常酷。想象一下从 JavaScript 运行 C 代码是多么容易！Bun 还声称运行速度比 nap 快 5 倍，比 Deno 快 100 倍，这真是令人震惊。

不幸的是，Bun 还没有准备好生产，但我肯定不能等到它！

我希望你喜欢这个在芬兰的小冒险。
你也可以在[https://github.com/sbvalois/bun-go-rust-blog](https://github.com/sbvalois/bun-go-rust-blog)找到这篇博文的回购

下次见！斯蒂芬·巴克伦德·瓦卢瓦