# 人性化——c#中令人兴奋的库

> 原文：<https://blog.devgenius.io/humanizer-an-exciting-library-in-c-ba2527410e42?source=collection_archive---------1----------------------->

![](img/3491c3330fe90cdf30a5c1e73df008ca.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上 [Maksym Zakharyak](https://unsplash.com/@zakharyak?utm_source=medium&utm_medium=referral) 拍摄的照片

这一次，我将向你介绍一个相当“酷”的图书馆。这个图书馆叫做“人性化”。它只有一个功能:将字符串、日期、……转换成人类可以阅读的文字(正如 Humanizer 这个名字的意思是“拟人”)。这听起来很简单，但你会惊讶于它的功能。

文章只是展示，介绍性的，所以不会有太多的代码。如果你好奇，你可以创建一个新项目，使用 **Nuget** 来安装人性化器并测试代码。人性化的一些显著特征。

# **将函数名和变量名转换成有意义的字符串**

在编程中，我们经常根据一个命名约定来命名字段、函数名、变量名。人性化允许把这些名字变成一个有意义的字符串。

```
"PascalCaseInputStringIsTurnedIntoSentence".Humanize();
//"Pascal case input string is turned into sentence"Underscored_input_string_is_turned_into_sentence".Humanize();
//"Underscored input string is turned into sentence"Underscored_input_String_is_turned_INTO_sentence".Humanize();
//Underscored input String is turned INTO sentence
```

可以将字符串转换成大写、小写、大写、首字母:

```
"Sentence casing".Transform(To.LowerCase);//sentence casing"Sentence casing".Transform(To.SentenceCase); //Sentence casing"Sentence casing".Transform(To.TitleCase); //Sentence Casing"Sentence casing".Transform(To.UpperCase); //SENTENCE CASING
```

# **折叠字符串**

为了显示长信息，我们通常会编写一个函数来切割字符串，然后以一个… Humanizer 有一个内置函数支持这一点。我们也可以减少字符或单词的数量

```
"Long text to truncate".Truncate(10, Truncator.FixedLength); 
//Cut 10 characters 
//Long text…"Long text to truncate".Truncate(2, Truncator.FixedNumberOfWords); //Cut 2 characters
//Long text…
```

# 将枚举转换为有意义的字符串

对于一些长的枚举，我们经常使用注释将其转换成有意义的字符串。人性化处理这个非常容易。

```
public enum EnumUnderTest
{
     [Description (Custom description)]
     MemberWithDescriptionAttribute,
     MemberWithoutDescriptionAttribute,
}

// If DescriptionAttribute, read the word Attribute
EnumUnderTest.MemberWithDescriptionAttribute.Humanize (); // Custom description// If not, convert enum to string
EnumUnderTest.MemberWithoutDescriptionAttribute.Humanize (); // Member without description attribute
```

# 处理日期时间

你想做的功能，计算时间发表评论像脸书(1 小时前，2 小时前，等等。).

```
Thread.CurrentThread.CurrentUICulture = new CultureInfo ("en-VN"); // Set Vietnamese language

DateTime.UtcNow.AddHours (-30) .Humanize (); // "yesterday
DateTime.UtcNow.AddHours (-60) .Humanize (); //"2 days ago

DateTime.UtcNow.AddHours (2) .Humanize (); // 2 hours
DateTime.UtcNow.AddDays (1) .Humanize (); //Tomorrow
```

# 处理时间跨度

TimeSpan 处理功能差不多，英语和越南语都运行的很好。

```
TimeSpan.FromMilliseconds(2).Humanize(); //2 milliseconds
TimeSpan.FromDays(1).Humanize(); //1 day
TimeSpan.FromDays(16).Humanize(); //2 weeks

// By default, the Humanize () function will give the largest unit (year-&amp;gt; month -&amp;gt; ...).// We pass more parameters to display the smaller units
TimeSpan.FromMilliseconds(1299630020).Humanize(); //2 weeks
TimeSpan.FromMilliseconds(1299630020).Humanize(3); //2 weeks, 1 day, 1 hour
TimeSpan.FromMilliseconds(1299630020).Humanize(4); //2 weeks, 1 day, 1 hour, 30 seconds
```

将数字转换成单词(仅限英语)

```
1.ToWords(); //one10.ToWords(); //ten11.ToWords(); //eleven122.ToWords(); //one hundred and twenty-two3501.ToWords(); //three thousand five hundred and one
```

# 处理文件大小(相当不错)

Humanizer 允许轻松处理与文件大小(KB、MB、GB、…)相关的数据:

```
**var** fileSize = (10).Kilobytes();fileSize.Bits      =&amp;gt; 81920fileSize.Bytes     =&amp;gt; 10240fileSize.Kilobytes =&amp;gt; 10// We can add and subtract file size
**var** total = (10).Gigabytes() + (512).Megabytes() - (2.5).Gigabytes();// Display as text
7.Bits().ToString();        // 7 b
8.Bits().ToString();        // 1 B
(.5).Kilobytes().Humanize();  // 512 B
(1000).Kilobytes().ToString(); // 1000 KB
(1024).Kilobytes().Humanize(); // 1 MB// Parse backwards from the text (Not case sensitive)ByteSize.Parse("1.55 mB");ByteSize.Parse("1.55 mb");ByteSize.Parse("1.55 GB");ByteSize.Parse("1.55 gB");ByteSize.Parse("1.55 gb");ByteSize.Parse("1.55 TB");
```

在本文的范围内，我只是介绍了 Humanizer 的一些突出特性。你可以在这里找到更多:[https://github.com/MehdiK/Humanizer](https://github.com/MehdiK/Humanizer)。

再次见到你。