# 在 Javascript[基础—第 1 部分]中，当你面临循环条件时，有很多选择

> 原文：<https://blog.devgenius.io/a-lot-of-options-when-you-face-a-looping-condition-in-javascript-basic-part-1-dda8d76afaf1?source=collection_archive---------13----------------------->

## 这么多的循环条件供你选择解决你的情况。

![](img/0a48bbdff76fcfec23008571e9d2e82c.png)

[亨利&公司](https://unsplash.com/@hngstrm?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

大家好，我是 Pandhu Wibowo，我是一名软件工程师。欢迎来到我的博客。

![](img/80591ddb4b48163f39ddad695a1fdd8c.png)

照片由[爆裂](https://unsplash.com/@burst?utm_source=medium&utm_medium=referral)上[未爆裂](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天，我将讨论作为一名 Javascript 开发人员在面对循环条件时的许多选择。

Javascript 中有很多选择循环。你真的应该小心你的任何循环条件。

![](img/6063cf0acbce9c517be3301ad351b07f.png)

照片由 [Srinivas JD](https://unsplash.com/@kirisrini?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

循环条件通常用于处理对象选择或数组内部。您可以搜索值、重新创建数组、查找对象内部的值等。如果你是 Javascript 初学者，你应该知道这些。因为有太多的选择了。所以，让我们开始吧。

## 为

第一，语句的**是你应该知道如何处理迭代的标准。`for`循环更复杂，但也是最常用的循环。**

for 语句创建一个由三个可选表达式组成的循环，这三个表达式用括号括起来并用分号分隔，后面跟一个要在循环中执行的语句(通常是一个 [block 语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block))。

```
for (begin;condition;step) {
  // your statements
}
```

举个例子，

```
for (let i=0; i<10; i++) {
  console.log(i)
}//Output:
0
1
2
3
4
5
6
7
8
9 
```

让我们一部分一部分地分解它。

*   **开始**，是初始化。循环的开始。
*   **条件**，有值**真**或**假**。如果条件为**假**，循环将停止。
*   **步骤，如果值始终为**真**，则**为重复条件。

例如在上面，

```
for (let i=0; i<10; i++) {
  console.log(i)
}
```

1.  我们有变量 **i** ，类型为 **let (begin)** 。
2.  **条件**有本人< 10。意思是我应该小于 10。
3.  **步**，如果 I 小于 10(真)，在 **i 大于 10** 期间反复运行。

## 为每一个

forEach 是普通循环中的 for … of 或 for 等函数。循环值、索引或数组值的另一种方式。

举个例子，

```
const number = [1,2,3,4,5]number.forEach(value => {
  return value
})// 1
// 2
// 3
// 4
// 5
```

我有这个循环的例子，如果在这个循环中有任何异步进程，不要使用这个函数。

即使您已经设置了“await”语句，在这个过程中也会跳过它。

## 为了…的

我真的经常用这个功能。因为什么？因为使用这个函数，与使用常规 for 相比，你的代码简单易读。看起来更容易使用。

举个例子，

```
const data = [
  {
    firstname: "Pandhu",
    lastname: "Wibowo",
    age: 17
  }
]for (const val of data) {
  console.log(val.firstname)
  console.log(val.lastname) 
  console.log(val.age)
}
```

你可以直接访问而不用索引参数，这样可读性更好。

## 发现

最后但同样重要的是， **find 函数**返回满足所提供的测试函数的第一个数组元素的值。

**find()语法**

`find()`函数的语法是:

```
arr.find(callback(element, index, arr),thisArg)
```

示例:

```
const ages = [18, 19, 20, 21]const findAge = (age) => age === 18const age = ages.find(findAge)// 18
```

# 警惕！

如果你们碰巧来自**印度尼西亚**并且想要支持我写更多，希望你们能从钱包里贡献一点。可以通过一些方式分享你的天赋，谢谢。

# 萨韦里亚

[https://saweria.co/pandhuwibowo](https://saweria.co/pandhuwibowo)

![](img/a74147716a509d8e6915e203333f8daf.png)

# 特拉克特尔

[https://trakteer.id/goodpeopletogivemoney](https://trakteer.id/goodpeopletogivemoney)

![](img/2728b5ff39237800335355f187993718.png)

## 多读我的文章

[](/learn-ci-cd-with-github-actions-to-deploy-a-nestjs-app-to-heroku-8feb715d3ce7) [## 使用 GitHub 动作学习 CI/CD，以将 Nestjs 应用程序部署到 Heroku

### 在 Nestjs 中使用 GitHub Actions 轻松部署到 Heroku。为了避免无聊的任务。:)

blog.devgenius.io](/learn-ci-cd-with-github-actions-to-deploy-a-nestjs-app-to-heroku-8feb715d3ce7) [](https://javascript.plainenglish.io/deploy-nestjs-app-on-heroku-also-connected-to-google-cloud-sql-postgresql-f0a085fea4be) [## 在 Heroku 上部署 NestJS 应用程序，同时连接到 Google Cloud SQL (PostgreSQL)

### 如何在 Heroku 上部署您的 NestJS 应用程序并将其连接到 Google Cloud SQL (PostgreSQL)的指南。

javascript.plainenglish.io](https://javascript.plainenglish.io/deploy-nestjs-app-on-heroku-also-connected-to-google-cloud-sql-postgresql-f0a085fea4be) [](https://javascript.plainenglish.io/tips-for-making-interfaces-strong-with-utility-types-in-typescript-5775cd5b6f53) [## 使用 TypeScript 中的实用工具类型增强接口的技巧

### TypeScript 中实用工具类型的介绍。

javascript.plainenglish.io](https://javascript.plainenglish.io/tips-for-making-interfaces-strong-with-utility-types-in-typescript-5775cd5b6f53) [](https://javascript.plainenglish.io/lets-hunting-the-pokemons-w-fetch-api-javascript-1a95d30b0d10) [## 让我们用 JavaScript 获取 API 来寻找口袋妖怪

### 大家好！今天我列了一个口袋妖怪的清单。收集了这么多真的很兴奋。好的，跟我来…

javascript.plainenglish.io](https://javascript.plainenglish.io/lets-hunting-the-pokemons-w-fetch-api-javascript-1a95d30b0d10) [](https://medium.com/easyread/error-how-to-solve-graphqlfieldextensions-and-friends-its-derivatives-when-use-graphql-and-718a5d318fd1) [## 错误:如何解决“GraphQLFieldExtensions and friends”——当使用 Graphql 和…

medium.com](https://medium.com/easyread/error-how-to-solve-graphqlfieldextensions-and-friends-its-derivatives-when-use-graphql-and-718a5d318fd1) [](https://javascript.plainenglish.io/what-is-void-0-in-javascript-bdd4b3eb19a7) [## JavaScript 中的“void 0”是什么？

### 类似于未定义吗？到底该不该回避？

javascript.plainenglish.io](https://javascript.plainenglish.io/what-is-void-0-in-javascript-bdd4b3eb19a7) 

# 参考

 [## JavaScript for...循环的

### 在本教程中，您将了解...在例子的帮助下。

www.programiz.com](https://www.programiz.com/javascript/for-of) [](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) [## for - JavaScript | MDN

### 初始化表达式(包括赋值表达式)或变量声明在循环前计算一次…

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for)