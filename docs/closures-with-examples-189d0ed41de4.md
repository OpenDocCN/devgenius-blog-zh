# 带示例的闭包

> 原文：<https://blog.devgenius.io/closures-with-examples-189d0ed41de4?source=collection_archive---------27----------------------->

![](img/79ccf8c3fe602438f94e002b3372024c.png)

在 Javascipt 中，闭包是最漂亮的部分，有时也是最复杂的部分。简而言之，闭包允许您从内部函数访问外部函数的范围。

闭包允许我们在封闭函数之外调用内部函数，同时保持对封闭函数词法范围(即封闭函数中的标识符)的访问。

看这个例子可以更好地理解:

```
function outer(){ 
    let value =10;
    return function(){
        console.log(value);
    }
}const innerFunction = outer();innerFunction();// 10
```

我们定义了一个**外部**函数，并从中返回一个函数。这里返回的函数是一个高阶函数。

我们调用外部函数并将其存储在 **innerFunction** 变量中。即使在外部函数返回后，innerFunction 仍保留该值。这是闭包在起作用。

> 函数是 javascript 中的一等公民。对其他函数进行操作的函数，无论是将它们作为参数还是返回它们，都被称为高阶函数。

让我们通过例子来看看闭包的作用。

## 1.一个函数被调用多少次？

检查`outer`函数的代码。请注意，我们正在返回一个函数，而该函数正在使用超出其作用域的`counter`变量。闭包是我们从外部作用域访问变量的方式。

## 2.循环迭代器

不要将它与生成器迭代器函数混淆。我们只是创建了一个函数，以循环的方式打印数组的值。

下面是它的代码:

断开`cyclicIterator`功能:

*   接受数组
*   有一个初始化为 0 的变量`index`,它将被用来在不同的函数调用中跟踪数组的索引。
*   返回内部函数。内部函数返回特定索引处的数组值，如果该索引是最后一个数组值；我们将索引重置为 0，以实现循环行为。
*   在第 11 行，我们用传入的数组调用`cyclicIterator`。这将运行函数并返回给我们内部函数。
*   内部函数的引用存储为`getDay`，这就是我们用来获取值的调用。

## 3.只调用一次函数

在这个例子中，我们将定义一个只被调用一次的函数。在任何进一步的调用中，我们将简单地返回先前的结果。

我们来分解一下上面的`once`功能:

*   接受函数`func`，该函数是在调用`once`时传递的匿名函数。
*   我们定义了一个`counter`变量来跟踪传入的函数是否被调用。我们还定义了一个`res`变量来存储函数`func`的响应。
*   在内部函数中，我们接受参数为`params`。然后我们检查这个函数是否被计数器变量调用过一次。如果是，我们简单地返回先前的结果，而不调用它，否则我们递增计数器，调用函数，并将结果发送回来。
*   我们通过调用`once`在第 14 行传递回调。它运行这个函数，并以`onceFunc`的形式返回给我们一个内部函数的引用。然后我们调用`onceFunc`来获得结果。

## **4。记忆**

![](img/9781b8633488e4958f38c2c516a8372e.png)

特雷弗·麦金农在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**记忆化**是一种优化技术，通过存储昂贵的函数调用的结果并在相同的输入再次出现时返回缓存的结果来加速应用程序。

让我们编写一个例子来更好地理解这一点。我们将举一个简单的例子，我们将把传入的值乘以 5。

函数如下:

```
function multiplyBy5(num){
    //we can think of this function to be doing some heavy computations
    console.log('Calculate');
    return num * 5;
}
```

现在如果我们调用这个函数，它会一直执行。

```
multiplyBy5(4);// 20
multiplyBy5(7);// 35
multiplyBy5(4);// 20
```

我们希望处理第三个调用，其中再次传递与第一个调用相同的输入。通过记忆化，我们希望通过不再调用函数来减少计算量。

让我们写一个记忆函数:

```
function memoize(fn){
    let map = new Map();
    return function(param){
        if(map.get(param)){
            console.log(‘Cached’);
            return map.get(param); 
         }else{
            const result = fn(param);
            map.set(param, result);
            return result;
         }
    }
}
```

这就是`memoize`函数正在做的事情:

*   接受先前定义为`multiplyBy5`功能的功能。
*   创建一个`Map`，它将存储任何先前计算值的结果并返回结果。
*   返回一个接受参数的函数。内部函数在执行时检查 map 是否有传递的参数值的结果；如果有，我们返回结果并且不计算。否则我们调用函数`fn`并在 map 中设置结果，最后返回结果。

下面是完整的代码:

观察上面的最后一个日志。它返回第一次`multiply`调用的缓存结果(在第 23 行),并且不再调用该函数。

# 结束语

在这篇文章中，我试图用例子解释闭包的一些用例。

如果你需要任何进一步的澄清，请在评论中告诉我。我很乐意帮忙。

# 资源

[](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) [## 关闭

### 闭包是捆绑在一起(封闭的)的函数与对其周围状态的引用的组合

developer.mozilla.org](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) [](https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99) [## 高阶函数(构成软件)

### 注:这是“作曲软件”系列的一部分(现在是一本书！)关于学习函数式编程和…

medium.com](https://medium.com/javascript-scene/higher-order-functions-composing-software-5365cf2cbe99)