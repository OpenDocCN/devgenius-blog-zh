# 节点中的异步方法链接

> 原文：<https://blog.devgenius.io/async-method-chaining-in-node-8c24c8c3a28d?source=collection_archive---------6----------------------->

![](img/90e1875b9268f7eb29de56d5dab04ae7.png)

链式连接方法< == >链式连接栅栏

# 什么是方法链？

我们来谈谈方法链。这是一种贯穿整个编码过程的模式，也是我在 Javascript 中最喜欢做的事情之一。如果您熟悉这个概念，请随意浏览这一部分。

下面是一个让我内心感觉很棒的代码示例:

```
numbers = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20];numPrimes = numbers
   .map(n => Math.pow(2, n) % n)
   .filter(n => n !== 2)
   .length
```

这是一个非常好的方法来找出一个数字列表中有多少个素数。如果我们真的想变酷，我们应该把上面的 pow 方法改成更有用的方法。然后我们会发现超级大数是否是质数。但那是以后的事了。

这里有一个例子，我们可以用另一种方式来写:

```
numbers = [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]; numPrimes = 0;
**for** ( **let** i = 0; i < numbers.length; i++ ) {
   calc = Math.pow(2, numbers[i]) % numbers[i];
   **if** ( calc === 2 )  {     numPrimes++;   }
}
```

这个例子的目的是展示我们如何能够使用某些类方法的令人敬畏的属性。在这个例子中，我们使用了一个数组类，它在对象原型中有各种各样的好东西。对于那些不知道原型是什么的人来说，姑且说它是一个东西能做什么的定义。开箱即用，我们的 Array 类将允许我们使用 map、reduce、filter 和许多其他功能。这些方法做了很多伟大的事情，但我最喜欢的部分是它们返回一个数组。这意味着无论返回什么，我们都可以继续调用数组方法。让我们通过一个简单的例子来更好地描述这一点。

```
array = [1,2,3,4]; *// the result is a brand new array* 
odds = array.filter(n => n % 2 === 1);  
*// output :: [1,3]* *// filter keeps your original array intact* 
evens = array.filter(n => n % 2 === 0); 
*// output :: [2,4]* moreEvens = evens.map(n => n * 2);      
*// output :: [4,8]* *// if you think really hard, you may be able to see that evens is just a placeholder* 
*// we know filter comes back with and we wont use it later, why not do this:* 
moreEvens = array.filter(n => n % 2 === 0).map(n => n * 2);
```

# 我如何使用这个模式？

基于我们上面所回顾的，这种模式将很容易像 pi 一样在您自己的类中重新创建。神奇酱:解决这个问题！

现在，不要误解我的意思，这是 Javascript 中任何人都不应该轻视的一个关键字。这无疑是(对我来说)最令人困惑的事情。但是在这里，我们要放轻松，不要做任何疯狂的事情。让我们构建一个可以链接方法的类。

```
class Chainer {
   foo(x)  {
     this.value = x
     return this;
   } bar()  {
     this.value += 42
     return this;
   }

   baz()  {
     this.value -= 13
     return this;
   }

   result()  {
     return this.value;
   }
}

chainer = new Chainer(); 
value = chainer.foo(15) // foo assigns         
               .bar()   // bar adds
               .baz()   // baz subtracts
               .result() // output :: 44
```

我们不需要过多讨论这个。这里有两个想法。

首先，我们在方法结束时返回这个事实是关键。关键字指的是最直接的包含范围。在我们的例子中，它是链条。在我们每个方法的末尾返回它意味着它们解析为 Chainer 对象，这个对象又定义了我们所有的方法。有道理，对吧？

我们在我们的对象上保存数据。在面向对象的世界中，这被称为实例变量。这让我们可以在对象中保存数据的上下文。这样，我们使用我们的方法在我们的上下文上运行一系列转换。我们的 Chainer 可以认为是一个管道，这正是我们想要的效果！

# 等等，异步呢？

哦，伙计，如果你在考虑异步方法，你一定是在游戏的顶端！到目前为止，我们都很放松。但是为了真正很好地利用这个模式，我们必须施加一些压力。毕竟是 Javascript 不管我们喜欢与否，都将会有异步调用。

我们都熟悉异步呼叫和承诺。以下是使用它们的两种方法:

