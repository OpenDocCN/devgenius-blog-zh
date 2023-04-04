# PHP — P79:文件大小检查

> 原文：<https://blog.devgenius.io/php-p79-file-size-checks-e419bc6483bf?source=collection_archive---------8----------------------->

![](img/3476a9ce1ae78b63502632c9976649d9.png)

上传文件时，我们还需要进行一些检查。我们已经上传了一个文件，并限制用户上传所有文件类型，但仍有一些事情我们需要检查，如文件大小。我有目的地对这个主题采取一种更深入的方法，因为大多数人在看到完整的脚本运行时会感到不知所措，而从来没有看到它们的组成部分:单独的检查。

[](/php-p78-checking-file-types-ffb2762d2d19) [## PHP — P78:检查文件类型

### 我们在上一篇文章中看到了文件上传，并使用了最少的步骤来完成它。这一次…

blog.devgenius.io](/php-p78-checking-file-types-ffb2762d2d19) 

# 概述

我们有一个基本的 HTML 表单和一个简单的流程脚本。

在上面的文件上传脚本中，第一个检查是文件扩展名检查。通过后，文件将从其临时位置移动到最终位置。

# 文件大小检查

接下来我们要做的是将文件大小限制在某个阈值以上。我们需要确保文件存储不会被不必要的大文件填满。

我们如何做到这一点？让我们看看我们的`$_FILES`数组包含了哪些可能对我们有帮助的内容。

```
var_dump($_FILES);**array** *(size=1)*
  'file_name' => 
    **array** *(size=5)*
      'name' => string '75-Get-vs-Post.jpg' *(length=18)*
      'type' => string 'image/jpeg' *(length=10)*
      'tmp_name' => string '/tmp/phpbRYmkz' *(length=14)*
      'error' => int 0
      'size' => int 133821
```

显而易见。在我们的`$_FILES['file_name']`数组中有一个`size`键。要访问它，我们只需使用`$_FILES['file_name']['size']`。它是以字节为单位的，所以我们需要用字节来限制它。

```
1KB = 1024 bytes
1MB = 1048576 bytes
1GB = 1073741824 bytes
1TB = 1099511627776 bytes
```

如果我们想将用户限制在 1MB，我们需要测试 1，048，576 字节。

计算尺寸的简单技巧。1MB * 1024 * 1024 = 1048576 字节。所以你要限制 5MB，就做 5MB * 1024 * 1024 就行了。这就是我在我们的例子中列出它的方式。我想限制用户上传任何超过 1/2MB 的内容。

```
$max_size_mb = 0.5;
$max_size_bytes = $max_size_mb * 1024 * 1024;

if ( $_FILES["file_name"]["size"] > $max_size_bytes )
{
    die("The file size is too large.);
}
```

完整的脚本现在是这样的。

你可以看到每次都是这样。代码就是这样，一个缓慢的建立。在下一个脚本中，我们将处理一些最终的检查，然后我们将在第 81 篇文章中用一种更加面向对象的方法来看这个问题。下次见。

[](https://github.com/dinocajic/php-youtube-tutorials) [## GitHub-dinocajic/PHP-YouTube-tutorials:PHP YouTube 教程的代码

### PHP YouTube 教程的代码确保你已经安装了 Docker。克隆回购。运行以下命令…

github.com](https://github.com/dinocajic/php-youtube-tutorials) ![](img/3d2ab350c561cf86cfd9e485ed3a5791.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)