# 在 Express 项目中使用带有 PostgreSQL 的 Sequelize ORM

> 原文：<https://blog.devgenius.io/use-sequelize-orm-with-postgresql-in-your-express-project-3c277b289522?source=collection_archive---------1----------------------->

![](img/1cd178703d3fffaa13913721f6c76c56.png)

使用原始 SQL 查询可能非常具有挑战性，而且非常耗时。在本文中，我们将介绍如何在 Express 项目中建立一个名为 [Sequelize](https://sequelize.org/) 的对象关系映射(ORM)框架。我们将使用 PostgreSQL 来连接和存储我们的数据。

我们将使用 NPM 作为包装经理，但可以自由替换 NPM 纱。

# 初始化项目并安装依赖项

我们需要做的第一件事是初始化 Express.js 项目。

```
npm init -y
```

安装快速

```
npm install express
```

安装 dotenv

```
npm install dotenv
```

将 Nodemon 作为开发依赖项安装

```
npm install nodemon --save-dev
```

安装序列

```
npm install sequelize sequelize-cli pg pg-hstore
```

# 快速设置

## 设置基本结构

我们将首先打开我们的 **package.json** 文件，并在 scripts 标签中添加以下内容，这样我们就可以使用 *nodemon* 来启动我们的应用程序，这样每当我们更改文件中的某些内容时，我们都可以获得即时反馈。

```
"scripts": {
    "dev": "nodemon app.js"
},
```

现在让我们创建我们的 **app.js** 文件，它将包含以下内容。

```
'use strict'

require('dotenv').config()

var createError = require('http-errors')
var express = require('express')
var **path** = require('path')

var **app** = express()

**app**.use(express.json())
**app**.use(express.urlencoded({ extended: false }))

// catch 404 and forward to error handler
**app**.use(function (req, res, next) {
    next(createError(404))
})

// error handler
**app**.use(function (err, req, res, next) {
    // 404 Not Found.
    if (err.status === 404) {
      return res
        .status(404)
    }

    // 500 Internal Server Error (in production, all other errors send this response).
    if (req.app.get('env') !== 'development') {
      return res
        .status(500)
    }

    // set locals, only providing error in development
    res.**locals**.message = err.message
    res.**locals**.error = req.app.get('env') === 'development' ? err : {}

    // render the error page
    res.status(err.status || 500)
})

// Start listening.
**app**.listen(3000, async () => {
    **console**.log('Server started on http://localhost:3000')
    **console**.log('Press Ctrl-C to terminate...')
})

module.exports = **app**
```

现在，我们可以开始尝试在我们的机器上看到它的工作。

```
npm run dev
```

如果您看到类似这样的内容，那么一切都正常运行了。

![](img/ffbc92934c8d33a81f421f11f489cbeb.png)

npm 运行开发

现在，我们没有任何路由、控制器或服务，但我们会在 Sequelize 设置完成后解决这个问题，这样我们就可以通过 HTTP 请求从数据库中获取数据。

# 在本地设置 PostgreSQL 数据库

我们需要建立并运行一个开发数据库，这样我们就可以向其中添加数据库模式并填充一些数据。你可以随意设置你的 PostgreSQL 数据库，但是对于这篇文章，我们将使用 Docker，因为它非常容易设置。

如果我们简单地运行下面的命令，它将设置数据库。显然，首先要确保在你的电脑上安装 Docker。

```
docker run --name my_db \
    -e POSTGRESQL_PORT=5432 \
    -e POSTGRESQL_DB=my_db \
    -e POSTGRESQL_USER=postgres \
    -e POSTGRES_PASSWORD=test1234 \
    -d postgres
```

![](img/a1144b172aa8c394322efaa37d7175d1.png)

运行命令的输出

将容器名称更改为您想要的任何名称(在第一行)，数据库名称和凭证。我们稍后在设置配置时会用到这些，所以请记住这些。

现在我们只需要创建数据库 **my_db** 。为了登录到服务器，在这个例子中，我使用了由 *Jetbrains* 开发的 *DataGrip* (这是付费的，但是你可以免费试用)。右键点击服务器，按**新建**和**数据库**，写入 **my_db** 。

![](img/914fd1abc37fd3c2c505180bc1a4e5ae.png)

# 设置顺序配置

现在我们需要设置配置，当我们运行 Sequelize 命令时，它将知道文件放在哪里。

首先创建一个**。sequelizerc** 文件，代码如下。

```
const path = require('path')

module.exports = {
  config: path.resolve('./database/config', 'config.js'),
  'models-path': path.resolve('./database/models'),
  'seeders-path': path.resolve('./database/seeders'),
  'migrations-path': path.resolve('./database/migrations'),
}
```

现在我们需要创建名为**数据库**的目录和三个子目录:**配置**、**模型**、**种子**和**迁移**。

在 config 目录中，我们将放入下面的 **config.js** 文件。

```
require('dotenv').config()

module.exports = {
    development: {
        url: **process**.env.DEV_DATABASE_URL,
        dialect: 'postgres',
    },
    test: {
        url: **process**.env.TEST_DATABASE_URL,
        dialect: 'postgres',
    },
    production: {
        url: **process**.env.DATABASE_URL,
        dialect: 'postgres',
    },
}
```

我们对 url 使用环境变量。这样的 URL 如下所示。

```
DEV_DATABASE_URL=postgres://postgres:test1234@127.0.0.1:5432/my_db
NODE_ENV=development
```

能够在本地创建环境变量。创造一个**。env** 文件，并将代码放在上面。

# 设置顺序模型

现在我们准备创建我们的模型，为了创建一个模型。我们只需运行以下命令。

```
sequelize model:generate --name Post --attributes title:string,message:text
```

请注意，逗号后面不能有空格，否则模型的生成将不起作用。

![](img/6c50861bb6e552582f9bda25450b9a24.png)

在我们运行该命令后，我们现在可以看到模型已经生成，还有一个迁移文件，它定义了到 PostgreSQL 的迁移数据。

注意，我们会自动获得两个字段 **createdAt** 和 **updatedAt** 。这些可以通过改变模型来指定不同的列名来改变。

```
createdAt: {type: DataTypes.DATE, allowNull: false, field: ‘created_at’}, 
updatedAt: {type: DataTypes.DATE, allowNull: false, field: ‘updated_at’},
```

请注意，我们添加了一个新的字段值 created_at 和 updated_at，因此我们使用小写字母和下划线。看起来更好。

我们还可以通过在 Post 模型的选项中的 modelName 下添加这个表名来更改表名以使用 Post 而不是 Post。

```
tableName: ‘post’
```

它应该是什么样子的例子。

```
Post.init({
  title: DataTypes.STRING,
  message: DataTypes.TEXT,
  likes: DataTypes.INTEGER
}, {
  sequelize,
  modelName: 'Post',
  tableName: 'posts'
});
```

我们还必须在数据库/模型目录中设置一个包含以下内容的 **index.js** 文件。

```
'use strict';

require('dotenv').config()

const fs = require('fs');
const path = require('path');
const Sequelize = require('sequelize');
const basename = path.basename(__filename);
const db = {};

let sequelize

if (process.env.NODE_ENV === 'development') {
  sequelize = new Sequelize(process.env.DEV_DATABASE_URL)
} else if (process.env.NODE_ENV === 'test') {
  sequelize = new Sequelize(process.env.TEST_DATABASE_URL)
} else {
  sequelize = new Sequelize(process.env.DATABASE_URL)
}

fs
  .readdirSync(__dirname)
  .filter(file => {
    return (file.indexOf('.') !== 0) && (file !== basename) && (file.slice(-3) === '.js');
  })
  .forEach(file => {
    const model = require(path.join(__dirname, file))(sequelize, Sequelize.DataTypes);
    db[model.name] = model;
  });

Object.keys(db).forEach(modelName => {
  if (db[modelName].associate) {
    db[modelName].associate(db);
  }
});

db.sequelize = sequelize;
db.Sequelize = Sequelize;

module.exports = db;
```

# 初始迁移

现在我们需要迁移我们的新模式。运行以下命令进行迁移。

```
sequelize db:migrate
```

![](img/7a13c4b8fde6506cc03ab9318d218e5f.png)

数据库顺序化:迁移

现在，我们在终端中看到我们已经成功迁移。让我们在一个 GUI 数据库工具中检查数据库，以便可视化它的外观。

![](img/e7713ca54885b25bccbd83b3f1e1cf32.png)

帖子表现已添加

![](img/f3f74356684a309b621a8b6d0e7d019f.png)

帖子表的列

我们已经成功地将新模式添加到数据库中。

# 生成顺序迁移文件

当我们创建一个模型时，正如我们在上一节中看到的，它会自动生成一个迁移文件。在某些情况下，当我们进行迁移，然后我们希望根据新的模型定义用新的列更改数据库模式或删除现有的列时，我们可以使用新的迁移文件来更改模式。

为了生成新的迁移文件，我们运行下面的命令。

```
npx sequelize-cli migration:create --name adding_new_field_to_post
```

这里我们使用 sequelize-cli 工具创建一个名为**的迁移文件，它将描述迁移的操作。**

为此迁移添加以下代码。

```
'use strict';

module.exports = {
  async up (queryInterface, Sequelize) {
    await queryInterface.addColumn('Posts', 'likes', {
      type: Sequelize.INTEGER,
      allowNull: true,
    })
  },

  async down (queryInterface, Sequelize) {
    await queryInterface.removeColumn('Posts', 'likes')
  }
};
```

在第一个异步函数 **up** 中，我们用我们想要更改的内容编写我们的更改命令，在第二个异步函数 **down** 中，我们编写我们的恢复命令，如果某些东西不起作用，我们就进行回滚。

在这种情况下，我们添加一个新的列**like**，类型为整数，并且我们允许空值。

重要的是要知道，在编写迁移文件时，我们也需要更新模型，这样我们就不会在代码中使用错误的模型定义。

所以现在让我们更新模型。

```
'use strict';
const {
  Model
} = require('sequelize');
module.exports = (sequelize, DataTypes) => {
  class Post extends Model {
    static associate(models) {
      // define association here
    }
  }
  Post.init({
    title: DataTypes.STRING,
    message: DataTypes.TEXT,
    likes: DataTypes.INTEGER
  }, {
    sequelize,
    modelName: 'Post',
  });
  return Post;
};
```

# 将序列模式迁移到 PostgreSQL 数据库

当我们完成迁移文件和模型文件时，我们就准备好迁移了。要迁移，我们运行命令。

```
sequelize db:migrate
```

![](img/c576629f00995277fa0a6e6a1bc775cf.png)

添加了名为“喜欢”的新专栏。

现在我们增加了新的“喜欢”栏。

# 生成用于测试的种子数据

种子数据对于测试用途非常有用，所以我们用一些样本数据填充了数据库。生成种子数据并不难。让我们生成一个将用作种子数据的文件。

```
sequelize seed:generate --name Post
```

这里我们指定我们将生成模型文章的种子数据。

在文件中添加以下内容。

```
'use strict';

module.exports = {
  up: (queryInterface, Sequelize) =>
      queryInterface.bulkInsert(
          'Posts',
          [
            {
              id: 1,
              title: 'Sample post 1',
              message: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
              likes: 1,
              createdAt: new **Date**(0).toISOString(),
              updatedAt: new **Date**(0).toISOString(),
            },
            {
              id: 2,
              title: 'Sample post 2',
              message: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
              likes: 2, 
              createdAt: new **Date**(0).toISOString(),
              updatedAt: new **Date**(0).toISOString(),
            },
            {
              id: 3,
              title: 'Sample post 3',
              message: 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. ',
              likes: 3,
              createdAt: new **Date**(0).toISOString(),
              updatedAt: new **Date**(0).toISOString(),
            },
          ],
          {}
      ),
  down: (queryInterface, Sequelize) =>
      queryInterface.bulkDelete('Posts', null, {}),
}
```

现在让我们在脚本列表中的 package.json 中添加更多的定义。

```
“db:migrate”: “npx sequelize-cli db:migrate”,“db:reset”: “npx sequelize-cli db:drop && npx sequelize-cli db:create && npx sequelize-cli db:migrate && npx sequelize-cli db:seed:all — seeders-path ./database/seeders”,
```

现在，我们可以使用命令轻松地迁移数据库。

```
npm run db:migrate
```

然后完全重置数据库，并用新的种子填充数据库。

```
npm run db:reset
```

运行 db:reset 时，记得在 GUI 中断开数据库，否则它会阻塞事务。

![](img/7e6484bea5b34d2341c7a889e8c54487.png)

从种子填充的数据库

现在，来自播种器的数据的填充已经成功完成。太好了！

# 创建 PostgreSQL 配置文件

现在我们需要设置 PostgreSQL 配置文件，以便在我们的服务中使用它。创建一个名为**的目录配置**并在其中创建 **postgresql.js** 文件。

```
/**
 * PostgreSQL configuration.
 */

'use strict'

require('dotenv').config()

const { Sequelize } = require('sequelize')

let sequelize

if (**process**.env.NODE_ENV === 'development') {
    sequelize = new Sequelize(**process**.env.DEV_DATABASE_URL)
} else if (**process**.env.NODE_ENV === 'test') {
    sequelize = new Sequelize(**process**.env.TEST_DATABASE_URL)
} else {
    sequelize = new Sequelize(**process**.env.DATABASE_URL)
}

try {
    sequelize.authenticate()
        .then(r =>
            **console**.log('INFO: Connection to database has been established successfully.')
        );
} catch (err) {
    **console**.log('ERROR: Unable to connect to the database - ', err)
}

const **db** = {};

**db**.sequelize = sequelize
**db**.Sequelize = Sequelize

module.exports = **db**
```

# 创建路线、控制器和服务

现在，当我们完成 Sequelize 的基础设置后，是时候创建一个简单的路由、控制器和服务，通过 Express.js 作为 RESTful API 从数据库中获取一些数据。

首先在项目的根目录下创建三个目录。

```
mkdir routes
mkdir controllers
mkdir services
```

现在让我们在 routes 目录下创建 **postRoute.js** 文件。

```
/**
 * Post routes.
 */

'use strict'

const **express** = require('express')
const **router** = **express**.Router()

const controller = require('../controllers/postController')

**router**.get('/posts', controller.getAll)

module.exports = **router**
```

然后在控制器目录中添加 **postController.js** 文件。

```
/**
 * Post Controller
 */

'use strict'

let db = require("../configs/postgresql"),
    sequelize = db.sequelize,
    Sequelize = db.Sequelize

const **postService** = require('../services/postService')

const **postController** = {}

/**
 * Get all posts
 *
 * **@param** {object} req - Express request object.
 * **@param** {object} res - Express response object.
 */
**postController**.getAll = async (req, res) => {
    **postService**.getAll()
        .then(data => {
            res.json(data)
        })
        .catch(err => {
            res.status(404).send({
                message: err.message || 'Some error occurred while retrieving data.'
            })
        })
}

module.exports = **postController**
```

最后，在服务目录中创建 **postService.js** 文件。

```
/**
 * Post Service
 */

'use strict'

const db = require('../configs/postgresql')
const models = require("../database/models")

const **postService** = {}

/**
 * Get all posts
 */
**postService**.getAll = async () => {
    return models.Post.findAndCountAll({
        order: [
            ['id', 'ASC'],
        ],
    })
}

module.exports = **postService**
```

我们需要做的最后一件事是在 app.js 文件中定义它，这样我们就可以加载路线。在顶部添加以下一行:

```
var postRouter = require('./routes/postRouter')
```

以及这些之后的下一行:

```
**app**.use(**express**.**json**())
**app**.use(**express**.**urlencoded**({ extended: false }))**app**.use('/api/v1/', postRouter)
```

在这里，我们指定 posts 路由应该在/api/v1/下。因此，让我们尝试对/api/v1/posts 进行 API 调用，看看我们得到了什么内容(我们应该已经有了来自 seeder 的示例数据)。不要忘记通过运行以下命令来启动服务器:

```
npm run dev
```

现在使用 cURL 或 Postman 或任何其他 HTTP 客户端来尝试一下。我们将在这里测试 cURL:

```
curl [http://localhost:3000/api/v1/posts](http://localhost:3000/api/v1/posts)
```

我们得到了输出:

```
{“count”:3,”rows”:[{“id”:1,”title”:”Sample post 1",”message”:”Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. “,”likes”:1,”createdAt”:”1970–01–01T00:00:00.000Z”,”updatedAt”:”1970–01–01T00:00:00.000Z”},{“id”:2,”title”:”Sample post 2",”message”:”Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. “,”likes”:2,”createdAt”:”1970–01–01T00:00:00.000Z”,”updatedAt”:”1970–01–01T00:00:00.000Z”},{“id”:3,”title”:”Sample post 3",”message”:”Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. “,”likes”:3,”createdAt”:”1970–01–01T00:00:00.000Z”,”updatedAt”:”1970–01–01T00:00:00.000Z”}]}
```

![](img/0d2c9b5b4b88866b539b1f4483fdd732.png)

curl[http://localhost:3000/API/v1/posts](http://localhost:3000/api/v1/posts)

有用！太好了，我们从数据库中得到数据。我们还可以在控制台中看到，它显示了所使用的 SQL 查询。这有利于调试。

![](img/e0c6e05a4a6b1e29215b7bc777cb3ec6.png)

对数据库的 SQL 查询

# 摘要

所以在这篇文章中，你现在应该知道如何以一种基本的方式使用序列。还有很多更小的事情，你可以用不同的方式来做，以及如何用 Mocha 和 Chai 来设置运行单元测试。在我的下一篇文章中，我将详细介绍这些步骤。

# 与我联系

*   推特—[https://twitter.com/mjovanc](https://twitter.com/mjovanc)
*   GitHub—【https://github.com/mjovanc 
*   领英—【https://www.linkedin.com/in/marcuscvjeticanin/ 

# 参考

*   模型基础知识[https://sequelize . org/docs/V6/core-concepts/model-Basics/#:~:text = Models % 20 is % 20 the % 20 essence % 20 of，(and%20their%20data%20types)。](https://sequelize.org/docs/v6/core-concepts/model-basics/#:~:text=Models%20are%20the%20essence%20of,(and%20their%20data%20types).)
*   迁徙[https://sequelize.org/docs/v6/other-topics/migrations/](https://sequelize.org/docs/v6/other-topics/migrations/)
*   模型查询[https://sequelize . org/docs/V6/core-concepts/model-query-basics/](https://sequelize.org/docs/v6/core-concepts/model-querying-basics/)