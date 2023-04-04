# 关于 2022 年的 React 环境，您需要知道的一切，第 2 部分

> 原文：<https://blog.devgenius.io/everything-you-need-to-know-about-react-context-in-2022-part-2-c2ce63b3f7f3?source=collection_archive---------6----------------------->

## 文章节选

## *摘自* [*反应迅速，S*second*Edition*](https://www.manning.com/books/react-quickly-second-edition?utm_source=medium&utm_medium=referral&utm_campaign=book_mardan2_react_2_3_22)*by Morten Barklund*

*本节选探讨了 React 上下文的使用。*

如果你是 React 开发人员或者想了解更多关于 React 的信息，请阅读这篇文章。

快速反应，在[manning.com](https://www.manning.com/books/react-quickly-second-edition?utm_source=medium&utm_medium=referral&utm_campaign=book_mardan2_react_2_3_22)结账时将 **fccmardan2** 输入折扣代码框。

在这里查看第 1 部分。

## **React 语境解构**

让我们后退一步，仔细看看 React 上下文。如前所述，要使用 React 上下文，您需要创建一个提供者和一个消费者。您可以通过两种不同的方式创建消费者，要么作为一个挂钩，要么通过使用“渲染道具”。但是在做这些之前，您需要创建上下文本身。您可以通过使用 React 包中的函数`createContext`来实现:

```
import { createContext } from 'react';
 const MyContext = createContext(defaultValue);
```

这里要注意两件事:

*   用大写字母命名上下文变量是很常见的，因为它有点像 React 组件(或者至少是其中的属性)。
*   `createContext`采用单个参数，这是默认值。我们一会儿会回到这是如何发生的。

## 使用上下文

当你有一个上下文变量时，比如上面的 MyContext，它有两个属性，这是我们所关心的:MyContext。Provider 和 MyContext.Consumer

我们已经解释了如何使用 useContext 钩子来使用上下文。您可以用 MyContext 做类似的事情。消费者财产，但这有点棘手。

假设您想要显示一个段落，该段落的名称由名为 DisplayName 的组件中最近的名称上下文提供。我们可以使用如下所示的 useContext 挂钩来实现:

```
function DisplayName() {
   const name = useContext(NameContext);
   return <p>{name}</p>
 }
```

这很简单。我们调用钩子并把当前值作为变量取回，我们可以直接在组件中使用。

如果我们试图使用`Consumer`组件做同样的事情，我们必须调用消费者组件，并把一个函数作为第一个也是唯一的子组件，这个函数将用组件的值来调用:

```
function DisplayName() {
   return (
     <p>
       <NameContext.Consumer>
         {(name) => name}
       </NameContext.Consumer>
     </p>
   );
 }
```

您可能会看到这需要更多的输入工作，如果我们需要对返回值进行一些计算或逻辑运算，我们必须对组件进行相当多的重组。

使用`Consumer`组件在函数式代码库中非常少见。它可能只在旧的基于类的组件中使用。

## 语境构成

提供者用于创建可以使用的上下文。消费者用于消费最近的提供的上下文。请注意，您可以通过应用程序多次提供相同的上下文，甚至可以嵌套提供相同的上下文。您还可以多次使用同一个上下文，甚至在任何提供者之外。

当您使用一个上下文时，您将获得由文档中最近的提供者提供的值。如果消费者之上不存在提供者，您将获得我们创建上下文时定义的默认值。让我们在图 10 中展示所有这些。

![](img/44c657ec6ccef8c4430234e8507ba347.png)

图 10 您可以有许多相同上下文的提供者和消费者。

在图 10 中需要注意一些事情:

*   如果您使用一个上面没有提供者的上下文，就像在`TopComponent`中一样，您将从上下文的定义中获得缺省值(本例中为 0)。
*   如果您使用一个上面有多个提供者的上下文，如在`BottomComponent`中，您将通过查找文档树从最近的提供者获得值(例如，在本例中是 17 而不是 2)。

## 嵌套上下文示例

您可以想象 UI 变量的嵌套上下文的用例。例如，让我们想象一个应用程序，其中的按钮在整个应用程序中具有不同的边框宽度。

我们的 web 应用程序是一个网上商店，有不同的购买项目和业务页面。我们在页眉和页脚都有一些按钮。我们在页眉和页脚都有打开购物车的按钮。

默认情况下，所有按钮的边框宽度为 1 像素，但在页脚中，所有按钮的边框宽度为 2 像素。此外，每当我们有一个按钮去购物车，按钮必须始终有一个 5 像素的边框宽度，因为它是一个非常重要的按钮。

让我们首先在图 11 中画出这个系统的草图。

![](img/6942d19fb21094b16fd6a2cbfdad0224.png)

图 11 我们购物网站的组件树。请注意，我们始终有一个默认的上下文值和几个上下文提供者。

现在，每个按钮组件都会在组件树中查找最近的边框上下文提供者，并使用从那里获得的边框宽度。如果在树中没有找到提供者，按钮将使用在最初的上下文创建中定义的默认值。

让我们用图 12 中最近的提供者的所有这些查找来注释树。

![](img/064eef4dfff3a5cf1a7cf21e82598fc8.png)

图 12 组件树，最近的提供者(或根)用一个粗箭头标记每个按钮组件，以及为该组件解析的边框宽度。

让我们继续实施这一切，因为我们已经有了所有需要的信息。我们可以在清单 5 中这样做。

**清单 5 边框宽度由上下文决定。**

```
import { useContext, createContext } from 'react';
 const BorderContext = createContext(1);    #A
 function Button({ children }) {
   const borderWidth = useContext(BorderContext);    #B
   const style = {
     border: `${borderWidth}px solid black`,
     background: 'transparent',
   };
   return <button style={style}>{children}</button>
 }
 function CartButton() {
   return (
     <BorderContext.Provider value={5}>    #C
       <Button>Cart</Button>
     </BorderContext.Provider>
   )
 }
 function Header() {
   const style = {
     padding: '5px',
     borderBottom: '1px solid black',
     marginBottom: '10px',
     display: 'flex',
     gap: '5px',
     justifyContent: 'flex-end',
   }
   return (
     <header style={style}>
       <Button>Clothes</Button>
       <Button>Toys</Button>
       <CartButton />
     </header>
   )
 }
 function Footer() {
   const style = {
     padding: '5px',
     borderTop: '1px solid black',
     marginTop: '10px',
     display: 'flex',
     justifyContent: 'space-between',
   }
   return (
     <footer style={style}>
       <Button>About</Button>
       <Button>Jobs</Button>
       <CartButton />
     </footer>
   )
 }
 function App() {
   return (
     <main>
       <Header />
       <h1>Welcome to the shop!</h1>
       <BorderContext.Provider value={2}>    #D
         <Footer />
       </BorderContext.Provider>
     </main>
   );
 }
```

**#A 我们用默认值 1** 创建初始上下文

**#B 在按钮组件中，我们使用最近的提供者提供的任何值，并将其用作 CSS 中的边框宽度属性**

**#C 我们在购物车按钮内的按钮周围添加了一个边框宽度提供程序，为该按钮提供精确的 5px**

我们用一个提供者来包围页脚，确保所有按钮在默认情况下都有 2px 边框，除非另一个更具体的提供者告诉他们。

## **源代码**

您可以从[https://github . com/rq2e/rq2e/tree/main/ch10/rq10-border-context](https://github.com/rq2e/rq2e/tree/main/ch10/rq10-border-context)下载上述示例的源代码，或者您可以通过运行以下代码片段来初始化一个预填充了此示例的新 React 项目:

```
npx create-react-app rq10-border-context --template rq10-border-context
```

在浏览器中打开它后，我们会看到图 13。

![](img/920a93cb25dda683f269bc8e82d25680.png)

图 13 我们的商店网站显示了所有按钮，宽度与设计完全一致。看起来不太好，但出于某种原因，这是客户想要的！

## **将上下文带入下一个级别**

到目前为止，我们已经将数字和字符串放入上下文中，但是没有什么可以阻止我们将对象或函数放入上下文中。事实上，您甚至可以将一个由许多其他值组成的复杂对象与各种类型的值混合在一起。

一种常见的方法是使用上下文作为有状态值和设置器的交付机制。

例如，让我们想象我们有一个黑暗模式和光明模式的网站。我们在标题中有一个按钮，可以在两者之间切换。所有相关组件将查看当前状态，并根据该状态值更改其设计。

我们希望将两个东西放入状态:一个值告诉我们当前是否处于黑暗模式(`isDarkMode`)，一个函数允许按钮在两种模式之间切换(`toggleDarkMode`)。我们可以把这两个值放在一个对象中，然后把它作为值放入上下文中。

我们将在图 14 中描绘这个系统。

![](img/8b7fb8dcd47f81ddfceb784c07e07ab9.png)

图 14 是我们网站的文档树草图，带有暗模式/亮模式切换。请注意我们是如何将一个对象传递给上下文提供者的，然后我们可以在提供者下面的整个文档树的组件中需要这两个值中的任何一个时，对其进行解构和使用。

让我们在清单 6 中实现它。

## 清单 6 带有上下文的黑暗模式。

```
import { useContext, useState, createContext, memo } from 'react';
 const DarkModeContext = createContext({});    #A
 function Button({ children, ...rest }) {
   const { isDarkMode } = useContext(DarkModeContext);    #B
   const style = {
     backgroundColor: isDarkMode ? '#333' : '#CCC',
     border: '1px solid',
     color: 'inherit',
   };
   return <button style={style} {...rest}>{children}</button>
 }
 function ToggleButton() {
   const { toggleDarkMode } = useContext(DarkModeContext);    #C
   return <Button onClick={toggleDarkMode}>Toggle mode</Button>
 }
 const Header = memo(function Header() {
   const style = {
     padding: '10px 5px',
     borderBottom: '1px solid',
     marginBottom: '10px',
     display: 'flex',
     gap: '5px',
     justifyContent: 'flex-end',
   }
   return (
     <header style={style}>
       <Button>Products</Button>
       <Button>Services</Button>
       <Button>Pricing</Button>
       <ToggleButton />
     </header>
   )
 });
 const Main = memo(function Main() {    #D
   const { isDarkMode } = useContext(DarkModeContext);    #B
   const style = {
     color: isDarkMode ? 'white' : 'black',
     backgroundColor: isDarkMode ? 'black' : 'white',
     margin: '-8px',
     minHeight: '100vh',
     boxSizing: 'border-box',

   }
   return <main style={style}>
     <Header />
     <h1>Welcome to our business site!</h1>
   </main>
 });
 function App() {
   const [isDarkMode, setDarkMode] = useState(false);    #E
   const toggleDarkMode = () => setDarkMode(v => !v);    #E
   const contextValue = { isDarkMode, toggleDarkMode };    #F
   return (
     <DarkModeContext.Provider value={contextValue}>    #G
       <Main />
     </DarkModeContext.Provider>
   );
 }
```

这次我们用一个空对象来初始化我们的上下文。这是因为我们知道在应用程序的根目录下总是有一个上下文，所以永远不会使用默认值。

**#B 在这两个位置，我们仅使用上下文中的 isDarkMode 标志**

**#C 在切换按钮中，我们仅使用上下文中的 toggleDarkMode 函数**

**#D 我们记忆主要成分**

**#E 在主应用程序组件中，我们定义了进入我们上下文的两个值**

**#F 我们把这两个值放在一个对象里**

**#G 我们使用这个对象作为上下文提供者的值**

## **源代码**

您可以从[https://github.com/rq2e/rq2e/tree/main/ch10/rq10-dark-mode](https://github.com/rq2e/rq2e/tree/main/ch10/rq10-dark-mode)下载上述示例的源代码，或者您可以通过运行以下代码片段来初始化一个预填充了该示例的新 React 项目:

```
npx create-react-app rq10-dark-mode --template rq10-dark-mode
```

我们可以在图 15 中观察这个网站的运行。

![](img/4f3634819a04df53cf8b4f8ea4bc021b.png)

图 15 我们的网站分别处于亮模式和暗模式。

这里需要注意的重要事项是，我们如何在示例中的#E 和#F 行中为上下文提供两个不同的属性，以及如何在#D 行中记忆上下文提供程序中的一些组件。这一点非常重要，因为每次上下文发生变化时，我们的主应用程序组件都会重新呈现，也就是每次黑暗模式标志切换时(因为状态更新)。然而，我们不希望所有其他组件仅仅因为上下文而重新呈现。在这个实例中，主组件使用上下文，所以每次上下文更新时它都会重新呈现，但是头不使用上下文，所以它不应该重新呈现。随着我们记忆化的使用，它不会，所以这基本上是完美的。

我们不必就此止步。我们可以将一大堆属性和函数放入上下文值中。事实上，围绕使用功能提供者的上下文有一个完整的概念，称为提供者模式。在 barklund.dev/provider 有一篇关于这种模式的长篇文章，有更多的细节。

## 上下文选择器

在前一个例子中，我们的上下文提供者有一个小的次优问题。问题是，当上下文中的任何值发生变化时，使用特定上下文的所有组件都将重新呈现。

这是因为我们的上下文现在是一个具有多个属性的复杂对象，但是 React 并不真正关心这个。React 只是看到上下文值发生了变化，所以使用该上下文的每个组件都将被重新呈现。

然而，我们的切换组件从不需要重新呈现。切换组件使用可以被记忆为完全稳定的函数。`toggleDarkMode`函数不依赖于上下文的当前值，所以它可以被记忆为稳定的。

不幸的是，当上下文的特定属性更新时，我们不能告诉 React 只重新呈现特定的组件。至少，我们还做不到。这肯定会在未来的更新中出现，但现在还没有。预计它将与 React 18 一起发布，但没有发布。

因此，如果我们想这样做，我们需要使用一个外部库。这个库叫做`use-context-selector`，它不仅允许我们每次都使用整个上下文，还允许我们指定上下文中我们感兴趣的特定属性(我们“选择”相关的属性，因此有了选择器名称)，React 现在只在特定属性改变时才重新呈现我们的组件。

为了正确使用`use-context-selector`包，我们还需要使用这个包来创建我们的上下文。我们不能使用 React 包中的`createContext`创建的常规上下文，而是必须使用`use-context-selector`包提供的`createContext`函数。

让我们继续实现清单 7 中更新的、更优化的黑暗模式切换网站。

## 清单 7 带有上下文选择器的黑暗模式。

```
import { useState, useCallback, memo } from 'react';
 import { createContext, useContextSelector } from 'use-context-selector';    #A
 const DarkModeContext = createContext({});
 function Button({ children, ...rest }) {
   const isDarkMode = useContextSelector(    #B
     DarkModeContext,
     (contextValue) => contextValue.isDarkMode,    #C
   );
   const style = {
     backgroundColor: isDarkMode ? '#333' : '#CCC',
     border: '1px solid',
     color: 'inherit',
   };
   return <button style={style} {...rest}>{children}</button>
 }

 function ToggleButton() {
   const toggleDarkMode = useContextSelector(    #B
     DarkModeContext,
     (contextValue) => contextValue.toggleDarkMode,    #C
   );

   return <Button onClick={toggleDarkMode}>Toggle mode</Button>
 }

 const Header = memo(function Header() {
   const style = {
     padding: '10px 5px',
     borderBottom: '1px solid',
     marginBottom: '10px',
     display: 'flex',
     gap: '5px',
     justifyContent: 'flex-end',
   }
   return (
     <header style={style}>
       <Button>Products</Button>
       <Button>Services</Button>
       <Button>Pricing</Button>
       <ToggleButton />
     </header>
   )
 });

 const Main = memo(function Main() {
   const isDarkMode = useContextSelector(    #B
     DarkModeContext,
     (contextValue) => contextValue.isDarkMode,    #C
   );
   const style = {
     color: isDarkMode ? 'white' : 'black',
     backgroundColor: isDarkMode ? 'black' : 'white',
     margin: '-8px',
     minHeight: '100vh',
     boxSizing: 'border-box',

   }
   return <main style={style}>
     <Header />
     <h1>Welcome to our business site!</h1>
   </main>
 });

 function App() {
   const [isDarkMode, setDarkMode] = useState(false);
   const toggleDarkMode = useCallback(() => setDarkMode(v => !v), []);    #D
   const contextValue = { isDarkMode, toggleDarkMode };
   return (
     <DarkModeContext.Provider value={contextValue}>
       <Main />
     </DarkModeContext.Provider>
   );
 }
```

这次我们只改变了一些东西。我们从使用上下文选择器包中导入两个函数

**#B 每当我们需要来自上下文的值时，我们使用新的 useContextSelector 钩子，而不是普通的 useContext 钩子。**

除了可供选择的上下文之外，我们还提供了一个函数来指定我们对上下文的哪一部分感兴趣。React 现在将确保仅在上下文的特定部分更新时重新呈现该组件。

**#D 最后，我们需要使用 useCallback** 来记忆我们的切换函数

## **源代码**

您可以从[https://github . com/rq2e/rq2e/tree/main/ch10/rq10-dark-mode-selector](https://github.com/rq2e/rq2e/tree/main/ch10/rq10-dark-mode-selector)下载上述示例的源代码，或者您可以通过运行以下代码片段来初始化一个预填充了此示例的新 React 项目:

```
npx create-react-app rq10-dark-mode-selector --template rq10-dark-mode-selector
```

这样做的结果是与我们之前拥有的完全相同的网站具有完全相同的功能——除了现在`ToggleButton`从不重新呈现，因为它只使用上下文中的稳定值，该值从不更新，所以没有必要重新呈现组件。每次标志更新时，在上下文中监听`isDarkMode`标志的两个组件仍然会重新呈现，因为我们在这两个组件的`useContextSelector`挂钩中选择了确切的属性。

在这一点上，这似乎是一种过度优化，因为我们正在讨论单个组件是否会额外更新几次。然而，在具有许多上下文的大型应用程序中，这种情况会越来越多。而且加起来很快！因此，如果你使用上下文来共享整个应用程序的公共功能，你应该使用`useContextSelector`而不是普通的`useContext`钩子。除非在您阅读本文时，React 已经将选择逻辑实现为普通`useContext`钩子的一部分。

## 这有多大用处？

这看起来像是一个小模式，对于某些功能来说可能是聪明的，但它远不止于此。这种模式实际上可以作为在整个应用程序中分发和组织数据和功能的单一方式。

您的应用程序可以在许多不同的层上有几十个不同的上下文，它们相互作用，为部分或全部应用程序提供全局和本地功能。例如，您可以在一个上下文中使用当前用户信息以及登录和注销方法来授权您的用户，在第二个上下文中使用您的应用程序数据，在第三个上下文中使用控制 UI 的数据。

如果使用正确，这是 React 颤中最强大的模式，几乎可以应用于任何应用程序。它具有极强的通用性和通用性，可以适用于任何情况；并且它是可定制的，足以用于许多不同的结构。

有些人可能会说，与其使用 React Context 和`useContextSelector`来管理整个应用程序中的复杂状态，不如使用一个成熟的工具，比如`redux-toolkit`来提供这种功能。但是实话实说:`redux-toolkit`正是利用这种功能来提供它的魔力，所以使用这两种方法你会得到完全相同的性能。

本文到此为止。感谢阅读。