# 引导程序 5 —表单布局

> 原文：<https://blog.devgenius.io/bootstrap-5-form-layouts-8cd63fda0f38?source=collection_archive---------4----------------------->

![](img/5f43348220db62051967683570b1988d.png)

照片由 [Peace Itimi](https://unsplash.com/@peaceitimi?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 向表单添加布局。

# 表单布局

我们可以使用 Bootstrap 5 向表单添加一些布局。

它为我们提供了保证金工具，以增加一些结构。

例如，我们可以写:

```
<div class="mb-3">
  <label for="first-name" class="form-label">first name</label>
  <input type="text" class="form-control" id="first-name" placeholder="first name">
</div><div class="mb-3">
  <label for="last-name" class="form-label">last name</label>
  <input type="text" class="form-control" id="last-name" placeholder="last name">
</div>
```

我们使用了`mb-3`类在表单下方添加一些空白。

标签有`form-label`类。

输入有`form-control`类来应用引导样式。

# 表单网格

如果我们需要表单域并排，我们可以将表单域放在一个网格中。

例如，我们可以写:

```
<div class="row">
  <div class="col">
    <input type="text" class="form-control" placeholder="First name">
  </div> <div class="col">
    <input type="text" class="form-control" placeholder="Last name">
  </div>
</div>
```

有 2 个输入框并排。

我们有一个带有`row`类的 div 和两个带有`col`类的 div 来添加列。

`$enable-grid-classes` SASS 变量被启用，默认为开启。

# 沟壑

我们可以用`g-*`类来添加槽。

例如，我们可以写:

```
<div class="row g-3">
  <div class="col">
    <input type="text" class="form-control" placeholder="First name">
  </div>
  <div class="col">
    <input type="text" class="form-control" placeholder="Last name">
  </div>
</div>
```

我们用`g-3`类添加了一些水槽。

间隙被添加到 div 中。

我们可以使用网格添加更复杂的表单:

```
<form class="row g-3">
  <div class="col-md-6">
    <label for="email" class="form-label">Email</label>
    <input type="email" class="form-control" id="email">
  </div> <div class="col-md-6">
    <label for="password" class="form-label">Password</label>
    <input type="password" class="form-control" id="password">
  </div> <div class="col-12">
    <label for="address" class="form-label">Address</label>
    <input type="text" class="form-control" id="address" placeholder="Address">
  </div> <div class="col-12">
    <label for="address2" class="form-label">Address 2</label>
    <input type="text" class="form-control" id="address2" placeholder="Apartment, studio, or floor">
  </div> <div class="col-md-6">
    <label for="city" class="form-label">City</label>
    <input type="text" class="form-control" id="city">
  </div> <div class="col-md-4">
    <label for="inputState" class="form-label">Region</label>
    <select id="inputState" class="form-select">
      <option selected>Choose...</option>
      <option>...</option>
    </select>
  </div> <div class="col-md-2">
    <label for="postal" class="form-label">Postal Code</label>
    <input type="text" class="form-control" id="postal">
  </div> <div class="col-12">
    <div class="form-check">
      <input class="form-check-input" type="checkbox" id="agree">
      <label class="form-check-label" for="agree">
        I agree
      </label>
    </div>
  </div> <div class="col-12">
    <button type="submit" class="btn btn-primary">Sign up</button>
  </div>
</form>
```

我们在表单中添加了多个字段。

有些比其他的宽。

那些是用`col-*`类添加的

# 水平形式

我们可以添加`.row`类来添加表单组，用`.col-*-*`类来设置标签和控件的宽度。

此外，我们必须将`.col-form-label`类添加到标签中，这样它们就可以与表单控件垂直居中。

例如，我们可以写:

```
<form>
  <div class="row mb-3">
    <label for="email" class="col-sm-2 col-form-label">Email</label>
    <div class="col-sm-10">
      <input type="email" class="form-control" id="email">
    </div>
  </div> <div class="row mb-3">
    <label for="password" class="col-sm-2 col-form-label">password</label>
    <div class="col-sm-10">
      <input type="password" class="form-control" id="password">
    </div>
  </div> <fieldset>
    <div class="row mb-3">
      <legend class="col-form-label col-sm-2 pt-0">Fruit</legend> <div class="col-sm-10">
        <div class="form-check">
          <input class="form-check-input" type="radio" name="fruit" id="apple" value="apple" checked>
          <label class="form-check-label" for="apple">
            apple
          </label>
        </div><div class="form-check">
          <input class="form-check-input" type="radio" name="fruit" id="banana" value="banana">
          <label class="form-check-label" for="banana">
            Second radio
          </label>
        </div> <div class="form-check disabled">
          <input class="form-check-input" type="radio" name="fruit" id="orange" value="orange" disabled>
          <label class="form-check-label" for="orange">
            Third disabled radio
          </label>
        </div>
      </div>
    </div>
  </fieldset> <div class="row mb-3">
    <div class="col-form-label col-sm-2 pt-0">I agree</div>
    <div class="col-sm-10">
      <div class="form-check">
        <input class="form-check-input" type="checkbox" id="agree">
        <label class="form-check-label" for="agree">
          I agree
        </label>
      </div>
    </div>
  </div> <button type="submit" class="btn btn-primary">Sign up</button>
</form>
```

我们有一个标签和表单控件并列的表单。

此外，我们添加了`mb-3`类来给底部添加一些边距。

`pt-0`去除顶部填充。

![](img/bd5f24d8d9847ab396fb06b22d4b1a8f.png)

照片由[纳丁·普里莫](https://unsplash.com/@nadineprimeau?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用 Bootstrap 提供的各种实用程序类来添加表单布局。