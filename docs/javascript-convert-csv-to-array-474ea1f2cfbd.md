# 如何在 JavaScript 中将 CSV 转换成数组

> 原文：<https://blog.devgenius.io/javascript-convert-csv-to-array-474ea1f2cfbd?source=collection_archive---------7----------------------->

![](img/08bf96c3dee0ddb013fa6f7122a4f265.png)

您可以通过`csvtojson`库将 CSV 文件或字符串快速转换为 JavaScript 中的对象数组:

`index.js`

```
import csvToJson from 'csvtojson';

const csvFilePath = 'data.csv';

const array = await csvToJson().fromFile(csvFilePath);
console.log(array);
```

对于这样一个`data.csv`文件:

`data.csv`

```
color,maxSpeed,age
"red",120,2
"blue",100,3
"green",130,2
```

这将是生成的数组:

`Output`

```
[
  { color: 'red', maxSpeed: '120', age: '2' },
  { color: 'blue', maxSpeed: '100', age: '3' },
  { color: 'green', maxSpeed: '130', age: '2' }
]
```

# 安装`csvtojson`

在使用`csvtojson`之前，您需要将它安装到您的项目中。您可以使用 NPM 或 Yarn CLI 来完成此操作。

```
npm i csvtojson

# Yarn
yarn add csvtojson
```

安装后，您可以将其导入 JavaScript 模块，如下所示:

```
import csvToJson from 'csvtojson';

// CommonJS
const csvToJson = require('csvtojson');
```

# 用`fromFile()`将 CSV 文件转换成数组

我们调用`csvtojson`模块的默认导出函数来创建将 CSV 转换为数组的对象。这个对象有很多方法，每个方法都以某种方式与 CSV 到 JavaScript 对象的转换相关，而`fromFile()`就是其中之一。

它接受要转换的 CSV 文件的名称，并返回一个`Promise`，因为转换是一个异步过程。`Promise`将用产生的 JSON 字符串进行解析。

# 用`fromString()`将 CSV 字符串转换成数组

要直接从 CSV 数据字符串转换，而不是从文件转换，可以使用 converter 对象的异步`fromString()`方法:

`index.js`

```
import csvToJson from 'csvtojson';

const csv = `"First Name","Last Name","Age"
"Russell","Castillo",23
"Christy","Harper",35
"Eleanor","Mark",26`;
const array = await csvToJson().fromString(csv);
console.log(array);
```

`Output`

```
[
  { 'First Name': 'Russell', 'Last Name': 'Castillo', Age: '23' },
  { 'First Name': 'Christy', 'Last Name': 'Harper', Age: '35' },
  { 'First Name': 'Eleanor', 'Last Name': 'Mark', Age: '26' }
]
```

# 自定义 CSV 到数组的转换

`csvtojson`的默认导出功能接受一个对象，该对象用于指定自定义转换过程的选项。

一个这样的选项是`header`，一个用于指定 CSV 数据中的头的数组。

`index.js`

```
import csvToJson from 'csvtojson';

const csv = `"First Name","Last Name","Age"
"Russell","Castillo",23
"Christy","Harper",35
"Eleanor","Mark",26`;
const array = await csvToJson({
  headers: ['firstName', 'lastName', 'age'],
}).fromString(csv);
console.log(array);
```

`Output`

```
[
  { firstName: 'Russell', lastName: 'Castillo', age: '23' },
  { firstName: 'Christy', lastName: 'Harper', age: '35' },
  { firstName: 'Eleanor', lastName: 'Mark', age: '26' }
]
```

另一个是`delimeter`，用于表示分隔各列的字符:

```
import csvToJson from 'csvtojson';

const csv = `"First Name"|"Last Name"|"Age"
"Russell"|"Castillo"|23
"Christy"|"Harper"|35
"Eleanor"|"Mark"|26`;
const array = await csvToJson({
  headers: ['firstName', 'lastName', 'age'],
  delimiter: '|',
}).fromString(csv);
```

`Output`

```
[
  { firstName: 'Russell', lastName: 'Castillo', age: '23' },
  { firstName: 'Christy', lastName: 'Harper', age: '35' },
  { firstName: 'Eleanor', lastName: 'Mark', age: '26' }
]
```

我们还有`ignoreColumns`，一个使用正则表达式指示转换器忽略某些列的选项。

```
import csvToJson from 'csvtojson';

const csv = `"First Name"|"Last Name"|"Age"
"Russell"|"Castillo"|23
"Christy"|"Harper"|35
"Eleanor"|"Mark"|26`;
const array = await csvToJson({
  headers: ['firstName', 'lastName', 'age'],
  delimiter: '|',
  ignoreColumns: /lastName/,
}).fromString(csv);
console.log(array);
```

`Output`

```
[
  { firstName: 'Russell', age: '23' },
  { firstName: 'Christy', age: '35' },
  { firstName: 'Eleanor', age: '26' }
]
```

# 将 CSV 转换为行数组

通过将`output`选项设置为`'csv'`，我们可以生成一个数组列表，其中每个数组代表一行，包含该行所有列的值。

例如:

`index.js`

```
import csvToJson from 'csvtojson';

const csv = `color,maxSpeed,age
"red",120,2
"blue",100,3
"green",130,2`;
const array = await csvToJson({
  output: 'csv',
}).fromString(csv);
console.log(array);
```

`Output`

```
[
  [ 'red', '120', '2' ],
  [ 'blue', '100', '3' ],
  [ 'green', '130', '2' ]
]
```

# CSV 到数组的本机转换

不使用任何第三方库也可以将 CSV 转换成 JavaScript 数组。

`index.js`

```
function csvToArray(csv) {
  // \n or \r\n depending on the EOL sequence
  const lines = csv.split('\n');
  const delimeter = ',';
  const result = [];
  const headers = lines[0].split(delimeter);
  for (const line of lines) {
    const obj = {};
    const row = line.split(delimeter);
    for (let i = 0; i < headers.length; i++) {
      const header = headers[i];
      obj[header] = row[i];
    }
    result.push(obj);
  }
  // Prettify output
  return JSON.stringify(result, null, 2);
}
const csv = `color,maxSpeed,age
"red",120,2
"blue",100,3
"green",130,2`;
const array = csvToArray(csv);
console.log(array);
```

`Output`

```
[
  {
    "color": "color",
    "maxSpeed": "maxSpeed",
    "age": "age"
  },
  {
    "color": "\"red\"",
    "maxSpeed": "120",
    "age": "2"
  },
  {
    "color": "\"blue\"",
    "maxSpeed": "100",
    "age": "3"
  },
  {
    "color": "\"green\"",
    "maxSpeed": "130",
    "age": "2"
  }
]
```

您可以修改上面的代码，以允许变化和更复杂的 CSV 数据。

*原载于*【codingbeautydev.com】

# *JavaScript 做的每一件疯狂的事情*

*一本关于 JavaScript 微妙的警告和鲜为人知的部分的迷人指南。*

*![](img/143ee152ba78025ea8643ba5b9726a20.png)*

*[**报名**](https://cbdev.link/d3c4eb) 立即免费领取一份。*