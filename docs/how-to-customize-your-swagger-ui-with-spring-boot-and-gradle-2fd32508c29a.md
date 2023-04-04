# 如何用 Spring Boot 和格雷德定制你的 Swagger UI

> 原文：<https://blog.devgenius.io/how-to-customize-your-swagger-ui-with-spring-boot-and-gradle-2fd32508c29a?source=collection_archive---------2----------------------->

![](img/38289496ef119068de5b8cbc571c9abb.png)

因此，这几天我试图找到一种方法来定制我的 Swagger UI，以替换 Swagger 徽标，并修改一些 CSS，使其更适合我的项目。由于我对 Spring 相对陌生，所以我认为它并不明显或者没有任何简单的解决方案。我发现了一些别人认为有用的代码和一些对我没有帮助的过时教程等等。所以我想分享一下我对这个问题的解决方法可能是个好主意。

在本指南中，我将向您展示如何使用 Gradle 作为构建工具来修改文件，使其在 Spring 上运行。

首先，您需要从 Maven 资源库下载 webjar 文件。你可以在这里找到这个页面:[https://mvnrepository.com/artifact/org.webjars/swagger-ui](https://mvnrepository.com/artifact/org.webjars/swagger-ui)选择合适的版本(最好是最新的)点击**文件**行上的**罐子**，它就会下载一个**。jar** 文件。

下载完文件后，您需要解压缩该文件。使用命令提取它:

```
unzip swagger-ui-<version>.jar -d ./swagger
```

该命令将解压缩。jar 文件到当前路径中的一个名为**的目录中。用您下载的版本替换该版本。**

现在你可以开始定制文件了！

当我们完成定制后，我们需要生成一个新的。jar 文件，这样我们就可以将它包含在 Gradle 的构建环境中。生成新的。jar 文件我们将使用下面的命令，在运行它时，我们需要首先进入 swagger 目录:

```
jar cf swagger-ui-<version>.jar META-INF/
```

现在我们已经构建了一个. jar 文件，所以我们需要将这个文件移动到 Spring 项目中的一个适当的目录中。在本指南中，我将把它移到根目录下名为 **libs** 的目录中。然后我们需要打开我们的 **build.gradle** 并添加一些代码。在文件的顶部，您需要一个 buildscript 块(参见下面的我的代码):

```
buildscript **{** repositories **{** mavenCentral()
      flatDir **{** dirs 'libs'
      **}
   }** dependencies **{** classpath("org.springframework.boot:spring-boot-gradle-plugin:2.5.2.RELEASE")
   **}
}**
```

这里我们将目录 **libs** 链接为一个 flatDir 存储库。我们还需要排除使用依赖关系 **springdoc-openapi-ui** 时自动出现的 swagger-ui，所以我们将使用我们自己的依赖关系。最后一行显示了我们如何添加自己的库。

```
implementation (group: 'org.springdoc', name: 'springdoc-openapi-ui', version: '<version>') **{** exclude group: 'org.webjars', module: 'swagger-ui'
**}** implementation files('libs/swagger-ui-<version.jar')
```

这就是定制 JS/HTML/CSS 的全部内容。如果我们想给 API 添加一些关于标题、描述、版本和联系信息等的配置，我们可以创建一个名为**OpenApiConfig.java**的新 Java 文件，并放置以下代码:

```
package com.qryptic.api.security;

import io.swagger.v3.oas.models.Components;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * OpenAPI Configuration for Swagger UI
 *
 * **@author** Marcus Cvjeticanin
 */
@Configuration
public class OpenApiConfig {

    @Bean
    public OpenAPI customOpenAPI(
            @Value("${apiTitle}") String apiTitle,
            @Value("${apiDescription}") String apiDescription,
            @Value("${apiVersion}") String apiVersion,
            @Value("${apiContactName}") String apiContactName,
            @Value("${apiContactEmail}") String apiContactEmail,
            @Value("${apiContactUrl}") String apiContactUrl

    ) {
        return new OpenAPI()
                .components(new Components())
                .info(new Info()
                        .title(apiTitle)
                        .description(apiDescription)
                        .version(apiVersion)
                        .contact(new Contact().name(apiContactName).email(apiContactEmail).url(apiContactUrl))
                );
    }
}
```

我们可以删除 **@Value** 注释和参数，只在返回的新 **OpenAPI()** 中添加字符串值。但是我们可以使用上面的代码，并在 **application.yml** 中添加以下内容:

```
apiTitle: "Qryptic API"
apiDescription: "Qryptic is a place for crypto enthusiasts to discover cryptocurrencies, wallets, exchanges and much more."
apiVersion: "1.0.0"
apiContactName: "Marcus Cvjeticanin"
apiContactEmail: "mjovanc@protonmail.com"
apiContactUrl: "https://qryptic.net"
```

这就是我们需要做的！如果你有任何问题或想与我联系，请在 Twitter 上关注我:[https://twitter.com/mjovanc](https://twitter.com/mjovanc)