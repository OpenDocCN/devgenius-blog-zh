# 数据访问对象设计模式

> 原文：<https://blog.devgenius.io/data-access-object-design-pattern-96fdf484ae0a?source=collection_archive---------0----------------------->

# 概观

数据访问对象或 DAO 用于将所使用的低级数据访问 api 与高级数据访问 API 分开。道有三个部分:

*   **数据访问对象接口** —该接口包含可以在模型上执行的操作。
*   **数据访问对象类** —该类实现接口。类是从数据库或任何存储数据的地方检索数据的。
*   **模型对象** —该对象是一个简单的老式 Java 对象(POJO ),带有 getters 和 setters 来存储数据。要访问这个对象，你需要使用 DAO 类。

![](img/fb95c5e5840599641915e63f46934143.png)

照片由[卢卡斯·布拉塞克](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral)在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上拍摄

# 履行

## 模型对象

```
// Employee.java
public class Employee {
   private String name;
   private int id;

   Employee(String name, int id){
      this.name = name;
      this.species = species;
   }

   public String getName() {
      return this.name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public int getId() {
      return this.id;
   }

   public void setId(int id) {
      this.id = id;
   }
}
```

## **数据访问对象接口**

```
// EmployeeDao.java
import java.util.List;

public interface PetDao {
   public List<Employee> getAllEmployees();
   public Employee getEmployee(int id);
   public void updateEmployee(Employee employee);
   public void deleteEmployee(Employee employee);
}
```

## **数据访问对象类**

```
// Employee*DaoImpl.java*
import java.util.ArrayList;
import java.util.List;

public class EmployeeDaoImpl implements EmployeeDao {

   // list as a database
   List<Employee> employees;

   public EmployeeDaoImpl(){
      employees = new ArrayList<Employee>();
      Employee employees1 = new Employee("Mark",0);
      Employee employees2 = new Employee("Bob",1);
      employees.add(employees1);
      employees.add(employees2);		
   } @Override
   public void deleteEmployee(Employee employee) {
      employees.remove(employee.getId());
      System.out.println(employee.getId() + " deleted.");
   }

   @Override
   public List<Employee> getAllEmployees() {
      return this.employees;
   }

   @Override
   public Employee getEmployee(int id) {
      return employees.get(id);
   }

   @Override
   public void updateEmployee(Employee employee) {
      employees.get(employee.getId()).setName(employee.getName());
      System.out.println(employee.getId() + " updated.");
   }
}
```

## 示范

```
// Demo.java
public class Demo {
   public static void main(String[] args) {
      EmployeeDao employeeDao = new EmployeeDaoImpl();

      //print all employees
      for (Employee employee : employeeDao.getAllEmployees()) {
         System.out.println("Id: " + employee.getId() + ", Name: " + employee.getName());
      }

      // get employee
      Employee employee = employeeDao.getEmployee(0);
      employee.setName("Michael");
      System.out.println("Id: " + employee.getId() + ", Name: " + employee.getName());
      // update employee
      employeeDao.updateEmployee(employee);
      System.out.println("Id: " + employee.getId() + ", Name: " + employee.getName());
}>> Id: 0, Name: Mark
>> Id: 1, Name: Bob
>> Id: 0, Name: Mark
>> 0 updated.
>> Id: 0, Name: Michael
```

# 结论

当您需要更改数据库时，DAO 非常有用。例如，如果您需要从 SQL 转换到 Mongo，唯一需要做的更改是在 DAO 层。逻辑的分离确保了只有服务层依赖于 DAO 层，而不是视图。为单个组件编写测试更容易。DAO 还强调使用接口，这是 OOP 编程的一部分。