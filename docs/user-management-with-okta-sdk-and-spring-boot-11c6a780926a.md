# 使用 Okta SDK 和 Spring Boot 进行用户管理

> 原文：<https://blog.devgenius.io/user-management-with-okta-sdk-and-spring-boot-11c6a780926a?source=collection_archive---------6----------------------->

在这篇文章中，我将展示我们如何用 Okta SDK 和 Spring Boot 构建用户管理和认证。

# 介绍

作为任何应用程序的一部分，开发人员必须小心如何构建身份验证。尽管我们长期以来一直使用基于表单的身份验证，但它并不是最安全的。在这篇文章中，我计划展示基于表单的身份验证，在这种情况下，用户不必通过对照数据库中存储的密码来验证他们的加密密码。如果你想了解更多关于不同认证流程的 Spring 安全性，我最近发布了一本书[简化 Spring 安全性](https://betterjavacode.com/programming/simplifying-spring-security)。你可以在这里买到这本书[。](https://gumroad.com/l/VgSdH)

Okta 是一个身份提供商。这是一个应用程序，提供不同协议的用户管理和认证。

# Okta SDK APIs

Okta 为用户管理 API 和认证提供了两个库`okta-sdk-java`和`okta-auth-java`。

这些图书馆适合你吗？这取决于您的用例。Okta 还提供了`okta-spring-boot-starter`库，在您的 Spring Boot 应用程序中为不同的 OAuth 流使用 okta。我们不会在这个演示中使用这个库。

你可以在这里和这里找到关于这些库[的更多细节。](https://github.com/okta/okta-sdk-java)

将这些库包括在项目中，如下所示:

```
implementation 'com.okta.authn.sdk:okta-authn-sdk-api:2.0.1' runtimeOnly 'com.okta.authn.sdk:okta-authn-sdk-impl:2.0.1' runtimeOnly 'com.okta.sdk:okta-sdk-httpclient:3.0.1'
```

# Okta SDK 在 Spring Boot 应用中的用户管理

在这个演示中，我有一个待办事项列表的示例应用程序。当用户启动应用程序时，用户将看到一个登录屏幕。它有注册选项。如果用户在应用程序中不存在，用户必须创建一个帐户。

在注册页面，当用户输入“提交”按钮时，我们会将用户保存在我们的数据库中，然后调用 Okta SDK API 在 Okta 端创建用户。

要实现这一点，我们需要 Okta 客户端。

```
 @Bean
    public Client client()
    {

        Client clientConfig =
                Clients.builder().setOrgUrl("https://oktadomainurl").setClientCredentials(new TokenClientCredentials(secret))
                        .build();

        return clientConfig;

    }
```

正如您在上面看到的，我们正在创建一个客户端，我们将使用它来调用 Okta API。“秘密”是你可以在 Okta 管理界面中找到的 API 令牌。如果您找不到它，要么您没有管理员权限，要么您尚未创建令牌。还有另一种方法来创建带有访问令牌的客户机。

```
 @Bean
    public Client client()
    {

        Client clientConfig =
                Clients.builder().setOrgUrl("https://oktadomainurl")
                      .setAuthorizationMode(AuthorizationMode.PRIVATE_KEY).setClientId("{clientId}")
                      .setScopes(new HashSet<>(Arrays.asList("okta.users.read", "okta.apps.read")))
                      .setPrivateKey("/path/to/yourPrivateKey.pem")

        return clientConfig;

    }
```

这种客户机配置的优点是，您不需要知道基于管理员权限创建的 API 访问令牌。

现在，在我的控制器端，我将使用这个客户端在 Okta 中创建用户，如下所示:

```
 UserDto userDto = new UserDto();
        userDto.setEmail(email);
        userDto.setFirstName(firstname);
        userDto.setLastName(lastname);
        userDto.setPassword(encodedPassword);
        userDto.setRole("ADMIN");
        userDto.setEnabled(true);

        UserDto returnedUser = usersManager.createUser(userDto);

        LOGGER.info("Create the user in Okta");

        User oktaUser = UserBuilder.instance().setEmail(returnedUser.getEmail())
                .setFirstName(returnedUser.getFirstName())
                .setLastName(returnedUser.getLastName())
                .buildAndCreate(client);
```

这包括了用户管理部分。您可以类似地调用`GET`或`DELETE` API 来管理用户。

# 用户认证

现在到了认证的关键部分。在许多企业应用中，当使用第三方身份提供者时，麻烦总是伴随着用户数据同步。这两个应用程序都需要存储用户数据。

对于认证，我们将需要`authenticationClient` bean。这个客户端将允许我们调用 Okta API 进行身份验证。

```
 @Bean
    public AuthenticationClient authenticationClient()
    {
        AuthenticationClient authenticationClient =
                AuthenticationClients.builder()
                        .setOrgUrl("https://oktadomainurl")
                        .build();

        return authenticationClient;
    }
```

在我们的安全配置中，我将用一个定制的登录页面覆盖基于表单的登录。

```
 @Autowired
    private CustomAuthenticationProvider customAuthenticationProvider;

    @Bean(BeanIds.AUTHENTICATION_MANAGER)
    @Override
    public AuthenticationManager authenticationManagerBean() throws Exception
    {
        return super.authenticationManagerBean();
    }

    @Override
    protected void configure(HttpSecurity httpSecurity) throws Exception
    {

        httpSecurity.authorizeRequests()
                .antMatchers("/js/**","/css/**","/img/**").permitAll()
                .antMatchers("/signup","/forgotpassword").permitAll()
                .anyRequest().authenticated()
                .and()
                .formLogin()
                .loginPage("/login")
                .permitAll();

    }
```

正如你在上面的代码中看到的，我正在使用`customAuthenticationProvider`，这个提供者将使用`authenticationClient`与 Okta 进行认证。此 AuthenticationProvider 将如下所示:

```
package com.betterjavacode.sss.todolist.clients;

import com.betterjavacode.sss.todolist.security.AuthenticationStateHandler;
import com.okta.authn.sdk.client.AuthenticationClient;
import com.okta.authn.sdk.resource.AuthenticationResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.security.authentication.AuthenticationProvider;
import org.springframework.security.authentication.UsernamePasswordAuthenticationToken;
import org.springframework.security.core.Authentication;
import org.springframework.security.core.AuthenticationException;
import org.springframework.security.core.GrantedAuthority;
import org.springframework.security.core.authority.SimpleGrantedAuthority;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.stereotype.Component;

import java.util.ArrayList;
import java.util.List;

@Component
public class CustomAuthenticationProvider implements AuthenticationProvider
{

    private static final Logger LOGGER = LoggerFactory.getLogger(CustomAuthenticationProvider.class);

    @Autowired
    private AuthenticationClient authenticationClient;

    @Autowired
    private AuthenticationStateHandler authenticationStateHandler;

    @Override
    public Authentication authenticate (Authentication authentication) throws AuthenticationException
    {
        String username = authentication.getName();
        String password = authentication.getCredentials().toString();
        String relayState = "/index";
        AuthenticationResponse authnResponse = null;
        try
        {
            LOGGER.info("Going to connect to Okta");
            authnResponse = authenticationClient.authenticate(username, password.toCharArray(),
                    relayState,
                    authenticationStateHandler);
        }
        catch(com.okta.authn.sdk.AuthenticationException e)
        {
            LOGGER.error("Unable to authentcate the user", e);
        }

        if(authnResponse != null)
        {
            final List grantedAuths = new ArrayList<>();
            grantedAuths.add(new SimpleGrantedAuthority("ROLE_ADMIN"));
            final UserDetails principal = new User(username, password, grantedAuths);
            final Authentication authen = new UsernamePasswordAuthenticationToken(principal,
                    password, grantedAuths);
            return authen;
        }
        else
        {
            LOGGER.info("Unable to authenticate");
            return null;
        }

    }

    @Override
    public boolean supports (Class<?> authentication)
    {
        return true;
    }
}
```

我们使用`authenticationClient`来调用认证方法。`AuthenticationStateHandler`基本处理身份认证。该句柄的实现如下所示:

```
package com.betterjavacode.sss.todolist.security;

import com.okta.authn.sdk.AuthenticationStateHandlerAdapter;
import com.okta.authn.sdk.resource.AuthenticationResponse;
import com.okta.commons.lang.Strings;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;

@Component
public class AuthenticationStateHandler extends AuthenticationStateHandlerAdapter
{
    private static final Logger LOGGER = LoggerFactory.getLogger(AuthenticationStateHandler.class);

    @Override
    public void handleUnknown (AuthenticationResponse unknownResponse)
    {
        // TO DO
    }

    @Override
    public void handleSuccess (AuthenticationResponse successResponse)
    {
        if (Strings.hasLength(successResponse.getSessionToken()))
        {
            LOGGER.info("Login successful");
            String relayState = successResponse.getRelayState();
            String dest = relayState != null ? relayState : "/";

        }
    }
}
```

仅此而已。这包括用户认证。请记住，这仍然是基于表单的身份验证，您在自定义登录页面上输入用户凭证，并在屏幕后面调用 Okta API 进行身份验证。

在我的书[简化 Spring 安全](https://gum.co/VgSdH)中，我还添加了使用 Okta OAuth 登录的演示。

# 结论

在这篇文章中，我展示了如何在 Spring Boot 应用程序中使用 Okta SDK 进行用户管理和认证。如果你有任何问题，欢迎通过订阅我的博客[给我发邮件。](https://betterjavacode.com/subscribe)

*原载于 2021 年 3 月 7 日 https://betterjavacode.com*[](https://betterjavacode.com/spring-boot/user-management-with-okta-sdk-and-spring-boot)**。**