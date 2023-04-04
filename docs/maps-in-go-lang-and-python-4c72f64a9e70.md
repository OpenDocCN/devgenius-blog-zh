# 地图:在 Go-lang 和 Python 中。

> 原文：<https://blog.devgenius.io/maps-in-go-lang-and-python-4c72f64a9e70?source=collection_archive---------9----------------------->

[#python](https://dev.to/t/python) [#go](https://dev.to/t/go)

这将是我的“Python 和 Go-lang 系列中的数据结构”的第二篇文章。在我的第一篇文章中，我谈到了应用于 python 和 Go-lang 序列的单词 slice。只是重述一下这个话题。Slice 是一个内置的 python 函数，它返回一个 slice 对象，我们可以用参数“start”、“end”和“step”作为索引初始化该对象，然后我们可以使用这个 slice 对象从序列中“切片/提取”连续数量的元素。在 Go-lang 中，slice 是一种可变的数据结构，可以用来保存和管理相似类型的元素集合。今天我们来谈谈 Go-lang an Python 中的**地图**。

# 地图数据结构

在某些地方被称为关联数组或字典。它是许多编程语言中最重要的数据结构之一，它涉及到将唯一的键“映射”或“关联”到它们各自的值。
以下面的映射为例，使用关键字作为国家名称，使用其相关值作为该国家使用的货币单位:

肯尼亚= >先令
奥地利= >欧元
比利时= >欧元
智利= >比索
中国= >人民币
丹麦= >克朗
美国= >美元

如上图所示，映射的键应该是唯一的，而它们的值可以共享，例如上面的“奥地利”和“比利时”键都有类似的值“欧元”。
与标准数组不同，我们的地图索引不必是数字格式或连续的。例如，一个普通的数组应该是这样的:
countries =["肯尼亚"、"奥地利"、"比利时"、"智利"、"中国"、"丹麦"、"美国"]

在上面的例子中，数组中各个元素的索引是连续的数字。为了访问单个元素，我们使用如下数字索引:

国家[0] = "肯尼亚"
国家[1] = "奥地利"
国家[2] = "比利时"
国家[3] = "智利"
…..
国家[6] = "美洲"

PS:记住几乎所有语言中的数组都是零索引的。

# 然而，要访问我们的地图结构中的值…..

与标准数组不同，映射使用类似数组的语法进行索引，映射的索引(我使用 python 字典进行说明)不需要连续或数字。

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"]//to access the elements;  
currencies["Kenya"]   =  "Shilling"
currencies["Austria"] =  "Euro"
currencies["Belgium"] =  "Euro"
.....
currencies["China"]   =  "Yuan"
```

# 用 Python 和 Go-lang 创建地图

在 python 中，映射被实现为称为 dictionary 的抽象，其中唯一的键被映射到相关联的值:

语法为:identifier = {"key1":"value1 "，" key2":"value2 "，" key3":"value3"}

示例:

```
currencies = {
    "Kenya"  : "Shilling",
    "Austria": "Euro",
    "Belgium": "Euro",
    "Chile"  : "Peso",
    "China"  : "Yuan",
    "Denmark": "Krone",
    "America": "Dollar",
}
```

在 Go-lang 中，映射是键/值对的无序集合，基于使用键作为索引来快速检索数据的能力是映射作为数据存储的一个吸引人的特征:
它们的创建涉及下面的语法:

*   var identifier = map[key-type]value-type { " key 1 ":" value 1 "，" key2":"value2 "，" key3":"value"}

请注意，由于 Golang 严格的类型特性，我们在 map 实例化过程中显式地声明了键和值的类型。
举例:

```
package main
import "fmt"var currencies = map[string]string{"Kenya":"Shilling","Austria":"Euro", "Belgium":"Euro","Chile":"Peso","China":"Yuan","America":"Dollar"}
func main(){
    fmt.Println(currencies) 
}
```

*   var identifier = make(映射[键类型]值类型)

注意，在上面的语法中，我们使用 Go-lang 内置的 make 函数来创建一个 map 对象。

示例:

```
package main
import "fmt"func main(){
    var currencies = make(map[string]string)
    currencies["Kenya"]   = "Shilling"
    currencies["Austria"] = "Euro"
    currencies["Belgium"] = "Euro"
    currencies["Chile"]   = "Peso"
    currencies["China"]   = "Yuan"
    currencies["America"] = "Dollar" fmt.Println(currencies)    //will print the whole map
}
```

# 在 Go-lang 和 Python 地图中访问和赋值…

*   在 python 中，为了访问和分配地图的值，我们使用键作为地图的索引，语法是；标识符[键] =值

例如使用我们上面创建的货币地图

```
print(currencies["Kenya"])    // will print "shilling" value
print(currencies["Austria"])  // will print "Euro" value
print(currencies["Belgium"])  // will print "Euro" value
print(currencies["Chile"])    // will print "Peso" value
print(currencies["Chine"])    // Will print "Yuan" value
print(currencies["America"])  // will print "Dollar" value
```

*   在 Go-lang 中，我们使用类似的索引结构来访问地图中的单个元素，语法是:identifier[key] = value

示例:

```
package main
import "fmt"func main(){
    var currencies = make(map[string]string)
    currencies["Kenya"]   = "Shilling"
    currencies["Austria"] = "Euro"
    currencies["Belgium"] = "Euro"
    currencies["Chile"]   = "Peso" fmt.Println(currencies["Kenya"])
    fmt.Println(currencies["Austria"])
    fmt.Println(currencies["Belgium"])
    fmt.Println(currencies["Chile"])
}
```

# 在 Go-lang 和 Python 中确定地图长度

*   在 python 中，map 类实现了一个 **len** ()函数，它计算键值对的数量

语法是，len(标识符)

例子

```
currencies = {
    "Kenya"  : "Shilling",
    "Austria": "Euro",
    "Belgium": "Euro",
    "Chile"  : "Peso",
    "China"  : "Yuan",
    "Denmark": "Krone",
    "America": "Dollar",
}print len(currencies)   // will print out the value 7(number of currency key-value pairs)
```

*   在 Go-lang 中，我们坚持使用与 python 相同的语法，看起来像 len(标识符)

例子

```
package main
import "fmt"func main(){
    var currencies = make(map[string]string)
    currencies["Kenya"]   = "Shilling"
    currencies["Austria"] = "Euro"
    currencies["Belgium"] = "Euro"
    currencies["Chile"]   = "Peso"
    currencies["China"]   = "Yuan"
    currencies["America"] = "Dollar" fmt.Println(len(currencies))    //will print length of the whole map
}
```

# 在 Go-lang 和 Python 中向地图添加项目…

*   在 python 中，要向现有的地图对象添加一个键-值对，我们使用以下语法:identifier[" new-key "]= " new-value "

例子

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}//To add Germany Franc key value pair to the above map
currencies["Germany"] = "Franc"print(currencies)
```

打印语句将输出:

```
{"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan", "Germany":"Franc"}
```

*   在 Go-lang 中，我们引入了一个新的索引键并为其赋值，语法是:identifier[" new-key "]= " new-value "

```
Example:
package main
import "fmt"func main(){
    var currencies = map[string]string{"Kenya":"Shilling","Austria":"Euro","Belgium":"Euro"}
    fmt.Println(currencies)  //initial Map
    currencies["Chile"] = "Peso"  // add chile-peso key-value pair
    currencies["China"] = "Yuan"
    currencies["China"] = "Yuan"
}
```

# 在 Python 和 Go-lang 中更新现有地图中的值…

*   在 Python 中，可以通过引用键名来更新特定项的值，语法是]identifier[" old-key "]= " new-value "

