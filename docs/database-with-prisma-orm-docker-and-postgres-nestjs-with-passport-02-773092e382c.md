# 带有 Prisma ORM、Docker 和 Postgres 的数据库—带有 Passport #02 的 NestJs

> 原文：<https://blog.devgenius.io/database-with-prisma-orm-docker-and-postgres-nestjs-with-passport-02-773092e382c?source=collection_archive---------0----------------------->

![](img/a58f4a875bf1e32888b663494e88b6c3.png)

在上一篇文章中，从一个空白的配置开始，理解 NestJs 如何处理路由、控制器和服务。看到了设置 Fastify 来优化我们的应用程序是多么容易。

现在，将建立数据库和 ORM 来交互和存储我们的数据。我们将 PostgreSQL 用于数据库，使用 docker 创建应用程序的默认容器，将 Prisma 用于 ORM，因为这是目前与数据库交互的最佳 Orm。

现在我们有了自己的应用程序，让我们把它容器化。

首先，在项目的根目录中创建以下文件:

*   这个文件将负责导入 Docker 映像，将它们分成开发和生产环境，复制我们所有的文件，并安装依赖项。
*   `docker-compose.yml` -该文件将负责定义我们的容器、应用程序其他服务所需的图像、存储卷、环境变量等。

打开`Dockerfile`并添加

```
# DockerfileFROM node:alpine As developmentWORKDIR /usr/src/appCOPY package*.json ./
COPY prisma ./prisma/RUN yarn add glob rimrafRUN yarn — only=developmentCOPY . .RUN yarn buildFROM node:alpine as productionARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}WORKDIR /usr/src/appCOPY package*.json ./
COPY prisma ./prisma/RUN yarn add glob rimrafRUN yarn — only=productionCOPY . .COPY — from=development /usr/src/app/dist ./distCMD [“node”, “dist/main”]
```

打开“docker-compose.yml”文件并添加以下代码

```
# docker-compose.ymlversion: “3.7”services: main:
  container_name: main
  build:
   context: .
   target: development
  volumes:
   — .:/usr/src/app
   — /usr/src/app/node_modules
  ports:
   — 3000:3000
  command: yarn start:dev
  env_file:
   — .env
  networks:
   — api
  depends_on:
   — postgres postgres:
   image: postgres:13
   container_name: postgres
   networks:
    — api
   env_file:
    — .env
   ports:
    — 5432:5432
   volumes:
    — pgdata:/var/lib/postgresql/data
networks:
 api:
volumes:
 pgdata:
```

创建一个` . env '文件并添加 PostgreSQL 凭据

```
// .env# PostgreSQL
POSTGRES_USER=nestAuth
POSTGRES_PASSWORD=nestAuth
POSTGRES_DB=nestAuth
```

默认情况下，Fastify 只监听“localhost 127.0.0.1”接口。为了从其他主机访问我们的应用程序，我们需要在“main.ts”中添加“0.0.0.0”

```
// src/main.tsawait app.listen(3000, "0.0.0.0");
```

太棒了，我们已经把我们的文件归档了，然后我们去测试吧。在终端中运行进行开发

```
docker-compose up
```

![](img/1fe0e3e6a756be81cac4e1363d948516.png)

我们的应用程序正在运行

# 棱柱形

[Prisma](https://prisma.io) 是一个开源 ORM，它被用作编写普通 SQL 的替代方案，或者使用另一个数据库访问工具，如 SQL 查询构建器(如 knex.js)或 ORMs(如 TypeORM 和 Sequelize)。

开始安装 Prisma CLI 作为开发

```
yarn add Prisma -D
```

作为最佳实践，通过在 CLI 前面加上“npx”来本地调用 CLI，使用“init”命令创建您的初始 Prisma 设置

```
npx prisma init
```

该命令使用以下内容创建一个新的 Prisma 目录

- `schema.prisma `:指定您的数据库连接并包含数据库模式
- `.env `:一个 dotenv 文件，通常用于在一组环境变量中存储您的数据库凭证

默认情况下，您的数据库连接设置为“postgresql”

```
// prisma/schema.prismagenerator client {
  provider = "prisma-client-js"
}datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

我们的连接类型如何正确，我们将在`. env `中设置` DATABASE_URL '

```
# .envDATABASE_URL="postgresql://nestAuth:nestAuth@postgres:5432/nestAuth"
```

记得在 Github 中创建资源库之前，在` . gitignore '中添加` . env '并创建` . env.example '

生成 Prisma 客户端需要“schema.prisma”文件。“复制 prisma。/prisma/`复制整个 prisma 目录，以防您也需要迁移。

