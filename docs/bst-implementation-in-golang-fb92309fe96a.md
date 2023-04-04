# Golang 中的 BST 实现

> 原文：<https://blog.devgenius.io/bst-implementation-in-golang-fb92309fe96a?source=collection_archive---------5----------------------->

![](img/c2452c1b3c06827d7bc151584fbb5edf.png)

# 定义

> **二叉查找树:** *一种二叉树，其中每个节点的左子树的关键字少于该节点的关键字，而每个右子树的关键字多于该节点的关键字。*
> 
> **二叉树:** *每个节点最多有两个孩子的树。*

# 结构体

结构是用户定义的数据类型，其中包含几个元素。就像许多其他语言一样。 ) struct 是 golang 中处理自定义类型的默认方式。

> ***节点:*** *由实际数据和指向左右后续节点的指针组成。*
> 
> ***BST:*** *它由根节点组成。*
> 
> BST 可以通过树的高度、树的级别和树中节点的数量等参数进一步优化…

```
type node struct {
    left  *node
    data  int
    right *node
}
type bst struct {
    root *node
}
```

# 服务人员

服务器可以与同类型关联的方法的实现进行比较。一般我们创建一个构造函数来初始化一个类型，类似于其他语言中的一个 ***new*** 关键字。

这避免了直接操纵数据，使变量成为私有变量。方法是唯一可用于操作与类型关联的数据的方法。

```
type BSTService interface {
    Insert(data int) *bst
    GetRoot() *node
    Search(key int) bool
}
```

# 方法

方法是在特定类型实例上调用的关联函数。方法“消耗”调用者对象的资源。

## 什么时候使用方法？

这很容易与类方法相比较。当需要对特定类型进行操作时，最好实现与该类型相关联的方法。

## 搜索

我们定义了两种搜索方法。这两种方法都绑定到不同的类型。一个是 *bst 型*一个是*节点型*。搜索方法检查根。如果根满足键，立即返回。否则，调用节点类型搜索在后续节点中查找键。

## 插入

插入方法类似于搜索。唯一不同的是，它不检查键的存在；它通过在树中创建一个新节点来将键添加到树中。

```
func (tree *bst) Insert(data int) *bst {
    if tree.root == nil {
        tree.root = &node{
            data: data,
        }
    } else {
        tree.root.insert(data)
    }
    return tree
}
func (nd *node) insert(data int) {
    if nd == nil {
        return
    } else if data < nd.data {
        if nd.left == nil {
            nd.left = &node{
                data: data,
            }
        } else {
            nd.left.insert(data)
        }
    } else {
        if nd.right == nil {
            nd.right = &node{
                data: data,
            }
        } else {
            nd.right.insert(data)
        }
    }
}
func (tree *bst) Search(key int) bool {
    var keyFound bool
    if tree.root == nil {
        keyFound = false
    } else if tree.root.data != key {
        keyFound = tree.root.search(key)
    } else {
        keyFound = true
    }
    return keyFound
}
func (nd *node) search(key int) bool {
    var keyFound bool
    if nd == nil {
        keyFound = false
    } else if key == nd.data {
        keyFound = true
    } else {
        if nd.data > key {
            keyFound = nd.left.search(key)
        } else {
            keyFound = nd.right.search(key)
        }
    }
    return keyFound
}
func (tree *bst) GetRoot() *node {
    return tree.root
}
```

# 功能

另一方面，函数是独立的，不绑定到任何类型。接受一个对象作为参数，并在其上进行计算。

## 何时使用函数？

这取决于你想如何组织你的程序。将函数与类型绑定会产生一个方法。但是当涉及到多种类型的操作时，方法是最少的。

函数是程序中任何时候都需要调用的东西。使它成为关联函数总是需要一个对象来进行函数调用。对于不需要保存在内存中的东西来说，这是多余的。

简单来说， ***输入- >处理- >输出***；就是一个函数是为**T5 制作的，在这里它只执行一个单一的任务** 。

```
func preOrderTraversal(root *node) {
    if root == nil {
        return
    }
    fmt.Print(root.data, " ")
    preOrderTraversal(root.left)
    preOrderTraversal(root.right)
}
func inOrderTraversal(root *node) {
    if root == nil {
        return
    }
    inOrderTraversal(root.left)
    fmt.Print(root.data, " ")
    inOrderTraversal(root.right)
}
func postOrderTraversal(root *node) {
    if root == nil {
        return
    }
    postOrderTraversal(root.left)
    postOrderTraversal(root.right)
    fmt.Print(root.data, " ")
}
func printTree(root *node, space int) {
    if root == nil {
        return
    }
    space += COUNT
    printTree(root.right, space)
    for i := COUNT; i < space; i++ {
        fmt.Printf(":")
    }
    fmt.Printf("[")
    fmt.Printf("%v", root.data)
    fmt.Println("]")
    printTree(root.left, space)
}
```

# 构造器

构造函数是初始化树对象的函数。

```
func NewBST() BSTService {
    return &bst{}
}
```

# 主要的

定义测试和验证二叉查找树的主函数。

```
package main
import "fmt"
const (
    COUNT int = 5
)
func main() {
    tree := NewBST()
    tree.Insert(10)
    tree.Insert(5)
    tree.Insert(15)
    tree.Insert(4)
    tree.Insert(16)
    tree.Insert(6)
    tree.Insert(13)
    tree.Insert(1)
    tree.Insert(12)
    tree.Insert(2)
    tree.Insert(20)
    printTree(tree.GetRoot(), 0)
    fmt.Printf("Pre-Order: ")
    preOrderTraversal(tree.GetRoot())
    fmt.Printf("\nIn-Order: ")
    inOrderTraversal(tree.GetRoot())
    fmt.Printf("\nPost-Order: ")
    postOrderTraversal(tree.GetRoot())
    fmt.Println()
    fmt.Println("20 Exists:", tree.Search(20))
    fmt.Println("21 Exists:", tree.Search(21))
}
/* Output
:::::::::::::::[20]
::::::::::[16]
:::::[15]
::::::::::[13]
:::::::::::::::[12]
[10]
::::::::::[6]
:::::[5]
::::::::::[4]
::::::::::::::::::::[2]
:::::::::::::::[1]
Pre-Order: 10 5 4 1 2 6 15 13 12 16 20 
In-Order: 1 2 4 5 6 10 12 13 15 16 20 
Post-Order: 2 1 4 6 5 12 13 20 16 15 10
20 Exists: true
21 Exists: false
*/
```

*希望这篇文章有助于对 golang 的树结构有一个基本的了解。如有错误或进一步建议，请在下方评论。谢谢！*