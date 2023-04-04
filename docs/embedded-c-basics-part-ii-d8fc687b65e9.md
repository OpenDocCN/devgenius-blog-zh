# 嵌入式 C 基础:第二部分

> 原文：<https://blog.devgenius.io/embedded-c-basics-part-ii-d8fc687b65e9?source=collection_archive---------6----------------------->

## 打印、输入、运算符、条件和循环

![](img/813838fd1701125229a68d0c9193a582.png)

过山车循环

## 打印变量

如我上一篇[文章](https://dmarcr1997.medium.com/embedded-c-basics-i-17230dfb70a5)中的例子所示，printf 函数向控制台输出一个字符串。

这有它的用途，但是为了让这个函数更加模块化，我们来谈谈格式说明符

格式说明符将字符串的一部分替换为包含在 printf 参数中的变量

```
Format Specifiers:
╔══════════════╦════════════════════╗
║   Specifier  ║       Type         ║ 
╠══════════════╬════════════════════╣
║ %f           ║ Floating point     ║ 
║ %c           ║ Character          ║ 
║ %d           ║ Decimal            ║ 
║ %s           ║ String             ║
╚══════════════╩════════════════════╝int x = 20;
int y = 10;printf("The value of x is %d", x);
//Output: The value of x is 20
x+=1;printf("The value of x is %d,\nThe value of y is %d", x, y);
//Output: The value of x is 21
//The value of y is 10
```

如果省略变量参数，字符串将打印出说明符类型的大小。

## 读取输入数据

使用 scanf 功能读取用户输入。这个函数是 stdio 库的一部分，所以您需要导入它。

```
#include<stdio.h>
int main(){
   int x;
   float y; printf("Enter the integer: ");
   scanf("%d", &x); //(format specifier, reference to variable)
   // Sets x = to the users input without any need for conversion
   // the & is a reference to the variable x
   // this allows us to directly change the value of x from the 
   // scanf function printf("Enter the float: ");
   scanf("%f", &y);}
```

## 经营者

**算术**

*   单向—一个操作数/变量
    —增量:++
    —减量:“-”

```
//Can be postfix ++x or prefix x++
//Prefix inc/dec the variable first and then returns x, so
int x =10;
int y = ++x;
// y = 11
//postfix returns the variable and then inc/dec variable
int x = 10;
int y = x++
// y = 10
```

*   双向-接受两个操作数(x，y)
    –求和:x+y
    –减法:x-y
    –乘法:x * y
    –除法:x/y
    –模数，余数:x % y

**按位**

用于操作位

下表将显示不同的按位运算符

A 和 B 是可以为假(关、低、0)或真(开、高、1)的位

并且结果是给定 A 和 B 的算子的输出

```
AND Table:
╔═══════╦══════╦════════════════════╗
║   A   ║   B  ║       Result       ║ 
╠═══════╬══════╬════════════════════╣
║   0   ║   0  ║         0          ║ 
║   0   ║   1  ║         0          ║ 
║   1   ║   0  ║         0          ║ 
║   1   ║   1  ║         1          ║
╚═══════╩══════╩════════════════════╝OR Table:
╔═══════╦══════╦════════════════════╗
║   A   ║   B  ║       Result       ║ 
╠═══════╬══════╬════════════════════╣
║   0   ║   0  ║         0          ║ 
║   0   ║   1  ║         1          ║ 
║   1   ║   0  ║         1          ║ 
║   1   ║   1  ║         1          ║
╚═══════╩══════╩════════════════════╝XOR Table:
╔═══════╦══════╦════════════════════╗
║   A   ║   B  ║       Result       ║ 
╠═══════╬══════╬════════════════════╣
║   0   ║   0  ║         0          ║ 
║   0   ║   1  ║         1          ║ 
║   1   ║   0  ║         1          ║ 
║   1   ║   1  ║         0          ║
╚═══════╩══════╩════════════════════╝NOT Table:
╔═══════╦══════╗
║   A   ║   B  ║
╠═══════╬══════╣
║   0   ║   1  ║
║   1   ║   0  ║
╚═══════╩══════╝int x = 0b1010 // binary variables
int y = 0b0101int andOp = x & y; // 0
int orOp = x | y; // 1111 = 15
char notX = ~x; // 0101 = 5
char notY = ~y; // 1010 = 10
int xorOp = x ^ y; // 1111 = 15
```

移位运算符

*   左移位操作器<<
*   right shift operator >>
*   这些操作将把一个二进制数的位(将在以后的文章中介绍)在它的[寄存器](https://chortle.ccsu.edu/assemblytutorial/Chapter-10/ass10_4.html)中向左或向右移动一个指定的数字

```
 int x = 0b00001010; // 10 decimal
x >> 1; // shift bits to the right onceOriginal 8 bit register
╔═══════════════════════════════════════════════╗
║  0  ║  0  ║  0  ║  0  ║  1  ║  0  ║  1  ║  0  ║
╚═══════════════════════════════════════════════╝Right Shifted 8 bit register by 1
╔═══════════════════════════════════════════════╗
║  0  ║  0  ║  0  ║  0  ║  0  ║  1  ║  0  ║  1  ║
╚═══════════════════════════════════════════════╝int y = 0b00001010; 
y << 1;Left Shifted 8 bit register by 1
╔═══════════════════════════════════════════════╗
║  0  ║  0  ║  0  ║  1  ║  0  ║  1  ║  0  ║  0  ║
╚═══════════════════════════════════════════════╝Will convert the value from binary back to integer 
```

**分配**

```
int x = 20;
x += 3; // same as x = x + 3 or 23
x -= 5; // same as x = x - 5 or 18
x *= 2; // same as x = x * 2 or 36
x /= 4; // same as x = x / 4 or 9x %= 4; // remainder of 9/4 or 1
int y = 0b00001010;y &= 1; // 1010 & 1 = 0b00001011
y |= 15; // 1011 | 15 = 0b00001111
y ^= 2; // 1111 ^ 0010 = 0b00001101 
```

**关系和逻辑**

*   任何不等于零的数字都被认为是真的
*   零被视为假

```
x == y //true if x is equal to y
x != y //true if x is no equal to y
x > y //true if x is greater than y
x < y //true if x is less than y
x >= y //true if x is greater or equal to y
x <=y //true if x is less than or equal to yx && y //true if x and y are true
x || y //true if x or y are true
!x //true if x is false
```

**条件语句**

在特定条件下执行操作

在 C 语言中有两种主要类型

*   如果阻止

```
int x = 20;
if(x >= 20){
  printf("X is greater than or equal to 20");
} else if (x < 20) {
  printf("X is less than 20");
} else {
  printf("X is undefined");
}
//only required part of a conditional is the if (condition)
//statement, the rest is optional
```

*   Switch 语句

```
int x = 3
switch(x){ case 1:
   printf("X equals 1");
   break; case 2:
   printf("X equals 2");
   break;
  case 3:
   printf("X equals 3");
   break;
  default:
   printf("X is a different number");
   break;
}
// the case values must be constant and only used once 
// (no variables)
// the default case is optional
// need to include break at end of case or the switch statement will
// run every case after the one that is met
```

**循环**

*   对于循环

```
for(int i = 0; i < 20; i++){
   printf("Hello: %d", i);
} 
// for(variable initializer; condition; incrementer/decrementer)
// {Statement}
// Code above will print out hello 20 times with the number 0-19 in
// place of %d
```

*   While 循环

```
int x = 10;
while (x != 100){
   printf("Not yet");
   x += 10;
}
printf("x is now at 100");
// Run until the condition is false
// while(condition){statements}
```

*   Do-while 循环

```
char choice = 'y';
do {
  printf("Do you want to keep going: );
  scanf("%c", &choice); }while(choice == 'y')
printf("Terminating");
//will loop once and then check the conditional 
```

我希望你们都喜欢《嵌入式 c 语言基础》的第二部分。
如果你们喜欢，这是第一部分和第三部分。