# JavaScript 单元测试最佳实践— UI 测试

> 原文：<https://blog.devgenius.io/javascript-unit-test-best-practices-ui-tests-99ee3e7fe467?source=collection_archive---------9----------------------->

![](img/396d16953d4eae4fd58d02972b3cf9da.png)

戴维·坎泰利在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 基于不太可能改变的属性查询 HTML 元素

我们应该只检查属性不太可能改变的 HTML 元素。

这样，当我们做一些小的改变时，我们不必更新我们的测试。

此外，这确保了外观和感觉的变化不会破坏我们的测试。

例如，不要写:

```
test("whenever no data is passed, number of messages shows zero", () => {
  // ...
  expect(wrapper.find("[className='mr-2']").text()).toBe("0");
});
```

要测试:

```
<span id="metric" className="mr-2">{value}</span>
```

相反，我们将自己的 ID 添加到想要测试的元素中，然后在测试中使用它。

例如，如果我们有:

```
<h3>
  <Badge pill className="fixed_badge" variant="dark">
    <span data-testid="msgs-label">{value}</span>
  </Badge>
</h3>
```

我们可以用以下方法进行测试:

```
test("whenever no data is passed, number of messages shows zero", () => {
  const metricValue = undefined;
  const { getByTestId } = render(<dashboardMetric value={undefined} />);
  expect(getByTestId("msgs-label").text()).toBe("0");
});
```

我们不应该依赖于可以随时改变的 CSS 属性。

相反，我们添加一个很少或从不改变的 ID。

# 使用真实且完全渲染的组件进行测试

我们应该使用真实的、完全渲染的组件进行测试。

这样，我们可以相信我们的测试实际上是在测试组件中的东西。

如果我们模仿或者进行部分或浅层渲染，我们可能会在测试中遗漏一些东西。

如果用真实的东西测试太慢，那么我们可以考虑模仿。

例如，代替使用`shallow`的浅层渲染:

```
test("when click to show filters, filters are displated", () => {
  const wrapper = shallow(<Calendar showFilters={false} title="Select Filter" />);
  wrapper
    .find("FiltersPanel")
    .instance()
    .showFilters();

  expect(wrapper.find("Filter").props()).toEqual({ title: "Select Filter" });

});
```

我们写道:

```
test("when click to show filters, filters are displated", () => {
  const wrapper = mount(<Calendar showFilters={false} title="Select Filter" />);
  wrapper.find("button").simulate("click");
  expect(wrapper.text().includes("Select Filter"));
});
```

我们调用`mount`来完全安装`Calendar`组件。

然后我们像真正的用户一样点击按钮。

然后我们检查应该出现的文本。

# 使用框架内置的异步事件支持

我们应该在运行测试时测试框架内置的异步事件。

这样，我们实际上是在运行某个东西之前等待我们想要的东西出现。

在固定的时间睡觉是不可靠的，而且在做我们想做的事情之前等待项目出现也没有帮助。

这意味着我们的测试将是不可靠的。

还有，固定时间睡觉要慢很多。

例如，用柏树，我们可以写:

```
cy.get("#show-orders").click();
cy.wait("@orders");
```

当我们点击 ID 为`show-orders`的元素时，我们等待`orders`出现。

我们不想让代码等待我们自己的逻辑和`setInterval`:

```
test("user name appears", async () => {
  //...
  const interval = setInterval(() => {
    const found = getByText("james");
    if (found) {
      clearInterval(interval);
      expect(getByText("james")).toBeInTheDocument();
    }
  }, 100); const movie = await waitForElement(() => getByText("james"));
});
```

这很复杂，而且我们没有利用测试框架的全部功能。

![](img/8d5bbffe74173525a93f0a9bec71bf5b.png)

约翰·贝克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们应该用测试框架的等待功能来等待。

此外，我们应该用真实的组件进行测试。