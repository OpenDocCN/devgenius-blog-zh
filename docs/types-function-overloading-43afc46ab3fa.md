# TypeScript —函数重载

> 原文：<https://blog.devgenius.io/types-function-overloading-43afc46ab3fa?source=collection_archive---------7----------------------->

![](img/08f57d33b44c5cc395e86095d3598f6d.png)

格伦·卡斯滕斯-彼得斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

人们会期望 TypeScript 为 JS 提供函数重载，就像任何其他编译语言一样，例如 Java *和*它确实提供了函数重载，但是有些不同。

它不同于任何传统的编译语言，因为它不支持具有相同函数名的函数的多种定义/实现。如果 TS 找到多个同名的函数实现，它将抛出一个编译时错误，如图所示👇

```
TS code:// ❌ TypeError: Duplicate function implementation.
function isEmpty<T>(a1: T[], a3: T): boolean { ... };// ❌ TypeError: Duplicate function implementation.
function isEmpty <T>(a1: T, a2: T): boolean { ... };// ❌ TypeError: Duplicate function implementation.
function isEmpty<T>(a1: AnyLike<T>, a2: AnyLike<T>): boolean {...};Compiled JS code:function isEmpty(a1, a3) { return false; };function isEmpty(a1, a2) { return false; };function isEmpty(a1, a2) { return false; };
```

当 TS 将代码编译成 JS 时，它最终得到 3 个相同的函数，因为 JS 去掉了类型。TS 防止这种情况发生。

但是，TS 通过对参数进行类型检查来提供函数重载，并通过以下方式在代码中实现:

*   函数应该具有相同的名称和相同数量的参数。
*   一个函数可以有多个函数声明，但只能有一个函数实现。
*   实现的函数签名必须是其函数声明的超集/联合类型。

注意:具有相同名称和不同参数数量的函数不会被 ts 理解为重载函数。

让我们看一个例子。

我们定义了一个名为`AnyLike`的类型，它是泛型类型`T`和`T[]`的并集。这里 t 可以是一个复杂的物体，也可以是一个原语。

```
// Union type of generic type T and Array of T.
type AnyLike<T> = T[] | T;
```

现在，考虑一个名为`isEmpty`的函数，它接受两个参数并返回一个布尔值。参数的类型可以是`T`或`T[]`，分别如第 1 行和第 2 行所示。第三个是重载函数的实际实现，它接受类型为`AnyLike<T>`的参数——它需要是`T`和`T[]`的联合类型。

```
//1
function isEmpty <T>(a1: T[], a2: T[]): boolean;//2
function isEmpty <T>(a1: T, a2: T): boolean;// function implementation
function isEmpty<T>(a1: AnyLike<T>, a2: AnyLike<T>): boolean {
  // logic here
  return false;
}console.log(isEmpty<string>("foo", "bar")); // ✅ compiled!console.log(isEmpty<string>(["foo"], ["bar"])); // ✅ compiled!console.log(isEmpty<{}>({a: "foo"}, {b: "bar"})); // ✅ compiled!console.log(isEmpty<string>("foo", ["bar"])); // ❌ Oops!console.log(isEmpty<string>(["foo"], "bar")); // ❌ Oops!console.log(isEmpty<string>(["foo"], "bar")); // ❌ Oops!
```

让我们看看函数重载的另一个例子——在一个类中。我们也使用我们在上面创建的相同的示例类型`AnyLike`,

```
// overloaded function isEmpty
interface IEmptyType<T> {
  isEmpty(a1: T[], a2: T[]): boolean; isEmpty(a1: T, a2: T):  boolean; isEmpty(a1: AnyLike<T>, a2: AnyLike<T>):boolean;
}class Empty<T> implements IEmptyType<T> {
  //Function implementation
  public isEmpty(a1: AnyLike<T>, a2: AnyLike<T>): boolean 
    return false;
  }
} const instance = new Empty<String>();console.log(instance.isEmpty(["hi"], ["bye"]));  ✅ compiled!
console.log(instance.isEmpty(["a"], **[1,2]**)); ❌ Oops! // TypeError
```

# 外卖食品

函数重载可以在 TS 中实现，如下所示:

*   具有相同数目参数的同名函数。
*   函数参数可以有不同的类型和不同的返回类型。
*   函数实际实现的参数类型和返回类型必须是其函数声明的超集或联合类型。
*   方法重载可以被类似地实现，例如在一个类中或者在一个只有函数的类之外。