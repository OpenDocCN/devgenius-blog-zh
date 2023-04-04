# 使用 Typescript 进行类型转换

> 原文：<https://blog.devgenius.io/types-transformations-with-typescript-167d807adf25?source=collection_archive---------2----------------------->

关于类型用法的一些实用想法。

![](img/857bf10b9c4f82296f4c9d122dd8f660.png)

Typescript 有一些附加的保留关键字，允许基于其他类型设置新类型。流传最广的是推断、省略、挑选、记录。

让我们从头开始。

**Typescript 中的泛型—** 它是依赖于另一个类型的类型。例如，我们有一个类型位置。我们用某个参数< T >对其进行参数化。

让我们写许多类型参数，用逗号分隔。我们在调用它时添加必要的值。

```
interface Coordinate<Lat, Lng, Title> {
  latitude: Lat;
  longitude: Lng;
  label: Title;
}
const myLocation: Coordinate<number, number, string> = {
  latitude: 59.436962,
  longitude: 24.753574,
  label: 'Tallinn',
};
console.log(myLocation);
```

类型关系。
例如，对于字符串类型，我们有这样的依赖关系:

未知(任何类型的超类型)->字符串->从不(任何类型的子类型)。

包含所有可能的值->字符串->不会或不应该有值

借助关键字“extends ”,我们可以通过超类型来限制参数。例如，我们使用带有 1 个参数的泛型类型。并让它成为 string 的子类型。

```
function getBilliards<T extends string>(s: T): [T] {
  return [s];
}
// argument of this type is assignable to parameter of type <...>
const result = getBilliards<'carom' | 'snooker' | 'pool'>('carom');
console.log(result); // ["carom"]
```

条件类型—它类似于类型的三元运算符。

条件修剪对于缩小联合类型也很有用。标准库包括从联合类型中移除了`null`和`undefined`的`NonNullable<T>`类型。

```
type MyNonNullable<T> = T extends null | undefined ? never : T;
const result: MyNonNullable<1> = 1;
console.log(result);
const resultStr: MyNonNullable<'lorem ipsum'> = 'lorem ipsum';
console.log(resultStr);type MyNonNullable<T> = T extends null | undefined ? T : never;
const result3: MyNonNullable<null> = null;
console.log(result3);
```

“从不”、“未知”和“任何”是有意义的运算符:

1.  “从不”用于表示从未发生的事情。尤其是在条件类型中删除不需要的情况。
2.  “未知”包含所有可能的值，但它可能有任何类型。
3.  “任何”是非常特殊的情况。如果你不需要检查属性类型或者你不知道它(当从第三方库获取数据时)，" any "将不会检查它们的类型存在。

generics and never conditional type:infer fetch type or 不应该是值。

```
type ListItem<T extends any[]> = T extends (infer X)[] ? X : never;
type stringItems = ListItem<string[]>;const result: stringItems = '1';
const result2: stringItems = 2; //number is not assignable to string
console.log(result);type typesItem = ListItem<[string, number]>;
const different: typesItem = 'Lorem Ipsum';
console.log(different);
const different2: typesItem = 1.618034;
console.log(different2);
```

1.  **推断**

使用`infer`，编译器确保你已经显式声明了所有类型变量*。*

```
*type R = { a: number };
type MyType<T> = T extends infer R ? R : never;
  // infer new variable R from Ttype T1 = MyType<{b: string}>;    // T1 is { b: string; }const result: T1 = {
    b: 'Alex'
}
console.log(result);    //string*
```

*没有`infer`，编译器不会知道，你是否想要引入一个额外的类型变量`R`，或者`R`只是一个偶然的输入错误。*

```
*interface Billiard {
    pool: string
}type MyBilliard<T> = T extends Billiard2 ? 'snooker' : 'caromball'; // mistake: Billiard2 not declared yettype MyBilliard<T> = T extends infer Billiard ? Billiard : never;
type AnotherBilliard<T> = T extends Billiard ? Billiard : never;type myPool = MyBilliard<{lengthTable: number}>
type myPool2 = AnotherBilliard<{pool: string}> // assign string only// try use properties
const result: myPool = {
    lengthTable: 360
}
console.log(result);const result2: myPool2 = {
    pool: 'Pyramid'
}
console.log(result2);*
```

*检查函数类型:如果 T 类型扩展了一个函数，ReturnType 将推断函数结果的类型，否则将返回 never 类型。*

```
*type MyReturnType<T> = T extends (...args: unknown[]) => infer R ? R : never;
function getMobile() {
  return {
    platform: "default",
    version: "1.0",
  };
}
type Mobile = MyReturnType<typeof getMobile>;
const result: Mobile = {
  platform: "Android",
  version: "2.0"
};
console.log(result);*
```

***2。省略***

*从 3.5 版本的 Typescript 开始，我们有了标准的省略关键字。以前的版本允许通过选择和排除关键字的组合来实现省略行为。*

