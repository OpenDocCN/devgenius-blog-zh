# PHP — P78:检查文件类型

> 原文：<https://blog.devgenius.io/php-p78-checking-file-types-ffb2762d2d19?source=collection_archive---------5----------------------->

![](img/722d5a134273ee6f8bb22550762ebd24.png)

我们在上一篇文章中看到了文件上传，并使用了最少的步骤来完成它。这一次，让我们看看流程上传脚本中可能存在的一些其他功能。如果您还没有这样做，请阅读我关于文件上传基础的文章。

[](/php-p77-basics-of-file-uploading-ba377da3d072) [## PHP — P77:文件上传基础

### PHP 初学者最容易误解的话题之一。有人如何上传文件？当您点按时会发生什么…

blog.devgenius.io](/php-p77-basics-of-file-uploading-ba377da3d072) 

# 概述

我们有一个基本的 HTML 表单和一个简单的流程脚本。

# 检查文件类型

永远不要相信用户。即使你指定用户应该上传图片，他们会按照你的指示做吗？大多数用户会，但有一些因为各种原因不会。有时候只是基于自己的经验不足。word 文档中的图像是图像，对吗？有时候更穷凶极恶。黑客可能想要上传一个他们可以触发的脚本。如果你不限制用户可以上传什么，他们可以上传任何东西。

因此，让我们在上传脚本中检查图像。

代码是做什么的？

*   获取文件名。
*   创建文件以`target_file`结束的位置。
*   抓取临时存储位置并将其存储在`temp_file`变量中。我们稍后需要将文件从临时位置移动到新位置。
*   设置允许的扩展名数组。有很多方法可以做到这一点，但我们将采取`in_array`的方法。
*   抓取文件扩展名。如果用户上传一个`.jpg`，文件扩展名是一个`jpg`。
*   如果文件扩展名不在数组中，应用程序就会终止。
*   如果文件扩展名在数组中，文件将从临时位置移动到永久位置。
*   将显示一条成功消息。

在下一篇文章中，我们将介绍如何检查文件是否已经存在，以及文件大小是否合适。

![](img/1c548d51dbd03e2ccdc97af8cb99c8a9.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)