```
*// this makes a new Promise and will resolve after 1 second* asyncWait = **async** () => **new** Promise(resolve => setTimeout(resolve, 1000 )); *// this is extra credit; google IIFE if youre curious* 
(**async** () => { *// here we will make the process pause until the function resolves
*   **await** asyncWait();
   console.log('1');

   *// here we give a handler that runs when the function resolves* 
   *// but the process moves along without waiting
 *  asyncWait().then((res) => {
     console.log('2');
   })   
   console.log('3'); })(); 
*// output :: 1 3 2*
```

有了这两种使用异步调用的形式，我们将如何链接呢？在我们深入研究这个之前，我需要提到一些事情来解释为什么我们不能使用上面的常规链接。每当我们在定义一个函数时使用异步，它总是会在一个承诺中返回一些包装器。这意味着我们返回这个的方法实际上会在一个承诺中返回这个。反过来，这意味着我们必须等待它解决，否则我们不能链！

让我们试一试:

```
**class** AsyncChainer {
   constructor()  {
     **this**.waits = 0;
   }

   wait = **async** (x) => {
     **await** asyncWait(); *// defined in a past example
*     console.log(**this**.waits++);
     **return** **this**;
   } 
} (**async** () => {
   **const** chainer = **new** AsyncChainer(); *// this actually works, how gross
*   **await** (
     **await** (
       **await** (
         **await** (
           **await** chainer.wait()
         ).wait()
       ).wait()
     ).wait()
   ).wait();
   console.log('Finished awaits');

   *// this also works
*   **await** chainer.wait()
     .then(c => c.wait())
     .then(c => c.wait())
     .then(c => c.wait())
   console.log('Finished thens'); 
})(); *// output :: 0 1 2 3 4 Finished awaits * 
*//           5 6 7 8 Finished thens*
```

这些例子确实很糟糕。第一个看着真的很痛苦。我真的不喜欢把它打出来。但果然有效。我们只是确保将每个表达式嵌套在括号中，这样我们就可以等待下一个嵌套表达式解析成什么。不过，看起来有点像递归。我们不害怕递归；不，我们是朋友！

第二个看起来更好一点。但是仍然没有纯链式方法清晰。我们可以等到一切都完成了，这肯定是有利的。两种格式都不能做的一件事是互相传递参数。跨链链接获取数据的唯一方法是使用我们上面谈到的实例变量。

但是，如果我们能更进一步呢？如果我们把两者的优点结合起来，再增加更多，会怎么样呢？有时候你必须做一些别人不敢做的事情！

```
*class BestChainer {
*   constructor()  {
     **this**.waits = 0;
     *// this is the beginning of the chain
*     **this**.queue = Promise.resolve();
   } *// lets define the 'then' function we have been using
*   *// now our AsyncChainer can do what Promises do!
*   then(callback) { callback(**this**.queue); } *// shedule a callback into the task queue
*   *// basically builds the following:
*   *// o.then(o => o.then(o => o.then(o =>o.then(Promise.resolve())))*       
   chain(callback) {
      **return** **this**.queue = **this**.queue.then(callback);
   } *// one way to pass value
*   chainedAsyncWait = () => {
     **this**.chain(**async** () => {
       **await** asyncWait();
       console.log(**this**.waits++);
     });
     **return** **this**;
   }

   *// another way to pass value
*   chainedAsyncCalc = (y) => {
     **this**.chain((x) => {
       console.log(x);
       **if** (!x) { x = y; }
       **return** x * x;
     })
     **return** **this**;
   }
}

(**async** () => { **const** chainer = **new** BestChainer();
    x = **await** chainer
          .chainedAsyncWait()
          .chainedAsyncWait()
          .chainedAsyncWait()
          .chainedAsyncWait()
   console.log('Finished chains', x); x = **await** chainer
          .chainedAsyncCalc(2)
          .chainedAsyncCalc()
          .chainedAsyncCalc()
          .chainedAsyncCalc()
   console.log('Finished chains', x);

})(); 
*// output :: 0 1 2 3 Finished chains undefined* 
*//           undefined 4 16 256 Finished chains 65536*
```

这多整洁啊！我们能够使用我们整洁的小链接模式，并引入一种新的沿管道传递数据的方式！先说说这个为什么管用。

