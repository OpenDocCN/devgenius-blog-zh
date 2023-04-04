# 什么是干原理？

> 原文：<https://blog.devgenius.io/what-is-the-dry-principle-bb62a2ca156d?source=collection_archive---------7----------------------->

## ***DRY 原则是一个软件开发原则，声明“不要重复自己”。***

## 这个原则是基于这样的想法:多次编写相同的代码不仅会使代码更难维护，还会使代码更容易出错。

遵循 DRY 原则的一个主要好处就是可以在开发过程中节省大量的时间和精力。

![](img/3ce25edcf7b3b43b4c01d12bb8b694d0.png)

照片由[西格蒙德](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

当您多次编写相同的代码时，您也必须多次编写、测试和调试该代码。通过**避免重复**，你可以避免这些额外的工作，专注于项目的其他方面。

DRY 原则的另一个好处是它可以让你的代码更容易维护。如果您有相同代码的多个实例，那么很难跟踪所有的实例并确保它们在需要更改时都得到更新。
通过将你的代码整合到**一个单独的位置**，随着项目的进展，你可以更容易地维护和更新你的代码。

此外，遵循 DRY 原则还可以使你的代码更加模块化和可重用。

当同一代码有多个实例时，很难在不影响项目其他部分的情况下更改该代码。通过避免重复，您可以创建无需重复即可在多个地方使用的代码模块。

总之，DRY 原则在软件开发中是一个很好的实践，因为它可以**在开发过程**中节省时间和精力，使你的代码**更易于维护**，并使**更易于调试**。

下面是一个不遵循 **DRY** 原则的代码示例:

```
function validateUsername(username) {
  if (username.length < 5) {
    return false;
  }
  if (username.length > 15) {
    return false;
  }
  if (!username.match(/^[a-zA-Z0-9]+$/)) {
    return false;
  }
  return true;
}

function validatePassword(password) {
  if (password.length < 8) {
    return false;
  }
  if (password.length > 20) {
    return false;
  }
  if (!password.match(/^[a-zA-Z0-9]+$/)) {
    return false;
  }
  return true;
}
```

在这些代码片段中，用于检查用户名和密码的长度和字符集的代码在`validateUsername`和`validatePassword`函数中是重复的。这不是 ***干顺*** 。

为了纠正这段代码并遵循 **DRY** 原则，我们可以创建一个单独的`validateString`函数来检查字符串的长度和字符集，并在`validateUsername`和`validatePassword`函数中使用该函数，如下所示:

```
function validateString(str, minLength, maxLength, regex) {
  if (str.length < minLength) {
    return false;
  }
  if (str.length > maxLength) {
    return false;
  }
  if (!str.match(regex)) {
    return false;
  }
  return true;
}

function validateUsername(username) {
  return validateString(username, 5, 15, /^[a-zA-Z0-9]+$/);
}

function validatePassword(password) {
  return validateString(password, 8, 20, /^[a-zA-Z0-9]+$/);
}
```

现在，字符串验证代码不再重复，并遵循 DRY 原则。

我们甚至可以为它写一个测试！

```
describe("validateString", () => {
  it("should return true for a valid string", () => {
    const str = "abc123";
    const minLength = 1;
    const maxLength = 10;
    const regex = /^[a-zA-Z0-9]+$/;
    expect(validateString(str, minLength, maxLength, regex)).toBe(true);
  });

  it("should return false for a string that is too short", () => {
    const str = "abc";
    const minLength = 5;
    const maxLength = 10;
    const regex = /^[a-zA-Z0-9]+$/;
    expect(validateString(str, minLength, maxLength, regex)).toBe(false);
  });

  it("should return false for a string that is too long", () => {
    const str = "abcdefghijklmnopqrstuvwxyz";
    const minLength = 1;
    const maxLength = 10;
    const regex = /^[a-zA-Z0-9]+$/;
    expect(validateString(str, minLength, maxLength, regex)).toBe(false);
  });

  it("should return false for a string that contains invalid characters", () => {
    const str = "abc!@#";
    const minLength = 1;
    const maxLength = 10;
    const regex = /^[a-zA-Z0-9]+$/;
    expect(validateString(str, minLength, maxLength, regex)).toBe(false);
  });
});
```

这个测试用例将验证`validateString`函数对于有效的字符串返回`true`，对于过短或过长的字符串返回`false`，对于包含无效字符的字符串返回`false`。

对于非干代码段，我们将不得不单独测试所有验证字符串的函数。使用`validateString`函数，我们集中了验证逻辑，从而使我们的代码更安全、更易于测试。

就是这样！🥳

希望你现在更好的理解干原理的来龙去脉！

如果你喜欢这个内容，看看我的其他帖子，你可能会发现一些有趣的东西！

此外，如果你希望支持我，你可以关注我的帐户，我主要写编程和一般技术。

回头见！