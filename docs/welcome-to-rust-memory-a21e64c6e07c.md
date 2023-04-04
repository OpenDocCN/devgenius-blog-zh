# 欢迎来到信任记忆

> 原文：<https://blog.devgenius.io/welcome-to-rust-memory-a21e64c6e07c?source=collection_archive---------3----------------------->

# Rust 中的内存管理

所有权是 Rust 最独特的特性，它使 Rust 能够在不需要垃圾收集器的情况下保证内存安全。因此，理解所有权在 Rust 中是如何工作的非常重要

# **所有权**

*所有权*是 Rust 管理内存的方式。它是编译器在编译时检查的一组规则，这些规则不会降低程序运行的速度。

# 堆栈和堆

*   堆栈和堆都是程序在运行时可用的内存的一部分
*   在编译时，堆栈上的数据必须具有已知的固定大小
*   编译时大小未知或大小可能改变的数据必须存储在堆中
*   操作方面(分配、访问)堆更昂贵(更慢)
*   堆还需要簿记(代码的什么部分使用什么数据，重复数据删除，清理未使用的数据)，这种所有权地址

**所有权规则** 首先，我们来看看所有权规则。当我们通过举例说明这些规则时，请记住这些规则:

*   Rust 中的每个值都有一个变量，称为它的所有者。
*   一次只能有一个所有者。
*   当所有者超出范围时，该值将被丢弃。

# 变量作用域

*范围*是项目有效的程序范围。

```
{                      // s is not valid here, it’s not yet declared
    let s = "hello";   // s is valid from this point forward // do stuff with s
}                      // this scope is now over, and s is no longer valid
```

# `String`型

*   字符串文字是不可变的，它们的值在编译时必须是已知的
*   `String`另一方面，类型是在堆上分配的

```
// create a `String` from a literal:
let s = String::from("hello");
// mutate `String` (append to it):
s.push_str(", world!");
```

# 内存和分配

*   Rust 既不使用显式的`free`(将内存返回给操作系统)，也不使用垃圾收集(GC)
*   相反，一旦所有者超出范围，内存就会自动返回
*   然后调用特殊功能`drop`

# 移动

```
// `s1` is *moved* which makes a shallow copy of and then invalidates it
let s1 = String::from("hello");
let s2 = s1;
// s1 is not valid anymore
```

# 克隆

*   深层拷贝
*   更贵

```
let s1 = String::from("hello");
let s2 = s1.clone();// both, `s1` and `s2` are valid
println!("s1 = {}, s2 = {}", s1, s2);
```

# 复制(仅堆栈)

```
// make copy of `x`
let x = 5;
let y = x;
```

*   如果值完全存储在堆栈中，并且具有`Copy`特征，它将被复制而不是移动，也就是说，旧变量在被赋值给新变量后仍然可用
*   `Copy`和`Drop`特征是互斥的
*   一些具有`Copy`特质的类型有:
*   所有整数和浮点类型，例如`u32`和`f64`
*   `bool`
*   `char`
*   元组，如果它们只包含`Copy`的类型，例如`(i32, i32)`而不是`(i32, String)`

# 功能

与给变量赋值类似，将变量传递给函数会复制或移动它。

```
fn main() {
    let s = String::from("hello");
    takes_ownership(s);
    // `s` is not valid anymore, its value has been moved into function let x = 5;
    makes_copy(x);
    // `x` is still valid
}fn takes_ownership(some_string: String) {
    println!("{}", some_string);
}fn makes_copy(some_integer: i32) {
    println!("{}", some_integer);
}
```

# 返回值和范围

返回值也可以转移所有权。

```
fn main() {
    let s1 = gives_ownership(); let s2 = String::from("hello");
    let s3 = takes_and_gives_back(s2);
}fn gives_ownership() -> String {
    let some_string = String::from("hello");
    some_string
}fn takes_and_gives_back(a_string: String) -> String {
    a_string
    // note: if we would want to return additional data, we could use tuple. e.g.
    // let length = a_string.len();
    // (a_string, length)
}
```

如果我们想让函数使用一个值，但不获取所有权，而不是将它传递回来，我们可以使用*引用*。

# 参考和借用

*   可以传递一个*引用*，而不是像上一个例子那样从一个函数传回一个值
*   这个引用可以用来代替获取所有权
*   将引用作为函数参数称为*借用*
*   正如变量一样，默认情况下引用是不可变的
*   `&`:引用运算符，`*`:解引用运算符

```
fn main() {
    let s1 = String::from("hello");
    let len = calculate_length(&s1);
    println!("The length of '{}' is {}.", s1, len);
}fn calculate_length(s: &String) -> usize {
    s.len()
}
```

# 可变引用

```
fn main() {
    let mut s = String::from("hello");
    change(&mut s);
}fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

*   在特定的范围内，对特定值的可变引用可以有且只能有一个
*   这可以防止编译时的数据竞争
*   有不可变引用时不能有可变引用
*   不可变的值可能会改变
*   引用的范围从它被引入的地方开始，一直延续到它的最后一次使用

```
let mut s = String::from("hello");// declare two immutable references
let r1 = &s;
let r2 = &s;
println!("{} and {}", r1, r2);// scope of s1 and s2 have ended (they are not used anymore),
// we can now declare an immutable reference
let r3 = &mut s;
println!("{}", r3);
```

# 悬空引用

*   *悬空指针*:引用已经释放的内存的指针
*   Rust 编译器防止这种情况，因为它确保数据不会在对数据的引用超出范围之前超出范围

```
fn dangle() -> &String { // returns *reference* to a String
    let s = String::from("hello");
    &s // return reference to new String `s`
} // Here, s goes out of scope, and is dropped. Its memory goes away.
  // Error
```

下一次生锈寿命