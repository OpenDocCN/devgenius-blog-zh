# Bootstrap 5 —用 JavaScript 操纵选项卡

> 原文：<https://blog.devgenius.io/bootstrap-5-manipulating-tabs-with-javascript-29375a594124?source=collection_archive---------0----------------------->

![](img/a6482f752f0325a033ed8d5bd197fc4a.png)

照片由[Louis Hansel @ shotsoflouis](https://unsplash.com/@louishansel?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

**写这篇文章时，Bootstrap 5 还处于 alpha 版本，可能会有变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将看看如何通过 Bootstrap 5 用 JavaScript 操作 nav。

# 使用数据属性

我们可以添加`data-toggle='tab'`来添加链接，当我们点击它时显示内容。

例如，我们可以写:

```
<ul class="nav nav-tabs" id="tab">
  <li class="nav-item">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" >Home</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" >Profile</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="messages-tab" data-toggle="tab" href="#messages">Messages</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="settings-tab" data-toggle="tab" href="#settings" >Settings</a>
  </li>
</ul><div class="tab-content">
  <div class="tab-pane active" id="home">Lorem ipsum dolor sit amet, consectetur adipiscing elit. </div> <div class="tab-pane" id="profile"> Nam at neque orci. In vehicula metus eu euismod rhoncus.</div> <div class="tab-pane" id="messages">Pellentesque pharetra ligula sed efficitur ultricies. </div> <div class="tab-pane" id="settings">Quisque et congue metus, id rutrum purus. </div>
</div>
```

我们将`data-toggle`属性设置为`tab`,使链接成为开放内容。

# Java Script 语言

我们可以用`addEventListener`方法向选项卡添加点击监听器。

例如，我们可以写:

```
const triggerTabList = [...document.querySelectorAll('#myTab a'];
triggerTabList.forEach((triggerEl) => {
  const tabTrigger = new bootstrap.Tab(triggerEl)

  triggerEl.addEventListener('click', (e) => {
    e.preventDefault()
    tabTrigger.show()
  })
})
```

我们用`bootstrap.Tab`构造函数创建标签对象。

然后我们用`addEventListener`方法将点击监听器连接到它们。

然后我们调用 click listener 回调函数中的`show`来显示内容。

# 褪色效果

我们可以用`.fade`类给每个`.tab-pane`添加一个渐变效果。

例如，我们可以写:

```
<ul class="nav nav-tabs" id="tab">
  <li class="nav-item">
    <a class="nav-link active" id="home-tab" data-toggle="tab" href="#home" >Home</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="profile-tab" data-toggle="tab" href="#profile" >Profile</a>
  </li>
  <li class="nav-item">
    <a class="nav-link" id="messages-tab" data-toggle="tab" href="#messages">Messages</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="settings-tab" data-toggle="tab" href="#settings" >Settings</a>
  </li>
</ul><div class="tab-content">
  <div class="tab-pane fade active" id="home">Lorem ipsum dolor sit amet, consectetur adipiscing elit. </div> <div class="tab-pane fade" id="profile"> Nam at neque orci. In vehicula metus eu euismod rhoncus.</div> <div class="tab-pane fade" id="messages">Pellentesque pharetra ligula sed efficitur ultricies. </div> <div class="tab-pane fade" id="settings">Quisque et congue metus, id rutrum purus. </div>
</div>
```

添加了`.fade`类后，当我们点击标签链接时，我们会看到渐变效果。

除了`show`方法，还有`dispose`方法来销毁元素的 tav。

`getInstance`获取选项卡实例。

# 事件

选项卡触发一些事件。

`hide.bs.tab`当我们隐藏它时，在当前活动的选项卡上发出。

当我们点击 T14 时，它会出现在待显示选项卡上。

`hidden.bs.tab`上一个激活的标签页是我们隐藏的标签页。

`shown.bs.tab`在我们显示新激活的选项卡时发出。

![](img/f9c8952ff1351db299d42b18762d9281.png)

照片由[卢茨·鲍曼](https://unsplash.com/@luba1304?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用 JavaScript 操纵标签。

它们可以显示或隐藏。

我们可以听到各种各样的事件。