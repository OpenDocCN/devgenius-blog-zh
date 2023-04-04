# 通过斐波纳契数列、记忆和制表介绍动态规划

> 原文：<https://blog.devgenius.io/an-introduction-to-dynamic-programming-through-the-fibonacci-sequence-memoization-and-tabulation-67a8624be61a?source=collection_archive---------2----------------------->

![](img/5aa4484a5e6e4758b2fa29c701ad7619.png)

罗斯·斯奈登在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

最近我一直在学习算法和数据结构，同时努力准备技术面试。有些事情来得容易，但我总是喜欢好的挑战。我喜欢白板上的问题，感觉就像一个你很快就能拼起来的拼图。在某个时候，它会在你的大脑中一闪而过，你就知道你需要做什么了。例如，我真的很喜欢我的第一个使用斐波那契数列的问题。如果您不熟悉，一般的斐波那契问题可能是这样的:

```
You have an array with the following values: 
array = [0,1,1,2,3,5,8,13,21,34]Given any index, n, determine what the value at that index in the array would be, array[n].
```

当时，我没有接触过这个序列，所以这个问题对我来说很难解决。挑战结束后，我想更好地理解序列的解决方案。经过一番练习，我得出了以下结论:

```
The relationship between values in the array is:
array[n] = array[n-1] + array[n-2]function fibonacci(n){
   if(n <= 2) return 1
   return fibonacci(n-1) + fibonacci(n-2)
}
```

