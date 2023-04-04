# 茉莉花-海关间谍

> 原文：<https://blog.devgenius.io/jasmine-custom-spies-9022b163530a?source=collection_archive---------0----------------------->

![](img/9ab255b2400d240a150160bf21eedea3.png)

沃洛德梅尔·赫里先科在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

测试是 JavaScript 的一个重要部分。

在本文中，我们将看看如何创建自定义间谍。

# 海关间谍

我们可以使用`jasmine.setDefaultSpyStrategy`来创造一个做我们想做的事情的间谍。

例如，我们可以写:

```
describe("spy tests", function () {
  beforeEach(function () {
    jasmine.setDefaultSpyStrategy(and => and.returnValue("Hello World"));
  }); it('calls custom spy', function () {
    const spy = jasmine.createSpy();
    expect(spy()).toEqual("Hello World");
  });
});
```

我们用返回`'Hello World'`值的回调函数调用了`setDefaultSpyStrategy`。

一旦我们这样做了，我们的间谍就会返回`'Hello world'`。

所以当我们调用`spy`时，它返回`'Hello World'`。

同样，我们可以不带参数地调用`setDefaultSpyStrategy`来删除一个定制的默认值。

例如，我们可以写:

```
describe("spy tests", function () {
  beforeEach(function () {
    jasmine.setDefaultSpyStrategy(and => and.returnValue("Hello World"));
  }); it('calls custom spy', function () {
    const spy = jasmine.createSpy();
    expect(spy()).toEqual("Hello World");
  }); it("throws if you call any methods", function () {
    jasmine.setDefaultSpyStrategy(and => and.throwError(new Error("Do Not Call Me")));
    const program = jasmine.createSpyObj(['start']);
    jasmine.setDefaultSpyStrategy(); expect(() => {
      program.start();
    }).toThrowError("Do Not Call Me");
  });
});
```

我们在第二个测试中调用了`createSpyObj`，在`porgram` spy 对象中包含了一个`'start'`方法。

然后我们可以通过调用`start`来检查是否抛出了错误。

# 窥探财产

我们可以用`spyOnProperty`方法对属性进行 spu。

例如，我们可以写:

```
describe("property spy test", function () {
  let someObject = {
    get myValue() {
      return 1
    }
  }; beforeEach(function () {
    this.propertySpy = spyOnProperty(someObject, "myValue", "get").and.returnValue(1);
  }); it("lets you change the spy strategy later", function () {
    this.propertySpy.and.returnValue(3);
    expect(someObject.myValue).toEqual(3);
  });
});
```

我们用`myValue` getter 创建了一个`someObject`来返回一些东西。

然后在`beforeEach`钩子中，我们调用`spyOnPropoerty`窥探对象，返回我们想要的嘲讽值。

在我们的测试中，我们调用了`returnValue`让 getter 返回另一个值。

然后我们可以检查刚刚设置的返回值。

我们还可以通过传入一个属性散列来窥探一个具有多个属性的对象。

例如，我们可以写:

```
describe("property spy test", function () {
  it("lets you change the spy strategy later", function () {
    const obj = jasmine.createSpyObj("myObject", {}, { x: 3, y: 4 });
    expect(obj.x).toEqual(3); Object.getOwnPropertyDescriptor(obj, "x").get.and.returnValue(7);
    expect(obj.x).toEqual(7);
  });
});
```

我们创建一个具有一些属性的间谍对象。

它具有`x`和`y`属性。

然后我们可以检查`obj.x`并返回我们想要的值。

然后我们可以检查一下。

![](img/f1355f26f488c7d79249b9c528309268.png)

照片由[里奇·史密斯](https://unsplash.com/@richwilliamsmith?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 结论

窥探对象和返回值有不同的方法。

我们可以创建间谍对象并将间谍策略设置为我们想要的。