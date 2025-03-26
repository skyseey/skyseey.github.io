# Git 管理 📚Flow

1.常用提交代码

` 1.git add . 2.git commit -m "" 3.git push`

2.撤销更改 也就是撤销暂存区里的更改

`git restore ./git restore <file>`

3.存入/取出-栈存区(也就是 你更改了 但是想切换分支 或者 想拉去最新代码 这时就需要)

`git stash /git stash pop`

# Node👫 包 👫 镜像管理 🎒

- NVM 和 NRM

![nvm](nvm.png)

![](nrm.png)

# 网络 🛜

## OCI 七层模型

## TCP

# 小技巧 🔨

- 前端的网络状态怎么判断 ，在线或者离线
  - 使用**navigator.onLine** 返回 true / false
  - window.addEventListener("online/offline",fn())
- 如何区分当前网络是弱网还是强网
  - navigator.connection 返回 object

# Nodejs（模块化）🦜

Nodejs 模块化规范遵循两套一 套`CommonJS`规范另一套`esm`规范

### CommonJS 规范

引入模块（require）支持四种格式

1. 支持引入内置模块例如 `http` `os` `fs` `child_process` 等 nodejs 内置模块
2. 支持引入第三方模块`express` `md5` `koa` 等
3. 支持引入自己编写的模块 ./ ../ 等
4. 支持引入 addon C++扩展模块 .node 文件

```js
const fs = require("node:fs"); // 导入核心模块
const express = require("express"); // 导入 node_modules 目录下的模块
const myModule = require("./myModule.js"); // 导入相对路径下的模块
const nodeModule = require("./myModule.node"); // 导入扩展模块
```

导出模块 exports 和 module.exports

```js
module.exports = {
  hello: function () {
    console.log("Hello, world!");
  },
};
```

如果不想导出对象直接导出值

```js
module.exports = 123;
```

### ESM 模块规范

引入模块 `import` 必须写在头部

> 注意使用 ESM 模块的时候必须开启一个选项 打开 package.json 设置 type:module

```js
import fs from "node:fs";
```

> 如果要引入 json 文件需要特殊处理 需要增加断言并且指定类型 json node 低版本不支持

```js
import data from "./data.json" assert { type: "json" };
console.log(data);
```

加载模块的整体对象

```js
import * as all from "xxx.js";
```

动态导入模块

import 静态加载不支持掺杂在逻辑中如果想动态加载请使用 import 函数模式

```js
if (true) {
  import("./test.js").then();
}
```

模块导出

- 导出一个默认对象 default 只能有一个不可重复 export default

```js
export default {
  name: "test",
};
```

- 导出变量

```js
export const a = 1;
```

- ps:

  1 `import xxx from xx`

  2 `export xxx /export default xxx`

  它是不支持导入 json 的 但是在 vite webpack 使用 loader 帮我们处理了 或者 node 版本在 18 以上 是可以的

  使用 `import json from './data.json' assert {type:'json'}`

### Cjs 和 ESM 的区别

1. Cjs 是基于运行时的同步加载，esm 是基于编译时的异步加载
2. Cjs 是可以修改值的，esm 值并且不可修改（可读的）
3. Cjs 不可以 tree shaking，esm 支持 tree shaking
4. commonjs 中顶层的 this 指向这个模块本身，而 ES6 中顶层 this 指向 undefined

### 环境

- 全局变量
  - Javascript 是有 DOM BOM ECMAscript 三部分组成
  - node -》global
  - 浏览器环境 -》window
  - globalThis 很强大 自动判断 是 node 还是 浏览器

### 关于其他全局 API

> 由于 nodejs 中没有 DOM 和 BOM，除了这些 API，其他的 ECMAscriptAPI 基本都能用

例如

```js
setTimeout setInterval Promise Math  console  Date fetch(node v18) 等...
```

这些 API 都是可以正常用的

### nodejs 内置全局 API

```js
__dirname;
```

它表示当前模块的所在`目录`的绝对路径

```js
__filename;
```

它表示当前模块`文件`的绝对路径，包括文件名和文件扩展名

```js
require module
```

引入模块和模块导出上一章已经详细讲过了

```js
process;
```

1. `process.argv`: 这是一个包含命令行参数的数组。第一个元素是 Node.js 的执行路径，第二个元素是当前执行的 JavaScript 文件的路径，之后的元素是传递给脚本的命令行参数。
2. `process.env`: 这是一个包含当前环境变量的对象。您可以通过`process.env`访问并操作环境变量。
3. `process.cwd()`: 这个方法返回当前工作目录的路径。
4. `process.on(event, listener)`: 用于注册事件监听器。您可以使用`process.on`监听诸如`exit`、`uncaughtException`等事件，并在事件发生时执行相应的回调函数。
5. `process.exit([code])`: 用于退出当前的 Node.js 进程。您可以提供一个可选的退出码作为参数。
6. `process.pid`: 这个属性返回当前进程的 PID（进程 ID）。

这些只是`process`对象的一些常用属性和方法，还有其他许多属性和方法可用于监控进程、设置信号处理、发送 IPC 消息等。

需要注意的是，`process`对象是一个全局对象，可以在任何模块中直接访问，无需导入或定义。

```js
Buffer;
```

1. 创建 `Buffer` 实例：
   - `Buffer.alloc(size[, fill[, encoding]])`: 创建一个指定大小的新的`Buffer`实例，初始内容为零。`fill`参数可用于填充缓冲区，`encoding`参数指定填充的字符编码。
   - `Buffer.from(array)`: 创建一个包含给定数组的`Buffer`实例。
   - `Buffer.from(string[, encoding])`: 创建一个包含给定字符串的`Buffer`实例。
2. 读取和写入数据：
   - `buffer[index]`: 通过索引读取或写入`Buffer`实例中的特定字节。
   - `buffer.length`: 获取`Buffer`实例的字节长度。
   - `buffer.toString([encoding[, start[, end]]])`: 将`Buffer`实例转换为字符串。
3. 转换数据：
   - `buffer.toJSON()`: 将`Buffer`实例转换为 JSON 对象。
   - `buffer.slice([start[, end]])`: 返回一个新的`Buffer`实例，其中包含原始`Buffer`实例的部分内容。
4. 其他方法：
   - `Buffer.isBuffer(obj)`: 检查一个对象是否是`Buffer`实例。
   - `Buffer.concat(list[, totalLength])`: 将一组`Buffer`实例或字节数组连接起来形成一个新的`Buffer`实例。

请注意，从 Node.js 6.0 版本开始，`Buffer`构造函数的使用已被弃用，推荐使用`Buffer.alloc()`、`Buffer.from()`等方法来创建`Buffer`实例。

`Buffer`类在处理文件、网络通信、加密和解密等操作中非常有用，尤其是在需要处理二进制数据时

# 数据库 🔶

- mysqol 开源免费 关系型 表
- oracle 商业收费 关系型 表
- mongoDB 非关系型数据库 键值对

## 1.mysql

- 命令

登录 `mysql - root -p`

展示表格 `show databases`

`create database 'name'`

`if not exists `

`设置字符集 default character set = 'utf8mb4' `

` 创建表 create table ‘user’ ()`

...
