# 如何用 Ruby on Rails“播种”

> 原文：<https://blog.devgenius.io/how-to-seed-with-ruby-on-rails-%EF%B8%8F-1d2dceda3e7d?source=collection_archive---------0----------------------->

![](img/61192a611dc3187204e031fa591acdcf.png)

安德烈科·波迪尔尼克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

本文旨在帮助那些想了解 Rails 中 seed 特性的人。在这篇文章之后，读者将能够用预定义的记录快速填充他们的数据库。

在一个[标准 Rails 项目](https://github.com/kubilaycaglayan/rails-react-template)中，我们在`[db/seed.rb](https://github.com/kubilaycaglayan/rails-react-template/tree/development/db)`有一个**种子文件**。

这个文件由`rails db:seed`命令触发，并在 Rails 上下文中运行。这意味着所有的模型结构将在文件中可用。

它也由`rails db:reset`和`rails db:setup`命令触发。

顾名思义，这个文件的主要目的是给数据库播种。

这是一个非常有用的特性，尤其是当您处于学习阶段，一天中要多次重置数据库时。

在这种情况下，您不希望手动将记录添加到数据库中，而是希望将它们保存在`db/seed.rb`文件中。

## 在活动

假设我们有一个带有用户模型的 Rails 应用程序，在这个模型中，我们可以用名字属性来创建用户。

如果我们希望我们的数据库在初始化后有两个用户，我们可以简单地在种子文件中创建这些记录。

```
# db/seed.rbputs "Seeding..."User.create(name: 'Matz')
User.create(name: 'DHH')puts "Seeding done."
```

我们有三个选项来激活该文件:

1.  当**数据库已经创建时**T5 将是最佳选择。
2.  换句话说，当数据库还没有准备好时，我们想先创建数据库，然后我们可以先用`rails db:setup`运行迁移，然后用**播种**。
3.  最后一个选项是当我们想要**重置数据库**时，`rails db:reset`将删除数据库，再次创建，并植入应用程序。

以上所有选项都将这两条记录植入数据库。

## 播种 CSV

在某些情况下，我们可能希望在应用程序中植入数千行记录。

假设我们希望将数据以逗号分隔值的形式从不同的数据库传输到 Rails 应用程序。

那么我们可能有一个目录来保存。csv 文件。姑且说是在`db/data/users.csv`里。

```
# db/data/users.csvname, age
Matz, 55
DHH, 41
```

在这种情况下，我也更喜欢使用一个隔离的文件来保存“CSV 到模型的映射”，像`db/user_seeding.rb`。

在`db/user_seeding.rb`中，我们可以创建从 CSV 到模型的映射，并借助 Ruby 中的 CSV 类逐个创建记录。这个逻辑将被封装在一个方法中，并在`db/seed.rb`文件中被调用。

*   `db/user_seeding.rb`⏬

当你准备好`db/user_seeding.rb`时，你可以在`db/seed.rb`中调用它⏬

## 播种 Heroku

从`heroku run`开始，你也可以在 Heroku 上使用 seed 命令。

例子:`heroku run rails db:seed`

## 结论

知道如何播种对你很有帮助，尤其是在发展阶段。
准备填充的数据应该总是在您的种子文件中。通过这种方式，您可以将更多的精力放在改进应用程序的主要部分上，而不是将时间花在构建初始数据库状态上。

## 感谢

*   [https://edge guides . ruby on rails . org/active _ record _ migrations . html # migrations-and-seed-data](https://edgeguides.rubyonrails.org/active_record_migrations.html#migrations-and-seed-data)➡️`rails db:seed`
*   [https://guides . ruby on rails . org/active _ record _ migrations . html # setup-the-database](https://guides.rubyonrails.org/active_record_migrations.html#setup-the-database)➡️`rails db:setup`
*   [https://guides . ruby on rails . org/active _ record _ migrations . html #重置数据库](https://guides.rubyonrails.org/active_record_migrations.html#resetting-the-database) ➡️ `rails db:reset`
*   [https://ruby-doc.org/stdlib-2.6.1/libdoc/csv/rdoc/CSV.html](https://ruby-doc.org/stdlib-2.6.1/libdoc/csv/rdoc/CSV.html)➡️`CSV class`
*   [https://ruby-doc.org/core-2.5.0/File.html#method-c-new](https://ruby-doc.org/core-2.5.0/File.html#method-c-new)➡️`File.new`

感谢您的阅读😊

[kubilaycaglayan.com](https://kubilaycaglayan.com/)👋