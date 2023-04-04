# 微软面试任务的最佳解决方案。模式识别。

> 原文：<https://blog.devgenius.io/best-solutions-for-microsoft-interview-tasks-pattern-recognition-fe331643a9c0?source=collection_archive---------2----------------------->

![](img/bf5875794727fcdf7e3f0a290ac4769d.png)

# 描述:

[](https://leetcode.com/discuss/interview-question/928806/) [## 微软| OA 2020 |模式识别- LeetCode 讨论

### 697 VIEWS 编程挑战描述:给定一个模式作为第一个参数和一个由|…

leetcode.com](https://leetcode.com/discuss/interview-question/928806/) 

# 解决方案:

看起来这个任务不是关于算法的，这只是关于写一个简单的解析器，它将给定的字符串分解成多个部分，并写一个函数，它在给定的字符串中查找并计算所有的子字符串。

在这个任务中唯一有趣的主题是寻找子串算法的实现。但是任务不要求实现这个算法，字符串很短，我们对子字符串的搜索没有特殊要求，所以我们可以使用标准库中的实现。我们应该记住的唯一事情是，这个实现的复杂性可能非常不同。像这样的简单算法的平均复杂度约为 2*N，其中 N 是字符串的长度，复杂度更低，为 O(N*P ),其中 P 是模式的长度，这取决于 std::string.find()方法的实现。

```
int count_substrings(const std::string & s, const std::string& sub, bool overlapped = true) {
    size_t sub_size = sub.size();
    if ( (sub_size == 0) || (sub_size > s.size()) ) return 0;
    int count = 0;
    for (size_t offset = s.find(sub); 
      offset != string::npos;
      offset = s.find(sub, overlapped?offset+1:offset+sub.size())) {
        ++count;
    }
    return count;}
```

对于像“aaa…(1.000.000 多一个‘a’)…aaaB”和重叠子串“aaa…(999.000 多一个‘a’)…aaaB”这样的字符串，它会工作得非常慢，因为我们需要比较 1.000.000 * 999.000 个字符。

即使复杂度为 O(N ),也有许多更快的算法，但它们都适用于特殊情况或有附加要求。这就是为什么有如此多的算法被发明出来，而人们并不是只使用其中最好的一个。下面我将展示几个最著名的通用算法，它们适用于大多数情况，并且没有太多额外的要求。

最著名的是[Knuth-Morris-Pratt 算法](https://en.wikipedia.org/wiki/Knuth%E2%80%93Morris%E2%80%93Pratt_algorithm)

```
int KMP_count(const string &s, const string &sub) {
    int num_subs = 0;
    size_t sub_size = sub.size();
    size_t s_size = s.size();
    if ((sub_size == 0) || (sub_size > s.size())) { return 0; }
    vector<int> prefixes(sub_size);
    for (int k = 0, i = 1; i < sub_size; ++i) {
        while ((k > 0) && (sub[i] != sub[k])) {
            k = prefixes[k - 1];
        }
        if (sub[i] == sub[k]) { k++; }
        prefixes[i] = k;
    } for (int k = 0, i = 0; i < s_size; ++i) {
        while ((k > 0) && (sub[k] != s[i])) {
            k = prefixes[k - 1];
        }
        if (sub[k] == s[i]) { k++; } if (k == sub_size) {
            // to get position of substrings in the string
            // (i - sub_size + 1); 
            ++num_subs;
        }
    }
    return num_subs;
}
```

有一种线性复杂度的快速算法，其工作原理类似于 KMP 算法，但看起来更容易理解。这就是所谓的 [Z 功能](https://cp-algorithms.com/string/z-function.html)。与前面的算法一样，它需要分配一个大小为 S 的额外整数数组，其中 S 是字符串 N 的长度+子字符串 p 的长度。然后，我们将 Z 函数的计算结果保存在数组中，遍历该数组并识别搜索的子字符串的位置。这个算法的实现如下:

```
vector<int> z_function (const string & s) {
    int n = (int) s.size();
    vector<int> z (n);
    for (int i=1, l=0, r=0; i<n; ++i) {
        if (i <= r)
            z[i] = min (r-i+1, z[i-l]);
        while (i+z[i] < n && s[z[i]] == s[i+z[i]])
            ++z[i];
        if (i+z[i]-1 > r)
            l = i,  r = i+z[i]-1;
    }
    return z;
}int z_count(const std::string & s, const std::string& sub) {
    size_t sub_size = sub.size();
    if ( (sub_size == 0) || (sub_size > s.size()) ) return 0; string working_string(sub + s);
    size_t ws_size = working_string.size(); auto z = z_function(working_string); int number_subs = 0; for(int i = sub_size; i < ws_size; ++i) {
        if (z[i] >= sub_size) {
// to get exact position of substrings in the string
//            cout << i - sub_size << endl; 
            ++number_subs;
        }
    }
    return number_subs;
}
```

最后，我们应该实现解析器，它将输入字符串分解为模式和 blobs，我们将在其中搜索模式。

不幸的是，标准 C++仍然没有像普通 C 中的 strtok 那样好的标记器，所以我将使用它。即使它看起来不像纯 C++解决方案那么安全，但它允许在一个类似的地方标记一个字符串，而不是在纯 C++中标记几十行。

```
string parser(const string & s) {
    string output;
    int total_patterns = 0; size_t pos = s.find_first_of(';'); string pattern = s.substr(0, pos);
    string blobs = s.substr(pos+1); for(char *token = strtok((char*)blobs.c_str(), "|"); 
        token != NULL; token = strtok(NULL, "|")) 
    {
        string stoken(token);
//        int num_patterns = count_substrings(stoken, pattern);
//        int num_patterns = z_count(stoken, pattern);
        int num_patterns = KMP_count(stoken, pattern);
        total_patterns += num_patterns;
        output.append(to_string(num_patterns));
        output.append("|");
    }
    output.append(to_string(total_patterns));
    return output;
}
```

您可以在这里找到包含完整项目的资源库:[https://github . com/jolly-fellow/Microsoft/tree/master/pattern _ recognition](https://github.com/jolly-fellow/microsoft/tree/master/pattern_recognition)

返回到[目录。](https://medium.com/@molchevsky/best-solutions-for-microsoft-interview-tasks-cae6b0f3ff86)