# React.js 中如何使用 loop？

> 原文：<https://blog.devgenius.io/how-to-use-loop-in-react-js-822f99e8ac6f?source=collection_archive---------11----------------------->

![](img/07f9593a684ea94cd34c035b592d7086.png)

[React JS 中如何使用 loop？](https://www.blog.duomly.com/6-most-popular-front-end-interview-questions-and-answers-for-beginners-part-2/#how-to-use-loop-in-react-js)

本文原载于[https://www . blog . duomly . com/6-most-popular-front-end-interview-question-and-answers-for-初学者-part-2/# how-to-use-loop-in-react-js](https://www.blog.duomly.com/6-most-popular-front-end-interview-questions-and-answers-for-beginners-part-2/#how-to-use-loop-in-react-js)

[如何在 ReactJS 中使用 loop？](https://youtu.be/DZ3n3nGxF_U)

像 if/else 语句一样，当我们想要在 JavaScript 或 TypeScript 逻辑中执行循环时，我们不需要考虑任何特殊的规则。

它只是一个 JS 循环，一如既往，我们可以使用所有类型的循环(当然，并不是所有的循环都适用于所有情况，但这是可能的)。

无论如何，我们有一个特殊的原因，为什么我们在为 React.js 开发应用程序时应该关注迭代方法。

我们使用迭代方法来渲染元素。例如，我们可以使用迭代来呈现产品数组中的整个产品列表。

为此，我们可以使用一些方法，其中最常用的是 map 方法，但我们将在单独的部分中讨论 map，现在我们应该关注其他方法，如循环或 forEach 方法。

使用像 for-loop(大多数情况下是最快的)、for-in 或 for-of 这样的循环来迭代元素是非常流行的。

当我们使用单独的函数来呈现组件的一部分时，这种方法很有用，并且是性能最佳的方法。

我在示例中包含的第二个方法是带有 array.forEach()的方法。

与 for-loops 和 map 方法相比，这种方法是最慢的，并且不像 map 那样返回值，所以您需要有特殊情况才能使用它。

让我们看一下带有两个 for-loop 和 forEach 方法的代码示例:

```
// First one with For of loop
function renderComponent() {
 const products = [‘orange’, ‘apple’, ‘watermelon’];
 const list = []
 for (const [i, product] of products.entries()) {
   list.push(<li>{product}</li>)
 }
 return (
   <div>
     {list}
   </div>
  )
}function Parent(props) {
 return renderProducts();
} // Second with forEach method
function renderComponent() {
  const products = [‘orange’, ‘apple’, ‘watermelon’];
  const list = []

  products.forEach((product) => {
    list.push(<li>{product}</li>)
  }) return (
    <div>{list}</div>
  )
}function Parent(props) {
 return renderProducts();
}
```

![](img/03d18bac6abce30b3e43642dbc2e8504.png)

[Duomly —编程在线课程](https://www.duomly.com/?code=lifetime-80)

感谢您的阅读，
来自 Duomly 的 Radek