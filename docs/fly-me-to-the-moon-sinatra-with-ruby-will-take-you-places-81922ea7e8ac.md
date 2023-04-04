# 带我飞向月球:辛纳特拉与红宝石将带你去的地方！

> 原文：<https://blog.devgenius.io/fly-me-to-the-moon-sinatra-with-ruby-will-take-you-places-81922ea7e8ac?source=collection_archive---------8----------------------->

读者朋友们，你们好！我回来了，带来了更多关于我为熨斗学校做的项目的报道。本文将讨论我使用 Ruby 和 Sinatra 创建全栈 web 应用程序的第一次尝试！

![](img/dae4f7484ebbe227ae1c47d7a15b591f.png)

由[耶稣·基特克](https://unsplash.com/@jesuskiteque?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

## 辛纳特拉？那不是歌手吗？

虽然你认为 Sinatra 是歌手并没有错，但是，我们将要讨论的 Sinatra 是在 Ruby 和 Rack 之上实现的 DSL(领域特定语言),类似于 Rails 的轻量级版本。Sinatra 旨在以最少的努力，简单而容易地开始创建 web 应用程序。

本质上，Sinatra 是 Ruby Gem，它大大加快了创建 web 应用程序的过程。它在处理嵌套目录的方法上非常有效。

## 什么是不爱？

我必须喊出一个特殊的 Sinatra 框架，它为创建快速 web 应用程序创建了基本的文件结构。 [Corneal](https://github.com/thebrianemory/corneal) 实际上是一个拯救生命的工具，它去除了所有与创建基本应用程序相关的初始繁琐工作。Corneal 将以这种方式建立文件结构:

```
Directory structure:├── config.ru
├── Gemfile
├── Gemfile.lock
├── Rakefile
├── README
├── app
│   ├── controllers
│   │   └── application_controller.rb
│   ├── models
│   └── views
│       ├── layout.erb
│       └── welcome.erb
├── config
│   ├── initializers
│   └── environment.rb
├── db
│   └── migrate
├── lib
│   └── .gitkeep
└── public
|   ├── images
|   ├── javascripts
|   └── stylesheets
|       └── main.css
└── spec
    ├── application_controller_spec.rb
    └── spec_helper.rb
```

我发现特别有帮助的是 Corneal 附带了一些默认的 CSS 样式，当你忙于在 MVC 模型中创建控制器时，可以很容易地看到你的项目。我发现这非常有帮助，当我建立数据库和模型时，可以很容易地看到我的代码是否如预期的那样工作。

角膜也有这个可怕的方法建立在创造容易耙迁移。

```
corneal [model/scaffold] NAME name:string age:integer
```

这个简单的命令做两件事。首先，它创建一个 rake 迁移文件。第二，它为你写表。从本质上来说，节省了你很多宝贵的时间。向...告别:

```
rake db:create_migration NAME=[migration_name]&&class CreateUsers < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.string :name
      t.age :integer

      t.timestamps null: false
    end
  end
end
```

所以，非常感谢布莱恩·埃默里发明了这个救生工具。

## 一些控制器动作呢？

在模型-视图-控制器(MVC)格式中，控制器可以被看作是服务员，从餐馆后面的厨师那里发出/返回订单，并从客户或视图那里接收/返回订单。这实际上说明了控制器对于一个正常运行的 web 应用程序是多么重要。当您创建 web 应用程序时，可以保证您将大部分时间花在这些控制器上。

在 Sinatra 中，控制器是用 Ruby 编写的，由“路由”组成，这些“路由”接受来自浏览器的请求(“获取该数据”、“发布该数据”)，通过使用模型运行基于这些请求的代码，然后呈现。供用户查看的 erb(视图)文件。此外，application_controller.rb 文件是您的程序从 Sinatra::Base 继承的地方，使所有这些令人敬畏的 Sinatra 方法对您的应用程序可用。

## 帮手呢？

说到 Sinatra 中的控制器，在你的 application_controller 中使用一个助手方法可以让你的 web 应用摆脱一些“代码味”或者方法中的冗余。作为一名开发人员，我在培训中真正领悟到的一点是，开发人员不喜欢重复自己，重复自己会让你的程序看起来混乱不堪。记得保持代码干爽(不要重复)！因此，创建助手方法确实允许我们作为开发人员编写一次代码，并且仍然能够在整个 web 应用程序中使用它。

```
helpers dodef logged_in?
    !!current_user
enddef current_user
    User.find_by(id: session[:user_id])
enddef authorized_to_edit?(post)
    post.user == current_user
endend
```

上面的 current_user 助手方法允许我们将当前的 user :id 与 current_user 方法相关联。这种方法在与 logged_in 配合使用时非常有用。方法，该方法将 current_user 的值转换为布尔值，该值将返回 true 或 false，以确定 current_user 当时是否登录。

如何在 posts_controller.rb 文件中使用它的示例:

```
get '/posts/new' doif logged_in?
    erb :'/posts/new'
else
    flash[:error] = "You must be logged in to create a review"
    redirect '/'
end
end
```

很自然，这允许我们使用 logged_in？方法来呈现/posts/new 表单。等于 true，使得只有登录的人才能创建新帖子！

你可能已经注意到了，

```
flash[:error] = "You must be logged in to create a review"
```

这是一个超级棒的宝石，让用户知道什么时候有问题，否则用户永远不会知道实际发生了什么。s[in tra-Flash](https://github.com/SFEley/sinatra-flash)的生命周期是一次刷新，这意味着一旦用户刷新页面，消息就会消失！这是另一个超级方便的东西，让程序更加动态！

## 判决结果？

Sinatra 是一个非常棒的 DSL，可以用最少的努力创建快速的网络应用程序！我真的很喜欢用 Sinatra 创作和学习。尽管我必须承认辛纳屈并不完美。最让我恼火的一件事是，当你创建你的路线时，顺序很重要！如果你没有按照辛纳特拉希望你拥有的方式来安排你的路线，你会得到奇怪的错误，或者像辛纳特拉喜欢说的“辛纳特拉不知道这首小曲。”除了那个奇怪的怪癖，辛纳特拉真的很有趣！