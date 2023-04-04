# 利用认知复杂性进行重构

> 原文：<https://blog.devgenius.io/refactoring-using-cognitive-complexity-7e55197335b6?source=collection_archive---------1----------------------->

![](img/456c3bc04483dc0414b1870dadfc3753.png)

重构代码是软件开发过程的重要部分。但是，有时很难确定应该重构什么以及为什么要重构。判断什么需要重构的一个标准是认知复杂性。

认知复杂性是 SonarSource 开发的一种度量标准，sonar source 是我们在 AWH 使用的代码质量工具的开发者，该工具名为 [SonarQube](https://www.sonarqube.org/) 。SonarQube 挂钩到一个项目的 CI 管道，并对其执行代码质量分析。SonarQube 能找到的问题类型从简单的事情(这个变量从来不用，把它去掉)到更复杂的重构(这个方法太复杂，重构一下)。为了确定一个方法是否太复杂，是否应该重构，需要计算一个认知复杂性得分。

# 定义复杂性

编程和计算机科学中的复杂性有几种含义。大多数程序员可能都熟悉圈复杂度，它试图告诉我们测试或维护一个方法有多困难。它通过为一个方法中的每个分支创建一个分数来做到这一点。像这样:

```
public static string HowMany(int n) // +1 (at least one path through each method)
{
    switch (n)
    {
        case 1: // +1
            return "One"; case 2: // +1
            return "A couple"; case 3: // +1
            return "A few"; case 4: // +1
            return "Several"; default:
            return "Many";
    } // Cyclomatic Complexity = 5
}
```

虽然圈复杂度很好地告诉了我们一个方法在测试时有多复杂(分数通常是覆盖一个方法所需的最小测试数)，但它并没有很好地告诉我们代码的*可维护性*或*可理解性*如何。认知复杂性建立在圈复杂性的基础上，并适应圈复杂性，给我们一个更好的分数，这个分数更多地与理解和可维护性有关，而不是测试。

> 请注意，认知复杂性并不特定于任何编程语言或风格。这篇文章中的所有例子都使用 C#。

# 计算复杂性

关于计算认知复杂性，你首先应该知道的是，你不需要自己去做。sonar cube 将你的代码作为 CI/CD 管道的一部分进行分析，它不仅仅是一个认知复杂性分析的好工具。但是学习如何计算分数会有助于以更易维护的方式构建你的代码。

> 这篇文章只给出计算认知复杂性的基础知识。要了解全部细节，请查看 SonarSource 的白皮书。

以下是计算认知复杂性得分的基本规则:

1.  代码控制结构中的每一个断点都增加分数。
2.  对于每一级嵌套的控制结构断点，再次增加分数。
3.  忽略有助于可读性的简写结构。

让我们再细分一下。

## **控制结构中的中断**

这是圈复杂度最基本的规则。循环和条件会增加流程中每个断点的分数。不过，有一些警告。

*   ****最后*** 块被忽略，但是 ***catch*** 会递增分数，尽管不管捕获多少种类型的异常都只有一次。*
*   *一个 ***开关*** 只会把分数加 1，不管处理了多少个案件。这与圈复杂度不同，这样做是因为一个开关通常比一组等价的***if/else if/else****语句更容易扫描和读取。**
*   **逻辑运算符序列被组合在一起。考虑一下***a&&b&&c&&d***和***a&&b | | c&&d***的区别。第一个将增加 1，而第二个将增加 3，因为它更难读取。**
*   **递归被认为是一种循环，可以增加分数。**
*   **跳转( ***中断、继续、*** 等)会增加分数，但提前 ***返回*** 则不会。**

> **因为关于 ***切换*** 语句的规则，上面的***how much***方法只有一个认知复杂度得分 1。**

## ****嵌套控制结构****

**过度嵌套是非常讨厌的[代码气味](https://en.wikipedia.org/wiki/Code_smell)。认知复杂性通过在嵌套的每一层中增加每一个控制结构来实现这一点。不过，跳转不受嵌套额外增量的影响。**

```
**public string DoSomething(string str)
{
    var newStr = string.Empty; if (str == null) // +1
    {
        for (var i == 0; i < str.Length; i++) // +1 (+1 for nesting)
        {
            if (str[i] == 'a') // +1 (+2 for nesting)
                newStr += 'A';
            else if (str[i] == 'x') // +1 (+2 for nesting)
                continue; // +1
            else
                newStr = str[i];
        }
    } return newStr;
}// Cognitive Complexity = 10**
```

## ****忽略速记结构****

**认知复杂性分数意在奖励良好的编码实践，因此速记结构和语句不计入分数。最值得注意的是，像零合并操作符这样的东西是使用语言降低复杂性的好方法，如下面的例子所示:**

```
**// Not good, increments the score
var myString == string.Empty;
if (someObj == null)
    myString = someObj.StringProperty;// Good, does not increment the score
var myString = someObj?.StringProperty ?? string.Empty;**
```

# **例子**

**让我们使用这些规则并重构一些东西。利用我们所知道的，我们可以确定一些简单的方法来降低复杂性，使方法更容易阅读和维护。让我们从一个大的、讨厌的方法开始，确定最复杂的区域:**

```
**public async Task ParseAsync(string startingUrl, bool recurse = false, bool verbose = false)
{
    startingUrl = CleanUrl(startingUrl);
    _rootUrl = new Uri(startingUrl).GetLeftPart(UriPartial.Authority); var urls = new Dictionary<string, int>();
    urls.Add(startingUrl, 0); while (urls.Any(x => x.Value == 0))
    {
        // Grab any URLs from the dictionary that we have not checked. If we are recursing,
        // new URLs will be added with a status code of 0 and we will pick them up on the
        // next pass.
        var urlsToProcess = urls.Where(x => x.Value == 0).Select(x => x.Key).ToList(); foreach (var url in urlsToProcess)
        {
            var displayUrl = url.Length > (Console.BufferWidth - 10)
                ? $"{url.Substring(0, Console.BufferWidth - 10)}..."
                : url;
            Console.Write(displayUrl); HttpResponseMessage response; try
            {
                response = await _httpClient.GetAsync(url);
            }
            catch (HttpRequestException ex)
            {
                urls[url] = -1; Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine($" [{ex.Message}]");
                Console.ResetColor();
                continue;
            } urls[url] = (int)response.StatusCode; if (response.IsSuccessStatusCode)
            {
                if (verbose)
                {
                    Console.ForegroundColor = ConsoleColor.Green;
                    Console.WriteLine($" [{(int)response.StatusCode}]");
                    Console.ResetColor();
                }
                else
                {
                    // Clear the current line
                    Console.SetCursorPosition(0, Console.CursorTop);
                    Console.Write(new string(' ', Console.BufferWidth - 1));
                    Console.SetCursorPosition(0, Console.CursorTop);
                }
            }
            else
            {
                // Write the error code and exit the loop early
                Console.ForegroundColor = ConsoleColor.Red;
                Console.WriteLine($" [{(int)response.StatusCode}]");
                Console.ResetColor();
                continue;
            } // Exit early if we are not recursing unless we are checking the starting URL
            if (!recurse && url != startingUrl)
                continue; // Exit early if the URL is external or if the content is not HTML
            if (!IsInternalUrl(url) || !response.Content.Headers.ContentType.MediaType.StartsWith("text/html"))
                continue; try
            {
                // If we made it this far, we will parse the HTML for links
                var html = await response.Content.ReadAsStringAsync(); // We add each link to the dictionary with a status code of 0\. Since
                // we are using a dictionary, URLs that are already checked or slated
                // to be checked will be ignored.
                GetUrlsFromHtml(html).ForEach(x => urls.TryAdd(x, 0));
            }
            catch
            {
                Console.WriteLine(url);
                Console.WriteLine($"\tUnable to parse HTML.");
            }
        }
    }
}**
```

**一些正在发生的事情的背景将是很好的，对不对？这是我做的一个名为 [Linky](https://github.com/amoscardino/linky) 的小项目的主要方法。这是一个命令行应用程序，用于扫描网站并尝试识别断开的链接。我在这里省略了一些助手方法，所以我们可以把注意力集中在需要重构的大方法上。**

**在开始之前，让我们试着确定一些问题。首先，最多有 4 层嵌套。这很糟糕，应该是我们的主要目标。还有一些输出到控制台的模板代码可以被抽象掉。这不会对我们的复杂度评分有太大帮助，但是会使方法更容易阅读。**

**我计算出认知复杂性得分为 31。SonarQube 建议方法不要超过 15 个。**

**可以做哪些改变来降低这个分数？我将首先提取 ***大 foreach*** 循环内容作为新方法。这将减少 2 个循环中的嵌套，这将是一个巨大的胜利。然后，我会将各种控制台输出提取到一些新方法中。在某些情况下，这可以提取一些控制流中断，进一步减少嵌套。**

**让我们看看这一切是如何形成的:**

```
**public async Task ParseAsync(string startingUrl, bool recurse = false, bool verbose = false)
{
    _startingUrl = CleanUrl(startingUrl);
    _rootUrl = new Uri(_startingUrl).GetLeftPart(UriPartial.Authority);
    _recurse = recurse;
    _verbose = verbose; var urls = new Dictionary<string, int>();
    urls.Add(_startingUrl, 0); while (urls.Any(x => x.Value == 0))
    {
        // Grab any URLs from the dictionary that we have not checked. If we are recursing,
        // new URLs will be added with a status code of 0 and we will pick them up on the
        // next pass.
        var urlsToProcess = urls.Where(x => x.Value == 0).Select(x => x.Key).ToList(); foreach (var url in urlsToProcess)
            await ProcessUrlAsync(url, urls);
    }
}private async Task ProcessUrlAsync(string url, Dictionary<string, int> urls)
{
    DisplayUrl(url); HttpResponseMessage response; try
    {
        response = await _httpClient.GetAsync(url);
    }
    catch (HttpRequestException ex)
    {
        urls[url] = -1; DisplayErrorCode(ex.Message);
        return;
    } urls[url] = (int)response.StatusCode; if (response.IsSuccessStatusCode)
        DisplaySuccessCode(urls[url]);
    else
    {
        // Write the error code and exit the loop early
        DisplayErrorCode(urls[url].ToString());
        return;
    } // Exit early if we are not recursing unless we are checking the starting URL
    if (!_recurse && url != _startingUrl)
        return; // Exit early if the URL is external or if the content is not HTML
    if (!IsInternalUrl(url) || !response.Content.Headers.ContentType.MediaType.StartsWith("text/html"))
        return; try
    {
        // If we made it this far, we will parse the HTML for links
        var html = await response.Content.ReadAsStringAsync(); // We add each link to the dictionary with a status code of 0\. Since
        // we are using a dictionary, URLs that are already checked or slated
        // to be checked will be ignored.
        GetUrlsFromHtml(html).ForEach(x => urls.TryAdd(x, 0));
    }
    catch
    {
        Console.WriteLine(url);
        Console.WriteLine($"\tUnable to parse HTML.");
    }
}private void DisplayUrl(string url)
{
    var displayUrl = url.Length > (Console.BufferWidth - 10)
        ? $"{url.Substring(0, Console.BufferWidth - 10)}..."
        : url; Console.Write(displayUrl);
}private void DisplaySuccessCode(int statusCode)
{
    if (_verbose)
    {
        Console.ForegroundColor = ConsoleColor.Green;
        Console.WriteLine($" [{statusCode}]");
        Console.ResetColor();
    }
    else
    {
        // Clear the current line
        Console.SetCursorPosition(0, Console.CursorTop);
        Console.Write(new string(' ', Console.BufferWidth - 1));
        Console.SetCursorPosition(0, Console.CursorTop);
    }
}private void DisplayErrorCode(string errorCode)
{
    Console.ForegroundColor = ConsoleColor.Red;
    Console.WriteLine($" [{errorCode}]");
    Console.ResetColor();
}**
```

## **我们表现如何？**

*   **主方法， ***ParseAsync*** 现在的分值是 3。🎉**
*   *****processurlsync***是现在最大的方法，但是由于没有嵌套在两个循环中，所以得分只有 7。🎉🎉**
*   *****DisplayUrl*** 和***display success code***各 1 分。***display error code***得分为 0。**
*   **一些参数现在被分配给私有字段。这对分数没有影响，但是保持了代码的可读性，因为我们不需要到处传递它们。**

**从功能上讲，代码的执行和输出与以前完全一样。唯一的区别是我们如何构建我们正在做的事情。我们创建了更小、更集中的方法，更容易理解，因此也更容易维护。认知复杂性帮助我们确定方法中最复杂的部分在哪里，这样我们就可以在最需要的地方进行重构。**