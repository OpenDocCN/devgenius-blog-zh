# 如何用 JavaScript 获取选中单选按钮的值？

> 原文：<https://blog.devgenius.io/how-to-get-the-value-of-a-selected-radio-button-with-javascript-e5ac43a6f41a?source=collection_archive---------2----------------------->

![](img/5503f7f167ba1858b976a4ba8b9e4fc3.png)

照片由[埃里克诺帕宁](https://unsplash.com/@rexcuando?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

有时，我们可能想要获取 JavaScript web 应用程序中所选单选按钮的值。

在本文中，我们将看看如何用 JavaScript 获取选中单选按钮的值。

# 遍历单选按钮，找到 checked 属性设置为 true 的选项

我们可以选择一个组中的所有单选按钮，并获得将`checked`属性设置为`true`的单选按钮。

例如，我们可以编写以下 HTML:

```
<div id="fruits">
  <input type="radio" name="fruit" value="apple"> Apple
  <input type="radio" name="fruit" value="orange"> Orange
  <input type="radio" name="fruit" value="grape" checked="checked"> Grape
</div>
```

然后我们可以用`document.querySelectorAll`得到所有的单选按钮元素，并通过写:

```
const fruits = document.querySelectorAll('input[name="fruit"]')
for (const f of fruits) {
  if (f.checked) {
    console.log(f.value)
  }
}
```

`fruits`是一个包含所有单选按钮元素的 HTMLCollection 对象。

然后我们使用 for-of 循环遍历单选按钮。

在循环体中，我们得到单选按钮的`checked`属性。

我们记录被选中的那个的值。

因此，我们应该看到`'grape'`被记录。

# 使用:选中的选择器

另一种让单选按钮被选中的方法是使用`:checked`选择器。

例如，我们可以写:

```
const selected = document.querySelector('input[name="fruit"]:checked').value;
console.log(selected)
```

来做这个。

`input[name=”fruit”]:checked`选择器获得所有属性名为`fruit`的输入。

我们用`value`属性得到`value`属性的值。

所以`selected`的值应该是`'grape'`。

# 从表单值中获取检查值

我们还可以从表单值中获取单选按钮的选中值。

例如，我们可以写:

```
<form id="fruits">
  <input type="radio" name="fruit" value="apple"> Apple
  <input type="radio" name="fruit" value="orange"> Orange
  <input type="radio" name="fruit" value="grape" checked="checked"> Grape
</form>
```

然后我们得到单选按钮，通过写:

```
const form = document.getElementById("fruits");
console.log(form.elements["fruit"].value);
```

我们用`getElementById`得到表单元素。

然后，我们使用`form.elements[“fruit”]`将 name 属性设置为`fruit`的输入。

然后我们可以获取`value`属性来获取所选的值。

所以控制台日志应该记录`'grape'`。

# 结论

用 JavaScript 获取单选按钮的选中值有几种方法。