# 可维护的 JavaScript——分离 HTML 和 JavaScript

> 原文：<https://blog.devgenius.io/maintainable-javascript-separate-html-and-javascript-e4dfd06438de?source=collection_archive---------9----------------------->

![](img/d2ea75a8c7f9cf30106a1d325e697b96.png)

[大卫·罗蒂米](https://unsplash.com/@davidrotimi?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

如果想继续使用代码，创建可维护的 JavaScript 代码很重要。

在本文中，我们将通过查看 JavaScript 和 HTML 之间的松散耦合来了解创建可维护的 JavaScript 代码的基础。

# 让 HTML 远离 JavaScript

我们应该把 HTML 代码放在 JavaScript 之外。

在我们的 JavaScript 代码中加入 HTML 使得当代码中有任何文本或结构问题时，应用程序很难调试。

几乎不可能在我们的 JavaScript 代码中跟踪动态 HTML 的问题。

因此，我们不应该编写这样的代码:

```
const div = document.querySelector("div");
div.innerHTML = "<h3>Error</h3> <p>Invalid date.</p>";
```

在我们的 JavaScript 中嵌入 HTML 字符串是一种不好的做法。

它使跟踪任何文本或结构问题变得复杂，

很难通过我们浏览器的开发控制台中的 DOM inspector 来解决这个问题，因为它是动态更新的。

因此，这比追踪简单的 DOM 操作问题更难。

第二个问题是可维护性。

如果我们需要改变文本或标记，我们必须改变 HTML 和 JavaScript 代码来改变文本。

这比在 HTML 代码中改变它们要多得多。

JavaScript 中的标记不可更改。

这是因为我们不希望在 JavaScript 代码中使用 HTML。

编辑标记比编辑 JavaScript 更容易出错，因为标记是字符串形式的。

所以把 HTML 放在我们的 JavaScript 代码中，我们就把问题复杂化了。

有很多方法可以以松耦合的方式用 JavaScript 插入 HTML。

一种方法是使用一些模板系统，比如把手。

例如，我们可以写:

```
<script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script><script>
  const template = Handlebars.compile("<p>{{firstname}} {{lastname}}</p>"); console.log(template({
    firstname: "james",
    lastname: "smith"
  }));</script>
```

我们包含了 Handlebars 库，然后调用`Handlebars.compile`返回一个`template`函数，让我们在模板字符串中插入表达式。

如果我们想在把手上使用 HTML，我们添加一个特殊的脚本标签:

```
<script src="https://cdn.jsdelivr.net/npm/handlebars@latest/dist/handlebars.js"></script><script id="entry-template" type="text/x-handlebars-template">
  <div class="entry">
    <p>{{firstname}} {{lastname}}</p>
  </div>
</script>
```

然后我们可以添加下面的 JavaScript 将模板编译成 HTML:

```
const source = $("#entry-template").html();
const template = Handlebars.compile(source);
const context = {
  firstname: "james",
  lastname: "smith"
}
const output = template(context);
$("body").append(output);
```

脚本标签具有`text/x-handlebars-template`类型，以区别于 JavaScript 脚本标签。

所以我们可以把`template`编译成 HTML。

编译是通过用 jQuery 获取脚本来完成的。

然后我们调用`Handlebars.compile`来编译源代码。

`context`有我们想要在模板中插入的变量。

然后我们用`context`调用`template`来编译数据。

最后，我们用编译好的模板调用`append`。

或者，我们可以使用像 Vue、React 或 Angular 这样的框架来构建我们的应用程序，这样我们就不必担心混合 HTML 和 JavaScript 是一种无组织的方式。

框架让我们以 JavaScript 和 HTML 之间松散耦合的方式编写代码。

![](img/a412a6d2e3d99663b54584f5fb3cf654.png)

西蒙·瑞在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该把 HTML 代码放在 JavaScript 代码之外。

这样，我们就不必担心跟踪难以调试的代码。

我们可以用模板系统(比如把手)或者使用框架(比如 Angular、React 或 Vue)将它们分开。