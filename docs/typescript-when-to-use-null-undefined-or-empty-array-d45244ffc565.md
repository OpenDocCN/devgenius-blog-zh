# Typescript:何时使用 null、未定义或空数组？

> 原文：<https://blog.devgenius.io/typescript-when-to-use-null-undefined-or-empty-array-d45244ffc565?source=collection_archive---------0----------------------->

什么时候选择什么变体？

![](img/0773bcd78be01b779cf90df0a263848f.png)

乌孙挣脱枷锁。如此多不同的变体…

TypeScript 是强类型语言。

为了理解我们如何保护自己免受可空性问题的影响，我们需要首先理解 TypeScript 在处理 null 和 undefined 方面的设计方式。

Null 和 undefined 有不同的含义:

1.  某些东西还没有初始化，没有赋值:使用 undefined。
2.  某些内容当前不可用:请使用 null。

```
let name = {};     // no property defined
console.log(name); // undefined
```

值' undefined '表示一个变量已经被声明，但还没有被赋值。因此，变量的值是“未定义的”。

```
let name = undefined;
```

另一方面，‘null’指的是不存在的对象，基本意思是‘空’或者‘什么都没有’。

将“null”赋给变量，以指定该变量不包含任何值或为空。但是“undefined”用于检查变量在声明后是否被赋值。

**Null vs 空数组[] —** 返回空数组比返回 Null 好

我认为我们应该将 null 或 undefined 转换成[]，除非我们真的需要它是 null 或 undefined 的信息，但在这种情况下，我真的怀疑我们是否需要这个信息。这个简单的技巧将使代码变得容易得多，没有空检查，没有测试中要覆盖的额外分支，等等。这让我们的生活变得轻松了一些。

在 1 值的情况下，我们可以检查未定义的:

```
if (name !== undefined) { }
```

也足以用非常简短的方式写这张支票:

```
if (name) {}        // undefined, null and false check inside
```

**多种现代语言中可用的三元运算符—** if (a)？乙:丙

如果 **a** 不为空，则取 **b** 作为结果，否则取 **c** 作为余数。

```
const tariff = this.result?.tariff ? [this.result.tariff] : [];
```

**初始化空数组:**

```
const tariffs: Array<MyTariff> = new Array<MyTariff>();

const tariffs: Array<MyTariff> = [];   // as simplified syntaxconst tariffs: Array<MyTariff> = null; // unwanted behavior
```

当我们试图迭代数组时，空数组提供了稳定的行为，因为 null 或 undefined 将中断迭代。

```
this.result?.sellingTariffs?.forEach(item => {
  tariffs.push(item);
  console.log(item);
});
```

**操作员||或**

```
this.result.sellingTariff || true
```

对于 null 和 undefined 值，以及 false 值，将计算为 true。

```
this.result?.sellingTariffs || []      // if null return empty array
```

**空类型—** 表示只能取值为空的变量。

我们只能将 null 赋值给包含 null 变量的变量。它变得有用的地方是，我们可以用联合类型将多个变量的值赋给它。

```
// Defining nullable fields:
type MyString = {
  v: string | null;               // x could hold a value or a null
};
const resultStr: MyString = null; // or input string "Lorem ipsum";
console.log(resultStr);           // result null
```

**空值之间的差异&未定义**

Null 和 Undefined 看起来很相似，但它们之间几乎没有区别。

1.null 与==(相等检查)相比时等于未定义
null 与===(严格相等检查)相比时不等于未定义

2.当我们把 null 转换成一个数字时，它就变成了零。
当我们将 undefined 转换为 number 时，它变成 NaN

3.null 是 JSON 中的有效值。你可以把 undefined 表示成一个 JSON

## 检查未定义的

**typeOf** 也是一个经典的 JavaScript 方法，用于检查对象是否未定义:

```
if (typeOf tariff !== 'undefined') {
  console.log(tariff); // is now safe to use
}
```

同样通过使用等号运算符(`==`)和严格等号运算符(`===`)

```
let name: number | null | undefined;console.log(name)                       //undefinedconsole.log(typeof name)                //undefinedconsole.log(name ==null)  //true, == operator returns true for nullconsole.log(name === null)              //falseconsole.log(name == undefined)          //trueconsole.log(name === undefined)         //true
```

数组的 typeOf 返回它的类型对象。

```
const arrayTest = ["Test1", "Test2"];
console.log(typeOf arrayTest);           // object
console.log(arrayTest instanceOf Array); // true
```

检查空数组:首先我们检查未定义的，其次可以检查属性。

```
if (arrayTest && !arrayTest.length) { }
```

**操作员！—** 非空断言操作

这个操作符告诉编译器这个字段不是空的或者未定义的，但是它被定义了。

```
type Tariff = {
  name: string;
};let tariff: Tariff;
tariff = { name: "name" };  // initialize the object

console.log("Test it: ", tariff!.name);
```

但是这个操作符不会改变对象的值。如果值为空或未定义，这个操作符就帮不了我们。应用程序将被很好地编译，但无论如何都会发生异常。

原则上在 Kotlin 语言中是相当的，用两个！！

关税！！我们必须绝对确定这个对象不是空的或者未定义的。危险的假设。

**操作员！！在打字稿中**

通过添加`!!`，如果 customerData 为真，则表达式为`true`，如果 customerData 为假，则表达式为`false`，这就更容易管理了。

```
goToNextTab(): void {
  **if (!!this.customerData) {**
    this.router.navigateByUrl("myAnotherPage");
  }
}
```

而不是:

```
hasNextTab(): boolean {
  if(this.customerData) {
    return true;
  }
  return false
}
```

因为！！customerData 现在是一个布尔表达式，而 customerData 可以是任何东西。这种表达式将允许编写这样的函数来返回 true:

```
const isSelected = (tariffs: Array, selected: Tariff): boolean => {
  const found = tariffs?.find(tariff => tariff.Id === selected?.Id);
 ** return !!found;**
});
```

**无效合并运算符？？**同时检查**空**和**未定义**

在另一个值为`null`或`undefined`的情况下，`??`操作符可用于提供回退值。它有两个操作数，写成这样:

```
value ?? fallbackValue;
```

如果左操作数是`null`或`undefined`，则`??`表达式计算右操作数。

```
let result = null ?? "right value";
let result = undefined ?? "right value";
let lowercaseInput = input.toLowerCase() ?? ""; //if null, return ""
```

与科特林猫王算子有相同的逻辑？：

```
// take right operand if left is null
const result = first operand ?: second operand
```

![](img/31af7ed77942aaf69ab827a59848d11c.png)

华金·索罗拉，1910 年。“三名划手”

**结论**

TypeScript 从 JavaScript 发展而来。在基础层面上有很多相似之处。这两种语言正在并行发展。但在我看来，最好根本不要用 null，更喜欢 undefined。我还提到了 Kotlin 语言来展示与 null 斗争的相同方法。

**链接**

[](https://basarat.gitbook.io/typescript/recap/null-undefined) [## 空与未定义

### 记得我说过你应该用。你当然知道(因为我刚刚说了，^).不要把它用于根级别的东西。在…

basarat.gitbook.io](https://basarat.gitbook.io/typescript/recap/null-undefined) [](https://stackoverflow.com/questions/28975896/is-there-a-way-to-check-for-both-null-and-undefined) [## 有没有办法同时检查“空”和“未定义”呢？

### 我认为这个答案需要更新，检查旧答案的编辑历史。基本上，你有三个不同的…

stackoverflow.com](https://stackoverflow.com/questions/28975896/is-there-a-way-to-check-for-both-null-and-undefined)