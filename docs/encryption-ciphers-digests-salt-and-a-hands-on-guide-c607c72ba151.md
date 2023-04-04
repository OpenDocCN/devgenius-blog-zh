# 加密:密码、摘要、salt、IV 和实践指南

> 原文：<https://blog.devgenius.io/encryption-ciphers-digests-salt-and-a-hands-on-guide-c607c72ba151?source=collection_archive---------10----------------------->

![](img/1f4e6037e807f6f2742cad1b009694a7.png)

图片版权 Sergio Mijatovic 2022

# 什么是加密

加密是一种将数据转换成无用形式的方法，只有通过解密才能变得有用。目的是使数据只对能够解密的人可用(即使其可用)。通常，需要对数据进行加密，以确保在未经授权的情况下无法获取数据。这是攻击者成功突破授权系统和访问控制后的最后一道防线。

这并不意味着所有数据都需要加密，因为通常授权和访问系统可能就足够了，此外，加密和解密数据会有性能损失。数据是否以及何时加密是应用程序规划和风险评估的问题，有时也是法规要求，例如 HIPAA 或 GDPR。

数据可以在静态时加密，如在磁盘上，也可以在传输过程中加密，如在互联网上通信的双方之间。

在这里，您将学习如何使用密码加密和解密数据，也称为对称加密。交换信息的双方都必须知道该密码。

# 密码，文摘，盐，四

为了正确和安全地使用加密，需要解释一些概念。

密码是用于加密的算法。例如，AES256 是一种密码。谈到加密，大多数人都会想到密码。

摘要基本上是一种哈希函数，用于在密码被密码使用之前对密码(即加密密钥)进行加密和加长。为什么这样做？首先，它创建了一个随机的、长度一致的密钥散列，更适合加密。也很适合“腌制”，这是接下来要说的。

“盐”是一种击败所谓的“彩虹”表的方法。攻击者知道，如果原始值是相同的，两个散列值看起来仍然完全相同。但是，如果您将 salt 值添加到哈希中，它们就不会。它被称为“盐”,因为它和钥匙混合在一起会产生不同的东西。现在，彩虹表会尝试将已知的散列值与预先计算的数据进行匹配，以猜测密码。通常，salt 是为每个键随机生成的，并与它一起存储。为了匹配已知的散列，攻击者必须预先计算大量随机值的彩虹表，这通常是不可行的。

您将经常听说加密中的“迭代”。迭代是一个循环，在这个循环中，一个键和 salt 混合在一起，使得猜测键变得更加困难。这要做很多次，以使攻击者很难计算出反向猜测的密钥，因此“迭代”(复数)。通常，所需的最小迭代次数是 1000，但也可以不同于此。如果您从一个非常强的密码开始，一般来说您需要更少。

IV(或“初始化向量”)通常是一个随机值，用于加密每条消息。现在，盐被用来产生基于密码的密钥。当你已经有一个密钥并且正在加密消息时，就使用 IV。IV 的目的是使相同的消息在加密时以不同的方式出现。有时候，IV 也有一个序列成分，所以它是由一个随机的字符串加上一个不断增加的序列组成的。这使得“重放”攻击变得困难，这是攻击者不需要解密消息的地方；而是加密的消息被“嗅探”(即在发送者和接收者之间被截获)，然后重放，希望重复已经执行的动作。虽然在现实中，大多数高级协议已经有了一个序列，其中每条消息都有一个递增的数据包编号，所以在大多数情况下 IV 不需要它。

# 先决条件

