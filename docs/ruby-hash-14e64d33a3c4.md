# Ruby 哈希

> 原文：<https://blog.devgenius.io/ruby-hash-14e64d33a3c4?source=collection_archive---------3----------------------->

# 概观

ruby 中的 hash 是一个键值对，类似于 python 中的字典。它像字典一样无序、多变、有索引。键和值没有以任何特定的顺序存储，您可以更改键的值，并且它是索引的，因为每个键都有一个值，并且通过传递键值来访问它。

# 新散列

创建散列有不同的方法。一种方法是使用构造函数并传入单个值或不传入值。如果你传递一个值给构造函数，缺省值将是键不存在时传递的值。

```
hash = Hash.new("hello")
puts hash[0]
>> hello
puts hash["a"]
>> hello
```

要用现有的键和值创建一个散列，你可以使用散列和括号，或者只使用花括号。

```
hash = Hash["a"=>1, "b"=>2]
hash2 = {"a"=>1, "b"=>2}
puts hash["a"]
>> 1
puts hash["b"]
>> 2
puts hash2["a"]
>> 1
puts hash2["b"]
>> 2
puts hash == hash2
>> true
```

# 阅读

要从 hash 中访问值，需要将键传递给方括号内的 hash。

```
hash = {"a"=>1, "b"=>2}
puts hash["a"]
>> 1
puts hash["b"]
>> 2
```

# 更新

要添加新的键值对或更新现有的键，您需要访问该键并为其提供一个值。

```
hash = {"a"=>1, "b"=>2}
hash["a"] = 3
hash["c"] = 4
puts hash["a"]
>> 3
puts hash["c"]
>> 4
```

# 循环

要遍历散列的所有键和值，可以使用 each 方法。

```
hash = {"a"=>1, "b"=>2}
hash.each do |key,value|
    puts "#{key}-#{value}"
end
>> a-1
>> b-2
```

# 长度

要获得散列的长度，可以使用 length 方法。

```
hash = {"a"=>1, "b"=>2}
puts hash.length
>> 2
```

# 键和值

要获得散列中的所有键，可以使用 keys 方法获得所有键的数组。

```
hash = {"a"=>1, "b"=>2}
puts hash.keys
>> a
>> b
```

类似地，要获得散列中的所有值，可以使用 values 方法来获得所有值的数组。

```
hash = {"a"=>1, "b"=>2}
puts hash.values
>> 1
>> 2
```

# 删除

要删除散列中的键和值，可以使用 delete 方法并向其传递键。如果该项不存在，则不会引发错误。如果键值存在，它将返回键值。

```
hash = {"a"=>1, "b"=>2}
puts hash.delete("c")
>>
puts hash.delete("a")
>> 1
puts hash["a"]
>>
```

# 复制

要获得散列的副本，可以使用 dup 或 clone 方法。

```
hash = {"a"=>1, "b"=>2}
hash2 = hash.dup
hash3 = hash.clone
puts hash == hash2
>> true
puts hash == hash3
>> true
puts hash2 == hash3
>> true
```

# 结论

Hash 类似于 Pythons 字典和 Javas Hashmap，两者都支持键值对，没有重复的键，是无序的、可变的和有索引的。当您想要将某些值与键相关联时，可以使用 hash。