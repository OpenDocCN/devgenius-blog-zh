# 异步等待你爱的承诺

> 原文：<https://blog.devgenius.io/async-await-ing-for-your-loving-promises-98a892332769?source=collection_archive---------14----------------------->

![](img/fe19934c0049beeade4c43b8c176d6e7.png)

等待别人充满爱意的承诺是非常困难的，但在 JavaScript 中却不是这样；感谢 JavaScript 中 async/await 奶油般的语法糖。在本文中，我们将看到编写承诺的演变，从 then()和 catch()方法到超级棒的 async/await 语法。

真爱在等待，JavaScript 承诺也在等待。Promises 在 JavaScript 中提供了很多东西，从回调地狱回到提供一堆有用的方法来承诺异常处理的链接特性。但是在你的心里仍然有一个小故障，它提醒你你实际上是在处理一个异步操作，它不可能是同步的，甚至看起来也不是。

async/await 关键字处理了 JavaScript 中的承诺。现在你的承诺可以很棒，同时看起来也很迷人。在本文中，让我们来看看它们是如何工作的。

这篇文章将构成前两篇文章的一些代码逻辑，一篇是关于[回调](https://codeomelet.com/posts/why-your-crush-and-javascript-does-not-callback-anymore)，另一篇是关于[承诺](https://codeomelet.com/posts/making-promises-in-love-and-javascript)。如果你已经对 JavaScript 的承诺感到满意，就没有必要去查看它们了。

在开始编写代码之前，让我们为 async/await 设置一些基本规则，如下所示:

1.  Await 关键字只在异步函数和顶级模块体中有效。
2.  使用 try and catch 块处理承诺的解析和拒绝。

# 异步/等待

事不宜迟，看看下面这个超级复杂的 promise 例子，没什么，只是一个简单的偶数查找函数，如下所示:

```
const isEven = (number) => {
    return new Promise((resolve, reject) => {
        // super complex math problem
        if (number % 2 === 0) {
            resolve();
        } else {
            reject();
        }
    });
};

isEven(4)
    .then(() => console.log("Yay! it's even"))
    .catch(() => console.log("Meh! it's odd"));
```

到目前为止，生活还不错，但异步/等待触摸会更好。我们可以简单地用 await 关键字替换 then 和朗朗上口的方式，如下所示:

```
await isEven(2);
console.log("Yay! it's even");
```

上面一行肯定会给出一个错误，因为我们有一个没有异步的 await。因此，如果你在代码的顶层，想调用一个返回 promise 对象的函数，并且想使用 async/await 方式，只需使用[life](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)(立即调用函数表达式)。

```
(async _ => {
    await isEven(2);
    console.log("Yay! it's even");
})();
```

现在，上面的代码可以工作，但不适用于奇数，因为我们不处理承诺的拒绝，我们将面临一个异常。让我们将 await 调用包装在一个 try-catch 块中，如下所示:

```
(async _ => {
    try {
        await isEven(2);
        console.log("Yay! it's even");
    } catch {
        console.log("Meh! it's odd")
    }
})();
```

现在，我们已经成功地按照 async/await 规范重写了代码。

# 异步/等待解决和拒绝

让我们继续回忆一下[上一篇文章](https://codeomelet.com/posts/making-promises-in-love-and-javascript)中的一个例子；其中我们编写了一个基于 factorial finder promise 的方法，该方法将 number 作为参数，并返回一个解析为 factorial 的 promise，或者在 number 格式不正确的情况下返回一个错误。

```
const factorial = (number) => {
    return new Promise((resolve, reject) => {
        if (typeof number === 'number') {
            let fact = 1;
            for (let i = 1; i <= number; i++) {
                fact *= i;
            }
            resolve(fact);
        } else {
            reject('The number provided must be of type number');
        }
    });
};(async _ => {
    try {
        const result = await factorial(5);
        console.log('Factorial:', result);
    } catch (error) {
        console.log('Error:', error)
    }
})();
```

非常简单，你的异步代码将慢慢看起来像同步代码。

# 异步/等待承诺方法

很明显，您可以用 async/await 方式轻松地使用 promise 方法。下面我们简单地创建了一堆承诺，并用 Promise.all()方法将这些承诺组合成一个单元；通过将 await 放在 Promise.all()方法之前，我们可以简单地等待所有异步任务完成。

```
const p1 = new Promise((resolve, reject) => setTimeout(_ => resolve('value 1'), 3000));
const p2 = new Promise((resolve, reject) => setTimeout(_ => resolve('value 2'), 500));
const p3 = new Promise((resolve, reject) => setTimeout(_ => resolve('value 3'), 2000));const promises = [p1, p2, p3];(async _ => {
    try {
        const result = await Promise.all(promises);
        console.log('result:', result)
    } catch (error) {
        console.log('error:', error)
    }
})();// result: [ 'value 1', 'value 2', 'value 3' ]
```

我们同样可以使用所有的 promise 方法(allSetteleted，any，race)。

# 异步/等待承诺链

承诺链已经在[之前的文章](https://codeomelet.com/posts/making-promises-in-love-and-javascript)中展示过，所以我们将选择相同的场景。但是首先检查以下异步调用:

```
const db = require('./db')const fetchUserById = (id) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const results = db.users.find(i => i.id === id);
            results ?
                resolve(results) :
                reject('Not found');
        }, 500);
    });
};const fetchPostsByUserId = (userId) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const results = db.posts.filter(i => i.userId === userId);
            results.length ?
                resolve(results) :
                reject('Not found');
        }, 500);
    });
};const fetchCommentsByPostId = (postId) => {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            const results = db.comments.filter(i => i.postId === postId);
            results.length ?
                resolve(results) :
                reject('Not found');
        }, 500);
    });
};
```

我带来了来自[的前一篇文章](https://codeomelet.com/posts/making-promises-in-love-and-javascript)的相同代码，它解释了上述异步调用的承诺链:

```
let result;fetchUserById(1)
    .then(user => {
        result = user;
        return fetchPostsByUserId(user.id);
    })
    .then(posts => {
        result.posts = posts;
        return Promise.all(result.posts.map(i => fetchCommentsByPostId(i.id)));
    })
    .then(comments => {
        result.posts.forEach(post => post.comments = comments.flat().filter(i => i.postId === post.id));
        return result;
    })
    .then(result => console.log('User:', result))
    .catch(error => console.log('Error:', error));
```

现在，让我们用 async/await 来修饰上述代码逻辑，如下所示:

```
(async _ => {
    try {
        const user = await fetchUserById(1);
        const posts = await fetchPostsByUserId(user.id);
        const comments = await Promise.all(posts.map(i => fetchCommentsByPostId(i.id)));user.posts = posts.map(post => ({
            ...post,
            comments: comments.flat().filter(i => i.postId === post.id)
        }));
        console.log('User:', user);
    } catch (error) {
        console.log('Error:', error)
    }
})();
```

像冰淇淋一样甜，我们首先获取用户，然后是用户的帖子，然后是借助 Promise.all()方法获取每个帖子的评论。唉，我们把评论数组和它们各自的帖子拉平，形成最终的用户对象。

简单地说，在使用 async/await 时，我们需要记住以下几点:

1.  await 关键字只在异步函数中起作用，所以如果你的函数在顶层，那么你可以使用异步生命
2.  try 块用于解决的承诺，而 catch 用于拒绝的承诺
3.  您也可以使用 finally 块

[下载代码](https://github.com/ZakiMohammed/javascript-async-await/archive/master.zip)

[Git 存储库](https://github.com/ZakiMohammed/javascript-async-await)

# 摘要

自从我开始了解 async/await，我就投身其中；我找到了每一种按照 async/await 规范编码的可能方法。这不是什么花哨的编码方式，但它甚至可以让复杂的代码看起来容易理解。我见过一些混乱的回调地狱，甚至承诺被束缚得太多，以至于失去控制。除了友好之外，如果你已经是一个有 JavaScript 承诺的专业人士，这很容易理解。async/await 是魔法棒在你的 promise 代码上挥动后的效果。

希望这篇文章有所帮助。

*最初发表于*[*【https://codeomelet.com】*](https://codeomelet.com/posts/async-await-ing-for-your-loving-promises)*。*