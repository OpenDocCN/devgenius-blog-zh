# JavaScript 单元测试最佳实践—边缘案例

> 原文：<https://blog.devgenius.io/javascript-unit-test-best-practices-edge-cases-15e9f904eb87?source=collection_archive---------8----------------------->

![](img/601ce46f076a754be6e3c4329ad8f4a3.png)

安德鲁·休斯在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 使用真实的输入数据

我们应该在测试中使用真实的输入数据，这样我们才能知道我们在测试什么。

为了使生成假数据变得容易，我们可以使用 Faker 包。

它可以生成姓名、用户名、公司名称、信用卡号等等。

例如，我们可以写:

```
it("should add product", async () => {
  const addProductResult = addProduct(faker.commerce.productName(), faker.random.number());
  expect(addProductResult).to.be.true;
});
```

我们有一个测试来添加一个具有真实名称和 ID 号的产品，以便我们可以了解结果。

# 测试许多输入组合

我们应该测试许多输入组合。

这样，我们不会只选择那些我们知道会通过测试的案例。

我们可以使值随机。

我们还可以在测试中对一些数据进行多种排列。

例如，使用快速检查库，我们可以为我们的测试创建随机的数据组合:

```
import fc from "fast-check";describe("Product service", () => {
  describe("add new product", () => {
    it("add product with various combinations successfully", () =>
      fc.assert(
        fc.property(fc.integer(), fc.string(), (id, name) => {
          expect(addNewProduct(id, name).status).toEqual("success");
        })
      ));
  });
});
```

我们用随机值`id`和`name`调用`addnewProduct`，并检查返回的状态是否为`'success'`。

这样，我们就不能操纵我们的测试只通过一些值。

# 仅使用短的内联快照

我们应该在 or 测试中使用短的内联快照，这样我们就可以创建运行速度很快的 UI 测试。

如果它们可以内联添加，那么我们知道它们会很小。

如果它太大了，以至于只能存储在一个外部文件中，那么它可能会大大降低我们测试的速度。

例如，我们可以写:

```
it("when we go to example.com, a menu is displayed", () => {
  const receivedPage = renderer
    .create(<DisplayPage page="http://www.example.com">Example</DisplayPage>)
    .toJSON(); const menu = receivedPage.content.menu;
  expect(menu).toMatchInlineSnapshot(`<ul>
    <li>Home</li>
    <li>Profile</li>
    <li>Logout</li>
  </ul>`);
});
```

我们呈现`DisplayPage`组件，然后检查我们内联创建的快照。

# 避免全局测试夹具和种子

我们应该为每次测试创建数据，并在每次测试后清除它们。

这样，我们总是可以得到一个干净的测试环境。

测试也不会互相依赖。

这很重要，因为当测试相互依赖时，我们会遇到问题。

如果性能成为为每个测试创建数据的一个问题，那么我们必须简化数据。

因此，如果我们测试数据库交互，我们必须在每次测试后删除所有数据。

# 预期错误

如果我们预期会出现错误，那么我们会记录下应用程序中出现的错误。

在大多数 JavaScript 测试框架中，我们有这样的东西:

```
expect(method).toThrow())
```

检查`method`是否在我们做了某件事之后抛出了某个东西。

掩盖错误只会让错误难以被发现。

但它仍然没有达到我们的预期。

所以我们可以这样写:

```
it("when no data provided, it throws error 400", async () => {
  await expect(addUser({}))
    .to.eventually.throw(AppError)
    .with.property("code", "invalid input");
});
```

![](img/689d2c461965ac9f5ab34da2bfffe75d.png)

照片由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[睡眠音乐](https://unsplash.com/@sleepmusic?utm_source=medium&utm_medium=referral)拍摄

# 结论

我们应该使用真实的数据进行测试。

此外，我们使用内联快照来加快测试速度。

我们还应该用多种输入进行测试。