# 普通 Javascript 选择特性

> 原文：<https://blog.devgenius.io/vanilla-javascript-select-feature-179f189029e0?source=collection_archive---------7----------------------->

![](img/42974e3cab3a3ef5d138ed916ef71ed5.png)

能够让您的应用程序更具动态性是件好事。如果下拉选项越来越大，或者甚至可以从数据库中提取，没有人愿意在下拉选项中进行硬编码。我将一步一步地向您介绍如何使用数据库中的数据来呈现选择表单字段。

1.  表格

像平常一样编写我们的表单，如果用 Javascript 文件编写表单并附加它，会更容易一些。您的选择文件不需要有任何选项标签，Javascript 会为我们处理。

```
const form = (`
<form>
    <input type="type"></input>
    <select id="dropdown"></select>
    <button type="submit">Submit</button>
</form>
`)
```

2.功能

我们将编写一个函数来处理 API 数据的迭代，并为我们的选择框呈现选项。快速排练:

~我们要找 select 字段我个人喜欢按 id 找东西。

~我们希望有一个默认选项作为占位符

~然后调用您的 API

~一旦我们得到数据，我们想迭代数组中的每一项，我们想创建一个选项。只要记住你需要发回什么信息，你想向用户展示什么就行了。

```
function renderDropdown(){let dropdown = document.getElementById(‘dropdown’)let defaultOption = document.createElement(‘option’)defaultOption.text = “Select Image”dropdown.add(defaultOption)fetch(“http://localhost:3000/images")/.then(resp=> resp.json()).then(images=> {for(let i = 0; i < images.length; i++){let option = document.createElement(‘option’)option.innerHTML = images[i].nameoption.value = images[i].img_urldropdown.options.add(option)}})}
```

3.将它附加到表单中

我们再一次想找到表单的位置，如果你只有一个表单，你可以查询，莱达，或者我喜欢用 id。一旦你找到了形式，就用形式常数和。innerHTML，以便将表单呈现到 HTML。渲染完成后，您可以调用 renderDropdown 函数。

```
const FormDiv = document.getElementById('form-field')
//write the form here const here
formDiv.innerHTML = form               
renderDropdown()
```

请记住，Javascript 从上到下运行，所以在运行下拉菜单之前，您需要表单，否则您的选项将是空白的。

## 结论

Javascript 一开始很棘手，直到你掌握了它奇怪的规则。停止和伪代码是我能让事情工作的唯一方法。我最大的建议是考虑事物是如何呈现的，以及它需要什么样的顺序。一旦你掌握了这个，你将会真正爱上 Javascript。