# 嵌入式 C 基础 I

> 原文：<https://blog.devgenius.io/embedded-c-basics-i-17230dfb70a5?source=collection_archive---------5----------------------->

## 什么是 C 语言，为什么要学习 C 语言

c 是一种中级语言。这意味着它高于低级的机器可读语言(汇编语言)和更接近于书面语言的高级语言(python、ruby 等)。)

如果你打算从事计算机网络、编译器设计、计算机体系结构、操作系统或嵌入式系统，那么 C 是一个很好的起点。因为这种语言更接近于低级代码，所以这些项目并没有从语言中抽象出来。

用 C 编写上述主题的程序的一个主要原因是由于 C 语言的快速运行时间。c 没有额外的处理开销，而是让程序员来跟踪这些项目。这个博客系列将涵盖嵌入式 C 的基础知识、数据类型、控制结构、标准输入/输出、文件输入/输出、数学库、问题解决、函数、数组、动态内存和指针。

![](img/7388ab69ce54deca406d029b34960e4e.png)

电脑:屏幕上的你好

# 你好世界

有许多编译器和代码编辑器，但是如果你想不费吹灰之力就开始编写 C 语言，从这里开始。

现在您的设置让我们从一个简单的 C 程序开始

c 程序由小块代码或函数组成
下面的例子

```
#include<stdio.h>
// Includes the stdio library into the program so we can use printfint main(void) {
   printf("Hello World");
   return 0;
}
```

只有一个函数，main，它在程序执行时运行

要运行这个文件
，打开你最喜欢的编译器和终端运行
g++ filename.c
。/a

# 评论

注释是用于文档、解释程序逻辑或可变目的的代码行。

要用 C 编写注释，请执行以下操作

```
//Single line comment
/* Multi
-
Line
Comment */
```

# 字符串和转义字符

可以包含文本、字母、空格、特殊字符和数字的字符集

在 C #中，字符串放在" "之间。上面使用的 printf 函数接收一个字符串以打印到控制台。

```
Escape Characters:
╔══════════════╦════════════════════╗
║   Operator   ║       Use          ║ 
╠══════════════╬════════════════════╣
║ \n           ║ New Line           ║ 
║ \t           ║ Horizontal Tab     ║ 
║ \v           ║ Vertical Tab       ║ 
║ \"           ║ Double quote       ║ 
╚══════════════╩════════════════════╝#include<stdio.h>
#include <stdlib.h>int main(){
   char ary[]="Name or Other Info";
   printf("%s", ary);
   return 0;
}
```

如上所示，要在 C 中初始化一个 a 字符串，你需要创建一个字符数组，并立即将其设置为一个值

```
char ary[]="Name or Other Info";
```

或者给它一个固定的长度

```
char nameString[10]; //can only hold strings with ten characters
char nameString[6] = {'H', 'e', 'l', 'l', 'o', '\0'}; 
// Hello string with null character
```

要从键盘输入中获取字符串，您可以执行以下操作

```
#include<stdio.h>
#include <stdlib.h>int main(){
   char str[50]; printf("-----------------------------------\n");
   printf("Input the string : ");
   fgets(str, sizeof str, stdin); 
   // set string storage location, size of the string, 
   // and the input stream used printf("The string you entered is : %s\n", str);
   printf("-----------------------------------\n");
}
```

对于某些 num 输入，字符串也可以转换为各种数字类型:

```
#include<stdio.h>
#include<stdlib.h>
int main ()
{
	const int size = 123;
        char buffer[size];
        char buffer2[size];
        char buffer3[size]; unsigned long ul;
        int in;
        float flo; printf ("\nInput a unsigned long number: "); fgets(buffer,size,stdin);  
        printf ("\nInput an integer number: ");
        fgets(buffer2,size,stdin); printf ("\nInput a float number: ");
	fgets(buffer3,size,stdin);

	ul = strtoul (buffer,NULL,0); //convert to unsigned long
        in = atoi(buffer2); //convert to integer
        flo = atof(buffer3); //convert to float printf ("Output1: %lu\n",ul);
        printf ("Output2: %i\n",in);
        printf ("Output3: %f\n\n",flo); return 0;
}
```

# 变量

变量可以被认为是保存一段数据的地方

C 语言的基本数据类型有

浮动类型:

*   浮点型— 4 字节
*   double — 8 个字节

小数类型

*   字符— 8 个字节
*   int — 1 个字节

要定义变量

```
char x = 'A';
int y; 
// no initial value will hold undefined value(garbage) until
// initialized
float z = 6.43;
```

当我们在内存中初始化一个变量时，会发生以下步骤:

*   编译器将进入 RAM 并给出一个变量名的位置
*   然后，它会将数据存储在这个位置
*   根据数据类型的不同，变量在 RAM 中会占用不同的空间

— 1 字节=1 个位置，8 字节= 8 个位置

## C 的命名约定

*   变量名的第一个字符应该包含字母或下划线符号
*   不允许使用空格和逗号
*   不应是保留字—由编译器使用
*   不允许使用特殊符号(不包括下划线)
*   变量不能在同一代码块/范围内重复

可能包含以下内容

*   强调
*   数字:0–9
*   字母:a-z，A-Z

如果您喜欢本书并想了解更多，请继续阅读第二部分