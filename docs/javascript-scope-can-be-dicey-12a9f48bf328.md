# JavaScript 范围可能是不确定的

> 原文：<https://blog.devgenius.io/javascript-scope-can-be-dicey-12a9f48bf328?source=collection_archive---------9----------------------->

理解范围可以帮助你避免可怕的*代码味道。让我们剥离 JavaScript 中的作用域层——全局、局部、函数和块。*

![](img/e4324ec6f58bc2903cd3471146ff2343.png)

学习范围规则(尤其是 JavaScript)对任何新开发人员来说都是痛苦的，甚至会继续困扰经验丰富的开发人员。受*代码味道*困扰的调试代码通常会暴露出对作用域的肤浅理解(比臭名昭著的洋葱味还要糟糕)。我将暂停洋葱类比，直接切入正题。

在单个模块中工作时，理解最高级别的范围是一个很好的起点:

1.  作用域描述了程序的哪一部分可以访问特定的变量。
2.  一个变量可以有**全局**或**局部**作用域，这取决于它的声明位置和方式。

可以从程序中的任何地方访问全局变量。这意味着你可以从程序的任何地方检索和重新分配全局变量，而不会有任何问题。(嗯，这可能有点夸张…变量隐藏*会*影响你在外部作用域中访问变量的能力。你可以在这里阅读关于如何避免可变阴影[的更详细描述。)全局变量可以在程序中的任何地方声明，也可以用`let`或`const`关键字声明(`var`也是一个选项，但有自己的注意事项)，只要它们不是在函数或块中声明的。](https://medium.com/dev-genius/foxy-javascript-a647f9a18dc2)

```
let veggies = ['garlic', 'cabbage', 'celery'];if (true) {
  veggies.push('onion');
}console.log(veggies);

// ⟶ ['garlic', 'cabbage', 'celery', 'onion'];
```

在上面的例子中，变量`veggies`在全局范围内，被称为“全局变量”

非全局作用域的变量被称为具有**局部作用域**，局部作用域有两种形式:

1.  **函数作用域**:函数体内定义的变量被认为具有函数作用域，并且只能从该函数内部访问。
2.  **块范围**:块被认为是由左花括号和右花括号`{ }`组成的代码语句。我们看到最常使用`if/else`、`for`循环和`while`循环来定义块。

以下示例说明了函数范围:

```
let veggies = ['garlic', 'cabbage', 'celery', 'onion'];function chop(food) {    let dinner = food.slice(0, 2);

  veggies = food.slice(2); return dinner;
}chop(veggies);console.log(dinner);    // *ReferenceError: dinner is not defined*console.log(veggies);    *// ['celery', 'onion'];*
```

在上面的例子中，变量`veggies`在全局范围内，可以从代码中的任何地方访问，而变量`dinner`具有局部范围，因为它是在`chop`的函数体内声明的。请注意，我们可以在函数中访问和重新分配`veggies`，而这些更改在函数范围之外仍然存在。

对于块范围，它的工作方式类似:

```
let veggies = ['garlic', 'cabbage', 'celery', 'onion'];for (let veggie of veggies) {
  let firstLetter = veggie[0];
  let dinner = []; if (firstLetter !== 'c') {
    dinner.push(veggie);
  } else {
    let scrap = veggie;
    console.log(scrap);
  }
}console.log(dinner);    // *ReferenceError: dinner is not defined*console.log(scrap);    // *ReferenceError: scrap is not defined*
```

试图访问其本地范围之外的`dinner`(`for`块)会导致引用错误，因为`dinner`在该块之外是不可访问的。

注意，在上面的例子中，我们有两个块:一个`for`块和嵌套在其中的`if/else`条件块。就像剥洋葱的皮一样，您会看到变量的作用域是由**嵌套**进一步定义的:在嵌套函数或嵌套块的最内层声明的变量只能从该最内层访问。在上面的例子中，变量`scrap`只有在声明后才能在`if/else`块中访问。

虽然可以说变量的范围都是相对的，但它比理论或相对论更容易理解(我希望！).让我们试一试:

*   在最里面的块中声明的变量(比如`scrap`)可以从**任何**外部作用域中访问变量，包括全局作用域中的变量。
*   外部块**不能** 访问任何内部作用域(嵌套比那个块更深)中声明的变量。所以试图从全局范围访问`dinner`或`scrap`会抛出一个错误。

意识到外部作用域中的变量不仅可以被内部作用域访问，还可以被变异或重新分配，这很好。这是避免简短或模糊变量名的另一个重要原因。(当同一级别的两个函数或两个块使用相同的变量名时，这不是问题，也称为*对等作用域*，但这可能会使您的代码更难阅读。)

在处理需求时考虑范围也很重要。如果一个函数应该被设计为返回一个特定的变量，那么在考虑这些需求时，考虑作用域的分支是很重要的。在下面的例子中，应该在哪里声明`missingItems`数组？

```
let dinner = ['garlic', 'cabbage', 'celery', 'onion'];
let foodWeHave = ['cabbage', 'celery'];function makeGroceryList(meal, food) { for (let veggie of dinner) {
    if (!foodWeHave.includes(veggie)) {
       missingItems.push(veggie);
    }
  }
  return missingItems;
}console.log(makeGroceryList(dinner, foodWeHave));
```

**答:**变量`missingItems`应该在紧接着函数声明的那一行声明，因为它需要在函数作用域级别可访问，才能在 return 语句中可访问。(这也意味着嵌套的`for`和`if` 块可以访问它。)如果`missingItems`只需要作为`makeGroceryList`函数的一部分，它可能不需要在全局范围内。

像洋葱一样，范围可以减少一些眼泪。仔细跟踪变量的作用域，记住总是显式声明变量，并且在命名变量时要有意识，这有助于避免任何失败。最后，如果你还不熟悉可变阴影和全局范围污染这些概念，现在是时候重温一下了。重用变量名并在全局范围内声明大部分变量可能很诱人，但这可能会导致意想不到的错误，并使您的代码难以测试。你可以在这里找到这些陷阱的概要。

无论如何，理解 JavaScript 中的作用域将使您(和您的团队)远离可怕的代码味道，这种味道可能是由于不小心使用作用域而导致的。