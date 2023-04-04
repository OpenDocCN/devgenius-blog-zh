# 用 RSpec 测试 Rails API

> 原文：<https://blog.devgenius.io/testing-a-rails-api-with-rspec-82dedc9f15df?source=collection_archive---------1----------------------->

![](img/652f01fd898864099a36a5b38c94d32f.png)

昆顿·库切在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

在本教程中，我将展示如何使用最流行的 Ruby 测试框架 RSpec 来测试 Ruby on Rails 纯 API 应用程序的 GET、POST 和 DELETE 方法。

首先，我将编写会失败的测试，然后编写控制器的代码来解决这些测试。换句话说，我将使用 [TDD](https://en.wikipedia.org/wiki/Test-driven_development#:~:text=Test%2Ddriven%20development%20(TDD),software%20against%20all%20test%20cases.) 方法。

我们来编码吧！

# 第一步。创建一个 Rails API

一开始，从头开始创建一个简单的 Rails API 专用应用程序。

```
rails new post-api --api -T
cd post-api
```

旗帜`--api`继承了`ActionController::API`的`ApplicationController`而不是`ActionController::Base`。这意味着 Action Controller 提供了浏览器应用程序使用的模块，并跳过了生成我们不需要的视图、助手和资源。

我们将使用 *RSpec* 而不是 *MiniTest* 。命令`-T`跳过`Minitest::Unit`文件和文件夹的生成。

# 第二步。安装 gems

将`rspec-rails`添加到应用程序`Gemfile`的`:development`和`:test`组中。另外，在`:test`环境中添加一个`factory_bot_rails` gem 用于设置测试数据对象，添加`faker`用于生成假数据。

```
group :development, :test do
  gem 'rspec-rails'
endgroup :test do
    gem 'factory_bot_rails'
    gem 'faker'
end
```

安装。

```
bundle
```

# 第三步。设置测试环境

要完成 RSpec 安装，请使用下面的命令。

```
rails generate rspec:install
```

该命令生成一个包含`rails_helper.rb`和`spec_helper.rb`文件的*规格*文件夹。

接下来，用`spec/factories.rb`中的假数据创建 *Post* 工厂。

```
FactoryBot.define do
  factory :post do
    title { Faker::Lorem.sentence }
    content { Faker::Lorem.paragraph }
  end
end
```

为了最小化代码，我将编写一个`json`方法。它只是解析响应体字符串。还是写在`spec/support/api_helpers.rb`吧。

```
module ApiHelpers
  def json
    JSON.parse(response.body)
  end
end
```

最后要做的是要求所有的支持文件，并在`spec/rails_helper.rb`中包含 *ApiHelpers* 模块。

```
Dir[Rails.root.join("spec/support/**/*.rb")].each { |f| require f }RSpec.configure do |config|
  # ...
  config.include ApiHelpers
end
```

# 第四步。API 规格

我将使用 [REST API 版本控制](https://www.freecodecamp.org/news/how-to-version-a-rest-api/#:~:text=so%20let's%20recap%3A-,API%20versioning%20is%20the%20practice%20of%20transparently%20managing%20changes%20to,effective%20API%20change%20management%20principles.)。当在 API 中识别出需要的变更时，它**帮助我们更快地迭代。这是最佳实践。**

## 得到

创建`spec/requests/get_posts_spec.rb`并插入以下代码:

开始时，我使用 FactoryBot 方法`create_list`创建 10 个帖子，然后调用一个 HTTP GET。接下来，我检查创建的帖子的大小和 HTTP 状态代码。

## 邮政

接下来，创建`spec/requests/post_posts_spec.rb`并插入它:

我在`let!`方法内部写了一个 post 实例。通常，如果我需要使用下面的实例，我会使用`let!`，否则使用`before`。

## 删除

而最后一个就是创建`spec/requests/delete_posts_spec.rb`并插入下面的代码。

进行测试。

```
rspec
```

测试肯定会失败，让我们在下一步解决。

# 第五步。通过测试

通过测试的第一步是**生成一个模型*后*** 。

```
rails g model Post title:string content:text --no-test-framework
rails db:migrate
```

`--no-test-framework`标志不创建模型测试。如果您需要模型测试，您可以移除该标志。

下一步是**验证模型**。

```
class Post < ApplicationRecord
  validates :title, :content, presence: true
end
```

仅对属性的存在进行验证。

**建造*岗位*控制器**以满足所有测试要求。

创建`app/controllers/api/v1/posts_controller.rb`文件并插入以下代码:

更新`routes.rb`。

```
Rails.application.routes.draw do
  namespace :api do
    namespace :v1 do
      resources :posts
    end
  end 
end
```

再次进行测试。

```
rspec
```

有用！🥳:这是一个非常简单的例子，它将为你的 API 应用程序编写新的测试提供一个起点。祝你好运！