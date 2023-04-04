# 如何将 Google Sign In(SSO)with device 添加到 Ruby on Rails 7 应用程序中

> 原文：<https://blog.devgenius.io/how-to-add-google-sign-in-sso-with-devise-to-a-ruby-on-rails-7-app-6d8c5ef7641b?source=collection_archive---------0----------------------->

![](img/6a27f766c6867810fa8457368357754b.png)

# 开始之前

Youtube 上 [Deanin 的所有功劳](https://www.youtube.com/c/deanin)。这篇文章基于他的教程，并做了一些我自己的调整。关注他的帐户，访问他丰富的现代 Rails 资源！

本文涵盖了使用 Devise 安装 Google SSO 的最低要求。它并没有涵盖其他好的实践。例如，在运行 install Devise 之后，它会提示您在应用程序布局中包含 flash 消息——我不会讨论这些方面。

确保您安装了 Rails 7 和兼容的 Ruby 版本。以下是我在 MacOS Monterey 上运行的版本:

```
$ rails --version && ruby --version
Rails 7.0.3
ruby 3.1.2p20 (2022-04-12 revision 4491bb740a) [arm64-darwin21]
```

# 初始 gem 安装和配置

将以下宝石添加到您的`Gemfile`中:

```
gem "dotenv-rails"
gem "devise"
gem "omniauth"
gem "omniauth-google-oauth2"
gem "omniauth-rails_csrf_protection"
```

安装这些宝石:

```
$ bundle install
```

安装设备:

```
$ rails g devise:install
```

在`config/initializers/devise.rb`中，添加以下配置并传递您的凭证(我们将在后面介绍如何获取这些凭证):

```
config.omniauth :google_oauth2, ENV[‘GOOGLE_OAUTH_CLIENT_ID’], ENV[‘GOOGLE_OAUTH_CLIENT_SECRET’]
```

# 设置设备的用户模型

创建用户模型:

```
$ rails g devise user
```

暂停运行迁移。我们将首先添加一些调整。

将`omniauth`模块添加到`app/models/user.rb`:

```
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
  :recoverable, :rememberable, :validatable,
  **# for Google OmniAuth
  :omniauthable, omniauth_providers: [:google_oauth2]**
end
```

转到`db/migrate/`目录中的迁移文件(我的名为`20220709132706_devise_create_users.rb`，但你的名字会稍有不同)。向用户数据库添加其他列:

```
class DeviseCreateUsers < ActiveRecord::Migration[7.0]
  def change
    create_table :users do |t| **## Custom columns**
      **t.string :full_name
      t.string :uid
      t.string :avatar_url
      t.string :provider** ## Database authenticatable
      t.string :email,              null: false, default: ""         
      t.string :encrypted_password, null: false, default: "" ...
```

迁移数据库:

```
$ rails db:migrate
```

# 设置设备的控制器

为设备用户创建控制器:

```
$ rails g devise:controllers users
```

在`routes.rb`的设备配置中指定这些控制器:

```
Rails.application.routes.draw do
  devise_for :users**, controllers: {
    omniauth_callbacks: 'users/omniauth_callbacks',
    sessions: 'users/sessions',
    registrations: 'users/registrations'**
  } ...
```

## 在设计控制器中设置方法

在`app/controllers/users/registrations_controller.rb`文件中:

```
class Users::RegistrationsController < Devise::RegistrationsController **def update_resource(resource, params)
    if resource.provider == 'google_oauth2'
      params.delete('current_password')
      resource.password = params['password']
      resource.update_without_password(params)
    else
      resource.update_with_password(params)
    end
  end**end
```

在`app/controllers/users/sessions_controller.rb`文件中:

```
class Users::SessionsController < Devise::SessionsController
 **def after_sign_out_path_for(_resource_or_scope)
    new_user_session_path
  end** **def after_sign_in_path_for(resource_or_scope)
    stored_location_for(resource_or_scope) || root_path
  end**
end
```

在`app/controllers/users/omniauth_callbacks_controller.rb`文件中:

```
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController **def google_oauth2
    user = User.from_omniauth(auth)** **if user.present?
      sign_out_all_scopes
      flash[:success] = t 'devise.omniauth_callbacks.success', kind: 'Google'
      sign_in_and_redirect user, event: :authentication
    else
      flash[:alert] =
        t 'devise.omniauth_callbacks.failure', kind: 'Google', reason: "#{auth.info.email} is not authorized."
      redirect_to new_user_session_path
    end
  end** **protected** **def after_omniauth_failure_path_for(_scope)
    new_user_session_path
  end** **def after_sign_in_path_for(resource_or_scope)
    stored_location_for(resource_or_scope) || root_path
  end** **private** **def auth** [**@**](http://twitter.com/auth)**auth ||= request.env['omniauth.auth']
  end**
end
```

看到上面文件中调用的`from_omniauth`方法了吗？我们将在`app/models/user.rb`的用户模型中定义它。该方法获取从身份验证请求中获得的用户数据，并在用户模型中创建新记录(如果不存在的话):

```
class User < ApplicationRecord
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         # for Google OmniAuth
         :omniauthable, omniauth_providers: [:google_oauth2]
  **
  def self.from_omniauth(auth)
    where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
      user.email = auth.info.email
      user.password = Devise.friendly_token[0, 20]
      user.full_name = auth.info.name # assuming the user model has a name
      user.avatar_url = auth.info.image # assuming the user model has an image
    end
  end**
end
```

# 获取 Google 登录凭据

进入 [Google Cloud Console 网站](https://console.cloud.google.com/)，新建一个项目，然后选择/打开新建的项目。

导航到 OAuth 同意屏幕页面。选择`External`用户类型。在下一个窗口中，输入您的应用程序的名称、用户支持电子邮件和开发者联系信息。对于我的业余爱好项目，我在两个电子邮件栏都输入了我的个人电子邮件地址。点击至末尾，完成 OAuth 同意屏幕的设置。

导航到“凭据”页面。点击`+ Create Credentials`并选择`OAuth client ID`。选择`Web application`作为“应用类型”。输入你的应用名称。在“授权重定向 URIs”下，添加以下 URI:

```
[http://localhost:3000/users/auth/google_oauth2/callback](http://localhost:3000/users/auth/google_oauth2/callback)
```

如果/当您已将应用程序部署到生产环境中时，请返回此页面，将您的应用程序的域添加为另一个具有相同格式的 URI:

```
[http://**yourwebsite.com**/users/auth/google_oauth2/callback](http://localhost:3000/users/auth/google_oauth2/callback)
```

瞧。您应该会看到一个弹出窗口，显示您的 Google 客户端 ID 和客户端密码。

在应用程序的根目录下创建一个名为`.env`的文件，并将你的谷歌客户端 ID 和密码放入其中:

```
GOOGLE_OAUTH_CLIENT_ID=Your client id
GOOGLE_OAUTH_CLIENT_SECRET=Your client secret
```

将`/.env`添加到您的`.gitignore`文件中，这样您就不会将这些凭证暴露给坏人。

# 设置 Google 登录按钮

创建设备视图:

```
$ rails g devise:views
```

将 Google 登录按钮图像文件下载到您的`app/assets/images/`目录中。我推荐下载[谷歌的登录品牌指南页面](https://developers.google.com/identity/branding-guidelines)上提供的文件——我最喜欢的是这个，因为它在亮暗两种模式下都能工作:`/google_signin_buttons/web/1x/btn_google_signin_dark_normal_web.png`

![](img/08785f4f76d63c70875bf6b837004a9b.png)

更新`app/views/devise/shared/_links.html.erb`中的登录链接，使用 Google OmniAuth 对其进行正确配置，并将其变成一个可点击的按钮:

```
...<%- if devise_mapping.omniauthable? %>
  <%- resource_class.omniauth_providers.each do |provider| %>
 **<%= form_for "Login",
      url: omniauth_authorize_path(resource_name, provider),
      method: :post,
      data: {turbo: "false"} do |f| %>
        <%= f.submit "Login", type: "image", src: url_for("/assets/btn_google_signin_dark_normal_web.png") %>
    <% end %>** 
  <% end %>
<% end %>...
```

*更新上面的图像路径(* `*/assets/btn_google_signin_dark_normal_web.png*` *)，以匹配您最终选择的按钮图像的路径。*

最后，将下面的代码暂时放在你的主页视图上，或者你想显示这些链接的任何地方，以测试你新添加的 Google 登录/注销功能:

```
<% if current_user %>
  <h2><%= current_user.email %></h2>
    <%= image_tag(current_user.avatar_url) %>
    <%= link_to "Edit Account", edit_user_registration_path %>
    <%= button_to "Logout", destroy_user_session_path, data: {turbo: "false"}, method: :delete %>
  <% else %>
    <%= link_to "Login", new_user_session_path %>
  <% end %>
```