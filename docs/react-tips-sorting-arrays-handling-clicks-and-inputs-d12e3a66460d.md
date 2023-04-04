# React 提示—排序数组、处理点击和输入

> 原文：<https://blog.devgenius.io/react-tips-sorting-arrays-handling-clicks-and-inputs-d12e3a66460d?source=collection_archive---------2----------------------->

![](img/8db0b02c16bc18445e8c4564db3f3036.png)

Luiza Sayfullina 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React 是一个用于创建 web 应用程序和移动应用程序的流行库。

在本文中，我们将了解一些编写更好的 React 应用程序的技巧。

# 对 React 中的对象数组进行排序并渲染

我们可以用`sort`方法对一组对象进行排序。

然后我们可以调用`map`来呈现排序后的条目。

例如，在`render`方法或函数组件中，我们可以写:

```
const myData = this.state.data
  .sort((a, b) => a.name > b.name ? 1 : -1)
  .map((item) => (
     <div key={item.id}> {item.name}</div>
  ));
```

我们调用`sort`方法按照`name`属性对数据进行排序。

我们检查`a.name > b.name`是否是`true`。

字符串可以直接比较。

# 在 React 中使用 onClick 和 div

如果我们给它提供一个尺寸，我们可以用 div 来使用`onClick`。

例如，我们可以写:

```
class App extends React.component {
  constructor() {
    this.state = {
      color: 'black'
    };
  }, changeColor() {
    const newColor = this.state.color == 'white' ? 'black' : 'white';
    this.setState({
      color: newColor
    });
  }, render() {
    return (
       <div>
          <div
             style = {{
                background: this.state.color,
                width: 100,
                height: 100
             }}
             onClick = {this.changeColor}
          >
          </div>
      </div>
    );
  }
}
```

我们通过传入一个带有`style`属性中样式的对象来创建一个带有背景的 div。

我们将`width`和`height`设置为 100px，这样我们就可以显示一个内部空无一物的 div。

然后我们可以向`onClick`道具传递一个点击处理程序。

在`changeColor`方法中，我们只是在`color`状态的白色和黑色之间切换。

# 在两个字符串之间添加一个
标签

我们可以在字符串中添加一个换行符来添加一个换行符。

然后我们可以使用`white-space: pre-line`样式来显示它们。

例如，我们可以写:

```
render() {
   message = `Hello \n World.`;
   return (
       <div className='new-line'>{message}</div>
   );
}
```

在我们的组件中。

然后我们可以添加下面的 CSS:

```
.new-line {
  white-space: pre-line;
}
```

# 在 React 组件中提交表单后清除输入值

我们可以在表单提交后在 React 组件中清除输入值。

为此，我们写道:

```
onHandleSubmit(e) {
  e.preventDefault();
  const name = this.state.name;
  this.props.searchByName(name);
  this.setState({
    name: ''
  });
}
```

我们调用`preventDefault`来停止默认的提交行为。

然后我们从我们输入的`state`中得到`name` 。

然后我们调用搜索函数。

最后，我们通过给输入值分配一个空字符串来清除`name`状态，从而清除输入值。

# 在 React 中编辑多个输入控制组件

我们可以通过提供自己的变更处理程序来编辑多个输入控制的组件。

例如，我们可以写:

```
class Form extends React.Component {
  constructor() {
    this.state = {};
  } changeFirstName(event) {
    const contact = this.state.contact;
    contact.firstName = event.target.value;
    this.setState({ contact });
  } changeLastName(event) {
    const contact = this.state.contact;
    contact.lastName = event.target.value;
    this.setState({ contact });
  } changePhone(event) {
    const contact = this.state.contact;
    contact.phone = event.target.value;
    this.setState({ contact });
  } render() {
    return (
      <div>
        <input type="text" onChange={this.changeFirstName.bind(this)} value={this.state.contact.firstName}/>
        <input type="text" onChange={this.changeLastName.bind(this)} value={this.state.contact.lastName}/>
        <input type="text" onChange={this.changePhone.bind(this)} value={this.state.contact.phone}/>
      </div>
    );
  }
}
```

我们有 3 个输入，一个用于名，一个用于姓，一个用于电话号码。

然后，在每个处理程序中，我们用`event.target.value`获取值，然后将其分配给状态。

每个处理程序都被传递到每个输入的`onChange`属性中。

我们必须调用`bind(this)`来返回一个函数，该函数将组件实例作为`this`的值。

`value`设置为事件处理程序改变的状态。

这样，将显示输入的值。

更好的方法是用计算出的属性键动态设置名称。

例如，我们可以写:

```
class Form extends React.Component {
  constructor() {
    this.state = {};
  }
    handleChange(propertyName, event) {
    const contact = this.state.contact;
    contact[propertyName] = event.target.value;
    this.setState({ contact: contact });
  } render() {
    return (
      <div>
        <input type="text" onChange={this.handleChange.bind(this, 'firstName')} value={this.state.contact.firstName}/>
        <input type="text" onChange={this.handleChange.bind(this, 'lastName')} value={this.state.contact.lastName}/>
        <input type="text" onChange={this.handleChange.bind(this, 'phone')} value={this.state.contact.phone}/>
      </div>
    );
  }
}
```

我们有一个用于所有 3 个输入的变更处理程序。

然后，我们可以将该字段作为`bind`的第二个参数传入，将其设置为处理程序的第一个参数。

![](img/1db37ddd8149222b77cf8abafd3b978f.png)

照片由[александргоссс](https://unsplash.com/@boney?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以为每个输入添加更改处理程序，这样我们就可以将它们的输入值更新为新的状态值。

我们可以对数组进行排序并呈现它们。

此外，如果我们为它设置一个大小，我们可以使空的 div 可点击。