*Omit <t k="">通过从 T 中选取所有属性然后删除 k 来创建一个类型。换句话说，Omit 是一个通用的实用程序类型，它删除 k 中指定的 T 的键。映射 DTO 是使用 Omit 的常用位置。</t>*

```
*interface Fish{ 
    property1?: string; 
    property2?: string; 
    property3?: string; 
}
// omit single property 
type Fisher = Omit<Fish, 'property3'>;// omit multiple properties
type Fisher = Omit<Fish, 'property2' | 'property3'>;const result: Fisher = {property2: 'fish'}; //not assignable to typeconst resultWorks: Fisher= {property1: 'fisher'};
console.log(resultWorks);*
```

*另一个带有挑选和省略逻辑的示例:随时添加新属性*

```
*interface Fisher {
    fullname: string;
    email: string;
}
interface FisherNew extends Pick<Fisher, 'email'> { }

type MyFisher = Omit<Fisher, keyof FisherNew> & { // Fisher only
    experience: number;
} 
const result: MyFisher = {
    experience: 10,
    fullname: 'Alex',
    //email: '' // does not exist in type MyFisher
}
console.log(result);*
```

***3。选择** —当需要重用一个类型中的属性时*

*Pick 节省了我们为了复制现有类型的属性而创建新类型的时间。*

*`Pick`做与`Omit`相反的事。Pick < T，K >通过从 T 中选取一组属性 K 来构造一个类型*

```
*interface User {
  id: number;
  name: string;
  phone: string;
  age: number;
}

type Person = Pick<User, 'id' | 'name'>;const myPerson: Person = {
  id: number;
  name: string;
  phone: string; //error
}// or we can declare type manually without using a Pick keyword
type Person = {
  id: number;
  name: string;
}*
```

***4。Partial** —使所提供接口的所有属性都是可选的*

*它将对象的所有属性设置为可选，并允许您只考虑更新的键。*

```
*interface Profile {
  id: number,
  username: string,
  phone: string
}const updateWithPartial: Partial<Profile> = {
  username: 'Alex'
};const instance: Profile = {
  id: 1,
  username: 'my Name',
  phone: '(+xxx) xxxx xxx xxx'
};* 
```

***5。Readonly** —处理不可变数据。*

*像 **readonly** 和 **const** 这样的关键字是使用属性的另一种情况: **readonly** 用于类属性， **const** 用于变量。*

*Readonly 逻辑非常简单:接受它接收的类型，并将它的所有属性标记为只读。如果重新分配返回类型的属性，这将导致引发编译错误。*

```
*interface Work{
  start: Date,
  end: Date, 
  comments: string
  status: boolean
}
interface State {
  myWork: Readonly<Work>[]
}
const task: Readonly<State> = {
  myWork: [
    { status: false, comments: 'What has been done?' },
  ]
};//task.myWork[0].status = true; // impossible, due to a read-only// ability to change it:
const myNewWork = {
  myWork: [
    {...task.myWork[0], status: true }, ...task.myWork.slice(1)
  ]
};
console.log(myNewWork);*
```

***6。记录** —在构造配置类型时非常有用。*

*记录允许您从联合中创建新的类型。联合中的值用作新类型的属性。*

```
*type SampleProps = Record<'property1' | 'property2', Cat>;// in other words, it is the same as:
type SampleProps = {
  property1: Cat;
  property2: Cat;
}*
```

*带枚举的示例:*

```
*enum Billiard {
    Pool, Snooker, Carom, Pyramid
}
function getBilliardName(c: Billiard): string {
  const getNames: Record<Billiard, string> = {
    [Billiard.Pool]: 'American Pool',
    [Billiard.Snooker]: 'English Snooker',
    [Billiard.Carom]: 'French Carom ball',
    [Billiard.Pyramid]: 'Russian Pyramid'
  }
  return getNames[c] || '';
}
getBilliardName('Pyramid');*
```

*另一个购物例子:*

```
*type ProductId = string;
type Status = 'preOrder' | 'inStock' | 'soldOut';
interface Availability {
    availability: Status;
    amount: number;
};const x: Record<ProductId, Availability> = {
    '1001': { availability: 'inStock', amount: 5},
    '1002': { availability: 'preOrder', amount: 10 },
    '1003': { availability: 'soldOut', amount: 17},
};
console.log(x['1002']);*
```

***结论**:*

*实用程序类型只是让我们的代码更干净，可读性更好。它可以使应用程序更加结构化，简化我们的工作并减少工作量。肯定是不可能用 JavaScript 实现的。*

*[](https://www.typescriptlang.org/docs/handbook/utility-types.html) [## 文档-实用程序类型

### TypeScript 提供了几种实用工具类型来促进常见的类型转换。这些实用程序可用…

www.typescriptlang.org](https://www.typescriptlang.org/docs/handbook/utility-types.html)  [## TS Playground——探索 TypeScript 和 JavaScript 的在线编辑器

### Playground 让你以一种安全和可共享的方式在线编写类型脚本或 JavaScript。

www.typescriptlang.org](https://www.typescriptlang.org/play)*