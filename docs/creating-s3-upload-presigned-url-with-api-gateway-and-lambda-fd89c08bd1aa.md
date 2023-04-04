# 使用 API 网关和 Lambda 创建 S3 上传预先设计的 URL

> 原文：<https://blog.devgenius.io/creating-s3-upload-presigned-url-with-api-gateway-and-lambda-fd89c08bd1aa?source=collection_archive---------3----------------------->

![](img/94350956fa6208a9751e721ae27fbc31.png)

让我们使用 API 网关前端的 Lambda 创建一个 S3 上传预先设计的 URL 生成器。

![](img/aa44f64fcabcbc072188922f9276cbc0.png)

对于要上传文件到特定 S3 桶的用户，他/她首先向 API 网关发出请求，API 网关将请求发送给 lambda 函数，lambda 函数又与 S3 桶对话以获得预先签名的上传 URL，然后该 URL 作为 API 网关的响应返回给用户。然后，用户将他/她的文件上传到带有 URL 的桶中。

Terraform 用于实现上述架构。

## λ函数

lambda 函数是使用 Golang 实现的。

```
package mainimport (
 "context"
 "log"
 "os" "github.com/aws/aws-lambda-go/events"
 "github.com/aws/aws-lambda-go/lambda"
 "github.com/aws/aws-sdk-go-v2/aws"
 "github.com/aws/aws-sdk-go-v2/config"
 "github.com/aws/aws-sdk-go-v2/service/s3"
)
**func handler(ctx context.Context, request events.APIGatewayProxyRequest) (events.APIGatewayProxyResponse, error)** {
 cfg, err := config.LoadDefaultConfig(context.TODO())
 if err != nil {
  log.Fatalf("Failed to load default config: %v", err)
 } client := s3.NewFromConfig(cfg)
 psClient := s3.NewPresignClient(client) bucket := os.Getenv("BUCKET")
 filename := request.QueryStringParameters["filename"] #log.Printf("bucket: %s, filename: %s", bucket, filename) resp, err := psClient.PresignPutObject(context.TODO(), &s3.PutObjectInput{
  Bucket: aws.String(bucket),
  Key:    aws.String(filename),
 }) if err != nil {
  log.Fatalf("Failed to request for presign url: %v", err)
 } return events.APIGatewayProxyResponse{Body: resp.URL, StatusCode: 200}, nil
}func main() {
 lambda.Start(handler)
}
```

注意在 lambda 处理程序中，函数参数是`events.APIGatewayProxyRequest`。返回类型为`events.APIGatewayProxyResponse`。我们可以从查询字符串中获取文件名，并将其用作 S3 的键。从环境变量中获取 bucket。我们调用 S3 API 来创建一个 PresignPutObject 请求。预先指定的 URL 可以从 S3 响应中获得。

现在让我们用地形创建 lambda。

```
resource "aws_lambda_function" "s3-presigned-url" {
  function_name = "s3-presigned-url"
  filename      = "./lambda-code/src/s3-presigned-url.zip"
  handler       = "s3-presigned-url.bin"
  source_code_hash = filebase64sha256("./lambda-code/src/s3-presigned-url.zip") role          = aws_iam_role.lambda-s3-role.arn
  runtime       = "go1.x" environment {
    variables = {
      BUCKET = aws_s3_bucket.my-bucket.bucket
    }
  }
}
```

可执行文件被命名为`s3-presigned-url.bin.`。zip 文件仅由可执行文件创建。

在环境部分，我们定义了一个名为 BUCKET 的变量，其值引用了 S3 terraform 资源。(*此处忽略 s3 资源的 hcl 来源*)。

要运行 lambda，我们需要分配适当的 IAM 角色。这是通过以下方式实现的，

```
variable "lambda_assume_role_policy_document" {
  type        = string
  description = "assume role policy document"
  default     = <<-EOF
   {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": "sts:AssumeRole",
          "Principal": {
            "Service": "lambda.amazonaws.com"
          },
          "Effect": "Allow"
        }
      ]
   }
  EOF
}resource "aws_iam_policy" "my-lambda-iam-policy" {
  name        = "my-lambda-iam-policy"
  path        = "/"
  description = "My lambda policy - base" policy = <<-EOF
    {
      "Version": "2012-10-17",
      "Statement": [
        {
          "Action": [
            "logs:CreateLogGroup",
            "logs:CreateLogStream",
            "logs:PutLogEvents"
          ],
          "Effect": "Allow",
          "Resource": "*"
        }
      ]
    }
  EOF
}resource "aws_iam_role" "lambda-s3-role" {
  name = "my_lambda_iam_s3_role"
  assume_role_policy = var.lambda_assume_role_policy_document
}resource "aws_iam_role_policy_attachment" "base-role" {
  role       = aws_iam_role.lambda-s3-role.name
  policy_arn = aws_iam_policy.my-lambda-iam-policy.arn
}data "aws_iam_policy" "s3-admin-policy"{
  name = "AmazonS3FullAccess" 
}resource "aws_iam_role_policy_attachment" "s3-role" {
  role       = aws_iam_role.lambda-s3-role.name
  policy_arn = data.aws_iam_policy.s3-admin-policy.arn
}
```

