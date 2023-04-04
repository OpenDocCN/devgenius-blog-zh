# Jasmine —测试数组、字符串和依赖于时间的代码

> 原文：<https://blog.devgenius.io/jasmine-test-array-strings-and-time-dependent-code-29397e7e04fd?source=collection_archive---------3----------------------->

![](img/b6a2ad020ac97f1c932d3ca599799b3b.png)

Elena Mozhvilo 在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

测试是 JavaScript 的一个重要部分。

在本文中，我们将看看如何用 Jasmine 创建更复杂的测试。

# 数组检查

我们可以使用`jasmine.arrayContaining`方法来检查数组的内容。

例如，我们可以写:

```
describe("jasmine.arrayContaining", function () {
  let foo; beforeEach(function () {
    foo = [1, 2, 3];
  }); it("matches arrays with some of the values", function () {
    expect(foo).toEqual(jasmine.arrayContaining([3, 1]));
    expect(foo).not.toEqual(jasmine.arrayContaining([6]));
  }); describe("when used with a spy", function () {
    it("is useful when comparing arguments", function () {
      const callback = jasmine.createSpy('callback');
      callback([1, 2, 3, 4]);
      expect(callback)
        .toHaveBeenCalledWith(
          jasmine.arrayContaining([4, 2, 3])
        );
      expect(callback)
        .not
        .toHaveBeenCalledWith(
          jasmine.arrayContaining([5, 2])
        );
    });
  });
});
```

我们有`jasmine.arrayContaining`方法来检查数组中的数字。

如果被检查的数组包含了我们传递给`arrayContaining`的数组中的所有条目，它将返回`true`。

我们可以对函数参数做同样的事情。

# 字符串匹配

我们可以使用`jasmine.stringMatching`来检查一个字符串是否有给定的子串或模式。

例如，我们可以写:

```
describe('jasmine.stringMatching', function () {
  it("matches as a regex", function () {
    expect({ foo: 'baz' })
      .toEqual({
        foo: jasmine.stringMatching(/^baz$/)
      });
    expect({ foo: 'foobarbaz' })
      .toEqual({
        foo: jasmine.stringMatching('bar')
      });
  }); describe("when used with a spy", function () {
    it("is useful for comparing arguments", function () {
      const callback = jasmine.createSpy('callback');
      callback('foobarbaz');
      expect(callback)
        .toHaveBeenCalledWith(
          jasmine.stringMatching('baz')
        );
      expect(callback)
        .not
        .toHaveBeenCalledWith(
          jasmine.stringMatching(/^baz$/)
        );
    });
  });
});
```

我们有带正则表达式和字符串的`jasmine.stringMatching`方法。

它让我们检查正则表达式模式和子串。

# 不对称等式测试器

我们可以创建自己的等式测试器来进行检查。

例如，我们可以写:

```
describe("custom asymmetry", function () {
  const tester = {
    asymmetricMatch(actual) {
      return actual.includes('bar');
    }
  }; it("dives in deep", function () {
    expect("foo,bar,baz,quux").toEqual(tester);
  }); describe("when used with a spy", function () {
    it("is useful for comparing arguments", function () {
      const callback = jasmine.createSpy('callback');
      callback('foo,bar,baz');
      expect(callback).toHaveBeenCalledWith(tester);
    });
  });
});
```

我们用`asymmericMatch`方法创建了自己的`tester`对象来检查我们想要的东西。

我们只是返回一个布尔表达式，其中包含我们想要检查的条件。

然后我们可以配合`toHaveBeenCalledWith`或者`toEqual`使用。

因此，我们可以检查值和参数。

# 茉莉花钟

我们可以用`jasmine.clock`方法测试依赖于时间的代码。

例如，我们可以写:

```
describe("custom asymmetry", function () {
  let timerCallback; beforeEach(function () {
    timerCallback = jasmine.createSpy("timerCallback");
    jasmine.clock().install();
  }); afterEach(function () {
    jasmine.clock().uninstall();
  }); it("causes a timeout to be called synchronously", function () {
    setTimeout(function () {
      timerCallback();
    }, 100);
    expect(timerCallback).not.toHaveBeenCalled();
    jasmine.clock().tick(101);
    expect(timerCallback).toHaveBeenCalled();
  });
});
```

我们有`jasmine.clock().install()`让茉莉嘲笑时间。

然后我们调用`jasmine.clock().uninstall()`来移除茉莉时钟。

在我们的测试中，我们可以根据需要改变时间。

然后我们可以在一段时间后进行检查。

![](img/d8a861207471c4aa473e53623baa3912.png)

西蒙·雷伊在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 结论

我们可以用 Jasmine 的时钟方法测试依赖于时间的代码。

此外，我们可以使用内置函数检查数组和字符串项。

我们可以自己做一些比赛来测试我们想要的。