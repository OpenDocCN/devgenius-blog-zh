# React 中的类—还不错。

> 原文：<https://blog.devgenius.io/class-in-react-isnt-bad-653e98a628fb?source=collection_archive---------11----------------------->

![](img/475e3fb9e2124eda36b2820c89336172.png)

由 [Kvistholt 摄影](https://unsplash.com/es/@freeche?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

如你所知，React 试图从**类组件**迁移到**功能组件**中，但它不能完全做到，并且**类组件**与功能组件有所不同，功能组件尚未添加。

## **最受欢迎的案例:**

> **useEffect** 为异步，**componentid mount**为同步。

可能你看到的最常见的情况是，当我们用包装器绑定一个 ref 并试图查看 ref 的宽度时，如果你使用一个 **useEffect** 我们将会看到`0 -> width(px)`但是 **componentDidMount** 会立即这样做。

主要原因是**使用效果**和**组件安装**工作机构的不同，尤其是安装阶段的不同；

1.  在类组件中，**componentdimount**生命周期的机制除了 **async** 回调之外，都立即设置状态函数。
2.  `render`方法创建一个新的 **VRDOM** 。

> *一般来说，****use effect****在油漆提交后运行。*

## UPD: React 18 改变了使用效果机制。

如果你使用的是 **createRoot** 方法——所有的 React setState 函数都会被同步刷新。

```
ReactDOM.createRoot(rootElement).render(<App />)
```

但是**方法**渲染不允许这么做。

```
ReactDOM.render(<App />, rootElement)
```

> *主要区别* ***使用效果*** *和* ***组件卸载*** *是这个生命周期实现时的一个阶段。*

**正如你所理解的，useEffect 可能是类和功能组件之间区别的最流行的例子。**

> 但这还不是全部，除此之外，它们之间还有其他不同之处。

## React 中的类，但不涉及类组件。

考虑到上次关于 React 中**类** vs **泛函组件**的讨论——很多学弟认为 React 中**类**不好。

React 开发人员试图将**函数式编程范式**融入他们的哲学中，因为这一点，许多低年级学生认为 **FP** 比 **OOP** 更好，但事实并非如此。

**让我们以 OOP 的基本示例以及如何创建和管理应用商店为例:**

**MobX:**

```
class Profile {
  is_exit = false;

  constructor() {
    makeAutoObservable(this);
  }

  exitFromAccount() {
    this.is_exit = !this.is_exit;
  }
}

const account = new Profile();

const Block = observer(() => {
  console.log(account.is_exit);
  return (
    <div>
      <button
        onClick={account.exitFromAccount}>
          Click!
      </button>
    </div>
  );
});
```

这是一个关于 MobX 如何工作的非常简单的例子。

> *我不会只讲概念就讲它是如何工作的。*

**MobX** 使用**函数反应式编程**，但是我们使用了一个类来描述我们的商店模型，乍一看这可能会产生一些疑问。

> 没有错误的编程范式，只有特定情况下的正确范式。

在这个 **MobX** 片段中，我们使用 **OOP** 表示数据，但是嵌套函数允许你使用 **MobX** 使用 **FP【函数范例】**。

`makeAutoObservable` -是一个数学函数或清零函数，这是 **FP** 的主要标志。

[](/mcrtip-1-difficult-about-simple-2df82ad79e52) [## MCRTIP #1 /简单的困难

### MCRTIP 只是我关于编程思想的汇编。仅依我看。

blog.devgenius.io](/mcrtip-1-difficult-about-simple-2df82ad79e52) 

> 你可以在这篇文章中阅读更多关于数学函数的内容。

## PromakeAutoObservablegramming 范例对商店经理很重要。

我不会谈论 **FP** 和 **OOP** 之间的区别以及哪个更好——让我们看看其他的状态管理器和他们选择的**编程范例**。

**Redux:**

```
import { createSlice, configureStore } from '@reduxjs/toolkit'

const counterSlice = createSlice({
  name: 'counter',
  initialState: {
    value: 0
  },
  reducers: {
    incremented: state => {
      state.value += 1
    }
  }
});

const { incremented, decremented } = counterSlice.actions

const store = configureStore({
  reducer: counterSlice.reducer
});

store.subscribe(() => console.log(store.getState()));
store.dispatch(incremented());
```

> ****Redux****大概是最受欢迎的状态管理器和许多后辈学习的****React****和****Redux****。**

*有方法`subscribe, dispatch`和明确的函数`configureStore, createSlice`。换句话说， **Redux** 使用了 **FP** 和 **OOP** 范例，我们可以用公式表达这种思想:*

> *我们可以将 FP 和 OOP 范例结合成一个模型。*

## *我们可以选择的主要结论。*

1.  ***反应 18** 中的**类**和**功能组件**有区别，但没有那么严重。*
2.  *您可以在同一个项目**中成功地将**功能范式**用于**视图**，将 **OOP** 用于**模型**。***
3.  *这证明了使用 **OOP** 管理商店和使用 **FP** 改变商店行为和来自商店的表示数据的合理性。*