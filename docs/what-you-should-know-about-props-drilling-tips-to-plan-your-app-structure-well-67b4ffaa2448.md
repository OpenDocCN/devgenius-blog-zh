# 1 开发人员在学习 ReactJs 时常犯的错误

> 原文：<https://blog.devgenius.io/what-you-should-know-about-props-drilling-tips-to-plan-your-app-structure-well-67b4ffaa2448?source=collection_archive---------2----------------------->

## 指导更好地反应道具规划

![](img/23408c5b12c6407cc95f6cc74075187d.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[felipelaquim](https://unsplash.com/@felipepelaquim?utm_source=medium&utm_medium=referral)拍摄的照片

在像 React 这样的 Javascript 框架中，术语 props 对应于传递给组件的变量。web 应用程序中的组件是网页的构建块。Web 应用程序可以包含数百个组件，其中的数据可以通过 Props 跨组件共享。

# 为什么道具演练是一个问题

这篇文章将带你了解道具训练在反应中可能引起的一些问题。

让我们看下面的例子:

```
const App = () => {
 ....
 const [count, setCount] = useState();
 ....
 return (<div>
   <Component1 count={count}/>
 </div>);
}const Component1 = ({count}) => <div>
 <Component2 count={count}/>
 ... some more stuff 
</div>const Component2 = ({count}) => <div>
 <Component3 count={count}/>
 ... some more stuff
</div>const Component3 = ({count}) => <div>
 // do something with count
</div>
```

*Props drilling* 在本例中也称为( *threading* )是当 Component3 需要访问状态变量 count 时，我们将该 Props 从< app >向下传递到< Component1 >并一直向下传递到< Component3 >。

尽管道具训练有助于保持单一的真相来源，因此是有益的，但它也会引发许多问题。

# **让我们来看看道具训练可能引起的一些问题**

> [在 React 中，默认情况下，当你的组件的状态或者道具改变时，你的组件会重新渲染。](https://reactjs.org/docs/components-and-props.html)

这意味着如果 count 更新，订阅它的每个组件都将重新呈现。了解这一点应该让我们思考我们应用程序的整个结构，并质疑在应用程序级别定义计数的必要性。当我们将道具向下传递给不需要它的组件，只是为了让它们在树中进一步向下传递给需要它的孩子，从而导致整个树重新渲染时，这甚至会变得更成问题。

让我们来看看这个片段

```
const App = () => {
 ....

 ....
 return (<div>
   <Component1/>
  </div>);
}const Component1 = () => <div>
 <Component2 />
 ... some more stuff 
</div>const Component2 = () => <div>
 <Component3 />
 ... some more stuff
</div>const Component3 = () => {
 const [setCount,count] = useState();
 return (<div>
  // do something with count
 </div>)
}
```

React 的行为会和这个结构一样吗？显然，它只会在每次计数更新时重新渲染<component3>。</component3>

# 让我们来看另一个道具演练**问题**

在上面的例子中，钻道具可能不是问题(性能方面),因为应用程序相当小。然而，随着应用程序的增长，我们可能会偶然发现一些用例，使我们的应用程序性能下降，或者使我们的编码更加耗时。

**1-重命名道具**变得更加困难，因为我们需要手动找到每个订户的道具并重命名，这可能需要一些时间。如果应用程序非常复杂和庞大，这可能会很乏味和有压力。

**2-重构组件**或改变它们的结构肯定会使跟踪转发的属性变得更加困难，从而导致一些问题，如可读性更差的代码和未使用的属性，使组件不必要地重新呈现。

# **会有什么样的工作？**

道具钻取**的发生是有原因的**，通常当开发人员设计他们的应用程序结构时，他们错过了规划应用程序的依赖注入，换句话说，他们没有考虑如何将这些数据提供给他们指定的 UI 容器，导致一个混乱的组件结构和一个流传下来的道具线程。

这里有一些**提示**，我发现有助于避免陷入这个问题，
仔细考虑你的应用程序，将你的数据分成两部分:

**A.** **UI 相关数据**，像`isToggled shouldPopUp isActive`之类的东西，这些东西应该放在离它们的 UI 消费者很近的地方，当我们发现自己在许多组件之间共享一个道具的时候，这是我们的组件树结构没有被很好地设计的一个迹象，我们总是可以利用像 [React Context API](https://reactjs.org/docs/context.html) 这样的概念来控制我们的应用程序的依赖注入和优化应用程序结构。

**B.** 我们可以利用像 Flux 这样的状态管理概念，一个著名的工具是 Redux。上下文 Api 在这里是部分有用的，因为这两个概念都可以很好地服务于这个用例。

# **结论**

在开始写代码之前规划你的应用程序，肯定会有助于减少在重构上花费的时间和精力。向下传递道具在向下传递 3 级时被认为是钻取，这是无计划 UI 结构的标志。