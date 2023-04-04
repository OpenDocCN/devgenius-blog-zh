# Bootstrap 5 —关于 nav 的更多信息

> 原文：<https://blog.devgenius.io/bootstrap-5-more-about-navs-dd71c42aa07?source=collection_archive---------14----------------------->

![](img/495796c11eb42c53362c999958642907.png)

照片由[丹金](https://unsplash.com/@danielcgold?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会更改。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 定制 nav。

# 使用下拉菜单

我们可以在导航中添加下拉菜单。

例如，我们可以写:

```
<ul class="nav nav-tabs">
  <li class="nav-item">
    <a class="nav-link active" aria-current="page" href="#">foo</a>
  </li>
  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-expanded="false">Dropdown</a>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" href="#">action 1</a></li>
      <li><a class="dropdown-item" href="#">action 2</a></li>
      <li><a class="dropdown-item" href="#">action 3</a></li>
      <li>
        <hr class="dropdown-divider">
      </li>
      <li><a class="dropdown-item" href="#">actiob 4</a></li>
    </ul>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">bar</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
  </li>
</ul>
```

我们通过添加带有`dropdown`和`nav-link`类的`li`来添加下拉菜单。

它们一起使下拉开关适合菜单。

然后我们用`.dropdown-toggle`类添加下拉开关。

而`.dropdown-item`类应用于下拉列表项。

我们也有`.nav-tabs`风格的导航标签。

# 带滴剂的药丸

我们可以将`.nav-tabs`更改为`.nav-pills`，以将链接显示为药丸:

```
<ul class="nav nav-pills">
  <li class="nav-item">
    <a class="nav-link active" aria-current="page" href="#">foo</a>
  </li>
  <li class="nav-item dropdown">
    <a class="nav-link dropdown-toggle" data-toggle="dropdown" href="#" role="button" aria-expanded="false">Dropdown</a>
    <ul class="dropdown-menu">
      <li><a class="dropdown-item" href="#">action 1</a></li>
      <li><a class="dropdown-item" href="#">action 2</a></li>
      <li><a class="dropdown-item" href="#">action 3</a></li>
      <li>
        <hr class="dropdown-divider">
      </li>
      <li><a class="dropdown-item" href="#">actiob 4</a></li>
    </ul>
  </li>
  <li class="nav-item">
    <a class="nav-link" href="#">bar</a>
  </li>
  <li class="nav-item">
    <a class="nav-link disabled" href="#" tabindex="-1" aria-disabled="true">Disabled</a>
  </li>
</ul>
```

# JavaScript 行为

我们可以添加选项卡，当我们点击它时显示内容。

为此，我们写道:

```
<ul class="nav nav-tabs" id="myTab" role="tablist">
  <li class="nav-item" role="presentation">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">Home</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">Profile</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="contact-tab" data-toggle="tab" href="#contact" role="tab" aria-controls="contact" aria-selected="false">Contact</a>
  </li>
</ul>
<div class="tab-content" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam at neque orci. In vehicula metus eu euismod rhoncus. Pellentesque pharetra ligula sed efficitur ultricies. Quisque et congue metus, id rutrum purus. Maecenas finibus efficitur mauris, quis varius enim elementum a. Phasellus vel aliquet diam. Proin tincidunt eros pellentesque mauris laoreet, nec ullamcorper magna vulputate. Suspendisse potenti. Nullam lobortis ultricies nunc id vulputate. Curabitur pulvinar, lorem ac tempus lobortis, tortor tortor dignissim enim, quis tempor est lacus ut augue. Sed tincidunt lectus urna, ac aliquet velit suscipit a. Nullam tempus sem id nibh vulputate, nec ultricies lorem porttitor. Sed pharetra lacinia odio non sodales.
  </div>
  <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">Mauris ullamcorper ornare dui at bibendum. Duis sed quam ac purus tincidunt aliquet. Suspendisse potenti. Praesent vitae turpis dui. Fusce ut tincidunt est, ut sagittis mi. Sed euismod pharetra pulvinar. Morbi at facilisis quam. Vestibulum sagittis arcu sit amet scelerisque vestibulum. Nulla nisl libero, ornare et facilisis et, tempus sagittis massa. Sed tincidunt metus non ex ornare, eu elementum lorem sagittis. Nam a sapien eleifend, dignissim purus sit amet, ultrices neque. Suspendisse imperdiet purus eu posuere pellentesque. Nulla facilisi.
  </div>
  <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">Sed dui purus, tempor ac consectetur quis, viverra ut tellus. Duis eget venenatis nibh, eget placerat diam. Suspendisse a ex libero. Ut quis turpis tempor sem pharetra ullamcorper sed at metus. In eget nibh orci. Aenean ac nulla nulla. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Proin eu facilisis felis. Nulla sed risus mattis, tristique odio eu, mattis ex. Morbi aliquet nulla eget vestibulum lobortis. Sed aliquam odio magna, et dictum sapien scelerisque sed. Praesent odio sapien, gravida ac leo quis, ornare placerat nibh. Donec id ex mauris. Nunc ut sapien quis ex malesuada pretium non sed elit. Cras maximus ipsum sed augue sollicitudin, ut scelerisque velit ultrices. Sed rutrum viverra massa in condimentum.
  </div>
</div>
```

添加选项卡。

`nav-item`类应用于导航项目。

并将`tab-pane`类应用于内容类。

`href`是内容的 URL。它应该匹配选项卡内容 divs 的 ID 加上它前面的散列。

`fade`为过渡添加淡入淡出效果。

![](img/a9cea16382e828b8a540cc07ad71629c.png)

照片由 [ja ma](https://unsplash.com/@ja_ma?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

# 结论

我们可以添加导航来显示内容。

造型可以改变。