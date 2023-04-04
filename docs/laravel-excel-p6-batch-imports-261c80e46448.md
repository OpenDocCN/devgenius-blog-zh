# laravel-Excel-P6:批量导入

> 原文：<https://blog.devgenius.io/laravel-excel-p6-batch-imports-261c80e46448?source=collection_archive---------8----------------------->

![](img/7a7a673ea4981ff0a75d65932779b093.png)

到目前为止，当我们将 excel 表导入数据库时，我们所做的是为每一行创建一个导入:每次都会发生一个实际的插入查询。如果我们有 1000 行呢？没错:将会发生 1000 次插入查询。Laravel 允许批量插入，Laravel-Excel 支持这种形式的插入。如果将批处理大小设置为 100，这意味着只发生 10 次查询，而不是 1000 次。

批量插入限制了查询次数，提高了速度。要在单个批处理中插入的行数将视具体情况而定。开始试验要导入的行数，看看什么有效。1000 行的批处理大小可能不是最终的解决方案。这实际上取决于多种因素，比如每行处理的数据量。

[](https://dinocajic.medium.com/laravel-excel-p5-importing-sheets-with-formulas-4016dcc68862) [## Laravel-Excel — P5:导入带有公式的工作表

### 在导入 excel 表格时要注意的最后一件事是，公式通常不会像您预期的那样运行…

dinocajic.medium.com](https://dinocajic.medium.com/laravel-excel-p5-importing-sheets-with-formulas-4016dcc68862) 

# 实现批量插入

要实现批量插入，Laravel-Excel 只需要`WithBatchInserts`关注点和需要的方法`batchSize`。

就是这样。Laravel-Excel 让它变得超级简单。让我们在我们的`UserController`和我们的路线中创建方法，以便我们可以测试。我使用了下面的[模拟数据](https://gist.github.com/dinocajic/ba325bd291c79b8029628adaa2603bec)。

运行导入产生了我们想要的结果。

# 插入新的和更新现有的

我们还可以实现导入新项目但更新现有项目的功能。例如，`email`字段是`users`表中的唯一字段。如果`email`存在，更新该条目的内容，否则插入它。

为了获得这个功能，我们实现了`WithUpserts`关注点，并指定哪个字段在`uniqueBy`方法中是惟一的。

在不清除您的`users`表的情况下，再次运行您的导入，并查看它是否没有中断。如果您更新了任何字段，如名字或姓氏，这些字段将被更新。

[](https://github.com/dinocajic/package-laravel-excel) [## GitHub-dinocajic/package-Laravel-Excel:显示了 Laravel Excel 的示例功能

### Laravel 是一个 web 应用程序框架，具有丰富、优雅的语法。我们相信发展必须是令人愉快的…

github.com](https://github.com/dinocajic/package-laravel-excel) ![](img/63f574d211354528afd1c653ac542fdc.png)

迪诺·卡伊奇目前是 [LSBio(生命周期生物科学公司)](https://www.lsbio.com/)、[绝对抗体](https://absoluteantibody.com/)、 [Kerafast](https://www.kerafast.com/) 、[珠穆朗玛生物](https://everestbiotech.com/)、[北欧 MUbio](https://www.nordicmubio.com/) 和 [Exalpha](https://www.exalpha.com/) 的 IT 主管。他还担任我的自动系统的首席执行官。他有十多年的软件工程经验。他拥有计算机科学学士学位，辅修生物学。他的背景包括创建企业级电子商务应用程序、执行基于研究的软件开发，以及通过写作促进知识的传播。

你可以在 [LinkedIn](https://www.linkedin.com/in/dinocajic/) 上联系他，在 [Instagram](https://instagram.com/think.dino) 上关注他，或者[订阅他的媒体出版物](https://dinocajic.medium.com/subscribe)。

[*阅读迪诺·卡吉克(以及媒体上成千上万其他作家)的每一个故事。你的会员费直接支持迪诺·卡吉克和你阅读的其他作家。你也可以在媒体上看到所有的故事。*](https://dinocajic.medium.com/membership)