本例使用 C 语言编程，框架为[vs .](https://vely.dev/)。

请注意，使用自定义密码和摘要，以及初始化向量和密钥缓存的显式使用从 15.2 开始可用——如果您使用的是早期版本，您应该[安装](https://vely.dev/pkg/) 15.2 或更高版本来运行这些示例。

# 加密示例

要运行这里的例子，在它自己的目录中创建一个应用程序“enc”(参见 [vf](https://vely.dev/vf.html) 了解更多关于 Vely 的程序管理器):

```
mkdir enc_example
cd enc_example
sudo vf -i -u $(whoami) enc
```

使用 [encrypt-data](https://vely.dev/encrypt-data.html) 语句加密数据。最简单的形式是加密以 null 结尾的字符串。创建一个文件“加密”,然后复制它:

```
#include "vely.h"

void encrypt() {
    out-header default
    char *str = "This contains a secret code, which is Open Sesame!";
    // Encrypt
    encrypt-data str to define enc_str password "my_password"
    p-out enc_str
    @
    // Decrypt
    decrypt-data enc_str password "my_password" to define dec_str
    p-out dec_str
    @
}
```

你可以看到加密数据和解密数据的基本用法。你提供数据(原始的或加密的)，密码，然后就可以走了。数据被加密，然后解密，产生原始数据。

在源代码中，一个字符串变量“enc_str”(创建为“char *”)会包含加密版的“这里面包含了一个秘密代码，是芝麻开门！”而“dec_str”将是必须完全相同的解密数据。

要从命令行运行这段代码，首先使应用程序:

```
vv -q
```

然后让 Vely 生成 bash 代码来运行它—请求路径是“/encrypt”，在我们的例子中，它是由源文件“encrypt.vely”中定义的函数“void encrypt()”处理的。事实上，这些名称总是匹配的，这使得编写、读取和执行代码变得很容易。使用 [vv](https://vely.dev/vv.html) 中的“-r”选项指定请求路径，并获取运行程序所需的代码:

```
vv -r --req="/encrypt"
```

这将产生类似于:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
```

将它复制并粘贴到 bash shell 中并执行。您将得到响应:

```
Content-type: text/html;charset=utf-8
Cache-Control: max-age=0, no-cache
Pragma: no-cache
Status: 200 OK

72ddd44c10e9693be6ac77caabc64e05f809290a109df7cfc57400948cb888cd23c7e98e15bcf21b25ab1337ddc6d02094232111aa20a2d548c08f230b6d56e9
This contains a secret code, which is Open Sesame!
```

你这里有的是加密的数据，然后这个加密的数据用同一个密码解密。不出所料，结果与我们最初加密的字符串相匹配。

注意，默认情况下，encrypt-data 会以人类可读的十六进制形式产生加密值，这意味着由十六进制字符“0”到“9”和“a”到“f”组成。这样，您可以将加密的数据存储到一个常规字符串中。例如，它可能会转到 JSON 文档或数据库中的 VARCHAR 列，或者其他任何地方。然而你也可以产生二进制加密数据。稍后会有更多的介绍。

上面的结果是一个带有适当报头的有效 HTTP 响应。要跳过标题，请将变量 VV_SILENT_HEADER 设置为“yes”:

```
export VV_SILENT_HEADER=yes
/var/lib/vv/bld/enc/enc
```

如果再次执行程序“/var/lib/vv/bld/enc/enc ”,结果就是这样，没有任何头文件:

```
72ddd44c10e9693be6ac77caabc64e05f809290a109df7cfc57400948cb888cd23c7e98e15bcf21b25ab1337ddc6d02094232111aa20a2d548c08f230b6d56e9
This contains a secret code, which is Open Sesame!
```

# 将数据加密为二进制结果

在前面的示例中，生成的加密数据是人类可读的十六进制形式。您还可以创建二进制加密数据，它不是人类可读的字符串，也更短。为此，请使用“binary”子句。将“加密”中的代码替换为:

```
#include "vely.h"

void encrypt() {
    out-header default
    char *str = "This contains a secret code, which is Open Sesame!";
    // Encrypt
    encrypt-data str to define enc_str password "my_password" \
        binary output-length define outlen
    // Save the encrypted data to a file
    write-file "encrypted_data" from enc_str length outlen
    get-app directory to define app_dir
    @Encrypted data written to file <<p-out app_dir>>/encrypted_data
    // Decrypt data
    decrypt-data enc_str password "my_password" \
        input-length outlen binary to define dec_str
    p-out dec_str
    @
}
```

当您想要获得二进制加密数据时，您也应该获得它的字节长度，否则您可能不知道它在哪里结束，因为它可能包含空字符。为此使用“输出长度”子句。在这段代码中，变量“enc_str”中的加密数据被写入文件“encrypted_data”，写入的长度为“outlen”字节。当一个文件在没有路径的情况下被写入时，它总是被写入应用程序的主目录中(参见 [how_vely_works](https://vely.dev/how_vely_works.html) )，所以你可以使用 [get-app](https://vely.dev/get-app.html) 来获得那个目录。

解密数据时，注意“input-length”子句的使用。它表示加密数据有多少字节。显然，您可以从“outlen”变量中获得该值，encrypt-data 在该变量中存储加密数据的长度。当加密和解密是分离的，也就是说，在不同的程序中运行，你要确保这个长度是可用的。

还要注意，当数据被加密为“二进制”(意味着产生二进制输出)时，解密必须使用相同的。

制作申请:

```
vv -q
```

像以前一样运行它:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
```

结果是:

```
Encrypted data written to file /var/lib/vv/enc/app/encrypted_data
This contains a secret code, which is Open Sesame!
```

解密后的数据与原始数据完全相同。

您可以通过使用“八进制转储”(“od”)Linux 实用程序来查看写入文件的实际加密数据:

```
$ od -c /var/lib/vv/enc/app/encrypted_data
0000000   r 335 324   L 020 351   i   ; 346 254   w 312 253 306   N 005
0000020 370  \t   )  \n 020 235 367 317 305   t  \0 224 214 270 210 315
0000040   # 307 351 216 025 274 362 033   % 253 023   7 335 306 320
0000060 224   #   ! 021 252     242 325   H 300 217   #  \v   m   V 351
0000100
```

这就是了。您会注意到数据是二进制的，它实际上包含空字符。

# 加密二进制数据

在这些示例中，要加密的数据是一个字符串，即以空值分隔的字符串。您可以通过在“input-length”子句中指定二进制数据的长度来轻松加密二进制数据，例如，将它复制到“encrypt.vely”:

```
#include "vely.h"

void encrypt() {
    out-header default
    char *str = "This c\000ontains a secret code, which is Open Sesame!";
    // Encrypt
    encrypt-data str to define enc_str password "my_password" \
        input-length 12
    p-out enc_str
    @
    // Decrypt
    decrypt-data enc_str password "my_password" to define dec_str \
        output-length define res_len
    // Output binary data; present null char as octal \000
    int i;
    for (i = 0; i < res_len; i++) {
        if (dec_str[i] == 0) {
            p-out "\\000"
        } else {
            pf-out "%c", dec_str[i]
        }
    }
    @
}
```

这将在内存位置“enc_str”加密 12 个字节，而不考虑任何空字符。在这种情况下，这是“This c ”,后跟一个空字符，后跟“ontain”字符串，但它可以是任何类型的二进制数据，例如 JPG 文件的内容。

在解密端，你会在“output-length”子句中获得解密的字节数。最后，解密的数据被显示为完全是原始的，并且空字符以典型的八进制表示出现。

制作程序:

```
vv -q
```

像以前一样运行它:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
```

结果是:

```
6bea45c2f901c0913c87fccb9b347d0a
This c\000ontai
```

加密的值更短，因为在这种情况下数据也更短，结果与原始值完全匹配。

# 使用任何密码或摘要

默认使用的加密是来自标准 [OpenSSL](https://www.openssl.org/) 库的 [AES256](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) 和 [SHA256](https://en.wikipedia.org/wiki/SHA-2) 哈希，两者都广泛应用于密码学。但是，您可以使用 OpenSSL 支持的任何可用的密码和摘要(即哈希)(甚至是您提供的自定义密码和摘要)。

要查看哪些算法可用，在命令行中执行:

```
#get list of cipher providers
openssl list -cipher-algorithms

#get list of digest providers
openssl list -digest-algorithms
```

这两个将提供一个密码和摘要(哈希)算法的列表。其中一些可能比 vly 选择的默认选项要弱，而另一些可能只是为了向后兼容旧系统。然而其他的可能是相当新的，没有足够的时间被验证到你想要的程度。所以在选择这些算法时要小心，并确保知道为什么要改变默认算法。也就是说，这里有一个使用 Camellia-256(即“CAMELLIA-256-CFB1”)加密和“SHA3–512”摘要的示例。将“加密”中的代码替换为:

```
#include "vely.h"

void encrypt() {
    out-header default
    char *str = "This contains a secret code, which is Open Sesame!";
    // Encrypt data
    encrypt-data str to define enc_str password "my_password" \
        cipher "CAMELLIA-256-CFB1" digest "SHA3-512"
    p-out enc_str
    @
    // Decrypt data
    decrypt-data enc_str password "my_password"  to define dec_str \
        cipher "CAMELLIA-256-CFB1" digest "SHA3-512"
    p-out dec_str
    @
}
```

提出申请:

```
vv -q
```

运行它:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
```

在这种情况下，结果是:

```
f4d64d920756f7220516567727cef2c47443973de03449915d50a1d2e5e8558e7e06914532a0b0bf13842f67f0a268c98da6
This contains a secret code, which is Open Sesame!
```

同样，你得到了原始数据。注意你必须在加密数据和解密数据中使用相同的密码和摘要！

你当然可以像以前一样，通过使用“binary”和“output-length”子句来生成二进制加密值。如果您有加密数据的外部系统，并且您知道它们使用哪种密码和摘要，您可以匹配它们并使您的代码具有互操作性。Vely 使用标准的 OpenSSL 库，所以其他软件也可能使用。

# 使用盐

要将 salt 添加到加密中，请使用“salt”子句。您可以使用 [random-string](https://vely.dev/random-string.html) 语句生成随机 salt。以下是“加密”的代码:

```
#include "vely.h"

void encrypt() {
    out-header default
    char *str = "This contains a secret code, which is Open Sesame!";
    // Get salt
    random-string to define rs length 16
    // Encrypt data
    encrypt-data str to define enc_str password "my_password" salt rs
    @Salt used is <<p-out rs>>, and the encrypted string is <<p-out enc_str>>
    // Decrypt data
    decrypt-data enc_str password "my_password" salt rs to define dec_str
    p-out dec_str
    @
}
```

提出申请:

```
vv -q
```

运行几次:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
/var/lib/vv/bld/enc/enc
/var/lib/vv/bld/enc/enc
```

结果是:

```
Salt used is VA9agPKxL9hf3bMd, and the encrypted string is 3272aa49c9b10cb2edf5d8a5e23803a5aa153c1b124296d318e3b3ad22bc911d1c0889d195d800c2bd92153ef7688e8d1cd368dbca3c5250d456f05c81ce0fdd
This contains a secret code, which is Open Sesame!
Salt used is FeWcGkBO5hQ1uo1A, and the encrypted string is 48b97314c1bc88952c798dfde7a416180dda6b00361217ea25278791c43b34f9c2e31cab6d9f4f28eea59baa70aadb4e8f1ed0709db81dff19f24cb7677c7371
This contains a secret code, which is Open Sesame!
Salt used is nCQClR0NMjdetTEf, and the encrypted string is f19cdd9c1ddec487157ac727b2c8d0cdeb728a4ecaf838ca8585e279447bcdce83f7f95fa53b054775be1bb2de3b95f2e66a8b26b216ea18aa8b47f3d177e917
This contains a secret code, which is Open Sesame!
```

如您所见，每次加密都会生成一个随机的 salt 值(本例中为 16 字节长),并且每次加密的值都不同，即使加密的数据是相同的！这样就很难破解加密了。

请注意，如果 salt 是二进制值，通常会更好。在这种情况下，它是一个字符串，只是为了举例。

当然，要解密，必须记录下盐，并且和加密时完全一样使用。在这里的代码中，变量“rs”保存盐。如果您将加密值存储在数据库中，您可能会将盐存储在它旁边。

# 初始化向量

实际上，您不会对每个消息使用不同的 salt 值。它每次都会创建一个新的密钥，这会降低性能。实际上没有必要这样做:盐的使用使得每个键(即使是相同的键)更难猜测。一旦你这样做了，你可能不需要再做一次，或者经常做。

相反，您应该为每条消息使用 IV(初始化向量)。它通常是一个随机字符串，使相同的消息看起来不同，并增加了破解密码的计算成本。以下是“加密”的新代码:

```
#include "vely.h"
void encrypt() {
    out-header default
    // Get salt
    random-string to define rs length 16
    // Encrypt data
    num i;
    for (i = 0; i < 10; i++) {
        random-string to define iv length 12
        encrypt-data "The same message" to define enc_str password "my_password" salt rs iterations 2000 init-vector iv cache
        @The encrypted string is <<p-out enc_str>>
        // Decrypt data
        decrypt-data enc_str password "my_password" salt rs iterations 2000 init-vector iv to define dec_str cache
        p-out dec_str
        @
    }
}
```

提出申请:

```
vv -q
```

运行几次:

```
export REQUEST_METHOD=GET
export SCRIPT_NAME="/enc"
export PATH_INFO="/encrypt"
export QUERY_STRING=""
/var/lib/vv/bld/enc/enc
/var/lib/vv/bld/enc/enc
/var/lib/vv/bld/enc/enc
```

结果可能是:

```
The encrypted string is 787909d332fd84ba939c594e24c421b00ba46d9c9a776c47d3d0a9ca6fccb1a2
The same message
The encrypted string is 7fae887e3ae469b666cff79a68270ea3d11b771dc58a299971d5b49a1f7db1be
The same message
The encrypted string is 59f95c3e4457d89f611c4f8bd53dd5fa9f8c3bbe748ed7d5aeb939ad633199d7
The same message
The encrypted string is 00f218d0bbe7b618a0c2970da0b09e043a47798004502b76bc4a3f6afc626056
The same message
The encrypted string is 6819349496b9f573743f5ef65e27ac26f0d64574d39227cc4e85e517f108a5dd
The same message
The encrypted string is a2833338cf636602881377a024c974906caa16d1f7c47c78d9efdff128918d58
The same message
The encrypted string is 04c914cd9338fcba9acb550a79188bebbbb134c34441dfd540473dd8a1e6be40
The same message
The encrypted string is 05f0d51561d59edf05befd9fad243e0737e4a98af357a9764cba84bcc55cf4d5
The same message
The encrypted string is ae594c4d6e72c05c186383e63c89d93880c8a8a085bf9367bdfd772e3c163458
The same message
The encrypted string is 2b28cdf5a67a5a036139fd410112735aa96bc341a170dafb56818dc78efe2e00
The same message
```

您可以看到，相同的消息在加密时看起来不同，但在解密时又是一样的。当然，加密和解密的密码、salt、迭代次数和 init-vector 必须相同。

请注意在 encrypt-data 和 decrypt-data 中使用了“cache”子句。它有效地缓存了计算出的密钥(给定密码、salt、密码/摘要算法和迭代次数)，所以它不是每次都在循环中计算。使用“cache”时，密钥被计算一次，然后不同的 IV(在“init-vector”子句中)被用于每条消息。

如果您想偶尔重建密钥，请使用“clear-cache”子句，该子句提供一个布尔值。如果为真，则重新计算密钥，否则不进行计算。更多信息见[加密数据](https://vely.dev/encrypt-data.html)。

# 结论

您已经学习了如何使用不同的密码、摘要、salt 和 IV 值来加密和解密数据。您还可以创建人类可读的加密值和二进制输出，以及加密字符串和二进制值(如文档)。