从定义承担角色策略的变量开始，我们创建 aws_iam_role 资源。lambda 函数需要访问 CloudWatch 日志和 S3 服务。为 CloudWatch 创建新策略。通过将数据资源引用到 AWS 管理的策略来重用 S3 权限。然后调用“aws_iam_role_policy_attachment”两次，将策略附加到角色。(*当然，对于生产用途，我们可能希望将 S3 权限收紧到 s3:putObject only*

## API 网关

api 网关由 API、阶段、集成和路由创建。

```
resource "aws_apigatewayv2_api" "s3upload" {
  name          = "s3upload"
  protocol_type = "HTTP"
}resource "aws_apigatewayv2_stage" "v1" {
  api_id      = aws_apigatewayv2_api.s3upload.id
  name        = "v1"
  auto_deploy = true
}resource "aws_apigatewayv2_integration" "s3upload" {
  api_id           = aws_apigatewayv2_api.s3upload.id
  integration_type = "AWS_PROXY" connection_type           = "INTERNET"
  description               = "s3upload presign url"
  integration_method        = "POST"
  integration_uri = aws_lambda_function.s3-presigned-url.invoke_arn
}resource "aws_apigatewayv2_route" "s3upload" {
  api_id    = aws_apigatewayv2_api.s3upload.id
  operation_name = "s3upload"
  route_key      = "GET /url" target="integrations/${aws_apigatewayv2_integration.s3upload.id}"
}
```

从定义 HTTP API 网关开始，stage 资源将生成一个调用 URL 库来触发 API。我们在这里使用“v1”而不是“$default”。集成指定了 API 网关如何将请求转发给 Lambda 函数。注意 integration_type 和 integration_method 的组合分别是 AWS_PROXY 和 POST。似乎 POST 是 API Gateway 集成到 lambda 的唯一方法。路由资源通过目标属性连接 API 和集成。URL 路径和 HTTP 方法在 route_key 属性中定义。有了这些设置，我们将期望完整的调用 URL 和路径如下所示，

`https://{random string}.execute-api.us-east-1.amazonaws.com/v1/url`

这还不够，我们需要允许 API 网关能够调用 lambda 函数。如果我们使用 AWS web 控制台，则有一个默认设置来启用权限。在 Terraform 的情况下，我们使用 lambda 资源策略启用权限，

```
resource "aws_lambda_permission" "apigw-permission" {
  statement_id  = "AllowAPIInvoke"
  action        = "lambda:InvokeFunction"
  function_name = "s3-presigned-url"
  principal     = "apigateway.amazonaws.com" # The /*/*/* part allows invocation from any stage, method and resource path
  source_arn = "${aws_apigatewayv2_api.s3upload.execution_arn}/*/*/*"
}
```

请注意，我们可以进一步收紧 resource_arn 属性中阶段、方法和路径组合的权限。

应用所有的地形代码。我们可以获得 API 网关调用 URL。

![](img/dc5a1b8235dd3798845f4f0f11951e53.png)

## 测试

假设我们想上传一个文件 abc.txt 到 S3 桶。

运行以下 curl 命令从 API 网关获取预签名的 url。

```
curl '[https://o2cfq73ski.execute-api.us-east-1.amazonaws.com/v1/url?filename=abc.txt'](https://o2cfq73ski.execute-api.us-east-1.amazonaws.com/v1/url?filename=abc.txt')
```

响应是 S3 上传预先签名的 URL

```
[https://my-bucket-1f8efb28-8ca0-af97-10ff-dc3c5c2c8255.s3.us-east-1.amazonaws.com/abc.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIA5GCH7TYAE664OJNK%2F20220102%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220102T075911Z&X-Amz-Expires=900&X-Amz-Security-Token=](https://my-bucket-1f8efb28-8ca0-af97-10ff-dc3c5c2c8255.s3.us-east-1.amazonaws.com/abc.txt?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=ASIA5GCH7TYAE664OJNK%2F20220102%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20220102T075911Z&X-Amz-Expires=900&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDgaCXVzLWVhc3QtMSJHMEUCICLRjrbmbvlLc2U0qGzypPKl5n7iqPF77gCecZc2xOWTAiEA33zFTKmIq%2Bm2IsAbA%2Bnl6cZSNikx%2BtZC3x8iudnvEHgqlQIIQRABGgw5MDYzODkwNjkzMTIiDCe3KrBmM2fAIPhDNCryAQ97ZTM5NBYvRmJRXsfKp%2BxaSzp4vfOJlkA8RGc98gd6FXXAHC5EIEWPGAs3SxilLgG%2BDi5FkofFsciCEoVxrfP2GISj1npUxhnyllmHGJ%2FlkJ7pnLhz3GBj4FDKD6GQTshqeCEuxJFNgW90SmKd6rk%2BFxaunkJ1EyU0QR9gBGXSG1J6DHnRGs18xHLNeoy1bxjmOLOvixAHDcqd%2BYwUg9oYBIfL9KIZRjulhbxcm0GGr5qXl%2BLLHVRwkBVZ0B%2FLeSKc%2FTZeXldSPcyPx5eFRzlysSpOfieXQZLKi4yuFTpwP1x9J6mtqkoqpRcMuWU71w0bMM%2B2xY4GOpoBuH%2FfgeGVkTyHn5p9TjFX6qJ6DHXotcDN8zKMGu3x1f%2FPcTA40gufOUMwApTYY%2B1YbwdJvnOmwZv7UjugzUQ1g49VAHmRmR%2BFMISgUNnCLvxoc3yKnvu0rJYFbsI5d50og%2B7dqSZC0cpmYFaI7vugsfjRhOFMVOiwNZEXi%2BkeAEzfh0jA5Ps4tqdjZM3xqfoK1CoLyT9eXdyRNg%3D%3D&X-Amz-SignedHeaders=host&x-id=PutObject&X-Amz-Signature=b5c45ca93915b3268efe88be73403bc53bfe71e11f8e58f12e0a691adc4bd75)...
```

用 VS 代码启动 Thunder 客户端，用 URL 创建一个新的 PUT 请求，选择二进制文件作为主体。

![](img/cce97d0d6af59c4bcdafc501881a3bde.png)

请求成功了。我们现在看到文件 abc.txt 被上传到 S3 存储桶中。

![](img/8902235c64eec18c40df6ef45b796092.png)