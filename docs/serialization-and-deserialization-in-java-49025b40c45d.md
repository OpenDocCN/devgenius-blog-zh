# Java 中的序列化和反序列化

> 原文：<https://blog.devgenius.io/serialization-and-deserialization-in-java-49025b40c45d?source=collection_archive---------5----------------------->

我们可能都听说过这些术语在软件开发过程中到处出现。在本文中，我们将通过例子和用例来揭开这些术语的神秘面纱。

![](img/4cf711a1a91b10d7bb1dce2edf986c8f.png)

马库斯·斯皮斯克在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄的照片

# 定义

`define: serialization`和`define: deserialization`

> 在计算中，**串行化**(美国拼法)或**串行化**(英国拼法)是将数据结构或对象状态转换成可以存储(例如，在文件或内存数据缓冲区中)或传输(例如，通过计算机网络)并在以后重建(可能在不同的计算机环境中)的格式的过程
> 
> 相反的操作，从一系列字节中提取数据结构，是**反序列化**

# 通过一个例子来理解它

在本节中，我们将以一个非常常见的数据结构**二叉树**为例，尝试将其序列化为 S **tring** 并反序列化回**二叉树**。

我们已经创建了一个算法类，它有两个方法叫做 serialize 和 deserialize，将二叉树序列化为一个字符串，并将字符串反序列化为二叉树。

**序列化**算法非常简单。我们以**预排序**的方式遍历树，并创建一个字符串，其中节点值由“，”连接，无论我们在哪里得到`null`，我们都将其表示为特殊字符“N”(该特殊字符必须是唯一的，以便我们可以区分可能的节点值和`null`

在**反序列化**中，我们在“”处分割字符串，并将这些字符串存储为队列。然后遍历队列，每当我们检测到一个特殊字符(在我们的例子中是“N”)时，我们知道我们必须在这里停下来并返回`null`，否则，我们递归地继续创建节点。

```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;
import java.util.Queue;/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode(int x) { val = x; }
 * }
 *//**
 * This algorithm class gives two methods
 * to serialize and deserialize to serialize 
 * a Binary Tree to String and String to 
 * Binary Tree respectively.
 * 
 * We will just separate nodes value with ","
 * and whenever there is null then we can represent
 * that node with "N" so that later we can decrypt it.
 */
public class Algorithm {
    public String serialize(TreeNode root) {
        if (null == root) return "N";
        StringBuilder sb = new StringBuilder();
        sb.append(root.val);
        sb.append(",");
        sb.append(serialize(root.left));
        sb.append(",");
        sb.append(serialize(root.right));
        return sb.toString();
    } public TreeNode deserialize(String data) {
        Queue<String> q = new LinkedList<>(
                new ArrayList<String>(
                    Arrays.asList(data.split(","))
                )
            );
        return util(q);
    } public TreeNode util(Queue<String> q) {
        String str = q.poll();
        if (str.equals("N")) return null;
        TreeNode node = new TreeNode(Integer.valueOf(str));
        node.left = util(q);
        node.right = util(q);
        return node;
    }
}
```

# Spring Boot 的序列化和反序列化

*Rest 方面，API****序列化*** *是 Spring boot 将 Java 对象转换为 JSON 对象时所做的事情，同理，* ***反序列化*** *是将 JSON 对象转换为 Java 对象时所做的事情。*

## 序列化

通常，我们使用`Serializable`接口将我们的类标记为可序列化的。通过这样做，我们确保我们的类对象可以被转换成流。例如，这里我们的`Student`类实现了 Serializable，并且定义了`serialVersionUID`。

```
@Builder
@NoArgsConstructor
@AllArgsConstructor
@Data
public class Student implements Serializable {
    private static final long serialVersionUID = 1L; 
    private Integer id;
    private String name;
    private String class;

    // Client don't want this unprocessed data
    @JsonIgnore
    private String jsonResult; 

    // They want processed data instead
    private Result result;
}
```

使用这种方法时，我们需要考虑的几个注意事项是

1.  当一个类实现了`Serializable`接口时，它的所有子类也是可序列化的。相反，当一个对象引用另一个对象时，这些对象必须单独实现`Serializable`接口，否则将抛出`NotSerializableException`。
2.  我们使用`serialVersionUID`来验证保存和加载的对象具有相同的属性，因此与序列化兼容。如果我们不声明它，那么 JVM 将在运行时自动生成一个依赖于编译器的，因此可能会导致意外的`InvalidClassException`。

## 反序列化

当我们从某个 API 获取数据或者读取某个外部 JSON 文件并将其存储为一个定义良好的类时，我们可以使用`ObjectMapper`。例如，这里我们正在读取一个 JSON 值，并将该值存储为 Student class。

```
try {
    ObjectMapper objectMapper = new ObjectMapper();
    studentDetails = objectMapper.
        readValue(studentJson,  Student.class);
} catch (JsonProcessingException e) {
    return null;
}
```

*有各种* ***禅师注解*** *也有助于我们在* ***序列化*** *和* ***反序列化*** *的过程。我在这篇**[***文章***](https://medium.com/dev-genius/three-jackson-annotations-which-all-spring-boot-developers-should-know-1b6304dda19) *中已经涵盖了重要的。**

*如果您有兴趣了解更多，请查看这些文章。*

1.  *[https://www.baeldung.com/java-serialization](https://www.baeldung.com/java-serialization)*
2.  *[https://en.wikipedia.org/wiki/Serialization](https://en.wikipedia.org/wiki/Serialization)*
3.  *[https://www . endpoint dev . com/blog/2020/03/序列化-问题-spring-rest/](https://www.endpointdev.com/blog/2020/03/serialization-issues-spring-rest/)*
4.  *[https://www . javaguides . net/2018/06/guide-to-serialization-in-Java . html](https://www.javaguides.net/2018/06/guide-to-serialization-in-java.html)*
5.  *[https://blog . dev genius . io/three-Jackson-annotations-which-all-spring-boot-developers-should-know-1b 6304 DDA 19](/three-jackson-annotations-which-all-spring-boot-developers-should-know-1b6304dda19)*