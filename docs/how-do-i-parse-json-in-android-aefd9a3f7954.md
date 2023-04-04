# 我如何在 Android 中解析 JSON？

> 原文：<https://blog.devgenius.io/how-do-i-parse-json-in-android-aefd9a3f7954?source=collection_archive---------14----------------------->

![](img/273685d92b0b588ab4b4b1babf009d8b.png)

来源:谷歌

**在 Android 中解析 JSON，可以使用** `**org.json**` **包，默认包含在 Android 操作系统中。**下面是一个如何在 Android 中解析 JSON 字符串的例子:

```
import org.json.JSONObject;

// ...

String jsonString = "{\"name\":\"John\", \"age\":30, \"city\":\"New York\"}";

try {
    JSONObject json = new JSONObject(jsonString);

    String name = json.getString("name");
    int age = json.getInt("age");
    String city = json.getString("city");
} catch (JSONException e) {
    // handle exception
}
```

在这个例子中，我们首先从`org.json`包中导入`JSONObject`类。然后，我们创建一个 JSON 字符串，并将其传递给`JSONObject`构造函数，以创建一个新的 JSON 对象。然后，我们可以使用`getString`和`getInt`方法提取“姓名”、“年龄”和“城市”键的值。

**将代码包装在 try-catch 块中很重要，因为如果 JSON 字符串无效，解析 JSON 会抛出** `**JSONException**` **。**

这只是一个简单的例子，`org.json`包提供了更多解析和操作 JSON 数据的方法。您可以在`org.json`包的 [JavaDocs](https://developer.android.com/reference/org/json/package-summary) 中找到更多信息。

下面是一个在 Android 中解析更复杂的 JSON 对象的例子:

```
import org.json.JSONObject;

// ...

String jsonString = "{\"employees\":[{\"firstName\":\"John\", \"lastName\":\"Doe\"}, {\"firstName\":\"Anna\", \"lastName\":\"Smith\"}, {\"firstName\":\"Peter\", \"lastName\":\"Jones\"}]}";

try {
    JSONObject json = new JSONObject(jsonString);
    JSONArray employees = json.getJSONArray("employees");

    for (int i = 0; i < employees.length(); i++) {
        JSONObject employee = employees.getJSONObject(i);
        String firstName = employee.getString("firstName");
        String lastName = employee.getString("lastName");
        System.out.println(firstName + " " + lastName);
    }
} catch (JSONException e) {
    // handle exception
}
```

在这个例子中，JSON 对象包含一个 employee 对象数组，每个对象都有一个“firstName”和“lastName”字段。我们使用`getJSONArray`方法提取数组，然后使用 for 循环迭代数组元素。对于每个元素，我们使用`getString`方法提取“名字”和“姓氏”值。

这只是在 Android 中解析更复杂的 JSON 对象的一种方式。您可以根据需要使用`org.json`包提供的各种方法来提取和操作数据。

![](img/bb7693f46b6d79742b76ce569ea34d4a.png)

来源:谷歌

`org.json`包提供了许多在 Android 中提取和操作 JSON 数据的方法。以下是一些最常用的方法:

*   `**getInt(String key)**` **:从 JSON 对象中提取整数值。**
*   `**getLong(String key)**` **:从 JSON 对象中提取一个长整型值。**
*   `**getDouble(String key)**` **:从 JSON 对象中提取一个 double 值。**
*   `**getBoolean(String key)**` **:从 JSON 对象中提取一个布尔值。**
*   `**getString(String key)**` **:从 JSON 对象中提取一个字符串值。**
*   `**getJSONObject(String key)**` **:从 JSON 对象中提取一个嵌套的 JSON 对象。**
*   `**getJSONArray(String key)**` **:从 JSON 对象中提取一个 JSON 数组。**

这只是几个例子，`org.json`包提供了更多提取和操作 JSON 数据的方法。您可以在`org.json`包的 [JavaDocs](https://developer.android.com/reference/org/json/package-summary) 中找到完整的方法列表。

下面是一个使用这些方法从更复杂的 JSON 对象中提取数据的例子:

```
import org.json.JSONObject;

// ...

String jsonString = "{\"employees\":[{\"firstName\":\"John\", \"lastName\":\"Doe\", \"age\":30, \"active\":true}, {\"firstName\":\"Anna\", \"lastName\":\"Smith\", \"age\":25, \"active\":false}, {\"firstName\":\"Peter\", \"lastName\":\"Jones\", \"age\":35, \"active\":true}]}";

try {
    JSONObject json = new JSONObject(jsonString);
    JSONArray employees = json.getJSONArray("employees");

    for (int i = 0; i < employees.length(); i++) {
        JSONObject employee = employees.getJSONObject(i);
        String firstName = employee.getString("firstName");
        String lastName = employee.getString("lastName");
        int age = employee.getInt("age");
        boolean active = employee.getBoolean("active");
        System.out.println(firstName + " " + lastName + " (" + age + ") " + (active ? "active" : "inactive"));
    }
} catch (JSONException e) {
    // handle exception
}
```

在本例中，我们从数组中的每个 employee 对象中提取“firstName”、“lastName”、“age”和“active”值，并打印出来。

**关于在 Android 中解析 JSON，你可能还想知道一些事情:**

*   **JSON 数组由** `**org.json**` **包中的** `**JSONArray**` **类表示。可以使用** `**length**` **方法获取数组中元素的个数，使用** `**getJSONObject**` **方法获取特定元素作为** `**JSONObject**` **。**
*   **你可以使用** `**opt***` **的方法(例如**`**optInt**`**`**optString**`**等)。)从 JSON 对象中提取值，但是如果在对象中没有找到键，这些方法将返回默认值。如果 JSON 数据没有严格定义，并且您希望优雅地处理丢失的键，这将非常有用。****
*   ****可以使用** `**put**` **的方法(如**`**putInt**`**`**putString**`**等)。)将键值对添加到一个** `**JSONObject**` **。如果您想以编程方式创建一个 JSON 对象，这可能很有用。******
*   ******你可以使用** `**toString**` **方法将一个** `**JSONObject**` **或者** `**JSONArray**` **转换回一个 JSON 字符串。如果您想将 JSON 数据发送回服务器或保存到文件中，这可能很有用。******

