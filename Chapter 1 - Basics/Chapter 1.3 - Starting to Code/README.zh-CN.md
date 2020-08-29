# 第1.3章-开始编写代码

安装了必备软件之后，就可以开始编写登录到Steam并模拟Steam客户端所需的代码。首先，让我们在当前文件夹中创建一个新文件并将其命名为“ project1.js”。复制以下代码到此文件中。

```js
const SteamUser = require('steam-user');
const client = new SteamUser();

const logOnOptions = {
  accountName: 'your_steam_username',
  password: 'your_steam_password'
};

client.logOn(logOnOptions);

client.on('loggedOn', () => {
  console.log('Logged into Steam');
});
```

用 `node project1.js` 来运行这个程序。如果遇到错误，请查阅主要README中的故障排除部分。 
如果一切正常，则系统将提示您输入2FA (Steam Guard) 代码（如果您的帐户上设置了2FA），输入之后您应该会在命令行中看到“Logged into Steam”消息。

现在，让我们逐步讲解这段代码，

### 模块

在程序的开头，我们看到：

```js
const SteamUser = require('steam-user');
const client = new SteamUser();
```

这里, 我们先将之前用NPM安装的 `steam-user` 模块导入, 然后用 `new SteamUser()` 新建了一个叫做 `client` 的模块实例。
我们可以把 `SteamUser` 和 `client` 换成其他名字，但我们通常给会尽量给起有意义的变量名来增加可读性。


### 对象

当我们导入模块和创建实例之后，我们接下来新建了一个对象。

```js
const logOnOptions = {
  accountName: 'your_steam_username',
  password: 'your_steam_password'
};
```

我们会用 `logOnOptions` 这个对象 来储存你将用来登陆Steam的账号信息，`accountName` and
`password` 作为对象内的键。

### 调用函数

```js
client.logOn(logOnOptions);
```

然后，我们将此`logOnOptions`对象作为参数传递给以下对象的`logOn`方法, 我们的“客户端”，它是 `SteamUser` 的实例。 换一种说法，
我们告诉我们的`SteamUser`实例使用我们的用户名和密码登录Steam.


一个方法只是模块中的一些代码，我们可以通过方法的名字调用它，这里方法的名字是`logOn`。我们可以提供参数给方法，也就是括号中的内容。

### 事件

然后，我们继续添加事件侦听器 (Event listener).

```js
client.on('loggedOn', () => {
  console.log('Logged into Steam');
});
```



`on` 方法具有两个参数 - 事件名称 (`loggedOn`) 和 函数。 当 `client` 发出一个事件，该事件的名称与在 `on` 方法中指定的事件名称匹配时，我们提供的函数将被执行。


当`client`发出一个名为`loggedOn`的事件时，我们告诉它执行一个函数，这个函数我们使用箭头函数定义。 箭头函数，或 `(）=> {...}`，它的效果几乎和 `function()`相同，但是之间有非常重要的区别。我们会在之后的章节会对此差别进行解释。
在函数内部，我们让Node.js将 `Logged into Steam` 输出到命令行。

-----

这样我们就完成了我们的第一个Steam机器人，虽然现在的它看上去并没有什么卵用，都不能让我们的Steam变为上线状态。
但是这很简单，添加以下代码到文件中。



```js
client.on('loggedOn', () => {
  console.log('Logged into Steam');

  client.setPersona(SteamUser.EPersonaState.Online);
  client.gamesPlayed(440);
});
```

如果我们现在使用`node project1.js`运行文件，则应该再次在命令行中看到"Logged into
Steam"。现在如果我们查看Steam账户的信息，它应该上线并且正在玩Team Fortress 2.

这两行代码是我们的机器人将其状态更改为在线并开始玩TF2所需要的。 setPersona方法可以采用两个参数，第一个是 [EPersonaState constant](https://github.com/DoctorMcKay/node-steam-user/blob/master/enums/EPersonaState.js)，
第二个是Steam名称。 Steam名称不是必需的，但是如果您想更改您的Steam名称，则可以设置角色名称。 例如，我们可以使用：



```js
client.setPersona(SteamUser.EPersonaState.Online, 'andrewda');
```

来改变我们的Steam名字到 "andrewda"。 `gamesPlayed`方法需要一个参数 – Steam游戏的appid或非Steam游戏的字符串。 它也可以用于同时闲置多个游戏，但本指南不会对此进行介绍。 请注意，除非是TF2之类的免费游戏，否则您必须拥有游戏才能开始运行。



[继续阅读](../Chapter%201.4%20-%20TOTP)
