# 嵌入式 C 基础:第三部分

> 原文：<https://blog.devgenius.io/embedded-c-basics-part-iii-210a8a1df114?source=collection_archive---------12----------------------->

功能、无效和范围

函数是被调用时运行的代码块。它们最适合用于需要在项目中重复的代码。

![](img/3af70995b2e55546de3c392a8125b567.png)

袖珍多功能工具

在 C 语言中，你使用最多的函数是 main 函数。这个函数是任何 C 程序的入口点。还有许多其他内置功能，例如:

*   打印函数
*   c 语言的格式输入函数

您创建的任何自定义函数都被称为用户定义函数。
下面列出了一些基本的例子

```
// A Function cosists of the following
// A function definition contains the following
// Return type name (parameters)
int addTen(int x) {
   return x + 10;
}// Not all functions have to return values
// These functions will have the void type
void countDown(int start){
    while(start >= 0){
        printf("%d!", start);
        start--;
    }}
```

如上所示，功能由以下部分组成:

*   函数原型
    —int function name(params)；
    —声明函数
    —可以跟在函数体后面，也可以在文件开头作为声明，让编译器在定义函数之前就知道它
*   函数体
    —{ }
    之间的所有东西——函数实际做什么
*   函数调用
    —int number = function name(param 1，param 2)；
    —执行功能
    —可以多次调用

```
//Function prototype
int product(int x, int y);int main(){
   int pro = product(10, 6); //Function call
   printf("The product of 10 and 6 is: %d", pro);
   return 0;
}//Function definition
int product(int x, int y){
  return x * y;
}
```

## Void 函数返回类型

可用作不返回值的函数的返回类型

```
void printName(string name) {
   printf("Your name is: %s", name);
}
```

## 变量作用域

作用域是 C #中变量的生存期
这决定了变量何时可用

范围有两种主要类型:

*   局部变量
    ——在函数
    内部定义的变量——可以在变量声明之后访问，直到函数返回

```
void localScope(){
   int a = 20;
   printf("A locally is: %d", a);
}int main(){
   printf("Will cause an error, because a doesn't exist: %d", a);
}
```

*   全局变量
    ——定义在文件
    中函数之外的变量——可以在文件中的任何地方使用

```
int x = 20;int main(){
   printf("X is: %d", x); //X is: 20
   x++;
   doubleX();
   printf("X is: %d", x); //X is: 40
   return 0;
}void doubleX(){
  x*=2;
}
```

如果您还没有机会阅读我以前的文章，请看看:

*   [第一部分](/embedded-c-basics-i-17230dfb70a5)
*   [第二部分](/embedded-c-basics-part-ii-d8fc687b65e9)

这个系列的最后一部分将在下周出版。