我对这个解决方案很满意。它对我来说是有意义的，通过了我的测试，并利用了我刚刚学到的递归(如果你想重温递归，请查看我的博客文章[这里](https://medium.com/dev-genius/an-introduction-to-recursion-in-programming-b38f25bee843))。后来，我意识到我的解决方案有问题。如果我用大于 35 的数字运行我的解决方案，它需要一段时间来返回值，如果我用大于 50 的数字运行它，它会冻结我的计算机或使应用程序崩溃。发生了什么事？这个解决方案的时间复杂度非常高。这是 O(2^n).因此，随着我的输入增加，我的计算机必须做的工作量也呈指数增长。我必须以某种方式降低时间复杂度。这是我了解动态编程的时候。

动态规划是一种问题解决方法，它利用重叠子问题和最佳结构来减少解决问题所需的时间或工作量。它优化了解决方案。这如何适用于斐波那契？嗯，斐波那契问题有最优结构和重叠的子问题，因为相同的输入应该总是给出相同的结果，而且有重复的子问题。例如，fibonacci(6)将始终返回 8。但是看看这是如何通过我的递归函数计算出来的:

```
fib(6) =
fib(5) + fib(4) = 
fib(4) + fib(3) + fib(3) + fib(2) = 
fib(3) + fib(2) + fib(2) + fib(1) + fib(2) + fib(1) + 1 =
fib(2) + fib(1) + 1 + 1 + 1 + 1 + 1 + 1 =
1 + 1 + 1 + 1 + 1 + 1 + 1 + 1 =
8
```

这里有一个问题。给定相同的输入，序列将总是返回相同的数字，那么为什么我让我的计算机运行 fib(4)两次，fib(3)三次，fib(2)四次呢？6 是一个很小的输入，你能想象如果我最初的输入是 fib(30)我会让我的计算机运行 fib(3)多少次吗？我们是程序员，我们不喜欢在代码中重复自己，也不喜欢重复工作。

进入记忆化。不是“记忆”。这是我最初的假设，但这并不是一个可怕的思考方式。你是在‘记忆’你已经运行过的输入的返回值，这样，如果你的计算机试图第二次计算 fib(4 ),就像你输入 fib(6)时一样，你阻止了它再次递归分解它，并且只是说“嘿，你已经这样做了！答案是 3 记住！?"。

你必须记住，你的电脑不是魔术。它一次计算一个 fib(n)调用**的答案**。所以，不像上面的代码片段，我们是按行从左到右读的，你的计算机更多的是按列工作。它不会寻找下一个 fib(n)调用，直到它对第一个调用有了明确的答案。让我试着想象一下。如果你调用 fib(6 ),下面是你的计算机将按照它的调用栈的顺序运行的内容，其中**粗体**函数是你的计算机运行的下一步，在下一行:

```
fib(6) = **fib(5)** + fib(4)
fib(5) = **fib(4)** + fib(3)
fib(4) = **fib(3)** + fib(2)
fib(3) = **fib(2)** + fib(1)
fib(2) = 1 //YAY we removed a fib call from the stack and bubble up
fib(3) = 1 + **fib(1) //**we now get to run the 2nd fib call from herefib(1) = 1 //another fib call done, we bubble up again!
fib(3) = 1 + 1 = 2 //Now those first 2 calls let us finish this call
fib(4) = 2 + **fib(2) //**We solved fib(2) before but are doing it again
fib(2) = 1 //same answer we got last time we did fib(2)
fib(4) = 2 + 1 = 3 //and now we bubble up again to finish this call
fib(5) = 3 + **fib(3) //**we already solved fib(3) once! Why again?!
fib(3) = **fib(1)** + fib(2) //See how repetitive this is?
fib(1) = 1 //same solution we got the first time
fib(3) = 1 + **fib(2)** //have I made my point yet?
fib(2) = 1 //This isn't new information
fib(3) = 1 + 1 = 2 //yep, fib(3) still equals 2
fib(5) = 3 + 2 = 5 //Finally finish 1/2 of the original problem
fib(6) = 5 + **fib(4)** //you've got to be kidding me...
fib(4) = **fib(3)** + fib(2) //we could've finished this a long time ago
fib(3) = **fib(1)** + fib(2) //all of this is not fun to write out
fib(1) = 1 //We meet again fib(1)
fib(3) = 1 + **fib(2)** //this is the 4th time we are doing fib(2)
fib(2) = 1 //who would've thought!? (sarcasm)
fib(3) = 1 + 1 = 2 //dejavu
fib(4) = 2 + **fib(2)** //Yeah sure, a 5th time for good measurefib(2) = 1 //I genuinely hope all this is helpful to you
fib(4) = 2 + 1 = 3
fib(6) = 5 + 3 = 8 //FINALLY WE HAVE AN ANSWER
```

正如您所看到的，我们的 fib(6)调用使我们的计算机运行 fib(n) 15 次，而这些 fib(n)调用中只有 6 次是唯一的！如果我们能够记忆，我们就可以大大减少工作量，跳过几乎每一个重复的谎言:

```
fib(6) = **fib(5)** + fib(4)
fib(5) = **fib(4)** + fib(3)
fib(4) = **fib(3)** + fib(2)
fib(3) = **fib(2)** + fib(1)
fib(2) = 1 //lets remember that
fib(3) = 1 + **fib(1)** fib(1) = 1 //lets remember that too
fib(3) = 1 + 1 = 2 //Cool, lets remember that as well
fib(4) = 2 + **fib(2) //**oh yeah! fib(2) = 1
fib(4) = 2 + 1 = 3 // sweet lets remember that
fib(5) = 3 + **fib(3) //**oh yeah! fib(3) = 2
fib(5) = 3 + 2 = 5
fib(6) = 5 + **fib(4)** //oh yeah! fib(4) = 3
fib(6) = 5 + 3 = 8 //DONE
```

如你所见，通过记住我们的返回值，我们大大减少了调用 fib(n)的次数。我们如何做这样的事情？我们如何使用记忆化？我们通过递归调用持久化一个返回值数组，并且每次都检查这个数组，以查看我们之前是否已经解决了 fib(n)。我们如何知道在数组中的哪里存储和检索我们的值呢？我们只是将 fib(n)的答案存储在 array[n]中。这是它的样子:

```
function fib(n, memo=[]){
  if(memo[n] !== undefined) return memo[n];
  if(n <= 2) return 1
  let num = fib(n-1, memo) + fib(n-2, memo)
  memo[n] = num
  return num
}
```

事情是这样的。我们传入存储返回值的数组(称为“memo”表示“memoization”)，并默认它只在第一次调用时作为一个空数组。然后，我们检查我们的 memo 数组，看看我们是否已经在它的第 n 个位置存储了一个答案；如果是这样，我们就把它退回去。然后，如果 n 是 1 或 2，我们返回 1，就像我们最初做的那样。然后，我们像以前一样递归地调用 fib(n-1)和 fib(n-2 ),但是这一次，传递我们的持久 memo 数组，并将总和设置为一个变量。然后，我们将总和放在 memo 数组的第 n 个位置，并返回总和。

![](img/627d60d4f7043ddff661e10e232b1a6c.png)

照片由[基思·卢克](https://unsplash.com/@lukephotography?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

所以使用这个解决方案，任何时候调用 fib(n)而 n 已经被求解过一次，memo[n]将已经保存答案，并返回它，而不是继续递归调用 fib(n-1) + fib(n-2)。这使得时间复杂度从 O(2^n)降低到 O(n ),这要好得多。这意味着以前 fib(39)需要一分多钟才能解决的问题，现在只需要不到一秒钟！

不要太早庆祝…继续尝试在 [repl.it](https://repl.it/languages/javascript) 上运行 fib(8349)。它会在不到一秒的时间内返回无穷大。所以时间很好，返回的数字并不理想，但是我们使用的是 javascript，事实就是这样。真正的问题是当你把 1 加到 n…运行 fib(8350)，你得到:

```
RangeError: Maximum call stack size exceeded
```

![](img/51bfa46166c97c2c7aa3198dcc694f92.png)

照片由[克里斯多佛罗拉](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

经典堆栈溢出。我们有太多的递归调用等待我们的堆栈，这是太多的处理。记忆化背叛了我们。经典的西斯复仇——“你应该是被选中的人”。

没关系，来个‘制表’来化险为夷！制表是一个过程，在这个过程中，你将结果存储在一个表(通常是一个数组)中，同时进行迭代，而不是递归调用。这节省了空间复杂度并防止了堆栈溢出。它看起来是这样的:

```
function fib(n){
  if(n <= 2) return 1
  const fibNums = [0,1,1]
  for(let i = 3; i <= n; i++){
    fibNums[i] = fibNums[i-1] + fibNums[i - 2]
  }
  return fibNums[n]
}
```

这一次，不是将数组作为参数传递，而是在函数中紧接在边缘情况之后进行赋值。它用[0，1，1]初始化，因为我们知道这些值，我们需要给我们的循环一些东西来计算。在我们的循环中，我们从 i = 3 开始，因为我们已经填充了数组索引 0–2。然后，当 I 小于**或** **等于** n 时，我们运行循环。这是正确达到第 n 个值的关键。我们现在在循环中向上迭代，每次增加 I，而不是递归分解。这确保我们不会重新计算任何值，并允许我们在 O(n)时间内返回当前值，而不会导致堆栈溢出。所以我们现在可以毫无问题地运行 fib(8350)。

随着我们越来越擅长解决问题，我们能够识别模式，并轻松地提出自己的解决方案。我们也许能够写出一个通过所有测试用例的解决方案，但是作为程序员，这不是我们工作的终点。我们需要关心时间和空间的复杂性……这很复杂。尝试改进解决方案的时间复杂性的一个很好的方法是采用动态编程方法。在你的问题中寻找更小的问题。寻找重复的计算、循环和函数调用。尝试在递归工作时实现记忆化，或者在循环时实现列表化，并尝试提高你的 BigO。平衡时间复杂度和空间复杂度之间的权衡，因为一个通常可以帮助另一个。我希望你和我一样对这些概念感兴趣，并且我希望下次你面对另一个算法时，它能激发出另一个灵感。感谢阅读！