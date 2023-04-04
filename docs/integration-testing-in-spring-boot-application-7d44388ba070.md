# 集成测试在 Spring Boot 应用中的应用

> 原文：<https://blog.devgenius.io/integration-testing-in-spring-boot-application-7d44388ba070?source=collection_archive---------2----------------------->

在这篇文章中，我将展示如何在 Spring Boot 应用程序中添加集成测试。

集成测试在确保应用程序质量方面起着关键作用。有了像 Spring Boot 这样的框架，集成这样的测试就更容易了。然而，用集成测试来测试应用程序，而不是将它们部署到应用服务器上，这一点很重要。

集成测试有助于测试应用程序的数据访问层。集成测试也有助于测试多个单元。对于 Spring Boot 应用程序，我们需要在`ApplicationContext`中运行一个应用程序来运行测试。集成测试也可以帮助测试[异常处理](https://betterjavacode.com/programming/controller-advice-exception-handler-in-spring-boot)。

# Spring Boot 应用

对于这个演示，我们将使用 REST APIs 构建一个简单的 Spring Boot 应用程序。我们将使用 H2 内存数据库来存储数据。最后，我将展示如何编写一个集成测试。该应用程序从国家漏洞数据库中读取一个 JSON 漏洞文件，并将其存储在 H2 数据库中。REST APIs 允许用户以更易读的格式获取数据。

# 属国

首先，我们希望在这个应用程序中构建集成测试，所以我们需要包含依赖关系`spring-boot-starter-test`。

```
dependencies { 
implementation 'org.springframework.boot:spring-boot-starter-data-jpa' 
implementation 'org.springframework.boot:spring-boot-starter-web' implementation 'junit:junit:4.13.1' 
runtimeOnly 'com.h2database:h2:1.4.200' 
testImplementation 'org.springframework.boot:spring-boot-starter-test' 
}
```

`spring-boot-starter-test`的这种依赖性允许我们添加测试相关的注释，我们很快就会看到。

# REST API

正如我之前所说，我们将有一个 REST API 来获取国家漏洞数据库数据。我们将创建一个带有两个 API 的 REST 控制器，一个获取漏洞列表，另一个通过 CVE id 获取漏洞。

```
@RestController
@RequestMapping("/v1/beacon23/vulnerabilities")
public class CveController
{

    @Autowired
    private CveService cveService;

    @GetMapping("/list")
    public List getAllCveItems(@RequestParam(required = false, name="fromDate") String fromDate, @RequestParam(required = false, name=
            "toDate") String toDate)
    {
        List cveDTOList = cveService.getCveItems(fromDate, toDate);

        if(cveDTOList == null || cveDTOList.isEmpty())
        {
            return new ArrayList<>();
        }
        else
        {
            return cveDTOList;
        }
    }

    @GetMapping
    public ResponseEntity getCveItemById(@RequestParam("cveId") String cveId)
    {
        CveDTO cveDTO = cveService.getCveItemByCveId(cveId);

        if(cveDTO != null)
        {
            return new ResponseEntity<>(cveDTO, HttpStatus.OK);
        }
        else
        {
            return new ResponseEntity<>(HttpStatus.NO_CONTENT);
        }
    }

}
```

所以我们有

*   /v1/beacon 23/vulnerabilities/list—获取漏洞列表
*   /v1/beacon 23/漏洞？cveId = value 通过 cveId 获取漏洞。

# 服务

现在，大多数业务逻辑和验证都发生在服务类中。正如我们在 API 中看到的，我们使用`CVEService`来获取所需的数据。

```
 @Autowired
    public CveDataDao cveDataDao;

    public List getCveItems(String from, String to)
    {
        LOGGER.debug("The date range values are from = {} and to = {}", from, to);
        List cveDataList = cveDataDao.findAll();
        List cveDTOList = new ArrayList<>();

        for(CveData cveData : cveDataList)
        {
            List cveList = cveData.getCveItems();
            for(CveItem cveItem: cveList)
            {
                Date fromDate;
                Date toDate;

                if(!isNullOrEmpty(from) && !isNullOrEmpty(to))
                {
                    fromDate = DateUtil.formatDate(from);
                    toDate = DateUtil.formatDate(to);

                    Date publishedDate = DateUtil.formatDate(cveItem.getPublishedDate());

                    if(publishedDate.after(toDate) || publishedDate.before(fromDate))
                    {
                        continue;
                    }
                }
                CveDTO cveDTO = convertCveItemToCveDTO(cveItem);
                cveDTOList.add(cveDTO);
            }
        }
        return cveDTOList;
    }

    private boolean isNullOrEmpty (String str)
    {
        return (str == null || str.isEmpty());
    }

    private String buildDescription (List descriptionDataList)
    {
        if(descriptionDataList == null || descriptionDataList.isEmpty())
        {
            return EMPTY_STRING;
        }
        else
        {
            return descriptionDataList.get(0).getValue();
        }
    }

    private List buildReferenceUrls (List referenceDataList)
    {
        return referenceDataList.stream().map(it -> it.getUrl()).collect(Collectors.toList());
    }

    public CveDTO getCveItemByCveId(String cveId)
    {
        List cveDataList = cveDataDao.findAll();
        CveDTO cveDTO = null;

        for(CveData cveData : cveDataList)
        {
            List cveItems = cveData.getCveItems();

            Optional optionalCveItem =
                    cveItems.stream().filter(ci -> ci.getCve().getCveMetadata().getCveId().equals(cveId)).findAny();
            CveItem cveItem = null;
            if(optionalCveItem.isPresent())
            {
                cveItem = optionalCveItem.get();
            }
            else
            {
                return cveDTO;
            }
            cveDTO = convertCveItemToCveDTO(cveItem);
        }

        return cveDTO;
    }
```

# @SpringBootTest 的用法

Spring Boot 提供了一个注释`@SpringBootTest`，我们可以在集成测试中使用它。有了这个注释，测试可以启动应用程序上下文，该上下文可以包含应用程序运行所需的所有对象。

集成测试提供了一个几乎类似于生产的场景来测试我们的代码。用`@SpringBootTest`标注的测试通过用`@SpringBootConfiguration`标注的应用程序类创建了测试中使用的应用程序上下文。

这些测试启动一个嵌入式服务器，创建一个 web 环境，然后运行`@Test`方法进行集成测试。我们需要添加一些属性来确保我们可以在使用`@SpringBootTest`时启动 web 环境。

我们也可以通过一个活动的概要文件来传递用于测试的属性。通常，我们为不同的环境使用这些概要文件，但是我们也可以只为测试使用一个特殊的概要文件。我们创建`application-dev.yml`、`application-prod.yml`配置文件。类似地，我们可以创建`application-test.yml`并在测试中使用注释`@ActiveProfiles('test')`。

# 集成测试示例

对于我们的 REST API，我们将创建一个集成测试来测试我们的控制器。我们还将使用`TestRestTemplate`来获取数据。该集成测试将如下所示:

```
package com.betterjavacode.beacon23.tests;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.boot.web.server.LocalServerPort;
import org.springframework.http.HttpEntity;
import org.springframework.http.HttpHeaders;
import org.springframework.http.HttpMethod;
import org.springframework.http.ResponseEntity;
import org.springframework.test.context.junit4.SpringRunner;

import static org.junit.Assert.assertNotNull;

@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class CveControllerTest
{
    @LocalServerPort
    private int port;

    TestRestTemplate testRestTemplate = new TestRestTemplate();

    HttpHeaders headers = new HttpHeaders();

    @Test
    public void testGetAllCveItems()
    {
        HttpEntity entity = new HttpEntity<>(null, headers);

        ResponseEntity responseEntity = testRestTemplate.exchange(createURLWithPort(
                "/v1/beacon23/vulnerabilities/list"),HttpMethod.GET, entity, String.class);

        assertNotNull(responseEntity);

    }

    private String createURLWithPort(String uri)
    {
        return "http://localhost:" + port + uri;
    }
}
```

我们为测试类使用了`@SpringBootTest`注释，并通过使用带有 RANDOM_PORT 的`webEnvironment`来设置应用程序上下文。我们还通过用`@LocalServerPort`设置一个模拟端口来模拟本地 web 服务器。

`TestRestTemplate`允许我们模拟一个将调用我们的 API 的客户端。一旦我们运行这个测试(通过`gradle build`或者通过 IntelliJ ),我们将看到 Spring Boot 应用程序上下文设置正在运行，应用程序在一个随机端口上运行。

用`@SpringBootTest`创建集成测试的一个缺点是它会降低构建应用程序的速度。在大多数企业环境中，您将通过持续集成和持续部署来实现这一点。在这种情况下，如果您有许多集成测试，它会减慢集成和部署的过程。

# 结论

最后，您应该在 Spring Boot 应用程序中使用集成测试，这取决于您的应用程序。但是尽管有缺点，允许一次测试多个单元的集成测试总是有用的。`@SpringBootTest`是一个方便的注释，可用于设置应用程序上下文，允许我们在接近生产环境的情况下运行测试。

*原载于 2021 年 6 月 19 日 https://betterjavacode.com*[](https://betterjavacode.com/programming/integration-testing-in-spring-boot-application)**。**