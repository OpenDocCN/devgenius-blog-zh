# Docker 模板:Ruby on Rails with PostgreSQL

> 原文：<https://blog.devgenius.io/docker-template-ruby-on-rails-with-postgresql-db2cc2d31a3f?source=collection_archive---------7----------------------->

# 这是什么？

这是一个项目，作为一个模板，允许您将 Ruby on Rails(可以选择包含 React frontend)应用程序放入一个文件夹中，配置一些设置，然后使用 Docker 快速拥有一个带有自己包含的环境的容器化应用程序！从那里，应用程序可以部署在 VPS(虚拟专用服务器)上，几乎没有环境配置。使用 Docker 还可以更容易地管理在同一 VPS 上运行的多个应用程序，这使得当您部署了多个应用程序时非常经济高效。这个模板旨在使 Docker 易于安装和使用！

# 我如何使用它？

要使用这个模板，请访问这个项目的 [GitHub 页面](https://github.com/joshua-holmes/docker-template-ruby-on-rails)，将其克隆到您的本地机器上，然后按照这个博客或 GitHub 上的说明进行操作！两组指令是相同的。

# 说明:极速版！

*注意:如果您在应用程序中使用 React Router Dom，请访问下面的部分，因为有一些额外的部署步骤。这些步骤不是 Docker 特定的。不管部署方法如何，步骤都是一样的。*

1.  复制并粘贴你的 Ruby on Rails 应用程序(或者创建一个新的)到`ror/`中，这样`ror/`就是你的 RoR 应用程序的根目录。反应前端是可选的。
2.  在项目的本地根目录下运行`bin/dockerize -p <PORT_NUMBER>`，其中`<PORT_NUMBER>`是您希望 Docker 容器运行的端口号。当提示覆盖时，键入`y`或按回车键。此步骤将保留数据库名称，不会覆盖它们，但会覆盖其他设置。如果 ror/config/database.yml 中有除数据库名称之外的自定义设置，请按`n`并遵循黄色指示。下面是一个例子:`bin/dockerize -p 3001`
3.  将你的项目上传到 GitHub，然后下载到你的服务器上。现在，在项目的服务器 repo 中，运行`bin/server-setup -p '<DB_PASSWORD>' -m <MASTERKEY>`。万能钥匙可以在您的本地项目 repo: `ror/config/master.key`中找到。该密码是您的数据库密码，您正在这里创建一个新密码。**注意**密码用单引号括起来。那是为了防止 bash 试图解释特殊字符，比如'【T9]'。*可选地*，您也可以用-u 标志为您的数据库添加一个用户名。以下是整个命令的示例:

```
bin/server-setup -u username -p 'YourNewSecureP@$$w0rD' -m fakeMasterKey1234abcd5678efgh
```

# 说明:手动版(有利于学习)

1.  复制并粘贴你的 React on Rails 应用程序(或者创建一个新的)到`ror/`中，这样`ror/`就是你的 RoR 应用程序的根目录。
2.  在`.docker/`中，将`example.env`文件复制到同一个目录下，并重命名为`.env`。您不需要在本地存储库中更改任何变量，但是您需要在服务器存储库中配置该文件。
3.  将`.docker/example.database.yml`的内容复制并粘贴到`ror/config/database.yml`中，替换里面的内容，注意您已经为项目设置的任何数据库配置(如果有的话)。如果其中一个数据库包含需要保留在本地计算机上进行开发的数据，请保留原始文件中的数据库名称。
4.  在这个项目的根目录中打开`docker-compose.yml`文件，只改变第一个端口号，指定你希望你的应用程序在哪个端口上运行。例如，如果您想在端口 4000 上运行应用程序，请将其更改为`"4000:3000"`。不要改变第二个数字。
5.  *如果您没有 React 前端，请跳过这一步！*如果你想在本地测试你的 dockerization，在你的项目根目录下打开你的终端，输入`bin/build-frontend`。然后运行`sudo docker compose up --build`。请记住，加载完成后，终端中显示的端口是内部端口号，而不是外部端口号。这意味着，如果您在上一步中将端口设置为 4000，终端仍将显示端口 3000。在这种情况下，您可以忽略终端，转到[http://0 . 0 . 0:4000](http://0.0.0.0:4000/)来访问端口 4000，这样您的应用程序就应该启动了。
6.  **您的应用程序现已完成对接！！**如果您从未使用过 Docker Compose，请访问下面的“操作说明”部分，了解如何在您的服务器上安装该应用程序并运行它。
7.  你现在可以把你的应用上传到 GitHub，然后下载到你的服务器上。在您的服务器上，转到您的服务器项目 repo 中的`.docker/`目录并重复上面的步骤 2。这一次，选择一个安全的密码，并将正确的变量设置为您选择的密码。确保密码用单引号括起来，如示例文件所示。
8.  现在，在`ror/config/master.key`中从您的本地 repo 复制主密钥，并将该密钥粘贴到您的服务器 repo 上的`ror/config/master.key`、*或*中，取消对`.docker/.env`中的`RAILS_MASTER_KEY`变量的注释，并将该行中的密钥粘贴到您的服务器 repo 上。

# 操作的

**您现在可以运行您的应用程序了！！**确保 Docker 和 Docker Compose 安装在你的服务器上，然后**从你的项目根目录运行** `**sudo docker compose up --build -d**`在你的服务器上构建镜像并运行你的应用。若要查看您的应用程序，**请访问**T5。要删除你的应用，运行`sudo docker compose down`。如果您的应用正在运行，并且您已经对其进行了更改，并且想要更新服务器上的应用，我已经包含了一个有用的脚本，它将自动运行`git pull`，使您的应用脱机，重建 Docker 映像，并使其重新联机，所有这些都在一个命令中完成！**只需从项目的根目录运行** `**bin/update-app**`。

当你更新你的应用程序时，确保在将它上传到 GitHub 并更新你的服务器之前，在你的本地回购*上运行`bin/build-frontend`。RoR 的静态前端文件通过 git 与你的项目的其余部分一起，这样做的原因是因为你的服务器不太可能有构建前端所需的`npm`包管理器。*

***警告:*** *当你运行你的 app 时，命令行会告诉你它正在 3000 端口上运行，即使这不是真的。只是不要上当，访问您之前指定的端口。它指的是内部 Docker 端口，而不是您访问应用程序时使用的外部端口。*

***另一个警告:*** *您可能需要在您的服务器防火墙上启用端口，然后才能通过在浏览器中直接键入 IP 地址和端口来查看应用程序。如果您使用 Nginx 或 Apache，则没有必要更改防火墙设置，因为所有流量通常都通过端口 80 或 443 进入，然后被重定向到您应用程序的内部端口。*

# 接下来去哪里

如果您是 VPS 服务器部署的新手，并且想知道如何连接您的域名以及更多，下面是一些后续步骤:

1.  从廉价域名或谷歌域名购买域名。通常这大约是每年 10-12 美元。
2.  进入 DNS 设置，创建一个指向你的服务器的公共 IP 地址的 A 记录(IP 应该由你的服务器平台提供商提供。通常在您登录后，它会出现在您的门户网站中。).
3.  安装一个像 Nginx 这样的路由软件，将请求转发到应用程序运行的适当端口。
4.  在您的服务器上安装“让我们加密”来安装 SSL 证书，这样您的服务器就可以接受 HTTPS，这样您的浏览器就会认为您的站点是安全的。

# 说明

这是对上述步骤的解释，对你的教育意义大于阅读的必要性。

## 检查手动版本中的步骤

1.  `ror/`目录代表 Ruby on Rails 或 React on Rails。你的 React 应用程序应该在 ruby 应用程序目录下的`client/`文件夹中。
2.  的。docker/。env 文件包含在 Docker 容器中设置的环境变量。这些变量被加载到数据库的容器和应用程序的容器中。通过共享相同的环境变量，它们可以共享有用的信息，比如数据库的密码、活动用户和 Rails 环境。主密钥也存储在这里。这个主密钥用于加密数据，不应该是公开的，这就是 git 忽略它的原因，也是为什么您必须手动将主密钥加载到服务器的 repo 上，即使您的本地 repo 已经有了它。当主密钥作为环境变量存储在`.docker/.env`文件中，或者存储在`ror/config/`目录中自己的`master.key`文件中时，Rails 能够查看主密钥。这两种选择都适用于 Rails，因此如何存储密钥完全取决于您。如果你需要生成一个新的，删除`ror/config/master.key`和`ror/config/credentials.yml.enc`，然后在你的 Rails 目录下运行`rails credentials:edit`命令。这将生成一个新的`master.key`文件，但是也会重新创建凭证文件。因为凭证文件是通过 git 上传的(这是一件好事)，所以请确保在发出该命令后更新了所有其他的 repos。git 忽略了`master.key`文件(这也是一件好事)，所以不要公开暴露那个文件。
3.  `ror/config/database.yml`文件是数据库的配置文件。为了让它与 Docker 一起工作，需要做一些修改:为了安全起见，需要指定一个主机，需要输入用户名和密码。任何包含在`<%= %>`中的东西实际上都是 ruby 代码。用户名和密码使用`ENV["VAR"]`而不是`ENV.fetch("VAR")`来获取环境变量，因为如果没有找到变量，第一个选项返回`nil`，而第二个选项抛出一个错误。通过使用用户名和密码的第一个选项，它允许用户名和密码是可选的，因为这些环境变量将在 Docker 容器中建立(通过`.docker/.env`文件)，而不太可能在您的计算机上建立。本质上，在本地运行时不需要密码，但通过 Docker 在服务器上运行时需要密码。类似的故事与主机键。如果没有找到`POSTGRES_PASSWORD`环境变量，程序会假设您在本地机器上，不需要指定主机。但是当数据库和应用程序通过不同的容器运行时，需要一个主机名。
4.  `docker-compose.yml`文件是`docker compose`获取构建和运行应用程序的指令的地方。这个文件最重要的部分可能是端口号。要调整运行应用程序的端口号，您必须更改这两个数字中的第一个。因此输入`"4000:3000"`将在端口 4000 上运行应用程序。第二个数字不能更改的原因是因为该数字指定了内部端口号。这是 Rails 在 Docker 容器内部技术上运行的端口号，但是随后 Docker 将这个内部端口转发到一个外部端口，这个外部端口可以在容器外部访问。这个外部端口号是第一个数字(在我上面的例子中是端口 4000)。在这个文件中，您还可以看到类似于`env_file`键的东西，这是引用`.env`文件的地方，它包含容器的环境变量。此外，`dockerfile`键值对指向 Dockerfile 文件，其中包含如何构建应用程序的指令。如果你好奇的话，可以打开看看。
5.  在`bin/`文件夹中有更多的自定义脚本。请随意用文本编辑器打开它们，看看里面有什么！请记住，这些是 bash 脚本，不是 ruby 脚本。
6.  一旦应用程序被 dockerized，它可以被带到服务器进行部署。

# 免费知识！(又名提示和常见问题)

## 使用 React 路由器 Dom？

这会将所有未知的`GET`请求转发给`fallback#index`，后者会返回您的 React 应用。如果没有这些步骤，在除主页之外的任何页面上刷新页面都会导致 404 错误。这个错误不是 Docker 特有的，而是在使用 React 路由器 Dom 时 React on Rails 特有的。

1.  在`ror/app/controllers/`中创建一个名为`fallback_controller.rb`的控制器，并将这段代码粘贴到:

```
class FallbackController < ActionController::Base
  def index
    render file: 'public/index.html'
  end
end
```

2.打开在`ror/config/routes.rb`中找到的 routes 文件，将这一行- `get "*path", to: "fallback#index", constraints: ->(req) { !req.xhr? && req.format.html? }` -添加到主代码块的底部，看起来像这样:

```
Rails.application.routes.draw do
  ...
  get "*path", to: "fallback#index", constraints: ->(req) { !req.xhr? && req.format.html? }
end
```

## Docker 提示和命令

*提示*

1.  Docker 命令应该总是用`sudo`运行，因为 Docker 守护进程是用管理员权限运行的。有一种方法可以绕过它，但对我来说，使用`sudo`似乎更容易。如果这是你想去的方向，你可以随意搜索一下。
2.  Docker Compose 的一些安装带有别名，允许您使用`docker-compose`命令而不是`docker compose`。其他人没有，所以我在这个教学集中以及在我的定制 bash 脚本中使用了`docker compose`。
3.  Docker 图像就像是你的应用程序及其环境的截图。这个映像甚至包括它自己的 Linux 发行版和文件系统。但既然是截图，就不能修改原文。如果做了改动，必须重新截图。
4.  Docker 容器运行 Docker 映像。它允许代码存储在内存中，所以你的应用程序可以是动态的。它类似于虚拟机，但环境不是存储在虚拟机中，而是存储在映像中。容器的性能比虚拟机高得多。
5.  Docker 卷是一种在容器上的指定文件夹和本地计算机上的文件夹之间同步数据的方式。之所以有必要，是因为请记住，数据不能持久存储到 Docker 映像中，因为映像不能被修改。由于是这种情况，docker 容器将数据存储在内存中，并将其与本地机器上的文件夹同步，因此当 Docker 关闭时，可以在下次运行连接的容器时访问数据。这些卷的最佳用途是将数据保存在数据库中。在这个项目中，Docker 容器中的数据库与我们的项目文件夹`.docker/volumes/database/`同步。如果将来需要访问这些数据，您可以在那里找到它们。
6.  Docker 网络将两个或多个 Docker 容器联系在一起，并允许它们彼此交互。在这个项目包含的示例应用程序中，PostgreSQL 数据库运行在一个容器中，而 RoR 应用程序运行在一个单独的容器中。他们能够通过 Docker 网络相互通信。
7.  Docker Compose 是一个解决方案，允许您在 YAML 文件中配置 Docker 设置，而不是在每次运行容器时在命令中键入这些设置。例如，在指定了端口、环境变量、网络等等之后，运行我的容器的命令可能是这样的:`sudo docker -p 3000:3000 -e POSTGRES_USER=rails -e POSTGRES_PASSWORD=1234 -e RAILS_ENVIRONMENT=production --network host ...`等等，等等。通过将我的设置放在 YAML 文件中，Docker Compose 可以读取该文件并自动应用这些设置。

*命令*

*注意:当使用* `*docker compose*` *命令时，确保你在你想要操作的项目的根目录下。Docker Compose 使用根目录中的* `*docker-compose.yml*` *文件来知道将命令应用于哪些容器。*

1.  `sudo docker compose build`会用你的 app 建立一个 docker 镜像，保存到你电脑上一个神秘的目录里。图像*永远不会改变*，所以每当你更新你的应用程序，你必须建立一个新的图像来取代旧的。这个命令可以用来做那件事。每次构建映像时，它都会自动替换旧的映像。
2.  `sudo docker compose up`将从您所在的目录中获取构建的映像，并在`docker-compose.yml`文件中指定的端口上运行它！如果您添加了`--build`标志，它将首先构建一个新的映像，然后运行该映像。它的基本功能类似于`sudo docker compose build`紧跟着`sudo docker compose up`。如果您添加了`-d`标志，它会在后台运行您的应用程序，因此您无需在应用程序运行的整个过程中盯着终端。在我的服务器上更新我的应用程序时，我最喜欢使用的命令是`sudo docker compose up --build -d`,因为它会构建我的应用程序，然后在后台运行它。
3.  `sudo docker compose down`将使你的应用脱机。
4.  `sudo docker ps`将向您显示所有在线并监听您的一个端口的容器。`sudo docker ps -a`将显示所有在线*或离线*的容器。
5.  `sudo docker images`将显示保存在您系统上的所有图像。
6.  `sudo docker stop <CONTAINER_ID>`使用容器的 ID 使容器脱机。
7.  `sudo docker rm <CONTAINER_ID>`从本地存储中删除一个容器。如果您使用`su`登录到 root，然后输入您的密码，您可以使用`docker rm $(docker ps -aq)`删除您系统上的所有容器。注意`sudo`未使用。这是因为作为 root 用户，您不需要`sudo`来提升您的权限，因为 root 用户已经拥有最高权限。
8.  `sudo docker rmi <IMAGE_ID>`从本地存储器中删除图像。*在删除图像之前，确保容器不依赖于该图像。*如果您使用`su`登录 root，然后输入密码，您可以使用`docker rmi $(docker images -aq)`删除系统上的所有图像。注意这里也没有使用`sudo`。

## 一般提示

*   如果您更改了`.docker/.env`中的数据库用户名或密码，容器化的应用程序将无法连接到`.docker/volumes/database`中的数据库。如果在开发过程中出现这种情况，并且数据库内容是一次性的，那么只需使用`sudo rm -fr .docker/volumes/database`从项目根目录中删除数据库文件夹。这将删除数据库和其中存储的数据，允许 Docker 用您的新用户名和密码重建数据库。
*   如果找不到`ror/config/master.key`，运行`ror/`内的`rails credentials:edit`生成一个新的。
*   如果需要新的主密钥，删除`ror/config/master.key`和`ror/config/credentials.yml.enc`，然后在`ror/`中运行`rails credentials:edit`生成新的主密钥。