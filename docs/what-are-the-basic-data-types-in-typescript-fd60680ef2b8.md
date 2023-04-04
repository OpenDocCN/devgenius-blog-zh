# Typescript 中的基本数据类型是什么？

> 原文：<https://blog.devgenius.io/what-are-the-basic-data-types-in-typescript-fd60680ef2b8?source=collection_archive---------12----------------------->

![](img/f6923c16e08fcb271e6fbabac8637de9.png)

solmaz hatamian 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

TypeScript 是 JavaScript 的超集，所以有些数据类型是从它继承的。但是 TS 引入了许多更有用的数据类型。让我们在下面的列表中分解它们。

1.  **布尔**:最简单的数据类型，用来表示一个逻辑值，只能有两个值:`true`或`false`。它与 JavaScript 布尔原语类型相同。
    `let isBoolean: boolean = true;`
    `let isLoading: boolean = false;`
2.  **数字**:和 JavaScript 中一样，TypeScript 中的所有数字都是浮点值。除了十六进制和十进制文本，TypeScript 还支持 ECMAScript 2015 中引入的二进制和八进制文本。
    `let decimal: number = 6;`
    `let hex: number = 0xf00d;`
    `let binary: number = 0b1010;`
    
3.  **字符串**:是文本数据类型，它代表以 Unicode UTF-16 代码存储的字符序列。就像 JavaScript 一样，TypeScript 也使用双引号`"`或单引号`'`将字符串数据括起来。TypeScript 中的
    `let str: string = 'this is a string';` 也可以使用*模板字符串*，可以跨多行，有内嵌表达式。这些字符串由反斜线/反引号(```)字符包围，嵌入表达式的形式为`${expr}`。
    `let name: string = 'Riccardo';`
    `let age: number = 39;`
    `let sentence: string = `Hi my name is ${name}.`
    `I will be age ${age + 1} next year.`;`
4.  **数组**:和 JavaScript 一样，数组类型可以用两种方式之一编写。在第一种方式中，使用元素的类型后跟`[]`来表示该元素类型的数组:
    `let arrayOfNumbers: number[] = [1, 2, 3];` 第二种方式使用通用数组类型，`Array<elemType>` :
    `let numbers: Array<number> = [1, 2, 3];`
5.  Tuple:这种类型允许开发者用固定数量的元素来表示一个数组，这些元素的类型是已知的，但不需要相同。例如，您可能希望将一个值表示为一对`string`和`number` :
    `let x: [string, number] = ['Hello World', 13];`
6.  Enum :这是 TypeScript 的一个新特性，也是对 JavaScript 标准数据类型的一个有益补充。`enum`是一种给数值集取更友好名称的方式。
    `enum Colours { Red, Green, Blue};`
    `let colour: Colours = Colours.Green;` 默认情况下，`enum`从`0`开始对成员进行编号。但这可以通过手动设置其中一个成员的值来改变，或者设置`enum` :
    `enum Colours { Red = 1, Green = 5, Blue = 20};` 中的所有值。这意味着使用这些值可以获得示例中所示颜色的名称:
    `enum Colours { Red = 1, Green = 5, Blue = 20};`
    `let colorName: string = Colours[5];`
7.  **Any** :当开发人员不知道从应用程序或第三方库返回的值的类型，或者每次调用函数时值都会动态变化时，`any`是一个占位符，用于选择退出类型检查，并让值通过编译时检查。
    T17
    
8.  **Void** :这通常是没有任何类型。开发人员通常会将它用作不返回值的函数的返回类型。
9.  **空**和**未定义**:这些是所有其他类型的子类型。这意味着你可以将`null`和`undefined`分配给类似`number`的东西。
10.  **Never** : `never` type 表示从不出现的值的类型。例如，`never`是函数表达式或箭头函数表达式的返回类型，它总是抛出异常或从不返回；当被任何不可能为真的类型守卫缩小时，变量也会获得类型`never`。
    `never`类型是每种类型的子类型，并可分配给每种类型；然而，*没有*类型是`never`的一个子类型，或可分配给【】(除了`never`本身)。甚至`any`也不能分配给`never`。
11.  **对象** : `object`是一个表示非原始类型的类型，即不是`number`、`string`、`boolean`、`bigint`、`symbol`、`null`或`undefined`的任何东西。
12.  **类型断言**:当开发人员比 TypeScript 更了解某个值时，*类型断言*是告诉编译器信任指定值的一种方式。它对运行时没有影响，纯粹由编译器使用。TypeScript 假定程序员已经执行了他们需要的任何特殊检查。
    类型断言有两种形式。一个是“尖括号”语法:
    `let variableName: any = 'Hello World';`
    `let strLenght: number = (<string>variableName).length;` 另一个是`as`语法:
    `let variableName: any = 'Hello World';`
    `let strLenght: number = (variableName as string).length;`

这篇文章想尽可能地提供更多的信息，对于每个人来说，官方文档是必须的。