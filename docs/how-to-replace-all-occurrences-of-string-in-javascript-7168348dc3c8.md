# 如何替换 Javascript 中出现的所有字符串

> 原文：<https://blog.devgenius.io/how-to-replace-all-occurrences-of-string-in-javascript-7168348dc3c8?source=collection_archive---------15----------------------->

在本文中，我们将看到如何替换 javascript 中出现的所有字符串。

您可以使用 javascript replace()和 replaceAll()方法来替换所有出现的字符串。

因此，我们将学习如何用正则表达式替换所有出现的内容。

所以，让我们看看 javascript 替换所有出现的字符串，jquery 替换所有出现的字符串，regex 替换所有出现的字符串。

**replace()** 方法在一个字符串中搜索一个值，并返回一个替换了该值的新字符串。

同样，replace()方法不改变原始字符串。

**语法:**

**string.replace(搜索值，新值)**

**replaceAll()** 方法通过替换出现的所有字符串来返回一个字符串。

**语法:**

**string.replaceAll(搜索值，替换值)**

**示例:**

```
<!DOCTYPE html>
<html lang="en">
<body>
    <h4>Original String</h4>
    <p>PHP is an easy programming language.</p>
    <h4>String After Replacement</h4>

    <script>
        var myStr = 'PHP is an easy programming language.';
        var newStr = myStr.replace('PHP','Laravel');

        // Printing the modified string
        document.write('<p>' + newStr + '</p>');

    </script>
</body>
</html>
```

**输出:**

> **原始字符串**
> 
> PHP 是一种简单的编程语言。
> 
> **更换后的管柱**
> 
> Laravel 是一种简单的编程语言。

**阅读也:** [**如何使用 Javascript**](https://websolutionstuff.com/post/how-to-redirect-another-page-using-javascript) 重定向另一个页面

**例如:**

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>JavaScript Replace All Occurrences a String</title>
</head>
<body>
    <h4>Original String</h4>
    <p>This is multiple occurrences example. multiple occurrences string example.</p>
    <h4>String After Replacement</h4>

    <script>
        var myStr = 'This is multiple occurrences example. multiple occurrences string example.';
        var newStr = myStr.replace(/multiple/g, "two");

        // Printing the modified string
        document.write('<p>' + newStr + '</p>');
    </script>
</body>
</html>
```

**输出:**

> **原始字符串**
> 
> 这是多次出现的例子。多次出现的字符串示例。
> 
> **更换后的管柱**
> 
> 这是两个例子。两次出现的字符串示例。

**例如:**

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>JavaScript Replace All Words String Example</title>
</head>
<body>
    <h4>Original String</h4>
    <p>This is replaceAll string example. replaceAll string example.</p>
    <h4>String After Replacement</h4>

    <script>    

    var myStr = 'This is replaceAll string example. replaceAll string example.';
    var newStr = replaceAll(myStr, 'replaceAll', 'multiple')

    document.write('<p>' + newStr + '</p>');
    </script>
</body>
</html>
```

**输出:**

> **原始字符串**
> 
> 这是 replaceAll 字符串示例。replaceAll 字符串示例。
> 
> **更换后的管柱**
> 
> 这是多个字符串的例子。多字符串示例。

**另请参阅:** [**如何在 Javascript**](https://websolutionstuff.com/post/how-to-remove-specific-item-from-array-in-javascript) 中从数组中移除特定项

**示例:**

```
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<title>JavaScript Regular Expression to Replace All Words</title>
</head>
<body>
    <h4>Original String</h4>
    <p>abc xyz abc</p>
    <h4>String After Replacement</h4>

    <script>

    function escapeRegExp(string){
        return string.replace(/[.*+?^${}()|[\]\\]/g, "\\$&");
    }

    function replaceAll(str, term, replacement) {
      return str.replace(new RegExp(escapeRegExp(term), 'g'), replacement);
    }

    var myStr = 'abc xyz abc';
    var newStr = replaceAll(myStr, 'abc', 'xyz')

    document.write('<p>' + newStr + '</p>');
    </script>
</body>
</html>
```

**输出:**

> **原字符串**
> abc xyz abc
> 
> **字符串替换后**
> xyz xyz xyz

**你可能也会喜欢:**

*   **另请阅读:** [**如何在 Ajax jQuery 中显示加载微调器**](https://websolutionstuff.com/post/how-to-show-loading-spinner-in-ajax-jquery)
*   **阅读另:** [**数据表在 jQuery**](https://websolutionstuff.com/post/datatables-show-and-hide-columns-dynamically-in-jquery) 中动态显示和隐藏列

如果这篇帖子有帮助，请点赞，分享和 comment✌️.