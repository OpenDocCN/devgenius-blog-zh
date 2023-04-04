# 茉莉花——争论和平等

> 原文：<https://blog.devgenius.io/jasmine-arguments-and-equality-1e98d70b6a3b?source=collection_archive---------1----------------------->

![](img/da68c7b45596786f1a123c064ec6ab9a.png)

Elena Mozhvilo 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

测试是 JavaScript 的一个重要部分。

在本文中，我们将看看如何以不同的方式检查相等性。

# 自定义参数匹配器

我们可以创建自己的自定义匹配来检查我们想要的任何类型的数据。

例如，我们可以写:

```
function multipleOf(number) {
  return {
    asymmetricMatch(compareTo) {
      return compareTo % number === 0;
    }, jasmineToString() {
      return `<multipleOf: ${number}>`;
    }
  };
}describe("multipleOf tests", function () {
  it('calls Buffer.alloc with some that is a multiple of 1024', function () {
    spyOn(Buffer, 'alloc').and.callThrough();
    Buffer.alloc(2048);
    expect(Buffer.alloc).toHaveBeenCalledWith(multipleOf(1024));
  });
});
```

来创建我们的匹配器和使用它的测试。

`mutlipleOf`函数接受`number`参数，这是我们传递给匹配器的参数。

在函数中，我们用`asymmetricMatch`和`jasmineToString`方法返回一个对象。

`asymmetricMatch`是比附什么。

如果匹配是我们要找的，我们返回`true`，否则返回`false`。

`jasmineToString`让我们在一个字符串中显示测试名称。

Out matcher 可以嵌套在任何地方。

所以我们可以写:

```
function multipleOf(number) {
  return {
    asymmetricMatch(compareTo) {
      return compareTo % number === 0;
    }, jasmineToString() {
      return `<multipleOf: ${number}>`;
    }
  };
}const foo = {
  bar() { }
}describe("multipleOf tests", function () {
  it('calls foo.bar with some that is a multiple of 10', function () {
    spyOn(foo, 'bar');
    foo.bar({ name: 'mary', age: 20 });
    expect(foo.bar)
      .toHaveBeenCalledWith(
        {
          name: jasmine.any(String),
          age: multipleOf(10)
        }
      );
  });
});
```

我们为`foo.bar`创建一个间谍来监视参数。

我们可以像使用`jasmine.any`匹配器一样使用`multipleOf`匹配器。

# 自定义等式测试器

我们可以用 Jasmine 创建自己的自定义等式测试器。

例如，我们可以写:

```
function customTester(first, second) {
  if (typeof first === 'string' && typeof second === 'string') {
    return first[0] === second[1];
  }
}describe("multipleOf tests", function () {
  beforeEach(function () {
    jasmine.addCustomEqualityTester(customTester);
  }); it('is equal using a custom tester', function () {
    expect('abc').toEqual(' a ');
  }); it('is not equal using a custom tester', function () {
    expect('abc').not.toEqual('abc');
  }); it('works in nested equality tests', function () {
    expect(['abc', '123']).toEqual([' a ', ' 1 ']);
  });
});
```

我们创建了带两个参数的`customTester`,并根据我们的标准检查两个东西是否相同。

`first`是我们正在检查的值，而`second`是我们期望的值。

我们检查它们是否都是字符串。

如果是，那么我们检查`first`的第一个字符是否与`second`的第二个字符相同。

它可以检查值，不管它们是否嵌套。

最后一个测试检查数组的每个条目。

![](img/f4ca751b39b5ef60c2d6d1f9f76a7976.png)

照片由[丘特尔斯纳普](https://unsplash.com/@chuttersnap?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

我们可以用各种方法来检查匹配。

我们可以检查参数和等式匹配。