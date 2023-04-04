# 使用 Hapi.js 进行服务器端开发——令牌、JWT 和秘密

> 原文：<https://blog.devgenius.io/server-side-development-with-hapi-js-tokens-jwt-and-secrets-8f79ab5b7d92?source=collection_archive---------1----------------------->

![](img/d680687c496482d3e883cb700bfe23b2.png)

克里斯蒂安·埃斯科瓦尔在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

Hapi.js 是一个用于开发后端 web 应用程序的小型节点框架。

在本文中，我们将看看如何用 Hapi.js 创建后端应用程序。

# 创建令牌

我们可以用`@hapi/iron`模块创建令牌。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');
const iron = require('@hapi/iron')const obj = {
  a: 1,
  b: 2,
  c: [3, 4, 5],
  d: {
      e: 'f'
  }
};const password = 'passwordpasswordpasswordpassword';const init = async () => {  
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    async handler(request, h) {    
      try {
        const sealed = await iron.seal(obj, password, iron.defaults);
        return sealed
      } catch (err) {
        console.log(err);
      }                    
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};

process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们用想要加密的对象调用`iron.seal`方法，用`password`访问加密的对象，还有设置，也就是`iron.defaults`。

他们的`sealed`字符串是字符串形式的对象的混乱版本。

然后为了解封它，我们调用`iron.unseal`方法。

例如，我们可以写:

```
const Hapi = require('@hapi/hapi');
const iron = require('@hapi/iron')const obj = {
  a: 1,
  b: 2,
  c: [3, 4, 5],
  d: {
      e: 'f'
  }
};const password = 'passwordpasswordpasswordpassword';const init = async () => {  
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); server.route({
    method: 'GET',
    path: '/',
    async handler(request, h) {    
      try {
        const sealed = await iron.seal(obj, password, iron.defaults);
        const unsealed = await iron.unseal(sealed, password, iron.defaults);
        return unsealed
      } catch (err) {
        console.log(err);
      }                    
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};

process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

用同样的`password`用`iron.unseal`方法解密加密的字符串。

# JSON Web 令牌

我们可以用`@hapi/jwt`模块创建和验证 JSON web 令牌。

例如，我们可以这样使用它:“

```
const Hapi = require('@hapi/hapi');
const Jwt = require('@hapi/jwt');
const jwt = require('jsonwebtoken');const init = async () => {  
  const server = new Hapi.Server({
    port: 3000,
    host: '0.0.0.0'
  }); await server.register(Jwt); server.auth.strategy('my_jwt_stategy', 'jwt', {
    keys: 'some_shared_secret',
    verify: {
      aud: 'urn:audience:test',
      iss: 'urn:issuer:test',
      sub: false,
      nbf: true,
      exp: true,
      maxAgeSec: 14400,
      timeSkewSec: 15
    },
    validate: (artifacts, request, h) => {
      return {
        isValid: true,
        credentials: { user: artifacts.decoded.payload.user }
      };
    }
  }); server.route({
    method: 'GET',
    path: '/',
    config: {
      handler(request, h) {    
        const token = jwt.sign({ 
          aud: 'urn:audience:test',
          iss: 'urn:issuer:test',
          sub: false,
          maxAgeSec: 14400,
          timeSkewSec: 15
        }, 'some_shared_secret');   
        return token;             
      },}
  }); server.route({
    method: 'GET',
    path: '/secret',
    config: {
      handler(request, h) {    
        return 'secret';             
      },
      auth: {
        strategy: 'my_jwt_stategy',        
      }
    }
  }); await server.start();
  console.log('Server running at:', server.info.uri);
};

process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});
init();
```

我们添加了`jsonwebtoken`模块来创建令牌。

然后我们调用`server.auth.strategy`来添加 JWT 认证策略。

`keys`有我们用来验证令牌的密钥。

`verify`有我们要验证的字段。

`validate`有一个函数返回`isValid`来表明令牌是有效的。

`credentials`拥有来自解码令牌的数据。

然后我们调用`jwt.sign`来签署`/`路由处理器中的令牌。

在`/secret`路由中，我们有`auth.strategy`属性来设置授权策略。

# 结论

我们可以创建各种令牌，并用哈比神插件验证它们。