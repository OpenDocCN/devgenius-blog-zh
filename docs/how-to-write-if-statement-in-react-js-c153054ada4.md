# React.js 中 if 语句怎么写？

> 原文：<https://blog.devgenius.io/how-to-write-if-statement-in-react-js-c153054ada4?source=collection_archive---------8----------------------->

![](img/02dba6d733eda2845707142680adf8cd.png)

[如何在 React.js 中写 if 语句](https://www.blog.duomly.com/6-most-popular-front-end-interview-questions-and-answers-for-beginners-part-2/#how-to-write-if-statement-in-react-js)

本文原载于[https://www . blog . duomly . com/6-most-popular-front-end-interview-question-and-answers-for-初学者-part-2/# how-to-write-if-statement-in-react-js](https://www.blog.duomly.com/6-most-popular-front-end-interview-questions-and-answers-for-beginners-part-2/#how-to-write-if-statement-in-react-js)

— -

[react . js 中的 If 语句](https://youtu.be/pl9DI9v_8-I)

当然，如果我们考虑 Javascript 或 Typescript 逻辑中的 if 语句，它在每个 Javascript 或 Typescript 地方都是一样的。

只是 if/else 像纯 javascript 一样，但在这种情况下，我们就不说普通的 if/else 了。

在 react 中，我们还需要一个 if 语句，那就是渲染。

它被命名为“条件渲染”，但为了简单起见，让我们继续使用“react 中的 if 语句”。

我们将在 React.js 代码中看到使用条件渲染的两种最流行的方法，根据具体情况，这两种方法都是正确的。

我们可以使用的第一种方法是直接在组件布局中定义条件呈现。

我们将最大限度地使用它，而且在某些情况下，它是性能最好的。

我们将在只有一个条件的情况下使用这种方式，更像是“如果”，当指定的条件通过时，我们希望呈现一些元素。

第二种方法是创建函数来处理布局的指定部分，并有条件地呈现它，为此我们不仅可以使用 if/else，还可以使用 switch case。

这种方法在我们有更多条件的情况下使用是正确的，特别是带有开关 1 的版本。

但是它无论如何都会触发函数，所以当我们有一个条件时使用它是没有意义的。

让我们来看看代码示例，我在其中添加了这两种方法:

```
// The first example with the code inside functional component
function Parent(props) {
 return(
 <>
 {name === “Duomly” && (
 <DuomlyComponent/> 
 )}
 </>
 )
}// The second example with the additional function
function renderComponent() {
 const name = ‘Duomly’;
 if (name === ‘Duomly’) {
 return ‘Duomly’;
 } else {
 return null;
 }
}function Parent(props) {
 return renderComponent();
}
```

![](img/03d18bac6abce30b3e43642dbc2e8504.png)

[Duomly —编程在线课程](https://www.duomly.com/?code=lifetime-80)

感谢您的阅读，
来自 Duomly 的 Radek