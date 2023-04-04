# 用 React Select 创建下拉菜单

> 原文：<https://blog.devgenius.io/creating-dropdown-menus-with-react-select-94f053123a5e?source=collection_archive---------2----------------------->

![](img/1d373d79494b2999af6b33e71a3e5c86.png)

[Lucija Ros](https://unsplash.com/@lucija_ros?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

React Select 是 React 应用程序的下拉菜单库。

它支持许多常规下拉菜单不支持的东西。

在本文中，我们将了解如何使用 React Select 添加菜单。

# 入门指南

我们可以通过运行以下命令来安装该软件包:

```
yarn add react-select
```

或者:

```
npm i react-select
```

然后我们可以通过写来使用它:

```
import React from "react";
import Select from "react-select";const options = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];export default function App() {
  return (
    <>
      <Select options={options} />
    </>
  );
}
```

我们添加了一个`options`数组，每个条目都有`value`和`label`属性。

然后我们将它传递给`Select`组件。

现在标签显示在下拉列表中。

# 动画

我们可以用`makeAnimated`功能添加动画。

例如，我们可以写:

```
import React from "react";
import Select from "react-select";
import makeAnimated from "react-select/animated";
const animatedComponents = makeAnimated();const options = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];export default function App() {
  return (
    <>
      <Select options={options} components={animatedComponents} />
    </>
  );
}
```

我们将`animatedComponents`作为`components`道具的值传递，我们将看到一些动画。

# 自定义样式

我们可以用一个对象添加自定义样式。

例如，我们可以写:

```
import React from "react";
import Select from "react-select";const dot = (color = "#ccc") => ({
  alignItems: "center",
  display: "flex",":before": {
    backgroundColor: color,
    borderRadius: 10,
    content: '" "',
    display: "block",
    marginRight: 8,
    height: 10,
    width: 10
  }
});const styles = {
  control: styles => ({ ...styles, backgroundColor: "white" }),
  option: (styles, { data, isDisabled, isFocused, isSelected }) => {
    return {
      ...styles,
      backgroundColor: "green",
      color: "#ccc",
      cursor: isDisabled ? "not-allowed" : "default",
      ":active": {
        ...styles[":active"],
        backgroundColor: "orange"
      }
    };
  },
  input: styles => ({ ...styles, ...dot() }),
  placeholder: styles => ({ ...styles, ...dot() }),
  singleValue: (styles, { data }) => ({ ...styles, ...dot(data.color) })
};const options = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];export default function App() {
  return (
    <>
      <Select options={options} styles={styles} />
    </>
  );
}
```

我们创建了一个`dot`函数，它返回一个带有点样式的对象。

`:before`包含了在文本左侧创建一个点的所有内容。

`styles`具有控件、选项、输入、占位符和选定值的各种属性。

我们可以根据`isFocused`属性设置样式，该属性指示选择是否处于焦点中。

`isSelected`表示选择是否被选中。

`data`是该项的数据。

然后，我们将所有这些内容传递给`styles` prop。

# 多选

我们可以使用`isMulti`道具启用多重选择。

例如，我们可以写:

```
import React from "react";
import Select from "react-select";const options = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];export default function App() {
  return (
    <>
      <Select options={options} isMulti />
    </>
  );
}
```

现在我们可以从列表中选择多个选项。

# 异步加载选择

我们可以以异步方式加载选择。

为此，我们使用了`AsyncSelect`组件:

```
import React from "react";
import AsyncSelect from "react-select/async";const options = [
  { value: "apple", label: "Apple" },
  { value: "orange", label: "Orange" },
  { value: "grape", label: "Grape" }
];const filterOptions = inputValue => {
  return options.filter(i =>
    i.label.toLowerCase().includes(inputValue.toLowerCase())
  );
};const loadOptions = (inputValue, callback) => {
  setTimeout(() => {
    callback(filterOptions(inputValue));
  }, 1000);
};export default function App() {
  const [inputValue, setInputValue] = React.useState("");
  const handleInputChange = newValue => {
    const inputValue = newValue.replace(/\W/g, "");
    setInputValue(inputValue);
    return inputValue;
  }; return (
    <div>
      <pre>inputValue: "{inputValue}"</pre>
      <AsyncSelect
        cacheOptions
        loadOptions={loadOptions}
        defaultOptions
        onInputChange={handleInputChange}
      />
    </div>
  );
}
```

我们给出了按值过滤的`filterOptions`方法。

`loadOptions`让我们异步加载选项。

它有一个`inputValue`参数，当我们输入一些东西时，这个参数会被传入。

在`App`组件中，我们用输入框设置`inputValue`。

此外，我们有`cacheOptions`来缓存选项。

`defaultOptions`确定何时触发选项请求。

![](img/136faa1dcf2a78f93de786092bfec8af.png)

彼得·奥斯拉内克在 Unsplash[拍摄的照片](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 结论

我们可以使用 React Select 轻松创建下拉列表。

我们可以创建一个下拉菜单，支持选项的多重选择、样式和异步加载。