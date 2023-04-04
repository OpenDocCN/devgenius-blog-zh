# Java 最佳实践:布尔与异常

> 原文：<https://blog.devgenius.io/java-best-practice-boolean-vs-exception-f561633239cc?source=collection_archive---------3----------------------->

选择什么:方法应该返回布尔值还是抛出异常？

![](img/5afdcba8c6917cf3d6d606babb1682d9.png)

日本古代武术——十臂功

这篇文章基于一些个人对旧 Java 代码的重构，而不是假装成一个导师。

假设我们的方法应该返回 void 或 boolean。最高效的返回方式是什么，如何让代码清晰？我们应该避免将异常输出与其他输出类型混合。

让我们重构这段虚幻的混合代码:

```
public boolean checkIfAccessGrant() throws NotAuthorizedException {
    Client client = clientService.findByUserName("mumba.yumba");
    if (client != null) {
        Country country = clientService.resolveUserCountry(client);
        if (country != null) {
            return !country.isChargeUnavailable();
        } else {
            throw new NotAuthorizedException(getException(client));
        }
    } else if (isCurrentUserHasTheRole(SUPER_USER)) {
        return true;
    } else {
        throw new NotAuthorizedException(getException(client));
    }       
}
private String getException(Client client) {
    String message = "";
    if (client != null) {
        message += "Client ID: " + client.getId();
    }
    else {
        message += "No Client given.";
    }
    return message;
}
private void checkIfClientHasTheRole(Role roleName) throws NotAuthorizedException {

    Set<Role> roles = retrieveRolesByClient();
    if (roles == null || !roles.contains(role)) {
        throw new NotAuthorizedException(getException(client));
    }
}
```

无论我们使用哪种编程语言，它总是相同的相似方法。将 void 作为方法的结果返回会更清楚。在负面情况下—抛出异常。

所以，我们重构的代码:

```
public void checkIfAccessGranted() throws NotAuthorizedException { Client client = clientService.findByUserName("mumbai");
    if (client != null) {
       Country country = clientService.resolveUserCountry(client);
       if (country == null || country.isChargeUnavailable()) {
          throw new NotAuthorizedException(getException(client));
       }
    } else {
       checkIfClientHasTheRole(MAINTENANCE_ROLE);
    }
}
```

当更多的全局函数调用这个方法 checkIfAccessGranted()时，不需要返回 boolean，检查异常就足够了。没有抛出异常，所以结论为真。

```
boolean isSuccessful = true;
try {
    service.checkIfAccessGranted(*MANAGER_ROLE*);
} catch (NotAuthorizedException e) {
    *LOG*.info(e);
    isSuccessful = false;
} finally {
    // add some measurement for statistics or clear the unused vars
}
```

当执行陷入停滞且没有替代方案时引发异常。但是我们只能在特定的地方抓住它们。这可能是从一层到另一层的大跳跃。

如果有必要在检查的每一步都使用“return”关键字，那么在最后一行也添加最终的 return。SonarQube 会坚持这个规则，他是对的。举例来说:

```
public Class receiveGenericClass() {
    try {
        return Class.forName(GENERIC_CLASS_NAME);
    } catch (ClassNotFoundException e) {
        LOG.error("No generic Logger Class found.");
    }
    return null;
}
```

还要添加一些代码覆盖率的单元测试。

```
@Autowired
private ClientAccessService serviceUnderTest;
@Autowired
private ClientService clientService;@Test
public void shouldCheckForSSOUserAndWhenCountryNull() {
    // given
    Client client = new Client();
    client.setId("superuser");
    client.setName("Lord"); // when
    *Mockito.when*(clientService.findByUserName("superuser")).
        thenReturn(client);... we can mock any real method to provide fake response for test // then exception was thrown
    *assertThrows*(NotAuthorizedException.class, () ->
        serviceUnderTest.checkIfAccessGranted());
}
```

![](img/e176fda04924f0d8c9985a5733b9c068.png)

另一个捕捉“异常”的设备——Kabutowari。用作头盔断路器。

**assert not throws——罕见的测试方法。**

有时有必要测试一下情况，因为没有抛出异常。或者我们只需要在特定的条件下抛出。实现依赖于 JUnit 版本或其他测试库。

最简单明了的方法:

```
try {
    serviceUnderTest.checkIfAccessGranted();
} catch (Exception e) {
    Assert.*fail*("Should not throw exception.");
}
```

在 JUnit 5.20 版本之后，我们可以使用本地方法`assertDoesNotThrow`:

```
@Test
public void shouldMethodNotThrowException(){ assertDoesNotThrow(myObject::methodToBeTested); org.junit.jupiter.api.Assertions.assertDoesNotThrow(()-> method());
}
```

**索纳库**

这也可能是 SonarQube 的问题，就像这样:“要么记录，要么重新抛出这个异常”。它用严格的规则检查代码。如果我们捕捉到异常，无论如何都应该处理它。

```
try {
    checkIfClientHasTheRole(*MANAGER_ROLE*);
} catch (NotAuthorizedException e) {
    *LOG*.info(e);
    //or
    throw new NotAuthorizedException(getException(client));
}
```

![](img/8501d1185cfa07b389f3e9c562a51d45.png)

朱特能抓住武士刀，理论上是可能的。不要试图实际去做！

**结论**。

选择哪种方法总是一个品味的问题。有时抛出异常比返回布尔值更好。否则。取决于任务逻辑。想法是基于这样一个事实，使代码风格清晰明了。

**链接**

 [## JUnit 5 发行说明

### 发布日期:2018 . 4 . 29 范围:JUnit BOM，支持 Maven Surefire 2.21.0 允许用 Java 9 和…

junit.org](https://junit.org/junit5/docs/5.2.0/release-notes/#new-features-and-improvements-2)