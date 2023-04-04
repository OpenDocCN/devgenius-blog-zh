# 普里是如何教我如何使用红宝石的

> 原文：<https://blog.devgenius.io/how-pry-taught-me-how-to-oo-ruby-433d0f842829?source=collection_archive---------14----------------------->

最初，我想写下关于 T0 的所有荣耀，而使用它可以帮助一个 Ruby 新手更快地掌握 OO 的概念。并帮助建立一个防弹的理解心智模型。`Self`在 object 的实例中是指实例本身，在 class 方法中，self 是指支配性的类。我是怎么走到这一步的？

特别是使用 OO Ruby，我已经看不到来自 [Mike Dane](https://www.mikedane.com/) (是的，来自*长颈鹿学院*的家伙)的内容了，烧代码不足以帮助我理解一个对象是如何自我感知的。当时，我们突然沉浸在一个由`self`、`@instance`、`@@class`、`<<`统治的世界中，这也于事无补。

在我的例子中，它使用了 gem `Pry`并编写了我自己的测试来确认 d̶e̶b̶u̶g 的代码按预期工作。IMHO，`[Pry](https://pry.github.io/)`是 Ruby 的标准 IRB shell 及其运行时调用的一个更好的替代方案。这意味着可以在运行程序的过程中对对象调用窥探会话。

为了评估之前的任何一点代码，放下`binding.pry`将启动一个交互式 shell，在其中你将有机会*通过代码窥探*。如果将它插入到一个其所有者不太清楚的方法中，这将特别有帮助。

```
@@all << self ...def self.all
  @@all
end ...def self.class_method
  binding.pry  # execution will freeze for the pry instance
  # does something
enddef instance_method
  binding.pry  # execution will freeze for the pry instance
  # does something
end
```

> 谁在调用该方法？类的`@instance`变量？或者说，`@@class`变量？`self`是谁或什么？

与撬，眼见为实特别为了解`self`；这个特殊的关键字让我们可以访问当前对象，无论是类的实例还是类本身。在下一节中，我们将提供一些代码片段来了解 Ruby 对象的生命周期。

C 配置为 [Rake 任务](https://www.seancdavis.com/posts/how-to-write-a-custom-rake-task/)，Pry 依赖于环境(首先加载特定的类和数据库连接)。撬工具可以通过输入`rake console`来触发。

```
# Rakefile
desc 'drop into the Pry console'
task console: :environment do
  Pry.start
end
```

窥探会话提供了使用查询方法通过关系(关系数据库)遍历数据集合的机会，这些查询方法包括但不限于:`.all`、`.first`、`.last`、`.find(id#)`、`.find_by(attribute:)`等。

```
def give_stuff(dev, item_name, value) Stuff.create(dev_id: dev.id, item_name: item_name, value: value, company_id: self.id)end# Rake console
[1] **pry(main)> Dev.all**
D, [2022-03-08T22:14:51.740658 #9806] DEBUG -- :   Dev Load (0.3ms)  SELECT "devs".* FROM "devs"
**=> [#<Dev:0x0000000136c5dca8 id: 1, name: "Rick">, #<Dev:0x0000000136c14788 id: 2, name: "Morty">, #<Dev:0x0000000136c145f8 id: 3, name: "Mr. Meseeks">, #<Dev:0x0000000136c0fdc8 id: 4, name: "Gazorpazop">]**
[2] **pry(main)> Dev.find(2)**
D, [2022-03-08T22:15:44.905104 #9806] DEBUG -- :   Dev Load (0.3ms)  SELECT "devs".* FROM "devs" WHERE "devs"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
**=> #<Dev:0x000000010782dd38 id: 2, name: "Morty">**
[3] pry(main)> **d = Dev.find(2)**
D, [2022-03-08T22:16:24.410506 #9806] DEBUG -- :   Dev Load (0.2ms)  SELECT "devs".* FROM "devs" WHERE "devs"."id" = ? LIMIT ?  [["id", 2], ["LIMIT", 1]]
**=> #<Dev:0x0000000136f63ee8 id: 2, name: "Morty">**
[4] pry(main)> **d.name**
**=> "Morty"**
```

在上面的窥探会话和下面的代码片段中，变量可以被分配给实例并用于执行该方法。

```
# Rake console
[1] pry(main)> **c.give_stuff(d, "computer", 2)**
D, [2022-03-08T22:25:32.036343 #9980] DEBUG -- :    (0.4ms)  SELECT sqlite_version(*)
D, [2022-03-08T22:25:32.037006 #9980] DEBUG -- :   Company Load (0.0ms)  SELECT "companies".* FROM "companies" ORDER BY "companies"."id" DESC LIMIT ?  [["LIMIT", 1]]
D, [2022-03-08T22:25:32.039061 #9980] DEBUG -- :   Dev Load (0.0ms)  SELECT "devs".* FROM "devs" ORDER BY "devs"."id" DESC LIMIT ?  [["LIMIT", 1]]
D, [2022-03-08T22:25:32.041861 #9980] DEBUG -- :   TRANSACTION (0.0ms)  begin transaction
D, [2022-03-08T22:25:32.042295 #9980] DEBUG -- :   Stuff Create (0.3ms)  INSERT INTO "stuffs" ("item_name", "value", "company_id", "dev_id") VALUES (?, ?, ?, ?)  [["item_name", "computer"], ["value", 2], ["company_id", 4], ["dev_id", 4]]
D, [2022-03-08T22:25:32.043133 #9980] DEBUG -- :   TRANSACTION (0.7ms)  commit transaction
**=> #<Stuff:0x0000000129ebe720 id: 15, item_name: "computer", value: 2, company_id: 4, dev_id: 4>**
```

使用各种查询展现数据的形状，然后您就能够清楚地理解面向对象编程:一个对象具有状态(实例变量)和行为，一个对象可以对其自身执行，也可能对其他对象执行(方法)。

值得一提的是，我们不必编写 SQL 命令来操作数据库表上的信息行。感谢 ActiveRecord，脏活累活替我们干了！

一想到必须编写 SQL 命令就不寒而栗。

![](img/c610bdf2bc01e2926cae3b17b5e7dcfc.png)

资料来源:xkcd

遍历代码的另一个好地方是在数组中循环。你可以在兔子洞的深处，却不知道哪条路是上还是下。你可以通过使用`whereami`或它的别名`@`来获得方向感，并尝试放松退出，而不是立即退出撬拨会话。

直到下一次…

![](img/8f2de9a7f9d0267f26cf7ce04d96fd05.png)

使用 binding.pry 学习 Ruby

PS。其他让生活变得更简单且值得一提的非窥探实用程序:

*   【VSCode 的 SQLite 扩展
*   [vs code 的 Ruby solar graph](https://marketplace.visualstudio.com/items?itemName=castwide.solargraph)
*   [dbdiagram.io](https://dbdiagram.io/home) 的实体关系图