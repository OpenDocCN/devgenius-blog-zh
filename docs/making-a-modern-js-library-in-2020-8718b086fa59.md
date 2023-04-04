# 2020 年打造现代 JS 图书馆

> 原文：<https://blog.devgenius.io/making-a-modern-js-library-in-2020-8718b086fa59?source=collection_archive---------3----------------------->

![](img/e40705b7c341c0b97737a974e82b9c50.png)

回购现代 hello world

最近，我被分配了一个任务，创建一个新的 JS 库来替换已经发布了近 8 年的过时的库。这是一个相当艰巨的任务，因为我也被允许尝试一切新的东西，使这个项目更加稳健。

*最初发表于*[*【https://pitayan.com】*](https://pitayan.com/posts/modernest-lib-hello-world/)*。*

[](https://pitayan.com/posts/modernest-lib-hello-world/?ref=medium) [## 打造 2020 年的现代 JS 图书馆——皮塔扬

### 最近，我被分配了一个任务，创建一个新的 JS 库来替换已经发布的过时的 JS 库…

pitayan.com](https://pitayan.com/posts/modernest-lib-hello-world/?ref=medium) 

我想到的第一件事是让自己有一个复杂但很棒的“开发环境”,它准确而生动地解释了为什么我是 DX 的第一个开发者。

在本文中，我将展示我是如何用一个小小的“hello-world”repo 实现它的。

为什么大惊小怪？值得吗？

假设你在打仗，大队长只给你刺刀和敌人战斗。当你的敌人使用机枪时，你认为你敢向前冲锋吗？我敢打赌，我们 99%的人都没有足够的勇气这么做(请不要告诉我你愿意为荣耀而死)。

那么，如果营长给了你最致命的武器，只需轻轻一击就能击败你的敌人，就像灭霸的弹指一样，那又会怎样呢？我想现在你有勇气对抗敌人了。

无论如何，我想成为那个为我的队友提供致命武器的大队长，以消除我们开发中的痛苦部分。当我们的发展成为一种快乐的体验时，我相信来回移动东西的忙乱绝对是值得的。

好吧，这是我的演示回购的链接:

[https://github.com/daiyanze/modern-hello-world](https://github.com/daiyanze/modern-hello-world)

# 灵感

为了让我们伟大的新图书馆成为一个真正的现代图书馆，我一直在研究各种现代 JS repos。

我发现所有这些库都有一个共同点:

> *他们都碰巧将 Jest 或 Mocha/Chai 作为他们的测试套件。*

事实上，Jest 和 Mocha/Chai 已经在市场上存在多年了，它们相当稳固。即使有一些像 Ava.js 这样的新来者，但他们仍然不能取代目前有更大社区的人。

选择社区较大的图书馆已经是一个常识。因为他们的代码正在被许多其他人测试，并且有更多的错误修正。一句话:几乎没有人有足够的勇气去使用那些没有经过彻底测试的库。

> *如何知道一个图书馆是否有很大的社区？*

简单，只要查查他们有没有很多 Github 明星或者问题。“Stars”通常意味着这个库是合格的，并且被开发人员所接受。“问题”在一定程度上反映了社区的互动性和图书馆的活跃性。这两个指标对于我们的技术选择应该非常可靠。

因此，我会从 Github 中选择那些有很多明星和问题的工具作为我们的 devDependencies。

# 依赖性特征

以下是我们新项目的一些 mayjor(“必须”)特性。在我看来，这些特性已经成为 2020 年新的 JS 库的技术选择标准。

## 1.以打字打的文件

编写没有类型的代码实际上是一件痛苦的事情，如果我们没有提前考虑我们的数据类型，“类型错误”肯定会出现。所以现在，因为 Typescript 已经成为几乎所有新诞生的 JS 库的标准或约定。毫无疑问，这个特性对我们的项目来说是“必须的”。

## 2.玩笑

测试是 JS 项目不可或缺的另一个东西。我相信没有一个团队领导会选择一项甚至自己都没有测试过的技术。所以 Jest 肯定是我们测试所需要的工具，正如你所知道的，他们有一个很大的社区。

## 3.较美丽

统一团队的编码风格可以节省时间。当你拜访你的队友时，这是最重要的。

第一次看到这个工具是 2017 年。当时，公开市场上几乎没有 JS 代码格式化程序。嗯，更漂亮的使它可用。您可以按照您希望的方式格式化代码。

更重要的是，在 ESlinter 或 TSlinter 的帮助下，编辑器可以成为 JS 开发人员非常酷的东西。

采用这些工具的原因很简单，因为:

> 他们给你提示！

只要看看 7 年前创建的 [Airbnb 的 javascript 风格指南](https://github.com/airbnb/javascript)，你就会知道代码风格有多重要。

## 4.哈士奇和传统-变更日志

我想每个人都有以下贪得无厌的愿望。

> *我希望我的项目能自动吐出变更日志。我希望提交格式可以统一。祝…*

这些工具对你来说可能听起来很陌生。但是它们实际上是一个很好的组合，可以基于 git 提交消息自动生成稳定的变更日志。Angular project 正在使用这种方法来创建更好的变更日志。

看看 Angular 的美丽日志:

```
11.0.0-next.3 (2020-09-23)Bug Fixescommon: add params and reportProgress options to HttpClient.put() overload (#37873) (dd8d8c8), closes #23600
compiler-cli: generate let statements in ES2015+ mode (#38775) (123bff7)
core: ensure TestBed is not instantiated before override provider (#38717) (c8f056b)
forms: type NG_VALUE_ACCESSOR injection token as array (#29723) (2b1b718), closes #29351
Featurescommon: Add ISO week-numbering year formats support to formatDate (#38828) (984ed39)
compiler: Parse and recover on incomplete opening HTML tags (#38681) (6ae3b68), closes #38596
router: add migration to update calls to navigateByUrl and createUrlTree with invalid parameters (#38825) (7849fdd), closes #38227
service-worker: add the option to prefer network for navigation requests (#38565) (a206852), closes #38194
BREAKING CHANGEScore: If you call TestBed.overrideProvider after TestBed initialization, provider overrides are not applied. This behavior is consistent with other override methods (such as TestBed.overrideDirective, etc) but they throw an error to indicate that, when the check was missing in the TestBed.overrideProvider function. Now calling TestBed.overrideProvider after TestBed initialization also triggers an error, thus there is a chance that some tests (where TestBed.overrideProvider is called after TestBed initialization) will start to fail and require updates to move TestBed.overrideProvider calls before TestBed initialization is completed.
```

> 不要自己写变更日志就好。让机器来处理它们。

好吧，这 4 个工具基本上是我作为一个“DX-first”开发者非常非常渴望的特性。当然，还有其他一些不错的功能，但我认为目前已经足够了。毕竟，新的更多的工具将增加我们每个成员的学习时间。

# “汇总”

当我在构建我的存储库的原型时，我从未想过汇总会是对我最大的挑战。Rollup 有一个很棒的文档，只要看一下例子，你就会明白它希望你立即做什么。但是真正的问题在于我应该如何处理我的输出文件。

因为我的输出是一个库，所以我需要将所有的源代码汇总到一个可以在浏览器中使用的 JS 文件中(或者 Node.js)。这可以很容易地用一些插件完成。我对这个神奇的工具相当陌生，这个工具已经为 Vue 和 React 等最著名的框架提供了动力。

坦白说，我不太清楚自己下一步该怎么走。

为了省去来回移动的步骤，我放弃了探索汇总配置。可以想象，一个“菜鸟”不可能从零开始创造出“伟大”的东西。

那好吧。让我试试另一种方法。

> 如果我不知道怎么做，那么别人应该知道。

Vue 和 React 已经把功课做好了，剩下的就是我抄袭他们了:d .(很自豪自己是个抄袭者~)

我选择 [Vue 3.0](https://github.com/vuejs/vue-next) 作为我的目标回购，因为这是一个相当新的项目。而 Vue 目前拥有非常高的人气。

它的配置有点复杂，但仍然非常容易理解。

```
// Part of rollup.config.js in Vue-next repoimport path from 'path'
import ts from 'rollup-plugin-typescript2'
import replace from '[@rollup/plugin-replace](http://twitter.com/rollup/plugin-replace)'
import json from '[@rollup/plugin-json](http://twitter.com/rollup/plugin-json)'if (!process.env.TARGET) {
  throw new Error('TARGET package must be specified via --environment flag.')
}const masterVersion = require('./package.json').version
const packagesDir = path.resolve(__dirname, 'packages')
const packageDir = path.resolve(packagesDir, process.env.TARGET)
const name = path.basename(packageDir)
const resolve = p => path.resolve(packageDir, p)
const pkg = require(resolve(`package.json`))
const packageOptions = pkg.buildOptions || {}// ensure TS checks only once for each build
let hasTSChecked = falseconst outputConfigs = {
  'esm-bundler': {
    file: resolve(`dist/${name}.esm-bundler.js`),
    format: `es`
  },
  ...
}
...
```

在探索了 [Vue 3.0](https://github.com/vuejs/vue-next) 配置文件`rollup.config.js`之后，我发现它只做了 3 件事:

*   通过另一个脚本接收命令行参数
*   为不同类型的生成生成配置列表
*   导出配置列表

仅仅通过复制和粘贴，我就创建了一个具有上述特性的自定义汇总配置文件。但我更换了一个汇总插件，因为我个人更喜欢官方包。

Vue 提供了各种类型的构建，我认为这是明智之举，因为用户会有不同的开发目的和环境。

现在，我们可以看到 Vue 基于 JS 代码的输出格式提供了以下类型的构建输出(`es` & `cjs` & `iife`)。文件名中带有`prod`的文件用于生产目的:

```
vue.cjs.js 
vue.cjs.prod.js 
vue.d.ts 
vue.esm-browser.js 
vue.esm-browser.prod.js 
vue.esm-bundler.js 
vue.global.js 
vue.global.prod.js 
vue.runtime.esm-browser.js 
vue.runtime.esm-browser.prod.js 
vue.runtime.esm-bundler.js 
vue.runtime.global.js 
vue.runtime.global.prod.js
```

我希望这种方法可以应用在我们的项目中。类似但不同的是，文件名中带有`dev`的构建输出是用于开发的。

更重要的是，我们并不像 Vue 那样通过判断是否是`runtime`来区分不同的版本。所以下面的输出是最终目标。

```
helloworld.cjs.js 
helloworld.cjs.dev.js 
helloworld.d.ts 
helloworld.esm.js 
helloworld.esm.dev.js 
helloworld.js 
helloworld.dev.js 
helloworld.modern.js 
helloworld.modern.dev.js
```

这里是`rollup.config.js` : [modern-hello-wrold 汇总配置](https://github.com/daiyanze/modern-hello-world/blob/master/rollup.config.js)的链接。

`TLDR;`...但是要有耐心:p。

# 我的汇总配置的一些问题

## 1.类型检查问题

似乎即使我希望一次只构建一个包，Typescript 也会检查 monorepo 中的所有包，不管它们是否依赖于构建目标。

此外，在构建多个包时，类型检查可能会发生多次。我能听到我的粉丝在制作过程中很忙。(这完全没有必要)

Vue 3.0 使用了一个标志来禁用重复类型检查，而我没有。我不太确定这是不是一个好方法。但它肯定会影响我们的开发甚至生产构建。

## 2.报关出口问题

我的 helloworld 使用相同的工具(API-Extractor)和 Vue 配置从源代码中提取类型声明。我使用的是不同的 Typescript 插件。重新升级建筑声明输出，我需要将`tsconfig.json`参数`declaration`传递给那个插件。

很明显，不是我干的。因为我固执地认为没有`declaration`的建筑会稍微快一点。这可能是个错误的想法。无论如何，我应该稍后优化这一部分。

# “构建”脚本

我认为 Vue project 在“构建”过程中相当聪明。他们直接使用命令和 [execa](https://github.com/sindresorhus/execa) 来避免使用可编程 API。

```
execa(
  'rollup',
  [
    '-wc',
    '--environment',
    [
      `NODE_ENV:development`,
      ...
    ]
      .filter(Boolean)
      .join(','),
  ],
  {
    stdio: 'inherit',
  }
);
```

execa 给了我们直接使用这些 farmiliar 命令的经验，只要把这些片段重新组合在一起。依我看，这让事情变得简单多了。

我曾经考虑过使用 Rollup APIs 来处理构建。但看了一下官方文件后，我意识到这是一个愚蠢的想法。这让我感觉像是强迫一个只会弹 3 个和弦的吉他新手在大型音乐会上打节奏。

简而言之:有时候，妥协是一个让事情变得更简单的好主意。

因为我希望使它成为一个“Monorepo”，`packages/`文件夹包含了所有必要的内置模块。

```
# In the demo repo, we have 2 modules in total
packages/
  helloworld/
    src/
      index.ts
    index.js
    package.json
  shared/
    src/
      print.ts
    index.js
    package.json
```

`shared`模块就像普通 repo 中的**助手**或**实用程序**，但是它是作为一个包使用的，所以我可以像使用第三方库一样导入它。

```
import { print } from '[@helloworld/shared](http://twitter.com/helloworld/shared)'function helloWorld() {
  if (__DEV__) {
    print("It's under development")
  }
  print('hello world')
}
```

我个人倾向于在包前加上前缀`@<global_module_name>`的命名约定。这使得我所有的模块看起来非常统一。

```
{
  "name": "[@helloworld/shared](http://twitter.com/helloworld/shared)"
  ...
}
```

我发现 [Vue 3.0](https://github.com/vuejs/vue-next) repo 使用`NODE_ENV`来定义目标 commonjs 模块(因为`require`上下文通常会忽略节点环境)。这将有助于用户相应地包含正确的脚本。

在每个模块的根目录中，我复制并粘贴了 [Vue 3.0](https://github.com/vuejs/vue-next) 如何通过添加一个新的入口文件来处理它的 commonjs 模块。

```
// packages/helloworld/index.js
'use strict'if (process.env.NODE_ENV === 'production') {
  module.exports = require('./dist/helloworld.cjs.js')
} else {
  module.exports = require('./dist/helloworld.cjs.dev.js')
}
```

在我的例子中，`helloworld.cjs.js`和`helloworld.cjs.dev.js`的区别在于它是否包含下面的代码块，这些代码块只为开发脚本服务。(不得不说，Rollup“摇树”让我大开眼界)

```
...
// "if (__DEV__)" is treeshaked by Rollup{
  print('It\'s under development')
}
...
```

在这几个星期对 Vue 3.0 库的调查中，我想我已经发现了足够多的新鲜事物来学习。没有他们那些聪明的想法，我最近的任务不会轻易开始。

现在我的项目成功发布了。当我看到我的队友在“深思熟虑的知识库”中玩得开心时，我觉得我的努力真的很值得。

*原载于*[*https://pitayan.com*](https://pitayan.com/posts/modernest-lib-hello-world/)*。*

[](https://pitayan.com/posts/modernest-lib-hello-world/?ref=medium) [## 2020 年打造现代 JS 图书馆——皮塔扬

### 最近，我被分配了一个任务，创建一个新的 JS 库来替换已经发布的过时的 JS 库…

pitayan.com](https://pitayan.com/posts/modernest-lib-hello-world/?ref=medium)