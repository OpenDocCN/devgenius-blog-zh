# 传输对象模式

> 原文：<https://blog.devgenius.io/transfer-object-pattern-cc039741df9f?source=collection_archive---------7----------------------->

# 概观

传输对象模式的主要用途是一次传递具有多个属性的数据。它由 3 部分组成:

*   传输对象——简单的老式 Java 对象(POJO ),只有 getters 和 setters
*   业务对象—包含操作传输对象的逻辑。
*   客户端—从业务对象传递或获取传输对象

![](img/65e5ceeb7a08aee3fea8e02be9b76137.png)

马库斯·温克勒在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 履行

## 转移对象

PersonTO.java

```
public class PersonTO {
   private String name;
   private int id;

   PersonTO(String name, int id){
      this.name = name; 
      this.id = id
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   } public String getId() {
      return id;
   }

   public void setId(String id) {
      this.id = id;
   }
}
```

## 业务对象

PersonBO.java

```
import java.util.ArrayList;
import java.util.List;

public class PersonBO {
   // database
   List<PersonTO> people;

   public PersonBO(){
      people = new ArrayList<PersonTO>();
      PersonTO p1 = new PersonTO("Gary", 0);
      PersonTO p2 = new PersonTO("Sam", 1);
      people.add(p1);
      people.add(p2);		
   }
   public void deletePerson(PersonTO person) {
      people.remove(person.getId());
      System.out.println(person.getId() + ". " + person.getName() + " deleted");
   }

   public List<PersonTO> getAllPeople() {
      return people;
   }

   public PersonTO getPerson(int id) {
      return people.get(id);
   }

   public void updatePerson(PersonTO person) {
      people.get(person.getId()).setName(person.getName());
      System.out.println("Id " + person.getId() + " updated");
   }
}
```

## 客户

Demo.java

```
public class Demo {
   public static void main(String[] args) {
      PersonBO personBusinessObject = new PersonBO();// Print all people
      for (PersonTO person : personBusinessObject.getAllPeople()) {
         System.out.println("Id : " + person.getId() + ", Name : " + person.getName());
      }

      // Update person
      PersonTO person = personBusinessObject.getAllPeople().get(0);
      person.setName("Jim");
      personBusinessObject.updatePerson(person);

      // Get person
      person = personBusinessObject.getPerson(0);
      System.out.println("Id : " + person.getId() + ", Name : " + person.getName());
   }
}>> Id: 0, Name: Gary
>> Id: 1, Name: Same
>> Id 0 updated
>> Id: 0, Name: Jim
```

# 结论

使用数据传输模式，您可以将多个呼叫组合在一起。在上面的例子中，它检索、更新和删除。然而，它不仅仅限于那些行动。如果客户端请求多个信息并且它们是相关的，那么应该使用这种模式来提高检索资源的性能，并且您希望减少调用次数。