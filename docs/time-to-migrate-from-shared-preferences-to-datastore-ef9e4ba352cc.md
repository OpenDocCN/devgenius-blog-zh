# 从共享首选项迁移到数据存储的时间

> 原文：<https://blog.devgenius.io/time-to-migrate-from-shared-preferences-to-datastore-ef9e4ba352cc?source=collection_archive---------8----------------------->

安卓|科特林

![](img/3d4221d4433f71d2f77cd933da9d5b89.png)

谷歌在 Android Jetpack 中引入了一个名为 DataStore 的新功能。一个存储键值对的库，与共享首选项一样，但效率更高。Datastore 使用 Kotlin 协同例程和流来异步、一致和过渡地存储数据。

数据存储是共享首选项的替代品，它避免了共享首选项带来的许多缺陷。

## 要点

1.  共享首选项是同步的，在主线程上运行。但是 dataStore 是异步的，使用 Kotlin 协同程序，运行在一个单独的线程上，这使得它是线程安全的。(在 Dispatcher.io 下面)
2.  用于存储和读取数据的异步 API。(流量)
3.  数据存储在数据解析期间也不会出现运行时异常。
4.  不提供任何类型安全。

## 有两种类型的数据存储

1.  **Preferences DataStore** -使用密钥存储和访问数据。这种实现不需要预定义的架构，也不提供类型安全。
2.  **Proto DataStore**——将数据存储为自定义数据类型的实例。这个实现要求你使用[协议缓冲区](https://developers.google.com/protocol-buffers)定义一个模式，但是它提供了类型安全。

## 这是一个关于集成数据存储的视频。请看一看！

> 如果两者都冻结了，在水上行走和开发软件就容易了——匿名
> 
> 所以，永远保持进步的空间，远离你的舒适区！..现在让我们深入编码。

## 1.向应用程序添加数据存储

从数据存储开始，您需要首先将打开 app.gradle 文件的库添加到您的项目中，并添加以下依赖项。

```
*//preference DataStore* implementation "androidx.datastore:datastore-preferences:1.0.0-beta01"
```

立即同步项目。

## **2。创建数据存储实例，并将数据存储命名为**

```
private val Context.*dataStore*: DataStore<Preferences> by *preferencesDataStore*(name = "user")
```

我已经使用委托创建了一个数据存储对象，并将其命名为“user”。

## 3.定义键

我们现在定义将作为键值对存储的数据。用我们需要存储的数据类型来存储数据。在访问字符串的情况下，它将是 *stringPreferencesKey。*

```
companion object {
    val NAME = *stringPreferencesKey*("NAME")
    val PHONE_NUMBER = *stringPreferencesKey*("PHONE_NUMBER")
    val ADDRESS = *stringPreferencesKey*("ADDRESS")
}
```

## 4.数据存储的保存功能

首先，我们需要使用一个挂起函数来保存和检索数据。挂起函数只能从另一个挂起函数中的协程调用。

现在，我们需要使用从使用 dataStore 的活动传递来的上下文来调用 edit 函数，以便根据我们创建的数据存储数据。

```
suspend fun savetoDataStore(user: User) {
    context.*dataStore*.edit **{
        it**[NAME] = user.name
        **it**[PHONE_NUMBER] = user.phone
        **it**[ADDRESS] = user.address
    **}** }
```

## 5.获取数据存储的函数

该函数有助于从数据存储中获取数据。它将数据存储中的所有键映射到我们已经创建的模型类。当需要时，我们可以在以后的任何活动或课程中检索它。

```
suspend fun getFromDataStore() = context.*dataStore*.data.*map* **{** User(
        name = **it**[NAME]?:"",
        phone = **it**[PHONE_NUMBER]?:"",
        address = **it**[ADDRESS]?:""
    )
**}**
```

## 6.正在初始化活动中的数据存储

```
//Declaring DataStoreManager
lateinit var dataStoreManager: DataStoreManager
```

初始化 datastore manager——我们需要在 create 函数中初始化它。

```
dataStoreManager = DataStoreManager(this@MainActivity)
```

## 7.将数据保存到数据存储中

这是我们将数据存储到数据存储区的地方，我们需要创建一个 GlobalScope 对象来帮助我们调用挂起函数。我们需要添加 GlobalScope.launch(Dispatchers。IO)- cause dataStore 是一个协程，需要作为协程线程来访问。

```
GlobalScope.*launch*(Dispatchers.IO) **{** dataStoreManager.savetoDataStore(User("Sharon", "3526281", "Bangalore"))
**}**
```

要从数据存储中获取存储的数据，我们需要使用同样的 GlobalScope 原因，这也是一个挂起函数。简单到只需使用 collect 函数获取数据。我只是在一个文本视图中显示它，例如它可以被添加到字符串中，并在需要的地方使用。

```
GlobalScope.*launch*(Dispatchers.IO) **{** dataStoreManager.getFromDataStore().collect userDetailsText.setText(**it**.name + " " + **it**.phone +" "+ **it**.address)
    **}
}**
```

这就是关于数据存储的全部内容…

android 官方文档中提到的最后一件事- *DataStore 只是共享首选项的替代品，如果存储大量数据和进行复杂计算，它总是建议使用房间数据库。*

为了更好的理解，我把所有的类都粘贴在这里。

## DataStoreManager:-

```
class DataStoreManager constructor(val context: Context) {

    private val Context.*dataStore*: DataStore<Preferences> by *preferencesDataStore*(name = "user")

    companion object {
        val NAME = *stringPreferencesKey*("NAME")
        val PHONE_NUMBER = *stringPreferencesKey*("PHONE_NUMBER")
        val ADDRESS = *stringPreferencesKey*("ADDRESS")
    }

    suspend fun savetoDataStore(phonebook: PhoneBook) {
        context.*dataStore*.edit **{
            it**[NAME] = phonebook.name
            **it**[PHONE_NUMBER] = phonebook.phone
            **it**[ADDRESS] = phonebook.address
        **}** }

    suspend fun getFromDataStore() = context.*dataStore*.data.*map* **{** PhoneBook(
            name = **it**[NAME]?:"",
            phone = **it**[PHONE_NUMBER]?:"",
            address = **it**[ADDRESS]?:""
        )
    **}** }
```

## 主要活动:-

```
class MainActivity: AppCompatActivity() {

    lateinit var dataStoreManager: DataStoreManager

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.*activity_main*)

        dataStoreManager = DataStoreManager(this@MainActivity)

        GlobalScope.*launch*(Dispatchers.IO) **{** dataStoreManager.getFromDataStore().collect **{** dataStoreManager.savetoDataStore(PhoneBook("Sharon", "3526281", "Bangalore"))
                phoneBookDetails.setText(**it**.name + " " + **it**.phone +" "+ **it**.address)
            **}
        }** }

    override fun onDestroy() {
        super.onDestroy()
    }
}
```

## 谢谢大家！..