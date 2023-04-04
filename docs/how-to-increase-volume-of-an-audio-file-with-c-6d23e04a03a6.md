# 如何用 c 增加一个音频文件的音量？

> 原文：<https://blog.devgenius.io/how-to-increase-volume-of-an-audio-file-with-c-6d23e04a03a6?source=collection_archive---------5----------------------->

![](img/db141630bd6189e3584bd6d8505095e9.png)

米盖尔·Á。帕德里纳恩恩[佩克斯](https://www.pexels.com/es-es/foto/disco-de-vinilo-negro-167092/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

这是 CS50 的第四个实验，我正在努力解决这些看似简单的问题，它们并没有那么简单，哈哈。我很难理解这一点。

在这里我们需要增加。WAV 文件，由 44 个无符号整数字节的头文件组成，头文件包含文件本身的信息和样本，这些样本是音频文件的“*音频*”部分，我们可以通过将它们乘以一个数字来改变音量。

为了解决这个问题，我们使用了三个头文件:

```
#include <stdint.h>#include <stdio.h>#include <stdlib.h>
```

首先，在 main 之前，我们声明。WAV 文件和一个数据类型 BYTE，其中 uint8 字节用于读取头，BYTE2 字节具有 int16 字节值，用于读取每个样本。

```
const int HEADER_SIZE = 44;typedef uint8_t BYTE;typedef int16_t BYTE2;
```

要运行程序，我们需要用户输入有 4 个命令行参数，如果不符合，我们返回。

```
int main(int argc, char *argv[]){// Check command-line argumentsif (argc != 4){ printf("Usage: ./volume input.wav output.wav factor\n"); return 1;}
```

现在，我们需要打开文件，文件在 C 中有它们自己的数据类型，所以在我们打开它之前，我们用一个文件数据类型声明输入变量，`input`指向内存中存储文件的地址，`fopen`接受用户在命令行上传递的文件名，`“r”`表示“只读取这个文件”。

```
// Open files and determine scaling factorFILE *input = fopen(argv[1], "r");if (input == NULL){printf("Could not open file.\n");return 1;}
```

然后，我们需要用用户输入的第二个文件名创建输出文件，`fopen`也可以通过向它传入`“w”`参数来创建文件，这是为了读取和写入，`output`指向存储变量第一个字符的地址。

创建文件后，我们需要修改音频文件音量的因子，这是用户需要输入的第三个参数，命令行参数是字符串，所以我们需要用`atof`将其转换为 int，并存储在`factor`中

```
FILE *output = fopen(argv[2], "w");if (output == NULL){ printf("Could not open file.\n"); return 1;}float factor = atof(argv[3]);
```

现在我们需要读取我们打开的文件，首先我们创建一个变量，其数据类型为我们之前创建的 BYTE，即 44 字节长，称为`header`，该变量在内存中腾出空间来存储原始文件的头信息并复制到输出文件中。

然后我们读取带有`fread`的文件头，它有 4 个参数:1-读取数据的第一个内存字节的地址，2-将占用内存的字节大小(这将是我们的`HEADER_SIZE` 常量)3-一次读取多少个该类型的文件，4-由`fopen`返回的文件的指针。然后用带有相同参数的`fwrite`,我们将头文件复制到输出文件。

```
BYTE header[HEADER_SIZE];fread(header, HEADER_SIZE, 1, input);fwrite(header, HEADER_SIZE, 1, output);
```

现在，我们需要将样本乘以`factor`来修改我们文件的容量，我们创建一个变量`sample`，其数据类型为我们之前创建的字节 2。

`fread`函数返回到目前为止它能够成功读取的字节数，我们将它用作 while 循环参数。因此，在 while 循环中，我们读取每个`int_16`直到没有为止，在每个循环中，我们将当前的`sample`乘以`factor`来修改音量，并将该值写入输出文件，在循环完成后，我们关闭文件。

```
BYTE2 sample;while(fread(&sample, sizeof(BYTE2), 1, input)){ sample *= factor; fwrite(&sample, sizeof(BYTE2), 1, output);}// Close filesfclose(input);fclose(output);}
```

# 完整的代码要点。

感谢阅读，请鼓掌别忘了关注我:)。