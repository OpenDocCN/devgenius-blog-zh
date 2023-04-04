# Typescript 在骗你吗？

> 原文：<https://blog.devgenius.io/is-typescript-lying-to-you-a2761f6bef4?source=collection_archive---------3----------------------->

你有没有想过为什么使用 Typescript 有时会让你如此头疼？我指的不是偶尔需要付出巨大努力来满足 TS 编译器的事实。不，我的意思是偶尔完美的工作代码在没有*的情况下停止工作，这是一个容易识别的*原因。你的 API 代码是*臭名昭著的嫌疑犯*之一，不是吗？

我经历过很多次——我的 API 一改变，我就开始注意到奇怪的错误或不寻常的应用程序行为。归根结底，不能保证 TS 类型在运行时与 JS 类型相同。到目前为止，Typescript 还没有以经典意义上的“类型”为特色。

Typescript 包含**类型注释**，而不是我们习惯于传统静态类型语言的真实类型。

机制“**不是类型，而是类型注释**”代表了 Typescript 和 Java 或 C#的本质区别。由于 TS 类型通常在编译后被剥离，所以没有对 TS 类型的运行时支持。

事实上，这就是 TS 失去其美丽和实用性的地方。

例如，我喜欢 C#中的反射。我认为这是一个新的魔术水平的便捷工具。没有这样的基础设施支持，动态语言就不能提供可靠的运行时串行化器。缺少了这部分功能，您的应用程序就包含了一个重要的**黑洞**，在那里您的应用程序行为可能会被破坏。

一个经典的例子是 API 调用；您有这样的代码:

```
interface User{
   Age: number;
}
```

对于服务器上有缺陷的数据，您可能很快就会得到一个“形状”像用户的对象，但是 Age 属性可能被设置为字符串、未定义的对象，甚至是一个复杂的对象。

**C#序列化器防止这些类型的错误，但是 ts(单独使用 JS 运行时)不能做到这一点。**这是我对 TS 最大的担忧。由于来自外界的坏数据，它经常对你撒谎。在静态编程中，运行时的行为就像你的*礼貌的朋友*，它会阻止坏数据感染应用程序的其余部分。在 JS 中，运行时就像一个不喜欢你的敌人，不断地试图破坏你的代码、努力，甚至你的个人生活:)。

## **帮助 JS 运行时**

为了支持 Typescript，您必须站在运行时的角度考虑问题。这通常意味着创建或找到工具来支持代码中的“类型”。

从你的 APIA 生成类型脚本类是一个很好的例子。理想的解决方案似乎是有一个 OpenAPI 格式的 API。然后，您可以使用这些软件包:

[https://open API-generator . tech](https://openapi-generator.tech/)

或者

【https://github.com/manifoldco/swagger-to-ts 号

在建立了这个工具之后，您就能够定期同步您的 API 和 Typescript 生成的类。我相信找到一种方法将你的 API 和你的 Typescript 类更紧密地联系起来，将会在未来给你*带来巨大的好处*。

**2)集成测试**

这是另一个提示。创建集成测试来检查您的 API 最近是否没有改变。简单地**做任何静态类型运行时免费为你做的事情**。

只需使用您最喜欢的测试运行程序，编写执行 API 调用的测试，并做出断言，以确定 API 是否没有被修改。

例如，在 JEST 中你可以使用:

```
const entity = await fetch(‘yourApi/getUser’)
const userModel = {
  Age:expect.nullOrAny(number),
  Name:expect.nullOrAny(string)
}
expect(entity).toEqual(expect.objectContaining(userModel));
```

3)过分严格

一种不同且细致的方法建议将 API 类/接口中的所有属性注释为可选。然后，您必须在代码中的任何地方检查它们的值。这似乎是一个非常费力的过程，你写了许多可能不必要的“如果”然而，考虑到罕见的边缘情况，我想这可能是有帮助的——但这仍然不是我喜欢的方式。而且，它没有解决**类型不匹配**。

4)拥有带有实验性元数据 API 的串行化器

当我说没有办法在运行时拥有 TS 类型时，我并没有说得很准确。我进行了一个小实验，用我的 TS 类和 Typescript 中的实验性反射创建了我自己的“序列化程序”。然而，这只是一个概念，还没有在我的任何项目中使用过。

事情是这样的:

```
import ‘reflect-metadata’
function prop(target : any, key : string) {
   var t = Reflect.getMetadata(“design:type”, target, key);
   console.log(`${key} type: ${t.name}`);
}export class User {
  @prop age: number;
  @prop name: string;
}let user = new User();console.log(Reflect.getMetadata(‘design:type’, user, “age”).name == “Number” ); //returns trueconsole.log(Reflect.getMetadata(‘design:type’, user, “age”).name == “String” ); //returns true
```

使用该代码，您可以检查对象的字段是否具有正确的类型。这段代码仅仅展示了一个片段来说明这个概念，我将在以后的文章中详细阐述我自己的序列化器。

我还怀疑 TS 中的反射过去和现在都相当有限，而且不知何故与装饰者奇怪地结合在一起。另一个困扰我的问题是，它目前只是一个 Typescript 特性。因此，举例来说，如果你像我一样使用 Babel 编译器，它没有内置支持。
*不过，还有一个* [*的变通办法*](https://github.com/leonardfactory/babel-plugin-transform-typescript-metadata#readme) *，当然是:)。*

在我看来，JS 运行时静态类型化的时机还没有到来。然而，我坚信这将是 TS/JS 世界中的另一件大事。

概括一下*，*是的，Typescript 倾向于对你撒谎，但它不是故意这样做的。显然，我觉得这是网络发展不可避免的事实。为了获得更多的支持，您必须稍微模拟一下运行时。构建一个小的基础设施来增强您在 javascript 中的*静态类型* *努力*可能总是有用的，值得一试。