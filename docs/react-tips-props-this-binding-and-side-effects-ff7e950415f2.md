# 反应提示——道具、绑定和副作用

> 原文：<https://blog.devgenius.io/react-tips-props-this-binding-and-side-effects-ff7e950415f2?source=collection_archive---------14----------------------->

![](img/6372d9c2cbf9381eb74813683e015ddb.png)

Joshua J. Cotten 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 有条件地将属性内联传递给组件

我们可以通过传入一个表达式作为 prop 值，有条件地将 prop 内联到一个组件。

例如，我们可以写:

```
<Child {...(this.props.canEdit ? { editOptions : this.state.options } : undefined)} >
```

我们检查道具看它是否真实。

如果是，那么我们把`editOptions`道具传递给这个孩子。

否则，我们就通过了`undefined`。

我们使用 spread 操作符将返回的对象分散到道具中。

# 在 React 组件中使用一个或多个 useEffect 挂钩

我们可以随意使用`useEffect`挂钩。

例如，我们可以写:

```
useEffect(() => {
  // run code
  return () => {
    // run clean up code
  }
}, []);useEffect(() => {
  // run code when props.foo changes
  return () => {
     // run clean up code when props.foo changes
  }
}, [props.foo])
```

第一个`useEffect`调用用于在组件加载时运行代码。

我们返回的函数在组件卸载时运行。

在第二个`useEffect`调用中，当`props.foo`改变时，我们运行代码。

当`props.foo`改变时，返回的函数运行清理代码。

它对于触发任何副作用都很有用，比如 API 调用或者类似的事情。

# 将属性对象传递给子组件

我们可以使用 object spread 操作符将对象的属性传递给子组件。

例如，我们可以写:

```
return <Child {...props} />;
```

`props`对象中的所有属性将作为道具传入`Child`。

# 避免将“this”绑定到每个方法

通过使用类字段，我们可以避免将所有方法绑定到`this`。

这不是 JavaScript 语法的一部分，但它可以用于 Babel 或 TypeScript。

例如，我们可以连接:

```
class Foo extends React.Component { onClick = () => {
    console.log('clicked');
  }; onMouseOver = () => {
    console.log('mouse over');
  }; render() {
    return (
      <div
        onClick={this.onClick}
        onMouseOver={this.onMouseOver}
      />
    );
  }}
```

它还在第三阶段，所以它不是最终的。

# 反作用钩子中的推动方法

我们不能使用`push`方法将一个项目追加到一个数组中，因为它返回被追加的项目并改变数组的位置。

相反，我们必须返回一个新的数组，其中包含我们想要添加的项。

例如，我们可以写:

```
const [array, setArray] = useState(initialArray);
```

然后我们写道:

```
setArray(oldArray => [...oldArray, newItem]);
```

我们获取旧的数组，并返回一个新的数组，其中添加了条目。

使用 spread 操作符将现有项目扩展到新数组中。

# PropTypes 使用动态键检查对象

在验证 props 时，我们可以使用一个函数来检查带有动态键的对象。

例如，我们可以写:

```
someProp: (props, propName, componentName) => {
  if (!/match/.test(props[propName])) {
    return new Error('match prop doesn't exist');
  }
}
```

如果`someProp`的值匹配我们指定的正则表达式模式，我们检查`someProp`的值。

如果没有，那么我们抛出一个错误。

`props`拥有作为对象的道具。

`propName`有道具的名字。

`componentName`有组件名。

# 反应属性类型:对象对形状

`objectOf`方法让我们检查一个属性值是否属于某种类型。

例如，我们可以写:

```
number: PropTypes.objectOf(PropTypes.number)
```

检查`number`属性的值是否是一个对象。

另一方面，`shape`让我们检查对象的结构。

例如，我们可以写:

```
styleObj: PropTypes.shape({
  color: PropTypes.string,
  fontSize: PropTypes.number
}),
```

然后`styleObj`道具必须有`color`属性，这是一个字符串。

它应该有一个`fontSize`属性，这是一个数字。

![](img/be1c8ad5b8d8053f408c65a6bda43ec1.png)

[Teddy sterblom](https://unsplash.com/@teddyosterblom?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

有各种方法来检查我们作为道具传入的对象的形状。

通过指定一个三元表达式，我们可以有选择地将属性传递给一个对象。

此外，我们可以在组件中多次使用`useEffect`。

动态键也可以用 prop-types 包中的自定义函数来检查。