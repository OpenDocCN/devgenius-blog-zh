# 业务委托设计模式

> 原文：<https://blog.devgenius.io/business-delegate-design-pattern-1d2c888bd470?source=collection_archive---------2----------------------->

# 概观

业务委托用于分离业务端和表示端。业务方面由 4 部分组成:

*   客户端—显示数据的表示代码
*   业务委托—提供对客户端使用的业务服务方法的访问
*   查找服务—负责获取相关的业务实现，并提供业务对象对业务委托对象的访问
*   业务服务——具体的类实现业务服务接口，以提供业务实现逻辑

![](img/98980370b8e76de424cebb9c0ca56ed6.png)

查尔斯先行者在 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的照片

# 履行

## BusinessService.java

```
public interface BusinessService {
   public void doSomething();
}
```

具体类实现的业务接口

## ServiceOne.java

```
public class ServiceOne implements BusinessService {
   @Override
   public void doSomething() {
      System.out.println("Something One!");
   }
}
```

## ServiceOne.java

```
public class ServiceTwo implements BusinessService {
   @Override
   public void doSomething() {
      System.out.println("Something Two!");
   }
}
```

实现业务接口的具体业务服务。

## LookUp.java

```
public class LookUp {
   public BusinessService getBusinessService(String serviceType){
      if(serviceType.equalsIgnoreCase("one")){
         return new ServiceOne();
      }
      else {
         return new ServiceTwo();
      }
   }
}
```

查找服务根据提供的字符串提供具体的业务服务。

## BusinessDelegate.java

```
public class BusinessDelegate {
   private LookUp lookupService = new LookUp();
   private BusinessService businessService;
   private String serviceType;

   public void setServiceType(String serviceType){
      this.serviceType = serviceType;
   }

   public void doTask(){
      businessService = lookupService.getBusinessService(serviceType);
      businessService.doSomething();		
   }
}
```

业务委托是客户端用来访问业务服务的。客户端可以根据服务类型更改要使用的服务。

## Client.java

```
public class Client {
   BusinessDelegate businessDelegate;

   public Client(BusinessDelegate businessDelegate){
      this.businessDelegate = businessDelegate;
   }

   public void doTask(){		
      businessService.doTask();
   }
}
```

## Demo.java

```
public class Demo {
   public static void main(String[] args) {
      BusinessDelegate businessDelegate = new BusinessDelegate();
      businessDelegate.setServiceType("one");

      Client client = new Client(businessDelegate);
      client.doTask();

      businessDelegate.setServiceType("two");
      client.doTask();
   }
}>> Something One!
>> Something Two!
```

# 结论

业务委托设计模式在企业 Java Beans(EJB)的早期更流行。现在随着 Spring 和依赖注入的流行，这种设计模式已经失去了很多意义。但是当它被使用时，它减少了与业务和客户端的耦合，并且它隐藏了业务服务的实现。然而，它增加了额外的一层，因此增加了维护代码所需的工作量。