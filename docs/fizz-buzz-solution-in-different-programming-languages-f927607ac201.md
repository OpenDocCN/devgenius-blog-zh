# 不同编程语言的 Fizz Buzz 解决方案

> 原文：<https://blog.devgenius.io/fizz-buzz-solution-in-different-programming-languages-f927607ac201?source=collection_archive---------2----------------------->

## 21 种不同的编程语言

![](img/02f2040214790115579d355a64d5d7a0.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由 [Silvana Carlos](https://unsplash.com/@silvana_carlos?utm_source=medium&utm_medium=referral) 拍摄的照片

# 什么是嘶嘶声测试？

> *“嘶嘶嗡测试”是一个面试问题，旨在帮助筛选出 99.5%的编程求职者，他们似乎无法从湿纸袋中走出自己的编程之路。(*[](http://wiki.c2.com/?FizzBuzzTest)**)**

**解决方案:**

> *写一个程序打印从 1 到 100 的数字。但是对于三的倍数打印“嘶嘶”而不是数字，对于五的倍数打印“嗡嗡”。对于 3 和 5 的倍数的数字，打印“嘶嘶”*

**结果示例:**

```
*1, 2, Fizz, 4, Buzz, Fizz, 7, 8, Fizz, Buzz, 11, Fizz, 13, 14, FizzBuzz...*
```

## *Java Script 语言*

```
*for (var i = 1; i <= 100; i++) {
  var f = i % 3 == 0, b = i % 5 == 0;
  if (i % 15 === 0) {
    console.log("FizzBuzz")
  }
  else if (i % 3 === 0) {
    console.log("Fizz");
  }
  else if (i % 5 === 0) {
    console.log("Buzz")
  }
  else {
    console.log(i);
  }
}*
```

## *计算机编程语言*

```
*for fizzbuzz in range(100):
    if fizzbuzz % 15 == 0
        print("fizzbuzz")
        continue
    elif fizzbuzz % 3 == 0:
        print("fizz")
        continue
    elif fizzbuzz % 5 == 0:
        print("buzz")
        continue
    print(fizzbuzz)*
```

## *C#*

```
*for (int i = 1; i <= 100; i++)
{
        bool fizz = i % 3 == 0;
        bool buzz = i % 5 == 0;
        if (fizz && buzz)
            Console.WriteLine ("FizzBuzz");
        else if (fizz)
            Console.WriteLine ("Fizz");
        else if (buzz)
            Console.WriteLine ("Buzz");
        else
            Console.WriteLine (i);
}*
```

## *F#*

```
*[1..100] 
|> Seq.map (function
    | x when x%15=0 -> "FizzBuzz"
    | x when x%3=0 -> "Fizz"
    | x when x%5=0 -> "Buzz"
    | x -> string x)
|> Seq.iter (printfn "%s")*
```

## *GO (GoLang)*

```
*for i = 1; i <= 100; i++
    if i % 15 == 0
        print "FizzBuzz"
    else if i % 3 == 0
        print "Fizz"
    else if i % 5 == 0
        print "Buzz"
    else
        print i*
```

## *红宝石*

```
*1.upto(100) do |i|
      if i % 15 == 0
        puts "FizzBuzz"
      elsif i % 5 == 0
        puts "Buzz"
      elsif i % 3 == 0
        puts "Fizz"
      else
        puts i
      end
    end*
```

## *迅速发生的*

```
*for i in 1...100
{
    if i % 3 == 0 {
        if i % 5 == 0 {
            print("FizzBuzz")
        } else {
            print("Fizz")    
        }
    } else if i % 5 == 0 {
        print("Buzz")
    } else {
        print(i)
    }
}*
```

## *C++*

```
*int i;
    for (i=1; i<=100; i++)
    {
        if (i%15 == 0)        
            printf ("FizzBuzz\t");    

        else if ((i%3) == 0)    
            printf("Fizz\t");                 

        else if ((i%5) == 0)                       
            printf("Buzz\t");                 

        else         
            printf("%d\t", i);                 
   }*
```

## *Java 语言(一种计算机语言，尤用于创建网站)*

```
*int n = 100;
for (int i=1; i<=n; i++)                                 
{
    if (i%15==0)                                                 
       System.out.print("FizzBuzz"+" "); 

    else if (i%5==0)     
        System.out.print("Buzz"+" "); 

    else if (i%3==0)     
        System.out.print("Fizz"+" ");

     else
        System.out.print(i+" ");                         
 }*
```

## *斯卡拉*

```
*for (i <- 1 to 100) {
  if (i % 15 == 0) 
    println("FizzBuzz")
  else if (i % 3 == 0) 
    println("Fizz")
  else if (i % 5 == 0) 
    println("Buzz")
  else
    println(i)
}*
```

## *尝试*

```
*for ((i=1;i<=100;i++)); do
    if ! ((i%15)); then
        echo FizzBuzz
    elif ! ((i%3)); then
        echo Fizz
    elif ! ((i%5)); then
        echo Buzz
    else
        echo $i
    fi;
done*
```

## *C*

```
*for each number 1 to 100:
    if number % 15 == 0:
        print number, "fizzbuzz"
    else if number % 5 == 0:
        print number, "buzz"
    else if number % 3 == 0:
        print number, "fizz"
    else:
        print number*
```

## *哈斯克尔*

```
*fizz **::** Int **->** String
fizz n | n `mod` 15 == 0  **=** "FizzBuzz"
       | n `mod` 3  == 0  **=** "Fizz"
       | n `mod` 5  == 0  **=** "Buzz"
       | otherwise        **=** show n

main **::** IO()
main **=** mapM_ putStrLn $ map fizz [1..100]*
```

## *锈*

```
*fn main() {
    for i in 1..100 {
        if i % 15 == 0 { println!("FizzBuzz") }
        else if i % 3 == 0 { println!("Fizz") }
        else if i % 5 == 0 { println!("Buzz") }
        else { println!("{}", i) }
    }
}*
```

## *目标-c*

```
*for (int i = 1; i <= 100; ++i) {
            if (i % 15 == 0) {
                NSLog(@"FizzBuzz");
            } else if (i % 3 == 0) {
                NSLog(@"Fizz");
            } else if (i % 5 == 0) { 
                NSLog(@"Buzz");
            } else {
                NSLog(@"%d", i);
            }
        }*
```

## *稀有*

```
*for (i in 1:100){

  if(i%%15 == 0) {
    print('FizzBuzz')
  }
  else if(i%%3 == 0) {
    print('Fizz')
  }
  else if (i%%5 == 0){
    print('Buzz')
  }
  else {
    print(i)
  }

}*
```

## *帕*

```
*program FizzBuzz(output);
var
   i : integer;
begin
   for i := 1 to 100 do
      if i mod 15 = 0 then
         writeln('FizzBuzz')
      else if i mod 3 = 0 then
         writeln('Fizz')
      else if i mod 5 = 0 then
         writeln('Buzz')
      else
         writeln(i)
end.*
```

## *Perl 语言*

```
*use strict;
use warnings;
for my $i ( 1 .. 100) {
    if ( $i % 15 == 0) { print 'fizzbuzz' }
    elsif ( $i % 3 == 0 ) { print 'fizz' }
    elsif ( $i % 5 == 0 ) { print 'buzz' }
    else { print $i }
    print "\n";
}*
```

## *占线小时*

```
*run() -> run(1).run(N) -> lists:foreach(fun(S) -> io:format("~s~n", [S]) end, run(N, [])).run(N, Res) when N > 100 -> Res;
run(N, Res) when N rem 15 =:= 0 -> run(N + 1, Res ++ ["FizzBuzz"]);
run(N, Res) when N rem 3 =:= 0 -> run(N + 1, Res ++ ["Fizz"]);
run(N, Res) when N rem 5 =:= 0 -> run(N + 1, Res ++ ["Buzz"]);
run(N, Res) -> run(N + 1, Res ++ [integer_to_list(N)]).fizz_test() -> [1, 2, "Fizz"] = run(3, []).*
```

## *服务器端编程语言（Professional Hypertext Preprocessor 的缩写）*

```
*for ($i = 1; $i <= 100; $i++) {
    if ($i % 15 == 0) {
        echo 'FizzBuzz<br>';
    } elseif ($i % 3 == 0) {
        echo 'Fizz<br>';
    } elseif ($i % 5 == 0) {
        echo 'Buzz<br>';
    } else {
        echo $i . '<br>';
    }
}*
```

## *VB。网*

```
*For i = 1 To 100
        If i Mod 15 = 0 Then
            Console.WriteLine("FizzBuzz")
        ElseIf i Mod 5 = 0 Then
            Console.WriteLine("Buzz")
        ElseIf i Mod 3 = 0 Then
            Console.WriteLine("Fizz")
        Else
            Console.WriteLine(i)
        End If
    Next*
```

# *结论*

*我没有太多的上述语言的经验，所以我相信可能有一个更简单的方法来编码上面的一些。*

*并排看到所有语言之间的语法差异和相似之处是很有趣的！*

*希望你喜欢阅读！*