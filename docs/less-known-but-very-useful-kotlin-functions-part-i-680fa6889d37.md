# 鲜为人知但非常有用的科特林函数[第一部分]

> 原文：<https://blog.devgenius.io/less-known-but-very-useful-kotlin-functions-part-i-680fa6889d37?source=collection_archive---------2----------------------->

![](img/c3c06bd0502b28a4fe74e3c87e4e81e6.png)

下面是一些可以让你的程序员生活变得容易得多的函数，因为几乎所有的程序员都经常找到这些函数的用例，但因为他们不知道它们的存在，所以他们采取了困难的方式，这通常容易出错，冗长，耗时，并且代码混乱(不，这不是一个词)😜

下面所有的函数都作用于集合，如果你通过在代码中引入循环和条件来使用集合，你有可能弄乱索引，导致**IndexOutOfBoundsException**或者你可能从错误的索引中获取错误的数据，或者你可能使用了错误的条件等等。

**记住:**

> 代码越少，错误越少
> 
> 代码越少，维护越少
> 
> cod 越少，消耗的时间越少
> 
> 代码越少越好

# 多好的一条线啊😻

# **查找()**

`find()`用于在集合中查找一个元素，如果没有找到元素，它使用一个谓词返回与条件`null`匹配的第一个元素。

例如，您有一个用户列表，您想查找 id 为

***没有*** `***find()***`

```
fun findUser(id:Int):User?{
 for(i in 0..users.size - 1){
     if(users[i].id == id){
        return users[i]
     }
 }
 return null
}
```

***同*** `***find()***`

`val user = users.find{ it == id}`

# 过滤器()

`filter()`顾名思义就是根据一个条件过滤集合中的一些元素，如果没有元素匹配该条件则返回空列表，否则过滤列表

例如，您想从用户列表中找到有阅读爱好的用户

***不带*** `***filter()***`

```
fun getReadingUsers():List<User>{
 val readingUsers = mutableListOf<User>()
 for(i in 0..users.size - 1){
     if(users[i].hobby == "reading"){
        readingUsers.add(users[i])
     }
 }
 return readingUsers
}
```

***同*** `***filter()***`

`val hobbyUsers = users.filter{it.hobby == “reading”}`

# filterNot()

filter not 与`filter()`正好相反，它将从一个列表中过滤所有不符合给定条件的元素。

例如，在一个游戏中，你想把价格发给获胜的用户，这样你就能找到赢家

***不带*** `***filterNot()***`

```
fun getWinners():List<User>{
 val winners = mutableListOf<User>()
 for(i in 0..users.size - 1){
     if(users[i].won == true){
        winners.add(users[i])
     }
 }
 return winners
}
```

***同*** `***filterNot()***`

`val winners = users.filterNot{it.won == false}`

# 排序依据()

`sortBy()`用于根据标准或属性对集合进行排序

例如，从一个玩家列表中，你想根据他们的分数对他们进行排序

***不带*** `***sortBy()***`

```
fun sortByScore()  {
    for(i in 0..*players*.size - 2){
        for(j in 0..*players*.size - i - 2){
            if(*players*[j].score > *players*[j + 1].score){
                val temp = *players*[j]
                *players*[j] = *players*[j + 1]
                *players*[j + 1] = temp
            }
        }
    }
}
```

***同*** `***sortBy()***`

`players.sortBy{it.score}`

# 索引 0f()

的索引用于返回列表中对象的第一个索引的匹配项。

例如，你有一个球员名单，你想更新一个特定球员的分数

***没有*** `***index0f()***`

```
fun updateScore(player: User, newScore: Int) {
    for (i in 0..*players*.size - 1) {
        if (*players*[i] == player) {
            *players*[i].score = newScore
            break
        }
    }
}
```

***同*** `***indexOf()***`

`val index = players.indexOf(myPlayers)`

`players[index].scrore = newScore`

在标准库中还有许多与上述函数同名的兄弟函数，我没有介绍，但是我鼓励你也去看看

[](https://medium.com/@thegoldycopythat/less-known-but-very-useful-kotlin-functions-part-ii-673c3457f5a2) [## 鲜为人知但非常有用的科特林函数[第二部分]

### 虽然这篇文章是独立的，但如果你还没有阅读[第一部分],我会鼓励你去读一读，因为他们的…

medium.com](https://medium.com/@thegoldycopythat/less-known-but-very-useful-kotlin-functions-part-ii-673c3457f5a2) 

谢谢，祝你愉快😊