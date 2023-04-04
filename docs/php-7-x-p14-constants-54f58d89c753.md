# PHP — P14:常量

> 原文：<https://blog.devgenius.io/php-7-x-p14-constants-54f58d89c753?source=collection_archive---------12----------------------->

![](img/7b5cf63bb4ffb275cd23c6b84532f8e9.png)

常数与变量相似，只是它们不能改变。它们不能改变，因为常量存储在只读数据段中。为什么您不想保持数据不变？一个很好的例子是当您想要存储数据库连接信息时。您不希望稍后在代码中某处意外更改连接信息，从而意外切断数据库连接。

我们如何在代码中定义常量？用 define 函数。define()接受两个参数:

1.  第一个参数是常量名称。
2.  第二个参数是常量值。

```
<?phpdefine("DB_USERNAME", "dances_with_squirrels");
define("DB_PASSWORD", "hakuna_matata");?>
```

按照惯例，常量名称全部使用大写字符声明。这纯粹是为了让查看您的代码的人，包括您自己，能够毫不费力地看到哪些值是常量。

常量遵循与变量相同的命名约定。常量可以以字母或下划线开头，后跟一系列字母、下划线和/或数字。主要的区别是常量不像变量那样以美元符号开头。

为了调用我们刚刚创建的常量，我们将只使用我们在 define 函数中提供的常量名称。

```
<?phpecho DB_USERNAME; // outputs dances_with_squirrels?>
```

常数的实际用途是存储特定图像序列的文件夹位置。例如，假设我们将熊的图片存储在/images/animals/bears/ folder 中。对我们来说，不断地给每个图像添加完整的文件夹路径会变得非常重复。如果我们写砸了怎么办？如果以后我们想改变代码，把所有的东西从/images/animals/bears/移到/images/animals/哺乳动物/bears/怎么办？根据我们有多少图像，我们将不得不修改每个位置；通过使用常数，您只需修改一个位置。你想使用常量的原因是你不想在代码的后面某个地方意外地改变文件夹的位置。

```
<?phpdefine("IMG_FOLDER", "/images/animals/bears/");echo IMG_FOLDER . "black_bear.jpg";?>
```

从 PHP 7 开始，你可以在常量中存储多维数组。

```
<?phpdefine("CAR", [
  "year" => 1995,
  "make" => "Toyota",
  "model" => "Supra",
  "specs" => [
    "hp" => 1000,
    "tq" => 1200
  ]
]);var_dump(CAR);?>
```

创建常量时，不必使用 define 函数。您可以在常量名前面加上 **const** 关键字。我更喜欢 const 关键字。

```
<?phpconst FAV_FOOD = "Burger";echo FAV_FOOD;?>
```

PHP 也有神奇的常量，比如 __DIR__。我将单独写一篇关于魔法常数的文章，我们到时候再讨论。

[](https://github.com/dinocajic/php-7-youtube-tutorials) [## dinocajic/PHP-7-YouTube-教程

### PHP 7.x YouTube 教程的代码。

github.com](https://github.com/dinocajic/php-7-youtube-tutorials) ![](img/0767d37e5e73f2bb5673b45a8775d74a.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)