```
# DockerfileFROM node:alpine As developmentWORKDIR /usr/src/appCOPY package*.json ./# Here Prisma folder to the container
COPY prisma ./prisma/RUN yarn add glob rimrafRUN yarn --only=developmentCOPY . .RUN yarn buildFROM node:alpine as productionARG NODE_ENV=production
ENV NODE_ENV=${NODE_ENV}WORKDIR /usr/src/appCOPY package*.json ./# Here Prisma folder to the container
COPY prisma ./prisma/RUN yarn add glob rimrafRUN yarn --only=productionCOPY . .COPY --from=development /usr/src/app/dist ./distCMD ["node", "dist/main"]
```

# 第一个模型

现在，为了测试连接，我们将在“schema . prisma”insert 中创建一个“用户”模型

```
// prisma/schema.prismamodel User {
  id        Int            [@id](http://twitter.com/id) [@default](http://twitter.com/default)(autoincrement())
  email     String         [@unique](http://twitter.com/unique)
  name      String
  password  String
  createdAt DateTime       [@default](http://twitter.com/default)(now())
  updatedAt DateTime       [@updatedAt](http://twitter.com/updatedAt)
}
```

有了模型，就可以生成 SQL 迁移文件，并对数据库运行它们。在这里，我使用“migrate dev”在开发模式下运行，并为迁移设置“init”名称

```
npx prisma migrate dev --name init
```

![](img/a99fe7a39855d1151bdfb6cadff47330.png)

好消息是，我们的配置正在工作，现在我们的数据库与我们的应用程序同步👏

# 设置 Prisma

我们希望抽象出 Prisma 客户端 API 用于服务中的数据库查询。因此，让我们创建一个新的“PrismaService ”,它负责实例化和“PrismaClient”连接到您的数据库。

在“src”文件夹中创建一个“prisma.service.ts”

```
// src/prisma.service.tsimport { INestApplication, Injectable, OnModuleInit } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import { PrismaClient } from "[@prisma/client](http://twitter.com/prisma/client)";[@Injectable](http://twitter.com/Injectable)()
export class PrismaService extends PrismaClient implements OnModuleInit {
  async onModuleInit() {
    await this.$connect();
  }async enableShutdownHooks(app: INestApplication) {
    this.$on("beforeExit", async () => {
      await app.close();
    });
  }
}
```

# 初制动

现在我们可以编写一个用户服务来进行数据库调用。因此，NestJs CLI 有一个命令‘nest g’来生成服务、控制器、策略和其他结构。现在，我们跑

```
nest g service users
```

在开始创建服务之前，我们需要生成 Prisma 模型类型，我们可以用这个来生成

```
npx prisma generate
```

在“src”文件夹中，该命令创建了一个包含“users.service.ts”和文件 test“users . service . spec . ts”的“users”文件夹

- `user `:使用 Prisma 接口` Prisma . user where unique input ` forget user by unique columns
-` create user `:使用接口` Prisma.userCreateInput `中的数据创建新用户，该接口在模型需要创建新寄存器时自动获取字段

在“createUser”中，我们需要加密用户密码，因此让我们为此创建一个提供程序，在“src”文件夹中创建一个“providers”文件夹，并创建一个“password.ts”文件

```
// src/providers/password.tsimport { Injectable } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import * as bcrypt from "bcrypt";const SALT_OR_ROUNDS = 10;[@Injectable](http://twitter.com/Injectable)()
export class PasswordProvider {
  async hashPassword(password: string): Promise<string> {
    return bcrypt.hashSync(password, SALT_OR_ROUNDS);
  }async comparePassword(password: string, hash: string): Promise<boolean> {
    return bcrypt.compareSync(password, hash);
  }
}
```

该类有两个方法，“hashPassword”和“comparePassword ”,用于使用 brcypt 加密和比较密码。在“UsersService”类中，我们需要在构造函数中添加提供程序“PasswordProvider ”,以便在方法中使用。

