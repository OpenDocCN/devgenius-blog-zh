# 实施房间数据库

> 原文：<https://blog.devgenius.io/implementing-room-database-bc9e4deb6600?source=collection_archive---------3----------------------->

安卓| JAVA

![](img/1910fcf75a634f8afdb5f400b46e44f0.png)

房间数据库是一个持久性库。

**数据持久化:-** 指应用程序从非易失性存储系统中持久化和检索信息。

房间数据库是一种使用 SQL lite 的流畅方式，它更简单，减少了样板文件。room persistence 库在 SQL lite 上提供了一个抽象层，允许流畅的数据库访问，同时利用 SQL lite 的全部功能。

*   SQL 查询的编译时验证。
*   说服注释最大限度地减少重复和容易出错的样板代码。
*   简化的数据库迁移路径。

***我已经用房间数据库创建了一个 TODO 应用程序。请查看视频，找到下面的 Github 链接。***

## 1.设置

要设置房间数据库，请将以下依赖项添加到 app.gradle 并同步项目

```
*//Room Database* implementation "androidx.room:room-runtime:2.2.5"
annotationProcessor "androidx.room:room-compiler:2.2.5"
```

## 2.正在创建数据库

我们从创建 AppDatabase 类开始。这个类需要扩展房间数据库类。扩展后，我们将看到 3 个函数被这个房间数据库类覆盖。这个类是房间的主要类。

我们必须添加一个名为@database 的注释来保存模型类的版本和导出方案。

```
@Database(entities = {Task.class}, version = 1, exportSchema = false)
public  abstract class AppDatabase extends RoomDatabase {

    public abstract OnDataBaseAction dataBaseAction();
    private static volatile AppDatabase *appDatabase*;

    @NonNull
    @Override
    protected SupportSQLiteOpenHelper createOpenHelper(DatabaseConfiguration config) {
        return null;
    }

    @NonNull
    @Override
    protected InvalidationTracker createInvalidationTracker() {
        return null;
    }

    @Override
    public void clearAllTables() {

    }
}
```

下一个类是数据库客户机，它将是执行所有操作的访问点，如插入、删除、选择、创建、更新等。这个类的构造函数将初始化房间数据库，我们需要在这里添加数据库名称。我的数据库的名称是“task.db”。

```
public class DatabaseClient {
    private Context mCtx;
    private static DatabaseClient *mInstance*;

    *//our app database object* private AppDatabase appDatabase;

    private DatabaseClient(Context mCtx) {
        this.mCtx = mCtx;
        appDatabase = Room.*databaseBuilder*(mCtx, AppDatabase.class, "Task.db")
                .fallbackToDestructiveMigration()
                .build();
    }

    public static synchronized DatabaseClient getInstance(Context mCtx) {
        if (*mInstance* == null) {
            *mInstance* = new DatabaseClient(mCtx);
        }
        return *mInstance*;
    }

    public AppDatabase getAppDatabase() {
        return appDatabase;
    }
}
```

## 3.数据访问对象

这是一个用@Dao 注释的接口。这包含了执行操作和与表交互的所有功能。一些基本操作有**选择、插入、删除、更新**等。

```
@Dao
public interface OnDataBaseAction {

    @Query("SELECT * FROM Task")
    List<Task> getAllTasksList();

    @Query("DELETE FROM Task")
    void truncateTheList();

    @Insert
    void insertDataIntoTaskList(Task task);

    @Query("DELETE FROM Task WHERE taskId = :taskId")
    void deleteTaskFromId(int taskId);

    @Query("SELECT * FROM Task WHERE taskId = :taskId")
    Task selectDataFromAnId(int taskId);

    @Query("UPDATE Task SET taskTitle = :taskTitle, taskDescription = :taskDescription, date = :taskDate, " +
            "lastAlarm = :taskTime, event = :taskEvent WHERE taskId = :taskId")
    void updateAnExistingRow(int taskId, String taskTitle, String taskDescription , String taskDate, String taskTime,
                            String taskEvent);

}
```

## 4.数据实体

我有一个名为 Task 的实体，它基本上是我数据库中的表。这里，我的应用程序中只有一个表，根据我的要求，我必须有多个与一个相关的功能，所以一个对我来说就足够了。您可以根据您的要求添加尽可能多的表。实体必须实现 serializable，此模型中声明的所有其他变量都将是表的列。

要声明主键，我们需要用@Primarykey 对其进行注释，该值可以自动生成，也可以添加我给定的主键的唯一值，自动生成为 true，这样一旦可以通过注释声明列，任务 id 就会自动创建***@ column info(name = { column _ name })***添加 getter&setter 将帮助我们设置和检索数据。

```
@Entity
public class Task implements Serializable {

    @PrimaryKey(autoGenerate = true)
    int taskId;
    @ColumnInfo(name = "taskTitle")
    String taskTitle;
    @ColumnInfo(name = "date")
    String date;
    @ColumnInfo(name = "taskDescription")
    String taskDescrption;
    @ColumnInfo(name = "isComplete")
    boolean isComplete;
    @ColumnInfo(name = "firstAlarmTime")
    String firstAlarmTime;
    @ColumnInfo(name = "secondAlarmTime")
    String secondAlarmTime;
    @ColumnInfo(name = "lastAlarm")
    String lastAlarm;
    @ColumnInfo(name = "event")
    String event;

    public Task() {

    }
}
```

