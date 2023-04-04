# 最常见的 Java/JEE 面试问题(第 1 部分)

> 原文：<https://blog.devgenius.io/most-commonly-asked-java-jee-interview-questions-part-1-8ec5072d484d?source=collection_archive---------5----------------------->

![](img/fb06f3b7b0da9793d0940fbca48f6938.png)

米歇尔·勒恩斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

在这篇文章中，我将谈论一些最常见的 Java/JEE 面试问题。

## **1-什么是 J2EE？**

J2EE 的意思是 Java 2 企业版。

J2EE 的功能是开发多层基于 web 的应用程序。

J2EE 平台由一组服务、应用编程
接口(API)和协议组成。

J2EE 应用程序的四个组成部分是什么？
应用客户端组件。
Servlet 和 JSP 技术都是 web 组件。
业务组件(JavaBeans)。
资源适配器组件

**3)J2EE 客户有哪些类型？**
小程序
应用客户端
支持 Java Web Start 的客户端，采用 Java Web Start 技术。
基于 MIDP 技术的无线客户端。

**4)什么是 web 组件？**
Java Servlet 和 Java Server Pages 技术组件都是 web 组件。

Servlet 是一个 Java 软件组件，它动态地接收请求并做出响应。

JSP 页面作为 servlets 执行，但是允许用更自然的方法来创建静态内容。

## 什么是 JSF？

JavaServer Faces (JSF)是一个为 Java web 应用程序设计用户界面(UI)的框架。

JSF 提供了一组可重用 UI 组件，这是 web 应用程序的标准。

JSF 是基于 MVC 设计模式的。

## **6)定义哈希表**

哈希表就像哈希表，但是是同步的。

**哈希表**存储键/值对。

## 什么是冬眠？

Hibernate 是一个开源的对象关系映射和查询服务。

在 hibernate 中，我们可以编写 HQL 来代替 SQL，这样可以节省开发人员花在编写原生 SQL 上的时间。

Hibernate 有更强大的关联、继承、多态、组合和集合。

## **8)hibernate 的局限性是什么？**

执行查询比直接使用查询要慢。
仅查询语言支持组合键。
没有对值类型的共享引用。

## 冬眠的优势是什么。

Hibernate 是数据库独立的。它可以用来连接任何数据库，如 Oracle、MySQL、Sybase 和 DB2 等等。

Hibernate 支持一种强大的查询语言，叫做 HQL (Hibernate Query Language)。

Hibernate 的透明持久性确保了应用程序对象与数据库表之间的自动连接。

## 什么是 ORM？

ORM 代表对象关系映射。

Java 类中的对象，使用描述对象和数据库之间映射的元数据映射到关系数据库的表中。

它的工作原理是将数据从一种表示转换成另一种表示。

## **11)保存和保存或更新的区别**

## **a)保存()**

hibernate 中的这个方法用于将对象存储到数据库中。

如果记录不存在，则插入一个条目，否则不插入。

**b)saveourupdate()**
这个方法在 hibernate 中用于更新使用标识符的对象。

如果缺少标识符，该方法调用 save()。

如果标识符存在，它将调用 update 方法。

Hibernate 基于我们的映射在运行时生成大量的 SQL 语句，所以它比 JDBC 慢了一点。

## load 和 get 方法的区别？

***如果找不到对象，get()*** 方法返回 null。

***load()*** 方法可能会返回一个代理，而不是一个真正的持久实例 ***get()*** 永远不会返回代理。
**13)hibernate 中如何调用存储过程？**
{？=调用 thissitheprocedure()}

ORM 有什么好处？
生产率
可维护性
性能
厂商独立性

**15)hibernate 框架的核心接口有哪些？**
会话接口
会话工厂接口
配置接口
事务接口
查询和条件接口

**16)hibernate 映射文件使用的文件扩展名是什么？**
文件名应该是这样的:filename.hbm.xml

**17)hibernate 配置文件的文件名是什么？**
文件名应该是这样的:hibernate.cfg.xml

**18)hibernate 或 JPA 是如何独立于数据库的？**
数据库独立性是指编写不依赖于数据库供应商的代码。

Hibernate，或者总的来说 JPA，阻止你根据 Oracle 规范或者 MySQL 规范编写代码。

您使用 JPA 类和接口，并让 JPA 实现(如 Hibernate)完成剩下的工作。

## 19)定义连接池？

连接池是一种重用连接的机制。

它包含已经创建的对象连接的数量。

所以，每当有一个必要的 for 对象时，这个机制就被用来
直接获取对象而不用创建它。

什么是 hibernate 代理？对象代理只是一种避免在你需要之前获取对象的方法。默认情况下，Hibernate 2 不代理对象。

## 什么是 HQL？

HQL 代表 Hibernate 查询语言。

Hibernate 允许用户用自己的可移植 SQL 扩展来表达查询，这被称为 HQL。

它还允许用户用原生 SQL 来表达。

## **22)Hibernate 中的集合类型有哪些？**

集合，列表，数组，地图，包

# 结论:

所以这就把我们带到了 Java 面试问题第一部分的结尾。

这些 Java 面试问题一定会帮助你在求职面试中取得成功。

感谢检查下一部分。

祝你好运:-)

**如果你发现这个有用，点击那个**👏**按钮:)**

## 奖金:

技术好固然很好，但要想成功，你还必须擅长沟通。

沟通技巧，在为框架和库编写文档时，或者在给同事发送电子邮件或松散消息时，发挥着重要作用。

他们是两个或更多人如何相互传达复杂想法和概念的重要因素，这是作为软件开发人员进行协作的核心。

最近，沟通技能已经成为软件开发人员面试的一个重要组成部分，大多数公司会检查候选人的沟通技能水平。

所以，拥有一个工具来帮助你写出大胆、清晰、无错的文章是非常好的。

我推荐 [**Grammarly 的 AI 助力写作助手**](https://www.jdoqocy.com/click-100203662-11275739) **。**

一些您可能感兴趣的相关文章:

[1-5 条原则会让你的代码更加健壮](https://selcote.com/2020/11/09/5-principles-will-make-your-code-robust/)

2-OOP 现在是计算机科学的基础

[有史以来最好的 3- 6 名程序员](https://selcote.com/2020/10/27/6-best-programmers-of-all-time/)

[4-未来编程最有前途的领域](https://selcote.com/2020/10/22/the-most-promising-fields-for-programming-in-the-future/)

Web 开发中最常用的 5 种语言

与我联系:[博客](https://selcote.com/)， [Youtube](https://www.youtube.com/channel/UCU_LhClyNOtEQw7R0q9ovoQ?view_as=subscriber) ，[脸书](https://www.facebook.com/zelakioui)，[推特](https://twitter.com/zelakioui)

来源:[selcote.com](https://selcote.com/2020/11/12/most-commonly-asked-java-jee-interview-questions-part-1/)