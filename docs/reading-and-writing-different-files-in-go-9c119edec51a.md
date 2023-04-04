# 在 Go 中读写不同的文件

> 原文：<https://blog.devgenius.io/reading-and-writing-different-files-in-go-9c119edec51a?source=collection_archive---------6----------------------->

读写文件是程序的一个重要特性。并非所有的数据都存储在与程序相同的内存空间中，有时您需要与其他程序共享数据，或者以后用不同的程序查看数据。将数据存储在文件中是实现这些目标的好方法。今天，我们将看看如何读写常用的文件类型。

# io。读者，木卫一。作者

你有没有想过围棋怎么会读写这么多不同的东西？这都要归功于这些强大的接口。`io.Reader`描述的是`Read`的任何东西，`io.Writer`描述的是`Write`的任何东西。因为这种行为很容易复制，所以任何实现了`io.Reader`和`io.Writer`接口的东西都可以用于 I/O 操作。这意味着您可以即插即用不同的输入和输出。您可以从 CSV 文件读取并将其输出到 JSON，或者您可以从`stdin`读取并写入 CSV。

# 战斗支援车

CSV 文件由数据行组成，其中每个值由逗号分隔(因此得名逗号分隔值)。CSV 不是读写速度最快的格式，但是它非常通用，可以被许多其他工具理解。

## 阅读

```
package mainimport (
    "encoding/csv"
    "fmt"
    "os"
)func main() {
    f, err := os.Open("see-es-vee.csv")
    if err != nil {
        fmt.Println(err)
    }
    defer f.Close() r := csv.NewReader(f) records, err := r.ReadAll()
    if err != nil {
        fmt.Println(err)
    } fmt.Println(records)
}[[id fruit color taste] [0 apple red sweet] [1 banana yellow sweet] [2 lemon yellow sour] [3 grapefruit red sour]]
```

这里有一个从 CSV 文件中读取数据的非常简单的方法。

*   我们首先打开我们的文件`see-es-vee.csv`，并将实例保存为`f`。
*   我们想在完成后关闭`f`。这是节省内存的好方法。虽然这是一个很短的程序，而且关闭并不是必须的，但是养成这样做的习惯是有好处的。
*   `f`属于`*os.File`类型，它实现了`io.Reader`接口。所以我们可以通过`f`到`csv.NewReader`。
*   这会返回一个`csv.Reader`对象`r`。`csv.Reader`是`io.Reader`的一种，专门读取 CSV 文件。
*   CSV 文件的每一行被称为一个*记录*。因此，我们可以把 CSV 文件看作是记录的切片。`r.ReadAll`返回这段记录。
*   如果我们打印`records`，我们可以看到 2D 切片的行。

这很好，但是如果我们想在阅读时对每个记录应用一些操作呢？幸运的是，我们可以采用更细粒度的方法。

```
func main() {
    f, err := os.Open("see-es-vee.csv")
    if err != nil {
        fmt.Println(err)
    }
    defer f.Close() r := csv.NewReader(f) for {
        record, err := r.Read()
        if err != io.EOF {
            break
        }
        if err != nil {
            fmt.Println(err)
        } fmt.Println(record)
    }
}
```

看起来和上面差不多吧？直到创建`r`的部分是相同的。让我们看看接下来会发生什么。

*   我们进入一个无限 for 循环，因为我们要逐行读取文件，而 Go 不知道文件有多长。
*   我们使用`r.Read`读取每条记录。
*   我们如何知道我们是否到达了文件的末尾？`r.Read`返回一个名为`io.EOF`的特殊错误。EOF 代表*文件结束*。我们首先捕获这个错误，并告诉我们的程序一旦到达末尾就退出 for 循环。
*   对于任何其他错误，您可以像平常一样处理它们。
*   在我们处理错误之后，我们可以对提取的记录做任何我们想做的事情。我能想到的一些想法是大写，舍入，将其与任意值进行比较等。

现在让我们看看如何写入 CSV 文件。

```
package mainimport (
    "encoding/csv"
    "os"
)func main() {
    f, err := os.Create("output.csv")
    if err != nil {
        panic(err)
    }
    defer f.Close() w := csv.NewWriter(f) records := [][]string{
        {"id", "fruit", "color", "taste"},
        {"0", "apple", "red", "sweet"},
        {"1", "banana", "yellow", "sweet"},
        {"2", "lemon", "yellow", "sour"},
        {"3", "grapefruit", "red", "sour"},
    } w.WriteAll(records)
}id,fruit,color,taste
0,apple,red,sweet
1,banana,yellow,sweet
2,lemon,yellow,sour
3,grapefruit,red,sour
```

非常简单明了。

*   我们首先需要创建一个可以将数据转储到其中的文件。我们将称之为`output.csv`。我们只是调用`os.Create`，并将它的一个实例保存为`f`。
*   还记得我们之前是如何创建一个`csv.Reader`的吗？这里我们正在创建一个`csv.Writer`对象`w`。`csv.NewWriter`接受`io.Writer`接口，由`f`实现。
*   我们把数据准备成字符串的 2D 片段。我们称之为`records`。
*   最后我们只是用`w.WriteAll`写`records`到`output.csv`。

# JSON

因为 Go 在 web 服务中被广泛使用，所以它对 JSON 有强大的支持。我写了一整篇关于阅读和编写 JSON 文件的文章，所以请查看这篇文章以获得更深入的指导！

## 阅读

