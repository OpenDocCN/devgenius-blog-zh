# Javascript 中混洗数组的最佳解决方案

> 原文：<https://blog.devgenius.io/jthe-optimal-solution-to-shuffle-an-array-in-javascript-2d9c8ac066d9?source=collection_archive---------5----------------------->

![](img/c818bbd57184f9f2cfffa5600b093275.png)

我最近遇到了一个小问题，关于在旧数组的基础上创建一个新的随机有序数组。简而言之，最终目标是得到一个混洗的数组。

【https://pitayan.com/posts/javascript-shuffle-array/】访问阅读原文带**源代码集锦**。

以下是我在搜索网页之前，经过几分钟的实验后得出的解决方案。(我以为我自己能行:p)

```
var arr = [1, 2, 3, 4, 5, 6, 7]function shuffle (arr) {
  let i = 0,
      res = [],
      index while (i <= arr.length - 1) {
    index = Math.floor(Math.random() * arr.length) if (!res.includes(arr[index])) {
      res.push(arr[index])
      i++
    }
  } return res
}// expected
arr = shuffle(arr)
// [6, 3, 4, 1, 7, 2, 5]
```

正如你所看到的，这不是一个处理洗牌的好方法，所以我决定做一些研究。

在 google 和 stackoverflow 上找了一些答案后，找到了一个最满意的[解](https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array)来洗牌。(答案从 2010 年就有了……但是，确实非常合格。)

首先，我们来看看答案。这很简单，但足够快。

```
function shuffle(array) {
  var currentIndex = array.length, temporaryValue, randomIndex; // While there remain elements to shuffle...
  while (0 !== currentIndex) { // Pick a remaining element...
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1; // And swap it with the current element.
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  } return array;
}
```

# [为什么我的解决方案不好](https://pitayan.com/#why-my-solution-is-bad)

一开始，我只是考虑在一个`while`循环中创建新的随机索引，并将旧的数组元素作为返回推送到一个新的数组中。

```
while (i <= arr.length - 1) {
  // create random index
  index = Math.floor(Math.random() * arr.length) // insert the element to new array
  if (!res.includes(arr[index])) {
    res.push(arr[index])
    i++
  }
}
```

它运行良好，回报非常令人满意。但是时间复杂度相当糟糕。在`while`循环中，它检查要插入的元素是否存在于每个循环的新数组中。这就产生了 *O(n2)* 。

如果一个数组没有那么大，那么我的函数就很好。但事实是，我的项目需要生成一个超过 1000 个元素的列表。所以还是优化算法比较好。(我觉得做这样的优化总是比较好的。不要害怕对计算机指手画脚(:D)

# [费希尔-耶茨洗牌](https://pitayan.com/#the-fisheryates-shuffle)

stackoverflow 的答案似乎很简单，然而实际上它使用了由罗纳德·费雪和弗兰克耶茨发明的算法。

> *Fisher–Yates shuffle 是一种用于生成有限序列的随机排列的算法——简单来说，该算法对序列进行洗牌。*
> 
> *…后又被称为* [*唐纳德·克努特*](https://en.wikipedia.org/wiki/Donald_Knuth) *。*
> 
> *—来自* [*百科*](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)

有一篇旧的博客文章可视化了洗牌算法。[https://bost.ocks.org/mike/shuffle/](https://bost.ocks.org/mike/shuffle/)

`shuffle`函数是算法的描述。

```
function shuffle(array) {
  var currentIndex = array.length, temporaryValue, randomIndex; // While there remain elements to shuffle...
  while (0 !== currentIndex) { // Create a random index to pick from the original array
    randomIndex = Math.floor(Math.random() * currentIndex);
    currentIndex -= 1; // Cache the value, and swap it with the current element
    temporaryValue = array[currentIndex];
    array[currentIndex] = array[randomIndex];
    array[randomIndex] = temporaryValue;
  } return array;
}
```

这个解决方案很好，但仍有改进的潜力。我相信在这里做一个纯函数更有意义。所以我宁愿返回一个新的数组，而不是修改原来的参数作为副作用。

为了避免修改原始数据，我还可以在传递 arugment 的同时创建一个克隆。

```
shuffle(arr.slice(0))
```

# [其他变化](https://pitayan.com/#other-variations)

对于我在 [stackoverflow](https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array) 上找到的解决方案，有一些值得尊敬的替代方案，我认为它们是经过适当优化的。

## [德斯滕费尔德洗牌](https://pitayan.com/#the-durstenfeld-shuffle)

该解决方案出现在[堆栈溢出](https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array)页面上。最后我找到了一个要点备忘录。

[https://gist . github . com/web bower/8d 19 b 714 ded 3 EC 53d 1d 7 ed 32 b 79 fdbac](https://gist.github.com/webbower/8d19b714ded3ec53d1d7ed32b79fdbac)

```
// Pre-ES6
function shuffleArray(array) {
  for (var i = array.length - 1; i > 0; i--) {
    var j = Math.floor(Math.random() * (i + 1));
    var temp = array[i];
    array[i] = array[j];
    array[j] = temp;
  }
}// ES6+
function shuffleArray(array) {
  for (let i = array.length - 1; i > 0; i--) {
    let j = Math.floor(Math.random() * (i + 1));
    [array[i], array[j]] = [array[j], array[i]];
  }
}
```

## [数组扩展方法](https://pitayan.com/#array-extension-method)

实际上，我更喜欢这个，因为它简单，而且有一个关于整数的小技巧。这里的窍门是用`>>>` ( [无符号右移运算符](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Unsigned_right_shift))代替`Math.floor`。

```
Array.prototype.shuffle = function() {
  let m = this.length, i;
  while (m) {
    i = (Math.random() * m--) >>> 0;
    [this[m], this[i]] = [this[i], this[m]]
  }
  return this;
}
```

好了，研究到此为止。希望您也能从这篇文章中很好地理解`shuffle`算法。如果你觉得这篇文章很棒，请分享到社交网络上。

感谢阅读！

# [参考文献](https://pitayan.com/#references)

*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Operators/Unsigned _ right _ shift](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Unsigned_right_shift)
*   [https://en.wikipedia.org/wiki/Fisher–Yates_shuffle](https://en.wikipedia.org/wiki/Fisher%E2%80%93Yates_shuffle)
*   [https://stack overflow . com/questions/2450954/how-to-randomize-shuffle-a-JavaScript-array](https://stackoverflow.com/questions/2450954/how-to-randomize-shuffle-a-javascript-array)
*   [https://gist . github . com/web bower/8d 19 b 714 ded 3 EC 53d 1d 7 ed 32 b 79 fdbac](https://gist.github.com/webbower/8d19b714ded3ec53d1d7ed32b79fdbac)

原本在[Pitayan.com](https://pitayan.com?ref=medium)

[https://pitayan.com/posts/javascript-shuffle-array/](https://pitayan.com/posts/javascript-shuffle-array/?ref=medium)

[](https://pitayan.com/posts/javascript-shuffle-array/?ref=medium) [## Javascript - Pitayan 中混洗数组的最佳解决方案

### 我最近遇到了一个小问题，关于在旧数组的基础上创建一个新的随机有序数组。简而言之，决赛…

pitayan.com](https://pitayan.com/posts/javascript-shuffle-array/?ref=medium)