# 所有登上铁路嵌套路线快车

> 原文：<https://blog.devgenius.io/all-aboard-the-rails-nested-routes-express-aa311387563e?source=collection_archive---------3----------------------->

![](img/99e388a58ca854c94246c5c593b11460.png)

由[克里斯蒂安·霍尔辛格](https://unsplash.com/@pixelatelier?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 这篇博客文章将关注:

*   Rails 嵌套资源
*   ActiveRecord 查询方法

这篇博文将通过我自己对 Rails 应用的精彩评论来讨论这些问题。

什么是有价值的评论？

简单来说， *Brew-Ti-Ful Reviews* 是一个用 Rails 构建的应用，允许用户添加他们已经品尝或尚未品尝的咖啡，并给他们评论。

# Rails 嵌套资源

rails 中的嵌套资源使我们能够直接在我们的路由中记录父/子关系。

```
class Coffee < ApplicationRecord
  has_many :reviews
end class Review < ApplicationRecord
  belongs_to :coffee
end
```

这里我们有来自两个模型 User 和 Review 的父模型(Coffee)和子模型(Review)。嵌套路由是通过路由获取这些关系的另一种方式。这意味着我们的 URL 将看起来美观，而不是一串随机的数字和字符。

```
resources :coffees do
    resources :reviews, only: [:new, :index, :show]
end
```

此操作将为我们的咖啡创建路线，并且还会将我们的评论发送给评论管理员。嵌套路由还将为我们提供我前面提到的那些美观的 URL 路径。

```
coffee_reviews GET    /coffees/:coffee_id/reviews(.:format)                                                    reviews#indexnew_coffee_review GET    /coffees/:coffee_id/reviews/new(.:format)                                                reviews#newcoffee_review GET    /coffees/:coffee_id/reviews/:id(.:format)                                                reviews#showcoffees GET    /coffees(.:format)                                                                       coffees#index
```

因此，如果我们想查看特定咖啡的所有评论，URL 在您的浏览器中看起来会像`/coffees/1/reviews`。这个方便的方法也将创建路由助手，如`new_coffee_review`。

嵌套资源也可以有深度嵌套，如下所示:

```
resources :coffees do
    resources :reviews, only: [:new, :index, :show]
      resources :photosend
```

然而，做这些深度嵌套的资源是一个很好的方法，可以让你迷失在所有这些繁琐的路由助手和 URL 路径中。因此，为了保持良好的编程实践，最好避免深度嵌套路由。

通过嵌套资源，您可以利用这一强大的工具来帮助保持您的路线整洁、动态。

# ActiveRecord::查询方法

在进入 Flatiron 的 Rails mod 之前，我已经习惯了用传统的 Ruby 语法编写方法。直到这个项目，我才发现 ActiveRecord 查询方法的迷人世界。

关于编程，我最喜欢的事情之一是发现新的方法来做我以前已经学会如何做的事情。因此，当我发现这些新的查询方法可以编写类似于我用普通的旧 ruby 语法编写的代码时，我感到非常惊讶。ActiveRecord 查询方法启动命令并在数据库中执行它们，本质上是通过在数据库中执行来加速命令。

这里是我在做这个项目时学到的一些我最喜欢的查询方法。

***构建***

`build` 方法相当于对一个对象调用`.new`。就像 *new 一样，* build 不会自动将其保存到数据库中，所以您需要调用变量的 save 方法。

```
@review = @coffee.reviews.build
```

**通过找到 _ 或 _ 创建 _ 被**

`find_or_create_by`方法检查具有指定属性的记录是否存在。如果没有，则调用`create`。本质上，这是告诉我们的数据库找到某个东西，如果那个东西存在，然后继续，但是，如果数据库中没有记录，那么在数据库中为它创建一个新的条目。这里的方法真的很漂亮！

```
@user = User.find_or_create_by(email: user_email)
```

下面是一个例子，我在项目中使用这个查询方法为 omniauth 创建一个控制器操作。

有了这些新的查询方法，我觉得我好像给我的开发者腰带添加了一个新工具！我绝对建议您在这里看看《Rails guidelines》[,了解更多查询方法的优点！](https://guides.rubyonrails.org/active_record_querying.html)