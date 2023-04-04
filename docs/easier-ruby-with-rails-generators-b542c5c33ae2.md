# 更简单的 Ruby with Rails 生成器

> 原文：<https://blog.devgenius.io/easier-ruby-with-rails-generators-b542c5c33ae2?source=collection_archive---------20----------------------->

当涉及到利用 Ruby on Rail 的任何一个方便的生成器时，一点点就可以走很长的路。这篇博文将详细介绍何时、用什么以及如何使用 common Rail 的发电机。首先，打开一个新的 Rail 项目，运行“bundle install”后，在终端中键入以下内容:

```
rails g
```

这将向您的终端输出一个命令列表，并作为如何继续的蓝图。“rails g”输出如下所示。

```
Please choose a generator below.Rails: assets channel controller generator helper integration_test jbuilder job mailer migration model resource scaffold scaffold_controller task
```

哇！有了这么多发电机选项，**我们该何去何从？**

![](img/8f9eae00c5c61d9aad4ba6cfd2c27a2c.png)

选择任何特定生成器的最佳方法应该与项目文件本身已经包含的内容相关。如果您需要的只是一个新的模型或控制器，那么就没有必要使用“rails g resource ”,它会生成这两个文件甚至更多。过多的混乱无疑会导致错误，所以在运行任何生成器之前都要记住 DRY 原则。

让我们从绝对应该避免的唯一生成器开始；除非你的目标是非常快速地证明概念，或者只是展示 Ruby on Rails 的强大功能。说到功率过大的 Rail 发电机，“rails g scaffold”独占鳌头。这个生成器将充实整个 Ruby on Rails web 应用程序，包括模型、控制器、视图、路径，以及其他一些通常不必要的文件。乍一看，这听起来很神奇，几乎像是一条无害的捷径。

然而，我劝你不要被愚弄。对于几乎每一个应用程序来说，“rails g scaffold”都应该谨慎使用，因为它留给开发人员的控制权很少，而且会产生过多的代码，这只会让您的项目陷入困境。但是，如果您需要从头开始生产一些模型、控制器和视图目录；那么我强烈推荐‘rails g resource’

rail 的发电机“资源”本质上是一个发电机，由四个依次运行的其他发电机组成。为了更好地了解“rails g resource”在幕后做些什么，让我们来分解一下它的各个部分。例如，如果我们要运行“rails g 模型”,我们将使用以下公式:

```
rails g model ModelName column_name:datatype column_name2:datatype2 --no-test-framework
```

也许我们希望为一个作者类生成一个模型，如下所示:

```
rails g model Author name:string genre:string --no-test-framework
```

该命令不仅会在我们的 Models 文件夹中生成一个“Author.rb”文件，还会创建以下迁移文件:

```
# db/migrate/20150301576893_create_authors.rb
class CreateAuthors < ActiveRecord::Migration[5.1]
  def change
    create_table :authors do |t|
      t.string :name
      t.string :genre
      t.timestamps
    end
  end
end
```

顺便提一下，在我们的 generate 命令的末尾加上'— no-test-framework '只是告诉 Rails 不要生成任何 rspec/test 文件。在某些情况下，这些代码可能会派上用场，但在大多数情况下，它们是不必要的代码块，因此最好省略掉。

使用模型生成命令，您还可以使用以下公式生成一个单独的迁移文件，而无需创建新的模型类:

```
rails g migration add_column_name_to_table_name column_name:datatype --no-test-framework
```

假设我们想在作者表中添加一个简历，如下所示:

```
rails g migration add_bio_to_authors bio:text --no-test-framework
```

运行此命令后，将创建以下迁移文件:

```
# db/migrate/20120301776896_add_bio_to_authors.rb
class AddBioToAuthors < ActiveRecord::Migration[5.1]
  def change
    add_column :authors, :bio, :text
  end
end
```

这个框架对于添加多个列和删除列是一样的。如果我们想添加多列，公式是:

```
rails g migration add_more_to_authors first_publication_date:integer number_of_books_sold:integer --no-test-framework
```

相反，删除列就像下面这样简单:

```
rails g migration remove_number_of_books_sold_from_authors number_of_books_sold:integer --no-test-framework
```

这将我们带到“rails g resource ”,它将创建一个新的模型、数据库迁移文件、控制器和一个空的视图文件夹。该命令遵循与模型生成器相同的公式:

```
rails g resource ModelName column_name:datatype --no-test-framework
```

资源生成器通常应该在还没有创建控制器、迁移或模型时使用。本质上，当你从头开始一个新的 rails 应用时，你应该使用“rails g resource”。它给你空间来控制你的代码，而不会创建太多不必要的文件。请在这里阅读更多关于 Rails 生成器的信息。