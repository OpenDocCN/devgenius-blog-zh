# 我对 ES2020 的功能感觉非常好

> 原文：<https://blog.devgenius.io/i-have-very-good-feelings-about-es2020-features-42b9c2aec2cc?source=collection_archive---------7----------------------->

![](img/d7d2c467c7372cd0b02dd40b23f48e9e.png)

ES2020 已经出来一段时间了。我猜很多开发者已经采用了这些特性。有些人甚至在这些功能还处于提议阶段时就开始使用它们了。没错。我的团队开始使用第三阶段的一些特性已经有一段时间了。

在本文中，我将谈谈我使用这些 ES2020 功能的感受。因此，我认为这些功能非常棒，非常重要。

(访问[https://pitayan.com/posts/vue-techniques/](https://pitayan.com/posts/es2020-features/?ref=medium)阅读原文。**源代码高亮显示。**)

# [1。操作员:无效合并](https://pitayan.com/posts/es2020-features/#1-operator-nullish-coalescing)

一开始，我的“为你的代码提供恰当的解释”的想法否定了这种改进。我认为既然一个`nullish`或者`falsy`的值应该在一个`if condition`或者其他一些战略`function`下给出一个彻底的“解释”。通过这种方式，我还可以添加一些额外的逻辑，而不用在以后重构速记表达式。

```
let website = {}let url = 'https://pitayan.com'
if (website.url !== undefined && typeof website.url === String) {
  url = website.url
}
```

但是在项目中到处尝试“无效合并”之后，我很快做出了妥协。我的担心被证明是多余的。因为我想要的是确保目标变量在大多数情况下有具体的值。

在 Typescript 中，操作一个`nullish`值可能会给出错误或警告。这意味着我的代码可以很容易地按照忠告进行优化。

```
let url: string = website.url ?? 'https://pitayan.com'
```

总之，我对`nullish coalescing`的感觉是相当支持的。在给一个变量赋值时，这是非常有帮助的。

# [2。异步模块:动态导入](https://pitayan.com/posts/es2020-features/#2-asychronous-modules-dynamic-import)

从第二阶段开始，我就一直在使用这个特性。你知道，我们的应用程序需要“及时”的能力。

它帮助我将 Javascript / JSON 文件作为模块导入我的应用程序。现在它可以出现在我的项目中的任何地方(主要是前端)。没有在服务器端体验过)。不得不说这个功能是不可或缺的。

```
let answer = await import('./myanswer.json')import('./component.js')
  .then(module => {
    module.default.mount(answer)
  })
```

# [3。Promise.allSettled](https://pitayan.com/posts/es2020-features/#3-promiseallsettled)

`Promise.all`为我们带来了著名的“回调地狱”的有用解决方案。嵌套函数真的很讨厌。

```
// Before
getUp().then(() => {
  washFace().then(() => {
    brushTeeth().then(() => {
      eatBreakfast()
    })
  })
})// After
Promise.all([
  getUp,
  watchFace,
  brushTeeth
]).then(() => {
  eatBreakfast()
})
```

如你所知，一旦其中一个任务遇到异常，`Promise.all`就会抛出错误。我从来不希望不洗脸就吃不到早餐。

当然，我可以将`finally`添加到`Promise`链中，以确保吃早餐。但是`finally`没有提供我想要的正确上下文。甚至不用提用`catch`吃早餐，那是一种不好的做法。

最后，`allSettled`允许我们在洗脸或刷牙的时候设置一个回调。我只想吃早餐！感觉你长大了，搬出了父母家。所以妈妈关于洗脸刷牙的责骂都没有了。

```
// Bad
Promise.all([
  getUp,
  watchFace,
  brushTeeth
]).then(() => {
  eatBreakfast()
}).catch(() => {
  eatBreakfast()
})// Good
Promise.allSettled([
  getUp,
  watchFace,
  brushTeeth
]).then(results => {
  eatBreakfast()
})
```

# [4。非常大的数字:BigInt](https://pitayan.com/posts/es2020-features/#4-very-large-number-bigint)

Javascript `Number`类型的范围从-(253-1)到 253-1(数字。MIN_SAFE_INTEGER ~数字。MAX_SAFE_INTEGER)。

有了这个新 API，任何大量数据都可以在浏览器中正确处理，而不会损失任何精度。

```
let bitInteger = BitInt(9007199254740995)// Orlet bitInteger = 9007199254740995n
```

在我的例子中，大整数被转换成`String`以避免在它们被执行到前面之前的精度问题。目前在我的项目中使用`BitInt`的确是一个罕见的案例。我相信在其他项目中也有围绕这个特性的其他一般需求。

我能想到的一个简单的例子是:如果一个数据库的模型 ID 是数字的并且相当长(像一个购物订单 ID)，那么当它被传递到前端时,`BigInt`就能帮助我们。

# [5。regex:string . prototype . match all](https://pitayan.com/posts/es2020-features/#5-regex-stringprototypematchall)

其实`matchAll`已经提出很久了。它返回一个包含所有匹配字符的`Array`。与`match`相比，无论字符串匹配与否，返回类型`RegExpStringIterator`都会给出一致的结果。通过使用像`Array.from`这样的工具，我最终可以从`iterator`中提取所有结果。

依我拙见，这毕竟是一个很好的进步。因为返回的数据类型总是相同的。

```
let url = 'https://pitayan.com'
let blank = ''
let reg = /pitayan.com/g// match
url.match(reg) // ["pitayan.com"]
blank.match(reg) // null// matchAll
Array.from(url.matchAll(reg)) // [["pitayan.com", index: 8, input: "https://pitayan.com", groups: undefined]]
Array.from(blank.match(reg)) // []
```

# 6。一个标准化的全局对象:GlobalThis

有时候 JS 代码需要跨平台，但是 Node.js 用的`global`和浏览器的`window`不一样(web worker 用的是`self`)。所以在开始一切之前，我需要首先处理环境兼容性。

```
const globalThis = ((global, window, self) => {
  if (global) return global
  if (window) return window
  if (self) return self
  throw new Error('...')
})(global, window, self)
```

我个人认为识别环境是语言系统的职责。所以`globalThis`填补了这个令人讨厌的空白。真的真的很欣赏这个功能的发布。

# [7。优雅链条:可选链条](https://pitayan.com/posts/es2020-features/#7-chain-with-elegance-optional-chaining)

我从第二阶段就开始使用这个特性了。它有助于减少许多`if conditions`或`ternary operators`，这使得我的代码看起来更简单。

```
let food = restuarant && restuarant.cooking && restuarant.cooking.food// after
let food = restuarant?.cooking?.food
```

除了访问属性，我还可以在方法上使用它。

```
let food = restuarant?.cooking?().food
```

这不是很好看吗？

# [8。模块名称空间导出:export * as](https://pitayan.com/posts/es2020-features/#8-module-namespace-exports-export--as)

这是一个令人惊叹的 Javascript API。我以前也是这样导出一些模块的。

```
import A from './A.js'
import B from './B.js'
import C from './C.js'export default { A, B, C }
```

现在我可以做这个。

```
export * as A from './A.js'
export * as B from './B.js'
export * as C from './C.js'
```

并且导入这些模块的用法保持不变。

```
import { A, B, C } from './index.js'
```

花哨但很实用！

# [9。其他功能](https://pitayan.com/posts/es2020-features/#9-other-features)

还有一些其他的功能我还没有体验好，不好下结论。他们的定义很清楚，足以推测这些变化。我相信它们非常有用，否则不可能将它们引入新标准。

## [for … in loop order](https://pitayan.com/posts/es2020-features/#for--in-loop-order)

ECMAScript 在开始时没有指定`for in`循环顺序。每个浏览器都有不同的行为，但现在他们都统一了 ECMA 标准。

## [导入元数据](https://pitayan.com/posts/es2020-features/#import-meta)

现在，您可以从导入的模块中访问信息。

```
<script src="script.js"></script>console.oog(import.meta) // { url: "https://pitayan.com/script.js" }
```

# [结论](https://pitayan.com/posts/es2020-features/#conclusion)

Javascript 这几年给我们带来了很多方便又强大的 API。随着新标准的不断出台，我们的发展也在不断进步。事实证明，它们是我们工程师的救命稻草。我希望将来会有更强大的功能，这样也许有一天我就不用输入任何代码来构建一个精彩的应用程序了。

好了，以上是对 ES2020 特性的一些浅见。希望你也有同样的感受。

如果你觉得这篇文章很棒，那么请分享到社交网络，让更多人参与进来。

感谢您的阅读！

# [参考文献](https://pitayan.com/posts/es2020-features/#references)

*   [https://www . freecodecamp . org/news/JavaScript-new-features-es 2020/](https://www.freecodecamp.org/news/javascript-new-features-es2020/)
*   【https://www.jianshu.com/p/416a0931e96c 

原本在[Pitayan.com](https://pitayan.com/?ref=medium)

[https://pitayan.com/posts/es2020-features/](https://pitayan.com/posts/es2020-features/?ref=medium)

[](https://pitayan.com/posts/es2020-features/?ref=medium) [## 我对 ES2020 的特性很有感觉——皮塔扬

### ES2020 已经出来一段时间了。我猜很多节点开发者已经采用了这些特性。有些甚至…

pitayan.com](https://pitayan.com/posts/es2020-features/?ref=medium)