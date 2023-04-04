# JavaScript 单元测试最佳实践—关注点

> 原文：<https://blog.devgenius.io/javascript-unit-test-best-practices-concerns-85bbc0e48fb3?source=collection_archive---------10----------------------->

![](img/9bfe65a441da85d904663a47c02943a0.png)

穆斯塔法·梅拉吉在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

单元测试对于检查我们的应用程序如何工作非常有用。

否则，我们以后会遇到各种各样的问题。

在本文中，我们将看看在编写 JavaScript 单元测试时应该遵循的一些最佳实践。

# 不要在同一测试中测试多个关注点

为了简单起见，我们应该在每次测试中只测试一个东西。

如果我们发现问题，它将帮助我们更容易地定位任何问题。

例如，不要写:

```
it('should save profile data and update the view with profile data', () => {
  // expect(...)to(...);
  // expect(...)to(...);
});
```

我们写道:

```
it('should save profile data', () => {
  // expect(...)to(...);
});it('should update the view with profile data', () => {
  // expect(...)to(...);
});
```

在测试描述中写‘and’或‘or’意味着我们应该把它们分成多个测试。

# 涵盖一般情况和边缘情况

我们要报道边缘案件，这样我们就不会错过任何东西。

他们很容易被忽略，但人们会注意到他们。

例如，不写:“

```
it('should properly calculate arithmetic expression', () => {
  const result = calculate('(5 + 10) / 2');
  expect(result).toBe(7.5);
});
```

我们写道:

```
describe('The arithmetic expression calculator', () => {
  it('should return null when the expression is an empty string', () => {
    const result = calculator('');
    expect(result).toBeNull();
  }); it('should return value if expression is the value', () => {
    const result = calculator('12');
    expect(result).toBe(12);
  }); it('should properly calculate arithmetic expression', () => {
    const result = calculate('(5 + 10) / 2');
    expect(result).toBe(7.5);
  }); it('should throw an error whenever an invalid expression is passed', () => {
    const calc = () => calculator('1 + 1 -');
    expect(calc).toThrow();
  });
});
```

# 总是从用 TDD 编写最简单的失败测试开始

我们应该总是先用 TDD 写一个失败的测试。

然后我们可以构建我们的功能来让它通过。

# 在每个测试优先的周期中，总是要迈出一小步

在每个测试的第一个周期中迈出一小步会让我们从失败到通过测试。

例如，不写:

```
it('should properly calculate arithmetic expression', () => {
  const result = calculate('((5 + 10) / 2)  + 10');
  expect(result).toBe(17.5);
});
```

为了测试一切，我们分别测试小案例，并使每个案例都通过:

```
describe('arithmetic expression calculator', () => {
  it('should return null when the expression is an empty string', () => {
    const result = calculator('');
    expect(result).toBeNull();
  }); it('should return value if the expression only has one value', () => {
    const result = calculator('12');
    expect(result).toBe(12);
  }); describe('addition', () => {
    it('should properly calculate a simple addition', () => {
      const result = calculator('10 + 1');
      expect(result).toBe(11);
    }); it('should properly calculate a complex addition', () => {
      const result = calculator('1 + 2 + 3');
      expect(result).toBe(6);
    });
  }); // ... describe('complex expressions', () => {
    it('should properly calculate an expression containing all operators', () => {
      const result = RPN('5 + (10 / 2) * 2 - 1'));
      expect(result).toBe(14);
    });
  });
});
```

我们的测试从最简单的情况到最复杂的情况。

这样，我们知道我们不会错过太多东西。

![](img/c1af9e34fe39d9c8348cec4da29c926e.png)

由[莫斯塔法·梅拉吉](https://unsplash.com/@mostafa_meraji?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 拍摄的照片

# 结论

我们应该在每次测试中测试一个东西。

此外，我们应该有一个测试来测试从最简单到最复杂的情况。