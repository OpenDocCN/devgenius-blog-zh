# 引导 5 —输入组

> 原文：<https://blog.devgenius.io/bootstrap-5-input-groups-6913def6e034?source=collection_archive---------17----------------------->

![](img/4934f7c711e343792499db1d85cd1aa1.png)

[engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

**写这篇文章时，Bootstrap 5 还处于 alpha 版本，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何用 Bootstrap 5 添加输入组。

# 胶料

我们可以用`form-file-lg`或`form-file-sm`类改变文件输入的大小。

例如，我们可以写:

```
<div class="form-file form-file-lg mb-3">
  <input type="file" class="form-file-input" id="large-input">
  <label class="form-file-label" for="large-input">
    <span class="form-file-text">Choose a file...</span>
    <span class="form-file-button">Browse</span>
  </label>
</div><div class="form-file form-file-sm">
  <input type="file" class="form-file-input" id="small-input">
  <label class="form-file-label" for="small-input">
    <span class="form-file-text">Choose a file...</span>
    <span class="form-file-button">Browse</span>
  </label>
</div>
```

添加一个大的和小的文件输入。

# 范围滑块

Bootstrap 5 带有用于范围输入的样式。

例如，我们可以写:

```
<label for="range-input" class="form-label">range input</label>
<input type="range" class="form-range" id="range-input">
```

然后我们有一个范围输入，我们可以拖动手柄。

我们将`type`设置为`range`，并使用`form-range`类对其进行样式化。

# 最小值和最大值

我们可以设置`min`和`max`属性来限制我们可以用它设置的值。

例如，我们可以写:

```
<label for="range-input" class="form-label">range input</label>
<input type="range" class="form-range" id="range-input" min="0" max="15">
```

# 步伐

默认情况下，Bootstrap 5 的范围滑块会捕捉到最接近的整数。

为了让它捕捉到不同种类的值，我们可以使用`step`属性。

例如，我们可以写:

```
<label for="range-input" class="form-label">range input</label>
<input type="range" class="form-range" id="range-input" min="0" max="15" step="0.5">
```

捕捉到最接近的一半而不是最接近的整数。

# 输入组

我们可以通过在文本输入、选择或文件输入的左侧和右侧添加文本、按钮或按钮组来扩展表单控件。

要添加一个基本的输入组，我们可以写:

```
<div class="input-group mb-3">
  <span class="input-group-text">@</span>
  <input type="text" class="form-control" placeholder="Username">
</div>
```

用`span`和`input-group-text`类添加一个输入组。

我们可以将它添加到其他元素中，或者用不同的位置或内容:

```
<div class="input-group mb-3">
  <input type="text" class="form-control" placeholder="username">
  <span class="input-group-text">[@example](http://twitter.com/example).com</span>
</div><label for="basic-url" class="form-label">URL</label>
<div class="input-group mb-3">
  <span class="input-group-text" id="basic-addon3">https://example.com/</span>
  <input type="text" class="form-control" id="basic-url">
</div><div class="input-group mb-3">
  <span class="input-group-text">$</span>
  <input type="text" class="form-control" aria-label="Amount ">
  <span class="input-group-text">.00</span>
</div><div class="input-group">
  <span class="input-group-text">textarea</span>
  <textarea class="form-control"></textarea>
</div>
```

我们有添加到 div 的`input-group`类的输入。

然后我们有了包含`input-group-text`类的`span`来添加插件。

# 包装材料

默认情况下，输入组用`flex-wrap: wrap`换行，以适应定制表单域验证。

我们可以用`.flex-nowrap`类禁用这种风格:

```
<div class="input-group flex-nowrap">
  <span class="input-group-text" id="addon-wrapping">@</span>
  <input type="text" class="form-control" placeholder="Username">
</div>
```

# 胶料

输入组的大小可以用`input-group-sm`和`input-group-lg`类改变。

例如，我们可以写:

```
<div class="input-group input-group-sm mb-3">
  <span class="input-group-text" id="inputGroup-sizing-sm">Small</span>
  <input type="text" class="form-control">
</div><div class="input-group mb-3">
  <span class="input-group-text" id="inputGroup-sizing-default">Default</span>
  <input type="text" class="form-control">
</div><div class="input-group input-group-lg">
  <span class="input-group-text" id="inputGroup-sizing-lg">Large</span>
  <input type="text" class="form-control">
</div>
```

不支持调整单个组元素的大小。

# 复选框和单选按钮

我们可以在输入组插件中添加复选框和单选按钮。

例如，我们可以写:

```
<div class="input-group mb-3">
  <div class="input-group-text">
    <input class="form-check-input" type="checkbox" value="">
  </div>
  <input type="text" class="form-control">
</div><div class="input-group">
  <div class="input-group-text">
    <input class="form-check-input" type="radio" value="">
  </div>
  <input type="text" class="form-control">
</div>
```

在第一个输入的左边添加一个复选框。

我们在第二个输入的左边添加了一个单选按钮。

![](img/85d50bb97f22455c4fc61e47ea32c7cc.png)

由 [Raphael Nogueira](https://unsplash.com/@phaelnogueira?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

除了输入框之外，我们还可以添加输入组来添加各种内容。