****下面的示例展示了如何使用这些方法:****

```
**import org.json.JSONObject;

// ...

String jsonString = "{\"employees\":[{\"firstName\":\"John\", \"lastName\":\"Doe\", \"age\":30, \"active\":true}, {\"firstName\":\"Anna\", \"lastName\":\"Smith\", \"age\":25, \"active\":false}, {\"firstName\":\"Peter\", \"lastName\":\"Jones\", \"age\":35, \"active\":true}]}";

try {
    JSONObject json = new JSONObject(jsonString);
    JSONArray employees = json.getJSONArray("employees");

    for (int i = 0; i < employees.length(); i++) {
        JSONObject employee = employees.getJSONObject(i);
        String firstName = employee.optString("firstName", "Unknown");
        String lastName = employee.optString("lastName", "Unknown");
        int age = employee.optInt("age", 0);
        boolean active = employee.optBoolean("active", false);
        System.out.println(firstName + " " + lastName + " (" + age + ") " + (active ? "active" : "inactive"));
    }

    // Add a new employee to the array
    JSONObject newEmployee = new JSONObject();
    newEmployee.put("firstName", "Bob");
    newEmployee.put("lastName", "Smith");
    newEmployee.put("age", 40);
    newEmployee.put("active", true);
    employees.put(newEmployee);

    // Convert the JSON object back to a string
    String updatedJsonString = json.toString();
    System.out.println(updatedJsonString);
} catch (JSONException e) {
    // handle exception
}**
```

****在本例中，我们解析并操作一个表示雇员列表的 JSON 对象。JSON 对象只有一个键“employees”，它是 employee 对象的数组。每个雇员对象都有“名字”、“姓氏”、“年龄”和“活动”字段。****

****我们首先从`org.json`包中导入`JSONObject`类，并使用`JSONObject`构造函数从 JSON 字符串创建一个 JSON 对象。然后，我们使用`getJSONArray`方法从 JSON 对象中提取“employees”数组。****

****接下来，我们使用 for 循环迭代“employees”数组的元素。对于每个元素，我们使用`optString`和`optInt`方法提取“名字”、“姓氏”、“年龄”和“活动”值。如果在 JSON 对象中找不到这些键，这些方法允许我们为它们指定默认值。****

****在 for 循环之后，我们使用`JSONObject`构造函数创建一个新的 employee 对象，并使用`put`方法向其添加键值对。然后我们使用`JSONArray`对象的`put`方法将新的 employee 对象添加到“employees”数组中。****

****最后，我们使用`JSONObject`的`toString`方法将其转换回 JSON 字符串，并将其打印出来。****

****我希望这有助于阐明这个例子。谢谢大家！****