示例:

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}// To update the Belgium item value to "dollar"
print(currencies)   // Initial map
currencies["Belgium"] = "Dollar"
print(currencies)   // Updated map
```

*   在 Golang 中，我们遵循与上面的 Python 类似的路径

例子

```
package main
import "fmt"func main(){
    var currencies = map[string]string{"Kenya":"Shilling","Austria:"Euro","Belgium":"Euro","Chile":"Peso","China":"Yuan"}
    fmt.Println(currencies)  // Initial map
    //updating the value of the Belgium key
    currencies["Belgium"] = "Dollar"
    fmt.Println(currencies)
}
```

# 正在删除 Go-lang 和 Python 地图中的项目…

*   为了在 Python 中删除一个条目，我们使用内置的 del 函数，语法如下:del(标识["key"])

示例:

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}
print(currencies) // initial map
del(currencies["Belgium"])
print(currencies)  // Map with Belgium deleted
```

-在 python Map 中，我们可以使用内置的 pop 函数来删除项目

语法；identifier.pop("Key ")

示例:

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}
print(currencies) // initial Map
// to delete the Belgium item
currencies.pop("Belgium")
print(currencies)  // Map with the Belgium item deleted
```

*   在 Go-lang 中，内置的 delete 函数从给定的 map 中删除一个与提供的键相关联的项目

例子

```
package main
import "fmt"func main() {
    var currencies = make(map[string]string)
    currencies["Kenya"] = "Shilling"
    currencies["Austria"] = "Euro"
    currencies["Belgium"] = "Euro"
    currencies["Chile"]   = "Peso"
    currencies["China"]   = "Yuan" fmt.Println(currencies)
     delete(currencies, "Belgium")
     fmt.Println(currencies)
}
```

# 用 Go-lang 和 Python 迭代地图

-在 Python 中，我们可以使用 for 循环来迭代映射，使用语法:
表示键，值在范围内(identifier.items())，迭代键，值对
表示键在范围内(identifier.keys())，迭代映射中的键
表示值在范围内(identifier.value())，迭代映射的值

示例:

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}for key,value in range(currencies.items()):
    print(key, ":" ,value)//to print only the keys
for key in range(currencies.keys()):
    print(key)//to print only the values
for value in range(currencies.values()):
    print(value)
```

