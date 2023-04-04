# 苗条原理分析与评论

> 原文：<https://blog.devgenius.io/svelte-principle-analysis-and-review-bdf3a50d1258?source=collection_archive---------8----------------------->

![](img/80ccf750c2f2a757a436ed570dad6d2e.png)

# 介绍

Svelte 是一个用于构建 web 应用程序的工具，类似于 React 和 Vue 等 JavaScript 框架，其核心是使构建交互式用户界面变得更容易。

但是有一个关键的区别:Svelte 在**构建/编译**阶段将您的应用程序转换成理想的 JavaScript 应用程序，而不是在运行阶段解释应用程序代码。这意味着您不必为框架消耗的性能付费，并且在应用程序首次加载时不会有额外的损失。

您可以使用 Svelte 构建您的整个应用程序，也可以逐渐将其融入您现有的代码中。您还可以在任何地方将组件作为独立的包交付，而没有传统框架的额外开销。

# 特征

## 干净的代码

*   使用 svelte，代码如下。

```
<script>

  let animal = 'dog';

  const showModal = () => {

    alert(`My favorite animal is ${animal}`);

  };

</script>

<input type="text" bind:value={animal} />

<button on:click={showModal}>弹出</button>
```

*   反应

```
export default function App() {

  const [animal, setAnimal] = useState('dog');

  const showModal = () => {

    alert(`My favorite animal is ${animal}`);

  };

  return (

    <>

      <input

        type="text"

        value={animal}

        onChange={() => {

          setAnimal(animal);

        }}

      />

      <button onClick={showModal}>弹出</button>

    </>

  );

}
```

*   某视频剪辑软件

```
<template>

  <div>

    <input type="text" v-model="animal" />

    <button @click="showModal">弹出</button>

  </div>

</template>

<script>

import { defineComponent, ref } from 'vue';

export default defineComponent({

  setup() {

    const animal = ref('dog');

    const showModal = () => {

      alert(`My favorite animal is ${animal.value}`);

    };

    return {

      animal,

      showModal,

    };

  },

});

</script>
```

## 没有虚拟 dom

Svelte 能够将代码编译成小的、独立于框架的普通 js 代码，使得应用程序快速启动和运行。

**性能更佳**

许多学习 react 或 vue 的学生可能听说过诸如“虚拟 dom 很快”之类的评论，所以当他们看到这一点时，他们想知道为什么 svelte 在没有虚拟 dom 的情况下更快？

这其实是一个误区，react 和 vue 等框架实现虚拟 dom 的主要目的不是性能，而是覆盖底层的 dom 操作，让用户可以通过声明式的、基于状态驱动 UI 的方式来构建我们的应用，提高代码的可维护性。

react 或 vue 所说的虚拟 dom 的良好性能是指框架仍然可以提供良好的性能保证，而无需对页面进行任何特殊的优化。比如下面这个场景，我们每次从服务器接收数据都重新渲染列表，如果不通过普通的 dom 操作做特殊的优化，每次都会重新渲染所有的列表项，性能消耗比较高。react 之类的框架通过键来标记列表项，并且只重新呈现已经改变的列表项，因此性能得到了提高。

想想上面的场景，如果我们在操作真实 dom 的时候也标记列表项，只重新渲染发生变化的列表项，不需要虚拟 dom diff，那么性能甚至比虚拟 dom 还要高。

然后 svelte 通过将数据映射到真实的 dom 来实现这种优化，通过 ast 在编译时计算并保存数据，当数据发生变化时直接更新 dom，由于不依赖于虚拟 dom，所以在初始化和更新时速度非常快。

## 真正的反应

Svelte 为 JavaScript 本身增加了响应能力，而不需要复杂的状态管理库。svelte 的响应实现将在后面的源代码解释部分解释。

# 发展趋势

svete 是 Rich Harris(汇总作者)，svete 在 2016 年开始开放采购，并在 2019 年开始吸引更多的关注。

Github 上的苗条现在是 49.9k 星。

Npm 上 svelte 的每周下载量约为 15w。

尽管它在明星数量和下载量方面仍然远远落后于反应、vue 和 angular，但考虑到它相对较晚的首次亮相，这是可以理解的。此外，根据该框架的研究，其用户满意度和兴趣是过去两年中最高的，其使用和受欢迎程度也在迅速增加。

总的来说，未来看起来很有希望！

# 摘要

总的来说，svelte 遵循前端三大框架来推动新的、新的思维方式来实现响应性，因为启动时间不是很长，目前它的生态性还不完整，在大型项目中的应用仍有待考察，但在一些简单的页面如事件页面、静态页面等场景中感觉非常适合目前，个人对其未来发展持乐观态度。

因为它简单的语法和类似 vue 语法的特点，启动成本很小，我们感兴趣的是花一点时间去理解，去充实自己的武库。