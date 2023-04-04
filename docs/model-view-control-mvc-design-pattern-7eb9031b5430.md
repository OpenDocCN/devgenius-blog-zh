# 模型视图控制(MVC)设计模式

> 原文：<https://blog.devgenius.io/model-view-control-mvc-design-pattern-7eb9031b5430?source=collection_archive---------3----------------------->

# 概观

顾名思义，MVC 设计模式包括 3 个部分:

*   模型—包含数据的对象。它不应该包含显示数据的逻辑。
*   视图—向用户显示数据。它应该只有显示数据的逻辑，而没有操作数据的逻辑。
*   控制器(controller )-控制模型和视图之间的数据流，使两者保持分离。视图触发导致控制器改变模型的事件。

![](img/b74c3eedb9c1c08cfd73a63d11d7b99a.png)

在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上由[halance](https://unsplash.com/@halacious?utm_source=medium&utm_medium=referral)拍摄的照片

# 履行

## 模型

```
// Employee.java
public class Employee {
   private int id;
   private String name;

   public String getId() {
      return rollNo;
   }

   public void setId(int id) {
      this.id = id;
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }
}
```

该模型包含必要的信息，包括 getters 和 setters。模型本身不需要考虑数据将如何显示，只需要关心模型数据的操作。

## 视角

```
// EmployeeView.java
public class EmployeeView {
   public void printEmployeeDetails(String name, String id){
      System.out.println("Name: " + name);
      System.out.println("Id: " + id);
   }
}
```

在大多数 web 应用程序中，前端是显示数据的视图。

## 控制器

```
// EmployeeController.java
public class EmployeeController {
   private Employee model;
   private Employee view;

   public EmployeeController(Employee model, EmployeeView view){
      this.model = model;
      this.view = view;
   }

   public void setEmployeeName(String name){
      model.setName(name);		
   }

   public String getEmployeeName(){
      return model.getName();		
   }

   public void setEmployeeId(int id){
      model.setId(id);		
   }

   public int getEmployeeId(){
      return model.getId();		
   }

   public void updateView(){				
      view.printEmployeeDetails(model.getName(), model.getRollNo());
   }	
}
```

在 web 应用程序中，控制器通常在后端处理，前端可以调用 api。控制器负责处理这些 api 调用。

## 演示

```
// Demo.java
public class Demo {
   public static void main(String[] args) {
      // simulate retrieving employee from database
      Employee model  = getEmployee();

      // create view to display data
      EmployeeView view = new EmployeeView();

      EmployeeController controller = new EmployeeController(model, view);

      controller.updateView();

      //update model data
      controller.setStudentName("George");

      controller.updateView();
   }

   private static Employee getEmployee(){
      Employee employee = new Employee();
      employee.setName("Tom");
      employee.setId(16);
      return employee;
   }
}>> Name: Tom
>> Id: 16
>> Name: George
>> Id: 16
```

这个演示让你了解 MVC 是如何工作的。在 web 应用程序中，用户将与视图进行交互。例如，如果用户单击登录按钮，它将对控制器进行 api 调用。控制器将在找到正确的数据后检索必要的模型。控制器会将数据发送回显示数据的视图。

# 结论

MVC 模式是一种流行的设计模式，被许多语言使用，不仅仅是 Java。MVC 的一些优点包括允许多人处理代码、逻辑分离以及允许模型有多个视图。MVC 的一些缺点包括难以导航代码，因为有新的抽象层，并且通常包括多种技术。