*   在戈朗森林里..range loop 语句可用于获取映射的索引和元素:key，value 的语法:= range identifier {//do something }

示例:

```
package main
import "fmt"func main() {
    var currencies = map[string]string{"Kenya":"Shilling","Austria":"Euro","Belgium":"Euro","Chile":"Peso","China":"yuan","Amerika":"Dollars"}for key,value := range currencies {
    fmt.Println("Key: ", key, "=>", "Value: ", value)
}
}
```

# Golang 和 Python 中映射键和值的排序…

*   在 Go-lang 中，我们可以对地图的键值进行如下排序:创建一个 keys 切片来存储 map 的键值，然后对切片进行排序。已排序的切片用于按关键字顺序打印地图值。

例子

```
Package main
import (
    "fmt"
    "sort"
)func main() {
    unSortedCurrencies := map[string]string{"Kenya":"Shilling", "Austria":"Euro","Belgium":"Euro","Chile":"Peso","China":"yuan","America":"Dollar"}keys := make([]string, 0, len(unSortedCurrencies))
for k := range unSortedCurrenciesMap{
    keys = append(keys, k)
}
sort.Strings(keys)
for _, k := range keys {
    fmt.Println(k, unSortedCurrencies[k])
}}
```

对地图值进行排序

```
Package main
import (
    "fmt"
    "sort"
)func main() {
    unSortedCurrencies := map[string]string{"Kenya":"Shilling", "Austria":"Euro","Belgium":"Euro","Chile":"Peso","China":"yuan","America":"Dollar"}values := make([]string, 0, len(unSortedCurrencies))
for _,v := range unSortedCurrenciesMap{
    values = append(values, v)
}
//sort slice values
sort.String(values)//Print values of sorted Slice
for _, v := range values {
    fmt.Println(v)
}
}
```

*   在 Python 中，我们可以对地图进行如下排序

为了对键进行排序，我们使用了内置的排序函数，
语法:sorted(identifier)

示例:

```
currencies = {"Kenya":"Shilling", "Austria":"Euro", "Belgium":"Euro",Chile:"Peso","China":"Yuan"}sortedCurrencyKeys = sorted(currencies)
print(sortedCurrencyKeys)
```

要对映射的值进行排序，我们可以使用以下结构

货币= {“肯尼亚”:100，“奥地利”:200，“比利时”:110，智利:150，“中国”:98}

sorted currency = { k:v for k，v in sorted(currences . items()，key=lambda item: item[1])}

打印(分类货币)

# 合并两个或多个地图…

*   在 Go-lang 中，我们可以合并两个地图，如下例所示

```
package main
import "fmt"func main() {
    firstCurrencies = map[string]int{"Kenya":1,"Belgium":2,"China":3}
secondCurrencies = map[string]int{"Austria":2,"Belgium":4,"Chile":6, "America":7}for key, value := range secondCurrencies {
    firstCurrencies[key] = value
}fmt.Println(firstCurrencies)
}
```

-在 python 中，我们可以使用 update()方法和(**)运算符来合并字典。

例子

```
firstCurrencies = {"Kenya":"Shilling","Austria":"Euro","Belgium":"Euro"}
secondCurrencies = {"Austria":"Dollar","Chile":"Peso","China":"Yuan"}secondCurrencies.update(firstCurrencies)
print(secondCurrencies)
```

使用**是一个技巧，其中单个表达式用于合并两个字典并存储在第三个字典中

```
firstCurrencies = {"Kenya":"Shilling","Austria":"Euro","Belgium":"Euro"}
secondCurrencies = {"Austria":"Dollar","Chile":"Peso","China":"Yuan"}combinedCurrencies = {**firstCurrencies, **secondCurrencies}
print(combinedCurrencies)
```

# 结论

我们设法用 Go-lang 和 Python 查看地图，询问诸如访问和添加项目、遍历项目、删除项目、合并地图之类的事情。我将会不断更新这篇文章，把我忽略的任何信息放到未来。谢谢你。回头见。