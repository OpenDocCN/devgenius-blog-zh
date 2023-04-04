# 编写干净代码的 5 个最佳实践

> 原文：<https://blog.devgenius.io/5-best-practices-for-writing-clean-code-be1a5095eae5?source=collection_archive---------10----------------------->

## 如何从一个普通开发人员的角度编写干净的代码

![](img/8e44449eab7e3d48defd742e0df34b67.png)

Goran Ivos 在 Unsplash 上拍摄的照片

在我成为软件工程师之前，我曾经编写代码来自动化繁琐的系统管理员任务。在这个角色中，我不必担心写干净的代码，因为任何有用的东西都是可以接受的。然而，当我转行成为一名开发人员时，我很快意识到我以前的经验和知识并没有让我充分认识到编写干净代码的重要性。

# 编写干净代码的重要性

干净的代码是有价值的资产，因为它易于阅读、理解和修改。这允许您和其他人高效地使用代码，使其更易于维护。因此，在不中断现有代码的情况下，查找和修复错误或添加新功能变得更加容易。

干净的代码是可重用的，因为它组织良好，编写清晰。这允许您轻松地将代码用于其他项目或与其他人共享。

# 如何编写干净的代码

要写出好的代码，你需要明白没有“最好”的方法。不同的人有不同的偏好和编码风格，我们都会受到自己偏见的影响。但是，有一些通用的最佳实践可以帮助您编写清晰、组织良好且高效的代码。这里有一些提示:

## **1。使用易于阅读的长描述性名称**

对变量、函数和代码中的其他元素使用清晰的描述性名称。这使得其他人更容易理解您的代码在做什么。一个描述性的名字应该是不言自明的，这样你就不需要写注释来解释它了。例如，如果您有一个保存商品总价的变量，您可以将其命名为`totalItemPrice`，这样可以清楚地描述它的用途，并且不需要单独的注释。这是另一个例子:

```
// Variables
let firstName = "John"
let lastName = "Smith"
let fullName = firstName + " " + lastName

// Functions
func calculateAreaOfCircle(radius: Double) -> Double {
  let area = Double.pi * radius * radius
  return area
}
```

避免在代码中使用缩写或首字母缩略词，除非它们广为人知。通过使用完整的单词和短语，您可以使您的代码更加自我解释，并减少注释或额外解释的需要。

## 2.避免重复

代码中的重复会使阅读和维护变得困难。例如，如果在程序的多个地方有相同的代码块执行某个操作，那么就很难跟踪所有这些实例并对它们进行一致的更改。这里有一个应该避免代码重复的例子:

```
let array1 = [1, 2, 3, 4, 5]
let array2 = [6, 7, 8, 9, 10]

// This is an example of code repetition that should be avoided
for number in array1 {
    print(number)
}

for number in array2 {
    print(number)
}
```

避免重复的一个方法是重构您的代码，这样您就可以从多个地方重用一段代码。这可以通过创建执行所需操作的函数或方法来实现，然后从代码中需要执行该操作的地方调用该函数或方法。它还使更改代码变得更加容易，因为您只需要更新执行该操作的函数或方法，而不是更新程序中每个单独的代码实例。这是重构后的版本:

```
let firstArray = [1, 2, 3, 4, 5]
let secondArray = [6, 7, 8, 9, 10]

func printNumbers(in array: [Int]) {
    for number in array {
        print(number)
    }
}

// Now we can use the function to print the numbers in both arrays
printNumbers(in: firstArray)
printNumbers(in: secondArray)
```

## 3.函数应该做一件事

编写函数时，重要的是专注于做一件事，并把它做好。这意味着每个功能都应该有一个清晰明确的目的，并且应该在没有任何不必要或无关代码的情况下完成这个目的。此外，设计良好的函数更容易测试、调试和在代码的其他部分重用。这里有一个例子:

```
func calculateArea(width: Double, height: Double) -> Double {
  return width * height
}
```

## 4.保持你的代码有组织和良好的结构

为了使你的代码更具可读性和更容易理解，保持它的组织性和良好的结构是很重要的。您可以使用缩进和空白来改善代码的视觉结构，使其更易于浏览和导航。通过一致地使用缩进，您可以显示代码的层次和结构，通过明智地使用空白，您可以提高代码的可读性和清晰度。这里有一个例子:

```
struct User {
    let name: String
    let email: String
    let age: Int
}

class UserManager {
    var users: [User] = []

    func addUser(name: String, email: String, age: Int) {
        let user = User(name: name, email: email, age: age)
        users.append(user)
    }

    func removeUser(email: String) -> Bool {
        if let index = users.firstIndex(where: { $0.email == email }) {
            users.remove(at: index)
            return true
        }
        return false
    }
}
```

## 5.让你的项目井井有条

一个组织良好的项目会让开发人员很容易理解和处理它。当项目增长时，如果文件和目录组织不当，导航会变得很困难。为了避免这种情况，重要的是要注意项目的组织，并确保文件、文件夹和目录的结构清晰合理。这将使其他人更容易理解项目并对其进行更改。

# 对自己要有耐心

干净的代码是一项宝贵的技能，需要时间和实践来培养。它包括编写易于阅读、理解和维护的代码。这意味着使用有意义的变量名和函数名，将代码分解成更小的模块，并遵循标准的编码惯例。从长远来看，编写干净的代码可以节省您的时间，使您更容易处理和修改您的代码，并减少错误和 bug 的风险。它还可以提高您工作的整体质量，使其更加可靠和高效。此外，干净的代码对于其他可能需要处理您的代码的开发人员，以及将来可能需要重新访问旧代码的您也是有益的。总的来说，学习编写干净的代码可以让你在整个职业生涯中受益。

-电弧