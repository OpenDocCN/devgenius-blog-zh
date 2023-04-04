# 创建您的第一个自定义 react 挂钩

> 原文：<https://blog.devgenius.io/creating-your-first-custom-react-hook-6a812ab031fd?source=collection_archive---------7----------------------->

如果您不熟悉 React 挂钩，它们是跨功能组件重用有状态逻辑和行为的一种方式。它们是在 React 16.8 中引入的，并且已经成为管理组件中的状态和副作用的流行方法。

![](img/0435294d5c8e0c10b7a086ec9a37c59e.png)

钩子的一个常见用例是从本地存储中存储和检索数据，本地存储是一种 web 存储，允许您在浏览器中存储数据，并在页面刷新之间保存数据。在本教程中，我们将创建一个名为`useLocalStorage`的定制钩子，使得在 React 组件的本地存储中存储和检索数据变得容易。

首先，让我们使用 Create React App 创建一个新的 React 项目。一旦项目建立，打开你的`App.js`文件，从 React 导入`useState`钩子。我们稍后还需要`useEffect`钩子，所以继续导入它。

接下来，让我们创建我们的`useLocalStorage`钩子。在您的项目中，创建一个名为`useLocalStorage.js`的新文件，并添加以下代码:

```
import { useState, useEffect } from 'react';

export default function useLocalStorage(key, initialValue) {
  const [storedValue, setStoredValue] = useState(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.log(error);
      return initialValue;
    }
  });

  const setValue = value => {
    try {
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      setStoredValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.log(error);
    }
  };

  return [storedValue, setValue];
}
```

这个钩子有几个作用:

1.  它使用`useState`钩子创建一个名为`storedValue`的状态，该状态被初始化为在`key`参数中指定的键下存储在本地存储器中的值。如果在本地存储器中没有找到值，它使用`initialValue`参数作为默认值。
2.  它创建了一个名为`setValue`的函数，允许您更新`storedValue`的值并将其保存到本地存储中。
3.  它返回一个带有`storedValue`和`setValue`的数组，就像`useState`钩子一样。

## 现在我们有了我们的`useLocalStorage`钩子，让我们在我们的`App`组件中使用它。首先，导入`App.js`文件顶部的钩子:

```
const App = () => {
  const [name, setName] = useLocalStorage('name', '');

  return (
    <div>
      <input
        type="text"
        placeholder="Enter your name"
        value={name}
        onChange={e => setName(e.target.value)}
      />
    </div>
    );
   };
```

这里，我们使用 **useLocalStorage** 钩子创建一个名为 **name** 的状态，它被初始化为一个空字符串。我们还将**名**键传入挂钩，这样**名**的值将存储在本地存储中的**名**键下。现在，每当您在输入字段中输入内容时，名称**的值就会被更新并保存到本地存储中。您可以刷新页面，由于本地存储的魔力， **name** 的值仍然存在！使用 **useLocalStorage** 挂钩还可以做一些其他的事情。**

例如，当组件挂载时，您可以使用 **useEffect** 钩子从本地存储中自动检索值:

```
useEffect(() => {
const storedName = window.localStorage.getItem('name');
if (storedName) setName(storedName);
}, []);
```

## 您也可以向 **setValue** 传递一个函数，根据之前的值更新该值:

```
setValue(prevName => prevName + '!');
```

我希望本教程对您有所帮助，向您展示了如何创建和使用自定义钩子来存储和检索 React 组件中本地存储的数据。编码快乐！

如果你喜欢这个博客，请关注我。下一篇博客再见。