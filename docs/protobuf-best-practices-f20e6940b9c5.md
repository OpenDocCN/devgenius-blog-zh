# Protobuf 最佳实践

> 原文：<https://blog.devgenius.io/protobuf-best-practices-f20e6940b9c5?source=collection_archive---------1----------------------->

proto3 的约定

如果你使用 protobuf 作为序列化器。Protobuf 更喜欢结构化数据，具有更严格的向后兼容性，并且在网络传输方面非常高效。Protobuf 和 gRPC 是一个强大的组合，下面是一组推荐的最佳实践

Protobuf 有 3 种主要类型的组件:服务、消息和枚举。原型文件可以有任意数量的这样的组件

# 服务

顾名思义，它用于定义资源封装 API。
首选协议:grpc

```
service UserService {
    rpc getUser(GetUserRequest) returns (GetUserResponse);
    rpc creatUser(CreatUserRequest) returns (CreatUserResponse) {}
}
```

*   为每个域实体创建一个服务。
*   结尾为；或{}，两者都有效。传统上优选；
*   DTO 消息名称应为<api>请求和<api>响应，分别为请求、响应消息。</api></api>

注意:[风格指南](https://docs.buf.build/style-guide/)命名约定

# 消息

这是一个传统的物体造型空间。消息包含 3 个重要方面，属性名、数据类型和唯一字段号。

```
message SearchRequest {
    *repeated* string params = 1;
    int32 page_number = 2;
    int32 result_per_page = 3;
    reserved 14, 15;
}
```

*   我们可以嵌套“消息”

## 现场会议

*   属性名首选 snake_case

如果发布的源协议带有额外的字段，并且客户端使用的是旧版本的契约，比版本 3.5 更新的序列化程序会忽略这些字段；类似于`*JsonIgnoreProperties(ignoreUnknown = true)`*

## 选择什么数据类型？

数据类型可以是原始数据类型或自定义数据类型(其他消息/枚举)。

[不同语言的原型数据类型映射](https://developers.google.com/protocol-buffers/docs/proto3#scalar)

*   检查你的选择，明智地选择。例如 int32 对 int64
*   `重复'关键字用于定义有序集合
*   原始数据类型有默认值，即字符串-"，布尔值-false，*数字* -0
*   避免将没有模式的开放字段建模为字符串/映射，例如 json、xml
*   ` Any '是一种特殊的数据类型。这就像嵌入了任何未知的“信息”。
    > >这是一个占位符，实际上可以包含任何“消息”
    > >只有当域不理解内容时才使用“任何”,便于存储。例如，元字段
    >>“[Any](https://github.com/protocolbuffers/protobuf/blob/master/src/google/protobuf/any.proto)”字段具有动态嵌入的消息原型的值和 schemaTypeUrl。
    > >因此，在将它反序列化为本机实体时，建议在这种情况下同时拥有值和模式属性

# 如何给一个属性编号？

*   字段编号是一个重要的补充。为了向后兼容，建议**不要更改客户正在使用的**号。
*   另一个需要注意的重要事情是，字段编号 1 到 15 编码为 1 个字节，然后 16 到 2047 编码为 2 个字节(更多关于 [protobuf 编码](https://developers.google.com/protocol-buffers/docs/encoding#structure))。因此，为了优化性能:
*   对于**所有强制的、更频繁的字段，使用字段编号 1 到 15** ，以优化消息有效载荷的大小
*   为将来的扩展保留几个空槽。要阻止这样的缓冲区，使用保留的关键字
    例如，在上面的请求中，数字 14，15
*   对最不常用的属性使用 16 位以上的数字

# 列举型别

顾名思义，这只是另一个用于配置枚举值的传统白名单块。“枚举”也可以是任何“消息”的一部分

```
enum OrderStatus {
    ORDER_STATUS_UNSPECIFIED = 0;
    ORDER_STATUS_CREATED = 1;
    ORDER_STATUS_PAID = 2;
    ORDER_STATUS_DISPATCHED = 3;
}
```

*   值的首选大写字母
*   按照惯例，以未指定的值 0 开始，以允许
*   编译器生成一个带有一个额外值的枚举，该值对于 Java 这样的严格枚举语言来说是不可识别的。
*   因此，在处理请求之前，请确保验证该值不是不可识别的。

# 不常用的功能

# 之一

有多少次您不想添加自定义验证，而客户端应该发送 externalId 或 clientReferenceId。现在你可以用一个。

```
oneof id {
    external_id = 2;
    client_reference_id = 6;
}
```

现在，即使客户端同时提供了这两个属性，也只有 oneof 的最后一个属性被反序列化

# 地图

这只是传统的地图，有一个警告，它不能“重复”

# 参考

[](https://developers.google.com/protocol-buffers/docs/proto3) [## 语言指南(proto3) |协议缓冲区| Google 开发者

### 本指南描述了如何使用协议缓冲区语言来构建您的协议缓冲区数据，包括。原型…

developers.google.com](https://developers.google.com/protocol-buffers/docs/proto3) [](https://docs.buf.build/style-guide/) [## 风格指南| Buf

### 这是我们的 Protobuf 风格指南。本文档有意做到简明扼要，旨在作为…的简短参考

doc . buf . build](https://docs.buf.build/style-guide/)