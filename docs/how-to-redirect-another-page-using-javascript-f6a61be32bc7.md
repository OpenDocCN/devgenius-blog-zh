# 如何使用 Javascript 重定向另一个页面

> 原文：<https://blog.devgenius.io/how-to-redirect-another-page-using-javascript-f6a61be32bc7?source=collection_archive---------13----------------------->

本文将展示如何使用 javascript 重定向另一个页面。我们将以不同的方式将页面重定向到另一个页面。

使用 javascript 重定向到另一个页面有很多方法。此外，您可以点击重定向到另一个页面，也可以在成功登录后重定向到另一个页面。

因此，让我们看看 jquery 中重定向到另一个页面以及如何用 javascript 重定向页面。

还有最流行的重定向方法是 **location.href** 和 **location.replace.**

**语法:**

```
location.href = "URL"
      or
location.replace("URL")
```

**举例:**

```
// Same as clicking on a link
window.location.href = "https://websolutionstuff.com/";

// Same as an HTTP redirect
window.location.replace("https://websolutionstuff.com/");
```

*   如果你想模拟某人点击一个链接，使用`**location.href**`
*   如果你想模拟一个 HTTP 重定向，使用`**location.replace**`

**位置示例. href:**

**location.href** 是设置或返回当前页面的完整 URL 的属性。

```
<!DOCTYPE html>
<html>
<body>

<p>Click the button to set the href value to https://websolutionstuff.com/quick-links</p>

<button onclick="myHrefExample()">Websolutionstuff - Quick Links</button>

<script>
function myHrefExample() {
  location.href = "https://websolutionstuff.com/quick-links";
}
</script>

</body>
</html>
```

**另请参阅:** [**如何在 Javascript 中从数组中移除特定项**](https://websolutionstuff.com/post/how-to-remove-specific-item-from-array-in-javascript)

**位置替换示例:**

**location.replace** 方法用新文档替换当前文档。

```
<!DOCTYPE html>
<html>
<body>

<button onclick="myReplaceExample()">Click Here</button>

<script>
function myReplaceExample() {
  location.replace("https://websolutionstuff.com/")
}
</script>

</body>
</html>
```

**示例位置分配:**

加载新文档的 **location.assign** 方法。

**注:**

赋值()和替换()的区别

replace()从文档历史记录中删除当前 URL。

使用 replace()无法使用“back”导航回原始文档。

```
<!DOCTYPE html>
<html>
<body>

<button onclick="myAssignExample()">Assign Redirect Example</button>

<script>
function myAssignExample() {
  location.assign("https://websolutionstuff.com/");
}
</script>

</body>
</html>
```

**阅读另:** [**如何使用 Javascript 生成条形码**](https://websolutionstuff.com/post/how-to-generate-barcode-using-javascript)

**例如:**

在本例中，我们将使用 jquery 中的 **prop()** 方法设置 location 属性中的 URL。

```
<!DOCTYPE html>
<html>
<body>

<p>Click the button to redirect another page</p>

<button onclick="redirectFunction()">Redirect</button>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>

<script>
function redirectFunction() {  	
  	var url = 'https://websolutionstuff.com/';
	$(location).prop('href', url);
}
</script>

</body>
</html>
```

**示例:**

在 javascript 或 jquery 中有很多方法可以重定向到另一个页面。

```
// window.location
window.location.replace('https://websolutionstuff.com/')
window.location.assign('https://websolutionstuff.com/')
window.location.href = 'https://websolutionstuff.com/'
document.location.href = 'https://websolutionstuff.com/'

// window.history
window.history.back()
window.history.go(-1)

// window.navigate; ONLY for old versions of Internet Explorer
window.navigate('top.jsp')

// Probably no bueno
self.location = 'https://websolutionstuff.com/';
top.location = 'https://websolutionstuff.com/';

// jQuery
$(location).attr('href','https://websolutionstuff.com/')
$(window).attr('location','https://websolutionstuff.com/')
$(location).prop('href', 'https://websolutionstuff.com/')
```

**你可能也会喜欢:**

*   **阅读也:** [**Laravel 9 电话号码验证使用 Regex**](https://websolutionstuff.com/post/laravel-9-phone-number-validation-using-regex)
*   **阅读另:** [**如何使用 jQuery 验证电话号码**](https://websolutionstuff.com/post/how-to-validate-phone-number-using-jquery)

如果这篇帖子有帮助，请点赞、分享、comment✌️