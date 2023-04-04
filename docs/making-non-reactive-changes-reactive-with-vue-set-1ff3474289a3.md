# 使非反应性变化与 Vue.set 反应

> 原文：<https://blog.devgenius.io/making-non-reactive-changes-reactive-with-vue-set-1ff3474289a3?source=collection_archive---------4----------------------->

![](img/8291b6ffc6df4f38d816e2f9e7efddd6.png)

[T L](https://unsplash.com/@onelast?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍照

如果我们在 Vue 应用中对 Vue 不能自动选择的反应属性进行更改，那么我们需要使用`Vue.set`方法。

在本文中，我们将了解反应性和非反应性变更，以及如何将`Vue.set`用于非反应性变更。

# 反应

通过定义遍历对象并为它们定义 getters 和 setters 来跟踪反应变量的变化。

它们用于跟踪依赖关系并通知 Vue 变化。

每个组件都有一个相应的观察器实例来观察属性的变化。

每次触发依赖设置器时，观察器都会得到通知，并导致组件重新呈现。

# 目标

Vue 无法检测属性的添加或删除。

这是因为 getter 和 setter 转换过程需要属性存在，这样 Vue 才能使其具有反应性。

例如，如果我们有:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app"></div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          a: 1
        }
      });
      app.b = 2;
    </script>
  </body>
</html>
```

那么`b`属性不会被自动监视。

我们不能将属性添加到已经创建的实例的根级别。

但是我们可以用`Vue.set`向嵌套属性添加新属性。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{someObject.b}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          someObject: {}
        }
      });
      Vue.set(app.someObject, "b", 2);
    </script>
  </body>
</html>
```

我们用属性和要添加属性的对象来调用`Vue.set`。

然后是属性名，然后是值。

我们也可以在 Vue 实例对象本身中使用`this.$set`方法。

例如，我们可以写:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{someObject.b}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          someObject: {}
        },
        mounted() {
          this.$set(this.someObject, "b", 2);
        }
      });
    </script>
  </body>
</html>
```

做同样的事情。

我们也可以使用`Object.assign`并将其分配给同一个属性来添加属性:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{someObject.b}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          someObject: {}
        },
        mounted() {
          this.someObject = Object.assign({}, this.someObject, { a: 1, b: 2 });
        }
      });
    </script>
  </body>
</html>
```

重新分配还会触发 Vue 重新渲染组件。

# 数组

Vue 无法检测到对数组的某些更改。

它不能检测直接设置带有索引的项目。

并且它无法检测到对数组`length`的修改。

所以这两个变化不是被动的:

```
vm.items[1] = 'x' 
vm.items.length = 2
```

为了进行修改，我们可以再次使用`Vue.set`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{items[1]}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          items: ["foo", "bar"]
        }
      });
      Vue.set(app.items, 1, "qux");
    </script>
  </body>
</html>
```

我们用数组属性、索引和为索引设置的值来调用`Vue.set`。

我们也可以使用`splice`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{items[1]}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          items: ["foo", "bar"]
        }
      });
      app.items.splice(1, 1, "qux");
    </script>
  </body>
</html>
```

第一个参数是要更改的索引，第二个参数是要更改多少项，第三个参数是要将具有给定索引的条目更改为的值。

同样，我们可以在组件实例中调用`this.$set`:

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>app</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
  </head>
  <body>
    <div id="app">
      {{items[1]}}
    </div>
    <script>
      const app = new Vue({
        el: "#app",
        data: {
          items: ["foo", "bar"]
        },
        mounted() {
          this.$set(this.items, 1, "qux");
        }
      });
    </script>
  </body>
</html>
```

`this.$set`采用与`Vue.set`相同的参数。

# 结论

`Vue.set`方法让我们把非反应性的对象和数组操作变成反应性的。