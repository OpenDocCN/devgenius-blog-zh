# 关于栈数据结构的一切😮

> 原文：<https://blog.devgenius.io/every-thing-about-stack-data-structure-f98b0130a0bd?source=collection_archive---------41----------------------->

**本文包括括号平衡||数组中堆栈||链表中的堆栈||代码||以及更多解释。**

> 堆栈是一种线性数据结构，它遵循特定的操作执行顺序。

![](img/8a7bf0e6ee66e8ca8dabaf2eb7112868.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上[的照片](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral)

把书架想象成一摞书，假设你有 6 本书，你把它们放在桌子上，最后一本书放在上面，如果你想从书架上拿走一本书，它将是你书架上的第一本。因为它遵循后进先出法。

# 为什么是 Stack？

**让我们看看它的一些应用，我们用它的地方:**

## 表达式评估

> 想象一下，你给了一个表达式，你要求对它求值，然后存储在一个堆栈中。

## 括号检查

> 假设你给了一个表达式为(54*6(5+7)+(90+6))，你要检查这个表达式是否有效。这里我们再次使用堆栈。**代码将在本文**中

## 递归函数

> 当你应用递归函数时，递归系统是基于一个叫做调用栈的栈。

# 收藏，关系，运营？

正如定义告诉它的那样，这种数据结构是基于项目和关系的集合之上的。有两种主要操作**弹出**和**推动**。Push 是将一个项目添加到栈顶，POP 是移除最后添加的顶层元素。因为它是基于后进先出的数据结构。我们将在本文后面看到代码示例。

# 在哪种数据结构中可以实现堆栈？

你可以在数组和链表上实现堆栈。我们来了解一下哪个更好，为什么。假设你已经创建了一个大小为 5 的数组，其中有 4 个元素，你正在执行 push 操作，那么你的数组将会变满，不能再放入更多的元素，这是使用 stack with array 的最大缺点。当你使用链表时，它们会动态增加大小。

***我们将看到使用堆栈*** 的数据结构的实现

# 使用数组堆栈(列表)

现在，我们将用 python 实现代码，并使用 array for Stack 解释这些代码:

```
class Stack:
  def __init__(self):
    self.stack = []
```

在上面的代码中，我们创建了一个空堆栈。现在我们将在同一个类中添加其他 require 操作方法。

```
 def isEmpty(self):
    return self.stack == []

  def push(self,ele):
    self.stack.append(ele) def pop(self):
    if not self.isEmpty():
      return self.stack.pop()
    else:
      return -1
  def peek(self):
    if not self.isEmpty():
      return self.stack[-1]
    else:
      return -1
```

isEmpty 方法不检查堆栈是否为空，而 push 方法将一项追加到堆栈的末尾。Pop 将首先检查列表是否为空，如果列表为空，它将返回-1，如果不是，它将删除最后一个元素。Peek 将只显示最后一个元素。

现在我们将创建一个实例，并在控制台中使用上述方法:

```
s = Stack()while True:
  print("push")
  print("pop")
  print("peek")
  do = input("What action you want to perform?")
  if do == 'push':
    ele = int(input("Enter the element which you want to push"))
    s.push(ele)
  elif do == 'pop':
    ele = s.pop()
    if ele == -1:
      print("Stack is empty")
    else:
      print("Deleted element form Stack is = {0}".format(ele))
  elif do == 'peek':
    ele = s.peek()
    if ele == -1:
      print("Stack is empty")
    else:  
      print("Top of the stack element is = {}".format(ele)) else:
     break
```

在上面的代码中，我们要求用户输入，并根据他们的输入给他们操作。

# 使用链表堆栈

现在，我们将使用堆栈的链表来实现 python 中的代码及其解释:

```
class Node:
    def __init__(self,value):
        self.value = value
        self.next = Noneclass Stack:
    def __init__(self):
        self.head = None

    def push(self,value):
        if self.head is None:
            self.head = Node(value)
        else:
            new_Node = Node(value)
            new_Node.next = self.head
            self.head = new_Node

    def pop(self):
        if self.head is None:
            return -1
        else:
            poppedEle = self.head.value
            self.head = self.head.next
            return poppedEle
    def peek(self):
        if self.head is None:
            return -1
        else:
            peekEle = self.head.value
            return peekEle
s = Stack()while True:
    print("push")
    print("pop")
    print("peek")
    #print("display") if you wanna diplay your stack, travel the linked list.
    do = input("What action you want to perform")
    if do == 'push':
        ele = int(input("Enter the element you want to push"))
        s.push(ele)
    elif do == 'pop':
        ele = s.pop()
        if ele == -1:
            print("Stack is empty")
        else:
            print("Deleted element is = {0}".format(ele))
    elif do == 'peek':
        ele = s.peek()
        if ele == -1:
            print("Stack is empty")
        else:
            print("Peek element is : {}".format(ele))
    else:
        break
```

在上面的代码中，我们首先创建一个节点，然后是堆栈。在我们的堆栈中，我们执行所有需要的操作，并通过请求用户在控制台中发出命令来调用实例。

# 如何使用堆栈检查括号？

堆栈是检查等式是否正确的最好方法。假设一个表达式为{5+(5 *3 ) / 4}，这个等式是对的，因为它是用相等且正确的大括号顺序排列的。但是{5 + (5 *4} /4 不是一个正确的值，那么如何使用 stack 进行检查呢？

现在让我们为它编码:

```
class Parentheses: def __init__(self):
    self.stack = [] def __init__(self,exp):
    for i in range(len(exp)):
      if exp[i] == '(' or exp[i] == '{' or exp[i] == '[': 
        self.stack.append(exp[i])
        continue

      if len(self.stack) == 0:
        return False

      if exp[i] == '(':
        char = self.stack.pop()
        if char != ')':
          return False if exp[i] == '{':
        char = self.stack.pop()
        if char != '}':
          return False if exp[i] == '[':
        char = self.stack.pop()
        if char != ']':
          return False if len(self.stack):
      return False
    else:
      return True

p = Parentheses()
expre = input("Enter the expression")
if p.check(expre):
    print("Given expression is balanced")
else:
    print("Given expression is unbalanced")
```

在上面的代码中，我们从用户那里获取一个字符串表达式，然后检查我们的条件，你可以很容易地理解上面的代码。

# 来源

我的 LinkedIn:-[**linkedin.com/in/my-pro-file**](https://www.linkedin.com/in/my-pro-file)

# 您可能感兴趣的主题:

*   **链表:**[https://medium . com/swlh/all-about-single-Linked-List-d 69836814 ace](https://medium.com/swlh/all-about-singly-linked-list-d69836814ace)
*   **合并排序**:[https://medium.com/swlh/title-1692d9fb5ced](https://medium.com/swlh/title-1692d9fb5ced)
*   **插入排序**:[https://medium . com/dev-genius/Insertion-Sort-program-in-swift-31740 a 454573](https://medium.com/dev-genius/insertion-sort-program-in-swift-31740a454573)
*   **计数排序**:[https://medium . com/@ MD code 2021/Counting-Sort-algorithm-c 32d 71 F2 cc 79](https://medium.com/@mdcode2021/counting-sort-algorithm-c32d71f2cc79)
*   **选择排序**:[https://medium . com/@ MD code 2021/line-by-line-Selection-Sort-algorithm-explained-in-c-DD 49638 b15e](https://medium.com/@mdcode2021/line-by-line-selection-sort-algorithm-explained-in-c-dd49638b15e)