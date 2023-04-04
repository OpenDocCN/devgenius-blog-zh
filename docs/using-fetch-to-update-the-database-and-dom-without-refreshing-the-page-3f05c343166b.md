# 使用 fetch 更新数据库和 DOM，而不刷新页面

> 原文：<https://blog.devgenius.io/using-fetch-to-update-the-database-and-dom-without-refreshing-the-page-3f05c343166b?source=collection_archive---------1----------------------->

在过去的一周里，我学习了如何使用 fetch 通过 JavaScript 操作 DOM。随着我的进展，我很难在不需要刷新页面的情况下，在页面和数据库中提交新数据或更新现有数据。

![](img/e3fc4b4b4c08a5bbc7d9b11c5a85f7d7.png)

亚历克斯·纪尧姆在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

在我讨论我的解决方案之前，我想谈谈使用 fetch 的基础知识，这是我在学习中的经验。

第一步是学习一个简单的 GET 请求，以 JSON 格式从数据库或端点接收信息。我使用 fetch 注释和所需的端点 URL 作为参数。这返回了一个 Promise“object”，我们需要将它转换成 JSON，然后根据需要进行处理。在本例中，我们将 console.log 放在控制台中查看:

```
fetch("http://example.com/books.json")       // desired endpoint URL
.then(response => response.json())       // convert response to JSON
.then(json => console.log(json));      // console.log converted JSON
```

在理解了 GET 请求之后，我转到了 PATCH 和 POST，这允许我在 JSON 数据库中添加或更新条目。这需要向 fetch 命令添加第二个参数，它是一个`init`或`config object`，包含关于我们想要对 API 调用做什么的详细信息。

```
let configObject = {
  method: "POST",                    // declares HTTP request method
  headers: {
    "Content-Type": "application/json"    // declares format of data
  },
  body: JSON.stringify(               // turns data into JSON string
    {
      "key": "value",              // keys and values we want to add
      "key2": "value2"
    }
  )
};
```

综上所述，假设我们想要将一本书的“喜欢”更新为“5”。获取补丁请求可能如下所示:

```
fetch("http://www.example.com/books/1", { // note we are going to /1
  method: "PATCH",
  headers: {
      "Content-Type" : "application/json"
    },
  body: JSON.stringify(
    {
      "likes": 5           // we are changing the "likes" value to 5
    }
  )
});
```

非常好。现在我们已经更新了数据库，但我们还想更改页面上显示的赞数。让我们假设显示“4 个赞”的`<span>`有一个“已显示赞”的 id，即:`<span id="likes-displayed">`。我们想要获取该元素，因此我们可以将它的`InnerText`值从“4 个喜欢”更改为“5 个喜欢”:

```
let likesContainer = document.getElementById("likes-displayed");
```

现在，我们可以使用从补丁请求中获得的响应来更新这个变量的`innerText`!让我们添加上面的代码:

```
let likesContainer = document.getElementById("likes-displayed");fetch("http://www.example.com/books/1", {
  method: "PATCH",
  headers: {
      "Content-Type" : "application/json"
    },
  body: JSON.stringify(
    {
      "likes": 5
    }
  )
})
.then(response => response.json())
.then(json => {
  likesContainer.innerText = `${json.likes} likes`;
});
```

以上，我们是:

1.  将 likesContainer 声明为变量，这是我们的 id 为`"likes-displayed"`的`<span>`，它内置在 HTML 中。
2.  向`[http://www.example.com/books/1](http://www.example.com/books/1)`发送获取请求
3.  指定我们要打补丁，即更新现有信息
4.  指定我们发送的内容类型是 JSON
5.  指定新值，在本例中，将“likes”更新为 5。
6.  然后，我们获取响应，将其转换为 JSON，然后使用`json.likes`(现在是 5)的值来更新`likesContainer.innerText`的字符串值。

![](img/f5b2d36b5d32cbca09b446233939c248.png)

[马克·柯尼希](https://unsplash.com/@markkoenig?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

太棒了。fetch 和它的 config 对象更新数据库中的信息，或者使用响应 JSON 更新 DOM，这些都是在不刷新页面的情况下完成的，但是还有最后一个问题需要解决:我们需要一种动态增加 likes 的方法。目前，我们明确地将它改为“5”，但是我们怎样才能写得更动态，并且不管喜欢的数量是多少，都增加 1？

让我们回到我们的`likesContainer`变量。我们可以用 JavaScript 做的一件很酷的事情是为变量创建属性。让我们尝试给 likesContainer 一个“potato”属性:

```
let likesContainer = document.getElementById("likes-displayed");likesContainer.potato = "Holy potatoes!";likesContainer.potato;
 // "Holy potatoes!"
```

天哪…好吧，回到我们的增量问题，让我们创建一个名为“likes”的自定义属性，以整数形式跟踪我们当前的赞数:

```
likesContainer.likes = 4;
```

现在，让我们将上面的获取请求封装到一个函数中，以增加赞数。在这个函数中，在获取之前，让我们递增`.likes`属性。我们将使用`likesContainer.likes ++`，它的作用与`+= 1`相同，将当前值增加 1:

```
let likesContainer = document.getElementById("likes-displayed");function incrementLikes() {
  likesContainer.likes ++;
  fetch("http://www.example.com/books/1", {
    method: "PATCH",
    headers: {
        "Content-Type" : "application/json"
      },
    body: JSON.stringify(
      {
        "likes": 5
      }
    )
  })
  .then(response => response.json())
  .then(json => {
    likesContainer.innerText = `${json.likes} likes`;
  });
}
```

最后，让我们换出`"likes": 5`来使用`likesContainer.likes:`的当前值

```
let likesContainer = document.getElementById("likes-displayed");function incrementLikes() {
  likesContainer.likes ++;
  fetch("http://www.example.com/books/1", {
    method: "PATCH",
    headers: {
        "Content-Type" : "application/json"
      },
    body: JSON.stringify(
      {
        "likes": likesContainer.likes
      }
    )
  })
  .then(response => response.json())
  .then(json => {
    likesContainer.innerText = `${json.likes} likes`;
  });
}
```

现在，当我们运行`incrementLikes`函数时，我们:

1.  将`likesContainer.likes`值增加 1
2.  使用 fetch 发送补丁请求，将`"likes"`的值更改为`likesContainer.likes`的当前值
3.  使用包含来自数据库的更新的 JSON 信息的响应，更新`likesContainer.innerText`以表示新增加的值

![](img/3c9141062b2cab76ede2c9241d2e3431.png)

照片由[瓦伦丁·乔雷尔](https://unsplash.com/@valentinjrl?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

哇，我们成功了！我们现在可以向我们的数据库*发布和修补更新的信息，并且*更新 DOM(页面)而不需要刷新！太棒了。