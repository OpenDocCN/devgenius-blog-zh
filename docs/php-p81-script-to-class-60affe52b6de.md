# PHP — P81:脚本到类

> 原文：<https://blog.devgenius.io/php-p81-script-to-class-60affe52b6de?source=collection_archive---------9----------------------->

![](img/c32ec1414262c0fcd05b4be7144489bd.png)

我们在前几篇文章中讨论了文件上传，并有意偏离了面向对象编程。为什么？因为大多数时候，你在网上找到这样一个问题的解决方案，它不会很好地为你展示出来。它将只是一堆代码，不会完全适合你的架构。我记得当我还是一名初级开发人员时，这曾让我有些焦虑。“难道他们不能把这个作为一个类来创建，而不是这个脚本，”我发现自己经常问。自己做这件事真的没那么远，而且你会经常做。

如果您有兴趣了解我们是如何构建整个上传脚本的，请阅读下面的文章。

[](/php-p77-basics-of-file-uploading-ba377da3d072) [## PHP — P77:文件上传基础

### PHP 初学者最容易误解的话题之一。有人如何上传文件？当您点按时会发生什么…

blog.devgenius.io](/php-p77-basics-of-file-uploading-ba377da3d072) [](/php-p78-checking-file-types-ffb2762d2d19) [## PHP — P78:检查文件类型

### 我们在上一篇文章中看到了文件上传，并使用了最少的步骤来完成它。这一次…

blog.devgenius.io](/php-p78-checking-file-types-ffb2762d2d19) [](/php-p79-file-size-checks-e419bc6483bf) [## PHP — P79:文件大小检查

### 上传文件时，我们还需要进行一些检查。我们已经上传了一个文件并限制…

blog.devgenius.io](/php-p79-file-size-checks-e419bc6483bf) [](https://dinocajic.medium.com/php-p80-final-upload-checks-8b24427fd928) [## PHP — P80:最终上传检查

### 为了巩固我们的表单提交，我们还需要完成一些检查。你再小心也不为过…

dinocajic.medium.com](https://dinocajic.medium.com/php-p80-final-upload-checks-8b24427fd928) 

# 概述

我们有一个基本的 HTML 表单和一个简单的流程上传脚本。

我们的上传脚本的作用是:

*   用户选择文件并点击`Upload`按钮后，首先抓取文件名和临时文件位置。
*   接下来，它会检查最终目标中是否存在具有相同文件名的文件。
*   根据允许的项目列表检查文件扩展名。
*   它会继续检查图像大小，因为用户可能会添加有效的扩展名，但它仍然会上传不是图像的文件。
*   检查最大文件大小是为了防止用户上传非常大的文件。
*   如果一切顺利，文件将从临时位置移动到永久位置。

# 面向对象的方法

查看我们现有的文件，我们看到代码块可以组合在一起。这些都将成为我们的职能。让我们开始分组。

## **file_exists()**

```
if ( file_exists($target_file) )
{
    die("The file already exists");
}
```

## 允许的文件扩展名()

```
$allowed   = ['jpg', 'png'];
$extension = pathinfo($file_name, *PATHINFO_EXTENSION*);

if ( ! in_array($extension, $allowed) )
{
    die("The format is not correct. You may only upload: " . 
         implode(", ", $allowed)
       );
}
```

## 检查图像大小()

```
if ( getimagesize($temp_file) === false )
{
    die("You must upload an image");
}
```

## check_max_size()

```
$max_size_mb = 0.5;
$max_size_bytes = $max_size_mb * 1024 * 1024;

if ( $_FILES["file_name"]["size"] > $max_size_bytes )
{
    die("The file size is too large. You may only upload up to: " . 
        $max_size_mb . "MB"
       );
}
```

很容易将它们分开，因为我们就是这样构建它们的。为了我们的目的，我想我们保持简单。你可以随心所欲地发挥创造力。

*   `index.html`文件持有形式。
*   `upload.php`是获取数据并实例化我们的类的文件。它还会将数据传递给我们的对象。
*   将由我们班来承担重任。

```
app/
   uploads/
   index.html
   upload.php
   ImageUpload.php
```

我们知道`index.html`将会是什么样子。那里不需要改变。

现在，让我们为`ImageUpload.php`文件创建一个类框架。我们将在最后查看，尽管实际上我会在处理我的`upload.php`文件的同时查看它。

我给它添加了一个名称空间，只是为了好玩。请记住，名称空间只是虚拟目录。如果您需要复习名称空间，可以在这里阅读。

[](/php-p69-defining-namespaces-5a8e0a4bfdc0) [## PHP — P69:定义名称空间

### 定义名称空间非常简单。在文件的顶部，就在开始的 PHP 标签之后，名称空间关键字是…

blog.devgenius.io](/php-p69-defining-namespaces-5a8e0a4bfdc0) 

太好了，接下来让我们继续处理我们的`upload.php`文件。我们需要做的就是实例化我们的`ImageUpload`对象，并传递给它`$_FILES`数组。我们不会将整个`$_FILES`数组传递给它，因为它由一个文件数组组成。我们只是把上传的文件传给它。它将被存储在`$_FILES["file_name"]`中。

还记得名称空间吗？我们需要将它添加到`ImageUpload`的开头。我们完成了`upload.php`。其他的都将在`ImageUpload`类中处理。

# 图片上传

我们知道 upload.php 正在实例化 ImageUpload 对象，并向我们发送了`$_FILES["file_name"]`数组。让我们在构造函数中获取该数组，并将其分配给一个类属性。

该文件通过构造函数传入，并被分配给`$_file`属性。代码的下一部分取决于您。我们应该进行所有的检查`private`还是`public`？这个类的唯一功能应该是上传一张图片，还是我们应该允许用户使用这些功能进行零星的检查？这是你在创建类的时候需要考虑的事情。

我为什么要问这个？接下来，我们将创建一个`process_upload`函数。问题是我们是在实例化期间自动调用它(从构造函数中)还是从`upload.php`文件中调用它？我喜欢能够从`upload.php`调用它，因为如果我们想在将来扩展它，它对于错误处理来说似乎更干净。

我们收到的输出显示，到目前为止它是有效的。

```
**array** *(size=5)*
  'name' => string '2 Comments.jpg' *(length=14)*
  'type' => string 'image/jpeg' *(length=10)*
  'tmp_name' => string '/tmp/phpDh5748' *(length=14)*
  'error' => int 0
  'size' => int 468430
```

我们可以继续将我们的脚本移入新的`ImageUpload`类。

```
$file_name   = $_FILES["file_name"]["name"];
$target_file = "uploads/" . $file_name;
$temp_file   = $_FILES["file_name"]["tmp_name"];
```

我们需要稍微修改一下，因为我们不再依赖于我们的`$_FILES`数组了。

接下来，让我们添加我们的`move_upload_file`方法，并确保这是可行的。

而且很管用！是时候开始添加我们的检查功能了。列表中的第一项，检查文件是否存在。这是一个如此简单的函数，为它创建一个单独的方法是没有意义的。我们会保持原样。

接下来是允许的文件扩展名列表。因为这是一个`ImageUpload`类，所以拥有一个允许文件扩展名的属性是有意义的。因此，我们可以将该数组移到一个属性中。

我们的`process_upload`方法调用了`file_extension_allowed`。如果文件扩展名与我们的`$_allowed_file_extensions array`中的扩展名匹配，那么它返回`true`，否则返回`false`。如果值为`false`，过程停止。

类似于我们的`file_exists`函数，我们真的不需要为`getimagesize`单独创建一个方法。

最后，我们需要检查最大文件大小。

我们将`$_max_size_mb`设置为一个类属性。process_upload 方法调用`check_max_file_size`方法。如果返回`true`，则文件大小超过最大文件大小。

现在来看看完整的剧本是什么样子的。

通过一点创造性，我们能够将我们的 PHP 脚本转换成一个适合我们现有架构的类。事情不应该是这样的。您可以按照自己的意愿编写独特的代码。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/09a0bbdcced62e6f461e60f0ed70eb56.png)

Dino Cajic 目前是 [Absolute Biotech](http://absolutebiotech.com/) 的 IT 负责人，该公司是 [LSBio(寿命生物科学公司)](https://www.lsbio.com/)、 [Absolute 抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、 [Everest BioTech](https://everestbiotech.com/) 、 [Nordic MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的母公司。他还担任我的自动系统的首席执行官。他拥有计算机科学学士学位，辅修生物学，并拥有十多年的软件工程经验。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

阅读 Dino Cajic(以及 Medium 上成千上万的其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。