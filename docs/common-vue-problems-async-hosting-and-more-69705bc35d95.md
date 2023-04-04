# 常见的 Vue 问题—异步托管等等

> 原文：<https://blog.devgenius.io/common-vue-problems-async-hosting-and-more-69705bc35d95?source=collection_archive---------28----------------------->

![](img/fbf9a7e52e9d3333d9e58689b1b35a40.png)

照片由[卡伦·艾姆斯利](https://unsplash.com/@kalenemsley?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄

Vue.js 让开发前端应用变得简单。然而，我们仍有可能遇到问题。

在这篇文章中，我们将看看一些常见的问题，并看看如何解决它们。

# 异步-等待和装载

我们可以使用`async` / `await`语法，这样我们就可以在`mounted`钩子中干净利落地使用承诺。

例如，我们可以写:

```
{
  ...
  data () {
    return {
      dataReady: false,      
    }
  },

  async mounted() {
    await doSomething();
    await doOtherThing();
    doMore();
    this.dataReady = true;
  },
  ...
}
```

# 防止 Vue 组件中的事件冒泡

我们更喜欢使用`self`修饰符的事件冒泡。

它表明我们不希望事件传播到父元素和祖先元素。

例如，我们可以写:

```
<div v-on:click.self="doSomething">...</div>
```

所以只有当 div 被点击时，才会调用`doSomething`

# 避免直接改变属性，因为值会被覆盖'错误

当我们想要通过 props 将一个值从父值更改为子值时，我们不应该直接更改它来更改父值的状态值。

此错误显示在控制台中，因为我们正在覆盖属性值。

相反，我们应该使用 getter 和 setter 来创建一个计算属性，而不是直接修改 props。

这是因为从父节点传入的属性值会覆盖我们的更改。

例如，我们可以写:

```
computed: {
  user: {
    get(){
      return this.username;
    },
    set(newValue){
      this.$emit('update:username', newValue)
    }   
  }
  ...
}
```

假设`username`是一个 prop 值，它作为`user`的值返回。

当我们设置`this.user`时，将会发出`update:username`事件，因此如果父节点监听`update:username`事件，它将会获得该值。

`newValue`是我们发出的价值。

现在在父节点中，我们可以写:

```
<child @update:username='doSomething'></child>
```

假设子组件名为`child`，我们监听`update:username`事件来更新值。

发出的值将作为`doSomething`的第一个参数。

# 在 Vue 组件中对 v-model 使用 contentEditable

如果我们使用`contenteditable`属性使一个元素的内容可编辑，我们需要监听`input`事件来获取最新的内容。

例如，我们可以写:

```
<template>
  <p
    contenteditable
    @input="onInput"
  >
    {{ content }}
  </p>
</template><script>
export default {
  data() {
    return { content: 'hello' };
  },
  methods: {
    onInput(e) {
      console.log(e.target.innerText);
    },
  },
};
</script>
```

我们有`onInput`，它将输入事件对象作为`e`的值。

然后我们可以用`e.target.innerText`得到最近一次改变的值。

# 具有嵌套级别路由的路由器视图

我们必须为每一层嵌套设置一个`router-view`。

例如，如果我们有:

```
routes: [
  {
    path: '/',
    component: Dashboard
  },
  {
    path: '/tasks',
    component: Tasks
  },
  {
    path: '/tasks/:id',
    name: 'task-detail',
    component: TaskDetails,
    children: [
      {
        path: '/tasks/:taskId/statuses',
        component: Statuses
      },
      {
        path: '/tasks/:taskId/statuses/:statusId/details',
        component: StatusDetails
      },
    ]
  }
]
```

如果我们有这些路线，那么我们需要一个`router-view`来存放顶层，另一个`TaskDetails`来存放状态路线。

这样，我们可以看到在`routes`数组中列出的所有东西。

# 将 URL 参数传递给 URL

我们可以使用`href`上的`v-bind`来设置路线。

例如，我们可以写:

```
<a :href="`/item/${item.id}`">Click</a>
```

然后我们可以将`item.id`插入到 URL 中。

# 将 Vue 应用程序部署到节点服务器

我们可以使用 Express 将应用程序作为静态资产托管，以便它加载 Vue 应用程序。

例如，我们可以写:

```
const express = require('express');
const path = require('path');
const serveStatic = require('serve-static');
app = express();
app.use(serveStatic(path.join(__dirname, `/dist`)));
const port = process.env.PORT || 5000;
const hostname = '127.0.0.1';app.listen(port, hostname, () => {
   console.log(`Server running`);
 });
```

我们使用`serveStatic`中间件将 Vue 应用作为静态文件提供。

`dist`文件夹有内置的 Vue app。

![](img/85db72f7de92d811a2a25ddbd8a1471c.png)

约翰·麦肯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以在我们的方法和生命周期挂钩中使用`async`和`await`。

此外，我们可以将 Vue 应用程序作为静态文件托管在 Express 应用程序中。

对于嵌套路由，我们应该有嵌套路由器视图。

如果我们使用`contentEditable`属性使元素的内容可编辑，我们可以从`input`事件中获得值。