```
// src/users/users.service.tsimport { HttpException, HttpStatus, Injectable } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import { PrismaService } from "../prisma.service";
import { User, Prisma } from "[@prisma/client](http://twitter.com/prisma/client)";
import { PasswordProvider } from "src/providers/password";[@Injectable](http://twitter.com/Injectable)()
export class UsersService {
  constructor(
    private prisma: PrismaService,
    private passwordProvider: PasswordProvider
  ) {}async user(
    userWhereUniqueInput: Prisma.UserWhereUniqueInput
  ): Promise<User | null> {
    const user = await this.prisma.user.findUnique({
      where: userWhereUniqueInput,
    });delete user.password;return user;
  }async createUser(data: Prisma.UserCreateInput): Promise<User> {
    const userExists = await this.prisma.user.findUnique({
      where: { email: data.email },
    });if (userExists) {
      throw new HttpException("User already exists", HttpStatus.CONFLICT);
    }const passwordHashed = await this.passwordProvider.hashPassword(
      data.password
    );const user = await this.prisma.user.create({
      data: {
        ...data,
        password: passwordHashed,
      },
    });delete user.password;return user;
  }
}
```

创建服务后，让我们创建一个用于路由的控制器

```
nest g controller users
```

这个命令创建了“users.controller.ts”和“src/users”中的测试文件，所以让我们在控制器中创建两个函数

*   ` signUpUser `:运行` createUser `服务并从其返回数据
    - `getUserProfile `:获取路由发送的用户 id 并运行` User `服务来查找他们

```
// src/users/users.controller.tsimport { Body, Controller, Get, Param, Post } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import { User } from "[@prisma/client](http://twitter.com/prisma/client)";
import { UsersService } from "./users.service";// Set prefix route for this group. Ex.: for get profile /users/8126321
[@Controller](http://twitter.com/Controller)("users")
export class UsersController {
  constructor(private readonly usersService: UsersService) {}
  // Create user -> POST /users
  [@Post](http://twitter.com/Post)()
  async signupUser(
    [@Body](http://twitter.com/Body)() userData: { name: string; email: string; password: string }
  ): Promise<User> {
    return this.usersService.createUser(userData);
  }// Get user Profile -> GET /users/:id
  [@Get](http://twitter.com/Get)("/:id")
  async profile([@Param](http://twitter.com/Param)("id") id: number): Promise<User> {
    return this.usersService.user({ id: Number(id) });
  }
}
```

在“users.module.ts”文件中，我们需要添加提供者、导出和控制器数组。

```
// src/users/users.module.tsimport { Module } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import { PrismaService } from "src/prisma.service";
import { PasswordProvider } from "src/providers/password";
import { UsersController } from "./users.controller";
import { UsersService } from "./users.service";[@Module](http://twitter.com/Module)({
  providers: [PasswordProvider, UsersService, PrismaService],
  exports: [UsersService],
  controllers: [UsersController],
})
export class UsersModule {}
```

并将“UsersModule”传递给“AppModule”以供使用。

```
//src/app.module.tsimport { Module } from "[@nestjs/common](http://twitter.com/nestjs/common)";
import { AppController } from "./app.controller";
import { PrismaService } from "./prisma.service";
import { UsersModule } from "./users/users.module";
import { UsersService } from "./users/users.service";
import { PasswordProvider } from "./providers/password";[@Module](http://twitter.com/Module)({
  imports: [UsersModule],
  controllers: [AppController],
  providers: [PrismaService, UsersService, PasswordProvider],
})
export class AppModule {}
```

# 让我们测试一下

现在让我们的码头集装箱

```
docker-compose up
```

![](img/1fe0e3e6a756be81cac4e1363d948516.png)

就是这样！应用程序正在运行👏

因此，在 Postman 中，让我们尝试使用“创建用户”和“获取配置文件”路径

```
curl --location --request POST '[http://0.0.0.0:3000/users'](http://0.0.0.0:3000/users') \
--header 'Content-Type: application/json' \
--data-raw '{
    "email": "[test@e3x.com](mailto:test@e3x.com)",
    "name": "Gabriel Menezes",
    "password": "123123"
}'
```

![](img/0e43eedecff3e2da4082f1b7c774d79c.png)

```
curl --location --request GET '[http://0.0.0.0:3000/users/37'](http://0.0.0.0:3000/users/37')
```

![](img/56fab9335b33424e944c6856a152e738.png)

# 直到下次

这就是我们在本文中要讨论的全部内容，我们对应用程序进行 dockerize，设置 Prisma，并创建两条路线。在本系列的下一篇文章中，我们将创建和定义我们的身份验证提供者，以便在我们的应用程序中进行身份验证。

感谢您的阅读！

https://github.com/mnzsss/nest-auth-explained

# 参考

- [用 Docker 为后端开发建立一个 NestJS 项目](https://dev.to/erezhod/setting-up-a-nestjs-project-with-docker-for-back-end-development-30lg)
- [用 Prisma 和 PostgreSQL 对一个 NestJS 应用进行 Docker 化](https://notiz.dev/blog/dockerizing-nestjs-with-prisma-and-postgresql)