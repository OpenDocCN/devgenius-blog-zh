# 在 Android Studio 中学习 JSON 解析

> 原文：<https://blog.devgenius.io/learn-json-parsing-in-android-studio-a95b1bda1c87?source=collection_archive---------1----------------------->

![](img/112d757b8f569ad9aa255f42c1d53baa.png)

在本文中，我们将学习如何在 Android 中进行 **JSON 解析。 [JSON](http://www.json.org/) 代表 JavaScript 对象符号。这是一种用于数据交换的轻量级格式。它基于 JavaScript 语言的子集(JavaScript 中构建对象的方式)。**

**JSON 建立在两个结构之上:**

*   名称/值对的集合。它也被称为对象、记录、结构、字典、哈希表、键列表或关联数组。
*   值的有序列表。它也被称为数组、向量、列表或序列。

# JSON 元素:-

1.  **JSON 数组-** 在 JSON 文件中，方括号([)代表一个 JSON 数组
2.  **JSON 对象-** 在 JSON 文件中，花括号({)代表一个 JSON 对象。
3.  **KEY-**JSON 对象包含一个只是字符串的键。成对的键/值组成了一个 JSON 对象。
4.  **VALUE-** 每个键都有一个值，可以是字符串、整数或双精度值等。

# ANDROID 中的 JSON 解析:-

**有 2 种类型:- JSON 数组和 JSON 对象。**

*   如果 JSON 从方括号([)开始，那么它就是 JSON 数组。为此使用 getJSONArray()方法。
*   如果 JSON 从花括号({)开始，那么它就是一个 JSON 对象。为此，请使用 getJSONObject()方法。

JSON 文件示例-(ex1.json)

```
{ "colors": [ { "color": "black", "category": "hue", "type": "primary", "code": { "rgba": [255,255,255,1], "hex": "#000" } }, { "color": "white", "category": "value", "code": { "rgba": [0,0,0,1], "hex": "#FFF" } }, { "color": "red", "category": "hue", "type": "primary", "code": { "rgba": [255,0,0,1], "hex": "#FF0" } }, { "color": "blue", "category": "hue", "type": "primary", "code": { "rgba": [0,0,255,1], "hex": "#00F" } }, { "color": "yellow", "category": "hue", "type": "primary", "code": { "rgba": [255,255,0,1], "hex": "#FF0" } }, { "color": "green", "category": "hue", "type": "secondary", "code": { "rgba": [0,255,0,1], "hex": "#0F0" } } ] }
```

*   将这个 JSON 结构复制到一个文件中，并保存为 Ex1.json
*   通过以下方式在 android studio 中创建资产文件夹:-
*   从安卓改包。
*   右键点击 **app** >选择**新建文件夹** > **资产** >点击完成。
*   复制 ex1.json 文件并将其粘贴到 assets 文件夹中。

在这个 JSON 文件中，我们有一个颜色列表，其中每个对象都包含颜色、类别、类型和不同类型的代码(如 rgba 和 hex)等信息。我们将使用垂直方向的 LinearLayoutManager 来显示数组项。

首先，我们在 XML 文件中声明一个 RecyclerView，然后在活动中获取它的引用。之后，我们获取 JSON 文件，然后解析 JSONArray 数据，最后设置适配器以显示 RecyclerView 中的项目。每当用户点击一个项目时，颜色名称就会在 Toast 的帮助下显示在屏幕上。

**必读**:[**Android Studio 中的谷歌地图教程【循序渐进】**](https://www.androidhire.com/google-map-tutorial-in-android-studio/)

# 解析 JSON 的步骤:-

*   创建新活动
*   打开 gradle>build.gradle(模块:app)

添加回收器视图和卡片视图的相关性:-

```
implementation "com.android.support:recyclerview-v7:23.0.1" // dependency file for RecyclerView implementation 'com.android.support:cardview-v7:23.0.1' // dependency file for CardView
```

在这一步中，我们在 XML 文件中创建一个 RecyclerView。

```
<?xml version="1.0" encoding="utf-8"?> <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android" xmlns:app="http://schemas.android.com/apk/res-auto" xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent" android:layout_height="match_parent" tools:context=".MainActivity"> <android.support.v7.widget.RecyclerView android:id="@+id/recyclerView" android:layout_width="match_parent" android:layout_height="match_parent" /> </RelativeLayout>
```

创建另一个名为“list_view.xml”的 XML 文件

若要显示数据，请在 list_view.xml 中创建 textview

```
<?xml version="1.0" encoding="utf-8"?> <android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android" android:layout_width="match_parent" android:layout_height="wrap_content" xmlns:card_view="http://schemas.android.com/apk/res-auto" card_view:cardMaxElevation="1dp" card_view:cardElevation="1dp" > <LinearLayout android:layout_width="match_parent" android:layout_height="wrap_content" android:orientation="vertical" > <TextView android:layout_width="match_parent" android:layout_height="wrap_content" android:id="@+id/color" android:textSize="40dp" android:background="@color/colorAccent" android:textColor="#ffffff"/> <TextView android:layout_width="wrap_content" android:layout_height="wrap_content" android:id="@+id/category" android:textSize="20dp" android:background="@color/cardview_light_background" android:textColor="#000000"/> <TextView android:layout_width="wrap_content" android:layout_height="wrap_content" android:id="@+id/hex" android:textSize="20dp" android:background="@color/cardview_light_background" android:textColor="#000000"/> </LinearLayout> </android.support.v7.widget.CardView>
```

在这一步中，我们首先获取 RecyclerView 的引用。之后，我们从 assets 获取 JSON 文件，使用 JSONArray 和 JSONObject 方法解析 JSON 数据，然后设置 LayoutManager，最后，我们设置适配器以显示 RecyclerView 中的项目。

```
6.	package com.example.json1; import android.support.v7.app.AppCompatActivity; import android.os.Bundle; import android.os.Bundle; import android.support.v7.app.AppCompatActivity; import android.support.v7.widget.LinearLayoutManager; import android.support.v7.widget.RecyclerView; import android.util.Log; import org.json.JSONArray; import org.json.JSONException; import org.json.JSONObject; import java.io.IOException; import java.io.InputStream; import java.util.ArrayList; import java.util.Arrays; public class MainActivity extends AppCompatActivity { ArrayList<String> color = new ArrayList<>(); ArrayList<String> category = new ArrayList<>(); ArrayList<String> hex = new ArrayList<>(); @Override protected void onCreate(Bundle savedInstanceState) { super.onCreate(savedInstanceState); setContentView(R.layout.activity_main); // get the reference of RecyclerView RecyclerView recyclerView = (RecyclerView) findViewById(R.id.recyclerView); // set a LinearLayoutManager with default vertical orientation LinearLayoutManager linearLayoutManager = new LinearLayoutManager(getApplicationContext()); recyclerView.setLayoutManager(linearLayoutManager); try { // get JSONObject from JSON file JSONObject obj = new JSONObject(loadJSONFromAsset()); // fetch JSONArray named colors JSONArray userArray = obj.getJSONArray("colors"); // implement a loop to get colors list data for (int i = 0; i < userArray.length(); i++) { // create a JSONObject for fetching single color data JSONObject userDetail = userArray.getJSONObject(i); // fetch color name and category and store it in arraylist color.add(userDetail.getString("color")); category.add(userDetail.getString("category")); // create an object for getting code data from JSONObject JSONObject contact = userDetail.getJSONObject("code"); // fetch hex value and store it in arraylist hex.add(contact.getString("hex")); } } catch (JSONException e) { e.printStackTrace(); } // call the constructor of CustomAdapter to send the reference and data to Adapter Custom_adapter customAdapter = new Custom_adapter(MainActivity.this, color, category, hex); recyclerView.setAdapter(customAdapter); // set the Adapter to RecyclerView } public String loadJSONFromAsset() { String json = null; try { InputStream is = getAssets().open("ex1.json"); int size = is.available(); byte[] buffer = new byte[size]; is.read(buffer); is.close(); json = new String(buffer, "UTF-8"); } catch (IOException ex) { ex.printStackTrace(); return null; } return json; } }
```

创建另一个名为 Custom_adapter.java 的 java 文件

# 自定义适配器. java

在这一步中，我们创建一个 CustomAdapter 类并扩展 RecyclerView。包含 ViewHolder 的适配器类。之后，我们实现被覆盖的方法，并创建一个从活动中获取数据的构造函数，在这个自定义适配器中有两个方法比较重要，第一个是 onCreateViewHolder，我们在其中膨胀布局项目 XML 并将其传递给 ViewHolder，另一个是 onBindViewHolder，我们在 View Holder 的帮助下在视图中设置数据。

```
package com.example.json1; import android.content.Context; import android.support.v7.widget.RecyclerView; import android.view.LayoutInflater; import android.view.View; import android.view.ViewGroup; import android.widget.TextView; import android.widget.Toast; import java.util.ArrayList; public class Custom_adapter extends RecyclerView.Adapter<Custom_adapter.MyViewHolder> { ArrayList<String> color; ArrayList<String> category; ArrayList<String> hex; Context context; public Custom_adapter(Context context, ArrayList<String>color, ArrayList<String> category, ArrayList<String> hex) { this.context=context; this.color=color; this.category=category; this.hex=hex; } @Override public MyViewHolder onCreateViewHolder(ViewGroup parent, int viewType) { // infalte the item Layout View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.list_view, parent, false); MyViewHolder vh = new MyViewHolder(v); // pass the view to View Holder return vh; } @Override public void onBindViewHolder(Custom_adapter.MyViewHolder holder, final int position) { // set the data in items holder.color.setText(color.get(position)); holder.category.setText(category.get(position)); holder.hex.setText(hex.get(position)); // implement setOnClickListener event on item view. holder.itemView.setOnClickListener(new View.OnClickListener() { @Override public void onClick(View view) { // display a toast with color name on item click Toast.makeText(context, color.get(position), Toast.LENGTH_SHORT).show(); } }); } @Override public int getItemCount() { return color.size(); } public class MyViewHolder extends RecyclerView.ViewHolder { TextView color, category, hex;// init the item view's public MyViewHolder(View itemView) { super(itemView); // get the reference of item view's color = (TextView) itemView.findViewById(R.id.color); category = (TextView) itemView.findViewById(R.id.category); hex = (TextView) itemView.findViewById(R.id.hex); } } }
```

祝贺您…您已经成功构建了这个应用程序。

当您将运行您的应用程序时，您可以看到您的界面如下所示:-

要访问代码，请点击下面的 GitHub 链接:-

这是一个简单的教程，**使用回收器视图和卡片视图在 Android** 应用中解析 JSON。

希望，你喜欢这篇文章。为，更多这样的文章…敬请期待！！！！如有任何疑问，请在下面评论。

*原载于 2019 年 4 月 8 日*[*【https://www.androidhire.com*](https://www.androidhire.com/json-parsing-in-android-tutorial/)*。*