# Vue 3 —列出过渡

> 原文：<https://blog.devgenius.io/vue-3-list-transitions-405a642ac8c7?source=collection_archive---------4----------------------->

![](img/3e1d56479f744a6c5e4287831ce30028.png)

照片由[阿拉尼·慕克吉](https://unsplash.com/@aaro?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

Vue 3 处于测试阶段，可能会有变化。

Vue 3 是 Vue 前端框架的最新版本。

它建立在 Vue 2 的普及性和易用性之上。

在本文中，我们将研究如何创建列表转换。

# 列出移动转换

我们可以添加列表移动转换。

例如，当项目改变位置时，我们可以使用`transition-group`组件来显示一些效果:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="[https://unpkg.com/vue@next](https://unpkg.com/vue@next)"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.15/lodash.min.js"></script><style>
      .list-move {
        transition: transform 0.8s ease;
      }
    </style>
  </head>
  <body>
    <div id="app">
      <button @click="shuffle">shuffle</button>
      <transition-group name="list" tag="div">
        <p v-for="item in items" :key="item">
          {{ item }}
        </p>
      </transition-group>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            items: Array(10)
              .fill()
              .map(() => Math.random()),
          };
        },
        methods: {
          shuffle() {
            this.items = _.shuffle(this.items);
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们添加了带有过渡效果的`list-move`类，以便在列表被打乱时显示它。

名单是用洛达什`shuffle`法洗牌的。

# 交错列表转换

我们可以在列表中错开过渡。

为了做到这一点，我们使用格林斯托克图书馆来帮助我们。

我们可以写作；

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.3.4/gsap.min.js"></script>
  </head>
  <body>
    <div id="app">
      <input v-model="query" />
      <transition-group
        name="fade"
        tag="div"
        :css="false"
        @before-enter="beforeEnter"
        @enter="enter"
        @leave="leave"
      >
        <p
          v-for="(item, index) in computedList"
          :key="item.name"
          :data-index="index"
        >
          {{ item.name }}
        </p>
      </transition-group>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            query: "",
            list: [
              { name: "james" },
              { name: "mary" },
              { name: "alex" },
              { name: "bob" },
              { name: "jane" }
            ]
          };
        },
        computed: {
          computedList() {
            return this.list.filter(item => {
              return item.name.toLowerCase().includes(this.query.toLowerCase());
            });
          }
        },
        methods: {
          beforeEnter(el) {
            el.style.opacity = 0;
            el.style.height = 0;
          },
          enter(el, done) {
            gsap.to(el, {
              opacity: 1,
              height: "1.3em",
              delay: el.dataset.index * 0.55,
              onComplete: done
            });
          },
          leave(el, done) {
            gsap.to(el, {
              opacity: 0,
              height: 0,
              delay: el.dataset.index * 0.45,
              onComplete: done
            });
          }
        }
      });
      app.mount("#app");
    </script>
  </body>
</html>
```

我们在应用程序中包含了 Greensock 库。

在`methods`属性中，我们有几个方法。

`beforeEnter`方法将容器的不透明度和高度设置为 0。

`enter`方法有我们的进入动画效果。

我们改变不透明度为 1，使不透明。

`height`是集装箱的高度。

`delay`是动画的延迟。

`onComplete`是我们调用的通知 Vue 动画完成的函数。

我们对`leave`过渡做同样的事情。

当`computedList`返回新值时，将应用动画效果。

因此，当我们在输入框中输入一些东西时，我们会看到应用的效果。

# 可重用转换

我们可以通过将转换移动到我们自己的组件中来使其可重用。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>App</title>
    <script src="https://unpkg.com/vue@next"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.3.4/gsap.min.js"></script>
  </head>
  <body>
    <div id="app">
      <input v-model="query" />
      <list-transition>
        <p
          v-for="(item, index) in computedList"
          :key="item.name"
          :data-index="index"
        >
          {{ item.name }}
        </p>
      </list-transition>
    </div>
    <script>
      const app = Vue.createApp({
        data() {
          return {
            query: "",
            list: [
              { name: "james" },
              { name: "mary" },
              { name: "alex" },
              { name: "bob" },
              { name: "jane" }
            ]
          };
        },
        computed: {
          computedList() {
            return this.list.filter(item => {
              return item.name.toLowerCase().includes(this.query.toLowerCase());
            });
          }
        }
      }); app.component("list-transition", {
        template: `
        <transition-group
          name="fade"
          tag="div"
          :css="false"
          [@before](http://twitter.com/before)-enter="beforeEnter"
          [@enter](http://twitter.com/enter)="enter"
          [@leave](http://twitter.com/leave)="leave"
        >
         <slot></slot>
        </transition-group>
        `,
        methods: {
          beforeEnter(el) {
            el.style.opacity = 0;
            el.style.height = 0;
          },
          enter(el, done) {
            gsap.to(el, {
              opacity: 1,
              height: "1.3em",
              delay: el.dataset.index * 0.55,
              onComplete: done
            });
          },
          leave(el, done) {
            gsap.to(el, {
              opacity: 0,
              height: 0,
              delay: el.dataset.index * 0.45,
              onComplete: done
            });
          }
        }
      }); app.mount("#app");
    </script>
  </body>
</html>
```

将我们的`transition`组件和钩子移动到它自己的组件。

我们只需创建一个组件，并在`transition-group`标签之间添加一个`slot`来为我们的内容添加位置。

现在我们可以在任何地方使用`list-transition`组件。

![](img/6692b12a01b7462e4de9e5b72a04a13f.png)

克里斯托夫·鲁舍夫在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用`transition-group`组件添加我们的列表过渡效果。

让我们添加钩子来创建 JavaScript 动画需要各种指令。