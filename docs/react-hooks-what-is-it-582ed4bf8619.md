# 反应钩——这是什么？

> 原文：<https://blog.devgenius.io/react-hooks-what-is-it-582ed4bf8619?source=collection_archive---------10----------------------->

![](img/eed41a8140b5b58f1dc2bac119c6010e.png)

不是这些钩子！

React 挂钩是 React 库的新增功能，它使得在不编写类的情况下使用状态和其他 React 特性成为可能。这允许开发人员编写功能组件而不是类，使代码更具可读性和可重用性。

下面是一个使用`useState`钩子来管理自己状态的功能组件的例子:

```
import { useState } from 'react';

function Example() {
  // The useState hook takes an initial state as an argument
  // and returns an array with the current state value and a
  // function to update it
  const [count, setCount] = useState(0);
  // This function uses the setCount function to update the
  // state when it is called
  const increment = () => setCount(count + 1);
  // This component renders a button that, when clicked,
  // calls the increment function to update the state
  return (
    <button onClick={increment}>
      You clicked me {count} times
    </button>
  );
}
```

钩子的一个主要好处是，它们允许开发人员在功能组件中使用状态和其他 React 特性。以前，状态只能在类组件中使用，这意味着功能组件仅限于表示性组件。有了钩子，功能组件现在可以有自己的状态，使它们更加通用和有用。

下面是一个使用`useEffect`钩子执行副作用的功能组件的例子:

```
import { useState, useEffect } from 'react';

function Example() {
  // The useState hook is used to manage the component's state
  const [count, setCount] = useState(0);
  // The useEffect hook is called whenever the component is rendered
  // It takes a function that contains the side effects to be performed
  useEffect(() => {
    // This function is called whenever the component is rendered
    // It fetches some data and updates the state with the result
    fetch('https://api.example.com/count')
      .then(response => response.json())
      .then(data => setCount(data.count));
  });
  // This component renders the current value of the state
  return (
    <div>
      The current count is {count}.
    </div>
  );
}
```

钩子的另一个优点是它们使代码更易读，更容易理解。因为钩子允许开发人员在功能组件中使用状态和其他 React 特性，所以用钩子编写的代码通常比用类编写的代码更简洁、更容易理解。这可以让新开发人员更容易阅读和理解现有代码，也可以让有经验的开发人员更容易编写新代码。

除了使代码更具可读性并允许功能组件具有状态之外，钩子还启用了以前只对类组件可用的其他特性。例如，`useEffect`钩子允许功能组件执行副作用，比如获取数据或更新 DOM。这使得将功能组件用于更广泛的任务成为可能，也使得编写更加模块化和可重用的组件变得更加容易。

![](img/7400c0b7ae9d9f9138dbe80aaea34063.png)

useEffect，使用中。

总的来说，React 钩子是 React 库的一个强大的补充，它使得编写更加通用、更易于阅读和理解的功能组件成为可能。它们受到了开发人员社区的好评，并且很可能在未来成为许多 React 应用程序的核心部分。有关更多信息，请参考[文档](https://reactjs.org/docs/hooks-intro.html)。