```
package mainimport (
    "encoding/json"
    "fmt"
    "os"
)type fruit struct {
    Id    int    `json:"id"`
    Fruit string `json:"fruit"`
    Color string `json:"color"`
    Taste string `json:"taste"`
}func main() {
    f, err := os.Open("jay-son.json")
    if err != nil {
        fmt.Println(err)
    }
    defer f.Close() dec := json.NewDecoder(f) // read opening bracket
    _, err = dec.Token()
    if err != nil {
        fmt.Println(err)
    } for dec.More() {
        var fr fruit
        err := dec.Decode(&fr)
        if err != nil {
            fmt.Println(err)
        } fmt.Println(fr)
    } // read closing bracket
    _, err = dec.Token()
    if err != nil {
        fmt.Println(err)
    }
}{0 apple red sweet}
{1 banana yellow sweet}
{2 lemon yellow sour}
{3 grapefruit red sour}// jay-son.json
[
    {"id": 0, "fruit": "apple", "color": "red", "taste": "sweet"},
    {"id": 1, "fruit": "banana", "color": "yellow", "taste": "sweet"},
    {"id": 2, "fruit": "lemon", "color": "yellow", "taste": "sour"},
    {"id": 3, "fruit": "grapefruit", "color": "red", "taste": "sour"}
]
```

这里有一个读取 JSON 文件的简单方法。通常，JSONs 是以流的形式出现的。也就是说，它们出现在对象列表中。Go 通过解码器处理数据流。

我们先来看看这个:

```
type fruit struct {
    Id    int    `json:"id"`
    Fruit string `json:"fruit"`
    Color string `json:"color"`
    Taste string `json:"taste"`
}
```

这个结构作为一个模型。因为 JSON 可以有各种形状和大小，所以理想情况下，您希望有一个反映 JSON 结构的模型。struct 字段需要是公共的，并且应该在右边有一个标记，表示它所引用的键。如果你事先不知道 JSON 会是什么样子，Go 只会用一个`map[string]interface{}`。

*   首先，我们打开文件并将它的实例保存到`f`。不要忘记推迟来电，以便稍后关闭`f`！
*   我们使用`json.NewDecoder`创建我们的解码器对象`dec`。就像`csv.NewReader`一样，它接受一个`io.Reader`。您可以开始看到接口的力量——读取的细节被抽象出来，并且读取的工作流在如此多的不同文件类型中是一致的。
*   一旦`dec`被创建，我们就可以读取我们的 JSON 了。不过，只有一个问题。我们需要确保使用`dec.Token`来捕捉开括号和闭括号。不这样做就像试图吃一个包装纸还在的赛百味三明治。糟糕。
*   就像我们一行一行地读取 CSV 文件一样，我们一个对象一个对象地读取 JSON 流。我们循环遍历`dec.More`，只要还有对象需要读取，就会返回`true`。
*   我们创建了一个`fruit`的实例来存储我们对象的数据。使用`dec.Decode`将对象数据转储到`f`。你现在可以做你想做的事了。
*   读完之后，一定要抓住右括号。

## 写作

写入 JSON 文件也很简单。我们称之为*编码*。

```
package mainimport (
    "encoding/json"
    "fmt"
    "os"
)type Fruit struct {
    Id    int    `json:"id"`
    Fruit string `json:"fruit"`
    Color string `json:"color"`
    Taste string `json:"taste"`
}func main() {
    f, err := os.Create("output.json")
    if err != nil {
        panic(err)
    }
    defer f.Close() enc := json.NewEncoder(f) apple := Fruit{Id: 0, Fruit: "apple", Color: "red", Taste: "sweet"}
    banana := Fruit{Id: 1, Fruit: "banana", Color: "yellow", Taste: "sweet"}
    lemon := Fruit{Id: 2, Fruit: "lemon", Color: "yellow", Taste: "sour"}
    grapefruit := Fruit{Id: 3, Fruit: "grapefruit", Color: "red", Taste: "sour"} fruits := []Fruit{apple, banana, lemon, grapefruit} err = enc.Encode(fruits)
    if err != nil {
        fmt.Println(err)
    }
}[{"id":0,"fruit":"apple","color":"red","taste":"sweet"},{"id":1,"fruit":"banana","color":"yellow","taste":"sweet"},{"id":2,"fruit":"lemon","color":"yellow","taste":"sour"},{"id":3,"fruit":"grapefruit","color":"red","taste":"sour"}]
```

这个也很简单。

*   我们创建一个名为`output.json`的文件，将数据转储到其中。
*   我们通过使用`json.NewEncoder`创建一个新的编码器`enc`。
*   我们准备我们的数据，它是`Fruit`对象的一部分。
*   最后，我们使用`enc.Encode`对这个切片进行编码。

# 擅长

Go 默认不支持读写 Excel 文件。然而，有一个叫做`qax-os/excelize`的流行库可以帮助你做到这一点。如果把源代码拆开，可以看到这个包大量使用了`*os.File`，也是一个`io.Reader`和`io.Writer`。我认为这展示了`io.Reader`和`io.Writer`接口的美妙之处，因为稍加调整，你就可以定制实现这些接口的阅读器和编写器，允许你支持更多的文件类型。

# 结论

希望这篇文章可以快速介绍在 Go 中读写文件，以及`io.Reader`和`io.Writer`接口有多强大。我认为这是 Go 的魅力之一——界面允许非常灵活和可重用的代码。当然，还有更多文件我们没有在这篇文章中讨论，但是总的要点是一样的:打开文件，创建一个阅读器或写入器，并从中读/写。谢谢大家！

你也可以在 [Dev.to](https://dev.to/jpoly1219/reading-and-writing-different-files-in-go-4m3i) 和[我的个人网站](https://jpoly1219.github.io)上阅读这篇文章。