# 科特林编程语言完整学习指南[2022] 3/6 函数

> 原文：<https://blog.devgenius.io/kotlin-programming-langauge-the-complete-learning-guide-2022-3-6-functions-70e634205288?source=collection_archive---------13----------------------->

![](img/7276c85b01655c7adf03c003a248617e.png)

学习 Kotlin 这种最强大和最有用的编程语言之一，并准备好开始开发 powerfull 原生移动应用程序和其他平台开发

# **功能**

```
fun main (){
} 
```

我们现在对函数很熟悉了，我们在前面的章节中使用了它，但是现在我们要创建我们自己的函数，我们已经看到的 main 函数，我们知道它是程序的入口点，所以如果没有写任何东西，它就不会被执行，没有 main 函数程序也不能运行

让我们开始创建我们的函数，但是现在我们将在主函数之外编写函数，然后在主函数内部调用它，如下所示

```
fun main (){
}
fun myFun(){}
```

**func** →关键字声明创建一个函数

**myFunc** →功能名称

**() - >** 我们在哪里添加参数，我们很快就会看到

**{} →** 函数体，在这里你要考虑函数应该做什么

```
fun main() {
    myFunc()
}

fun myFunc() {
    println("Iam The Function You Called Me")
}//output Iam The Function You Called Me
```

这就是我们调用函数的方式，我们也可以任意多次调用它

```
fun main() {
    myFunc()
    myFunc()
    myFunc()
    myFunc()

}

fun myFunc() {
    println("Iam The Function You Called Me")
}//output Iam The Function You Called Me
//Iam The Function You Called Me
//Iam The Function You Called Me
//Iam The Function You Called Me
```

让我们看看什么是()什么是参数，假设我们要做两个数相加

当然我们可以用+我们之前讨论过的，+基本上也是一个函数

```
fun main() {
    add()

}

fun add() {
}
```

现在，当我们运行时，什么也没有发生，首先是因为我们没有真正声明我们需要从 add()中得到什么，以及它应该应用于哪些变量

()中的变量称为参数

```
fun main() {
    add()
}

fun add(x: Int, y: Int) {
    println(x + y)
}
```

一旦你这样调用它，你会发现编译器会告诉你

没有为参数 x，y 传递值

所以你应该用 int 值填充括号，现在它们被称为**参数**

```
fun main() {
    add(1, 9)
}

fun add(x: Int, y: Int) {
    println(x + y)
}//output 10
```

功能的概念是你可以根据需要多次重复使用的东西

```
fun main() {
    add(1, 9)
add(50, 9)
add(1, 0)
}

fun add(x: Int, y: Int) {
    println(x + y)
}//output 10 
//59 
//1
```

但是这个函数只是执行一些东西，我也想使用它的值

这就是所谓的回归

**返回**

这是一个函数，它结束你想要的任何东西的执行，然后返回你声明的类型的值，这将作为 delcated 返回类型的值

```
fun main() {
    var x = add(1, 9)
    print(x)
}

fun add(x: Int, y: Int): Int {
    return x + y;
}//output 10 
```

看，现在在 main 函数中，add()被视为 int 类型的值，你可以像处理 int 类型的值一样处理它，并执行所有应用于 int 数据类型的操作

```
fun main() {
    var div = add(1, 9) / 2
    print(div)
}

fun add(x: Int, y: Int): Int {
    return x + y;
}//output 5
```

看，它被接受去除 add()，现在它是一个 int 值

> 注意:我们传递的参数是**位置参数**并且总是必需的，我们不能让它为空。它是强制的，而且你应该按照你在前面的值中声明的顺序添加你的值(参数)。我们没有注意到不同之处，因为它是一个加法，所以不会有什么不同，所以请看下面的例子

```
fun main() {
    print(myBio("mohab", "programming"))
}

fun myBio(hobby: String, name: String): String {
    return "my name is $name and i love $hobby"
}//output my name is programming and i love mohab
```

现在我们注意到这是我们不想要的，因为我们没有按照在函数中声明的顺序添加参数，并且参数是相同的类型，所以它不会通知错误，如下所示

```
fun main() {
    print(myBio("mohab", "programming"))
}

fun myBio(age: Int, name: String): String {
    return "my name is $name and i am $age  years old "
}//error 
```

现在 compielr 会告诉你一个类型不匹配，你声明了 int 并赋了 string

**命名参数**

kotlin 也允许我们用下面的方法传递参数

```
fun main() {
    print(myBio(name = "mohab", age = 24))
}

fun myBio(age: Int, name: String): String {
    return "my name is $name and i am $age years old "
}// output my name is mohab and i love 24
```

我们注意到输出是我们声明的方式，即使我们以不同的顺序传递它们，但是我们自己输入参数名并赋值给它

**默认参数**

假设我们有一个变量，它不需要总是给定一个值，如果没有给定，它应该带有一个默认值

```
fun main() {
    println(myBio(age = 24))
    println(myBio(age = 30, name = "Messi"))
}

fun myBio(age: Int, name: String = "Mohab"): String {
    return "my name is $name and i am $age years old "
}
//my name is Mohab and i am 24 years old 
my name is Messi and i am 30 years old 
```

看看当我们运行程序时，即使我们没有传递名字参数，它也能正常运行，没有任何问题，还打印出了它的默认值，我们把它加到了“Mohab”

> 注意函数也可以被称为方法，但是在所有方面都是一样的，但是它与 OOP 更相关，我们将在接下来的文章中详细介绍

**函数中的局部变量**

```
fun main() {
    println(myBio(age = 24))
    println(myBio(age = 30, name = "Messi"))
}

fun myBio(age: Int, name: String = "Mohab"): String {
    var bio = "my name is $name and i am $age years old "
    return bio
}
```

我们在函数块中添加的变量 **bio** 只能通过这个函数在本地访问

**这部分就到此为止，等待下一部分**

**如果这篇文章真的对你有帮助，请为我鼓掌**

**感谢阅读，等待您的评论和回复…**