首先我们实现一个 then 方法。这使得我们的 BestChainer 类可以使用 wait。对我们来说，确保我们的代码等待整个链被解析是非常重要的。这是一个可选功能，不需要包含，它不会影响下一个属性。

接下来我们实现一个链式方法。在内部，我们告诉我们的队列，不管它是什么，在它解析时运行一个给定的回调(使用 then from Promise，而不是我们刚刚编写的那个)。告诉队列这样做的结果是一个新的承诺，它成为我们队列的下一部分。从这里，您可以想象队列向外扩展的情况:

```
o4.then(
  o3 => o3.then(
    o2 => o2.then(
      o1 => o1.then(
        Promise.resolve()
      )
    )
  )
)
```

仔细想想，这很像递归！我们将承诺推给彼此，而不是调用堆栈。最后，我们有一个告诉它停止运行的基本案例。注意这是在构造函数中设置的，非常重要，否则你的代码将不会运行。

另一个很酷的特性是，我们正在链接的这些承诺将解析为一个值。这意味着此链中的任何给定链接都将能够从它之前的链接接收返回值(第一个链接将是未定义的)。

我想知道的一件事是，我们如何能找到一种方法来阻止链执行。我以前尝试过解决这个问题，但除了使用 throw，我似乎找不到其他方法。这对于负面的情况是完美的，例如一个错误。但是，如果我们已经满足了一些条件，并且已经做了足够的工作，那会怎么样呢？一个回归式的退出链条会很棒。我必须对这种事情是如何发生的做更多的研究。

# 我能在现实世界中看到这种模式的应用吗？

在文章的这一点上，我必须对读者说实话。我围绕这个模式进行思考和编码的原因是为了探索如何以不同的方式使用 Javascript。大家都用 Express 写了一个 API。有了我的公司使用的这样一个可靠的 API REST 模式，我当然会尽我所能去遵循它。

路由器->控制器->服务依赖注入简直太棒了。如果我记得/找到一篇文章，我会在这里放一个链接。本质上，这个想法是路由器使用调用服务方法的控制器。控制器持有在应用程序上下文中有意义的人/业务逻辑。控制器方法由计算机/系统逻辑的小服务方法组成。这里有一个例子:

*   您的应用程序有一个名为/renewId？员工=hgKing
*   路由器具有为 IdController 调用 renew 方法的逻辑
*   在 renew 方法中有许多不同服务中的数据库调用:IdService、EmployeeService 等。
*   不同部分的逻辑堆栈尽力更新员工 ID。

应用系统对数据库调用、实体、模型有深入的了解。这些细节是您希望隐藏在控制器和服务中的东西。这是 API 模式最棒的部分。

然而，我的一部分想知道如何改进它。我注意到的一点是路由器->控制器接口。我想看看是否有办法提高控制器的可视性。您可以想象在路由器中有这样的代码:

```
app.get('/renewId', (req, res) => {
   **try** {
     res.send(
       idController.renew(req.query.employee)
     );
   } **catch** (e) {
     res.send(e.message);
   }
})
```

很直接，对吧？这里有很多好东西。简单的黑盒实现；try catch 是从服务器获取错误消息的一个好方法。但是，你可以想象更新方法是巨大的。里面到底发生了什么？难倒我了。我们如何解决这个问题？

为什么当然是连锁！想象一个这样的世界:

```
app.get('/renewId', (req, res) => {
   **try** {
     res.send(
       idController.startRenew(req.query.employee)
                   .isValidEmployee()
                   .hasId()
                   .generateId()
                   .updateRecords()
                   .results()
     );
   } **catch** (e) {
     res.send(e.message);
   } 
})
```

这有多好？我们可以看到各种原本隐藏的重要信息。这些位中的每一位都是一种方法，减少了大量的逻辑块。如果您在函数中使用了良好的实践，您可以随时删除或重新排序链中的任何链接。在任何时候，这些链都可能抛出错误，比如 isValidEmployee()可能会抛出 InvalidEmployeeError(“用户 hgKing 不是有效的雇员”)。在我眼里周围都很棒！我最近一直在尝试在我的项目中实现这样的东西，稍后我会回到这篇文章，做一个附录来报告更多的发现。

感谢阅读！