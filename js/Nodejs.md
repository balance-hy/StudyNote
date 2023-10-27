#  Node.js

## npm构建工程

> https://blog.csdn.net/weixin_41619240/article/details/109787518

```shell
npm init  //自己选择

npm init -y //默认生成
```

![image-20231027123117926](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310271231243.png)

```json
{
  "name": "balance01", // 工程名
  "version": "1.0.0", // 版本
  "description": "我是一个node工程", // 描述
  "main": "index.js", // 入口js
  "scripts": { // 运行脚本
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [ // 关键字,用关键字描述项目
    "node"
  ],
  "author": "balance", //开发者
  "license": "ISC" // 授权协议
}
```

## npm 安装/卸载模块

### 快速安装第三方模块

在项目目录下命令行窗口输入：

```shell
npm install xxx
//或者
npm i xxx

//若要指定版本 后面加@版本号
npm i redis@3.0.2
```

**安装的模块会自动放入到项目的node_modules文件夹中**

若无node_modules文件夹，会自动创建，并且会创建package-lock.json文件

在package.json文件中会增加一个属性即依赖项

```json
"dependencies": {
    "redis": "^4.6.10"
}
```

在之后，如果需要重新构建新项目，也用到这些第三方模块，只需要将package.json复制过去，然后输入命令

```shell
npm install
```

会直接下载package.json中所有的依赖项

### 如何使用模块 redis为例

先导入

```js
import { createClient } from 'redis';
```

再使用

```js
import { createClient } from 'redis';

const client = await createClient()
  .on('error', err => console.log('Redis Client Error', err))
  .connect();

await client.set('key', 'value');
const value = await client.get('key');
await client.disconnect();
```

**或者使用require导入**

```js
const myModule = require('./myModule');
```

#### require 和 import

> https://www.jb51.net/article/276886.htm

`import`只能导入ES6模块或者使用Babel等工具转化为ES6模块的代码。而`require`则可以导入CommonJS模块、AMD模块、UMD模块以及Node.js内置模块等多种类型的模块。

### 安装缓慢

先换成阿里云的源，之后使用指令为cnpm install xxx，即可

![image-20231027130449969](https://raw.githubusercontent.com/balance-hy/typora/master/2023img/202310271304845.png)

### 卸载模块

```shell
npm uninstall xxx
```

### package/package-lock

> https://www.php.cn/faq/495596.html

我们每次，去npm install xxx会把内容记录到package.json文件中，下载的包都会发生变化。

为了系统的稳定性考虑，每次执行完npm install之后会对应生成package-lock文件，该文件记录了上一次安装的具体的版本号。

根据官方文档，package-lock.json 是生成的系统当前安装的库的具体来源和版本号，锁定版本。

当你执行npm install的时候， node会先从package.json文件中读取所有dependencies信息，然后根据dependencies中的信息与node_modules中的模块进行对比，没有的直接下载，==node是从package.json文件读取模块名称，从package-lock.json文件中获取版本号，然后进行下载或者更新==。

**当package.json与package-lock.json都不存在**。

执行"npm install"时，node会重新生成package-lock.json文件，然后把node_modules中的模块信息全部记入package-lock.json文件，但不会生成package.json文件。但是，你可以通过"npm init --yes"来生成package.json文件