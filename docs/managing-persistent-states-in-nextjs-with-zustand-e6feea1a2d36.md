# 用 Zustand 管理 Nextjs 中的持久状态

> 原文：<https://blog.devgenius.io/managing-persistent-states-in-nextjs-with-zustand-e6feea1a2d36?source=collection_archive---------0----------------------->

![](img/069948476e843a4a6781af6ef62a798b.png)

如果您正在构建更小的 web 应用程序，那么您将需要存储某种类型的数据，并在组件之间和页面之间传递这些数据。你通常将数据保存在一个*状态* 中，并且你通过使用上下文或者像 Redux 这样的状态管理框架来管理页面和组件之间的状态*共享*。这种方法会给你的应用程序带来很多复杂性，这是一个缺点，尤其是当你需要跟踪简单的事情时。

## 什么是 Zustand

Zustand 是一个轻量级且易于使用的解决方案，以简单的方式管理状态，它易于使用，并将解决您在开发需要状态管理的 web 应用程序时可能面临的大多数需求。此外，Zustand 可以在页面之间持久化数据并重新加载(稍后将详细介绍)

 [## 祖斯坦德

### 🐻承担国家管理中的必要反应

zustand.surge.sh](https://zustand.surge.sh/) 

## Zustand 和 Nextjs 合作吗

是啊！Zustand 与 Nextjs 完全兼容，但是如果你需要持久化状态，就需要做一些调整，这是由于 Nextjs 是如何工作的，但是只要增加几行代码，一切都会变得非常顺利(在我们的例子中，我们将持久化状态)

## Zustand 什么时候有用？

这取决于你的使用情况，但考虑到即使 Zustand 很轻很容易，但它非常强大，并获得了一些高级功能；所以每次都是从零开始，状态也不是真的复杂，可以给 Zustand 试试。如果您需要跟踪多步表单(如调查或复杂的注册)，如果您需要存储一些用户详细信息，如果您需要跟踪购物车(就像我们将要展示的示例)。此外，Zustand 允许使用 localStorage(或其他客户端存储方法)来保存这些数据，这样用户在重新加载或其他会话中就不会丢失数据。

## Zustand 是如何工作的？

在使用 Zustand 的最基本层面上，您可以创建一个自定义挂钩的商店，以便它可以在您的应用程序中的任何地方实现:

```
import create from 'zustand'
const useStore = create(set => ({
  bears: 0,
  increasePopulation: () => set(state => ({ bears: state.bears + 1 })),
}))
```

在页面或组件中，您只需访问挂钩，这样您就可以访问状态和功能，每次更新状态时，组件都会重新呈现:

```
function Home() {
  const bears = useStore(state => state.bears)
  const increasePopulation = useStore(state => state.increasePopulation)
   return {
      <h1>{bears}</h1>
      <button onClick={increasePopulation}>Add Bear</button>
   }
}
```

## 我们在建造什么？

在这个例子中，我们将利用 Zustand 来跟踪一个虚构的电子商务网站
中的用户购物车，具体如下:

*   我们将保持整个购物车处于一种状态:购物车的总金额，购物车中的商品数量，购物车中的产品列表
*   我们将创建一些状态函数:向购物车中添加商品，更新购物车中商品的数量，从购物车中移除商品，以及清理购物车
*   我们将把购物车存储在 localStorage 中，这样用户在浏览我们网站的页面时不会丢失购物车，即使在离开网站以后再回来时也不会丢失
*   我们将在页面中实现所需的代码，以使状态持久性与 Nextjs 一起工作

![](img/96f185976781aaee77956ad795815a3f.png)

## 设置项目

如果您不想编码，可以从这里下载这个项目的资源库:

[](https://github.com/popeating/state-mgt) [## GitHub - popeating/state-mgt

### 这是一个用 create-next-app 引导的 Next.js 项目。首先，运行开发服务器:打开…

github.com](https://github.com/popeating/state-mgt) 

使用以下内容初始化项目:

```
**npx create-next-app state-mgt**
```

然后进入新创建的文件夹`state-mgt`并安装 Zustand 模块:

```
**npm install zustand**
```

我们还将使用 Tailwind CSS 来设计我们的网站，这是完全可选的，遵循官方文档

[](https://tailwindcss.com/docs/guides/nextjs) [## 用 Next.js - Tailwind CSS 安装 Tailwind CSS

### 顺风 CSS 框架的文档。

tailwindcss.com](https://tailwindcss.com/docs/guides/nextjs) 

然后我们清理项目，从一个几乎空的`**index.js**`文件开始。

## 配置商店

首先，我们定义我们在`**store/store.js**`的商店

这基本上定义了我们的钩子( *useCart* )，它是使用 *create* 方法初始化的，store(它被扩展为 *persist* one)将包含一个由 *total* (一个表示购物车总值的数字)、 *totalqty* (一个包含购物车中商品总数的数字)、 *cartContent* (一个包含购物车中商品的数组，它包含商品的详细信息，如数量、id、名称
接下来，我们定义将更新( *set* )状态的函数:

*   ***addTocart*** 将一个产品添加到 *cartContent* 中，并增加 *total* 和 *totalqty*
*   ***updatecart*** 和前面的一样，只是在 *cartContent* 中增加一个新行，更新一个现有行的数量。
*   ***clearCart*** 将状态复位到初始(空)值
*   ***remove from cart***从 cartContent 中删除一行并更新*总计*和*总计数量*

文件末尾指定的名称是 localStorage 中对象的名称，在这里你也可以指定持久化的存储方法([更多信息在官方文档上](https://github.com/pmndrs/zustand/wiki/Persisting-the-store's-data)

## **产品**

为了简单起见，避免数据库设置，我们将产品作为对象保存在文件`**lib/products.js**`中

每个产品都是一个对象，具有 id、名称、价格和图像细节(图像存储在 Nextjs `**public**` 文件夹中)。根据您的需要/喜好添加任意数量的产品。

## 页眉

在本例中，我们将使用一个公共标题来演示组件中状态的使用以及跨不同页面的状态持久性:

标题将始终显示购物车的价值和购物车中的产品数量。如前所述，由于我们使用的是 Nextjs，页面呈现的方式(SSR 和 SSG)不允许直接访问客户机上的持久状态，所以解决方法是在组件挂载后访问它。在这种情况下，我们没有访问和呈现来自 **useCart** 钩子的 *total* 和 *totalqty* ，但是我们呈现了 *mytotal* 和 *mytotalqty* ，这是两个局部状态，当它们发生变化时被设置为 Zustand 状态。基本上，这是因为页面是在服务器上呈现的，在这个呈现过程中，它们不能访问客户端存储来读取值；一旦页面被水合，就访问客户端存储并检索值；此时，客户端值与服务器值发生冲突，从而导致警告。有了这个“双渲染”就解决问题了。

在 Nextjs 中解决这个问题的另一种方法是也远程存储状态(例如在 Redis 上)，这样在服务器呈现时它们不会不匹配。

## 页

该网站将有 3 个页面，共享一个共同的标题:与产品列表的索引页，通过单击一个产品，它将被添加到购物车；将显示购物车内容并允许从购物车中移除产品的购物车页面；只是占位符的“关于”页面。

先说最简单的一个`**pages/about.js**`:

这个页面除了包含标题之外什么也不会做

主页面将会是`**pages/index.js**`:

在这个页面上，我们循环我们的产品，并在一个网格中打印出来，我们将一个动作附加到每个产品点击(***【add product(…)***)上，这样每次点击一个产品时，产品本身就会通过 *useCart* 钩子中的*函数*被添加(或更新)到购物车中

购物车页面将会是`**pages/cart.js**`:

该页面将循环显示购物车中的产品，并添加一个按钮将它们从购物车中移除。和在 Header 组件中一样，我们需要使用水合后用 Zustand 状态填充的本地状态。

如果您现在运行该项目:

```
**npm run dev**
```

并将您的浏览器指向:

```
http://localhost:3000
```

你应该得到我们虚构的电子商务网站，点击一个产品会将其添加到购物车中(并且更新总数和数量状态会更新标题)，在页面之间移动不会重置购物车(购物车保存在 localStarage 中)；转到购物车页面，您可以删除产品(从而更新标题)。

![](img/96f185976781aaee77956ad795815a3f.png)

## 下一步是什么

这只是一个基本的例子，说明 Zustand 如何用于基本的(和不那么基本的)状态管理，当然，这不是一个完整的电子商务，状态管理也没有针对生产环境进行优化。无论如何，这可以作为一个起点，特别是对于需要将其集成到 Nextjs 项目中的开发人员来说

你可以给我买杯咖啡来支持我的工作