# 管理 API 调用的简单方法

> 原文：<https://blog.devgenius.io/a-simple-approach-to-managing-api-calls-536e1b594646?source=collection_archive---------24----------------------->

## 一个全面的入门解决方案，使 API 调用在代码架构和开发团队中结构化和可管理。

![](img/0e22e26b77007f7394cbc73e58642c88.png)

里卡多·维亚纳在 [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上的照片

在我的文章“[设计前端项目以适应规模](https://medium.com/swlh/why-frontend-architecture-matters-4e647c23d67c)”中，我们看了一下如何组织我们的前端代码库，使得团队的规模和成功变得更加容易。在本文中，我们将深入代码组织的服务层。具体来说，我们将研究一个简单的解决方案来管理第三方 API 或我们自己的数据源，这种方法将帮助我们避免管理我们的代码库时遇到的一些挫折，因为 API 会随着时间而变化。

当我们第一次开始构建功能时，大多数人倾向于将所有的功能逻辑都放在一个组件中。数据库调用、状态管理以及所有被管理或显示我们向最终用户呈现的数据的子组件都位于此处。这样做的结果是，我们开始创建一组非常臃肿的文件，这些文件消费、管理和呈现所有的逻辑，因为随着业务逻辑的增加，逻辑变得更加复杂。开始时可能只是简单的 CRUD(创建、读取、更新、删除)操作，但不可避免地会发展成大量专门的功能和错综复杂的业务逻辑。如果我们在代码架构设计过程中不小心，我们可能会发现自己被锁定在如此混乱的函数依赖中，以至于我们甚至害怕重构过程，因为我们不想创建一个可能需要我们花整个周末来修复的 bug。

# 避免混乱

我们可以避免的这种业务逻辑混乱的一部分是，不要将 API 调用直接硬编码到组件中。我们的目标是将与 API 逻辑相关的一切抽象到我们的服务层中，以便使我们的组件更加精简和可维护。这个概念直接与 Dan Abramov 的文章“表示和容器组件”保持一致，并在我们的前端框架中创建了一个模型/服务层，以从我们的可重用组件中抽象出大多数业务逻辑。

这里有一个简单的例子，你可以从它开始:

```
import React, { useEffect } from 'react';
import axios from 'axios';

let API_URL_TASKS = 'https://url.com/api/v1/tasks';

export function Tasks() {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    _getTasks();
  }, []);

  function _getTasks() {
    axios
      .get(API_URL_TASKS)
      .then((res) => {
        let arr = _parseTasks(res.results.data);
        setTasks(arr);
      })
      .catch((err) => {
        _handleError(err, type);
      });
  }

  function _parseTasks(tasks) {
    return tasks.map((task) => {
      // Parse task information
      return task;
    });
  }

  function _createTask(task) {
    axios
      .post(url, task)
      .then((res) => {
        _handleSuccess(res, 'post');
        // etc...
      })
      .catch((err) => {
        _handleError(err, 'post');
      });
  }

  function _updateTask(task) {
    let url = `${API_URL_TASKS}/${id}`;
    axios
      .patch(url, task)
      .then((res) => {
        _handleSuccess(res, 'patch');
        // etc...
      })
      .catch((err) => {
        _handleError(err, 'patch');
      });
  }

  function _removeTask(id) {
    let url = `${API_URL_TASKS}/${id}`;
    axios
      .delete(url)
      .then((res) => {
        _handleSuccess(res, 'delete');
        // etc...
      })
      .catch((err) => {
        _handleError(err, 'delete');
      });
  }

  function _handleSuccess(response, type) {
    // success message
    // actions against state with type
  }

  function _handleError(error, type) {
    // error message
    // actions based on type
    // etc...
  }

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>{task.name}</li>
      ))}
    </ul>
  );
}
```

正如您所看到的，我们组件的数据流直接相关，并且硬编码到它可能需要的一个或多个 API 端点。如果随着时间的推移，您开始对许多组件这样做，并且您的 API 需求从服务器或第三方 API 发生了变化，那么您现在已经陷入了寻找所有需要更改的实例的痛苦过程中，以避免最终用户的代码和接口失败。相反，我们将在我们的服务层中创建一些文件结构，以便更容易地维护随时间的变化。

```
my-app 
└── src
    ├── components
    ├── views
    |   └── tasks
    └── services
        ├── api
        |   ├── tasks
        |   └── utilities
        ├── model
        |   └── task
        └── etc...
```

# 服务设施

在 services 文件夹中，我们将创建几个实用程序，使我们的 API 对于所有组件和团队成员都是可重用的和标准化的。在这个例子中，我们将利用 JavaScript axios 库和 JavaScript 类来创建我们的 API 实用程序。

```
services
└── api
    └── utilities
        ├── core.js
        ├── index.js
        ├── provider.js
        └── response.js
```

这里我们将重点关注三个主要文件:

1.  **provider.js** —定义 axios 或任何 api 库应该如何与数据库连接，并将我们的响应数据连接回任何连接的文件或组件。
2.  **core.js** —定义可重用的类，该类使用我们的 provider.js 以及我们可以为每个 api 端点集合定义的选项。作为构造函数的结果，我们可以根据需要在单个 API 集合上扩展它的功能，同时仍然为我们的大部分代码保持一致的基础。
3.  **response.js** —处理响应解析、错误处理、日志记录等的中间件

# 提供者. js

```
// provider.js

import axios from 'axios'; 
import { handleResponse, handleError } from './response'; 

// Define your api url from any source.
// Pulling from your .env file when on the server or from localhost when locally
const BASE_URL = 'http://127.0.0.1:3333/api/v1'; 

/** @param {string} resource */ 
const getAll = (resource) => { 
  return axios 
    .get(`${BASE_URL}/${resource}`) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

/** @param {string} resource */ 
/** @param {string} id */ 
const getSingle = (resource, id) => { 
  return axios 
    .get(`${BASE_URL}/${resource}/${id}`) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

/** @param {string} resource */ 
/** @param {object} model */ 
const post = (resource, model) => { 
  return axios 
    .post(`${BASE_URL}/${resource}`, model) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

/** @param {string} resource */ 
/** @param {object} model */ 
const put = (resource, model) => { 
  return axios 
    .put(`${BASE_URL}/${resource}`, model) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

/** @param {string} resource */ 
/** @param {object} model */ 
const patch = (resource, model) => { 
  return axios 
    .patch(`${BASE_URL}/${resource}`, model) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

/** @param {string} resource */ 
/** @param {string} id */ 
const remove = (resource, id) => { 
  return axios 
    .delete(`${BASE_URL}/${resource}`, id) 
    .then(handleResponse) 
    .catch(handleError); 
}; 

export const apiProvider = { 
  getAll, 
  getSingle, 
  post, 
  put, 
  patch, 
  remove, 
};
```

# Core.js

在这个构造函数类中，我们可以定义将消耗哪些基础 API 资源。我们还可以扩展每个 API 实用程序中的类，以包含 API 表独有的自定义端点，而不会在远离该文件的代码库中意外创建一次性解决方案。

```
// core.js

import apiProvider from './provider';

export class ApiCore {
  constructor(options) {
    if (options.getAll) {
      this.getAll = () => {
        return apiProvider.getAll(options.url);
      };
    }

    if (options.getSingle) {
      this.getSingle = (id) => {
        return apiProvider.getSingle(options.url, id);
      };
    }

    if (options.post) {
      this.post = (model) => {
        return apiProvider.post(options.url, model);
      };
    }

    if (options.put) {
      this.put = (model) => {
        return apiProvider.put(options.url, model);
      };
    }

    if (options.patch) {
      this.patch = (model) => {
        return apiProvider.patch(options.url, model);
      };
    }

    if (options.remove) {
      this.remove = (id) => {
        return apiProvider.remove(options.url, id);
      };
    }
  }
}
```

# 响应. js

这是分开的，以保持我们的文件精简，并允许您在这里为所有 API 调用处理任何响应和错误逻辑。也许您想在这里记录一个错误，或者基于响应头为授权创建自定义操作。

```
// response.js

export function handleResponse(response) {
  if (response.results) {
    return response.results;
  }

  if (response.data) {
    return response.data;
  }

  return response;
}

export function handleError(error) {
  if (error.data) {
    return error.data;
  }
  return error;
}
```

# 单个 API

我们现在可以扩展我们的基本 api 类，以利用将用于任何 api 集合的所有 api 配置。

```
// Task API

const url = 'tasks';
const plural = 'tasks';
const single = 'task';

// plural and single may be used for message logic if needed in the ApiCore class.

const apiTasks = new ApiCore({
  getAll: true,
  getSingle: true,
  post: true,
  put: false,
  patch: true,
  delete: false,
  url: url,
  plural: plural,
  single: single
});

apiTasks.massUpdate = () => {
  // Add custom api call logic here
}

export apiTasks;
```

# 实施我们的变革

```
import React, { useEffect } from 'react';

import { apiTasks } from '@/services/api';

export function Tasks() {
  const [tasks, setTasks] = useState([]);

  useEffect(() => {
    _getTasks();
  }, []);

  function _getTasks() {
    apiTasks.getAll().then((res) => {
      let arr = _parseTasks(res.results.data);
      setTasks(arr);
    });
  }

  function _parseTasks(tasks) {
    return tasks.map((task) => {
      // Parse task information
      return task;
    });
  }

  function _createTask(task) {
    apiTasks.post(task).then((res) => {
      // state logic
    });
  }

  function _updateTask(task) {
    apiTasks.patch(task).then((res) => {
      // state logic
    });
  }

  function _removeTask(id) {
    apiTasks.remove(id).then((res) => {
      // state logic
    });
  }

  return (
    <ul>
      {tasks.map((task) => (
        <li key={task.id}>{task.name}</li>
      ))}
    </ul>
  );
}
```

现在我们已经完成了设置，我们可以根据需要将 api 调用导入并集成到多个组件中。这是一个更新的任务组件，包含我们的更改。

# 结论

通过将代码提取到可重用的服务实用程序中，我们的应用程序现在可以更容易地管理 API 更改。失败的 API 调用现在可以在一个位置解决，它的实现可以很容易地跟踪，我们的组件依赖关系可以快速更新，以反映数据流和操作的变化。我希望这能帮助你管理你的 API 结构，使你的代码不仅长期可持续，而且随着你的代码库和团队的成长，易于管理和理解！

这里有一个本文讨论的文件集合的链接:[要点链接](https://gist.github.com/michaelmcshinsky/2560d9494b241afce230d9c6f5de6448)