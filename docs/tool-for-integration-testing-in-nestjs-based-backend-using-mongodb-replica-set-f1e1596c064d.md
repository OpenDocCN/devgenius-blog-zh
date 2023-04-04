# 使用 Mongodb 副本集在基于 NestJS 的后端进行集成测试的工具

> 原文：<https://blog.devgenius.io/tool-for-integration-testing-in-nestjs-based-backend-using-mongodb-replica-set-f1e1596c064d?source=collection_archive---------4----------------------->

![](img/49f072eaac70086d83af1a407dc2d302.png)

照片由[鲁拜图·阿扎德](https://unsplash.com/@rubaitulazad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/mongodb?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

构建在 [NestJS 框架](https://nestjs.com/)之上的后端服务能够利用测试框架 [Jest](https://docs.nestjs.com/fundamentals/testing) 。NestJS 和 Jest 提出了开发单元测试和端到端测试的解决方案。虽然两者都是必要的，但它们超出了本文的范围。在这里，我们展示了集成测试的文档，其中测试是针对集成了其依赖项(例如数据库、云功能等)的服务运行的。

# 情况

以前我的项目中的测试是由 QA 或产品运营人员手工完成的。对于简单的情况，这种做法就足够了。然而，该项目正在发展，现在已经具备了业务流程的高级功能。在手工测试中，高级特性很难被详细地测试出来，因为手工测试很累人，而且容易出现人为错误。此外，QA 或产品运营还要处理其他需要时间的日常任务。

另一个问题来自具有高级特性的服务依赖。例如，Mongo DB 及其名为 mongoose 的 Node.js 库有一个对数据一致性非常重要的“事务”特性。我和我的团队在 2022 年中期经历了数据不一致。QA 和产品运营发现测试通过了手动测试。然而，在生产中不一致性仍然存在。

# 工作

手动测试不能用于预先工程问题。因此，测试自动化是解决这个难题的一种方法。来自依赖关系的问题不能在单元测试中测试，但是在端到端测试中测试则是大材小用。这就是工程师提供解决方案来创建集成测试原因。

# 行动

工程师准备在后端服务中进行集成测试的要求和步骤。

# 要求

1.  包含 mongodb 副本集初始化的 Shell 脚本放在根目录`scripts/setup.sh`中。此处预定义了最多 3 个副本集和 1 个主副本集的脚本。

2.Docker-compose 文件放在根目录下，`docker-compose.yml`。这个文件将创建 mongodb 副本集容器和一个 db 安装容器。

3.在`package.json`进行测试前、测试中和测试后的 Npm 脚本。

4.在测试目录`test/jest-integration.json`中创建一个文件，使 jest 能够识别集成测试的测试用例。

5.`.env`文件放在包含`DB_URI_TEST`的项目的根目录下

```
DB_URI_TEST=mongodb://localhost:27017/db-test?readPreference=primary&directConnection=true
```

6.在测试目录中使用模式`<any case>.integration.spec.ts`在文件名中创建测试用例。例如:

```
test/order.integration.spec.ts
test/product.integration.spec.ts 
```

确保在所有测试用例之前连接到主要主机。

# 运行测试的步骤

1.  在项目根目录下打开一个终端。
2.  运行测试命令，`npm run test:integration`。注意，我们不需要调用 pretest 和 posttest 命令。NestJS 会在测试前调用 pretest，在所有测试用例通过后调用 posttest。
3.  Npm 将首先运行预测试脚本。
4.  等待测试，直到它停止。
5.  测试完成了。

*   如果测试通过，npm 将运行测试后脚本。
*   否则，测试将停止，不运行后测试。它允许我们修复错误，然后重新运行测试命令。