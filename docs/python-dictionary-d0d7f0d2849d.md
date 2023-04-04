# Python 词典

> 原文：<https://blog.devgenius.io/python-dictionary-d0d7f0d2849d?source=collection_archive---------5----------------------->

# 概观

python 中的字典是无序的、可变的和索引的。它是无序的，因为它不跟踪插入数据的顺序。它是可变的，因为你可以改变键值。它被索引是因为它基于键检索数据，这意味着每个键只有一个值。

# 新词典

要创建一个字典，你可以使用花括号或者 dict()构造函数。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}otherDictionary = dict(name="Bob", age=21, email="bob@emai.com")
```

可以将空字符串和 None 值作为键和值。不可能有重复的密钥。

# 阅读

要从字典中获取值，您需要在方括号中提供键，或者使用 get()方法

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}print(dictionary["name"])
>> Bob
print(dictionary.get("age"))
>> 21
```

# 更新

要更改键的值，您需要像引用列表索引那样引用键。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dictionary["name"] = "Mike"
print(dictionary["name"])
>> Mike
```

# 循环

您可以使用 for 循环遍历键、值或两者。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}for key in dictionary:
  print(key)
>> name
>> age
>> emailfor key in dictionary:
  print(dictionary[key])
>> Bob
>> 21
>> bob@email.comfor key, value in dictionary.items:
  print(key, value)
>> name Bob
>> age 21
>> email bob@email.com
```

# 长度

要检查字典的长度，可以使用 len()方法

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
print(len(dictionary))
>> 3
```

# 创造

要在现有字典中创建新的键和值，可以引用新的键并给它一个值。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dictionary["job"] = "Teacher"
```

# 删除

要删除一个键和值，您可以通过提供键来使用 pop()方法。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dictionary.pop("email")
print(dictionary)
>> {'age': 21, 'age': 21}
```

在 python 版和更高版本中，可以使用 popitem()方法删除最新的键和值。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dictionary["job"] = "teacher"
dictionary.popitem()
print(dictionary)
>> {'age': 21, 'name': 'Bob', 'email': 'bob@email.com'}
```

要清除整个字典但保留数据结构，可以使用 clear()方法。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dictionary.clear()
print(dictionary)
>> {}
```

# 复制

要创建字典的副本，可以使用 copy 方法，或者将 copy 方法传递给 dict()构造函数。

```
dictionary = {
  "name": "Bob",
  "age": 21,
  "email": "bob@email.com"
}
dict1 = dictionary.copy()
dict2 = dict(dictionary)
```

# 嵌套词典

通过将一个键和另一个字典作为值，可以在一个字典中包含一个字典。

```
students = {
  "One" : {
    "name": "Bob",
    "age": "10"
  }, 
  "Two" : {
    "name": "Jack",
    "age": "9"
  },
}
```

# 结论

Pythons 字典类似于 Javas Hashmap，两者都支持键值对，没有重复的键，是无序的、可变的和有索引的。当您想要将某些值与键相关联时，可以使用字典。因为字典是散列的，所以存储和检索值比有序列表更快。