## 5.将数据插入表中

我们将使用 AsyncTask 作为 3 部分前，背景和后。对于 pre，也许我们可以添加一个进度条，并将其隐藏在帖子中，直到操作完成。主要部分将发生在 dolnBackground 函数中把所有的数据添加到模型类中并把它保存在数据库中。

```
private void createTask() {
    class saveTaskInBackend extends AsyncTask<Void, Void, Void> {
        @SuppressLint("WrongThread")
        @Override
        protected Void doInBackground(Void... voids) {
            Task createTask = new Task();
            createTask.setTaskTitle(addTaskTitle.getText().toString());
            createTask.setTaskDescrption(addTaskDescription.getText().toString());
            createTask.setDate(taskDate.getText().toString());
            createTask.setLastAlarm(taskTime.getText().toString());
            createTask.setEvent(taskEvent.getText().toString());
                DatabaseClient.*getInstance*(getActivity()).getAppDatabase()
                        .dataBaseAction()
                        .insertDataIntoTaskList(createTask);

            return null;
        }

        @Override
        protected void onPostExecute(Void aVoid) {
            super.onPostExecute(aVoid);
            if (Build.VERSION.*SDK_INT* >= Build.VERSION_CODES.*M*) {
                createAnAlarm();
            }
            setRefreshListener.refresh();
            Toast.*makeText*(getActivity(), "Your event is been added", Toast.*LENGTH_SHORT*).show();
            dismiss();

        }
    }
    saveTaskInBackend st = new saveTaskInBackend();
    st.execute();
}
```

## 6.更新表中的数据。

要更新表中的数据，我们需要获取主键，并使用主键的值找到列并更新其现有值。

```
DatabaseClient.*getInstance*(getActivity()).getAppDatabase()
                        .dataBaseAction()
                        .updateAnExistingRow(taskId, addTaskTitle.getText().toString(),
                                addTaskDescription.getText().toString(),
                                taskDate.getText().toString(),
                                taskTime.getText().toString(),
                                taskEvent.getText().toString());
```

**查询:-**

```
@Query("UPDATE Task SET taskTitle = :taskTitle, taskDescription = :taskDescription, date = :taskDate, " +
        "lastAlarm = :taskTime, event = :taskEvent WHERE taskId = :taskId")
```

## 7.从表中获取数据

一个非常常见的操作是获取表格并在屏幕上显示它，为此我们通常使用查询，如 ***SELECT * FROM Task*** 或***SELECT * FROM taskId =:taskId***此查询将被添加到我们将调用的函数中，并将数据添加到 arraylist 中并显示数据。在我的例子中，我使用 recyclerView 来显示数据。

**查询:-**

```
@Query("SELECT * FROM Task WHERE taskId = :taskId")
Task selectDataFromAnId(int taskId);@Query("SELECT * FROM Task")
List<Task> getAllTasksList();
```

**代码:-**

```
private void getSavedTasks() {

    class GetSavedTasks extends AsyncTask<Void, Void, List<Task>> {
        @Override
        protected List<Task> doInBackground(Void... voids) {
            tasks = DatabaseClient
                    .*getInstance*(getApplicationContext())
                    .getAppDatabase()
                    .dataBaseAction()
                    .getAllTasksList();
            return tasks;
        }

        @Override
        protected void onPostExecute(List<Task> tasks) {
            super.onPostExecute(tasks);
            noDataImage.setVisibility(tasks.isEmpty() ? View.*VISIBLE* : View.*GONE*);
            setUpAdapter();
        }
    }

    GetSavedTasks savedTasks = new GetSavedTasks();
    savedTasks.execute();
}
```

## 8.从表中删除列。

这个操作可以删除一列或删除整个表。

**查询:-**

```
@Query("DELETE FROM Task")
void truncateTheList();@Query("DELETE FROM Task WHERE taskId = :taskId")
void deleteTaskFromId(int taskId);
```

**代码:-**

```
private void deleteTaskFromId(int taskId, int position) {
    class GetSavedTasks extends AsyncTask<Void, Void, List<Task>> {
        @Override
        protected List<Task> doInBackground(Void... voids) {
            DatabaseClient.*getInstance*(context)
                    .getAppDatabase()
                    .dataBaseAction()
                    .deleteTaskFromId(taskId);

            return taskList;
        }

        @Override
        protected void onPostExecute(List<Task> tasks) {
            super.onPostExecute(tasks);
            removeAtPosition(position);
            setRefreshListener.refresh();
        }
    }
    GetSavedTasks savedTasks = new GetSavedTasks();
    savedTasks.execute();
}
```

## 房间数据库就这些。我正在下面添加 Github 链接。

[https://github.com/Sha489/todo-list](https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbE81YkVqT1hZZXpKNWt1MUJXcTlXcVlSZk51Z3xBQ3Jtc0ttME9RWFhyV210M3loNm12UklMdTRSS1ZucWFzU0Z5TTdkamFVclFFRHVsbU1ZdTRsN0kwcjVkUzN1TDZCMGtpaGhNZWlrVG9PY1pxYm8yX0JsTi01MGs4WUNVTm0zRXZoMEJheHFLRUFjZW1USkxEcw&q=https%3A%2F%2Fgithub.com%2FSha489%2Ftodo-list&v=_qmU3tUQTBk)

## 谢谢大家！