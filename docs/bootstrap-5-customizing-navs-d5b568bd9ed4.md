# 引导程序 5 —定制 nav

> 原文：<https://blog.devgenius.io/bootstrap-5-customizing-navs-d5b568bd9ed4?source=collection_archive---------13----------------------->

![](img/1d1c4fae0b0e49530be33ce2ab159553.png)

[史蒂夫·哈维](https://unsplash.com/@trommelkopf?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

**写这篇文章时，Bootstrap 5 处于 alpha 状态，可能会发生变化。**

Bootstrap 是任何 JavaScript 应用程序的流行 UI 库。

在本文中，我们将了解如何使用 Bootstrap 5 定制 nav。

# JavaScript 行为

我们还可以根据自己的需求定制 nav。

例如，我们可以写:

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
</ul><div class="tab-content" id="myTabContent">
  <div class="tab-pane fade show active" id="home" role="tabpanel" aria-labelledby="home-tab">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam at neque orci. In vehicula metus eu euismod rhoncus. Pellentesque pharetra ligula sed efficitur ultricies. Quisque et congue metus, id rutrum purus. Maecenas finibus efficitur mauris, quis varius enim elementum a. Phasellus vel aliquet diam. Proin tincidunt eros pellentesque mauris laoreet, nec ullamcorper magna vulputate. Suspendisse potenti. Nullam lobortis ultricies nunc id vulputate. Curabitur pulvinar, lorem ac tempus lobortis, tortor tortor dignissim enim, quis tempor est lacus ut augue. Sed tincidunt lectus urna, ac aliquet velit suscipit a. Nullam tempus sem id nibh vulputate, nec ultricies lorem porttitor. Sed pharetra lacinia odio non sodales.
  </div> <div class="tab-pane fade" id="profile" role="tabpanel" aria-labelledby="profile-tab">Mauris ullamcorper ornare dui at bibendum. Duis sed quam ac purus tincidunt aliquet. Suspendisse potenti. Praesent vitae turpis dui. Fusce ut tincidunt est, ut sagittis mi. Sed euismod pharetra pulvinar. Morbi at facilisis quam. Vestibulum sagittis arcu sit amet scelerisque vestibulum. Nulla nisl libero, ornare et facilisis et, tempus sagittis massa. Sed tincidunt metus non ex ornare, eu elementum lorem sagittis. Nam a sapien eleifend, dignissim purus sit amet, ultrices neque. Suspendisse imperdiet purus eu posuere pellentesque. Nulla facilisi.
  </div> <div class="tab-pane fade" id="contact" role="tabpanel" aria-labelledby="contact-tab">Mauris suscipit metus at felis maximus, a sagittis libero ornare. Sed eu placerat mauris. Cras venenatis, magna sed efficitur tristique, nibh diam mattis neque, non pellentesque turpis nibh in sapien. Proin porttitor sodales risus sit amet imperdiet. Aenean quis pretium sem, quis maximus purus. Pellentesque vestibulum elementum dui ut ultrices. Suspendisse potenti.
  </div>
</div>
```

通过添加带有我们自己结构的`li`我们有了选项卡组件。

此外，我们可以添加带有药丸的标签:

```
<ul class="nav nav-pills mb-3" id="pills-tab" role="tablist">
  <li class="nav-item" role="presentation">
    <a class="nav-link active" id="pills-home-tab" data-toggle="pill" href="#pills-home" role="tab" aria-controls="pills-home" aria-selected="true">Home</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="pills-profile-tab" data-toggle="pill" href="#pills-profile" role="tab" aria-controls="pills-profile" aria-selected="false">Profile</a>
  </li>
  <li class="nav-item" role="presentation">
    <a class="nav-link" id="pills-contact-tab" data-toggle="pill" href="#pills-contact" role="tab" aria-controls="pills-contact" aria-selected="false">Contact</a>
  </li>
</ul>
<div class="tab-content" id="pills-tabContent">
  <div class="tab-pane fade show active" id="pills-home" role="tabpanel" aria-labelledby="pills-home-tab">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam at neque orci. In vehicula metus eu euismod rhoncus. </div> <div class="tab-pane fade" id="pills-profile" role="tabpanel" aria-labelledby="pills-profile-tab">Pellentesque pharetra ligula sed efficitur ultricies. Quisque et congue metus, id rutrum purus.</div> <div class="tab-pane fade" id="pills-contact" role="tabpanel" aria-labelledby="pills-contact-tab">Curabitur pulvinar, lorem ac tempus lobortis, tortor tortor dignissim enim, quis tempor est lacus ut augue. 
  </div>
</div>
```

我们添加了带有药丸的导航链接和`.nav-pills`类。

然后我们有了内容。

具有类`.tab-pane`的 div 的 ID 必须匹配`href`的 URL。

我们还可以添加垂直药丸:

```
<div class="row">
  <div class="col-3">
    <div class="nav flex-column nav-pills" id="v-pills-tab" role="tablist" aria-orientation="vertical">
      <a class="nav-link active" id="v-pills-home-tab" data-toggle="pill" href="#v-pills-home" role="tab" aria-controls="v-pills-home" aria-selected="true">Home</a> <a class="nav-link" id="v-pills-profile-tab" data-toggle="pill" href="#v-pills-profile" role="tab" aria-controls="v-pills-profile" aria-selected="false">Profile</a> <a class="nav-link" id="v-pills-messages-tab" data-toggle="pill" href="#v-pills-messages" role="tab" aria-controls="v-pills-messages" aria-selected="false">Messages</a> <a class="nav-link" id="v-pills-settings-tab" data-toggle="pill" href="#v-pills-settings" role="tab" aria-controls="v-pills-settings" aria-selected="false">Settings</a>
    </div>
  </div> <div class="col-9">
    <div class="tab-content" id="v-pills-tabContent">
      <div class="tab-pane fade show active" id="v-pills-home" role="tabpanel" aria-labelledby="v-pills-home-tab">urabitur pulvinar, lorem ac tempus lobortis, tortor tortor dignissim enim, quis tempor est lacus ut augue.</div> <div class="tab-pane fade" id="v-pills-profile" role="tabpanel" aria-labelledby="v-pills-profile-tab">Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam at neque orci.  </div> <div class="tab-pane fade" id="v-pills-messages" role="tabpanel" aria-labelledby="v-pills-messages-tab">In vehicula metus eu euismod rhoncus. Pellentesque pharetra ligula sed efficitur ultricies. </div> <div class="tab-pane fade" id="v-pills-settings" role="tabpanel" aria-labelledby="v-pills-settings-tab">Proin tincidunt eros pellentesque mauris laoreet, nec ullamcorper magna vulputate. </div>
    </div>
  </div>
</div>
```

我们在`.col-3`类的导航中有导航链接。

项目在一个带有`.nav-pills`类的 div 中。

`flex-column`使垂直。

此外，我们在右边的列中添加了带有`.tab-pane`类的 div，用于添加当我们单击链接时要显示的内容。

如果`href`与选项卡窗格 div 的 ID 匹配，那么当我们添加类时，就会显示内容。

![](img/ef47bd8f4a05ff2c935a60bb62ae23cb.png)

照片由[加里·洛帕特](https://unsplash.com/@glopater?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以按照自己的方式布局标签和内容。

标签可以是垂直的，并把内容放在旁边。