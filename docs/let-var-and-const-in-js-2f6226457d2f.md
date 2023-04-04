# JS 中的 Let、Var 和 Const

> 原文：<https://blog.devgenius.io/let-var-and-const-in-js-2f6226457d2f?source=collection_archive---------8----------------------->

大家好，今天讨论的是关于变量让 var 和 const..

人们可能会在 var，let 和 const 之间搞不清为什么要用和什么时候要用，所以博客在这里来澄清这种混乱…

![](img/91097b202af28bc24f9d5d4855750409.png)

这个博客将帮助你理解为什么使用 const 和 let over var 以及在哪里使用它们，所以让我们开始…

> 建议:只是一个建议，用下面给出的例子，在你的游戏机上也试试，这样它会一直留在你的脑海里

# 1) Var:

Var 在 Javascript 中使用了很长时间，但是 let 和 const 是在 ES6 版本中引入的。使用**变量**你可以重新赋值给一个变量，也可以使用变量关键字重新声明变量..

**举例:**

```
var b = "Hello"
var b = "Hey" // user can redeclare it using var keyword
 b = "nice"  // you can also reassign value to the variable 
```

**范围:**

Var 关键字有全局作用域或函数作用域。这意味着如果任何带有 var 关键字的变量是全局声明的(在函数外部)，那么它在函数内部是可访问的，也是全局可访问的。看例子才能正确理解。

**例 1 :**

```
var b = "Hello"
        function Checkb(){
            console.log(b)
        }
    Checkb();
    console.log(b);
```

**解释:**在上面的例子中，你可以看到变量“b”和 var 关键字是全局声明的，我们尝试两次获取值，一次在函数内部，第二次在函数外部，所以这两种方法都能得到正确的结果。参见输出

***输出:***

```
"Hello"
"Hello"
```

但是，如果变量“b”在函数内部声明了 var 关键字，那么它只能在函数内部访问，在函数外部不可访问，并抛出一个引用错误。

**例 2:**

```
function f() {

        var b = "Hello";     // It can be accessible anywhere
        console.log(b)        // within this function
    }
    f();

    console.log(b);      // B cannot be accessible
                        //because outside of function
```

**输出:**

![](img/6c39e6cddadfb46022691ceb5ebd60f9.png)

**解释:**所以在第二个例子中，变量“b”是在函数内部声明的，所以我们试图在函数内部获取一次，然后在函数外部获取。在第一个示例中，我们得到了预期的结果，但在第二个示例中，我们得到了 ReferenceError，因为带有 var 关键字的变量在函数外部是不可访问的。所以我们知道 var 关键字有全局作用域，也有函数作用域。

# 2)让:

正如我上面提到的，**让**在 ES6 中引入。**让**是 **var 的改进版。**使用 let 关键字，您可以将值重新分配给带有 let 关键字的变量，但不能重新声明带有 let 关键字的变量。

**例如:**

```
let b = 1
let b = 2     //error~ user cannot redeclare the variable again
 b = "0"      // you can reassign value to the variable
```

**说明:**

变量“b”已经用 let 关键字声明，所以不能重新声明。如果你这样做，你将得到 SyntaxError( b 已经被定义)。但是你可以像我上面做那样给变量重新赋值。

**范围:**

**设**有块作用域。在`{}`内的任何东西都被认为是一个块。块范围意味着如果“b”变量是在块内声明的，那么它在块外是不可访问的。

**示例:**

```
let c = 10**if** (c == 10) {let a = "hi"console.log(a) }console.log(a)
```

**输出:**

```
hi
ReferenceError: a is not defined
```

**说明:**

我们试图在声明“a”的代码块中获取“a”的值，因此我们得到了预期的值。但是第二次我们没有得到预期的值，而是得到了 ReferenceError。因为" a "是在块内声明的，如果我们试图在块外访问它，会显示错误。发生这种情况是因为任何用 let 关键字声明的变量都变成了块范围。

# 3)常数:

Const 关键字也是在 ES6 中用`let`引入的。`Const`相当自明。`const`变量保持恒定值。虽然`const`的本质与`let`大不相同，但它们有很多相似之处，比如它们都有块范围。 *const* 关键字具有与 *let* 关键字相同的所有属性，除了用户不能更新的属性。参见下面的例子

```
//let
let a = "john"
a = "micheal"       //value can be updated//const
const b = "red"
b = "blue"       // give you error because can't be updated
```

使用 const 关键字，既不能重新赋值，也不能用 const 重新声明变量。如果你这样做，会给你带来错误。

**举例:**

```
const b = "red"
const b = "green"   // not allowed to redeclare 
b = "yellow"     // not allowed to reassign the value
```

**范围:**

正如我之前提到的，const 也有类似 let 的块范围。block 作用域是什么，上面已经讲清楚了。

**例如:**

```
{
 const x = 9
 console.log(x)
 }

 console.log(x)
```

**输出:**

```
9
ReferenceError: x is not defined
```

## **最可取:**

使用 const 关键字，代码变得更干净，但使用 var 关键字，代码变得混乱，因为如果在长代码中错误地将值重新分配给使用 var 关键字的变量，将很难调试代码。所以开发人员最喜欢的是 const 和 let……尤其是 const 使得代码更干净，更容易调试错误。

## 结论:

在结束之前，让我们总结一下我们刚刚理解的内容:

`var` : **函数作用域，可以更新和重新声明。**

`let` : **块范围，可以更新，但不能重新声明。**

`const` : **块范围的**，**不能更新和重新声明。**

## 我几乎清楚了所有主要的东西，但如果你想深入了解范围的细节，你可以查看这个 [freeCodeCamp](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/#:~:text=var%20declarations%20are%20globally%20scoped%20or%20function%20scoped%20while%20let,be%20updated%20nor%20re%2Ddeclared.) 。