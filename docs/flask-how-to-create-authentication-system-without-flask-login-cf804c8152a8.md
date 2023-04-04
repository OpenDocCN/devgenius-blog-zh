# Flask:如何在没有 Flask-Login 的情况下创建认证系统

> 原文：<https://blog.devgenius.io/flask-how-to-create-authentication-system-without-flask-login-cf804c8152a8?source=collection_archive---------6----------------------->

这是我通过发现如何在不使用 *Flask-Login* 的情况下创建一个体面的认证系统的经验？

![](img/8f5635092614be5387a79a533c71b041.png)

当我使用*烧瓶*后端*和*反应*前端创建一个应用程序时，我遇到了这个问题。我似乎无法让 *Flask-Login* 很好地与 *React* 前端一起工作，因此最终我实现了一种不同的方式来完成这项任务。*

在这篇文章中，我将带你了解我实现认证系统的方法，使用 *JSON Web 令牌*而不是 *Flask-Login 完全模块化。*

你可以在我的 [Github](https://github.com/Kwsswart/flask_jwt_authentication_blueprint) 里找到源代码。

# **问题**

我在 *Flask* 和*React*中构建了一个应用程序，其中我使用 *Flask* 作为一个完全独立的 *API* ，而 *React* 服务于前端。

然而，我没有预见到当前端不是由 *Flask 服务器服务时，将认证系统与 *Flask-Login* 集成是多么困难。*

虽然对于任何普通的 *Flask* 应用程序来说， *Flask-Login* 都是一个惊人的扩展，但是在 *Flask* 后端中似乎没有一个简单的方法来利用它，这个后端不提供它的模板，而是被称为 *API* 。

在做了大量的研究和绞尽脑汁之后，我找不到一个令我满意的解决方案，最后，我决定放弃集成这个令人惊奇的扩展的想法，而是想办法在没有它的情况下重新创建这种类型的身份验证。

## 这意味着什么

简而言之，我需要找到一种方法来重现 *Flask-Login* 的共同特征，主要是:

1.  找到让用户登录的方法。
2.  找到让用户注销的方法。
3.  找到创建用户会话的方法。
4.  找到一种方法来检查用户会话，以便在未经身份验证的情况下关闭某些路由。

# 解决方案

当我开始试图弄清楚我将如何做这件事时，我决定最好从简单地建立一个标准的*应用程序工厂*开始，并以蓝图的形式创建解决方案，这样它可以完全模块化和可重用。

我已经做了一些研究，并决定最好使用 *JSON Web 令牌*来实现这个结果，因为 *API* 将通过 *JSON* 和 *Flask* 返回所有信息，并对这种形式的认证进行了很大的扩展。

我遵循的过程如下，但是如果你不熟悉 [*烧瓶蓝图*](https://flask.palletsprojects.com/en/2.0.x/blueprints/) 我建议你在继续之前看一下[这个教程](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-xv-a-better-application-structure)。

## 设置基础应用程序工厂

我要做的第一件事是设置应用程序的文件结构。

这是使用蓝图的 flask 应用程序的基本文件结构。整个 *auth/* 目录将包含蓝图组件。

```
authentication_example/
    app/
        auth/
            __init__.py
            helpers.py
            routes.py
            models.py
        __init__.py
    api.py
    config.py
```

**设置好文件结构后，我需要创建一个虚拟环境并激活该虚拟环境。**

```
python -m venv venv
source venv/bin/activate 
venv/Scripts/activate // Windows command prompt
```

**进入环境后，我需要安装依赖项和包。**

```
pip install flask flask-sqlalchemy flask-cors flask-migrate flask-jwt-extended python-dotenv bcrypt
```

使用这些不同依赖关系的原因是:

**Flask-SQLAlchemy** —这是一个扩展，允许我们将 SQLAlchemy 用作我们的 *ORM。*

**Flask-Migrate** —这是处理所有数据库迁移的扩展。为了了解更多，这个扩展的创建者有这个[教程](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database)。

这是处理[跨来源资源共享](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)的扩展，这是使跨来源 *AJAX* 成为可能的简单方法。

这是一个扩展，它将在我们的 Flask 应用程序中添加处理 JSON Web 令牌的支持。

**Python-dotenv** —这是一个包，它允许我们将所有敏感信息保存在一个单独的。env 文件并将它们导入到应用程序中。

**bcrypt** —这是处理密码安全的包

现在我已经安装了所有的依赖项，我可以开始设置应用程序了。

我想做的第一件事是在 config.py 中创建一个 Config 对象，它可以将所有需要的变量导入到应用程序中。

```
***authentication_example/config.py*** import osfrom dotenv import load_dotenv basedir = os.path.abspath(os.path.dirname(__file__))load_dotenv(os.path.join(basedir, '.env'))class Config(object): SECRET_KEY = os.environ.get('SECRET_KEY') or 'you-will-never-guess' SQLALCHEMY_DATABASE_URI = os.environ.get('DATABASE_URL') or \ "sqlite:///" + os.path.join(basedir, "app_database.db") JWT_SECRET_KEY = os.environ.get('JWT_SECRET_KEY') or "super-secret-key" JWT_BLACKLIST_ENABLED = True JWT_BLACKLIST_TOKEN_CHECKS = ["access", "refresh"]
```

有了这个我决定成立 [**应用工厂**](https://flask.palletsprojects.com/en/2.0.x/patterns/appfactories/) **。**

我在三个单独的文件中做到了这一点: *api.py，app/__init__。py、*和 *app/auth/models.py*

```
***authentication_example/app/__init__.py*** import osfrom flask import Flaskfrom flask_cors import CORSfrom flask_migrate import Migratefrom flask_jwt_extended import JWTManagerfrom flask_sqlalchemy import SQLAlchemyfrom config import Configcors = CORS()db = SQLAlchemy()migrate = Migrate()jwt = JWTManager()def create_app(config_class=Config): app = Flask(__name__) app.config.from_object(Config) db.init_app(app) cors.init_app(app) migrate.init_app(app, db) jwt.init_app(app) return appfrom app.auth import models
```

*create_app()* 函数是一个工厂函数，当被调用时，它会创建一个 *Flask* 实例，并使用之前的配置初始化所有的 *flask 扩展*，然后返回应用程序。

*模型*被导入到文件的底部，以防止循环依赖。

```
 ***authentication_example/api.py*** from app import create_app, dbfrom app.auth.models import *from flask import Flaskapp = create_app()@app.shell_context_processordef make_shell_context(): return { 'db': db, 'Users': Users, }if __name__ == '__main__': app.run(debug=True)
```

在这个文件中，我调用了 *create_app()* 函数，但我也设置了 shell 上下文，这非常有用，因为它允许您在测试期间只需运行以下命令就可以与数据库和应用程序进行交互:

```
flask shell
```

该模型是数据库中表格的表示，以便 *ORM* 可以在 *Flask 中轻松使用它。*

```
***authentication_example/app/auth/models.py*** import osfrom app import dbclass Users(db.Model): id = db.Column('user_id', db.Integer, primary_key=True) username = db.Column(db.String(24)) pwd = db.Column(db.String(64)) def save(self):
        db.session.add(self) db.session.commit() def __init__(self, username, pwd): self.username = username self.pwd = pwd def __repr__(self): return "<User: Username - {}; password - {};>".format(self.username, self.pwd)
```

在这一部分，我只是简单地建立了一个认证系统需要的基本的*用户*模型。记住，一旦完成，您将需要像在[教程](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-iv-database)中所做的那样处理数据库迁移。

```
flask db init
flask db migrate -m "Users table"
flask db upgrade
```

## 创建身份验证蓝图

既然我已经设置好了应用程序并准备好工作，我必须注册蓝图，以便应用程序可以导入它和我需要实现的所有组件。

这个蓝图基本上由路线、模型和助手功能组成。

这是我更喜欢使用蓝图的方式，这样我就可以分离路由逻辑、数据库逻辑和我认为需要的任何其他逻辑。

我要做的第一件事是登记蓝图。

这是我在两个 *__init__ 里做的。py* 文件。

```
***authentication_example/app/auth/__init__.py*** from flask import Blueprint bp = Blueprint('auth', __name__) from app.auth import routes
```

在这个文件中，我简单地创建了一个标签为 *auth 的*蓝图*的实例。*

```
***authentication_example/app/__init__.py***import osfrom flask import Flaskfrom flask_cors import CORSfrom flask_migrate import Migratefrom flask_jwt_extended import JWTManagerfrom flask_sqlalchemy import SQLAlchemyfrom config import Configcors = CORS()db = SQLAlchemy()migrate = Migrate()jwt = JWTManager()def create_app(config_class=Config): app = Flask(__name__) app.config.from_object(Config) db.init_app(app) cors.init_app(app) migrate.init_app(app, db) jwt.init_app(app) **from app.auth import bp as auth_bp** **app.register_blueprint(auth_bp)** return appfrom app.auth import models
```

添加的两行以粗体显示，我正在注册蓝图，因为应用程序是在应用程序工厂中创建的。

一旦我注册了蓝图，我就可以自由地开始认证系统的工作了。

要在我们的应用程序中实现 [JSON Web 令牌](https://jwt.io/introduction)我需要做的第一件事是在我们的数据库中创建一个表来存储列入黑名单的令牌。

```
***authentication_example/app/auth/models.py***...class InvalidToken(db.Model): __tablename__ = "invalid_tokens" id = db.Column(db.Integer, primary_key=True) jti = db.Column(db.String) def save(self): db.session.add(self) db.session.commit() @classmethod def is_invalid(cls, jti): """ Determine whether the jti key is on the blocklist return bool""" query = cls.query.filter_by(jti=jti).first() return bool(query)
```

完成后，我必须记得再次迁移数据库:

```
flask db init
flask db migrate -m "Invalid Tokens table"
flask db upgrade
```

现在数据库已经准备好了，我可以创建助手函数了。

所有这些本质上都是为了将每个功能的路径与逻辑分开，这也有助于避免通过单独的路径重复代码。

```
***authentication_example/app/auth/helpers.py***from app import dbfrom app.auth.models import Usersfrom bcrypt import hashpw, gensalt, checkpwfrom base64 import b64encodefrom hashlib import sha256def get_users(): users = Users.query.all() return [{"id": i.id, "username": i.username, "pwd": i.pwd} for i in users]def get_user(user_id): users = Users.query.all() user = list(filter(lambda x: x.id == user_id, users))[0] return {"id": user.id, "username": user.username, "pwd": user.pwd}def add_user(username, pwd): if username and pwd : try: user = Users(username, pwd) user.save() return True except Exception as e: print(e) return False else: return Falsedef remove_user(user_id): if user_id: try: user = Users.query.get(user_id) db.session.delete(user) db.session.commit() return True except Exception as e: print(e) return False else: return Falsedef encrypt_pwd(pwd): return hashpw(b64encode(sha256(pwd.encode()).digest()), gensalt()).decode()def check_pwd(x, y): """ Check whether password hashed matches: * arg x** password to check * arg y** original hashed password """ return checkpw(b64encode(sha256(x.encode()).digest()), y.encode())
```

这些功能中的每一个只是执行它们的功能或者返回*假*。整个想法是根据需要从每个路径调用所述函数，从而将动作的逻辑与路径分离。

现在，我终于可以浏览路线本身的设置了。

以下全部在 *routes.py* 实现

```
***authentication_example/app/auth/routes.py*** from app import jwtfrom app.auth import bpfrom app.auth.helpers import *from app.auth.models import Users, InvalidTokenfrom flask import request, jsonifyfrom flask_jwt_extended import create_access_token,       create_refresh_token, get_jwt_identity, get_jwt, jwt_required@jwt.token_in_blocklist_loaderdef check_if_blacklisted_token(data, decrypted): """ Decorator designed to check for blacklisted tokens """ jti = decrypted['jti'] return InvalidToken.is_invalid(jti)
```

首先需要一个装饰器，用于确定令牌是否有效。完成后，我想创建*注册和登录路径:*

```
@bp.route("/api/login", methods=["POST"])def login(): try: username = request.json["username"] pwd = request.json["pwd"] if username and pwd: user = list(filter(lambda x: x["username"] == username and check_pwd(pwd, x["pwd"]), get_users())) if len(user) == 1: token = create_access_token(identity=user[0]["id"]) refresh_token = create_refresh_token(identity=user[0]["id"]) return jsonify({"token": token, "refreshToken": refresh_token}) else: return jsonify({"error": "Invalid credentials"}) else: return jsonify({"error":"Invalid Form"}) except: return jsonify({"error": "Invalid Form"})@bp.route("/api/register", methods=["POST"])def register(): try: pwd = encrypt_pwd(request.json['pwd']) username = request.json['username'] users = get_users() if len(list(filter(lambda x: x["username"] == username, users))) == 1: return jsonify({"error": "Invalid Form"}) add_user(username, pwd) return jsonify({"success": True}) except Exception as e:
        return jsonify({"error": str(e)})
```

这里有两个非常简单的端点。

*登录*端点获取*用户名*和*密码*并使用助手函数验证它们，如果凭证正确，它将返回一个 *JWT 令牌*供它们在创建的会话中使用。

*寄存器*端点简单地检查以确保*用户名*尚未被使用，然后将用户添加到数据库中。

**一旦我有了这些端点，我还需要实现一些功能:**

1.  前端检查令牌是否过期的方法
2.  一种前端在令牌过期时刷新令牌的方法。
3.  前端检查当前用户的方式，由 *Flask-Login 提供。*
4.  一种使令牌无效的方法，从而模拟一个注销系统。

既然我已经有了*登录*系统和对黑名单令牌的检查，我可以使用 *@jwt_required()* 装饰器来确保我只想允许授权用户访问的任何路由都受到保护。

有了它，就很容易在四条不同的路线上实现这四个要求:

```
@bp.route("/api/checkiftokenexpire", methods=["POST"])@jwt_required()def check_if_token_expire(): return jsonify({"success": True})@bp.route("/api/refreshtoken", methods=["POST"])@jwt_required(refresh=True)def refresh(): identity = get_jwt_identity() token = create_access_token(identity=identity) return jsonify({"token": token})@bp.route("/api/getcurrentuser")@jwt_required()def current_user(): uid = get_jwt_identity() return jsonify(get_user(uid))@bp.route("/api/logout/refresh", methods=["POST"])@jwt_required()def logout(): """ End-point to invalidate the token. Can be used with both log the user out or for the frontend to call after refreshing the token. """ jti = get_jwt()["jti"] try: invalid_token = InvalidToken(jti=jti) invalid_token.save() return jsonify({"success": True}) except Exception as e: print(e) return jsonify({"error": str(e)})
```

实现了这四个端点之后，我还剩下最后一件认证系统需要完成的事情:删除旧用户。

幸运的是，我已经掌握了阿哈所需的所有逻辑，我需要做的就是创建一个单一的路由来处理这个问题。

```
@bp.route("/api/deleteaccount", methods=["DELETE"])@jwt_required()def delete_account(): try: user = get_user(get_jwt_identity()) remove_user(user.id) return jsonify({"success": True}) except Exception as e: return jsonify({"error": str(e)})
```

## 就这样，我有了整个认证系统，准备好使用任何我可能在 *Flask* 中使用 *JSON Web Token* 认证构建的 *API* 。

以蓝图的形式设置它的美妙之处在于，每当想要使用类似的认证系统设置新的 *API* 时，您可以简单地将 *auth/* 目录引入应用程序，注册蓝图，然后迁移数据库，一切准备就绪。

蓝图在我的 [Github](https://github.com/Kwsswart/flask_jwt_authentication_blueprint) 中，如果你想使用它，请随意克隆这个库。

在计算机科学中有很多方法可以做同样的事情，我喜欢尝试寻找新的不同的方法，这些方法可能比我以前做过的更好。

你有没有找到另一种方法来实现类似的东西？

如果是这样，请留下评论解释如何，我很想看看你是如何做到的！