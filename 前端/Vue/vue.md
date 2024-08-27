# 	第一章 vue前置知识

## 1.1 ES6模块化

node.js 中默认仅支持 CommonJS 模块化规范，若想基于 node.js 体验与学习 ES6 的模块化语法，可以按照 如下两个步骤进行配置：

① 确保安装了 v14.15.1 或更高版本的 node.js 

② 在 package.json 的根节点中添加 "type": "module" 节点



 ES6 模块化的基本语法：

1. 默认导出： export default 默认导出的成员
   <font color="skyblue">注意：每个模块中，只允许使用唯一的一次 export default，否则会报错！</font>

   ```js
   let n1 = 10
   let n2 = 20
   function show() {
       console.log(n1, n2)
   }
   export default {
       n1,
       show
   }
   ```

2.  默认导入：import 接收名称 from '模块标识符'
   <font color="skyblue">注意：默认导入时的接收名称可以任意名称，只要是合法的成员名称即可</font>

   ```js
   import m1 from "./export.js";
   
   console.log(m1)
   console.log(m1.n1)
   console.log(m1.show)
   ```

3. 按需导出：export 按需导出的成员

   ```js
   export let s1 = 'aaa'
   export let s2 = 123
   export function say() { console.log('hello') }
   ```

4. 按需导入： import { s1 } from '模块标识符'

   ```js
   import { s1, say } from "./exportrequest.js";
   console.log(s1);
   console.log(say);
   ```

   注意：

   ① 每个模块中可以使用多次按需导出 

   ② 按需导入的成员名称必须和按需导出的名称保持一致 

   ③ 按需导入时，可以使用 as 关键字进行重命名 

   ④ 按需导入可以和默认导入一起使用

5. **直接导入并执行模块中的代码：**
   如果<font color="skyblue">只想单纯地执行某个模块中的代码</font>，并不需要得到模块中向外共享的成员。此时，可以直接导入并执行模 块代码

   ```js
   //当前文件模块为chapter01\section01\directExport.js
   for (let i = 0; i < 3; i++) {
       console.log(i);
   }
   
   //直接导入并执行模块代码，不需要得到模块向外共享的成员
   import './directExport.js'
   ```

## 1.2 Promise

### ①Promise 的基本概念

① Promise 是一个构造函数 

​	⚫ 我们可以创建 Promise 的实例 const p = new Promise() 

​	⚫ new 出来的 Promise 实例对象，代表一个异步操作

② Promise.prototype 上包含一个 .then() 方法 

​	⚫ 每一次 new Promise() 构造函数得到的实例对象，

​	⚫ 都可以通过原型链的方式访问到 .then() 方法，例如 p.then() 

③ .then() 方法用来预先指定成功和失败的回调函数 

​	⚫ p.then(成功的回调函数，失败的回调函数) 

​	⚫ p.then(result => { }, error => { }) 

​	⚫ 调用 .then() 方法时，成功的回调函数是必选的、失败的回调函数是可选的

### ②基于 then-fs 读取文件内容

先安装then-fs这个包

```js
npm install then-fs
```

调用 then-fs 提供的 readFile() 方法，可以异步地读取文件的内容，它的返回值是 Promise 的实例对象。因 此可以调用 .then() 方法为每个 Promise 异步操作指定成功和失败之后的回调函数。

```js
import thenFs from "then-fs";

thenFs.readFile('../files/file01.txt', 'utf8')
    .then(r1 => {
        console.log(r1)
        return thenFs.readFile('../files/file02.txt', 'utf8')
    })
    .then(r2 => {
        console.log(r2);
        return thenFs.readFile('../files/file03.txt', 'utf8')
    })
    .then(r3 => {
        console.log(r3);
    })
```

### ③ 通过 .catch 捕获错误

在 Promise 的链式操作中如果发生了错误，可以使用 Promise.prototype.catch 方法进行捕获和处理

```js
import thenFs from "then-fs";

thenFs.readFile('../files/file011.txt', 'utf8')
    .catch(err => {
        console.log(err);
    })
    .then(r1 => {
        console.log(r1)
        return thenFs.readFile('../files/file02.txt', 'utf8')
    })
    .then(r2 => {
        console.log(r2);
        return thenFs.readFile('../files/file03.txt', 'utf8')
    })
    .then(r3 => {
        console.log(r3);
    })
```

### ④Promise.all() 方法

Promise.all() 方法会发起并行的 Promise 异步操作，等所有的异步操作全部结束后才会执行下一步的 .then  操作（等待机制）。

```js
import thenFs from "then-fs";

const promiseArr = [
    thenFs.readFile('../files/file01.txt', 'utf8'),
    thenFs.readFile('../files/file02.txt', 'utf8'),
    thenFs.readFile('../files/file03.txt', 'utf8'),
]

Promise.all(promiseArr)
    .then(([r1, r2, r3]) => {
        console.log(r1, r2, r3);
    })
    .catch(err => {
        console.log(err.message);
    })
```

### ⑤ Promise.race() 方法

Promise.race() 方法会发起并行的 Promise 异步操作，只要任何一个异步操作完成，就立即执行下一步的 .then 操作（赛跑机制）。

```js
import thenFs from "then-fs";

const promiseArr = [
    thenFs.readFile('../files/file01.txt', 'utf8'),
    thenFs.readFile('../files/file02.txt', 'utf8'),
    thenFs.readFile('../files/file03.txt', 'utf8'),
]

Promise.race(promiseArr)
    .then((r1) => {
        console.log(r1);
    })
    .catch(err => {
        console.log(err.message);
    })
```

## 1.3async/await

### ①什么是 async/await

async/await 是 ES8（ECMAScript 2017）引入的新语法，用来简化 Promise 异步操作。在 async/await 出 现之前，开发者只能通过链式 .then() 的方式处理 Promise 异步操作。

### ②async/await 的基本使用

<font color="skyblue">注意事项：</font>

1.  如果在 function 中使用了 await，则 function 必须被 async 修饰
2.  在 async 方法中，第一个 await 之前的代码会同步执行，await 之后的代码会异步执行

```js
import thenFs from "then-fs";

console.log("A");
async function getAllFile() {
    console.log("B");

    const r1 = await thenFs.readFile('../files/file01.txt', 'utf8')
    const r2 = await thenFs.readFile('../files/file02.txt', 'utf8')
    const r3 = await thenFs.readFile('../files/file03.txt', 'utf8')
    console.log(r1, r2, r3);
    console.log("D");

}


getAllFile()
console.log("C");
```

```js
//结果
A
B
C
111 222 333
D
```

## 1.4 EventLoop

### ①JavaScript 是单线程的语言

![image-20220911173812835](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911173812835.png)



单线程执行任务队列的问题： 如果前一个任务非常耗时，则后续的任务就不得不一直等待，从而导致程序假死的问题

### ②同步任务和异步任务

为了防止某个耗时任务导致程序假死的问题，JavaScript 把待执行的任务分为了两类：

① 同步任务（synchronous） 

​	⚫ 又叫做非耗时任务，指的是在主线程上排队执行的那些任务 

​	⚫ 只有前一个任务执行完毕，才能执行后一个任务 

② 异步任务（asynchronous） 

​	⚫ 又叫做耗时任务，异步任务由 JavaScript 委托给宿主环境进行执行 

​	⚫ 当异步任务执行完成后，会通知 JavaScript 主线程执行异步任务的回调函数

### ③同步任务和异步任务的执行过程

![image-20220911173924787](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911173924787.png)

### ④EventLoop的基本概念

JavaScript 主线程从“任务队列”中读取异步 任务的回调函数，放到执行栈中依次执行。这 个过程是循环不断的，所以整个的这种运行机 制又称为 EventLoop（事件循环）。

![image-20220911174002758](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911174002758.png)

### 

# 第二章 webpack

## yarn的安装使用

### yarn包管理器(必须)

快速、可靠、安全的依赖管理工具。和 npm 类似, 都是包管理工具, 可以用于下载包, 就是比npm快

中文官网地址: https://yarn.bootcss.com/

### 下载yarn

下载地址:  https://yarn.bootcss.com/docs/install/#windows-stable 

* windows - 软件包(在笔记文件夹里)

* mac - 通过homebrew安装(看上面地址里)

  * mac如果没安装过homeBrew先运行这个命令

    ```bash
    /usr/bin/ruby -e "$(curl -fsSL http://cdn.jsdelivr.net/gh/ineo6/homebrew-install/install)"
    ```

* 上面命令不行: 试试这个: curl -o- -L https://yarnpkg.com/install.sh | bash (直接安装yarn)

==不要安到带中文的路径下, 建议在C盘/==

### 使用yarn

与npm类似, 可以试试, 新建一个空白文件夹, 执行以下命令尝试一下

```bash
# 1. 初始化, 得到package.json文件(终端路径所在文件夹下)
yarn init

# 2. 添加依赖(下包)
# 命令: yarn add [package]
# 命令: yarn add [package]@[version]
yarn add jquery
yarn add jquery@3.5.1

# 3. 移除包
# 命令: yarn remove [package]
yarn remove jquery
             
# 4. 安装项目全部依赖(一般拿到别人的项目时, 缺少node_modules)          
yarn
# 会根据当前项目package.json记录的包名和版本, 全部下载到当前工程中

# 5. 全局
# 安装: yarn global add [package]
# 卸载: yarn global remove [package]
# 注意: global一定在add左边
yarn global add @vue/cli
# 如何使用, 为明天学习vue做铺垫
```

### yarn可能遇到的问题

如果报错参考报错文档: http://itcz_jiaoyu.gitee.io/error/#811

## 知识点自测

对这些知识点了如指掌, 学习今天的内容会轻松很多

- [x] 什么是模块, 模块化开发规范(CommonJS / ES6)

  commonJS规范:

  ```js
  // nodejs - commonJS规范-规定了导出和导入方式
  // 导出 module.exports = {}
  // 导入 const 变量 = require("模块标识")
  ```

  ES6规范

  ```js
  // 导出 export 或者 export default {}
  // 导入 import 变量名 from '模块标识'
  ```

- [ ] 字体图标的使用

  1. 可以去阿里巴巴矢量图标库, 选中想要的图标, 登录后, 生成css文件和字体文件
  2. 下载css文件和字体文件, 也可以使用在线地址
  3. 在自己页面中引入iconfont.css, 并在想显示字体图标的标签上使用类名即可

- [ ] 箭头函数非常熟练

  ```js
  const fn = () => {}   
  fn()
  
  const fn2 = (a, b) => {return a + b} 
  fn(10, 20); // 结果是30
  
  // 当形参只有一个()可以省略
  const fn3 = a => {return a * 2}
  fn(50); // 结果是100
  
  // 当{}省略return也省略, 默认返回箭头后表达式结果
  const fn4 = a => a * 2;
  fn(50); // 结果是100
  ```

- [ ] 什么是服务器, 本地启动node服务, 服务器和浏览器关系, 服务器作用

  ```bash
  服务器是一台性能高, 24小时可以开机的电脑
  
  服务器可以提供服务(例如: 文件存储, 网页浏览, 资源返回)
  
  在window电脑里安装node后, 可以编写代码用node 启动一个web服务, 来读取本地html文件, 返回给浏览器查看
  
  浏览器 -> 请求资源 -> 服务器
  
  浏览器 <-  响应数据 <- 服务器
  ```

- [ ] 开发环境 和 生产环境 以及英文"development", "production" 2个单词尽量会写会读

- [ ] 初始化包环境和package.json文件作用

  ```bash
  npm下载的包和对应版本号, 都会记录到下载包时终端所在文件夹下的package.json文件里
  ```

- [ ] package.json中的dependencies和 devDependencies区别和作用

  ```bash
  * dependencies  别人使用你的包必须下载的依赖, 比如yarn add  jquery
  
  * devDependencies 开发你的包需要依赖的包,  比如yarn add webpack  webpack-cli -D (-D 相当于 --save-dev)
  ```

- [ ] 终端的熟练使用: 切换路径, 清屏, 包下载命令等

  ```bash
  切换路径  cd  
  
  清屏 cls 或者 clear
  ```

- [ ] 对base64字符串, 图片转base64字符串了解

  在线装换图片http://tool.chinaz.com/tools/imgtobase/

## 1. webpack基本概念

> 目标: webpack本身是, node的一个第三方模块包, 用于打包代码

[webpack官网](https://webpack.docschina.org/)

* 现代 javascript 应用程序的 **静态模块打包器 (module bundler)**

* 为要学的 vue-cli 开发环境做铺垫

> ### ==webpack能做什么==

把很多文件打包整合到一起, 缩小项目体积, 提高加载速度(**演示准备好的例子**)

![image-20220911181349514](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911181349514.png)

其中功能:

* less/sass -> css

* ES6/7/8 -> ES5

* html/css/js -> 压缩合并


## 2. webpack的使用步骤

### 2.0_webpack基础使用

> 目标: 把src下的2个js文件, 打包到1个js中, 并输出到默认dist目录下

默认入口: ./src/index.js

默认出口: ./dist/main.js

==注意:路径上, 文件夹, 文件名不能叫webpack/其他已知的模块名==

1. 初始化包环境

   ```bash
   yarn init
   ```

2. 安装依赖包

   ```bash
   yarn add webpack webpack-cli -D
   ```

3. 在package.json中配置scripts(自定义命令)

   ```bash
   scripts: {
   	"build": "webpack"
   }
   ```

4. 新建目录src

5. 新建src/add/add.js - 定义求和函数导出

   ```js
   export const addFn = (a, b) => a + b
   ```

6. 新建src/index.js导入使用

   ```js
   import {addFn} from './add/add'
   
   console.log(addFn(10, 20));
   ```

7. 运行打包命令

   ```bash
   yarn build
   #或者 npm run build
   ```

> 总结: src并列处, 生成默认dist目录和打包后默认main.js文件

### 2.1_webpack 更新打包

> 目标: 以后代码变更, 如何重新打包呢

1. 新建src/tool/tool.js - 定义导出数组求和方法

   ```js
   export const getArrSum = arr => arr.reduce((sum, val) => sum += val, 0)
   ```

2. src/index.js - 导入使用

   ```js
   import {addFn} from './add/add'
   import {getArrSum} from './tool/tool'
   
   console.log(addFn(10, 20));
   console.log(getArrSum([1, 2, 3]));
   ```

3. 重新打包

   ```bash
   yarn build
   ```

> 总结1: src下开发环境, dist是打包后, 分别独立
>
> 总结2: 打包后格式压缩, 变量压缩等

## 3. webpack的配置

### 3.0_webpack-入口和出口

> 目标: 告诉webpack从哪开始打包, 打包后输出到哪里

默认入口: ./src/index.js

默认出口: ./dist/main.js

webpack配置 - webpack.config.js(默认)

1. 新建src并列处, webpack.config.js
2. 填入配置项

```js
const path = require("path")

module.exports = {
    entry: "./src/main.js", // 入口
    output: { 
        path: path.join(__dirname, "dist"), // 出口路径
        filename: "bundle.js" // 出口文件名
    }
}
```

3. 修改package.json, 自定义打包命令 - 让webpack使用配置文件

```json
"scripts": {
    "build": "webpack"
},
```

4. 打包观察效果

### 3.1_打包流程图

![image-20210421125257233](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210421125257233.png)

==重点: 所有要被打包的资源都要跟入口产生直接/间接的引用关系==

### 3.2_案例-webpack隔行变色

> 目标: 工程化模块化开发前端项目, webpack会对ES6模块化处理

1. 回顾从0准备环境

   * 初始化包环境

   * 下载依赖包
   * 配置自定义打包命令

2. 下载jquery, 新建public/index.html

   ```bash
   yarn add jquery
   ```

   

   ![image-20220911181420397](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911181420397.png)

3. index.html 准备一些li

   * ==因为import语法浏览器支持性不好, 需要被webpack转换后, 再使用JS代码==

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <meta http-equiv="X-UA-Compatible" content="ie=edge">
     <title>Document</title>
   </head>
   <body>
   
   <div id="app">
     <!-- ul>li{我是第$个li}*10 -->
     <ul>
       <li>我是第1个li</li>
       <li>我是第2个li</li>
       <li>我是第3个li</li>
       <li>我是第4个li</li>
       <li>我是第5个li</li>
       <li>我是第6个li</li>
       <li>我是第7个li</li>
       <li>我是第8个li</li>
       <li>我是第9个li</li>
     </ul>
   </div>
   
   </body>
   </html>
   ```

4. 在src/main.js引入jquery

   ```bash
   yarn add jquery
   ```

5. src/main.js中编写隔行变色代码

   ```js
   // 引入jquery
   import $ from 'jquery'
   $(function() {
     $('#app li:nth-child(odd)').css('color', 'red')
     $('#app li:nth-child(even)').css('color', 'green')
   })
   ```

6. 执行打包命令观察效果

7. 可以在dist下把public/index.html引入过来

   ![image-20220911181433949](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220911181433949.png)

   在index.html中==手动==引入js

   ```vue
   <script src="../dist/bundle.js"></script>
   ```

> 总结: 前端工程化模块化, 需要的包yarn下, 被webpack打包后引入到html中使用

### 3.3_插件-自动生成html文件

> 目标: html-webpack-plugin插件, 让webpack打包后生成html文件并自动引入打包后的js

[html-webpack-plugin插件地址](https://www.webpackjs.com/plugins/html-webpack-plugin/)

  1. 下载插件

     ```
     yarn add html-webpack-plugin  -D
     ```

  2. webpack.config.js配置

     ```js
     // 引入自动生成 html 的插件
     const HtmlWebpackPlugin = require('html-webpack-plugin')
     
     module.exports = {
         // ...省略其他代码
         plugins: [
             new HtmlWebpackPlugin({
                 template: './public/index.html' // 以此为基准生成打包后html文件
             })
         ]
     }
     ```

3. 重新打包后观察dist下是否多出html并运行看效果

   ==打包后的index.html自动引入打包后的js文件==

> 总结: webpack就像一个人, webpack.config.js是人物属性, 给它穿什么装备它就干什么活

### 3.4_加载器 - 处理css文件问题

> 目标: 自己准备css文件, 引入到webpack入口, 测试webpack是否能打包css文件

1.新建 - src/css/index.css

2.编写去除li圆点样式代码

3.(重要) 一定要引入到入口才会被webpack打包

4.执行打包命令观察效果

> 总结: 保存原因, 因为webpack默认只能处理js类型文件

### 3.5_加载器 - 处理css文件

> 目标: loaders加载器, 可让webpack处理其他类型的文件, 打包到js中

原因: webpack默认只认识 js 文件和 json文件

[style-loader文档](https://webpack.docschina.org/loaders/style-loader/)

[css-loader文档](https://webpack.docschina.org/loaders/css-loader/)

1. 安装依赖

   ```
   yarn add style-loader css-loader -D
   ```

2. webpack.config.js 配置

   ```js
   const HtmlWebpackPlugin = require('html-webpack-plugin')
   
   module.exports = {
       // ...其他代码
       module: { 
           rules: [ // loader的规则
             {
               test: /\.css$/, // 匹配所有的css文件
               // use数组里从右向左运行
               // 先用 css-loader 让webpack能够识别 css 文件的内容并打包
               // 再用 style-loader 将样式, 把css插入到dom中
               use: [ "style-loader", "css-loader"]
             }
           ]
       }
   }
   ```

3. 新建src/css/li.css - 去掉li默认样式

   ```css
   ul, li{
       list-style: none;
   }
   ```

4. 引入到main.js (因为这里是入口需要产生关系, 才会被webpack找到打包起来)

   ```js
   import "./css/index.css"
   ```

5. 运行打包后dist/index.html观察效果和css引入情况

> 总结: 万物皆模块, 引到入口, 才会被webpack打包, css打包进js中, 然后被嵌入在style标签插入dom上

### 3.6_加载器 - 处理less文件

> 目标: less-loader让webpack处理less文件, less模块翻译less代码

[less-loader文档](https://webpack.docschina.org/loaders/less-loader/)

1. 下载依赖包

   ```bash
   yarn add less less-loader -D
   ```

2. webpack.config.js 配置

   ```js
   module: {
     rules: [ // loader的规则
       // ...省略其他
       {
       	test: /\.less$/,
       	// 使用less-loader, 让webpack处理less文件, 内置还会用less翻译less代码成css内容
           use: [ "style-loader", "css-loader", 'less-loader']
       }
     ]
   }
   ```

3. src/less/index.less  - 设置li字体大小24px

   ```less
   @size:24px;
   
   ul, li{
       font-size: @size
   }
   ```

4. 引入到main.js中

   ```js
   import "./less/index.less"
   ```

5. 打包运行dist/index.html 观察效果

> 总结: 只要找到对应的loader加载器, 就能让webpack处理不同类型文件

### 3.7_加载器 - 处理图片文件

> 目标: 用asset module方式(webpack5版本新增)

[asset module文档](https://webpack.docschina.org/guides/asset-modules/)

如果使用的是webpack5版本的, 直接配置在webpack.config.js - 的 rules里即可

```js
{
    test: /\.(png|jpg|gif|jpeg)$/i,
    type: 'asset'
}
```

如果你用的是webpack4及以前的, 请使用者里的配置

[url-loader文档](https://webpack.docschina.org/loaders/url-loader/)

[file-loader文档](https://webpack.docschina.org/loaders/file-loader/)

1. 下载依赖包

   ```bash
   yarn add url-loader file-loader -D
   ```

2. webpack.config.js 配置

   ```js
   {
     test: /\.(png|jpg|gif|jpeg)$/i,
     use: [
       {
         loader: 'url-loader', // 匹配文件, 尝试转base64字符串打包到js中
         // 配置limit, 超过8k, 不转, file-loader复制, 随机名, 输出文件
         options: {
           limit: 8 * 1024,
         },
       },
     ],
   }
   ```

   图片转成 base64 字符串

   - 好处就是浏览器不用发请求了，直接可以读取
   - 坏处就是如果图片太大，再转`base64`就会让图片的体积增大 30% 左右

3. src/assets/准备老师发的2个图文件

4. 在css/less/index.less - 把小图片用做背景图

   ```less
   body{
       background: url(../assets/logo_small.png) no-repeat center;
   }
   ```

5. 在src/main.js - 把大图插入到创建的img标签上, 添加body上显示

   ```js
   // 引入图片-使用
   import imgUrl from './assets/1.gif'
   const theImg = document.createElement("img")
   theImg.src = imgUrl
   document.body.appendChild(theImg)
   ```

6. 打包运行dist/index.html观察2个图片区别

> 总结:  url-loader 把文件转base64 打包进js中, 会有30%的增大, file-loader 把文件直接复制输出

### 3.8_webpack加载文件优缺点

图片转成 base64 字符串

- 好处就是浏览器不用发请求了，直接可以读取
- 坏处就是如果图片太大，再转`base64`就会让图片的体积增大 30% 左右

### 3.9_加载器 - 处理字体文件

> 目标: 用asset module技术, asset/resource直接输出到dist目录下

webpack5使用这个配置

```js
{ // webpack5默认内部不认识这些文件, 所以当做静态资源直接输出即可
    test: /\.(eot|svg|ttf|woff|woff2)$/,
    type: 'asset/resource',
    generator: {
    	filename: 'font/[name].[hash:6][ext]'
    }
}
```

webpack4及以前使用下面的配置

1. webpack.config.js - 准备配置

   ```js
    { // 处理字体图标的解析
        test: /\.(eot|svg|ttf|woff|woff2)$/,
            use: [
                {
                    loader: 'url-loader',
                    options: {
                        limit: 2 * 1024,
                        // 配置输出的文件名
                        name: '[name].[ext]',
                        // 配置输出的文件目录
                        outputPath: "fonts/"
                    }
                }
            ]
    }
   ```

2. src/assets/ - 放入字体库fonts文件夹

3. 在main.js引入iconfont.css

   ```js
   // 引入字体图标文件
   import './assets/fonts/iconfont.css'
   ```

4. 在public/index.html使用字体图标样式

   ```html
   <i class="iconfont icon-weixin"></i>
   ```

5. 执行打包命令-观察打包后网页效果

> 总结: url-loader和file-loader 可以打包静态资源文件

### 3.10_加载器 - 处理高版本js语法

> 目标: 让webpack对高版本 的js代码, 降级处理后打包

写代码演示: 高版本的js代码(箭头函数), 打包后, 直接原封不动打入了js文件中, 遇到一些低版本的浏览器就会报错

原因: **webpack 默认仅内置了 模块化的 兼容性处理**   `import  export`

babel 的介绍 => 用于处理高版本 js语法 的兼容性  [babel官网](https://www.babeljs.cn/)

解决: 让webpack配合babel-loader 对js语法做处理

[babel-loader文档](https://webpack.docschina.org/loaders/babel-loader/)

  1. 安装包

     ```bash
     yarn add -D babel-loader @babel/core @babel/preset-env
     ```

  2. 配置规则

     ```js
     module: {
       rules: [
         {
             test: /\.js$/,
             exclude: /(node_modules|bower_components)/,
             use: {
                 loader: 'babel-loader',
                 options: {
                     presets: ['@babel/preset-env'] // 预设:转码规则(用bable开发环境本来预设的)
                 }
             }
         }
       ]
     }
     ```

3. 在main.js中使用箭头函数(高版本js)

   ```js
   // 高级语法
   const fn = () => {
     console.log("你好babel");
   }
   console.log(fn) // 这里必须打印不能调用/不使用, 不然webpack会精简成一句打印不要函数了/不会编译未使用的代码
   // 没有babel集成时, 原样直接打包进lib/bundle.js
   // 有babel集成时, 会翻译成普通函数打包进lib/bundle.js
   ```

4. 打包后观察lib/bundle.js - 被转成成普通函数使用了 - 这就是babel降级翻译的功能

> 总结: babel-loader 可以让webpack 对高版本js语法做降级处理后打包

## 4. webpack 开发服务器

### 4.0_webpack开发服务器-为何学?

文档地址: https://webpack.docschina.org/configuration/dev-server/

抛出问题: 每次修改代码, 都需要重新 yarn build 打包, 才能看到最新的效果, 实际工作中, 打包 yarn build 非常费时 (30s - 60s) 之间

为什么费时? 

1. 构建依赖
2. 磁盘读取对应的文件到内存, 才能加载  
3. 用对应的 loader 进行处理  
4. 将处理完的内容, 输出到磁盘指定目录  

解决问题: 起一个开发服务器,  在电脑内存中打包, 缓存一些已经打包过的内容, 只重新打包修改的文件, 最终运行加载在内存中给浏览器使用

### ==4.1_webpack-dev-server自动刷新==

> 目标: 启动本地服务, 可实时更新修改的代码, 打包**变化代码**到内存中, 然后直接提供端口和网页访问

1. 下载包

   ```bash
   yarn add webpack-dev-server -D
   ```

2. 配置自定义命令

   ```js
   scripts: {
   	"build": "webpack",
   	"serve": "webpack serve"
   }
   ```

3. 运行命令-启动webpack开发服务器

   ```bash
   yarn serve
   #或者 npm run serve
   ```

> 总结: 以后改了src下的资源代码, 就会直接更新到内存打包, 然后反馈到浏览器上了

### 4.2_webpack-dev-server配置

1. 在webpack.config.js中添加服务器配置

   更多配置参考这里: https://webpack.docschina.org/configuration/dev-server/#devserverafter

   ```js
   module.exports = {
       // ...其他配置
       devServer: {
         port: 3000 // 端口号
       }
   }
   ```

## 今日总结

- [ ] 什么是webpack, 它有什么作用
- [ ] 知道yarn的使用过程, 自定义命令, 下载删除包
- [ ] 有了webpack让模块化开发前端项目成为了可能, 底层需要node支持
- [ ] 对webpack各种配置项了解
  - [ ] 入口/出口
  - [ ] 插件
  - [ ] 加载器
  - [ ] mode模式
  - [ ] devServer
- [ ] webpack开发服务器的使用和运作过程

## 面试题

### 1、什么是webpack（必会）

​	webpack是一个打包模块化javascript的工具，在webpack里一切文件皆模块，通过loader转换文件，通过plugin注入钩子，最后输出由多个模块组合成的文件，webpack专注构建模块化项目

### 2、Webpack的优点是什么？（必会）

1. 专注于处理模块化的项目，能做到开箱即用，一步到位
2. 通过plugin扩展，完整好用又不失灵活
3. 通过loaders扩展, 可以让webpack把所有类型的文件都解析打包
4. 区庞大活跃，经常引入紧跟时代发展的新特性，能为大多数场景找到已有的开源扩展

### 3、webpack的构建流程是什么?从读取配置到输出文件这个过程尽量说全（必会）

​    Webpack 的运行流程是一个串行的过程，从启动到结束会依次执行以下流程：

​	1. 初始化参数：从配置文件读取与合并参数，得出最终的参数

 	2. 开始编译：用上一步得到的参数初始化 Compiler 对象，加载所有配置的插件，开始执行编译
 	3. 确定入口：根据配置中的 entry 找出所有的入口文件
 	4. 编译模块：从入口文件出发，调用所有配置的 Loader 对模块进行翻译，再找出该模块依赖的模块，再递归本步骤直到所有入口依赖的文件都经过了本步骤的处理
 	5. 完成模块编译：在经过第4步使用 Loader 翻译完所有模块后，得到了每个模块被翻译后的最终内容以及它们之间的依赖关系
 	6. 输出资源：根据入口和模块之间的依赖关系，组装成一个个包含多个模块的 Chunk，再把每个 Chunk 转换成一个单独的文件加入到输出列表，这步是可以修改输出内容的最后机会
 	7. 输出完成：在确定好输出内容后，根据配置确定输出的路径和文件名，把文件内容写入到文件系统。

在以上过程中，Webpack 会在特定的时间点广播出特定的事件，插件在监听到感兴趣的事件后会执行特定的逻辑，并且插件可以调用 Webpack 提供的 API 改变 Webpack 的运行结果

### 4、说一下 Webpack 的热更新原理(必会)

​	webpack 的热更新又称热替换（Hot Module Replacement），缩写为 HMR。这个机制可以做到不用刷新浏览器而将新变更的模块替换掉旧的模块。

​    HMR的核心就是客户端从服务端拉去更新后的文件，准确的说是 chunk diff (chunk 需要更新的部分)，实际上 WDS 与浏览器之间维护了一个 Websocket，当本地资源发生变化时，WDS 会向浏览器推送更新，并带上构建时的 hash，让客户端与上一次资源进行对比。客户端对比出差异后会向 WDS 发起 Ajax 请求来获取更改内容(文件列表、hash)，这样客户端就可以再借助这些信息继续向 WDS 发起 jsonp 请求获取该chunk的增量更新。

​    后续的部分(拿到增量更新之后如何处理？哪些状态该保留？哪些又需要更新？)由 HotModulePlugin 来完成，提供了相关 API 以供开发者针对自身场景进行处理，像react-hot-loader 和 vue-loader 都是借助这些 API 实现 HMR。

### 5、webpack与grunt、gulp的不同？（必会）

​    **1)** **三者之间的区别**

​       三者都是前端构建工具，grunt和gulp在早期比较流行，现在webpack相对来说比较主流，不过一些轻量化的任务还是会用gulp来处理，比如单独打包CSS文件等。

​       grunt和gulp是基于任务和流（Task、Stream）的。类似jQuery，找到一个（或一类）文件，对其做一系列链式操作，更新流上的数据， 整条链式操作构成了一个任务，多个任务就构成了整个web的构建流程。

​       webpack是基于入口的。webpack会自动地递归解析入口所需要加载的所有资源文件，然后用不同的Loader来处理不同的文件，用Plugin来扩展webpack功能。

​    **2)** **从构建思路来说**

​       gulp和grunt需要开发者将整个前端构建过程拆分成多个`Task`，并合理控制所有`Task`的调用关系 webpack需要开发者找到入口，并需要清楚对于不同的资源应该使用什么Loader做何种解析和加工

​    **3)** **对于知识背景来说**

​       gulp更像后端开发者的思路，需要对于整个流程了如指掌 webpack更倾向于前端开发者的思路

### 6、有哪些常见的Loader？他们是解决什么问题的？（必会）

1、  file-loader：把文件输出到一个文件夹中，在代码中通过相对 URL 去引用输出的文件

2、  url-loader：和 file-loader 类似，但是能在文件很小的情况下以 base64 的方式把文件内容注入到代码中去

3、  source-map-loader：加载额外的 Source Map 文件，以方便断点调试

4、  image-loader：加载并且压缩图片文件

5、  babel-loader：把 ES6 转换成 ES5

6、  css-loader：加载 CSS，支持模块化、压缩、文件导入等特性

7、  style-loader：把 CSS 代码注入到 JavaScript 中，通过 DOM 操作去加载 CSS。

8、  eslint-loader：通过 ESLint 检查 JavaScript 代码

### 7、Loader和Plugin的不同？（必会）

​    **1)** **不同的作用**

​       Loader直译为"加载器"。Webpack将一切文件视为模块，但是webpack原生是只能解析js文件，如果想将其他文件也打包的话，就会用到loader。 所以Loader的作用是让webpack拥有了加载和解析非JavaScript文件的能力。

​    Plugin直译为"插件"。Plugin可以扩展webpack的功能，让webpack具有更多的灵活性。 在 Webpack 运行的生命周期中会广播出许多事件，Plugin 可以监听这些事件，在合适的时机通过 Webpack 提供的 API 改变输出结果。

**2)** **不同的用法**

​    Loader在module.rules中配置，也就是说他作为模块的解析规则而存在。 类型为数组，每一项都是一个Object，里面描述了对于什么类型的文件（test），使用什么加载(loader)和使用的参数（options）

​    Plugin在plugins中单独配置。 类型为数组，每一项是一个plugin的实例，参数都通过构造函数传入。

## 今日作业

1. 把课上webpack的配置从0开始, 再过一遍

2. (附加)可以调研, 在打包时, 如何把css提取成一个独立的文件

   效果: 打包后的文件夹下多一个独立的css文件里有css代码

   提示: 需要一个加载器

3. (附加)可以调用如何把vue文件让webpack打包使用 (vue-loader官网)

   想要把App.vue的东西显示到index.html

   (1): 在public/index.html 准备id叫app的div

   (2): yarn add vue - 必须下载vue (和其他加载器和插件-具体参考vue-loader官网)

   (3): 需要在main.js中引入App.vue模块对象并加入如下代码

   ```js
   import App from './App.vue' // 根vue文件
   import Vue from 'vue' // 引入vue.js对象
   
   new Vue({ 
     render: h => h(App) // 渲染函数, 渲染App组件里的标签
   }).$mount('#app') // 把vue文件的标签结构 -> 挂载到id为app的标签里
   ```

   (4): 打包后运行dist/index.html, 观察是否把vue文件里的标签渲染到页面了

4. 预习明天的笔记.md



# 第三章 Vue脚手架、基础API

## 预习安装项目

### 可选安装 - 谷歌访问助手

这是一个谷歌浏览器上的插件

安装

1. 必安插件(文件夹)下的 ==google-access-helper-2.3.0(文件夹)==  复制到你想放的文件夹下(==安装后不可以挪动位置==)

> 建议D盘下, 弄一个专门按软件的文件夹

2. 打开谷歌浏览器-扩展程序-开发者模式打开-把文件夹拖进来就安装完毕


功能如下:

谷歌浏览器上

* 同步书签(需要注册和登录, 开启同步功能) - 可以暂不使用(因为有的手机号可能注册不了谷歌账号)
* 谷歌商店(无需登录, 安装其他插件)

### 必安装 - vue-devtools

学习, 调试vue必备之利器 - 官方提供的呦

右上角-插件-谷歌访问助手-打开Chrome商店-搜索vue-devtools回车-然后添加至Chrome等待下载后自动安装-右上角显示已经添加即代表成功

如果实在打不开谷歌商店, 换个网 / 直接用备用文件夹里的vue-devtools插件包安装到浏览器扩展程序也一样用

==不要图标上带橘黄色beta的==

==如果这个网址打不开, 就用预习资料里备用的本地版安装也可以, 安装过程和上个插件安装过程一致==

### vscode-插件补充

vue文件代码高亮插件-vscode中安装

![image-20210212192713936](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210212192713936.png)

代码提示插件-vscode中安装

![image-20210304223236080](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210304223236080.png)

## 知识点自测

想学会今天的内容, 先测测这几个会不会

- [ ] 表达式, 变量是什么
- [ ] new的作用和含义
- [ ] 实例化对象
- [ ] 什么是对象上的, 属性和方法
- [ ] 对象的赋值和取值
- [ ] this的指向
- [ ] npm/yarn是什么, package.json干什么的, 下载包的命令是什么, 什么是模块化开发
- [ ] 函数的形参实参, 得马上反应过来, 哪个是变量哪个是值, 谁传给谁了

如果通不过, 请记住口诀:

1. 变量是一个容器, 表达式原地都有返回结果

   ```js
   var a = 10;
   console.log(a); // a就是变量, 运行后使用变量里的值再原地打印
   console.log(10 + 50); // 10 + 50 就是表达式
   console.log(a > 9); // 这叫判断表达式, 原地结果是true
   ```

2. new 类名() - 原地得到一个实例对象 - 对象身上有key(或叫属性, 叫键都行), 对应的值是我们要使用的

3. 实例化对象就是new 类名() 创造出来的对象, 身上包含属性(key, 键) 对应的 值

4. 什么是属性和方法(固定格式)

   ```js
   let obj = { // 属性指的是a, b, c, d, e这些名字
       a: 10,
       b: [1, 2, 3],
       c: function(){},
       d () {},
       e: () => {} // 值是冒号:右边的值
   }
   // 这个格式是固定的, 必须张口就来, 张手就写, 准确率100%
   ```

5. 对象的复制和取值(固定格式)

   有=(赋值运算符) 就是赋值, 没有就是取值

   ```js
   let obj = {
       a: 10,
       b: 20
   }
   console.log(obj.a); // 从obj对象的a上取值, 原地打印10
   obj.b = 100; // 有=, 固定把右侧的值赋予给左侧的键, 再打印obj这个对象, b的值是100了
   ```

6. this指向口诀

   在function函数中, this默认指向当前函数的调用者  调用者.函数名()

   在箭头函数中, this指向外层"函数"作用域this的值

## 今日学习目标

1. 能够理解vue的概念和作用
2. 能够理解vuecli脚手架工程化开发
3. 能够使用vue指令

## 1. Vue基本概念

### 1.0_为何学Vue

> 目标: 更少的时间,干更多的活. 开发网站速度快

例如: 把数组数据-循环铺设到li中, 看看分别如何做的?

原生js做法

```vue
<ul id="myUl"></ul>
<script>
    let arr = ["春天", "夏天", "秋天", "冬天"];
    let myUl = document.getElementById("myUl");
    for (let i = 0; i < arr.length; i++) {
        let theLi = document.createElement("li");
        theLi.innerHTML = arr[i];
        myUl.appendChild(theLi);
    }
</script>
```

Vue.js做法

```vue
<li v-for="item in arr">{{item}}</li>
<script>
    new Vue({
        // ...
        data: {
            arr: ["春天", "夏天", "秋天", "冬天"] 
        }
    })
</script>
```

注意: 虽然vue写起来很爽, 但是一定不要忘记, vue的底层还是原生js

开发更加的效率和简洁, 易于维护, 快!快!快!就是块 (甚至测试, Java, Python工程师都要学点vue, 方便与前端沟通)

现在很多项目都是用vue开发的

![image-20210317180240323](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210317180240323.png)

市场上90%工作都要求会vue, 会vue拿高薪, 甚至java或测试都要学点vue

### 1.1_Vue是什么

logo镇楼

![Vue](https://gitee.com/XXXTENTWXD/pic/raw/master/images/VUE-logo.png)

==渐进式==javacript==框架==, 一套拥有自己规则的语法

官网地址: https://cn.vuejs.org/ (作者: 尤雨溪)

> ### 什么是渐进式

渐进式: 逐渐进步, 想用什么就用什么, 不必全都使用
![image-20220912175048318](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175048318.png)

Vue渐进式: Vue从基础开始, 会循序渐进向前学习, 如下知识点可能你现在不明白, 但是学完整个vue回过头来看, 会很有帮助

![image-20220912175056691](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175056691.png)



> ### 什么是库和框架

补充概念:

库:  封装的属性或方法 (例jquery.js)

框架: 拥有自己的规则和元素, 比库强大的多 (例vue.js)

![image-20220912175106762](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175106762.png)

![image-20220912175116213](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175116213.png)

### 1.2_Vue学习的方式

+ 传统开发模式：基于html/css/js文件开发vue

  ![image-20210228083641377](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210228083641377.png)

+ 工程化开发方式：在webpack环境中开发vue，这是最推荐, 企业常用的方式

  ![image-20220912175126171](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175126171.png)



> ### Vue如何学

1. 每天的知识点自测最好做到了如指掌 - 做不到只能花30分钟去记住结论和公式
2. 记住vue指令作用, 基础语法 - 弄一个字典(一一映射关系)
3. 在课上例子, 练习, 案例, 作业, 项目中, 反复磨炼使用
4. 学会查找问题的方式和解决方式(弄个报错总结.md, 避免反复进坑)

> 总结: vue是渐进式框架, 有自己的规则, 我们要记住语法, 特点和作用, 反复磨炼使用, 多总结

## 2. @vue/cli脚手架

### 2.0_@vue/cli 脚手架介绍

> 目标: webpack自己配置环境很麻烦, 下载@vue/cli包,用vue命令创建脚手架项目

- @vue/cli是Vue官方提供的一个全局模块包(得到vue命令), 此包用于创建脚手架项目脚手架是为了保证各施工过程顺利进行而搭设的工作平台


![image-20220912175140241](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175140241.png)

> ### @vue/cli的好处

- 开箱即用

  0配置webpack

  babel支持

  css, less支持

  开发服务器支持

### 2.1_@vue/cli安装

> 目标: 把@vue/cli模块包按到全局, 电脑拥有vue命令, 才能创建脚手架工程

+ 全局安装命令

```bash
yarn global add @vue/cli
# OR
npm install -g @vue/cli
```

注意: 如果半天没动静(95%都是网速问题), 可以ctrl c 

1. 停止重新来

2. 换一个网继续重来

+ 查看`vue`脚手架版本

```bash
vue -V
```

> 总结: 如果出现版本号就安装成功, 否则失败

### 2.2_@vue/cli 创建项目启动服务

> 目标: 使用vue命令, 创建脚手架项目

==注意: 项目名不能带大写字母, 中文和特殊符号==

1. 创建项目

```bash
# vue和create是命令, vuecli-demo是文件夹名
vue create vuecli-demo
```

2. 选择模板

   ==可以上下箭头选择, 弄错了ctrl+c重来==

![image-20220912175149997](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175149997.png)

​	 	选择用什么方式下载脚手架项目需要的依赖包![image-20220912175158331](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175158331.png)

3. 回车等待生成项目文件夹+文件+下载必须的第三方包们

![image-20220912175208928](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175208928.png)

4. 进入脚手架项目下, 启动内置的热更新本地服务器

```bash
cd vuecil-demo

npm run serve
# 或
yarn serve
```

只要看到绿色的 - 啊. 你成功了(底层node+webpack热更新服务)

![image-20220912175223348](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175223348.png)

打开浏览器输入上述地址

![image-20220912175232025](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175232025.png)



> 总结: vue命令创建工程目录, 项目内置webpack本地热更新服务器, 帮我们打包项目预览项目

### 2.3 @vue/cli 目录和代码分析

> 目标: 讲解重点文件夹, 文件的作用, 以及文件里代码的意思

```bash
 vuecil-demo        # 项目目录
    ├── node_modules # 项目依赖的第三方包
    ├── public       # 静态文件目录
      ├── favicon.ico# 浏览器小图标
      └── index.html # 单页面的html文件(网页浏览的是它)
    ├── src          # 业务文件夹
      ├── assets     # 静态资源
        └── logo.png # vue的logo图片
      ├── components # 组件目录
        └── HelloWorld.vue # 欢迎页面vue代码文件 
      ├── App.vue    # 整个应用的根组件
      └── main.js    # 入口js文件
    ├── .gitignore   # git提交忽略配置
    ├── babel.config.js  # babel配置
    ├── package.json  # 依赖包列表
    ├── README.md    # 项目说明
	└── yarn.lock    # 项目包版本锁定和缓存地址
```

主要文件及含义

```js
node_modules下都是下载的第三方包
public/index.html – 浏览器运行的网页
src/main.js – webpack打包的入口文件
src/App.vue – vue项目入口页面
package.json – 依赖包列表文件
```

### 2.4_@vue/cli 项目架构了解

> 目标: 知道项目入口, 以及代码执行顺序和引入关系

![image-20210317201811310](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210317201811310.png)

### 2.5_@vue/cli 自定义配置

> 目标：项目中没有webpack.config.js文件，因为@vue/cli用的vue.config.js

src并列处新建vue.config.js

```jsx
/* 覆盖webpack的配置 */
module.exports = {
  devServer: { // 自定义服务配置
    host: 'localhost',	//使用localhost自动打开网页，否则是自动使用0.0.0.0打开
    open: true, // 自动打开浏览器
    port: 3000
  }
}
```

### 2.6_eslint了解

> 目标: 知道eslint的作用, 和如何暂时关闭, 它是一个==代码检查工具==

例子: 先在main.js 随便声明个变量, 但是不要使用

![image-20210326165406694](F:\前端\05、阶段五 Vue.js项目实战开发\资料\webpack+Vue基础课程资料\Day02_vue脚手架_基础API\01_笔记和ppt\images\image-20210326165406694.png)

观察发现, 终端和页面都报错了

==记住以后见到这样子的错误, 证明你的代码不严谨==

![image-20220912175251173](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175251173.png)

![image-20220912175301103](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220912175301103.png)

方式1: 手动解决掉错误, 以后项目中会讲如何自动解决

方式2: 暂时关闭eslint检查(因为现在主要精力在学习Vue语法上), 在vue.config.js中配置后重启服务

![image-20210511112152702](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511112152702.png)

### 2.7_@vue/cli 单vue文件讲解

> 目标: 单vue文件好处, 独立作用域互不影响

Vue推荐采用.vue文件来开发项目

template里只能有一个根标签

vue文件-独立模块-作用域互不影响

style配合scoped属性, 保证样式只针对当前template内标签生效

vue文件配合webpack, 把他们打包起来插入到index.html

```vue
<!-- template必须, 只能有一个根标签, 影响渲染到页面的标签结构 -->
<template>
  <div>欢迎使用vue</div>
</template>

<!-- js相关 -->
<script>
export default {
  name: 'App'
}
</script>

<!-- 当前组件的样式, 设置scoped, 可以保证样式只对当前页面有效 -->
<style scoped>
</style>

```

最终: Vue文件配合webpack, 把他们打包起来插入到index.html, 然后在浏览器运行

### 2.8_@vue/cli 欢迎界面清理

> 目标: 我们开始写我们自己的代码, 无需欢迎页面

* src/App.vue默认有很多内容, 可以全部删除留下框
* assets 和 components 文件夹下的一切都删除掉 (不要默认的欢迎页面)

## ==3. Vue指令==

### 3.0_vue基础-插值表达式

> 目的: 在dom标签中, 直接插入内容

又叫: 声明式渲染/文本插值

语法: {{ 表达式 }}

```jsx
<template>
  <div>
    <h1>{{ msg }}</h1>
    <h2>{{ obj.name }}</h2>
    <h3>{{ obj.age > 18 ? '成年' : '未成年' }}</h3>
  </div>
</template>

<script>
export default {
  data() { // 格式固定, 定义vue数据之处
    return {  // key相当于变量名
      msg: "hello, vue",
      obj: {
        name: "小vue",
        age: 5
      }
    }
  }
}
</script>

<style>
</style>

```

> 总结: dom中插值表达式赋值, vue的变量必须在data里声明

### 3.1_vue基础-MVVM设计模式

> 目的: 转变思维, 用数据驱动视图改变, 操作dom的事, vue源码内干了

设计模式: 是一套被反复使用的、多数人知晓的、经过分类编目的、代码设计经验的总结。

演示: 在上个代码基础上, 在devtool工具改变M层的变量, 观察V层(视图的自动同步)

等下面学了v-model再观察V改变M的效果

![1](https://gitee.com/XXXTENTWXD/pic/raw/master/images/1.gif)

+ MVVM，一种软件架构模式，决定了写代码的思想和层次
  + M：   model数据模型          (data里定义)	
  + V：    view视图                   （html页面）
  + VM： ViewModel视图模型  (vue.js源码)

- MVVM通过`数据双向绑定`让数据自动地双向同步  **不再需要操作DOM**
  - V（修改视图） -> M（数据自动同步）
  - M（修改数据） -> V（视图自动同步）

![MVVM](https://gitee.com/XXXTENTWXD/pic/raw/master/images/MVVM.png)

**1. 在vue中，不推荐直接手动操作DOM！！！**  

**2. 在vue中，通过数据驱动视图，不要在想着怎么操作DOM，而是想着如何操作数据！！**(思想转变)

![双向数据绑定](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E5%8F%8C%E5%90%91%E6%95%B0%E6%8D%AE%E7%BB%91%E5%AE%9A.png)

> 总结: vue源码内采用MVVM设计模式思想, 大大减少了DOM操作, 挺高开发效率

### 3.2_vue指令-v-bind

> 目标: 给标签属性设置vue变量的值

**vue指令, 实质上就是特殊的 html 标签属性, 特点:  v- 开头**

每个指令, 都有独立的作用

- 语法：`v-bind:属性名="vue变量"`
- 简写：`:属性名="vue变量"`

```html
<!-- vue指令-v-bind属性动态赋值 -->
<a v-bind:href="url">我是a标签</a>
<img :src="imgSrc">
```

> 总结: 把vue变量的值, 赋予给dom属性上, 影响标签显示效果

### 3.3_vue指令-v-on

> 目标: 给标签绑定事件

* 语法
  * v-on:事件名="要执行的==少量代码=="
  * v-on:事件名="methods中的函数"
  * v-on:事件名="methods中的函数(实参)" 
* 简写: @事件名="methods中的函数"

```html
<!-- vue指令:   v-on事件绑定-->
<p>你要买商品的数量: {{count}}</p>
<button v-on:click="count = count + 1">增加1</button>
<button v-on:click="addFn">增加1个</button>
<button v-on:click="addCountFn(5)">一次加5件</button>

<button @click="subFn">减少</button>

<script>
    export default {
        // ...其他省略
        methods: {
            addFn(){ // this代表export default后面的组件对象(下属有data里return出来的属性)
                this.count++
            },
            addCountFn(num){
                this.count += num
            },
            subFn(){
                this.count--
            }
        }
    }
</script>
```

> 总结: 常用@事件名, 给dom标签绑定事件, 以及=右侧事件处理函数

### 3.4_vue指令-v-on事件对象

> 目标: vue事件处理函数中, 拿到事件对象

* 语法:
  * 无传参, 通过形参直接接收
  * 传参, 通过$event指代事件对象传给事件处理函数

```vue
<template>
  <div>
    <a @click="one" href="http://www.baidu.com">阻止百度</a>
    <hr>
    <a @click="two(10, $event)" href="http://www.baidu.com">阻止去百度</a>
  </div>
</template>

<script>
export default {
  methods: {
    one(e){
      e.preventDefault()
    },
    two(num, e){
      e.preventDefault()
    }
  }
}
</script>
```

### 3.5_vue指令-v-on修饰符

> 目的: 在事件后面.修饰符名 - 给事件带来更强大的功能

* 语法:
  * @事件名.修饰符="methods里函数"
    * .stop - 阻止事件冒泡
    * .prevent - 阻止默认行为
    * .once - 程序运行期间, 只触发一次事件处理函数

```html
<template>
  <div @click="fatherFn">
    <!-- vue对事件进行了修饰符设置, 在事件后面.修饰符名即可使用更多的功能 -->
    <button @click.stop="btn">.stop阻止事件冒泡</button>
    <a href="http://www.baidu.com" @click.prevent="btn">.prevent阻止默认行为</a>
    <button @click.once="btn">.once程序运行期间, 只触发一次事件处理函数</button>
  </div>
</template>

<script>
export default {
  methods: {
    fatherFn(){
      console.log("father被触发");
    },
    btn(){
      console.log(1);
    }
  }
}
</script>
```

> 总结: 修饰符给事件扩展额外功能

### 3.6_vue指令-v-on按键修饰符

> 目标: 给键盘事件, 添加修饰符, 增强能力

* 语法:
  * @keyup.enter  -  监测回车按键
  * @keyup.esc     -   监测返回按键

[更多修饰符](https://cn.vuejs.org/v2/guide/events.html#%E6%8C%89%E9%94%AE%E4%BF%AE%E9%A5%B0%E7%AC%A6) 

```html
<template>
  <div>
    <input type="text" @keydown.enter="enterFn">
    <hr>
    <input type="text" @keydown.esc="escFn">
  </div>
</template>

<script>
export default {
 methods: {
   enterFn(){
     console.log("enter回车按键了");
   },
   escFn(){
     console.log("esc按键了");
   }
 }
}
</script>
```

> 总结: 多使用事件修饰符, 可以提高开发效率, 少去自己判断过程

### 3.7_课上练习-翻转世界

> 目标: 点击按钮 - 把文字取反显示 - 再点击取反显示(回来了)

> 提示: 把字符串取反赋予回去

正确代码:

```html
<template>
  <div>
    <h1>{{ message }}</h1>
    <button @click="btn">逆转世界</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: "HELLO, WORLD",
    };
  },
  methods: {
    btn(){
      this.message = this.message.split("").reverse().join("")
    }
  }
};
</script>
```

> 总结: 记住方法特点, 多做需求, vue是数据变化视图自动更新, 减少操作DOM时间, 提高开发效率

### 3.8_vue指令 v-model

> 目标: 把value属性和vue数据变量, 双向绑定到一起

* 语法: v-model="vue数据变量"
* 双向数据绑定
  * 数据变化 -> 视图自动同步
  * 视图变化 -> 数据自动同步
* 演示: 用户名绑定 - vue内部是MVVM设计模式

```vue
<template>
  <div>
    <!-- 
    	v-model:是实现vuejs变量和表单标签value属性, 双向绑定的指令
    -->
    <div>
      <span>用户名:</span>
      <input type="text" v-model="username" />
    </div>
    <div>
      <span>密码:</span>
      <input type="password" v-model="pass" />
    </div>
    <div>
      <span>来自于: </span>
      <!-- 下拉菜单要绑定在select上 -->
      <select v-model="from">
        <option value="北京市">北京</option>
        <option value="南京市">南京</option>
        <option value="天津市">天津</option>
      </select>
    </div>
    <div>
      <!-- (重要)
      遇到复选框, v-model的变量值
      非数组 - 关联的是复选框的checked属性
      数组   - 关联的是复选框的value属性
       -->
      <span>爱好: </span>
      <input type="checkbox" v-model="hobby" value="抽烟">抽烟
      <input type="checkbox" v-model="hobby" value="喝酒">喝酒
      <input type="checkbox" v-model="hobby" value="写代码">写代码
    </div>
    <div>
      <span>性别: </span>
      <input type="radio" value="男" name="sex" v-model="gender">男
      <input type="radio" value="女" name="sex" v-model="gender">女
    </div>
    <div>
      <span>自我介绍</span>
      <textarea v-model="intro"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: "",
      pass: "",
      from: "",
      hobby: [], 
      sex: "",
      intro: "",
    };
    // 总结:
    // 特别注意: v-model, 在input[checkbox]的多选框状态
    // 变量为非数组, 则绑定的是checked的属性(true/false) - 常用于: 单个绑定使用
    // 变量为数组, 则绑定的是他们的value属性里的值 - 常用于: 收集勾选了哪些值
  } 
};
</script>
```

> 总结: 本阶段v-model只能用在表单元素上, 以后学组件后讲v-model高级用法

### 3.9_vue指令 v-model修饰符

> 目标: 让v-model拥有更强大的功能

* 语法:
  * v-model.修饰符="vue数据变量"
    * .number   以parseFloat转成数字类型
    * .trim          去除首尾空白字符
    * .lazy           在change时触发而非inupt时

```vue
<template>
  <div>
    <div>
      <span>年龄:</span>
      <input type="text" v-model.number="age">
    </div>
    <div>
      <span>人生格言:</span>
      <input type="text" v-model.trim="motto">
    </div>
    <div>
      <span>自我介绍:</span>
      <textarea v-model.lazy="intro"></textarea>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      age: "",
      motto: "",
      intro: ""
    }
  }
}
</script>
```

> 总结: v-model修饰符, 可以对值进行预处理, 非常高效好用

### 3.10_vue指令 v-text和v-html

> 目的: 更新DOM对象的innerText/innerHTML

* 语法:
  * v-text="vue数据变量"    
  * v-html="vue数据变量"
* 注意: 会覆盖插值表达式

```vue
<template>
  <div>
    <p v-text="str"></p>
    <p v-html="str"></p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      str: "<span>我是一个span标签</span>"
    }
  }
}
</script>
```

> 总结: v-text把值当成普通字符串显示, v-html把值当做html解析

### 3.11_vue指令 v-show和v-if

> 目标: 控制标签的隐藏或出现

* 语法:
  * v-show="vue变量"            
  * v-if="vue变量" 
* 原理
  * v-show 用的display:none隐藏   (频繁切换使用)
  * v-if  直接从DOM树上移除
* 高级
  * v-else使用

```html
<template>
  <div>
    <h1 v-show="isOk">v-show的盒子</h1>
    <h1 v-if="isOk">v-if的盒子</h1>

    <div>
      <p v-if="age > 18">我成年了</p>
      <p v-else>还得多吃饭</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isOk: true,
      age: 15
    }
  }
}
</script>
```

> 总结: 使用v-show和v-if以及v-else指令, 方便通过变量控制一套标签出现/隐藏

### 3.12_案例-折叠面板

> 目标: 点击展开或收起时，把内容区域显示或者隐藏

![案例_折叠面板](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E6%A1%88%E4%BE%8B_%E6%8A%98%E5%8F%A0%E9%9D%A2%E6%9D%BF.gif)

此案例使用了less语法, 项目中下载模块

```bash
yarn add less@3.0.4 less-loader@5.0.0 -D
```

只有标签和样式

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" >
          收起
        </span>
      </div>
      <div class="container">
        <p>寒雨连江夜入吴,</p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      
    }
  }
}
</script>

<style lang="less">
body {
  background-color: #ccc;
  #app {
    width: 400px;
    margin: 20px auto;
    background-color: #fff;
    border: 4px solid blueviolet;
    border-radius: 1em;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
    padding: 1em 2em 2em;
    h3 {
      text-align: center;
    }
    .title {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .title h4 {
      line-height: 2;
      margin: 0;
    }
    .container {
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .btn {
      /* 鼠标改成手的形状 */
      cursor: pointer;
    }
  }
}
</style>
```

正确答案:

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="isShow = !isShow">
          {{ isShow ? '收起' : '展开' }}
        </span>
      </div>
      <div class="container" v-show="isShow">
        <p>寒雨连江夜入吴, </p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false
    }
  }
}
</script>

```

### 3.13_vue指令-v-for

> 目标: 列表渲染, 所在标签结构, 按照数据数量, 循环生成

* 语法

  * v-for="(值, 索引) in 目标结构"
  * v-for="值 in 目标结构"

* 目标结构:

  * 可以遍历数组 / 对象 / 数字 / 字符串 (可遍历结构)

* 注意:

  v-for的临时变量名不能用到v-for范围外

```vue
<template>
  <div id="app">
    <div id="app">
      <!-- v-for 把一组数据, 渲染成一组DOM -->
      <!-- 口诀: 让谁循环生成, v-for就写谁身上 -->
      <p>学生姓名</p>
      <ul>
        <li v-for="(item, index) in arr" :key="index">
          {{ index }} - {{ item }}
        </li>
      </ul>

      <p>学生详细信息</p>
      <ul>
        <li v-for="obj in stuArr" :key="obj.id">
          <span>{{ obj.name }}</span>
          <span>{{ obj.sex }}</span>
          <span>{{ obj.hobby }}</span>
        </li>
      </ul>

      <!-- v-for遍历对象(了解) -->
      <p>老师信息</p>
      <div v-for="(value, key) in tObj" :key="value">
        {{ key }} -- {{ value }}
      </div>

      <!-- v-for遍历整数(了解) - 从1开始 -->
      <p>序号</p>
      <div v-for="i in count" :key="i">{{ i }}</div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: ["小明", "小欢欢", "大黄"],
      stuArr: [
        {
          id: 1001,
          name: "孙悟空",
          sex: "男",
          hobby: "吃桃子",
        },
        {
          id: 1002,
          name: "猪八戒",
          sex: "男",
          hobby: "背媳妇",
        },
      ],
      tObj: {
        name: "小黑",
        age: 18,
        class: "1期",
      },
      count: 10,
    };
  },
};
</script>
```

> 总结: vue最常用指令, 铺设页面利器, 快速把数据赋予到相同的dom结构上循环生成

## 今日总结

- [x] Vue是什么
- [x] Vue-cli作用以及简单使用

  ```vue
  yarn global add @vue/cli
  # vue和create是命令, vuecli-demo是文件夹名
  vue create vuecli-demo
  cd vuecil-demo
  
  # 运行serve
  yarn serve
  ```
- [x] 插值表达式
- [x] MVVM设计模式
- [x] v-bind作用
- [x] v-on作用和事件对象以及修饰符使用
- [x] v-model的作用以及双向数据绑定解释
- [x] v-if和v-show的区别和本质
- [x] v-for的作用和使用
- [ ] vue的特点
  * 渐进式
  * 声明式渲染
  * 数据驱动视图 (响应式)
  * 极少的去写DOM操作相关代码
  * 双向绑定
  * 组件系统
  * 不兼容IE8及以下浏览器

## 面试题

### 1. Vue的最大优势是什么?

​	简单易学, 轻量级整个源码js文件不大, 双向数据绑定, 数据驱动视图, 组件化, 数据和视图分离, 

​	vue负责关联视图和数据, 作者中国人(尤雨溪), 文档都是中文的, 入门教程非常多, 上手简单. 

​	相比传统网页, vue是单页面可以只刷新某一部分

### 2. Vue和jQuery区别是什么?

​	jQuery应该算是一个插件, 里面封装了各种易用的方法, 方便你使用更少的代码来操作dom标签

​	Vue是一套框架, 有自己的规则和体系与语法, 特别是设计思想MVVM, 让数据和视频关联绑定, 省略了很多DOM操作. 然后指令还给标签注入了更多的功能

### 3. mvvm和mvc区别是什么?

​	MVC: 也是一种设计模式, 组织代码的结构, 是model数据模型, view视图, Controller控制器, 在控制器这层里编写js代码, 来控制数据和视图关联

​	MVVM: 即Model-View-ViewModel的简写。即模型-视图-视图模型, VM是这个设计模式的核心, 连接v和m的桥梁, 内部会监听DOM事件, 监听数据对象变化来影响对方. 我们称之为数据绑定

### 4. Vue常用修饰符有哪些?

​    .prevent: 提交事件不再重载页面；

​	.stop: 阻止单击事件冒泡；

​	.once: 只执行一次这个事件

### 5. Vue2.x兼容IE哪个版本以上

​	不支持ie8及以下，部分兼容ie9 ，完全兼容10以上， 因为vue的响应式原理是基于es5的Object.defineProperty(),而这个方法不支持ie8及以下。

### 6. 对Vue渐进式的理解

​	渐进式代表的含义是：主张最少, 自底向上, 增量开发, 组件集合, 便于复用

### 7. v-show和v-if的区别

​	v-show和v-if的区别? 分别说明其使用场景?

​	v-show 和v-if都是true的时候显示，false的时候隐藏

​	但是：false的情况下，

​	v-show是采用的display:none   

​	v-if采用惰性加载

​	如果需要频繁切换显示隐藏需要使用v-show

### 8. 说出至少4个Vue指令及作用

​	v-for 根据数组的个数, 循环数组元素的同时还生成所在的标签

​	v-show 显示内容

​	v-if    显示与隐藏  

​	v-else  必须和v-if连用  不能单独使用  否则报错  

​	v-bind  动态绑定  作用： 及时对页面的数据进行更改, 可以简写成:分号

​	v-on  给标签绑定函数，可以缩写为@，例如绑定一个点击函数  函数必须写在methods里面

​	v-text  解析文本

​	v-html   解析html标签

### 9. 为什么避免v-for和v-if在一起使用

​	Vue 处理指令时，v-for 比 v-if 具有更高的优先级, 虽然用起来也没报错好使, 但是性能不高, 如果你有5个元素被v-for循环, v-if也会分别执行5次.

## 附加练习-1.帅哥美女走一走

> 目标: 点击按钮, 改变3个li的顺序, 在头上的就到末尾.

> 提示: 操作数组里的顺序, v-for就会重新渲染li

![2.8.1_练习_帅哥美女走一走](F:\前端\05、阶段五 Vue.js项目实战开发\笔记\Day02_vue脚手架_基础API\01_笔记和ppt\images\2.8.1_练习_帅哥美女走一走.gif)

正确代码(==先不要看==)

```html
<template>
  <div id="app">
    <ul>
      <li v-for="item in myArr" :key="item">{{ item }}</li>
    </ul>
    <button @click="btn">走一走</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      myArr: ["帅哥", "美女", "程序猿"],
    };
  },
  methods: {
    btn() {
      // 头部数据加入到末尾
      this.myArr.push(this.myArr[0]);
      // 再把头部的数据删除掉
      this.myArr.shift();
    },
  },
};
</script>
```

## 附加练习-2.加加减减

> 目标: 点击生成按钮, 新增一个li(随机数字)和删除按钮, 点击删除按钮, 删除对应的li和值

> 提示: 数组渲染列表, 生成和删除都围绕数组操作

![2.8.2_练习_人生加加减减](https://gitee.com/XXXTENTWXD/pic/raw/master/images/2.8.2_%E7%BB%83%E4%B9%A0_%E4%BA%BA%E7%94%9F%E5%8A%A0%E5%8A%A0%E5%87%8F%E5%87%8F.gif)

正确代码:(==先不要看==)

```html
<template>
  <div id="app">
    <ul>
      <li v-for="(item, ind) in arr" :key="item">
        <span>{{ item }}</span>
        <button @click="del(ind)">删除</button>
      </li>
    </ul>
    <button @click="add">生成</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [1, 5, 3],
    };
  },
  methods: {
    add() {
      this.arr.push(Math.floor(Math.random() * 20));
    },
    del(index) {
      this.arr.splice(index, 1);
    },
  },
};
</script>
```

## 附加练习-3.购物车

> 目标: 完成商品浏览和删除功能, 当无数据给用户提示

* 需求1: 根据给的初始数据, 把购物车页面铺设出来
* 需求2: 点击对应删除按钮, 删除对应数据
* 需求3: 当数据没有了, 显示一条提示消息

![3.0_案例_购物车删除_干净了还有提示](https://gitee.com/XXXTENTWXD/pic/raw/master/images/3.0_%E6%A1%88%E4%BE%8B_%E8%B4%AD%E7%89%A9%E8%BD%A6%E5%88%A0%E9%99%A4_%E5%B9%B2%E5%87%80%E4%BA%86%E8%BF%98%E6%9C%89%E6%8F%90%E7%A4%BA.gif)

html+css和数据代码结构(==可复制接着写==)

```vue
<template>
  <div id="app">
    <table class="tb">
      <tr>
        <th>编号</th>
        <th>品牌名称</th>
        <th>创立时间</th>
        <th>操作</th>
      </tr>
      <!-- 循环渲染的元素tr -->
      <tr>
        <td>1</td>
        <td>车名</td>
        <td>2020-08-09</td>
        <td>
          <button>删除</button>
        </td>
      </tr>
      <tr>
        <td colspan="4">没有数据咯~</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        { id: 1, name: "奔驰", time: "2020-08-01" },
        { id: 2, name: "宝马", time: "2020-08-02" },
        { id: 3, name: "奥迪", time: "2020-08-03" },
      ],
    };
  },
};
</script>

<style>
#app {
  width: 600px;
  margin: 10px auto;
}

.tb {
  border-collapse: collapse;
  width: 100%;
}

.tb th {
  background-color: #0094ff;
  color: white;
}

.tb td,
.tb th {
  padding: 5px;
  border: 1px solid black;
  text-align: center;
}

.add {
  padding: 5px;
  border: 1px solid black;
  margin-bottom: 10px;
}
</style>

```

正确代码(==先不要看==)

```vue
<template>
  <div id="app">
    <table class="tb">
      <tr>
        <th>编号</th>
        <th>品牌名称</th>
        <th>创立时间</th>
        <th>操作</th>
      </tr>
      <!-- 循环渲染的元素tr -->
      <tr v-for="(item,index) in list" :key="item.id">
            <td>{{item.id}}</td>
            <td>{{item.name}}</td>
            <td>{{item.time}}</td>
            <td>
                <button @click="del(index)">删除</button>
            </td>
        </tr>
      <tr v-if="list.length === 0">
        <td colspan="4">没有数据咯~</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      list: [
        { id: 1, name: "奔驰", time: "2020-08-01" },
        { id: 2, name: "宝马", time: "2020-08-02" },
        { id: 3, name: "奥迪", time: "2020-08-03" },
      ],
    };
  },
  methods: {
    del(index) {
      // 删除按钮 - 得到索引, 删除数组里元素
      this.list.splice(index, 1);
    },
  },
};
</script>

```

## 今日作业

课上案例先来一遍

### 作业1-逛水果店

从0开始新建一个vuecli脚手架项目

本店收银系统采用vue开发, 冲这点, 你不来买点试试?

先看效果 - 无css(你想美化下, 你就写点哈哈)

> 提示: v-model="变量" 输入框的值会绑定给vue的这个变量(别忘了在data里先声明哦)

![Day01_作业_买水果](https://gitee.com/XXXTENTWXD/pic/raw/master/images/Day01_%E4%BD%9C%E4%B8%9A_%E4%B9%B0%E6%B0%B4%E6%9E%9C.gif)

只要你实现了功能 你就是对的 (只不过每个程序员的想法都不太一样)

### 作业2-选择喜欢的

目标: 用户选择栏目, 把用户选中的栏目信息在下面列表显示出来

> 提示: vue变量是数组类型, 绑定在checkbox标签上

```js
// 数据在这里
["科幻", "喜剧", "动漫", "冒险", "科技", "军事", "娱乐", "奇闻"]
```

![4.9.1_练习_选择喜欢的栏目](https://gitee.com/XXXTENTWXD/pic/raw/master/images/4.9.1_%E7%BB%83%E4%B9%A0_%E9%80%89%E6%8B%A9%E5%96%9C%E6%AC%A2%E7%9A%84%E6%A0%8F%E7%9B%AE.gif)



# 第四章 基础API、计算属性、过滤器、侦听器

## 知识点自测

- [ ] 会自己定义数据结构

```js
"红色","red", "蓝色","blue", 
// 上面的数据结构, 要用1个变量来装这4个值, 用什么数据结构呢?(数组还是对象) - 对象(可映射key->value)
    
"小明", "小蓝", "小赵"
// 上面的结构用数组比较合适
```

- [ ] 马上能反应过来循环遍历是什么, 索引(下角标)是什么

```js
let arr = [10, 32, 99];
// 索引就是数字, 标记每个值对应的序号, 从0开始
// 索引是0, 1, 2
// 数组需要用索引来换取值, 固定格式 arr[索引]
// 遍历就是挨个取出来
```

- [ ] 数组的filter方法使用

```js
let arr = [19, 29, 27, 20, 31, 32, 35];
let newArr = arr.filter((val) => {return val >= 30})
// 数组调用.filter()方法 - 传入一个函数体 (固定格式)
// 运行过程: filter会遍历数组里的每一项, 对每一项执行一次函数体(会把每个值传给形参)
// 作用: 每次遍历如果val值符合return的条件, 就会被filter收集起来
// 返回值: 当filter遍历结束以后, 返回收集到的符合条件的那些值形成的新数组
console.log(newArr);
```

- [ ] 重绘与回流(重排)的概念

```bash
回流(重排): 当浏览器必须重新处理和绘制部分或全部页面时，回流就会发生

重绘: 不影响布局, 只是标签页面发生变化, 重新绘制

注意: 回流(重排)必引发重绘, 重绘不一定引发回流(重排)
```

- [ ] localStorage浏览器本地存储语法使用

```bash
localStorage.setItem("key名", 值) - 把值存在浏览器本地叫key的对应位置上

localStorage.getItem("key名") - 把叫key的对应值, 从浏览器本地取出来

==值只能是字符串类型, 如果不是请用JSON.stringify转, 取出后用JSON.parse转==
```

- [ ] JSON的方法使用

```bash
JSON.stringify(JS数据) - 把JS数据序列化成JSON格式字符串

JSON.parse(JSON字符串)  - 把JSON格式化字符串, 再转回成JS数据
```

## 今日学习目标

1. 能够了解key作用, 虚拟DOM, diff算法
2. 能够掌握设置动态样式
3. 能够掌握过滤器, 计算属性, 侦听器
4. 能够完成品牌管理案例

## 1. vue基础

### 1.0_vue基础 v-for更新监测

> 目标: 当v-for遍历的目标结构改变, Vue触发v-for的更新

情况1: 数组翻转

情况2: 数组截取

情况3: 更新值

口诀:

数组变更方法, 就会导致v-for更新, 页面更新

数组非变更方法, 返回新数组, 就不会导致v-for更新, 可采用覆盖数组或this.$set()

```vue
<template>
  <div>
    <ul>
      <li v-for="(val, index) in arr" :key="index">
        {{ val }}
      </li>
    </ul>
    <button @click="revBtn">数组翻转</button>
    <button @click="sliceBtn">截取前3个</button>
    <button @click="updateBtn">更新第一个元素值</button>
  </div>
</template>

<script>
export default {
  data(){
    return {
      arr: [5, 3, 9, 2, 1]
    }
  },
  methods: {
    revBtn(){
      // 1. 数组翻转可以让v-for更新
      this.arr.reverse()
    },
    sliceBtn(){
      // 2. 数组slice方法不会造成v-for更新
      // slice不会改变原始数组
      // this.arr.slice(0, 3)

      // 解决v-for更新 - 覆盖原始数组
      let newArr = this.arr.slice(0, 3)
      this.arr = newArr
    },
    updateBtn(){
      // 3. 更新某个值的时候, v-for是监测不到的
      // this.arr[0] = 1000;

      // 解决-this.$set()
      // 参数1: 更新目标结构
      // 参数2: 更新位置
      // 参数3: 更新值
      this.$set(this.arr, 0, 1000)
    }
  }
}
</script>

<style>

</style>
```

这些方法会触发数组改变, v-for会监测到并更新页面

- `push()`
- `pop()`
- `shift()`
- `unshift()`
- `splice()`
- `sort()`
- `reverse()`

这些方法不会触发v-for更新

* `slice()`
* `filter()`
* `concat()` 

> 注意: vue不能监测到数组里赋值的动作而更新, 如果需要请使用Vue.set() 或者this.$set(), 或者覆盖整个数组

> 总结:  改变原数组的方法才能让v-for更新

### 1.1_vue基础_v-for就地更新

`v-for` 的默认行为会尝试原地修改元素而不是移动它们。

> 详解v-for就地更新流程(可以看ppt动画)

![image-20220913192439641](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192439641.png)

这种 虚拟DOM对比方式, 可以提高性能 - 但是还不够高

### 1.2_vue基础_虚拟dom

> 目标: 了解虚拟DOM的概念

.vue文件中的template里写的标签, 都是模板, 都要被vue处理成虚拟DOM对象, 才会渲染显示到真实DOM页面上

1. 内存中生成一样的虚拟DOM结构(==本质是个JS对象==)

   因为真实的DOM属性好几百个, 没办法快速的知道哪个属性改变了

   比如template里标签结构

   ```vue
   <template>
       <div id="box">
           <p class="my_p">123</p>
       </div>
   </template>
   ```

   对应的虚拟DOM结构

   ```js
   const dom = {
       type: 'div',
       attributes: [{id: 'box'}],
       children: {
           type: 'p',
           attributes: [{class: 'my_p'}],
           text: '123'
       }
   }
   ```

2. 以后vue数据更新

   * 生成新的虚拟DOM结构
   * 和旧的虚拟DOM结构对比
   * 利用diff算法, 找不不同, 只更新变化的部分(重绘/回流)到页面 - 也叫打补丁

==好处1: 提高了更新DOM的性能(不用把页面全删除重新渲染)==

==好处2: 虚拟DOM只包含必要的属性(没有真实DOM上百个属性)==

> 总结: 虚拟DOM保存在内存中, 只记录dom关键信息, 配合diff算法提高DOM更新的性能

在内存中比较差异, 然后给真实DOM打补丁更新上

![image-20220913192456671](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192456671.png)

### 1.3_vue基础_diff算法

vue用diff算法, 新虚拟dom, 和旧的虚拟dom比较

#### 情况1: 根元素变了, 删除重建 

旧虚拟DOM

```vue
<div id="box">
    <p class="my_p">123</p>
</div>
```

新虚拟DOM

```vue
<ul id="box">
    <li class="my_p">123</li>
</ul>
```

#### 情况2: 根元素没变, 属性改变, ==元素复用==, 更新属性

旧虚拟DOM

```vue
<div id="box">
    <p class="my_p">123</p>
</div>
```

新虚拟DOM

```vue
<div id="myBox" title="标题">
    <p class="my_p">123</p>
</div>
```

### 1.4_vue基础_diff算法-key

#### 情况3: 根元素没变, 子元素没变, 元素内容改变

##### 无key - 就地更新

v-for不会移动DOM, 而是尝试复用, 就地更新，如果需要v-for移动DOM, 你需要用特殊 attribute `key` 来提供一个排序提示

```vue
<ul id="myUL">
    <li v-for="str in arr">
        {{ str }} 
        <input type="text">
    </li>
</ul>
<button @click="addFn">下标为1的位置新增一个</button>
```

```js
export default {
    data(){
        return {
            arr: ["老大", "新来的", "老二", "老三"]
        }
    },
    methods: {
        addFn(){
            this.arr.splice(1, 0, '新来的')
        }
    }
};
```

![新_vfor更细_无key_就地更新](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E6%96%B0_vfor%E6%9B%B4%E7%BB%86_%E6%97%A0key_%E5%B0%B1%E5%9C%B0%E6%9B%B4%E6%96%B0.gif)

旧 - 虚拟DOM结构  和  新 - 虚拟DOM结构 对比过程

![image-20220913192558712](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192558712.png)

==性能不高, 从第二个li往后都更新了==

##### 有key - 值为索引 

 - 还是就地更新

因为新旧虚拟DOM对比, key存在就复用此标签更新内容, 如果不存在就直接建立一个新的

```vue
<ul id="myUL">
    <li v-for="(str, index) in arr" :key="index">
        {{ str }} 
        <input type="text">
    </li>
</ul>
<button @click="addFn">下标为1的位置新增一个</button>
```

```js
export default {
    data(){
        return {
            arr: ["老大", "新来的", "老二", "老三"]
        }
    },
    methods: {
        addFn(){
            this.arr.splice(1, 0, '新来的')
        }
    }
};
```



key为索引-图解过程 (又就地往后更新了)

![新_vfor更细_无key_就地更新](F:\前端\05、阶段五 Vue.js项目实战开发\笔记\Day03_基础API_计算属性_过滤器_侦听器_品牌管理案例\01_笔记和ppt\images\新_vfor更细_无key_就地更新.gif)

![image-20220913192640071](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192640071.png)



1. v-for先循环产生新的DOM结构, key是连续的, 和数据对应

2. 然后比较新旧DOM结构, 找到区别, 打补丁到页面上

   最后补一个li, 然后从第二个往后, 都要更新内容

> 口诀: key的值有id用id, 没id用索引

##### 有key - 值为id 

key的值只能是唯一不重复的, 字符串或数值

v-for不会移动DOM, 而是尝试复用, 就地更新，如果需要v-for移动DOM, 你需要用特殊 attribute `key` 来提供一个排序提示

新DOM里数据的key存在, 去旧的虚拟DOM结构里找到key标记的标签, 复用标签

新DOM里数据的key存在, 去旧的虚拟DOM结构里没有找到key标签的标签, 创建

旧DOM结构的key, 在新的DOM结构里没有了, 则==移除key所在的标签==

```vue
<template>
  <div>
    <ul>
      <li v-for="obj in arr" :key="obj.id">
        {{ obj.name }}
        <input type="text">
      </li>
    </ul>
    <button @click="btn">下标1位置插入新来的</button>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: '老大',
          id: 50
        },
        {
          name: '老二',
          id: 31
        },
        {
          name: '老三',
          id: 10
        }
      ],
    };
  },
  methods: {
    btn(){
      this.arr.splice(1, 0, {
        id: 19, 
        name: '新来的'
      })
    }
  }
};
</script>

<style>
</style>
```

图解效果:

![新_vfor更细_有key值为id_提高性能更新](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E6%96%B0_vfor%E6%9B%B4%E7%BB%86_%E6%9C%89key%E5%80%BC%E4%B8%BAid_%E6%8F%90%E9%AB%98%E6%80%A7%E8%83%BD%E6%9B%B4%E6%96%B0.gif)

![image-20220913192716915](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192716915.png)

> 总结: 不用key也不影响功能(就地更新), 添加key可以提高更新的性能

### 1.5_阶段小结

v-for什么时候会更新页面呢?

* 数组采用更新方法, 才导致v-for更新页面

vue是如何提高更新性能的?

* 采用虚拟DOM+diff算法提高更新性能

虚拟DOM是什么?

* 本质是保存dom关键信息的JS对象

diff算法如何比较新旧虚拟DOM?

* 根元素改变 – 删除当前DOM树重新建
* 根元素未变, 属性改变 – 更新属性
* 根元素未变, 子元素/内容改变
* 无key – 就地更新 / 有key – 按key比较

### 1.6_vue基础 动态class

> 目标: 用v-bind给标签class设置动态的值

* 语法:
  * :class="{类名: 布尔值}"

```vue
<template>
  <div>
    <!-- 语法:
      :class="{类名: 布尔值}"
      使用场景: vue变量控制标签是否应该有类名
     -->
    <p :class="{red_str: bool}">动态class</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      bool: true
    }
  }
}
</script>

<style scoped>
  .red_str{
    color: red;
  }
</style>
```

> 总结: 就是把类名保存在vue变量中赋予给标签

### 1.7_vue基础-动态style

> 目标: 给标签动态设置style的值

* 语法
  * :style="{css属性: 值}"

```vue
<template>
  <div>
    <!-- 动态style语法
      :style="{css属性名: 值}"
     -->
    <p :style="{backgroundColor: colorStr}">动态style</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      colorStr: 'red'
    }
  }
}
</script>

<style>

</style>
```

> 总结: 动态style的key都是css属性名

### 1.8_案例-品牌管理(铺)

> 目标: 数据铺设

* 需求1: 把默认数据显示到表格上
* 需求2: 注意资产超过100的, 都用红色字体标记出来

细节:

​	① 先铺设静态页面 --- 去.md文档里, 复制数据和标签模板

​	② 此案例使用bootstrap, 需要下载, 并导入到工程main.js中

​	③ 用v-for配合默认数据, 把数据默认铺设到表格上显示

​	④ 直接在标签上, 大于100价格, 动态设置red类名

图示:

![image-20220913192730007](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192730007.png)

1. 因为案例使用了bootstrap, 工程化开发, 模块化用npm/yarn下载引入使用

```bash
yarn add bootstrap
```

2. 在main.js - 引入bootstrap

```js
import "bootstrap/dist/css/bootstrap.css" // 默认找文件夹下的index文件(但是这个不是所以需要写路径)
```

3. 模板代码(在这个基础上写)

```vue
<template>
  <div id="app">
    <div class="container">
      <!-- 顶部框模块 -->
      <div class="form-group">
        <div class="input-group">
          <h4>品牌管理</h4>
        </div>
      </div>

      <!-- 数据表格 -->
      <table class="table table-bordered table-hover mt-2">
        <thead>
          <tr>
            <th>编号</th>
            <th>资产名称</th>
            <th>价格</th>
            <th>创建时间</th>
            <th>操作</th>
          </tr>
        </thead>
        <tbody>
          <tr >
            <td></td>
            <td></td>

            <!-- 如果价格超过100，就有red这个类 -->
            <td class="red"></td>
            <td></td>
            <td><a href="#" >删除</a></td>
          </tr>
        </tbody>
          <!-- 
        <tfoot >
          <tr>
            <td colspan="5" style="text-align: center">暂无数据</td>
          </tr>
        </tfoot>
            -->
      </table>

      <!-- 添加资产 -->
      <form class="form-inline">
        <div class="form-group">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="资产名称"
            />
          </div>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <div class="form-group">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="价格"
            />
          </div>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <!-- 阻止表单提交 -->
        <button class="btn btn-primary">添加资产</button>
      </form>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      name: "", // 名称
      price: 0, // 价格
      list: [
        { id: 100, name: "外套", price: 199, time: new Date('2010-08-12')},
        { id: 101, name: "裤子", price: 34, time: new Date('2013-09-01') },
        { id: 102, name: "鞋", price: 25.4, time: new Date('2018-11-22') },
        { id: 103, name: "头发", price: 19900, time: new Date('2020-12-12') }
      ],
    };
  },

};
</script>

<style >
.red{
  color: red;
}
</style>
```

==正确代码, 不可复制==

```vue
<tbody>
    <tr v-for="obj in list" :key="obj.id">
        <td>{{ obj.id }}</td>
        <td>{{ obj.name }}</td>

        <!-- 如果价格超过100，就有red这个类 -->
        <td :class="{red: obj.price > 100}">{{ obj.price }}</td>
        <td>{{ obj.time }}</td>
        <td><a href="#" >删除</a></td>
    </tr>
</tbody>

<script>
// 1. 明确需求
// 2. 标签+样式+默认数据
// 3. 下载bootstrap, main.js引入bootstrap.css
// 4. 把list数组 - 铺设表格
// 5. 修改价格颜色
</script>
```

### 1.9_案例-品牌管理(增)

> 目标: 数据新增

* 需求1: 实现表单数据新增进表格功能

* 需求2: 判断用户输入是否为空给提示

* 分析

  ① 添加资产按钮 – 绑定点击事件

  ② 给表单v-model绑定vue变量收集用户输入内容

  ③ 添加数组到数组中

  ④ 判断用户内容是否符合规定

图示:

![4.10_案例_资产列表_新增](https://gitee.com/XXXTENTWXD/pic/raw/master/images/4.10_%E6%A1%88%E4%BE%8B_%E8%B5%84%E4%BA%A7%E5%88%97%E8%A1%A8_%E6%96%B0%E5%A2%9E.gif)

在上个案例代码基础上接着写

==正确代码,不可复制==

```vue
<!-- 添加资产 -->
      <form class="form-inline">
        <div class="form-group">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="资产名称"
              v-model="name"
            />
          </div>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <div class="form-group">
          <div class="input-group">
            <input
              type="text"
              class="form-control"
              placeholder="价格"
              v-model.number="price"
            />
          </div>
        </div>
        &nbsp;&nbsp;&nbsp;&nbsp;
        <!-- 4. 阻止表单提交(刷新网页数据又回去了) -->
        <button class="btn btn-primary" @click.prevent="addFn">添加资产</button>
      </form>

<script>
// 目标: 新增
// 1. 按钮 - 事件
// 2. 给表单v-model绑定vue变量
export default {
  // ...省略其他
  methods: {
    addFn(){
      // 5. 判断是否为空
      if (this.name.trim().length === 0 || this.price === 0) {
        alert("不能为空")
        return
      }
      // 3. 把值以对象形式-插入list
      this.list.push({
        // 当前数组最后一个对象的id+1作为新对象id值
        id: this.list[this.list.length - 1].id + 1,
        name: this.name,
        price: this.price,
        time: new Date()
      })
    }
  }
};
</script>
```

### 1.10_案例-品牌管理(删)

> 目标: 数据删除

* 需求1: 点击删除的a标签, 删除数据

* 需求2: 删除没数据了要提示暂无数据的tfoot

* 分析

  ① a标签绑定点击事件

  ② 给事件方法传id

  ③ 通过id, 找到对应数据删除

  ④ 删除光了要让tfoot显示

  ⑤ 删除光了再新增, 有bug(id值问题)需要修复

图示:

![4.10_案例_资产列表_删除](https://gitee.com/XXXTENTWXD/pic/raw/master/images/4.10_%E6%A1%88%E4%BE%8B_%E8%B5%84%E4%BA%A7%E5%88%97%E8%A1%A8_%E5%88%A0%E9%99%A4.gif)

在上个案例代码基础上接着写

正确的代码(==不可复制==)

```vue
<td><a href="#" @click="delFn(obj.id)">删除</a></td>
          
<script>
// 目标: 删除功能
// 1. 删除a标签-点击事件
// 2. 对应方法名
// 3. 数据id到事件方法中
// 4. 通过id, 找到这条数据在数组中的下标
// 5. splice方法删除原数组里的对应元素
// 6. 设置tfoot, 无数据给出提示
// 7. 无数据再新增, id要判断一下
export default {
  // ...其他代码
  methods: {
    // ...其他代码
    delFn(id){
      // 通过id找到这条数据在数组中下标
      let index = this.list.findIndex(obj => obj.id === id)
      this.list.splice(index, 1)
    }
  }
};
</script>
```

## 2. vue过滤器

### 2.0_vue过滤器-定义使用

> 目的: 转换格式, 过滤器就是一个**函数**, 传入值返回处理后的值

过滤器只能用在, ==插值表达式和v-bind表达式==

Vue中的过滤器场景

* 字母转大写, 输入"hello", 输出"HELLO"
* 字符串翻转, "输入hello, world", 输出"dlrow ,olleh"

语法: 

* Vue.filter("过滤器名", (值) => {return "返回处理后的值"})

* filters: {过滤器名字: (值) => {return "返回处理后的值"}

例子:

* 全局定义字母都大写的过滤器
* 局部定义字符串翻转的过滤器

```vue
<template>
  <div>
    <p>原来的样子: {{ msg }}</p>
    <!-- 2. 过滤器使用
      语法: {{ 值 | 过滤器名字 }}
     -->
    <p>使用翻转过滤器: {{ msg | reverse }}</p>
    <p :title="msg | toUp">鼠标长停</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      msg: 'Hello, Vue'
    }
  },
  // 方式2: 局部 - 过滤器
  // 只能在当前vue文件内使用
  /*
     语法: 
     filters: {
       过滤器名字 (val) {
         return 处理后的值
       }
     }
  */
  filters: {
    toUp (val) {
      return val.toUpperCase()
    }
  }
}
</script>

<style>

</style>
```

> 总结: 把值转成另一种形式, 使用过滤器, Vue3用函数替代了过滤器.
>
> 全局注册最好在main.js中注册, 一处注册到处使用

### 2.1_vue过滤器-传参和多过滤器

> 目标: 可同时使用多个过滤器, 或者给过滤器传参

* 语法:
  * 过滤器传参:   vue变量 | 过滤器(实参) 
  * 多个过滤器:   vue变量 | 过滤器1 | 过滤器2

```vue
<template>
  <div>
    <p>原来的样子: {{ msg }}</p>
    <!-- 1.
      给过滤器传值
      语法: vue变量 | 过滤器名(值)
     -->
    <p>使用翻转过滤器: {{ msg | reverse('|') }}</p>
    <!-- 2.
      多个过滤利使用
      语法: vue变量 | 过滤器1 | 过滤器2
     -->
    <p :title="msg | toUp | reverse('|')">鼠标长停</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      msg: 'Hello, Vue'
    }
  },
  filters: {
    toUp (val) {
      return val.toUpperCase()
    }
  }
}
</script>

<style>

</style>
```

> 总结: 过滤器可以传参, 还可以对某个过滤器结果, 后面在使用一个过滤器

### 2.2_案例-品牌管理(时间格式化)

> 目标: 复制上个案例, 在此基础上, 把表格里的时间用过滤器+moment模块, 格式化成YYYY-MM-DD 格式

图示: 

![image-20210215155844500](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210215155844500.png)

1. 下载moment处理日期的第三方工具模块

   moment官网文档: http://momentjs.cn/docs/#/displaying/

   ```bash
   yarn add moment
   ```

2. 定义过滤器, 把时间用moment模块格式化, 返回我们想要的格式

   ```js
   // 目标: 处理时间
   // 1. 下载moment模块
   import moment from 'moment'
   
   
   // 2. 定义过滤器, 编写内部代码
   filters: { 
       formatDate (val){
           return moment(val).format('YYYY-MM-DD')
       }
   }
   
   <!-- 3. 使用过滤器 -->
   <td>{{ obj.time | formatDate }}</td>
   ```

## 3. vue计算属性

### 3.0_vue计算属性-computed

> 目标: 一个数据, 依赖另外一些数据计算而来的结果

语法:

* ```js
  computed: {
      "计算属性名" () {
          return "值"
      }
  }
  ```

需求: 

* 需求: 求2个数的和显示到页面上

```vue
<template>
  <div>
    <p>{{ num }}</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      a: 10,
      b: 20
    }
  },
  // 计算属性:
  // 场景: 一个变量的值, 需要用另外变量计算而得来
  /*
    语法:
    computed: {
      计算属性名 () {
        return 值
      }
    }
  */
 // 注意: 计算属性和data属性都是变量-不能重名
 // 注意2: 函数内变量变化, 会自动重新计算结果返回
  computed: {
    num(){
      return this.a + this.b
    }
  }
}
</script>

<style>

</style>
```

> 注意: 计算属性也是vue数据变量, 所以不要和data里重名, 用法和data相同

> 总结: 一个数据, 依赖另外一些数据计算而来的结果

### 3.1_vue计算属性-缓存

> 目标: 计算属性是基于它们的依赖项的值结果进行缓存的，只要依赖的变量不变, 都直接从缓存取结果

![image-20220913192841363](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192841363.png)

```vue
<template>
  <div>
    <p>{{ reverseMessage }}</p>
    <p>{{ reverseMessage }}</p>
    <p>{{ reverseMessage }}</p>
    <p>{{ getMessage() }}</p>
    <p>{{ getMessage() }}</p>
    <p>{{ getMessage() }}</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      msg: "Hello, Vue"
    }
  },
  // 计算属性优势:
  // 带缓存
  // 计算属性对应函数执行后, 会把return值缓存起来
  // 依赖项不变, 多次调用都是从缓存取值
  // 依赖项值-变化, 函数会"自动"重新执行-并缓存新的值
  computed: {
    reverseMessage(){
      console.log("计算属性执行了");
      return this.msg.split("").reverse().join("")
    }
  },
  methods: {
    getMessage(){
      console.log("函数执行了");
      return this.msg.split("").reverse().join("")
    }
  }
}
</script>

<style>

</style>
```

> 总结: 计算属性根据依赖变量结果缓存, 依赖变化重新计算结果存入缓存, 比普通方法性能更高

### 3.2_案例-品牌管理(总价和均价)

> 目标: 基于之前的案例, 完成总价和均价的计算效果

![image-20220913192854673](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913192854673.png)

此处只修改了变化的代码

```vue
<tr style="background-color: #EEE">
     <td>统计:</td>
     <td colspan="2">总价钱为: {{ allPrice }}</td>
     <td colspan="2">平均价: {{ svgPrice }}</td>
</tr>

<script>
// 目标: 总价和均价显示
// 1. 末尾补tr - 显示总价和均价
export default {
  // ...源代码省略
  // 2. 计算属性
  computed: {
      allPrice(){
          // 3. 求总价
          return this.list.reduce((sum, obj) => sum += obj.price, 0)
      },
      avgPrice(){
          // 4. 求均价 - 保留2位小数
          return (this.allPrice / this.list.length).toFixed(2)
      }
  }
}
</script>
```

> 总结: 总价来源于所有数据计算而来的结果, 故采用计算属性

### 3.3_vue计算属性-完整写法

> 目标: 计算属性也是变量, 如果想要直接赋值, 需要使用完整写法

语法:

```js
computed: {
    "属性名": {
        set(值){
            
        },
        get() {
            return "值"
        }
    }
}
```

需求: 

* 计算属性给v-model使用

页面准备输入框

```vue
<template>
  <div>
      <div>
          <span>姓名:</span>
          <input type="text" v-model="full">
      </div>
  </div>
</template>

<script>
// 问题: 给计算属性赋值 - 需要setter
// 解决:
/*
    完整语法:
    computed: {
        "计算属性名" (){},
        "计算属性名": {
            set(值){

            },
            get(){
                return 值
            }
        }
    }
*/
export default {
    computed: {
        full: {
            // 给full赋值触发set方法
            set(val){
                console.log(val)
            },
            // 使用full的值触发get方法
            get(){
                return "无名氏"
            }
        }
    }
}
</script>

<style>

</style>
```

> 总结: 想要给计算属性赋值, 需要使用set方法

### 3.4_案例-小选影响全选

> 目标: 小选框都选中(手选), 全选自动选中

* 需求: 小选框都选中(手选), 全选自动选中

分析:

① 先静态后动态, 从.md拿到静态标签和数据

② 循环生成复选框和文字, 对象的c属性和小选框的选中状态, 用v-model双向绑定

③ 定义isAll计算属性, 值通过小选框们统计c属性状态得来

图示:

![小选_影响多选](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E5%B0%8F%E9%80%89_%E5%BD%B1%E5%93%8D%E5%A4%9A%E9%80%89.gif)

模板标签和数据(==直接复制在这基础上写vue代码==)

```vue
<template>
  <div>
    <span>全选:</span>
    <input type="checkbox"/>
    <button>反选</button>
    <ul>
      <li>
        <input type="checkbox"/>
        <span>任务名</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false,
        },
        {
          name: "孙悟空",
          c: false,
        },
        {
          name: "唐僧",
          c: false,
        },
        {
          name: "白龙马",
          c: false,
        },
      ],
    };
  }
};
</script>
```

==正确代码,不可复制==

```vue
<template>
  <div>
    <span>全选:</span>
    <!-- 4. v-model 关联全选 - 选中状态 -->
    <input type="checkbox" v-model="isAll"/>
    <button>反选</button>
    <ul>
      <li v-for="(obj, index) in arr" :key="index">
        <!-- 3. 对象.c - 关联 选中状态 -->
        <input type="checkbox" v-model="obj.c"/>
        <span>{{ obj.name }}</span>
      </li>
    </ul>
  </div>
</template>

<script>
// 目标: 小选框 -> 全选
// 1. 标签+样式+js准备好
// 2. 把数据循环展示到页面上
export default {
  data() {
    return {
      arr: [
        {
          name: "猪八戒",
          c: false,
        },
        {
          name: "孙悟空",
          c: false,
        },
        {
          name: "唐僧",
          c: false,
        },
        {
          name: "白龙马",
          c: false,
        },
      ],
    };
  },
  // 5. 计算属性-isAll
  computed: {
    isAll () {
         // 6. 统计小选框状态 ->  全选状态
        // every口诀: 查找数组里"不符合"条件, 直接原地返回false
        return this.arr.every(obj => obj.c === true)
    }
  }
};
</script>
```

### 3.5_案例-全选影响小选

> 目标: 全选影响小选

* 需求1: 获取到全选状态 – 改装isAll计算属性
* 需求2: 全选状态同步给所有小选框

分析:

①: isAll改成完整写法, set里获取到全选框, 勾选的状态值

②: 遍历数据数组, 赋给所有小选框v-model关联的属性

图示:

![image-20210511120432874](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E5%85%A8%E9%80%89_%E5%BD%B1%E5%93%8D%E5%B0%8F%E9%80%89.gif)

==正确代码,不可以复制==

```vue
<script>
export default {
  // ...其他代码
  // 5. 计算属性-isAll
  computed: {
    isAll: {
      set(val){
        // 7. 全选框 - 选中状态(true/false)
        this.arr.forEach(obj => obj.c = val)
      },
      get(){
        // 6. 统计小选框状态 ->  全选状态
        // every口诀: 查找数组里"不符合"条件, 直接原地返回false
        return this.arr.every(obj => obj.c === true)
      }
    }
  }
};
</script>
```

### 3.6_案例-反选

> 目标: 反选功能

* 需求: 点击反选, 让所有小选框, 各自取相反勾选状态

分析:

①: 小选框的勾选状态, 在对象的c属性

②: 遍历所有对象, 把对象的c属性取相反值赋予回去即可

图示:

![反选_效果](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E5%8F%8D%E9%80%89_%E6%95%88%E6%9E%9C.gif)

==正确代码,不可以复制==

```vue
<button @click="btn">反选</button>

<script>
export default {
  // ...其他代码省略
  methods: {
    btn(){
      // 8. 让数组里对象的c属性取反再赋予回去
      this.arr.forEach(obj => obj.c = !obj.c)
    }
  }
};
</script>
```

## 4. vue侦听器

### 4.0_vue侦听器-watch

> 目标: 可以侦听data/computed属性值改变

语法:

* ```js
  watch: {
      "被侦听的属性名" (newVal, oldVal){
          
      }
  }
  ```

完整例子代码:

```vue
<template>
  <div>
    <input type="text" v-model="name">
  </div>
</template>

<script>
export default {
  data(){
    return {
      name: ""
    }
  },
  // 目标: 侦听到name值的改变
  /*
  语法:
    watch: {
      变量名 (newVal, oldVal){
        // 变量名对应值改变这里自动触发
      }
    }
  */
  watch: {
    // newVal: 当前最新值
    // oldVal: 上一刻值
    name(newVal, oldVal){
      console.log(newVal, oldVal);
    }
  }
}
</script>

<style>

</style>
```

> 总结: 想要侦听一个属性变化, 可使用侦听属性watch

### 4.1_vue侦听器-深度侦听和立即执行  

> 目标: 侦听复杂类型, 或者立即执行侦听函数

* 语法:

  ```js
  watch: {
      "要侦听的属性名": {
          immediate: true, // 立即执行
          deep: true, // 深度侦听复杂类型内变化
          handler (newVal, oldVal) {
              
          }
      }
  }
  ```

完整例子代码:

```vue
<template>
  <div>
    <input type="text" v-model="user.name">
    <input type="text" v-model="user.age">
  </div>
</template>

<script>
export default {
  data(){
    return {
      user: {
        name: "",
        age: 0
      }
    }
  },
  // 目标: 侦听对象
  /*
  语法:
    watch: {
      变量名 (newVal, oldVal){
        // 变量名对应值改变这里自动触发
      },
      变量名: {
        handler(newVal, oldVal){

        },
        deep: true, // 深度侦听(对象里面层的值改变)
        immediate: true // 立即侦听(网页打开handler执行一次)
      }
    }
  */
  watch: {
    user: {
      handler(newVal, oldVal){
        // user里的对象
        console.log(newVal, oldVal);
      },
      deep: true,
      immediate: true
    }
  }
}
</script>

<style>

</style>
```

> 总结: immediate立即侦听, deep深度侦听, handler固定方法触发

### 4.2_案例-品牌管理(数据缓存)

> 目标: 侦听list变化, 同步到浏览器本地

* 需求: 把品牌管理的数据实时同步到本地缓存

分析:

​	① 在watch侦听list变化的时候, 把最新的数组list转成JSON字符串存入到localStorage本地

​	② data里默认把list变量从本地取值, 如果取不到给个默认的空数组

效果:

​	新增/删除 – 刷新页面 – 数据还在

在之前的案例代码基础上接着写

==正确代码,不可复制==

```vue
<script>
import moment from "moment";
export default {
  data() {
    return {
      name: "", // 名称
      price: 0, // 价格
      // 3. 本地取出缓存list
      list: JSON.parse(localStorage.getItem('pList')) || [],
    };
  },
  // ...其他代码省略
  watch: {
    list: {
      handler(){
        // 2. 存入本地
        localStorage.setItem('pList', JSON.stringify(this.list))
      },
      deep: true
    }
  }
};
</script>
```

### 4.3_经典问题
- 父组件的值更新了，但是子组件里面值相应的内容缺未更新。
  解决办法：子组件监听从父组件传过来的props，并设置更新后的值。[博客](https://blog.csdn.net/qq_38880700/article/details/101195673)



## 今日总结

- [ ] v-for能监测到哪些数组方法变化, 更新页面
- [ ] key的作用是什么
- [ ] 虚拟dom好处, diff算法效果
- [ ] 动态设置class或style
- [ ] vue过滤器作用和分类
- [ ] vue计算属性作用
- [ ] vue侦听器的作用



## 面试题

### 1. Vue 中怎么自定义过滤器

​    Vue.js允许自定义过滤器，可被用于一些常见的文本格式化。过滤器可以用在两个地方：双花括号插值和v-bind表达式

​	全局的用Vue.filter()

​	局部的用filters属性

### 2. Vue中:key作用, 为什么不能用索引

​	:key是给v-for循环生成标签颁发唯一标识的, 用于性能的优化

​	因为v-for数据项的顺序改变，Vue 也不会移动 DOM 元素来匹配数据项的顺序，而是就地更新每个元素

​	:key如果是索引, 因为索引是连续的, 如果删除其中某一个, 会导致最后一个被删除

​	当我们再删除的时候, :key再根据数据来把新旧的dom对比时, 删除:key不存在的对应的标签(添加也是一样的插入到指定位置, 别的都不会动)

### 3. 数组更新有的时候v-for不渲染

​	因为vue内部只能监测到数组顺序/位置的改变/数量的改变, 但是值被重新赋予监测不到变更, 可以用 Vue.set() / vm.$set()

## 附加练习_1.买点书练习

> 目标: 把数据铺设到页面上, 当用户点击买书按钮, 书籍数量增加1, 并且要计算累计的和

演示:

![image-20220913193002313](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220913193002313.png)

标签结构和数据(==复制接着写==): 

```vue
<template>
  <div>
    <p>请选择你要购买的书籍</p>
    <ul>
    </ul>
    <table border="1" width="500" cellspacing="0">
      <tr>
        <th>序号</th>
        <th>书名</th>
        <th>单价</th>
        <th>数量</th>
        <th>合计</th>
      </tr>
    </table>
    <p>总价格为: </p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "水浒传",
          price: 107,
          count: 0,
        },
        {
          name: "西游记",
          price: 192,
          count: 0,
        },
        {
          name: "三国演义",
          price: 219,
          count: 0,
        },
        {
          name: "红楼梦",
          price: 178,
          count: 0,
        },
      ],
    };
  }
};
</script>
```

正确答案(==不可复制==)

```vue
<template>
  <div>
    <p>请选择你要购买的书籍</p>
    <ul>
      <li v-for="(item, ind) in arr" :key="ind">
        <span>{{ item["name"] }}</span>
        <button @click="buy(ind)">买书</button>
      </li>
    </ul>
    <table border="1" width="500" cellspacing="0">
      <tr>
        <th>序号</th>
        <th>书名</th>
        <th>单价</th>
        <th>数量</th>
        <th>合计</th>
      </tr>
      <tr v-for="(item, index) in arr" :key="index">
        <td>{{ index + 1 }}</td>
        <td>{{ item["name"] }}</td>
        <td>{{ item["price"] }}</td>
        <td>{{ item["count"] }}</td>
        <td>{{ item["price"] * item["count"] }}</td>
      </tr>
    </table>
    <p>总价格为: {{ allPrice }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          name: "水浒传",
          price: 107,
          count: 0,
        },
        {
          name: "西游记",
          price: 192,
          count: 0,
        },
        {
          name: "三国演义",
          price: 219,
          count: 0,
        },
        {
          name: "红楼梦",
          price: 178,
          count: 0,
        },
      ],
    };
  },
  methods: {
    buy(index) {
      this.arr[index]["count"]++;
    },
  },
  computed: {
    allPrice() {
      // 数组里放的是对象, 而对象是复杂类型, 引用关系, 值改变会触发计算属性重新执行
      return this.arr.reduce((sum, obj) => {
        return (sum += obj["price"] * obj["count"]);
      }, 0);
    },
  },
};
</script>

<style>
</style>
```

## 附加练习_2.选你爱我求和

> 目标: 把用户选中的数字, 累计求和显示

提示: 

* v-model绑定的变量是数组, 可以收集checkbox的value属性呦

演示:

![7.2_案例_选你爱的数字_给你求和](https://gitee.com/XXXTENTWXD/pic/raw/master/images/7.2_%E6%A1%88%E4%BE%8B_%E9%80%89%E4%BD%A0%E7%88%B1%E7%9A%84%E6%95%B0%E5%AD%97_%E7%BB%99%E4%BD%A0%E6%B1%82%E5%92%8C.gif)

数据(复制):

```js
[9, 15, 19, 25, 29, 31, 48, 57, 62, 79, 87]
```

正确答案:(==先不要看==)

```html
<template>
  <div>
    <!-- 无id时, 可以使用index(反正也是就地更新) -->
    <div
      v-for="(item, index) in arr"
      style="display: inline-block"
      :key="index"
    >
      <input type="checkbox" v-model="checkNumArr" :value="item" />
      <span>{{ item }}</span>
    </div>
    <p>你选中的元素, 累加的值和为: {{ theSum }}</p>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [9, 15, 19, 25, 29, 31, 48, 57, 62, 79, 87],
      checkNumArr: [], //  保存用户选中的数字
    };
  },
  computed: {
    theSum() {
      return this.checkNumArr.reduce((sum, val) => {
        return (sum += val);
      }, 0);
    },
  },
};
</script>

<style>
</style>
```

> 总结, 当计算属性函数里引用的vue变量发生改变, 函数就执行并重新返回结果并缓存起来

## 今日作业

先把课上案例搂一遍

### 移动端-导航切换效果

目标: 切换到移动端画面, 点击导航, 高亮

> 提示: 索引 / 高亮的class样式

图例:

![Day02_作业1_移动端导航](https://gitee.com/XXXTENTWXD/pic/raw/master/images/Day02_%E4%BD%9C%E4%B8%9A1_%E7%A7%BB%E5%8A%A8%E7%AB%AF%E5%AF%BC%E8%88%AA.gif)

不带vue的标签结构(==可复制接着写==)

```html
<template>
  <div class="wrap">
    <div class="nav_left" id="navLeft">
      <div class="nav_content">
        <span class="active">导航名字</span>
      </div>
    </div>
    <div class="down">
      <i class="iconfont icon-xiajiantoubeifen gray"></i>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      arr: [
        {
          first_id: "0",
          first_name: "热门"
        },
        {
          first_id: "621",
          first_name: "\u5496\u5561",
        },
        {
          first_id: "627",
          first_name: "\u996e\u98df",
        },
        {
          first_id: "279",
          first_name: "\u7537\u88c5",
        },
        {
          first_id: "294",
          first_name: "\u5973\u88c5",
        },
        {
          first_id: "122",
          first_name: "\u773c\u955c",
        },
        {
          first_id: "339",
          first_name: "\u5185\u8863\u914d\u9970",
        },
        {
          first_id: "391",
          first_name: "\u6bcd\u5a74",
        },
        {
          first_id: "35",
          first_name: "\u978b\u9774",
        },
        {
          first_id: "39",
          first_name: "\u8fd0\u52a8",
        },
        {
          first_id: "153",
          first_name: "\u7bb1\u5305",
        },
        {
          first_id: "119",
          first_name: "\u7f8e\u5986\u4e2a\u62a4",
        },
        {
          first_id: "355",
          first_name: "\u5bb6\u7eba",
        },
        {
          first_id: "51",
          first_name: "\u9910\u53a8",
        },
        {
          first_id: "334",
          first_name: "\u7535\u5668",
        },
        {
          first_id: "369",
          first_name: "\u5bb6\u88c5",
        },
        {
          first_id: "10",
          first_name: "\u5bb6\u5177",
        },
        {
          first_id: "223",
          first_name: "\u6570\u7801",
        },
        {
          first_id: "429",
          first_name: "\u6c7d\u914d",
        },
        {
          first_id: "546",
          first_name: "\u5065\u5eb7\u4fdd\u5065",
        },
        {
          first_id: "433",
          first_name: "\u5b9a\u5236",
        },
      ],
    };
  },

};
</script>

<style>
.wrap {
  width: 100%;
  display: flex;
  margin: 0.2rem 0 0 0;
  position: relative;
}

/*左侧的导航样式*/
.nav_left {
  width: 21.1875rem;
  overflow: scroll;
}

.nav_left::-webkit-scrollbar {
  display: none;
}

.nav_content {
  white-space: nowrap;
  padding: 0 0.7rem;
}

.nav_content span {
  display: inline-block;
  padding: 0.4rem 0.6rem;
  font-size: 0.875rem;
}

.nav_content .active {
  border-bottom: 2px solid #7f4395;
  color: #7f4395;
}

.nav_left,
.down {
  float: left;
}

/*右侧导航部分*/
.down {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.gray {
  color: gray;
  display: inline-block;
  vertical-align: middle;
}
</style>
```

### 学生信息管理

==如果1个按钮不会写, 用2个按钮写==

目标: 

* 需求1: 铺设页面, 准备初始的数据(自己手写数据结构) - 前面是数组索引+1 *作为序号
* 需求2: 当输入框没有值, 要给用户一个提示, 必须都有值才能增加新数据 (数据驱动页面哦)
* 需求3: 添加功能 - 想好数据结构统一对象的key
* 需求4: 点击编辑功能, 把值赋予到输入框上(不要操作dom, 数据驱动页面)
* 需求5: 用户修改后, 点击相同按钮 - 想想怎么判断怎么是添加还是修改的功能 (提示: 准备一个全局变量, 点过编辑按钮可以让它为true) - 实现编辑后更新页面效果
* 需求6: 点击删除, 删除这行数据

![Day02_作业_学生信息管理](https://gitee.com/XXXTENTWXD/pic/raw/master/images/Day02_%E4%BD%9C%E4%B8%9A_%E5%AD%A6%E7%94%9F%E4%BF%A1%E6%81%AF%E7%AE%A1%E7%90%86.gif)

不带vue代码的标签结构(==可复制接着写==)

```vue
<template>
  <div id="app">
    <div>
      <span>姓名:</span>
      <input type="text" />
    </div>
    <div>
      <span>年龄:</span>
      <input type="number" />
    </div>
    <div>
      <span>性别:</span>
      <select >
        <option value="男">男</option>
        <option value="女">女</option>
      </select>
    </div>
    <div>
      <button >添加/修改</button>
    </div>
    <div>
      <table
        border="1"
        cellpadding="10"
        cellspacing="0"
      >
        <tr>
          <th>序号</th>
          <th>姓名</th>
          <th>年龄</th>
          <th>性别</th>
          <th>操作</th>
        </tr>
        <tr >
          <td></td>
          <td></td>
          <td></td>
          <td></td>
          <td>
            <button >删除</button>
            <button >编辑</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>
<script>
export default {
  
}
</script>

```



# 第五章 Vue组件、组件通信、todo案例

## 知识点自测

- [ ] this指向

```js
let obj = {
    fn: function(){
        // this指向此函数的调用者
    },
    fn () {
        // this指向当前函数的调用者 (如果都是在vue里, this指向的都是vue实例对象)
    },
    fn: () => {
        // this指向外层函数作用域this的值
    }
}
obj.fn();

axios().then(res => {
    // 这里的this的值是多少哦?
})
```

- [ ] =作用

```js
let a = 10;
let b = a; 
b = 20; // 基础类型, 单纯的值的赋值

let a = {name: "哈哈"};
let b = a; // a变量的值是引用类型, a里保存的是对象在堆的内存地址, 所以b和a指向同一个对象 (引用类型=是内存地址的赋值)
b.name = "刘总";
```

## 今日学习目标

1. 能够理解vue组件概念和作用
2. 能够掌握封装组件能力
3. 能够使用组件之间通信
4. 能够完成todo案例

## 1. vue组件

### 1.0_为什么用组件

以前做过一个折叠面板

![image-20210115092834016](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210115092834016.png)

需求: 现在想要多个收起展开的部分

方案1: 复制代码

- 代码重复 冗余
- 不利于维护

1. 案例用less写的样式, 所以下载

```bash
yarn add less less-loader@5.0.0 -D
```

2. 模板标签 - 在这个基础上, 把==要复用的多复制几份==

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="isShow = !isShow">
          {{ isShow ? '收起' : '展开' }}
        </span>
      </div>
      <div class="container" v-show="isShow">
        <p>寒雨连江夜入吴, </p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false
    }
  }
}
</script>

<style lang="less">
body {
  background-color: #ccc;
  #app {
    width: 400px;
    margin: 20px auto;
    background-color: #fff;
    border: 4px solid blueviolet;
    border-radius: 1em;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
    padding: 1em 2em 2em;
    h3 {
      text-align: center;
    }
    .title {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .title h4 {
      line-height: 2;
      margin: 0;
    }
    .container {
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .btn {
      /* 鼠标改成手的形状 */
      cursor: pointer;
    }
  }
}
</style>

```

3. 上面复制3份, 发现变化一起变化

   解决方案: 不同的部分, 用不同的isShow变量

```html
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="isShow = !isShow">
          {{ isShow ? '收起' : '展开' }}
        </span>
      </div>
      <div class="container" v-show="isShow">
        <p>寒雨连江夜入吴, </p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="isShow1 = !isShow1">
          {{ isShow1 ? '收起' : '展开' }}
        </span>
      </div>
      <div class="container" v-show="isShow1">
        <p>寒雨连江夜入吴, </p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
    <div>
      <div class="title">
        <h4>芙蓉楼送辛渐</h4>
        <span class="btn" @click="isShow2 = !isShow2">
          {{ isShow2 ? '收起' : '展开' }}
        </span>
      </div>
      <div class="container" v-show="isShow2">
        <p>寒雨连江夜入吴, </p>
        <p>平明送客楚山孤。</p>
        <p>洛阳亲友如相问，</p>
        <p>一片冰心在玉壶。</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false,
      isShow1: false,
      isShow2: false
    }
  }
}
</script>

<style lang="less">
body {
  background-color: #ccc;
  #app {
    width: 400px;
    margin: 20px auto;
    background-color: #fff;
    border: 4px solid blueviolet;
    border-radius: 1em;
    box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
    padding: 1em 2em 2em;
    h3 {
      text-align: center;
    }
    .title {
      display: flex;
      justify-content: space-between;
      align-items: center;
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .title h4 {
      line-height: 2;
      margin: 0;
    }
    .container {
      border: 1px solid #ccc;
      padding: 0 1em;
    }
    .btn {
      /* 鼠标改成手的形状 */
      cursor: pointer;
    }
  }
}
</style>

```

> 总结: 代码非常的冗余和重复吧? 解决方案呢? 就是采用我们的组件化开发的方式, 往下看

### 1.1_vue组件_概念

> 组件是可复用的 Vue 实例, 封装标签, 样式和JS代码

**组件化** ：封装的思想，把页面上 `可重用的部分` 封装为 `组件`，从而方便项目的 开发 和 维护

**一个页面， 可以拆分成一个个组件，一个组件就是一个整体, 每个组件可以有自己独立的 结构 样式 和 行为(html, css和js)**

![image-20220920001442433](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220920001442433.png)

### 1.2_vue组件_基础使用

> 目标: 每个组件都是一个独立的个体, 代码里体现为一个独立的.vue文件

口诀: 哪部分标签复用, 就把哪部分封装到组件内

==(重要): 组件内template只能有一个根标签==

==(重要): 组件内data必须是一个函数, 独立作用域==

步骤:

1. 创建组件 components/Pannel.vue

> 封装标签+样式+js - 组件都是独立的, 为了复用

```vue
<template>
  <div>
    <div class="title">
      <h4>芙蓉楼送辛渐</h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? "收起" : "展开" }}
      </span>
    </div>
    <div class="container" v-show="isShow">
      <p>寒雨连江夜入吴,</p>
      <p>平明送客楚山孤。</p>
      <p>洛阳亲友如相问，</p>
      <p>一片冰心在玉壶。</p>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false,
    };
  },
};
</script>

<style scoped>
.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #ccc;
  padding: 0 1em;
}
.title h4 {
  line-height: 2;
  margin: 0;
}
.container {
  border: 1px solid #ccc;
  padding: 0 1em;
}
.btn {
  /* 鼠标改成手的形状 */
  cursor: pointer;
}
</style>
```

2. 注册组件: 创建后需要注册后再使用

> ### 全局 - 注册使用

全局入口在main.js, 在new Vue之上注册

语法:

```js
import Vue from 'vue'
import 组件对象 from 'vue文件路径'

Vue.component("组件名", 组件对象)
```

main.js - 立即演示

```js
// 目标: 全局注册 (一处定义到处使用)
// 1. 创建组件 - 文件名.vue
// 2. 引入组件
import Pannel from './components/Pannel'
// 3. 全局 - 注册组件
/*
  语法: 
  Vue.component("组件名", 组件对象)
*/
Vue.component("PannelG", Pannel)
```

全局注册PannelG组件名后, 就可以当做标签在任意Vue文件中template里用

单双标签都可以或者小写加-形式, 运行后, 会把这个自定义标签当做组件解析, 使用==组件里封装的标签替换到这个位置==

```vue
<PannelG></PannelG>
<PannelG/>
<pannel-g></pannel-g>
```

> ### 局部 - 注册使用

语法:

```js
import 组件对象 from 'vue文件路径'

export default {
    components: {
        "组件名": 组件对象
    }
}
```

任意vue文件中中引入, 注册, 使用

```vue
<template>
  <div id="app">
    <h3>案例：折叠面板</h3>
    <!-- 4. 组件名当做标签使用 -->
    <!-- <组件名></组件名> -->
    <PannelG></PannelG>
    <PannelL></PannelL>
  </div>
</template>

<script>
// 目标: 局部注册 (用的多)
// 1. 创建组件 - 文件名.vue
// 2. 引入组件
import Pannel from './components/Pannel_1'
export default {
  // 3. 局部 - 注册组件
  /*
    语法: 
    components: {
      "组件名": 组件对象
    }
  */
  components: {
    PannelL: Pannel
  }
}
</script>
```

组件使用总结:

1. (创建)封装html+css+vue到独立的.vue文件中
2. (引入注册)组件文件 => 得到组件配置对象
3. (使用)当前页面当做标签使用

### 1.3_vue组件-scoped作用

> 目的: 解决多个组件样式名相同, 冲突问题

需求: div标签名选择器, 设置背景色

问题: 发现组件里的div和外面的div都生效了

解决: 给Pannel.vue组件里style标签上加scoped属性即可

```vue
<style scoped>
```

在style上加入scoped属性, 就会在此组件的标签上加上一个随机生成的data-v开头的属性

而且必须是当前组件的元素, 才会有这个自定义属性, 才会被这个样式作用到

![image-20220920001456962](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220920001456962.png)

> 总结: style上加scoped, 组件内的样式只在当前vue组件生效

## 2. vue组件通信

因为每个组件的变量和值都是独立的

组件通信先暂时关注父传子, 子传父

父: 使用其他组件的vue文件

子: 被引入的组件(嵌入)

例如: App.vue(父)  MyProduct.vue(子)

### 2.0_vue组件通信_父向子-props

> 目的: 从外面给组件内传值, 先学会语法, 练习中在看使用场景

需求: 封装一个商品组件MyProduct.vue - 外部传入具体要显示的数据, 如下图所示

![image-20210305201956669](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210305201956669.png)

步骤:

1. 创建组件components/MyProduct.vue - 复制下面标签

2. 组件内在props定义变量, 用于接收外部传入的值

3. App.vue中引入注册组件, 使用时, 传入具体数据给组件显示

components/MyProduct.vue - 准备标签

```vue
<template>
  <div class="my-product">
    <h3>标题: {{ title }}</h3>
    <p>价格: {{ price }}元</p>
    <p>{{ intro }}</p>
  </div>
</template>

<script>
export default {
  props: ['title', 'price', 'intro']
}
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

App.vue中使用并传入数据

```vue
<template>
  <div>
    <!-- 
      目标: 父(App.vue) -> 子(MyProduct.vue) 分别传值进入
      需求: 每次组件显示不同的数据信息
      步骤(口诀):
        1. 子组件 - props - 变量 (准备接收)
        2. 父组件 - 传值进去
     -->
    <Product title="好吃的口水鸡" price="50" intro="开业大酬宾, 全场8折"></Product>
    <Product title="好可爱的可爱多" price="20" intro="老板不在家, 全场1折"></Product>
    <Product title="好贵的北京烤鸭" price="290" :intro="str"></Product>
  </div>
</template>

<script>
// 1. 创建组件 (.vue文件)
// 2. 引入组件
import Product from './components/MyProduct'
export default {
  data(){
    return {
      str: "好贵啊, 快来啊, 好吃"
    }
  },
  // 3. 注册组件
  components: {
    // Product: Product // key和value变量名同名 - 简写
    Product
  }
}
</script>

<style>

</style>
```

> 总结: 组件封装复用的标签和样式, 而具体数据要靠外面传入

### 2.1_vue组件通信_父向子-配合循环

> 目的: 把数据循环分别传入给组件内显示

数据

```js
list: [
    { id: 1, proname: "超级好吃的棒棒糖", proprice: 18.8, info: '开业大酬宾, 全场8折' },
    { id: 2, proname: "超级好吃的大鸡腿", proprice: 34.2, info: '好吃不腻, 快来买啊' },
    { id: 3, proname: "超级无敌的冰激凌", proprice: 14.2, info: '炎热的夏天, 来个冰激凌了' },
],
```

正确代码(==不可复制==)`

```vue
<template>
  <div>
    <MyProduct v-for="obj in list" :key="obj.id"
    :title="obj.proname"
    :price="obj.proprice"
    :intro="obj.info"
    ></MyProduct>
  </div>
</template>

<script>
// 目标: 循环使用组件-分别传入数据
// 1. 创建组件
// 2. 引入组件
import MyProduct from './components/MyProduct'
export default {
  data() {
    return {
      list: [
        {
          id: 1,
          proname: "超级好吃的棒棒糖",
          proprice: 18.8,
          info: "开业大酬宾, 全场8折",
        },
        {
          id: 2,
          proname: "超级好吃的大鸡腿",
          proprice: 34.2,
          info: "好吃不腻, 快来买啊",
        },
        {
          id: 3,
          proname: "超级无敌的冰激凌",
          proprice: 14.2,
          info: "炎热的夏天, 来个冰激凌了",
        },
      ],
    };
  },
  // 3. 注册组件
  components: {
    // MyProduct: MyProduct
    MyProduct
  }
};
</script>

<style>
</style>
```

> ### 单向数据流

在vue中需要遵循单向数据流原则

    1. 父组件的数据发生了改变，子组件会自动跟着变
    2. 子组件不能直接修改父组件传递过来的props  props是只读的

==父组件传给子组件的是一个对象，子组件修改对象的属性，是不会报错的，对象是引用类型, 互相更新==

![image-20210423161646951](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210423161646951.png)

> 总结: props的值不能重新赋值, 对象引用关系属性值改变, 互相影响

### 2.2_vue组件通信_单向数据流

> 目标: props变量本身是只读不能重新赋值

目标：从==父到子==的数据流向,叫==单向数据流==

原因: 子组件修改, 不通知父级, 造成数据不一致性

如果第一个MyProduct.vue内自己修改商品价格为5.5, 但是App.vue里原来还记着18.8 - 数据 不一致了

所以: Vue规定==props==里的变量, ==本身是只读==的

![image-20210511143218215](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210511143218215.png)

> 总结: 所以props变量本身是不能重新赋值的

问题:  那我怎么才能修改子组件接收到的值呢? - 其实要影响父亲, 然后数据响应式来影响儿子们

### 2.3_vue组件通信_子向父

> 目标: 从子组件把值传出来给外面使用

需求: 课上例子, 砍价功能, 子组件点击实现随机砍价-1功能

![image-20210307134253897](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210307134253897.png)

语法:

* 父: @自定义事件名="父methods函数"
* 子: this.$emit("自定义事件名", 传值) - 执行父methods里函数代码

![image-20220920001549268](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220920001549268.png)

components/MyProduct_sub.vue

```vue
<template>
  <div class="my-product">
    <h3>标题: {{ title }}</h3>
    <p>价格: {{ price }}元</p>
    <p>{{ intro }}</p>
    <button @click="subFn">宝刀-砍1元</button>
  </div>
</template>

<script>
import eventBus from '../EventBus'
export default {
  props: ['index', 'title', 'price', 'intro'],
  methods: {
    subFn(){
      this.$emit('subprice', this.index, 1) // 子向父
      eventBus.$emit("send", this.index, 1) // 跨组件
    }
  }
}
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

App.vue

```vue
<template>
  <div>
    <!-- 目标: 子传父 -->
    <!-- 1. 父组件, @自定义事件名="父methods函数" -->
    <MyProduct v-for="(obj, ind) in list" :key="obj.id"
    :title="obj.proname"
    :price="obj.proprice"
    :intro="obj.info"
    :index="ind"
    @subprice="fn"
    ></MyProduct>
  </div>
</template>

<script>

import MyProduct from './components/MyProduct_sub'
export default {
  data() {
    return {
      list: [
        {
          id: 1,
          proname: "超级好吃的棒棒糖",
          proprice: 18.8,
          info: "开业大酬宾, 全场8折",
        },
        {
          id: 2,
          proname: "超级好吃的大鸡腿",
          proprice: 34.2,
          info: "好吃不腻, 快来买啊",
        },
        {
          id: 3,
          proname: "超级无敌的冰激凌",
          proprice: 14.2,
          info: "炎热的夏天, 来个冰激凌了",
        },
      ],
    };
  },
  components: {
    MyProduct
  },
  methods: {
    fn(inde, price){
      // 逻辑代码
      this.list[inde].proprice > 1 && (this.list[inde].proprice = (this.list[inde].proprice - price).toFixed(2))
    }
  }
};
</script>

<style>
</style>
```

> 总结: 父自定义事件和方法, 等待子组件触发事件给方法传值

### 2.4_阶段小结

> 目标: 总结父子组件关系-通信技术口诀

组件是什么?

* 是一个vue实例, 封装标签, 样式和JS代码

组件好处?

* 便于复用, 易于扩展

组件通信哪几种, 具体如何实现?

* 父 -> 子

* 父 <- 子

### 2.5_vue组件通信-EventBus

> 目标: 常用于跨组件通信时使用

两个组件的关系非常的复杂，通过父子组件通讯是非常麻烦的。这时候可以使用通用的组件通讯方案：事件总线（event-bus)

![image-20210416122123301](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210416122123301.png)

核心语法

EventBus/index.js- 定义事件总线bus对象

```js
import Vue from 'vue'
// 导出空白vue对象
export default new Vue()
```

List.vue注册事件 - 等待接收要砍价的值 (==直接复制==) - 准备兄弟页面

```vue
<template>
  <ul class="my-product">
      <li v-for="(item, index) in arr" :key="index">
          <span>{{ item.proname }}</span>
          <span>{{ item.proprice }}</span>
      </li>
  </ul>
</template>

<script>
export default {
  props: ['arr'],
}
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

components/MyProduct_sub.vue(==带学生主要写触发eventBus身上事件==)

```vue
<template>
  <div class="my-product">
    <h3>标题: {{ title }}</h3>
    <p>价格: {{ price }}元</p>
    <p>{{ intro }}</p>
    <button @click="subFn">宝刀-砍1元</button>
  </div>
</template>

<script>
import eventBus from '../EventBus'
export default {
  props: ['index', 'title', 'price', 'intro'],
  methods: {
    subFn(){
      this.$emit('subprice', this.index, 1) // 子向父
      eventBus.$emit("send", this.index, 1) // 跨组件
    }
  }
}
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

List.vue正确代码(==EventBus接收方==)

```vue
<template>
  <ul class="my-product">
    <li v-for="(item, index) in arr" :key="index">
      <span>{{ item.proname }}</span>
      <span>{{ item.proprice }}</span>
    </li>
  </ul>
</template>

<script>
// 目标: 跨组件传值
// 1. 引入空白vue对象(EventBus)
// 2. 接收方 - $on监听事件
import eventBus from "../EventBus";
export default {
  props: ["arr"],
  // 3. 组件创建完毕, 监听send事件
  created() {
    eventBus.$on("send", (index, price) => {
      this.arr[index].proprice > 1 &&
        (this.arr[index].proprice = (this.arr[index].proprice - price).toFixed(2));
    });
  },
};
</script>

<style>
.my-product {
  width: 400px;
  padding: 20px;
  border: 2px solid #000;
  border-radius: 5px;
  margin: 10px;
}
</style>
```

> 总结: 空的Vue对象, 只负责$on注册事件, $emit触发事件, 一定要确保$on先执行

## 3. todo案例

完整效果演示

![品牌管理_铺增删](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/%E5%93%81%E7%89%8C%E7%AE%A1%E7%90%86_%E9%93%BA%E5%A2%9E%E5%88%A0.gif)

### 3.0_todo案例-创建工程和组件

> 目标: 新建工程, 准备好所需的一切

* 需求1: 创建新工程
* 需求2: 分组件创建 – 准备标签和样式(从.md笔记复制)

分析：

​	①：初始化todo工程

​	②：创建３个组件和里面代码(在预习资料.md复制)

​	③：把styles的样式文件准备好(从预习资料复制)

​	④:  App.vue引入注册使用, 最外层容器类名todoapp

预先准备: 把styles的样式文件准备好(从预习资料复制), 在App.vue引入使用

```js
// 1.0 样式引入
import "./styles/base.css"
import "./styles/index.css"
```

根据需求: 我们定义3个组件准备复用

![image-20220920001618737](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220920001618737.png)

components/TodoHeader.vue - 复制标签和类名

```vue
<template>
  <header class="header">
    <h1>todos</h1>
    <input id="toggle-all" class="toggle-all" type="checkbox" >
    <label for="toggle-all"></label>
    <input
      class="new-todo"
      placeholder="输入任务名称-回车确认"
      autofocus
    />
  </header>
</template>

<script>
export default {
 
}
</script>
```

components/TodoMain.vue - 复制标签和类名

```vue
<template>
  <ul class="todo-list">
    <!-- completed: 完成的类名 -->
    <li class="completed" >
      <div class="view">
        <input class="toggle" type="checkbox" />
        <label>任务名</label>
        <button class="destroy"></button>
      </div>
    </li>
  </ul>
  
</template>

<script>
export default {
}
</script>
```

components/TodoFooter.vue - 复制标签和类名

```vue
<template>
  <footer class="footer">
    <span class="todo-count">剩余<strong>数量值</strong></span>
    <ul class="filters">
      <li>
        <a class="selected" href="javascript:;" >全部</a>
      </li>
      <li>
        <a href="javascript:;">未完成</a>
      </li>
      <li>
        <a href="javascript:;" >已完成</a>
      </li>
    </ul>
    <button class="clear-completed" >清除已完成</button>
  </footer>
</template>

<script>
export default {

}
</script>
```

App.vue中引入和使用

```vue
<template>
  <section class="todoapp">
    <!-- 除了驼峰, 还可以使用-转换链接 -->
    <TodoHeader></TodoHeader>
    <TodoMain></TodoMain>
    <TodoFooter></TodoFooter>
  </section>
</template>

<script>
// 1.0 样式引入
import "./styles/base.css"
import "./styles/index.css"
    
import TodoHeader from "./components/TodoHeader";
import TodoMain from "./components/TodoMain";
import TodoFooter from "./components/TodoFooter";


export default {
  components: {
    TodoHeader,
    TodoMain,
    TodoFooter,
  },
};
</script>
```

### 3.1_todo案例-铺设待办任务

> 目的: 把待办任务, 展示到页面TodoMain.vue组件上

* 需求1: 把待办任务, 展示到页面TodoMain.vue组件上
* 需求2: 关联选中状态, 设置相关样式

分析：

​	①: App.vue – 准备数组传入TodoMain.vue内

​	②: v-for循环展示数据

​	③: v-model绑定复选框选中状态

​	④: 根据选中状态, 设置完成划线样式

App.vue

```js
 <TodoMain :arr="showArr"></TodoMain>

export default {
  data() {
    return {
      list: [
        { id: 100, name: "吃饭", isDone: true },
        { id: 201, name: "睡觉", isDone: false },
        { id: 103, name: "打豆豆", isDone: true },
      ],
    };
  }
};
```

TodoMain.vue

```vue
<template>
  <ul class="todo-list">
    <!-- 2.2 循环任务-关联选中状态-铺设数据 -->
    <!-- completed: 完成的类名 -->
    <li :class="{completed: obj.isDone}" v-for="(obj, index) in arr" :key='obj.id'>
      <div class="view">
        <input class="toggle" type="checkbox" v-model="obj.isDone"/>
        <label>{{ obj.name }}</label>
        <!-- 4.0 注册点击事件 -->
        <button @click="delFn(index)" class="destroy"></button>
      </div>
    </li>
  </ul>
</template>

<script>
export default {
  props: ["list"]
};
</script>

<style>
</style>
```

### 3.2_todo案例-添加任务

> 目标: 在顶部输入框输入要完成的任务名, 敲击回车, 完成新增功能

* 需求: 输入任务敲击回车, 新增待办任务

分析：

​	①: TodoHeader.vue – 输入框 – 键盘事件 – 回车按键

​	②: 子传父, 把待办任务 – App.vue中 – 加入数组list里

​	③: 原数组改变, 所有用到的地方都会更新

​	④: 输入框为空, 提示用户必须输入内容

TodoHeader.vue

```vue
<template>
  <header class="header">
    <h1>todos</h1>
    <input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll">
    <label for="toggle-all"></label>
    <!-- 3.0 键盘事件-回车按键
         3.1 输入框 - v-model获取值
     -->
    <input
      class="new-todo"
      placeholder="输入任务名称-回车确认"
      autofocus
      @keydown.enter="downFn"
      v-model="task"
    />
  </header>
</template>

<script>
// 3. 目标 - 新增任务
export default {
  data(){
    return {
      task: ""
    }
  },
  methods: {
    downFn(){
      if (this.task.trim().length === 0) {
        alert("任务名不能为空");
        return;
      }
      // 3.2(重要) - 当前任务名字要加到list数组里
      // 子传父技术
      this.$emit("create", this.task)
      this.task = ""
    }
  }
}
</script>
```

App.vue

```js
<TodoHeader @create="createFn"></TodoHeader>

methods: {
   createFn(taskName){ // 添加任务
      
      this.list.push({
        id: id,
        name: taskName,
        isDone: false
      })
    },
}
```

### 3.3_todo案例-删除任务

> 目标: 实现点x, 删除任务功能

* 需求: 点击任务后的x, 删除当前这条任务

分析：

​	①: x标签 – 点击事件 – 传入id区分

​	②: 子传父, 把id传回– App.vue中 – 删除数组list里某个对应的对象

​	③: 原数组改变, 所有用到的地方都会更新

App.vue - 传入自定义事件等待接收要被删除的序号

```js
<TodoMain :arr="showArr" @del="deleteFn"></TodoMain>

methods: {
    deleteFn(theId){ // 删除任务
      let index = this.list.findIndex(obj => obj.id === theId)
      this.list.splice(index, 1)
    },
},
```

TodoMain.vue - 把id传回去实现删除(想好数据在哪里, 就在哪里删除)

```js
<!-- 4.0 注册点击事件 -->
<button class="destroy" @click="delFn(obj.id)"></button>

methods: {
     delFn(id){
      // 4.1 子传父
      this.$emit('del', id)
    }
}
```

### 3.4_todo案例-底部统计

> 目的: 显示现在任务的总数

* 需求: 统计当前任务的条数

分析：

​	①: App.vue中 – 数组list – 传给TodoFooter.vue

​	②: 直接在标签上显示 / 定义计算属性用于显示都可以

​	③: 原数组只要改变, 所有用到此数组的地方都会更新

TodoFooter.vue - 接收list统计直接显示

```vue
<template>
  <footer class="footer">
    <span class="todo-count">剩余<strong>{{ count }}</strong></span>
    <ul class="filters">
      <li>
        <a class="selected" href="javascript:;">全部</a>
      </li>
      <li>
        <a href="javascript:;">未完成</a>
      </li>
      <li>
        <a href="javascript:;">已完成</a>
      </li>
    </ul>
    <button class="clear-completed">清除已完成</button>
  </footer>
</template>

<script>
export default {
  // 5.0 props定义
  props: ['farr'],
  // 5.1 计算属性 - 任务数量
  computed: {
    count(){
      return this.farr.length
    }
  },
}
</script>

<style>

</style>
```

App.vue - 传入数据

```vue
<TodoFooter :farr="showArr"></TodoFooter>
```

### 3.5_todo案例-数据切换

> 目的: 点击底部切换数据

* 需求1: 点击底部切换 – 点谁谁有边框
* 需求2: 对应切换不同数据显示

分析：

​	①: TodoFooter.vue – 定义isSel – 值为all, yes, no其中一种

​	②: 多个class分别判断谁应该有类名selected

​	③: 点击修改isSel的值

​	④: 子传父, 把类型isSel传到App.vue

​	⑤: 定义计算属性showArr, 决定从list里显示哪些数据给TodoMain.vue和TodoFooter.vue

App.vue

```vue
<TodoFooter :farr="showArr" @changeType="typeFn"></TodoFooter>

<script>
    export default{
       data(){
            return {
              // ...其他省略
              getSel: "all" // 默认显示全部
            }
        },
        methods: {
            // ...其他省略
            typeFn(str){ // 'all' 'yes' 'no' // 修改类型
              this.getSel = str
            },
        },
        // 6.5 定义showArr数组 - 通过list配合条件筛选而来
          computed: {
            showArr(){
              if (this.getSel === 'yes') { // 显示已完成
                return this.list.filter(obj => obj.isDone === true)
              } else if (this.getSel === 'no') { // 显示未完成
                return this.list.filter(obj => obj.isDone === false)
              } else {
                return this.list // 全部显示
              }
            }
          },
    }
</script>
```

TodoFooter.vue

```vue
<template>
  <footer class="footer">
    <span class="todo-count">剩余<strong>{{ count }}</strong></span>
    <ul class="filters" @click="fn">
      <li>
        <!-- 6.1 判断谁应该有高亮样式: 动态class
            6.2 用户点击要切换isSel里保存的值
         -->
        <a :class="{selected: isSel === 'all'}" href="javascript:;" @click="isSel='all'">全部</a>
      </li>
      <li>
        <a :class="{selected: isSel === 'no'}" href="javascript:;" @click="isSel='no'">未完成</a>
      </li>
      <li>
        <a :class="{selected: isSel === 'yes'}" href="javascript:;" @click="isSel='yes'">已完成</a>
      </li>
    </ul>
    <!-- 7. 目标: 清除已完成 -->
    <!-- 7.0 点击事件 -->
    <button class="clear-completed" >清除已完成</button>
  </footer>
</template>

<script>
// 5. 目标: 数量统计
export default {
  // 5.0 props定义
  props: ['farr'],
  // 5.1 计算属性 - 任务数量
  computed: {
    count(){
      return this.farr.length
    }
  },
  // 6. 目标: 点谁谁亮
  // 6.0 变量isSel
  data(){
    return {
      isSel: 'all' // 全部:'all', 已完成'yes', 未完成'no'
    }
  },
  methods: {
    fn(){ // 切换筛选条件
      // 6.3 子 -> 父 把类型字符串传给App.vue 
      this.$emit("changeType", this.isSel)
    }
  }
}
</script>
```

### 3.6_todo案例-清空已完成

> 目的: 点击右下角按钮- 把已经完成的任务清空了

* 需求: 点击右下角链接标签, 清除已完成任务 

分析：

​	①: 清空标签 – 点击事件

​	②: 子传父 – App.vue – 一个清空方法

​	③: 过滤未完成的覆盖list数组 (不考虑恢复)

App.vue - 先传入一个自定义事件-因为得接收TodoFooter.vue里的点击事件

```vue
<TodoFooter :farr="showArr" @changeType="typeFn" @clear="clearFun"></TodoFooter>

<script>
    methods: {
        // ...省略其他
        clearFun(){ // 清除已完成
          this.list = this.list.filter(obj => obj.isDone == false)
        }
    }
</script>
```

TodoFooter.vue

```vue
<!-- 7. 目标: 清除已完成 -->
<!-- 7.0 点击事件 -->
<button class="clear-completed" @click="clearFn">清除已完成</button>

<script>
	methods: {
        clearFn(){ // 清空已完成任务
          // 7.1 触发App.vue里事件对应clearFun方法
          this.$emit('clear')
        }
    }
</script>
```

### 3.7_todo案例-数据缓存

> 目的: 新增/修改状态/删除 后, 马上把数据同步到浏览器本地存储

* 需求: 无论如何变化 – 都保证刷新后数据还在

分析：

​	①: App.vue – 侦听list数组改变 – 深度

​	②: 覆盖式存入到本地 – 注意本地只能存入JSON字符串

​	③: 刷新页面 – list应该默认从本地取值 – 要考虑无数据情况空数组

App.vue

```vue
<script>
    export default {
        data(){
            return {
                // 8.1 默认从本地取值
                list: JSON.parse(localStorage.getItem('todoList')) || [],
                // 6.4 先中转接收类型字符串
                getSel: "all" // 默认显示全部
            }
        },
        // 8. 目标: 数据缓存
        watch: {
            list: {
                deep: true,
                handler(){
                    // 8.0 只要list变化 - 覆盖式保存到localStorage里
                    localStorage.setItem('todoList', JSON.stringify(this.list))
                }
            }
        }
    };
</script>
```

### 3.8_todo案例-全选功能

> 目标: 点击左上角v号, 可以设置一键完成, 再点一次取消全选

* 需求1: 点击全选 – 小选框受到影响
* 需求2: 小选框都选中(手选) – 全选自动选中状态

分析：

​	①: TodoHeader.vue – 计算属性 - isAll

​	②: App.vue – 传入数组list – 在isAll的set里影响小选框

​	③: isAll的get里统计小选框最后状态, 影响isAll – 影响全选状态

​	④: 考虑无数据情况空数组 – 全选不应该勾选

提示: 就是遍历所有的对象, 修改他们的完成状态属性的值

TodoHeader.vue

```vue
<!-- 9. 目标: 全选状态
9.0 v-model关联全选状态
页面变化(勾选true, 未勾选false) -> v-model -> isAll变量
-->
<input id="toggle-all" class="toggle-all" type="checkbox" v-model="isAll">

<script>
    export default {
        // ...其他省略
        // 9.1 定义计算属性
        computed: {
            isAll: {
                set(checked){ // 只有true / false
                    // 9.3 影响数组里每个小选框绑定的isDone属性
                    this.arr.forEach(obj => obj.isDone = checked)
                },
                get(){
                    // 9.4 小选框统计状态 -> 全选框
                    // 9.5 如果没有数据, 直接返回false-不要让全选勾选状态
                    return this.arr.length !== 0 && this.arr.every(obj => obj.isDone === true)
                }
            }
        },
    }
</script>
```

App.vue

```vue
<TodoHeader :arr="list" @create="createFn"></TodoHeader>
```

## 今日总结

- [ ] 组件概念和作用以及创建和使用方式


- [ ] 掌握组件通信包括父向子, 子向父传值


- [ ] 熟悉EventBus的使用和原理


- [ ] 跟随老师的视频完成todo案例的全部功能


## 面试题

### 1. 请说下封装 vue 组件的过程

​    首先，组件可以提升整个项目的开发效率。能够把页面抽象成多个相对独立的模块，解决了我们传统项目开发：效率低、难维护、复用性等问题。

* 分析需求：确定业务需求，把页面中可以复用的结构，样式以及功能，单独抽离成一个组件，实现复用

* 具体步骤：Vue.component 或者在new Vue配置项components中, 定义组件名, 可以在props中接受给组件传的参数和值，子组件修改好数据后，想把数据传递给父组件。可以采用$emit方法。

### 2. Vue组件如何进行传值的

父向子 -> props定义变量 -> 父在使用组件用属性给props变量传值

子向父 -> $emit触发父的事件 -> 父在使用组件用@自定义事件名=父的方法 (子把值带出来)

### 3. Vue 组件 data 为什么必须是函数

每个组件都是 Vue 的实例, 为了独立作用域, 不让变量污染别人的变量

### 4. 讲一下组件的命名规范

​    给组件命名有两种方式(在Vue.Component/components时)，一种是使用链式命名"my-component"，一种是使用大驼峰命名"MyComponent"，

​	因为要遵循W3C规范中的自定义组件名 (字母全小写且必须包含一个连字符)，避免和当前以及未来的 HTML 元素相冲突

## 附加练习_1.喜欢小狗狗吗

> 目标: 封装Dog组件, 用来复用显示图片和标题的

效果:

![image-20220920001637226](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220920001637226.png)

----

正确答案(==先不要看==)

components/practise/Dog1.vue

```html
<template>
  <div class="my_div">
    <img
      src="https://scpic.chinaz.net/files/pic/pic9/202003/zzpic23514.jpg"
      alt=""
    />
    <p>这是一个孤独可怜的狗</p>
  </div>
</template>

<script>
export default {};
</script>

<style>
.my_div {
  width: 200px;
  border: 1px solid black;
  text-align: center;
  float: left;
}

.my_div img {
  width: 100%;
  height: 200px;
}
</style>
```

在App.vue中使用

```vue
<template>
  <div>
    <Dog></Dog>
    <Dog/>
  </div>
</template>

<script>
import Dog from '@/components/practise/Dog1'
export default {
  components: {
    Dog
  }
}
</script>

<style>

</style>
```

> 总结: 重复部分封装成组件, 然后注册使用

## 附加练习_2.点击文字变色

> 目标: 修改Dog组件, 实现组件内点击变色

提示: 文字在组件内, 所以事件和方法都该在组件内-独立

图示:

![10.3.1_组件_事件变量使用](https://gitee.com/XXXTENTWXD/pic/raw/master/images/10.3.1_%E7%BB%84%E4%BB%B6_%E4%BA%8B%E4%BB%B6%E5%8F%98%E9%87%8F%E4%BD%BF%E7%94%A8.gif)

正确代码(==先不要看==)

components/practise/Dog2.vue

```html
<template>
  <div class="my_div">
    <img
      src="https://scpic.chinaz.net/files/pic/pic9/202003/zzpic23514.jpg"
      alt=""
    />
    <p :style="{backgroundColor: colorStr}" @click="btn">这是一个孤独可怜的狗</p>
  </div>
</template>

<script>
export default {
  data(){
    return {
      colorStr: ""
    }
  },
  methods: {
    btn(){
      this.colorStr = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)}, ${Math.floor(Math.random() * 256)})`
    }
  }
};
</script>

<style>
.my_div {
  width: 200px;
  border: 1px solid black;
  text-align: center;
  float: left;
}

.my_div img {
  width: 100%;
  height: 200px;
}
</style>
```

## 附加练习_3.卖狗啦

> 目标: 把数据循环用组件显示铺设

数据:

```js
[
    {
        dogImgUrl:
        "http://nwzimg.wezhan.cn/contents/sitefiles2029/10146688/images/21129958.jpg",
        dogName: "博美",
    },
    {
        dogImgUrl:
        "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1224576619,1307855467&fm=26&gp=0.jpg",
        dogName: "泰迪",
    },
    {
        dogImgUrl:
        "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2967740259,1518632757&fm=26&gp=0.jpg",
        dogName: "金毛",
    },
    {
        dogImgUrl:
        "https://pic1.zhimg.com/80/v2-7ba4342e6fedb9c5f3726eb0888867da_1440w.jpg?source=1940ef5c",
        dogName: "哈士奇",
    },
    {
        dogImgUrl:
        "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813435580&di=946902d419c3643e33a0c9113fc8d780&imgtype=0&src=http%3A%2F%2Fvpic.video.qq.com%2F3388556%2Fd0522aynh3x_ori_3.jpg",
        dogName: "阿拉斯加",
    },
    {
        dogImgUrl:
        "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813454815&di=ecdd2ebf479568453d704dffacdfa12c&imgtype=0&src=http%3A%2F%2Fwww.officedoyen.com%2Fuploads%2Fallimg%2F150408%2F1-15040Q10J5B0.jpg",
        dogName: "萨摩耶",
    },
]
```

图示:

![image-20210115112811452](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210115112811452.png)

正确代码(==不可复制==)

components/practise/Dog3.vue

```vue
<template>
  <div class="my_div">
    <img :src="imgurl" alt="" />
    <p :style="{ backgroundColor: colorStr }" @click="btn">{{ dogname }}</p>
  </div>
</template>

<script>
export default {
  props: ["imgurl", "dogname"],
  data() {
    return {
      colorStr: "",
    };
  },
  methods: {
    btn() {
      this.colorStr = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(
        Math.random() * 256
      )}, ${Math.floor(Math.random() * 256)})`;

      
    },
  },
};
</script>

<style scoped>
.my_div {
  width: 200px;
  border: 1px solid black;
  text-align: center;
  float: left;
}

.my_div img {
  width: 100%;
  height: 200px;
}
</style>
```

App.vue引入使用把数据循环传给组件显示

```vue
<template>
  <div>
    <Dog v-for="(obj, index) in arr"
    :key="index"
    :imgurl="obj.dogImgUrl"
    :dogname="obj.dogName"
    ></Dog>
  </div>
</template>

<script>
import Dog from '@/components/practise/Dog3'
export default {
  data() {
    return {
      // 1. 准备数据
      arr: [
        {
          dogImgUrl:
            "http://nwzimg.wezhan.cn/contents/sitefiles2029/10146688/images/21129958.jpg",
          dogName: "博美",
        },
        {
          dogImgUrl:
            "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1224576619,1307855467&fm=26&gp=0.jpg",
          dogName: "泰迪",
        },
        {
          dogImgUrl:
            "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2967740259,1518632757&fm=26&gp=0.jpg",
          dogName: "金毛",
        },
        {
          dogImgUrl:
            "https://pic1.zhimg.com/80/v2-7ba4342e6fedb9c5f3726eb0888867da_1440w.jpg?source=1940ef5c",
          dogName: "哈士奇",
        },
        {
          dogImgUrl:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813435580&di=946902d419c3643e33a0c9113fc8d780&imgtype=0&src=http%3A%2F%2Fvpic.video.qq.com%2F3388556%2Fd0522aynh3x_ori_3.jpg",
          dogName: "阿拉斯加",
        },
        {
          dogImgUrl:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813454815&di=ecdd2ebf479568453d704dffacdfa12c&imgtype=0&src=http%3A%2F%2Fwww.officedoyen.com%2Fuploads%2Fallimg%2F150408%2F1-15040Q10J5B0.jpg",
          dogName: "萨摩耶",
        },
      ],
    };
  },
  components: {
    Dog
  }
};
</script>
```

## 附加练习_4.选择喜欢的狗

> 目标: 用户点击狗狗的名字, 在右侧列表显示一次名字

效果:

![11.5_喜欢的狗狗](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/11.5_%E5%96%9C%E6%AC%A2%E7%9A%84%E7%8B%97%E7%8B%97.gif)

正确代码(==不可复制==)

components/practise/Dog4.vue

```vue
<template>
  <div class="my_div">
    <img :src="imgurl" alt="" />
    <p :style="{ backgroundColor: colorStr }" @click="btn">{{ dogname }}</p>
  </div>
</template>

<script>
export default {
  props: ["imgurl", "dogname"],
  data() {
    return {
      colorStr: "",
    };
  },
  methods: {
    btn() {
      this.colorStr = `rgb(${Math.floor(Math.random() * 256)}, ${Math.floor(
        Math.random() * 256
      )}, ${Math.floor(Math.random() * 256)})`;

      // 补充: 触发父级事件
      this.$emit("love", this.dogname);
    },
  },
};
</script>

<style scoped>
.my_div {
  width: 200px;
  border: 1px solid black;
  text-align: center;
  float: left;
}

.my_div img {
  width: 100%;
  height: 200px;
}
</style>
```

App.vue

```vue
<template>
  <div>
    <Dog
      v-for="(obj, index) in arr"
      :key="index"
      :imgurl="obj.dogImgUrl"
      :dogname="obj.dogName"
      @love="fn"
    ></Dog>

    <hr />
    <p>显示喜欢的狗:</p>
    <ul>
      <li v-for="(item, index) in loveArr" :key="index">{{ item }}</li>
    </ul>
  </div>
</template>

<script>
import Dog from "@/components/practise/Dog4";
export default {
  data() {
    return {
      // 1. 准备数据
      arr: [
        {
          dogImgUrl:
            "http://nwzimg.wezhan.cn/contents/sitefiles2029/10146688/images/21129958.jpg",
          dogName: "博美",
        },
        {
          dogImgUrl:
            "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=1224576619,1307855467&fm=26&gp=0.jpg",
          dogName: "泰迪",
        },
        {
          dogImgUrl:
            "https://ss2.bdstatic.com/70cFvnSh_Q1YnxGkpoWK1HF6hhy/it/u=2967740259,1518632757&fm=26&gp=0.jpg",
          dogName: "金毛",
        },
        {
          dogImgUrl:
            "https://pic1.zhimg.com/80/v2-7ba4342e6fedb9c5f3726eb0888867da_1440w.jpg?source=1940ef5c",
          dogName: "哈士奇",
        },
        {
          dogImgUrl:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813435580&di=946902d419c3643e33a0c9113fc8d780&imgtype=0&src=http%3A%2F%2Fvpic.video.qq.com%2F3388556%2Fd0522aynh3x_ori_3.jpg",
          dogName: "阿拉斯加",
        },
        {
          dogImgUrl:
            "https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1563813454815&di=ecdd2ebf479568453d704dffacdfa12c&imgtype=0&src=http%3A%2F%2Fwww.officedoyen.com%2Fuploads%2Fallimg%2F150408%2F1-15040Q10J5B0.jpg",
          dogName: "萨摩耶",
        },
      ],
      loveArr: []
    };
  },
  components: {
    Dog,
  },
  methods: {
    fn(dogName) {
      this.loveArr.push(dogName)
    },
  },
};
</script>

<style >
</style>
```

## 附加练习_5.卖完了

> 目标: 完成图示的卖完了效果

需求: 

* 如果为0了后面显示卖光了!!!
* 如果库存有值, 后面就不显示卖光了!!!
* 如果库存有值, 累计商品总数量

要求: 一行是一个组件进行复用, 这里要求必须用table>tr (也就是封装tr组件)

组件使用注意: html正常解析, table>tr或者select>option, 虽然vue渲染页面可以自定义, 但是还需要遵循浏览器的标签关系

* table>tr中不能直接使用组件, 需要在tr的is属性指定组件名
* select>option 也不能封装options组件, 需要在option的is属性指定组件名

效果演示:![11.7_课上练习](https://gitee.com/XXXTENTWXD/pic/raw/master/images/11.7_%E8%AF%BE%E4%B8%8A%E7%BB%83%E4%B9%A0.gif)

vue实例data里的数组如下

```js
goodsArr: [
    {
        count: 0,
        goodsName: "Watermelon"
    }, {
        count: 0,
        goodsName: "Banana"
    }, {
        count: 0,
        goodsName: "Orange"
    }, {
        count: 0,
        goodsName: "Pineapple"
    }, {
        count: 0,
        goodsName: "Strawberry"
    }
]
```

正确代码(不可复制)

components/practise/MyTr.vue

```html
<template>
  <tr>
    <td>
      <input type="number" v-model.number="obj['count']"/>
    </td>
    <td>
      <span>{{ obj["goodsName"] }}</span>
    </td>
    <td>
      <span v-show="obj['count'] == 0">卖光了!!!</span>
    </td>
  </tr>
</template>

<script>
export default {
    // 传入对象有风险, 但是如果是一对一关系可以传入对象-直接修改对象里的值影响外部效果
    props: ["obj"]
};
</script>

<style>
</style>
```

App.vue使用

```vue
<template>
  <div>
    <table>
      <!-- 2. 使用tr组件, 传入需要的数据 -->
      <tr
        is="myTr"
        v-for="(item, index) in goodsArr"
        :key="index"
        :obj="item"
        :index="index"
      ></tr>
    </table>
    <p>All Number:{{ sumNumber }}</p>
  </div>
</template>

<script>
import MyTr from '@/components/practise/MyTr'
export default {
  data() {
    return {
      goodsArr: [
        {
          count: 0,
          goodsName: "Watermelon",
        },
        {
          count: 0,
          goodsName: "Banana",
        },
        {
          count: 0,
          goodsName: "Orange",
        },
        {
          count: 0,
          goodsName: "Pineapple",
        },
        {
          count: 0,
          goodsName: "Strawberry",
        },
      ],
    };
  },
  components: {
    MyTr
  },
  computed: {
    sumNumber(){
      return this.goodsArr.reduce((sum, obj) => sum += obj.count * 1, 0)
    }
  }
};
</script>

<style>
</style>
```

## 附加练习_6.买点好吃的

> 目标: 商品列表显示一下, 然后封装组件实现增加减少功能并在最后统计总价

要求: 商品名, 增加 数量, 减少这一条封装成组件使用

效果演示:

![11.6_课上练习](https://gitee.com/XXXTENTWXD/pic/raw/master/images/11.6_%E8%AF%BE%E4%B8%8A%E7%BB%83%E4%B9%A0.gif)

数据:

```js
[
    {
        "shopName": "可比克薯片",
        "price": 5.5,
        "count": 0
    },
    {
        "shopName": "草莓酱",
        "price": 3.5,
        "count": 0
    },
    {
        "shopName": "红烧肉",
        "price": 55,
        "count": 0
    },
    {
        "shopName": "方便面",
        "price": 12,
        "count": 0
    }
]
```

正确代码(==不可复制==)

components/practise/Food.vue

```html
<template>
  <div>
    <span>{{ goodsname }}</span>
    <button @click="add(ind)">+</button>
    <span> {{ count }} </span>
    <button @click="sec(ind)">-</button>
  </div>
</template>

<script>
export default {
    props: ['goodsname', 'ind', 'count'], // 商品名,索引,数量
    methods: {
        add(ind){
            this.$emit('addE', ind)
        },
        sec(ind){
            this.$emit("secE", ind)
        }
    }
};
</script>
```

App.vue

```vue
<template>
  <div>
    <p>商品清单如下:</p>
    <div v-for="(obj, index) in shopData" :key="index">
      {{ obj.shopName }} -- {{ obj.price }}元/份
    </div>
    <p>请选择购买数量:</p>
    <Food
      v-for="(obj, index) in shopData"
      :key="index + ' '"
      :goodsname="obj.shopName"
      :ind="index"
      :count="obj.count"
      @addE="addFn"
      @secE="secFn"
    >
    </Food>
    <p>总价为: {{ allPrice }}</p>
  </div>
</template>

<script>
import Food from "@/components/practise/Food";
export default {
  data() {
    return {
      // 商品数据
      shopData: [
        {
          shopName: "可比克薯片",
          price: 5.5,
          count: 0,
        },
        {
          shopName: "草莓酱",
          price: 3.5,
          count: 0,
        },
        {
          shopName: "红烧肉",
          price: 55,
          count: 0,
        },
        {
          shopName: "方便面",
          price: 12,
          count: 0,
        },
      ],
    };
  },
  components: {
    Food,
  },
  methods: {
    addFn(ind){
      this.shopData[ind].count++
    },
    secFn(ind){
      this.shopData[ind].count > 0 && this.shopData[ind].count--
    }
  },
  computed: {
    allPrice(){
      return this.shopData.reduce((sum, obj) => sum += obj.count * obj.price, 0)
    }
  }
};
</script>
```

## 今日作业

==课上练习和案例主要==

### 购物车

目的: 把一行tr封装成一个组件, 然后v-for循环复用传值

> 提示: 对象类型传入到子组件, 内部修改也会相应外部这个对象 (对象是引用关系哦)

![image-20210115195904519](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/image-20210115195904519.png)

不带vue代码的标签结构(==可以复制==)接着写

```vue
<template>
  <div>
    <table
      border="1"
      width="700"
      style="border-collapse: collapse"
    >
      <caption>
        购物车
      </caption>
      <thead>
        <tr>
          <th>
            <input type="checkbox" /> <span>全选</span>
          </th>
          <th>名称</th>
          <th>价格</th>
          <th>数量</th>
          <th>总价</th>
          <th>操作</th>
        </tr>
      </thead>
      <tbody>
        
      </tbody>
      <tfoot>
        <tr>
          <td>合计:</td>
          <td colspan="5">
            
          </td>
        </tr>
      </tfoot>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      goodList: [
        {
          name: "诸葛亮",
          price: 1000,
          num: 1,
          checked: false,
        },
        {
          name: "蔡文姬",
          price: 1500,
          num: 1,
          checked: false,
        },
        {
          name: "妲己",
          price: 2000,
          num: 1,
          checked: false,
        },
        {
          name: "鲁班",
          price: 2200,
          num: 1,
          checked: false,
        },
      ],
    };
  },
};
</script>

<style>
</style>
```

### 做数学题

目的: 随机产生数学题, 输入答案提交后, 在下面对应序号显示结果

> 数字输入框按钮是一个组件, 下面每个序号和提示是一个组件

图示:

![Day04_作业_数学题](https://cdn.jsdelivr.net/gh/Young-Allen/pic@main/Day04_%E4%BD%9C%E4%B8%9A_%E6%95%B0%E5%AD%A6%E9%A2%98.gif)

Subject.vue - 题目一行组件 (样式和标签)(==可以复制接着写==)

```vue
<template>
  <div class="subject">
    <span></span>
    <span>+</span>
    <span></span>
    <span>=</span>
    <input type="number" />
    <button>提交</button>
  </div>
</template>

<script>
export default {

};
</script>

<style scoped>
.subject {
  margin: 5px;
  padding: 5px;
  font-size: 20px;
}
.subject span {
  display: inline-block;
  text-align: center;
  width: 20px;
}
.subject input {
  width: 50px;
  height: 20px;
}
</style>
```

Flag.vue - 下面结果一条的组件 (复制标签和样式)(==可以复制==)

```vue
<template>
  <span >1: 未完成</span>
</template>

<script>
export default {

};
</script>

<style scoped>
.right {
  color: green;
}
.error {
  color: red;
}
.undo {
  color: #ccc;
}
</style>
```

App.vue - 复制标签和样式

无vue代码的标签(==可以复制==)

```html
<template>
  <div id="app">
    <h2>测试题</h2>
    <subject ></subject>
    <div>
      <flag></flag>
    </div>
  </div>
</template>

<script>

export default {
  
};
</script>

<style>
body {
  background-color: #eee;
}

#app {
  background-color: #fff;
  width: 500px;
  margin: 50px auto;
  box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
  padding: 2em;
}
</style>
```



# 第六章 生命周期、axios、组价进阶

## 知识点自测

- [ ] 知道window.onload的作用
- [ ] ajax是什么, XMLHttpRequest的使用步骤
- [ ] jQ的 $.ajax底层是什么?
- [ ] Promise的作用
- [ ] async和await的作用-如何和Promise配合
- [ ] 同步异步的概念, 代码执行顺序
- [ ] 请求和响应, 以及JSON解析能力
- [ ] Vue基础, 组件使用, props传值, 组件通信, 计算属性使用, 对象引用类型使用

## 今日学习目标

1. 能够说出vue组件生命周期
2. 能够掌握axios的使用
3. 能够了解$refs, $nextTick使用和name使用
4. 能够完成购物车案例开发

## 1. vue生命周期

### 1.0_人的-生命周期

> 一组件从 创建 到 销毁 的整个过程就是生命周期

Vue_生命周期

![image-20210511152835915](F:\前端\05、阶段五 Vue.js项目实战开发\资料\webpack+Vue基础课程资料\Day05_生命周期_axios_购物车案例\01_笔记和ppt\images\image-20210511152835915.png)

### 1.1_钩子函数

> 目标: **Vue** 框架内置函数，随着组件的生命周期阶段，自动执行

作用: 特定的时间点，执行特定的操作

场景: 组件创建完毕后，可以在created 生命周期函数中发起Ajax 请求，从而初始化 data 数据

分类: 4大阶段8个方法

- 初始化
- 挂载
- 更新
- 销毁

| **阶段** | **方法名**    | **方法名** |
| -------- | ------------- | ---------- |
| 初始化   | beforeCreate  | created    |
| 挂载     | beforeMount   | mounted    |
| 更新     | beforeUpdate  | updated    |
| 销毁     | beforeDestroy | destroyed  |

[官网文档](https://cn.vuejs.org/v2/guide/instance.html#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE%E7%A4%BA)

下图展示了实例的生命周期。你不需要立马弄明白所有的东西，不过随着你的不断学习和使用，它的参考价值会越来越高。

![Day03](https://gitee.com/XXXTENTWXD/pic/raw/master/images/Day03.png)

### 1.2_初始化阶段

> 目标: 掌握初始化阶段2个钩子函数作用和执行时机

含义讲解:

1.new Vue() – Vue实例化(组件也是一个小的Vue实例)

2.Init Events & Lifecycle – 初始化事件和生命周期函数

3.beforeCreate – 生命周期钩子函数被执行

4.Init injections&reactivity – Vue内部添加data和methods等

5.created – 生命周期钩子函数被执行, 实例创建

6.接下来是编译模板阶段 –开始分析

7.Has el option? – 是否有el选项 – 检查要挂到哪里

​	没有. 调用$mount()方法

​	有, 继续检查template选项

![image-20210511153050932](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511153050932.png)



components/Life.vue - 创建一个文件

```html
<script>
export default {
    data(){
        return {
            msg: "hello, Vue"
        }
    },
    // 一. 初始化
    // new Vue()以后, vue内部给实例对象添加了一些属性和方法, data和methods初始化"之前"
    beforeCreate(){
        console.log("beforeCreate -- 执行");
        console.log(this.msg); // undefined
    },
    // data和methods初始化以后
    // 场景: 网络请求, 注册全局事件
    created(){
        console.log("created -- 执行");
        console.log(this.msg); // hello, Vue

        this.timer = setInterval(() => {
            console.log("哈哈哈");
        }, 1000)
    }
}
</script>
```

App.vue - 引入使用

```vue
<template>
  <div>
    <h1>1. 生命周期</h1>
 	<Life></Life>
  </div>
</template>

<script>
import Life from './components/Life'
export default {
  components: {
    Life
  }
}
</script>
```

### 1.3_挂载阶段

> 目标: 掌握挂载阶段2个钩子函数作用和执行时机

含义讲解:

1.template选项检查

​	有 - 编译template返回render渲染函数

​	无 – 编译el选项对应标签作为template(要渲染的模板)

2.虚拟DOM挂载成真实DOM之前

3.beforeMount – 生命周期钩子函数被执行

4.Create … – 把虚拟DOM和渲染的数据一并挂到真实DOM上

5.真实DOM挂载完毕

6.mounted – 生命周期钩子函数被执行

![image-20210511153649298](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511153649298.png)



components/Life.vue - 创建一个文件

```html
<template>
  <div>
      <p>学习生命周期 - 看控制台打印</p>
      <p id="myP">{{ msg }}</p>
  </div>
</template>

<script>
export default {
    // ...省略其他代码
    
    // 二. 挂载
    // 真实DOM挂载之前
    // 场景: 预处理data, 不会触发updated钩子函数
    beforeMount(){
        console.log("beforeMount -- 执行");
        console.log(document.getElementById("myP")); // null

        this.msg = "重新值"
    },
    // 真实DOM挂载以后
    // 场景: 挂载后真实DOM
    mounted(){
        console.log("mounted -- 执行");
        console.log(document.getElementById("myP")); // p
    }
}
</script>
```

### 1.4_更新阶段

> 目标: 掌握更新阶段2个钩子函数作用和执行时机

含义讲解:

1.当data里数据改变, 更新DOM之前

2.beforeUpdate – 生命周期钩子函数被执行

3.Virtual DOM…… – 虚拟DOM重新渲染, 打补丁到真实DOM

4.updated – 生命周期钩子函数被执行

5.当有data数据改变 – 重复这个循环

![image-20210511154016777](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511154016777.png)

components/Life.vue - 创建一个文件

准备ul+li循环, 按钮添加元素, 触发data改变->导致更新周期开始

```html
<template>
  <div>
      <p>学习生命周期 - 看控制台打印</p>
      <p id="myP">{{ msg }}</p>
      <ul id="myUL">
          <li v-for="(val, index) in arr" :key="index">
              {{ val }}
          </li>
      </ul>
      <button @click="arr.push(1000)">点击末尾加值</button>
  </div>
</template>

<script>
export default {
    data(){
        return {
            msg: "hello, Vue",
            arr: [5, 8, 2, 1]
        }
    },
    // ...省略其他代码

    // 三. 更新
    // 前提: data数据改变才执行
    // 更新之前
    beforeUpdate(){
        console.log("beforeUpdate -- 执行");
        console.log(document.querySelectorAll("#myUL>li")[4]); // undefined
    },
    // 更新之后
    // 场景: 获取更新后的真实DOM
    updated(){
        console.log("updated -- 执行");
        console.log(document.querySelectorAll("#myUL>li")[4]); // li
    }
}
</script>
```

### 1.5_销毁阶段

> 目标: 掌握销毁阶段2个钩子函数作用和执行时机

含义讲解:

1.当$destroy()被调用 – 比如组件DOM被移除(例v-if)

2.beforeDestroy – 生命周期钩子函数被执行

3.拆卸数据监视器、子组件和事件侦听器

4.实例销毁后, 最后触发一个钩子函数

5.destroyed – 生命周期钩子函数被执行

![image-20210511154330252](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511154330252.png)



components/Life.vue - 准备生命周期方法(Life组件即将要被删除)

```html
<script>
export default {
    // ...省略其他代码
    
    // 四. 销毁
    // 前提: v-if="false" 销毁Vue实例
    // 场景: 移除全局事件, 移除当前组件, 计时器, 定时器, eventBus移除事件$off方法
    beforeDestroy(){
        // console.log('beforeDestroy -- 执行');
        clearInterval(this.timer)
    },
    destroyed(){
        // console.log("destroyed -- 执行");
    }
}
</script>
```

主要: App.vue - 点击按钮让Life组件从DOM上移除 -> 导致Life组件进入销毁阶段

```vue
<Life v-if="show"></Life>
<button @click="show = false">销毁组件</button>

<script>
    data(){
        return {
            show: true
        }
    },
</script>
```

## 2. axios

### 2.0_axios基本使用

[axios文档](http://www.axios-js.com/)

特点

* 支持客户端发送Ajax请求
* 支持服务端Node.js发送请求
* 支持Promise相关用法
* 支持请求和响应的拦截器功能
* 自动转换JSON数据
* axios 底层还是原生js实现, 内部通过Promise封装的

axios的基本使用

```js
axios({
  method: '请求方式', // get post
  url: '请求地址',
  data: {    // 拼接到请求体的参数,  post请求的参数
    xxx: xxx,
  },
  params: {  // 拼接到请求行的参数, get请求的参数
   	xxx: xxx 
  }
}).then(res => {
  console.log(res.data) // 后台返回的结果
}).catch(err => {
  console.log(err) // 后台报错返回
})

```

### 2.1_axios基本使用-获取数据

> 目标: 调用文档最后_获取所有图书信息接口

功能: 点击调用后台接口, 拿到所有数据 – 打印到控制台

接口: 参考预习资料.md – 接口文档

引入: 下载axios, 引入后才能使用

效果:

![image-20210511154911824](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511154911824.png)

例子如下:

components/UseAxios.vue

```vue
<template>
  <div>
    <p>1. 获取所有图书信息</p>
    <button @click="getAllFn">点击-查看控制台</button>
  </div>
</template>

<script>
// 目标1: 获取所有图书信息
// 1. 下载axios
// 2. 引入axios
// 3. 发起axios请求
import axios from "axios";
export default {
  methods: {
    getAllFn() {
      axios({
        url: "http://123.57.109.30:3006/api/getbooks",
        method: "GET", // 默认就是GET方式请求, 可以省略不写
      }).then((res) => {
        console.log(res);
      });
      // axios()-原地得到Promise对象
    },
  }
};
</script>
```

### 2.2_axios基本使用-传参

> 目标: 调用接口-获取某本书籍信息

功能: 点击调用后台接口, 查询用户想要的书籍信息 – 打印到控制台

接口: 参考预习资料.md – 接口文档

效果:

![image-20210511160538891](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511160538891.png)

例子如下:

components/UseAxios.vue

```vue
<template>
  <div>
    <p>2. 查询某本书籍信息</p>
    <input type="text" placeholder="请输入要查询 的书名" v-model="bName" />
    <button @click="findFn">查询</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      bName: ""
    };
  },
  methods: {
    // ...省略了查询所有的代码
    findFn() {
      axios({
        url: "/api/getbooks",
        method: "GET",
        params: { // 都会axios最终拼接到url?后面
            bookname: this.bName
        }
      }).then(res => {
          console.log(res);
      })
    }
  },
};
</script>
```

### 2.3_axios基本使用-发布书籍

> 目标: 完成发布书籍功能

功能: 点击新增按钮, 把用户输入的书籍信息, 传递给后台 – 把结果打印在控制台

接口: 参考预习资料.md – 接口文档

效果:

![image-20210511161239034](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511161239034.png)

例子如下:

components/UseAxios.vue

```vue
<template>
  <div>
    <p>3. 新增图书信息</p>
    <div>
        <input type="text" placeholder="书名" v-model="bookObj.bookname">
    </div>
    <div>
        <input type="text" placeholder="作者" v-model="bookObj.author">
    </div>
    <div>
        <input type="text" placeholder="出版社" v-model="bookObj.publisher">
    </div>
    <button @click="sendFn">发布</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      bName: "",
      bookObj: { // 参数名提前和后台的参数名对上-发送请求就不用再次对接了
          bookname: "",
          author: "",
          publisher: ""
      }
    };
  },
  methods: {
    // ...省略了其他代码
    sendFn(){
       axios({
           url: "/api/addbook",
           method: "POST",
           data: {
               appkey: "7250d3eb-18e1-41bc-8bb2-11483665535a",
               ...this.bookObj
            // 等同于下面
            // bookname: this.bookObj.bookname,
            // author: this.bookObj.author,
            // publisher: this.bookObj.publisher
           }
       }) 
    }
  },
};
</script>
```

### 2.4_axios基本使用-全局配置

> 目标: 避免前缀基地址, 暴露在逻辑页面里, 统一设置

```js
axios.defaults.baseURL = "http://123.57.109.30:3006"

// 所有请求的url前置可以去掉, 请求时, axios会自动拼接baseURL的地址在前面
getAllFn() {
    axios({
        url: "/api/getbooks",
        method: "GET", // 默认就是GET方式请求, 可以省略不写
    }).then((res) => {
        console.log(res);
    });
    // axios()-原地得到Promise对象
},
```

## 3. $nextTick和$refs知识

### 3.0 $refs-获取DOM

> 目标: 利用 ref 和 $refs 可以用于获取 dom 元素

components/More.vue

```vue
<template>
  <div>
      <p>1. 获取原生DOM元素</p>
      <h1 id="h" ref="myH">我是一个孤独可怜又能吃的h1</h1>
  </div>
</template>

<script>
// 目标: 获取组件对象
// 1. 创建组件/引入组件/注册组件/使用组件
// 2. 组件起别名ref
// 3. 恰当时机, 获取组件对象
export default {
    mounted(){
        console.log(document.getElementById("h")); // h1
        console.log(this.$refs.myH); // h1
    }
}
</script>

<style>

</style>
```

> 总结: 通过id / ref, 都可以获取原生DOM标签

### 3.1 $refs-获取组件对象

> 目标: 获取组件对象, 调用组件里方法

components/Child/Demo.vue

```vue
<template>
  <div>
      <p>我是Demo组件</p>
  </div>
</template>

<script>
export default {
    methods: {
        fn(){
            console.log("demo组件内的方法被调用了");
        }
    }
}
</script>
```

More.vue - 获取组件对象 - 调用组件方法

```vue
<template>
  <div>
      <p>1. 获取原生DOM元素</p>
      <h1 id="h" ref="myH">我是一个孤独可怜又能吃的h1</h1>
      <p>2. 获取组件对象 - 可调用组件内一切</p>
      <Demo ref="de"></Demo>
  </div>
</template>

<script>
// 目标: 获取组件对象
// 1. 创建组件/引入组件/注册组件/使用组件
// 2. 组件起别名ref
// 3. 恰当时机, 获取组件对象
import Demo from './Child/Demo'
export default {
    mounted(){
        console.log(document.getElementById("h")); // h1
        console.log(this.$refs.myH); // h1

        let demoObj = this.$refs.de;
        demoObj.fn()
    },
    components: {
        Demo
    }
}
</script>
```

> 总结: ref定义值, 通过$refs.值 来获取组件对象, 就能继续调用组件内的变量

### 3.2 $nextTick使用

> #### Vue更新DOM-异步的

> 目标: 点击count++, 马上通过"原生DOM"拿标签内容, 无法拿到新值

components/Move.vue - 继续新增第三套代码

```vue
<template>
  <div>
      <p>1. 获取原生DOM元素</p>
      <h1 id="h" ref="myH">我是一个孤独可怜又能吃的h1</h1>
      <p>2. 获取组件对象 - 可调用组件内一切</p>
      <Demo ref="de"></Demo>
      <p>3. vue更新DOM是异步的</p>
      <p ref="myP">{{ count }}</p>
      <button @click="btn">点击count+1, 马上提取p标签内容</button>
  </div>
</template>

<script>
// 目标: 获取组件对象
// 1. 创建组件/引入组件/注册组件/使用组件
// 2. 组件起别名ref
// 3. 恰当时机, 获取组件对象
import Demo from './Child/Demo'
export default {
    mounted(){
        console.log(document.getElementById("h")); // h1
        console.log(this.$refs.myH); // h1

        let demoObj = this.$refs.de;
        demoObj.fn()
    },
    components: {
        Demo
    },
    data(){
        return {
            count: 0
        }
    },
    methods: {
        btn(){
            this.count++; // vue监测数据更新, 开启一个DOM更新队列(异步任务)
            console.log(this.$refs.myP.innerHTML); // 0

            // 原因: Vue更新DOM异步
            // 解决: this.$nextTick()
            // 过程: DOM更新完会挨个触发$nextTick里的函数体
             this.$nextTick(() => {
                console.log(this.$refs.myP.innerHTML); // 1
            })
        }
    }
}
</script>
```

> 总结: 因为DOM更新是异步的

### 3.3 $nextTick使用场景

> 目标: 点击搜索按钮, 弹出聚焦的输入框, 按钮消失

![$nextTick使用](https://gitee.com/XXXTENTWXD/pic/raw/master/images/$nextTick%E4%BD%BF%E7%94%A8.gif)

components/Tick.vue

```vue
<template>
  <div>
      <input ref="myInp" type="text" placeholder="这是一个输入框" v-if="isShow">
      <button v-else @click="btn">点击我进行搜索</button>
  </div>
</template>

<script>
// 目标: 点按钮(消失) - 输入框出现并聚焦
// 1. 获取到输入框
// 2. 输入框调用事件方法focus()达到聚焦行为
export default {
    data(){
        return {
            isShow: false
        }
    },
    methods: {
        async btn(){
            this.isShow = true;
            // this.$refs.myInp.focus()
            // 原因: data变化更新DOM是异步的
            // 输入框还没有挂载到真实DOM上
            // 解决:
            // this.$nextTick(() => {
            //     this.$refs.myInp.focus()
            // })
            // 扩展: await取代回调函数
            // $nextTick()原地返回Promise对象
            await this.$nextTick()
            this.$refs.myInp.focus()
        }
    }
}
</script>
```

### 3.4 组件name属性使用

> 目标: 可以用组件的name属性值, 来注册组件名字

问题: 组件名不是可以随便写的?

答案: 我们封装的组件-可以自己定义name属性组件名-让使用者有个统一的前缀风格

components/Com.vue

```vue
<template>
  <div>
      <p>我是一个Com组件</p>
  </div>
</template>

<script>
export default {
    name: "ComNameHaHa" // 注册时可以定义自己的名字
}
</script>
```

App.vue - 注册和使用

```vue
<template>
  <div>
    <h1>1. 生命周期</h1>
    <Life v-if="show"></Life>
    <button @click="show = false">销毁组件</button>
    <hr>
    <h1>2. axios使用</h1>
    <UseAxios></UseAxios>
    <hr>
    <h1>3. $refs的使用</h1>
    <More></More>
    <hr>
    <h1>4. $nextTick使用场景</h1>
    <Tick></Tick>
    <hr>
    <h1>5. 组件对象里name属性</h1>
    <ComNameHaHa></ComNameHaHa>
  </div>
</template>

<script>
import Life from './components/Life'
import UseAxios from './components/UseAxios'
import More from './components/More'
import Tick from './components/Tick'
import Com from './components/Com'
export default {
  data(){
    return {
      show: true
    }
  },
  components: {
    Life,
    UseAxios,
    More,
    Tick,
    [Com.name]: Com // 对象里的key是变量的话[]属性名表达式
    // "ComNameHaHa": Com
  }
}
</script>
```

## 4. 案例 - 购物车

![购物车全效果](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E8%B4%AD%E7%89%A9%E8%BD%A6%E5%85%A8%E6%95%88%E6%9E%9C.gif)

### 4.0 案例-购物车-项目初始化

> 目标: 初始化新项目, 清空不要的东西, 下载bootstrap库, 下载less模块

```bash
vue create shopcar
yarn add bootstrap
yarn add less less-loader@5.0.0 -D
```

图示:

![image-20210307092110985](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210307092110985.png)

1. 按照需求, 把项目页面拆分成几个组件, 在components下创建

* MyHeader组件

* MyFooter组件
* MyGoods组件 - 商品
* MyCount组件

2. 然后引入到App.vue上注册

3. 在main.js中引入bootStrap库

```js
import "bootstrap/dist/css/bootstrap.css" // 引入第三方包里的某个css文件
```

MyHeader.vue

```vue
<template>
  <div class="my-header">购物车案例</div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
  .my-header {
    height: 45px;
    line-height: 45px;
    text-align: center;
    background-color: #1d7bff;
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 2;
  }
</style>
```

MyGoods.vue

```vue
<template>
  <div class="my-goods-item">
    <div class="left">
      <div class="custom-control custom-checkbox">
        <input type="checkbox" class="custom-control-input" id="input"
        >
        <label class="custom-control-label" for="input">
          <img src="http://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg" alt="">
        </label>
      </div>
    </div>
    <div class="right">
      <div class="top">商品名字</div>
      <div class="bottom">
        <span class="price">¥ 100</span>
        <span>
            数量组件
        </span>
      </div>
    </div>
  </div>
</template>

<script>
export default {

}
</script>

<style lang="less" scoped>
.my-goods-item {
  display: flex;
  padding: 10px;
  border-bottom: 1px solid #ccc;
  .left {
    img {
      width: 120px;
      height: 120px;
      margin-right: 8px;
      border-radius: 10px;
    }
    .custom-control-label::before,
    .custom-control-label::after {
      top: 50px;
    }
  }
  .right {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    .top{
        font-size: 14px;
        font-weight: 700;
    }
    .bottom {
      display: flex;
      justify-content: space-between;
      padding: 5px 0;
      align-items: center;
      .price {
        color: red;
        font-weight: bold;
      }
    }
  }
}

</style>
```

> 目标: 完成商品组件右下角商品组件的开发

![image-20220923191603445](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220923191603445.png)

components/MyCount.vue

```vue
<template>
  <div class="my-counter">
    <button type="button" class="btn btn-light" >-</button>
    <input type="number" class="form-control inp" >
    <button type="button" class="btn btn-light">+</button>
  </div>
</template>

<script>
export default {
}
</script>

<style lang="less" scoped>
.my-counter {
  display: flex;
  .inp {
    width: 45px;
    text-align: center;
    margin: 0 10px;
  }
  .btn, .inp{
    transform: scale(0.9);
  }
}
</style>
```

components/MyFooter.vue

```vue
<template>
  <!-- 底部 -->
  <div class="my-footer">
    <!-- 全选 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="footerCheck">
      <label class="custom-control-label" for="footerCheck">全选</label>
    </div>
    <!-- 合计 -->
    <div>
      <span>合计:</span>
      <span class="price">¥ 0</span>
    </div>
    <!-- 按钮 -->
    <button type="button" class="footer-btn btn btn-primary">结算 ( 0 )</button>
  </div>
</template>

<script>
export default {
  
}
</script>

<style lang="less" scoped>
.my-footer {
  position: fixed;
  z-index: 2;
  bottom: 0;
  width: 100%;
  height: 50px;
  border-top: 1px solid #ccc;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
  background: #fff;

  .price {
    color: red;
    font-weight: bold;
    font-size: 15px;
  }
  .footer-btn {
    min-width: 80px;
    height: 30px;
    line-height: 30px;
    border-radius: 25px;
    padding: 0;
  }
}
</style>
```

### 4.1 案例-购物车-头部自定义

> 目的: 头部的标题, 颜色, 背景色可以随便修改, props类型的校验

思路

1. 在MyHeader.vue中准备props里变量, 然后使用
2. 在使用MyHeader.vue组件时, 传入相应的值 (color和backgroundColor)

MyHeader.vue

```vue
<template>
  <div class="my-header" :style="{backgroundColor: background, color}">{{ title }}</div>
</template>

<script>
// 目标: 让Header组件支持不同的项目 - 自定义
// 1. 分析哪些可以自定义 (背景色, 文字颜色, 文字内容)
// 2. (新) 可以对props的变量的值 进行校验
// 3. 内部使用props变量的值
// 4. 外部使用时, 遵守变量名作为属性名, 值的类型遵守
export default {
    props: {
        background: String, // 外部插入此变量的值, 必须是字符串类型, 否则报错
        color: {
            type: String, // 约束color值的类型
            default: "#fff" // color变量默认值(外部不给 我color传值, 使用默认值)
        },
        title: {
            type: String,
            required: true // 必须传入此变量的值
        }
    }
}
</script>

<style lang="less" scoped>
  .my-header {
    height: 45px;
    line-height: 45px;
    text-align: center;
    background-color: #1d7bff;
    color: #fff;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    z-index: 2;
  }
</style>
```

App.vue传入相应自定义的值

```vue
<MyHeader title="购物车案例"></MyHeader>
```

> 总结:  
>
> props: [] - 只能声明变量和接收, 不能类型校验
>
> props: {} - 声明变量和校验类型规则 - 外部传入值不对则报错

### 4.2 案例-购物车-请求数据

> 目标: 使用axios把数据请求回来

数据地址: https://www.escook.cn/api/cart  (get方式)

1. 下载axios

```vue
yarn add axios
```

2. main.js - 原型上挂载

```js
// 目标: 请求数据 - 打印
// 1. 下载axios库, main.js - 全局绑定属性 (确保任意.vue文件可以都访问到这个axios方法)
import axios from 'axios'
// 2. 基础地址
axios.defaults.baseURL = "https://www.escook.cn"
// 3. axios方法添加到Vue的原型上
Vue.prototype.$axios = axios

new Vue({
    render: h => h(App),
}).$mount('#app')

```

3. App.vue请求使用

```vue
<script>
export default {
  data(){
    return {
      list: [] // 商品所有数据
    }
  },
  created(){
    // 不必在自己引入axios变量, 而是直接使用全局属性$axios
    this.$axios({
      url: "/api/cart"
    }).then(res => {
      console.log(res);
      this.list = res.data.list
    })
  }
}
</script>
```

> 总结: 利用axios, 调用接口, 把数据请求回来

### 4.3 案例-购物车-数据渲染

> 目标: 把上面请求的数据, 铺设到页面上

App.vue

```vue
<MyGoods v-for="obj in list" 
         :key="obj.id"
         :gObj="obj"
></MyGoods>
```

MyGoods.vue

```vue
<template>
  <div class="my-goods-item">
    <div class="left">
      <div class="custom-control custom-checkbox">
        <!-- *重要:
          每个对象和组件都是独立的
          对象里的goods_state关联自己对应商品的复选框
         -->
         <!-- bug:
          循环的所有label的for都是input, id也都是input - 默认只有第一个生效
          解决: 每次对象里的id值(1, 2), 分别给id和for使用即可区分
          -->
        <input type="checkbox" class="custom-control-input" :id="gObj.id" v-model="gObj.goods_state">
        <label class="custom-control-label" :for="gObj.id">
          <img :src="gObj.goods_img" alt="">
        </label>
      </div>
    </div>
    <div class="right">
      <div class="top">{{ gObj.goods_name }}</div>
      <div class="bottom">
        <span class="price">¥ {{ gObj.goods_price }}</span>
        <span>
            <MyCount :obj="gObj"></MyCount>
        </span>
      </div>
    </div>
  </div>
</template>

<script>
import MyCount from './MyCount'
export default {
    props: {
      gObj: Object
    },
    components: {
        MyCount
    }
}
</script>
```

MyCount.vue

```vue
<template>
  <div class="my-counter">
    <button type="button" class="btn btn-light" >-</button>
    <input type="number" class="form-control inp" v-model.number="obj.goods_count">
    <button type="button" class="btn btn-light" >+</button>
  </div>
</template>

<script>
export default {
  props: {
    obj: Object // 商品对象
  }
}
</script>

```

> 总结: 把各个组件关联起来, 把数据都铺设到页面上

### 4.4 案例-购物车-商品选中

> 问题: 点击发现总是第一个被选中

原来id和for都是"input"

但是id是唯一的啊, 所以用数据的id来作为标签的id, 分别独立, 为了兼容label点击图片也能选中的效果

```vue
<input type="checkbox" class="custom-control-input" :id="gObj.id"
       v-model="gObj.goods_state"
       >
<label class="custom-control-label" :for="gObj.id">
    <img :src="gObj.goods_img" alt="">
</label>
```

> 总结: lable的for值对应input的id, 点击label就能让对应input处于激活

### 4.5 案例-购物车-数量控制

> 目标: 点击+和-或者直接修改输入框的值影响商品购买的数量

```vue
<template>
  <div class="my-counter">
    <button type="button" class="btn btn-light" :disabled="obj.goods_count === 1" @click="obj.goods_count > 1 && obj.goods_count--">-</button>
    <input type="number" class="form-control inp" v-model.number="obj.goods_count">
    <button type="button" class="btn btn-light" @click="obj.goods_count++">+</button>
  </div>
</template>

<script>
// 目标: 商品数量 - 控制
// 1. 外部传入数据对象
// 2. v-model关联对象的goods_count属性和输入框 (双向绑定)
// 3. 商品按钮 +和-, 商品数量最少1件
// 4. 侦听数量改变, 小于1, 直接强制覆盖1
export default {
  props: {
    obj: Object // 商品对象
  },
  // 因为数量控制要通过对象"互相引用的关系"来影响外面对象里的数量值, 所以最好传 对象进来
  watch: {
    obj: {
      deep: true,
      handler(){ // 拿到商品数量, 判断小于1, 强制修改成1
        if (this.obj.goods_count < 1) {
          this.obj.goods_count = 1
        }
      }
    }
  }
}
</script>
```



### 4.6 案例-购物车-全选功能

> 目标: 在底部组件上, 完成全选功能

![image-20220923191615671](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220923191615671.png)

思路: 

1. 点击获取它的选中状态
2. 同步给上面每个小选框 - 而小选框的选中状态又在数组里
3. 把数组传给MyFooter, 然后更新即可 - 因为对象都是引用关系的

MyFooter.vue

```vue
<template>
  <!-- 底部 -->
  <div class="my-footer">
    <!-- 全选 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="footerCheck" v-model="isAll">
      <label class="custom-control-label" for="footerCheck">全选</label>
    </div>
    <!-- 合计 -->
    <div>
      <span>合计:</span>
      <span class="price">¥ {{ allPrice }}</span>
    </div>
    <!-- 按钮 -->
    <button type="button" class="footer-btn btn btn-primary">结算 ( {{ allCount }} )</button>
  </div>
</template>

<script>
// 目标: 全选
// 1. v-model关联全选-复选框(v-model后变量计算属性)
// 2. 页面(视频层)v(true) -> 数据层(变量-) 计算属性(完整写法)
// 3. 把全选 true/false同步给所有小选框选中状态上

// 小选  -> 全选
// App.vue里list数组 -> MyFooter.vue
// isAll的get方法里, 统计状态影响全选框

// 目标: 总数量统计
// 1. allCount计算属性用 数组reduce+判断统计数量并返回

// 目标: 总价
// allPrice计算属性, 数组reduce+单价*数量, 判断选中, 才累加后返回

export default {
  props: {
    arr: Array
  },
  computed: {
    isAll: {
      set(val){ // val就是关联表单的值(true/false)
        this.$emit('changeAll', val)
      },
      get(){
        // 查找小选框关联的属性有没有不符合勾选的条件
        // 直接原地false
        return this.arr.every(obj => obj.goods_state === true)
      }
    },
    
    
  }
}
</script>
```

App.vue

```vue
<MyFooter @changeAll="allFn" :arr="list"></MyFooter>

<script>
methods: {
    allFn(bool){
      this.list.forEach(obj => obj.goods_state = bool)
      // 把MyFooter内的全选状态true/false同步给所有小选框的关联属性上
    }
  }
</script>
```

> 总结: 全选的v-model的值, 使用计算属性完整写法

### 4.7 案例-购物车-总数量

> 目标: 完成底部组件, 显示选中的商品的总数量

MyFooter.vue

```js
allCount(){
    return this.arr.reduce((sum, obj) => {
        if (obj.goods_state === true) { // 选中商品才累加数量
            sum += obj.goods_count;
        }
        return sum;
    }, 0)
},
```

> 总结: 对象之间是引用关系, 对象值改变, 所有用到的地方都跟着改变

### 4.8 案例-购物车-总价

> 目标: 完成选中商品计算价格

components/MyFooter.vue

```js
allPrice(){
      return this.arr.reduce((sum, obj) => {
        if (obj.goods_state){
          sum += obj.goods_count * obj.goods_price
        }
        return sum;
      }, 0)
    }
```

> 总结: 把数组传给了MyFooter组件, 统计总价

## 今日总结

vue的生命周期哪4个阶段, 哪8个方法

axios是什么, 底层是什么, 具体如何使用

axios返回的是什么, 如何接收结果

知道ref和$refs使用和作用以及场景

知道$nextTick的作用

跟着老师的视频完成购物车案例

## 面试题

### 1、Vue 的 nextTick 的原理是什么? （高薪常问）

​    \1. 为什么需要 nextTick ，Vue 是异步修改 DOM 的并且不鼓励开发者直接接触 DOM，但有时候业务需要必须对数据更改--刷新后的 DOM 做相应的处理，这时候就可以使用 Vue.nextTick(callback)这个 api 了。

​    \2. 理解原理前的准备 首先需要知道事件循环中宏任务和微任务这两个概念,常见的宏任务有 script, setTimeout, setInterval, setImmediate, I/O, UI rendering 常见的微任务有 process.nextTick(Nodejs),Promise.then(), MutationObserver;

​    \3. 理解 nextTick 的原理正是 vue 通过异步队列控制 DOM 更新和 nextTick 回调函数先后执行的方式。如果大家看过这部分的源码，会发现其中做了很多 isNative()的判断，因为这里还存在兼容性优雅降级的问题。可见 Vue 开发团队的深思熟虑，对性能的良苦用心。

### 2、vue生命周期总共分为几个阶段？（必会）

   Vue 实例从创建到销毁的过程，就是生命周期。也就是从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、卸载等一系列过程，我们称这是 Vue 的生命周期。

**1****）beforeCreate**

​    在实例初始化之后，数据观测 (data observer) 和 event/watcher 事件配置之前被调用。

**2****）created**

​    在实例创建完成后被立即调用。在这一步，实例已完成以下的配置：数据观测 (data observer)， 属性和方法的运算，watch/event 事件回调。然而，挂载阶段还没开始，$el 属性目前不可见。

**3****）beforeMount**

​    在挂载开始之前被调用：相关的 render 函数首次被调用。

**4****）mounted**

​    el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用该钩子。如果 root 实例挂载了一个文档内元素，当 mounted 被调用时 vm.$el 也在文档内。

**5****）beforeUpdate**

​    数据更新时调用，发生在虚拟 DOM 打补丁之前。这里适合在更新之前访问现有的 DOM，比如手动移除已添加的事件监听器。该钩子在服务器端渲染期间不被调用，因为只有初次渲染会在服务端进行。

**6****）updated**

​    由于数据更改导致的虚拟 DOM 重新渲染和打补丁，在这之后会调用该钩子。

**7****）activated**

​    keep-alive 组件激活时调用。该钩子在服务器端渲染期间不被调用。

**8****）deactivated**

​    keep-alive 组件停用时调用。该钩子在服务器端渲染期间不被调用。

**9****）beforeDestroy**

​    实例销毁之前调用。在这一步，实例仍然完全可用。该钩子在服务器端渲染期间不被调用。

**10****）destroyed**

​    Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。

**11****）errorCaptured（2.5.0+ 新增）**

​    当捕获一个来自子孙组件的错误时被调用。此钩子会收到三个参数：错误对象、发生错误的组件实例以及一个包含错误来源信息的字符串。此钩子可以返回 false 以阻止该错误继续向上传播。

### 3、第一次加载页面会触发哪几个钩子函数？（必会）

   当页面第一次页面加载时会触发 beforeCreate, created, beforeMount, mounted 这几个钩子函数

## 今日作业

把课上购物车再来一遍

## 接口文档

> axios请求接口使用

根域名: http://123.57.109.30:3006

### 案例 - 图书相关

#### 获取

* 请求方式:  GET
* 请求地址: 根域名/api/getbooks
* 请求参数: 

不传参获取所有默认书籍, 也可以选择传递下面任意1-多个参数, 获取指定的相关书籍信息

| 参数名称  | 参数类型 | 是否必选 | 参数说明 |
| --------- | -------- | -------- | -------- |
| id        | Number   | 否       | 图书Id   |
| bookname  | String   | 否       | 图书名称 |
| author    | String   | 否       | 作者     |
| publisher | String   | 否       | 出版社   |
| appkey    | String   | 否       | 个人ID   |

* 返回示例:

```json
{
  "status": 200, // 状态码200都代表完全成功 - 无任何意义随便写, 只是方便前端写判断条件
  "msg": "获取图书列表成功", // 后台返回的提示消息, 随便写, 只是方便前端直接打印提示消息
  "data": [ // 后台返回的数据
    { "id": 1, "bookname": "西游记", "author": "吴承恩", "publisher": "北京图书出版社" },
    { "id": 2, "bookname": "红楼梦", "author": "曹雪芹", "publisher": "上海图书出版社" },
    { "id": 3, "bookname": "三国演义", "author": "罗贯中", "publisher": "北京图书出版社" }
  ]
}

```

#### 添加

* 请求方式: POST
* 请求地址: 根域名/api/addbook
* 请求参数:

| 参数名称  | 参数类型 | 是否必选 | 参数说明                                          |
| --------- | -------- | -------- | ------------------------------------------------- |
| bookname  | String   | 是       | 图书名称                                          |
| author    | String   | 是       | 作者                                              |
| publisher | String   | 是       | 出版社                                            |
| appkey    | String   | 是       | 个人ID - 用'7250d3eb-18e1-41bc-8bb2-11483665535a' |

* 返回示例:

```js
{
    "status": 201, // 后台返回数据逻辑层的状态码, 201代表后台已经新增加了一个资源
    "data": {
        "author": "施大神"
        "bookname": "水浒传2"
        "id": 41
        "publisher": "未来出版社"
    }
    "msg": "添加图书成功"
}
```



# 第七章 动态组件、插槽、自定义指令

## 知识点自测

- [ ] ==组件创建, 注册和使用 - 伸手就来特别熟练==
- [ ] 指令的作用

## 今日学习目标

1. 能够了解组件进阶知识
2. 能够掌握自定义指令创建和使用
3. 能够完成tabbar案例的开发

## 1. 组件进阶

### 1.0 组件进阶 - 动态组件

> 目标: 多个组件使用同一个挂载点，并动态切换，这就是动态组件

需求: 完成一个注册功能页面, 2个按钮切换, 一个填写注册信息, 一个填写用户简介信息

效果如下:

![动态组件](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E5%8A%A8%E6%80%81%E7%BB%84%E4%BB%B6.gif)

1. 准备被切换的 - UserName.vue / UserInfo.vue 2个组件

2. 引入到UseDynamic.vue注册

3. 准备变量来承载要显示的"组件名"

4. 设置挂载点<component>, 使用is属性来设置要显示哪个组件

5. 点击按钮 – 修改comName变量里的"组件名"

```vue
<template>
  <div>
      <button @click="comName = 'UserName'">账号密码填写</button>
      <button @click="comName = 'UserInfo'">个人信息填写</button>

      <p>下面显示注册组件-动态切换:</p>
      <div style="border: 1px solid red;">
          <component :is="comName"></component>
      </div>
  </div>
</template>

<script>
// 目标: 动态组件 - 切换组件显示
// 场景: 同一个挂载点要切换 不同组件 显示
// 1. 创建要被切换的组件 - 标签+样式
// 2. 引入到要展示的vue文件内, 注册
// 3. 变量-承载要显示的组件名
// 4. 设置挂载点<component :is="变量"></component>
// 5. 点击按钮-切换comName的值为要显示的组件名

import UserName from '../components/01/UserName'
import UserInfo from '../components/01/UserInfo'
export default {
    data(){
        return {
            comName: "UserName"
        }
    },
    components: {
        UserName,
        UserInfo
    }
}
</script>
```

在App.vue - 引入01_UseDynamic.vue并使用显示

> 总结: vue内置component组件, 配合is属性, 设置要显示的组件名字

### 1.1 组件进阶 - 组件缓存

> 目标: 组件切换会导致组件被频繁销毁和重新创建, 性能不高

使用Vue内置的keep-alive组件, 可以让包裹的组件保存在内存中不被销毁

演示1: 可以先给UserName.vue和UserInfo.vue 注册created和destroyed生命周期事件, 观察创建和销毁过程

演示2: 使用keep-alive内置的vue组件, 让动态组件缓存而不是销毁

语法:

​		Vue内置的keep-alive组件 包起来要频繁切换的组件

02_UseDynamic.vue

```vue
<div style="border: 1px solid red;">
    <!-- Vue内置keep-alive组件, 把包起来的组件缓存起来 -->
    <keep-alive>
        <component :is="comName"></component>
    </keep-alive>
</div>
```

补充生命周期:

* activated - 激活
* deactivated - 失去激活状态

> 总结: keep-alive可以提高组件的性能, 内部包裹的标签不会被销毁和重新创建, 触发激活和非激活的生命周期方法

### 1.2 组件进阶 - 激活和非激活

> 目标: 被缓存的组件不再创建和销毁, 而是激活和非激活

补充2个钩子方法名:

​	activated – 激活时触发

​	deactivated – 失去激活状态触发

### 1.3 组件进阶 - 组件插槽

> 目标: 用于实现组件的内容分发, 通过 slot 标签, 可以接收到写在组件标签内的内容

vue提供组件插槽能力, 允许开发者在封装组件时，把不确定的部分定义为插槽

插槽例子:

![1ddad96d-f925-452b-8c40-85288fc2cbc4](https://gitee.com/XXXTENTWXD/pic/raw/master/images/1ddad96d-f925-452b-8c40-85288fc2cbc4.gif)

需求: 以前折叠面板案例, 想要实现不同内容显示, 我们把折叠面板里的Pannel组件, 添加组件插槽方式

![image-20220924114219966](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220924114219966.png)

语法口诀: 

1. 组件内用<slot></slot>占位
2. 使用组件时<Pannel></Pannel>夹着的地方, 传入标签替换slot

03/Pannel.vue - 组件(直接复制)

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">
      <h4>芙蓉楼送辛渐</h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? "收起" : "展开" }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
         <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isShow: false,
    };
  },
};
</script>

<style scoped>
h3 {
  text-align: center;
}

.title {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border: 1px solid #ccc;
  padding: 0 1em;
}

.title h4 {
  line-height: 2;
  margin: 0;
}

.container {
  border: 1px solid #ccc;
  padding: 0 1em;
}

.btn {
  /* 鼠标改成手的形状 */
  cursor: pointer;
}

img {
  width: 50%;
}
</style>
```

views/03_UserSlot.vue - 使用组件(==直接复制==)

框: 在这个基础重复使用组件

```vue
<template>
  <div id="container">
    <div id="app">
      <h3>案例：折叠面板</h3>
    </div>
  </div>
</template>

<script>
export default {
};
</script>

<style>
#app {
  width: 400px;
  margin: 20px auto;
  background-color: #fff;
  border: 4px solid blueviolet;
  border-radius: 1em;
  box-shadow: 3px 3px 3px rgba(0, 0, 0, 0.5);
  padding: 1em 2em 2em;
}
</style>
```

views/03_UseSlot.vue - 组件插槽使用

```vue
<template>
  <div id="container">
    <div id="app">
      <h3>案例：折叠面板</h3>
      <Pannel>
          <img src="../assets/mm.gif" alt="">
          <span>我是内容</span>
      </Pannel>
      <Pannel>
          <p>寒雨连江夜入吴,</p>
          <p>平明送客楚山孤。</p>
          <p>洛阳亲友如相问，</p>
          <p>一片冰心在玉壶。</p>
      </Pannel>
      <Pannel></Pannel>
    </div>
  </div>
</template>

<script>
import Pannel from "../components/03/Pannel";
export default {
  components: {
    Pannel,
  },
};
</script>
```

> 总结: 组件内容分发技术, slot占位, 使用组件时传入替换slot位置的标签

### 1.4 组件进阶 - 插槽默认内容

> 目标: 如果外面不给传, 想给个默认显示内容

口诀: <slot>夹着内容默认显示内容, 如果不给插槽slot传东西, 则使用<slot>夹着的内容在原地显示

```vue
<slot>默认内容</slot>
```

### 1.5 组件进阶 - 具名插槽

> 目标: 当一个组件内有2处以上需要外部传入标签的地方

传入的标签可以分别派发给不同的slot位置

要求: v-slot一般用跟template标签使用 (template是html5新出标签内容模板元素, 不会渲染到页面上, 一般被vue解析内部标签)

components/04/Pannel.vue - 留下具名slot

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">
      <slot name="title"></slot>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? "收起" : "展开" }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
     <slot name="content"></slot>
    </div>
  </div>
</template>
```

views/04_UseSlot.vue使用

```vue
<template>
  <div id="container">
    <div id="app">
      <h3>案例：折叠面板</h3>
      <Pannel>
        <template v-slot:title>
          <h4>芙蓉楼送辛渐</h4>
        </template>
        <template v-slot:content>
          <img src="../assets/mm.gif" alt="">
          <span>我是内容</span>
        </template>
      </Pannel>
      <Pannel>
        <template #title>
          <span style="color: red;">我是标题</span>
        </template>
        <template #content>
          <p>寒雨连江夜入吴,</p>
          <p>平明送客楚山孤。</p>
          <p>洛阳亲友如相问，</p>
          <p>一片冰心在玉壶。</p>
        </template>
      </Pannel>
    </div>
  </div>
</template>

<script>
import Pannel from "../components/04/Pannel";
export default {
  components: {
    Pannel,
  },
};
</script>
```

==v-slot可以简化成#使用==

> v-bind可以省略成:    v-on: 可以省略成@   那么v-slot: 可以简化成#

> 总结: slot的name属性起插槽名, 使用组件时, template配合#插槽名传入具体标签

### 1.6 组件进阶 - 作用域插槽

> 目标: 子组件里值, 在给插槽赋值时在父组件环境下使用

复习: 插槽内slot中显示默认内容

例子: 默认内容在子组件中, 但是父亲在给插槽传值, 想要改变插槽显示的默认内容

口诀: 

1. 子组件, 在slot上绑定属性和子组件内的值
2. 使用组件, 传入自定义标签, 用template和v-slot="自定义变量名" 
3. scope变量名自动绑定slot上所有属性和值

components/05/Pannel.vue - 定义组件, 和具名插槽, 给slot绑定属性和值 

```vue
<template>
  <div>
    <!-- 按钮标题 -->
    <div class="title">
      <h4>芙蓉楼送辛渐</h4>
      <span class="btn" @click="isShow = !isShow">
        {{ isShow ? "收起" : "展开" }}
      </span>
    </div>
    <!-- 下拉内容 -->
    <div class="container" v-show="isShow">
     <slot :row="defaultObj">{{ defaultObj.defaultOne }}</slot>
    </div>
  </div>
</template>

<script>
// 目标: 作用域插槽
// 场景: 使用插槽, 使用组件内的变量
// 1. slot标签, 自定义属性和内变量关联
// 2. 使用组件, template配合v-slot="变量名"
// 变量名会收集slot身上属性和值形成对象
export default {
  data() {
    return {
      isShow: false,
      defaultObj: {
        defaultOne: "无名氏",
        defaultTwo: "小传同学"
      }
    };
  },
};
</script>
```

views/05_UseSlot.vue

```vue
<template>
  <div id="container">
    <div id="app">
      <h3>案例：折叠面板</h3>
      <Pannel>
        <!-- 需求: 插槽时, 使用组件内变量 -->
        <!-- scope变量: {row: defaultObj} -->
        <template v-slot="scope">
          <p>{{ scope.row.defaultTwo }}</p>
        </template>
      </Pannel>
    </div>
  </div>
</template>

<script>
import Pannel from "../components/05/Pannel";
export default {
  components: {
    Pannel,
  },
};
</script>
```

> 总结: 组件内变量绑定在slot上, 然后使用组件v-slot="变量"  变量上就会绑定slot身上属性和值

### 1.7 组件进阶 - 作用域插槽使用场景

> 目标: 了解作用域插槽使用场景, 自定义组件内标签+**内容**

案例: 封装一个表格组件, 在表格组件内循环产生单元格

准备MyTable.vue组件 – 内置表格, 传入数组循环铺设页面, 把对象每个内容显示在单元格里

准备UseTable.vue – 准备数据传入给MyTable.vue使用

components/06/MyTable.vue - 模板(==直接复制==)

```vue
<template>
  <div>
      <table border="1">
          <thead>
              <tr>
                  <th>序号</th>
                  <th>姓名</th>
                  <th>年龄</th>
                  <th>头像</th>
              </tr>
          </thead>
          <thead>
              <tr>
                  <td></td>
                  <td></td>
                  <td></td>
                  <td></td>
              </tr>
          </thead>
      </table>
  </div>
</template>

<script>
export default {

}
</script>
```

views/06_UseTable.vue - 准备数据, 传入给MyTable.vue组件里循环使用

```js
list: [
    {
        name: "小传同学",
        age: 18,
        headImgUrl:
        "http://yun.itheima.com/Upload/./Images/20210303/603f2d2153241.jpg",
    },
    {
        name: "小黑同学",
        age: 25,
        headImgUrl:
        "http://yun.itheima.com/Upload/./Images/20210304/6040b101a18ef.jpg",
    },
    {
        name: "智慧同学",
        age: 21,
        headImgUrl:
        "http://yun.itheima.com/Upload/./Images/20210302/603e0142e535f.jpg",
    },
],
```

例子: 我想要给td内显示图片, 需要传入自定义的img标签

![image-20220924114232011](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220924114232011.png)

正确做法:	

​	在MyTable.vue的td中准备<slot>占位, 但是外面需要把图片地址赋予给src属性,所以在slot上把obj数据绑定



components/06/MyTable.vue   - ==正确代码==

```vue
<template>
  <div>
      <table border="1">
          <thead>
              <tr>
                  <th>序号</th>
                  <th>姓名</th>
                  <th>年龄</th>
                  <th>头像</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(obj, index) in arr" :key="index">
                  <td>{{ index + 1 }}</td>
                  <td>{{ obj.name }}</td>
                  <td>{{ obj.age }}</td>
                  <td>
                      <slot :row="obj">
                          <!-- 默认值给上,如果使用组件不自定义标签显示默认文字 -->
                          {{ obj.headImgUrl}}
                      </slot>
                  </td>
              </tr>
          </tbody>
      </table>
  </div>
</template>

<script>
export default {
    props: {
        arr: Array
    }
}
</script>
```

​	在UseTable使用MyTable的时候, template上v-slot绑定变量, 传入img组件设置图片地址

```vue
<template>
  <div>
    <MyTable :arr="list"></MyTable>
    <MyTable :arr="list">
        <!-- scope: {row: obj} -->
       <template v-slot="scope">
            <a :href="scope.row.headImgUrl">{{ scope.row.headImgUrl }}</a>
       </template>
    </MyTable>
    <MyTable :arr="list">
       <template v-slot="scope">
            <img style="width: 100px;" :src="scope.row.headImgUrl" alt="">
       </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from "../components/06/MyTable";
export default {
  components: {
    MyTable,
  },
  data() {
    return {
      list: [
        {
          name: "小传同学",
          age: 18,
          headImgUrl:
            "http://yun.itheima.com/Upload/./Images/20210303/603f2d2153241.jpg",
        },
        {
          name: "小黑同学",
          age: 25,
          headImgUrl:
            "http://yun.itheima.com/Upload/./Images/20210304/6040b101a18ef.jpg",
        },
        {
          name: "智慧同学",
          age: 21,
          headImgUrl:
            "http://yun.itheima.com/Upload/./Images/20210302/603e0142e535f.jpg",
        },
      ],
    };
  },
};
</script>

<style>
</style>
```

> 总结: 插槽可以自定义标签, 作用域插槽可以把组件内的值取出来自定义内容

## 2. 自定义指令

[自定义指令文档](https://www.vue3js.cn/docs/zh/guide/custom-directive.html)

除了核心功能默认内置的指令 (`v-model` 和 `v-show`)，Vue 也允许注册自定义指令。 `v-xxx`  

html+css的复用的主要形式是组件

你需要对普通 DOM 元素进行底层操作，这时候就会用到自定义指令

### 2.0 自定义指令-注册

> 目标: 获取标签, 扩展额外的功能

> ### 局部注册和使用

07_UseDirective.vue - 只能在当前组件.vue文件中使用

```vue
<template>
  <div>
      <!-- <input type="text" v-gfocus> -->
      <input type="text" v-focus>
      
  </div>
</template>

<script>
// 目标: 创建 "自定义指令", 让输入框自动聚焦
// 1. 创建自定义指令
// 全局 / 局部
// 2. 在标签上使用自定义指令  v-指令名
// 注意:
// inserted方法 - 指令所在标签, 被插入到网页上触发(一次)
// update方法 - 指令对应数据/标签更新时, 此方法执行
export default {
    data(){
        return {
            colorStr: 'red'
        }
    },
    //自定义组件
    directives: {
        focus: {
            inserted(el){
                el.focus()
            }
        }
    }
}
</script>

<style>

</style>
```

> ### 全局注册

在main.js用 Vue.directive()方法来进行注册, 以后随便哪个.vue文件里都可以直接用v-fofo指令

```js
// 全局指令 - 到处"直接"使用
Vue.directive("gfocus", {
  inserted(el) {
    el.focus() // 触发标签的事件方法
  }
})
```

> 总结: 全局注册自定义指令, 哪里都能用, 局部注册, 只能在当前vue文件里用

### 2.1 自定义指令-传值

> 目标: 使用自定义指令, 传入一个值

需求: 定义color指令-传入一个颜色, 给标签设置文字颜色

main.js定义处修改一下

```js
// 目标: 自定义指令传值
Vue.directive('color', {
  inserted(el, binding) {
    el.style.color = binding.value
  },
  update(el, binding) {
    el.style.color = binding.value
  }
})
```

Direct.vue处更改一下

```vue
<p v-color="colorStr" @click="changeColor">修改文字颜色</p>

<script>
  data() {
    return {
      theColor: "red",
    };
  },
  methods: {
    changeColor() {
      this.theColor = 'blue';
    },
  },
</script>
```

> 总结: v-xxx, 自定义指令, 获取原生DOM, 自定义操作

## 3. 案例-tabbar

> 完成如下案例和各步功能

![image-20201224004845495](https://gitee.com/XXXTENTWXD/pic/raw/master/images/21_tabbar%E6%A1%88%E4%BE%8B_%E6%89%80%E6%9C%89%E6%95%88%E6%9E%9C.gif)

知识点:

- 组件封装
- 动态组件
- keep-alive
- 作用域插槽
- 自定义指令

### 3.0 案例-tabbar-初始化项目

> 目标: 创建项目文件夹, 引入字体图标, 下载bootstrap, less, less-loader@5.0.0 axios, 在App.vue注册组件

![image-20210511172408766](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511172408766.png)

* 需求: 从0新建项目, 拆分组件, 创建使用

组件分析:

* 组件拆分:
  * MyHeader.vue – ==复用之前的==
  * MyTabBar.vue – 底部导航
  * MyTable.vue – 封装表格

* 三个页面
  * -MyGoodsList.vue – 商品页
  * MyGoodsSearch.vue – 搜索页
  * -MyUserInfo.vue – 用户信息页

思路分析：

​	①: vue create tabbar-demo

​	②: yarn add less less-loader@5.0.0 -D

​	③: yarn add bootstrap axios 并在main.js 引入和全局属性

​	④: 根据需求-创建需要的页面组件

​	⑤: 把昨天购物车案例-封装的MyHeader.vue文件复制过来复用

​	⑥: 从App.vue – 引入组织相关标签

新建工程:

```bash
vue create tabbar-demo
yarn add less less-loader@5.0.0 -D
yarn add bootstrap axios
```

在main.js中引入bootStrap.css和字体图标样式

```js
import "bootstrap/dist/css/bootstrap.css"
import "./assets/fonts/iconfont.css"
```

创建/复制如下文件

从昨天案例中-直接复制过来components/MyHeader.vue

components/MyTabBar.vue



views/MyGoodsList.vue

views/MyGoodsSearch.vue

views/MyUserInfo.vue



components/MyTable.vue

### 3.1 案例-tabbar-底部封装

> 目标: 实现MyTabBar.vue组件

![image-20210511172826513](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210511172826513.png)

* 需求: 把底部导航也灵活封装起来

分析：

​	①: 基本标签+样式(md里复制)

​	②: 为tabbar组件指定数据源

​	③: 数据源最少2个, 最多5个(validator)

​	④: 从App.vue给MyTabBar.vue传入底部导航的数据

​	⑤: MyTabBar.vue中循环展示

App.vue-数组准备

```js
tabList: [
    {
        iconText: "icon-shangpinliebiao",
        text: "商品列表",
        componentName: "MyGoodsList"
    },
    {
        iconText: "icon-sousuo",
        text: "商品搜索",
        componentName: "MyGoodsSearch"
    },
    {
        iconText: "icon-user",
        text: "我的信息",
        componentName: "MyUserInfo"
    }
]
```

MyTabBar.vue - 标签模板

```vue
<template>
  <div class="my-tab-bar">
  	<div class="tab-item">
      <!-- 图标 -->
      <span class="iconfont"></span>
      <!-- 文字 -->
      <span></span>
    </div>
  </div>
</template>

<script>
export default {
  
}
</script>

<style lang="less" scoped>
.my-tab-bar {
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  height: 50px;
  border-top: 1px solid #ccc;
  display: flex;
  justify-content: space-around;
  align-items: center;
  background-color: white;
  .tab-item {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
}
    
.current {
  color: #1d7bff;
}
</style>
```

MyTabBar.vue正确代码(==不可复制==)

```vue
<template>
  <div class="my-tab-bar">
    <div
      class="tab-item"
      v-for="(obj, index) in arr"
      :key="index"
    >
      <!-- 图标 -->
      <span class="iconfont" :class="obj.iconText"></span>
      <!-- 文字 -->
      <span>{{ obj.text }}</span>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    arr: {
      type: Array,
      required: true,
      // 自定义校验规则
      validator(value) {
        // value就是接到数组
        if (value.length >= 2 && value.length <= 5) {
          return true; // 符合条件就return true
        } else {
          console.error("数据源必须在2-5项");
          return false;
        }
      },
    },
  }
};
</script>
```

不要忘了把tabList数组从App.vue -> MyTabBar.vue

### 3.2 案例-tabbar-底部高亮

> 目标: 点击底部导航实现高亮效果

* 需求: 点击底部实现高亮效果

分析：

​	①: 绑定点击事件, 获取点击的索引

​	②: 循环的标签设置动态class, 遍历的索引, 和点击保存的索引比较, 相同则高亮

效果演示:

![image-20220924114254471](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220924114254471.png)

MyTabBar.vue(==正确代码==)

```vue
<template>
  <div class="my-tab-bar">
    <div class="tab-item" 
    v-for="(obj, index) in arr" 
    :key="index"
    :class="{current: activeIndex === index}"
    @click="activeIndex = index">
      <!-- 图标 -->
      <span class="iconfont" :class="obj.iconText"></span>
      <!-- 文字 -->
      <span>{{ obj.text }}</span>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return {
      activeIndex: 0 // 高亮元素下标
    }
  },
  // ....其他代码
};
</script>
```

### 3.3 案例-tabbar-组件切换

> 目的: 点击底部导航, 切换页面组件显示

需求: 点击底部切换组件

分析：

​	①: 底部导航传出动态组件名字符串到App.vue

​	②: 切换动态组件is属性的值为要显示的组件名

效果演示:

![tabbar_切换组件](https://gitee.com/XXXTENTWXD/pic/raw/master/images/tabbar_%E5%88%87%E6%8D%A2%E7%BB%84%E4%BB%B6.gif)

补充: 给内容div.app- 设置上下内边距

App.vue - 引入并注册

```vue
<template>
  <div>
    <MyHeader
      :background="'blue'"
      :fontColor="'white'"
      title="TabBar案例"
    ></MyHeader>
    <div class="main">
      <component :is="comName"></component>
    </div>
    <MyTabBar :arr="tabList"
    @changeCom="changeComFn"
    ></MyTabBar>
  </div>
</template>

<script>
import MyHeader from "./components/MyHeader";
// 目标: 完成底部封装
// 1. MyTabBar.vue 组件标签+样式 准备
// 2. 字体图标引入
// 3. 准备底部数据
// 4. 使用MyTabBar组件, 传入数据(父->子), 循环产生底部导航
// 5. 子组件内props自定义检验规则(2-5项)
// 6. 子组件内循环产生底部导航
import MyTabBar from './components/MyTabBar'

// 目标: 切换组件显示
// 1. 创建组件 - 编写内容
// 2. 引入App.vue注册
// 3. 挂载点设置is
// 4. 切换comName的值(重要)
// 5. 底部导航点击- MyTabBar.vue里
// 子 -> 父技术 (传要切换的组件名出来)

import MyGoodsList from './views/MyGoodsList'
import MyGoodsSearch from './views/MyGoodsSearch'
import MyUserInfo from './views/MyUserInfo'
export default {
  data() {
    return {
      comName: "MyGoodsList", // 默认显示的组件
      tabList: [ // 底部导航的数据
        {
          iconText: "icon-shangpinliebiao",
          text: "商品列表",
          componentName: "MyGoodsList",
        },
        {
          iconText: "icon-sousuo",
          text: "商品搜索",
          componentName: "MyGoodsSearch",
        },
        {
          iconText: "icon-user",
          text: "我的信息",
          componentName: "MyUserInfo",
        },
      ],
    };
  },
  components: {
    MyHeader,
    MyTabBar,
    MyGoodsList,
    MyGoodsSearch,
    MyUserInfo
  },
  methods: {
    changeComFn(cName){
      
      this.comName = cName; // MyTabBar里选出来的组件名赋予给is属性的comName
      // 导致组件的切换
    }
  }
};
</script>

<style scoped>
.main{
  padding-top: 45px;
  padding-bottom: 51px;
}
</style>
```

MyTabBar.vue - 点击传递过来组件名

```js
methods: {
    btn(index, theObj) {
      this.selIndex = index; // 点谁, 就把谁的索引值保存起来
      this.$emit("changeCom", theObj.componentName); // 要切换的组件名传App.vue
    },
  },
```

### 3.4 案例-tabbar-商品列表

> 目标: 为MyGoodsList页面, 准备表格组件MyTable.vue-铺设展示数据 

* 需求: 商品列表铺设页面

分析：

​	①: 封装MyTable.vue – 准备标签和样式

​	②: axios在MyGoodsList.vue请求数据回来

​	③: 请求地址: https://www.escook.cn/api/goods

​	④: 传入MyTable.vue中循环数据显示

​	⑤: 给删除按钮添加bootstrap的样式: btn btn-danger btn-sm

效果演示:

![image-20220924114319867](C:\Users\19241\AppData\Roaming\Typora\typora-user-images\image-20220924114319867.png)

MyTable.vue - 准备table整个表格标签和样式(可复制)

```vue
<template>
  <table class="table table-bordered table-stripped">
    <!-- 表格标题区域 -->
    <thead>
      <tr>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </tr>
    </thead>
    <!-- 表格主体区域 -->
    <tbody>
      <tr >
        <td>1</td>
        <td>商品</td>
        <td>998</td>
        <td>xxx</td>
        <td>xxx</td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'MyTable'
}
</script>


<style scoped lang="less">
.my-goods-list {
  .badge {
    margin-right: 5px;
  }
}
</style>
```

> 使用axios请求数据, 把表格页面铺设出来

main.js - 注册axios配置默认地址

```js
import axios from "axios";
axios.defaults.baseURL = "https://www.escook.cn";
```

MyGoodsList.vue - 使用axios请求数据, 把数据传入给MyTable.vue里循环铺设

```vue
<template>
  <div>
    <MyTable :arr="list">
    </MyTable>
  </div>
</template>

<script>
import MyTable from "../components/MyTable";
import axios from "axios";
axios.defaults.baseURL = "https://www.escook.cn";
// 目标: 循环商品列表表格
// 1. 封装MyTable.vue 整体表格组件-一套标签和样式
// 2. axios请求数据
// 3. 传入MyTable组件里循环tr显示数据

// 目标: 展示tags标签
// 1. tags数组 - 某个td循环span使用文字
// 2. span设置bs的样式

// 目标: 删除数据
// 1. 删除按钮 - 点击事件
// 2. 作用域插槽把索引值关联出来了
// scope身上就有row和index
// 3. 删除中使用scope.index的索引值
// 4. 删除事件里删除数组里对应索引值的数据
export default {
  components: {
    MyTable,
  },
  data() {
    return {
      list: [] // 所有数据
    };
  },
  created() {
    axios({
      url: "/api/goods",
    }).then((res) => {
      console.log(res);
      this.list = res.data.data;
    });
  }
};
</script>
```

MyTable.vue里正确代码(==不可复制==)

```vue
<template>
  <table class="table table-bordered table-stripped">
    <!-- 表格标题区域 -->
    <thead>
      <tr>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </tr>
    </thead>
    <!-- 表格主体区域 -->
    <tbody>
      <tr v-for="(obj, index) in arr"
      :key="obj.id"
      >
        <td>{{ obj.id }}</td>
        <td>{{ obj.goods_name }}</td>
        <td>{{ obj.goods_price }}</td>
        <td>{{ obj.tags }}</td>
        <td>
            <button class="btn btn-danger btn-sm">删除</button>
        </td>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'MyTable',
  props: {
      arr: Array
  }
}
</script>


<style scoped lang="less">
.my-goods-list {
  .badge {
    margin-right: 5px;
  }
}
</style>
```

### 3.5_案例-tabbar-商品表格-插槽

> 目标: 使用插槽技术, 和作用域插槽技术, 给MyTable.vue组件, 自定义列标题, 自定义表格内容

* 需求: 允许用户自定义表格头和表格单元格内容

分析：

​	①: 把MyTable.vue里准备slot

​	②: 使用MyTable组件时传入具体标签

步骤:

1. 提高组件==复用性和灵活性==, 把表格列标题thead部分预留<slot>标签, 设置name属性
2. 使用MyTable.vue时, 传入列标题标签
3. 表格内容td部分也可以让组件使用者自定义, 也给tbody下tr内留好<slot>标签和name属性名
4. 使用插槽需要用到插槽内的obj对象上的数据, 使用作用域插槽技术

![image-20220924114329123](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220924114329123.png)

MyTable.vue - 留好具名插槽

```vue
<template>
  <table class="table table-bordered table-stripped">
    <!-- 表格标题区域 -->
    <thead>
      <tr>
        <!-- <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th> -->
        <slot name="header"></slot>
      </tr>
    </thead>
    <!-- 表格主体区域 -->
    <tbody>
      <tr v-for="(obj, index) in arr"
      :key="obj.id"
      >
        <!-- <td>{{ obj.id }}</td>
        <td>{{ obj.goods_name }}</td>
        <td>{{ obj.goods_price }}</td>
        <td>{{ obj.tags }}</td>
        <td>
            <button class="btn btn-danger btn-sm">删除</button>
        </td> -->
        <slot name="body" :row="obj" :index="index"></slot>
      </tr>
    </tbody>
  </table>
</template>

<script>
export default {
  name: 'MyTable',
  props: {
      arr: Array
  }
}
</script>
```

MyGoodsList.vue 使用

```vue
<template>
  <div>
    <MyTable :arr="list">
      <template #header>
        <th>#</th>
        <th>商品名称</th>
        <th>价格</th>
        <th>标签</th>
        <th>操作</th>
      </template>
      <!-- scope的值: {row: obj, index: 索引值} -->
      <template #body="scope">
        <td>{{ scope.row.id }}</td>
        <td>{{ scope.row.goods_name }}</td>
        <td>{{ scope.row.goods_price }}</td>
        <td>
            {{ scope.row.tags }}
        </td>
        <td>
          <button class="btn btn-danger btn-sm"
          >删除</button>
        </td>
      </template>
    </MyTable>
  </div>
</template>

<script>
import MyTable from "../components/MyTable";
import axios from "axios";
axios.defaults.baseURL = "https://www.escook.cn";
// 目标: 循环商品列表表格
// 1. 封装MyTable.vue 整体表格组件-一套标签和样式
// 2. axios请求数据
// 3. 传入MyTable组件里循环tr显示数据

// 目标: 展示tags标签
// 1. tags数组 - 某个td循环span使用文字
// 2. span设置bs的样式

// 目标: 删除数据
// 1. 删除按钮 - 点击事件
// 2. 作用域插槽把索引值关联出来了
// scope身上就有row和index
// 3. 删除中使用scope.index的索引值
// 4. 删除事件里删除数组里对应索引值的数据
export default {
  components: {
    MyTable,
  },
  data() {
    return {
      list: [] // 所有数据
    };
  },
  created() {
    axios({
      url: "/api/goods",
    }).then((res) => {
      console.log(res);
      this.list = res.data.data;
    });
  }
};
</script>

<style>
</style>
```

### 3.6 案例-tabbar-商品表格-tags微标

> 目标: 把单元格里的标签, tags徽章铺设下

* 需求: 标签列自定义显示

分析：

​	①: 插槽里传入的td单元格

​	②: 自定义span标签的循环展示-给予样式

效果演示:

![image-20220924114338895](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20220924114338895.png)

bootstrap徽章: https://v4.bootcss.com/docs/components/badge/

MyGoodsList.vue - 插槽

```vue
<span v-for="(str, ind) in scope.row.tags" :key="ind"
      class="badge badge-warning"
      >
    {{ str }}
</span>
```

下面额外添加样式

```vue
<style lang="less" scoped>
.my-goods-list {
  .badge {
    margin-right: 5px;
  }
}
</style>
```

### 3.7 案例-tabbar-商品表格-删除功能

> 目标: 点击删除对应这条数据

* 需求: 点击删除按钮删除数据

分析：

​	①: 删除按钮绑定点击事件

​	②: 作用域插槽绑定id值出来

​	③: 传给删除方法, 删除MyGoodsList.vue里数组里数据

效果演示

![tabbar_删除功能](https://gitee.com/XXXTENTWXD/pic/raw/master/images/tabbar_%E5%88%A0%E9%99%A4%E5%8A%9F%E8%83%BD.gif)

提示: id在MyTable.vue里, 但是MyGoodsList.vue里要使用, 而且在插槽位置, 使用作用域插槽已经把整个obj对象(包含id)带出来了

MyTable.vue

```vue
<slot name="body" :row="obj"></slot>
```

1. MyGoodsList.vue - 注册点击事件

```jsx
<button class="btn btn-danger btn-sm"
    @click="removeBtn(scope.row.id)"
    >删除</button>
```

​	2. `my-goods-list.vue` 根据 id 删除

```jsx
removeBtn(id){
    let index = this.list.findIndex(obj => obj.id === id)
    this.list.splice(index, 1)
},
```

### 3.8 案例-tabbar-添加tab

> 目标: 实现点击tab按钮, 出现输入框自动获取焦点, 失去焦点关闭input, 回车新增tag, esc清空内容

* 需求1: 点击Tab, 按钮消失, 输入框出现
* 需求2: 输入框自动聚焦
* 需求3: 失去焦点, 输入框消失, 按钮出
* 需求4: 监测input回车, 无数据拦截
* 需求5: 监测input取消, 清空数据
* 需求6: 监测input回车, 有数据添加

效果目标: 

![tabbar_tab功能](https://gitee.com/XXXTENTWXD/pic/raw/master/images/tabbar_tab%E5%8A%9F%E8%83%BD.gif)

#### 3.8.0 点击按钮消失, 输入框出现

MyGoodsList.vue - 标签位置添加

注意: 每个tab按钮和input都是独立变量控制, 那么直接在row身上的属性控制即可

```vue
<input
          class="tag-input form-control"
          style="width: 100px;"
          type="text"
          v-if="scope.row.inputVisible"
          />
          <button 
          v-else 
          style="display: block;" 
          class="btn btn-primary btn-sm add-tag"
          @click="scope.row.inputVisible = true"
          >+Tag</button>

```

#### 3.8.1 input自动获取焦点

main.js - 定义全局自定义指令

```js
// 全局指令
Vue.directive("focus", {
  inserted(el){
    el.focus()
  }
})
```

MyGoodsList.vue - 使用 v-focus指令

#### 3.8.2 input失去焦点关闭input

监听input失去焦点事件, 让input消失

```js
@blur="scope.row.inputVisible = false"
```

#### 3.8.3 input回车新增tag

监听input的回车事件, 如果无数据拦截代码

```js
@keydown.enter="enterFn(scope.row)"
```

事件处理函数

```js
enterFn(obj){ // 回车
    // console.log(obj.inputValue);
    if (obj.inputValue.trim().length === 0) {
        alert('请输入数据')
        return
    }

    obj.tags.push(obj.inputValue) // 表单里的字符串状态tags数组
    obj.inputValue = ""
}
```

#### 3.8.4 input框esc清空内容

```js
@keydown.esc="scope.row.inputValue = ''"
```

## 今日总结

1. 动态组件的使用步骤
2. 组件缓存使用步骤和作用
3. 组件插槽默认使用
4. 插槽默认显示的内容
5. 多个插槽时, 具名插槽如何使用
6. 作用域插槽如何使用以及目的
7. 自定义命令如何使用
8. 跟随视频完成tabbar案例

## 面试题

### 1. vue中solt的使用方式，以及solt作用域插槽的用法

   使用方式：当组件当做标签进行使用的时候，用slot可以用来接受组件标签包裹的内容，当给solt标签添加name属性的 时候，可以调换响应的位置
  (高级用法) 插槽作用域： 当传递的不是单一的标签, 例如需要循环时, 把要循环的标签传入, 组件内使用v-for在slot标签上, 内部可以v-bind:把值传出来, 再外面把值赋予进去, 看示例

```js
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>

// current-user组件, user属性和值, 绑定给slotProps上
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```

> 扩展阅读: https://cn.vuejs.org/v2/guide/components-slots.html (了解即可, 一般用不上)

### 2. 跟keep-alive有关的生命周期是哪些？（必会）

​    **1****）前言：**在开发Vue项目的时候，大部分组件是没必要多次渲染的，所以Vue提供了一个内置组件keep-alive来缓存组件内部状态，避免重新渲染，在开发Vue项目的时候，大部分组件是没必要多次渲染的，所以Vue提供了一个内置组件keep-alive来缓存组件内部状态，避免重新渲染

​    **2****）生命周期函数：**在被keep-alive包含的组件/路由中，会多出两个生命周期的钩子:activated 与 deactivated。

​       **1****、activated钩子：**在在组件第一次渲染时会被调用，之后在每次缓存组件被激活时调用。

​       **2****、Activated钩子调用时机：** 第一次进入缓存路由/组件，在mounted后面，beforeRouteEnter守卫传给 next 的回调函数之前调用，并且给因为组件被缓存了，再次进入缓存路由、组件时，不会触发这些钩子函数，beforeCreate created beforeMount mounted 都不会触发

​       **1****、deactivated钩子：**组件被停用（离开路由）时调用。

​       **2****、deactivated钩子调用时机**：使用keep-alive就不会调用beforeDestroy(组件销毁前钩子)和destroyed(组件销毁)，因为组件没被销毁，被缓存起来了，这个钩子可以看作beforeDestroy的替代，如果你缓存了组件，要在组件销毁的的时候做一些事情，可以放在这个钩子里，组件内的离开当前路由钩子beforeRouteLeave => 路由前置守卫 beforeEach =>全局后置钩子afterEach => deactivated 离开缓存组件 => activated 进入缓存组件(如果你进入的也是缓存路由)

### 3. 自定义指令(v-check、v-focus)的方法有哪些?它有哪些钩子函数?还有哪些钩子函数参数?（必会）

​    全局定义指令：在vue对象的directive方法里面有两个参数，一个是指令名称，另外一个是函数。组件内定义指令：directives

​    钩子函数：bind(绑定事件触发)、inserted(节点插入的时候触发)、update(组件内相关更新)

​    钩子函数参数：el、binding

### 4. is这个特性你有用过吗？主要用在哪些方面？（高薪常问）

** 1****）动态组件**

​    <component :is="componentName"></component>， componentName可以是在本页面已经注册的局部组件名和全局组件名,也可以是一个组件的选项对象。 当控制componentName改变时就可以动态切换选择组件。

**  2****）is的用法**

​    有些HTML元素，诸如 <ul>、<ol>、<table>和<select>，对于哪些元素可以出现在其内部是有严格限制的。

​    而有些HTML元素，诸如 <li>、<tr> 和 <option>，只能出现在其它某些特定的元素内部。

​    <ul>

​      <card-list></card-list>

​    </ul>

​    所以上面<card-list></card-list>会被作为无效的内容提升到外部，并导致最终渲染结果出错。应该这么写：

​    <ul>

​      <li is="cardList"></li>

​     </ul>



## 附加练习_1.注册组件复用

目的: 封装一个复用的组件, 可以动态的插入标签, 来作为注册页的一块项

图示:

![image-20210115194708455](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210115194708455.png)

正确代码:

```html
<div id="app">
    <child-com :title="'姓名'">
        <input type='text'/>
    </child-com>
    <child-com :title="'密码'">
        <input type='password' />
    </child-com>
    <child-com :title="'性别'">
        <input type='radio' name="sex" value="男"/>男
        <input type='radio' name="sex" value="女"/>女
    </child-com>
    <child-com :title="'爱好'">
        <input type='checkbox' value="抽烟" />抽烟
        <input type='checkbox' value="喝酒" />喝酒
        <input type='checkbox' value="烫头" />烫头
    </child-com>
    <child-com :title="'来自于'">
        <select>
            <option value="北京">北京</option>
            <option value="天津">天津</option>
            <option value="南京">南京</option>
        </select>
    </child-com>

</div>
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
<script>
    new Vue({
        el: "#app",
        components: {
            childCom: { // 组件名字
                props: {
                    title: {
                        type: String
                    }
                },
                template: `<div style="border: 1px solid black;">
<p>{{title}}</p>
<slot></slot>
    </div>`
            }
        }
    })
</script>
```

## 今日作业

把课上的tabbar再来一遍

# 第八章 路由

## 知识点自测

- [ ] url的组成部分都有哪些, hash值指的什么

## 今日学习目标

1. 能够了解单页面应用概念和优缺点
2. 能够掌握vue-router路由系统使用
3. 能够掌握链接导航和编程式导航用法
4. 能够掌握路由嵌套和路由守卫
5. 能够掌握vant组件库基础使用

## 1. vue路由简介和基础使用

### 1.0 什么是路由

> 目标: 设备和ip的映射关系

![image-20221006204517011](https://gitee.com/XXXTENTWXD/pic/raw/master/image-20221006204517011.png)

> 目标: 接口和服务的映射关系

![image-20221006204557687](https://gitee.com/XXXTENTWXD/pic/raw/master/image-20221006204557687.png)

> 目标: 路径和组件的映射关系

![image-20221006204629895](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204629895.png)

### 1.1 为什么使用路由

> 目标: 在一个页面里, 切换业务场景

具体使用示例: 网易云音乐 https://music.163.com/

单页面应用(SPA): 所有功能在一个html页面上实现

前端路由作用: 实现业务场景切换

优点：

* 整体不刷新页面，用户体验更好

* 数据传递容易, 开发效率高

缺点：

* 开发成本高(需要学习专门知识)

* 首次加载会比较慢一点。不利于seo

### 1.2 vue-router介绍

> 目标: 如何在Vue项目中集成路由

官网: https://router.vuejs.org/zh/

vue-router模块包

它和 Vue.js 深度集成

可以定义 - 视图表(映射规则)

模块化的

提供2个内置全局组件

声明式导航自动激活的 CSS class 的链接

……

### 1.3 路由 - 组件分类

> 目标:  .vue文件分2类, 一个是页面组件, 一个是复用组件

.vue文件本质无区别, 方便大家学习和理解, 总结的一个经验

src/views(或pages) 文件夹 和 src/components文件夹

* 页面组件 - 页面展示 - 配合路由用
* 复用组件 - 展示数据/常用于复用

![image-20221006204728373](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204728373.png)



> 总结: views下的页面组件, 配合路由切换, components下的一般引入到views下的vue中复用展示数据

### 1.4 vue-router使用

> 目标: 学会vue官方提供的vue-router路由系统功能模块使用

App.vue - 页面标签和样式准备(==可复制继续写==)

```vue
<template>
  <div>
    <div class="footer_wrap">
      <a href="#/find">发现音乐</a>
      <a href="#/my">我的音乐</a>
      <a href="#/part">朋友</a>
    </div>
    <div class="top">
      
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.footer_wrap {
  position: fixed;
  left: 0;
  top: 0;
  display: flex;
  width: 100%;
  text-align: center;
  background-color: #333;
  color: #ccc;
}
.footer_wrap a {
  flex: 1;
  text-decoration: none;
  padding: 20px 0;
  line-height: 20px;
  background-color: #333;
  color: #ccc;
  border: 1px solid black;
}
.footer_wrap a:hover {
  background-color: #555;
}
.top {
  padding-top: 62px;
}
</style>
```

[vue-router文档](https://router.vuejs.org/zh/)

- 安装

```bash
yarn add vue-router
```

+ 导入路由

```js
import VueRouter from 'vue-router'
```

+ 使用路由插件

```jsx
// 在vue中，使用使用vue的插件，都需要调用Vue.use()
Vue.use(VueRouter)
```

+ 创建路由规则数组

```js
const routes = [
  {
    path: "/find",
    component: Find
  },
  {
    path: "/my",
    component: My
  },
  {
    path: "/part",
    component: Part
  }
]
```

+ 创建路由对象 -  传入规则

```jsx
const router = new VueRouter({
  routes
})
```

+ 关联到vue实例

```jsx
new Vue({
  router
})
```

+ components换成router-view

```vue
<router-view></router-view>
```

> 总结: 下载路由模块, 编写对应规则注入到vue实例上, 使用router-view挂载点显示切换的路由
>
> 总结2: 一切都围绕着hash值变化为准

## 2. vue路由 - 声明式导航

### 2.0 声明式导航 - 基础使用

> 目标: 可用全局组件router-link来替代a标签

1.  vue-router提供了一个全局组件 router-link
2.  router-link实质上最终会渲染成a链接 to属性等价于提供 href属性(to无需#)
3.  router-link提供了声明式导航高亮的功能(自带类名)

```vue
<template>
  <div>
    <div class="footer_wrap">
      <router-link to="/find">发现音乐</router-link>
      <router-link to="/my">我的音乐</router-link>
      <router-link to="/part">朋友</router-link>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
/* 省略了 其他样式 */
.footer_wrap .router-link-active{
  color: white;
  background: black;
}
</style>
```

> 总结: 链接导航, 用router-link配合to, 实现点击切换路由

### 2.1 声明式导航 - 跳转传参

> 目标: 在跳转路由时, 可以给路由对应的组件内传值

在router-link上的to属性传值, 语法格式如下

* /path?参数名=值

* /path/值 – 需要路由对象提前配置 path: “/path/参数名”

对应页面组件接收传递过来的值

* $route.query.参数名

* $route.params.参数名

1. 创建components/Part.vue - 准备接收路由上传递的参数和值

   ```vue
   <template>
     <div>
         <p>关注明星</p>
         <p>发现精彩</p>
         <p>寻找伙伴</p>
         <p>加入我们</p>
         <p>人名: {{ $route.query.name }} -- {{ $route.params.username }}</p>
     </div>
   </template>
   ```

2. 路由定义

   ```js
   {
       path: "/part",
       component: Part
     },
     {
       path: "/part/:username", // 有:的路径代表要接收具体的值
       component: Part
     },
   ```

3. 导航跳转, 传值给MyGoods.vue组件

   ```vue
   <router-link to="/part?name=小传">朋友-小传</router-link>
   <router-link to="/part/小智">朋友-小智</router-link>
   ```

> 总结: 
>
> ?key=value   用$route.query.key 取值
>
> /值   提前在路由规则/path/:key  用$route.params.key  取值

## 3. vue路由 - 重定向和模式

### 3.0 路由 - 重定向

> 目标: 匹配path后, 强制切换到目标path上

* 网页打开url默认hash值是/路径
* redirect是设置要重定向到哪个路由路径

例如: 网页默认打开, 匹配路由"/", 强制切换到"/find"上

```js
const routes = [
  {
    path: "/", // 默认hash值路径
    redirect: "/find" // 重定向到/find
    // 浏览器url中#后的路径被改变成/find-重新匹配数组规则
  }
]
```

> 总结: 强制重定向后, 还会重新来数组里匹配一次规则

### 3.1 路由 - 404页面

> 目标: 如果路由hash值, 没有和数组里规则匹配

默认给一个404页面

语法: 路由最后, path匹配*(任意路径) – 前面不匹配就命中最后这个, 显示对应组件页面

1. 创建NotFound页面

   ```vue
   <template>
     <img src="../assets/404.png" alt="">
   </template>
   
   <script>
   export default {
   
   }
   </script>
   
   <style scoped>
       img{
           width: 100%;
       }
   </style>
   ```

2. 在main.js - 修改路由配置

   ```js
   import NotFound from '@/views/NotFound'
   
   const routes = [
     // ...省略了其他配置
     // 404在最后(规则是从前往后逐个比较path)
     {
       path: "*",
       component: NotFound
     }
   ]
   ```

> 总结: 如果路由未命中任何规则, 给出一个兜底的404页面

### 3.2 路由 - 模式设置

> 目标: 修改路由在地址栏的模式

hash路由例如:  http://localhost:8080/#/home

history路由例如: http://localhost:8080/home  (以后上线需要服务器端支持, 否则找的是文件夹)

[模式文档](https://router.vuejs.org/zh/api/#mode)

router/index.js

```js
const router = new VueRouter({
  routes,
  mode: "history" // 打包上线后需要后台支持, 模式是hash
})
```

## 4. vue路由 - 编程式导航

> 用JS代码跳转, 声明式导航用a标签

### 4.0 编程式导航 - 基础使用

> 目标: 用JS代码来进行跳转

语法:

```js
this.$router.push({
    path: "路由路径", // 都去 router/index.js定义
    name: "路由名"
})
```

1. main.js - 路由数组里, 给路由起名字

```json
{
    path: "/find",
    name: "Find",
    component: Find
},
{
    path: "/my",
    name: "My",
    component: My
},
{
    path: "/part",
    name: "Part",
    component: Part
},
```

2. App.vue - 换成span 配合js的编程式导航跳转

```vue
<template>
  <div>
    <div class="footer_wrap">
      <span @click="btn('/find', 'Find')">发现音乐</span>
      <span @click="btn('/my', 'My')">我的音乐</span>
      <span @click="btn('/part', 'Part')">朋友</span>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
// 目标: 编程式导航 - js方式跳转路由
// 语法:
// this.$router.push({path: "路由路径"})
// this.$router.push({name: "路由名"})
// 注意:
// 虽然用name跳转, 但是url的hash值还是切换path路径值
// 场景:
// 方便修改: name路由名(在页面上看不见随便定义)
// path可以在url的hash值看到(尽量符合组内规范)
export default {
  methods: {
    btn(targetPath, targetName){
      // 方式1: path跳转
      this.$router.push({
        // path: targetPath,
        name: targetName
      })
    }
  }
};
</script>
```

### 4.1 编程式导航 - 跳转传参

> 目标: JS跳转路由, 传参

语法 query / params 任选 一个

```js
this.$router.push({
    path: "路由路径"
    name: "路由名",
    query: {
    	"参数名": 值
    }
    params: {
		"参数名": 值
    }
})

// 对应路由接收   $route.params.参数名   取值
// 对应路由接收   $route.query.参数名    取值
```

==格外注意: 使用path会自动忽略params==

App.vue

```vue
<template>
  <div>
    <div class="footer_wrap">
      <span @click="btn('/find', 'Find')">发现音乐</span>
      <span @click="btn('/my', 'My')">我的音乐</span>
      <span @click="oneBtn">朋友-小传</span>
      <span @click="twoBtn">朋友-小智</span>
    </div>
    <div class="top">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
// 目标: 编程式导航 - 跳转路由传参
// 方式1:
// params => $route.params.参数名
// 方式2:
// query => $route.query.参数名
// 重要: path会自动忽略params
// 推荐: name+query方式传参
// 注意: 如果当前url上"hash值和?参数"与你要跳转到的"hash值和?参数"一致, 爆出冗余导航的问题, 不会跳转路由
export default {
  methods: {
    btn(targetPath, targetName){
      // 方式1: path跳转
      this.$router.push({
        // path: targetPath,
        name: targetName
      })
    },
    oneBtn(){
      this.$router.push({
        name: 'Part',
        params: {
          username: '小传'
        }
      })
    },
    twoBtn(){
      this.$router.push({
        name: 'Part',
        query: {
          name: '小智'
        }
      })
    }
  }
};
</script>
```

> 总结: 传参2种方式
>
> query方式 
>
> params方式 

## 5. vue路由 - 嵌套和守卫

### 5.0 vue路由 - 路由嵌套

> 目标: 在现有的一级路由下, 再嵌套二级路由

[二级路由示例-网易云音乐-发现音乐下](https://music.163.com/)

router-view嵌套架构图

1. 创建需要用的所有组件

   src/views/Find.vue -- 发现音乐页

   src/views/My.vue -- 我的音乐页

   src/views/Second/Recommend.vue  -- 发现音乐页 / 推荐页面

   src/views/Second/Ranking.vue      -- 发现音乐页 / 排行榜页面

   src/views/Second/SongList.vue     -- 发现音乐页 / 歌单页面

2. main.js– 继续配置2级路由

   一级路由path从/开始定义

   二级路由往后path直接写名字, 无需/开头

   嵌套路由在上级路由的children数组里编写路由信息对象

3. 说明：

   App.vue的router-view负责发现音乐和我的音乐页面, 切换

   Find.vue的的router-view负责发现音乐下的, 三个页面, 切换

![image-20221006204745230](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204745230.png)

1. 配置二级导航和样式(==可直接复制==) - 在Find.vue中

```vue
<template>
  <div>
    <!-- <p>推荐</p>
    <p>排行榜</p>
    <p>歌单</p> -->
    <div class="nav_main">
      <router-link to="/find/recommend">推荐</router-link>
      <router-link to="/find/ranking">排行榜</router-link>
      <router-link to="/find/songlist">歌单</router-link>
    </div>

    <div style="1px solid red;">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
export default {};
</script>

<style scoped>
.nav_main {
  background-color: red;
  color: white;
  padding: 10px 0;
}
.nav_main a {
  text-align: center;
  text-decoration: none;
  color: white;
  font-size: 12px;
  margin: 7px 17px 0;
  padding: 0px 15px 2px 15px;
  height: 20px;
  display: inline-block;
  line-height: 20px;
  border-radius: 20px;
}
.nav_main a:hover {
  background-color: brown;
}
.nav_main .router-link-active{
  background-color: brown;
}
</style>
```

2. 配置路由规则-二级路由展示

```js
const routes = [
  // ...省略其他
  {
    path: "/find",
    name: "Find",
    component: Find,
    children: [
      {
        path: "recommend",
        component: Recommend
      },
      {
        path: "ranking",
        component: Ranking
      },
      {
        path: "songlist",
        component: SongList
      }
    ]
  }
  // ...省略其他
]
```

3. 说明：

* App.vue, 外层的router-view负责发现音乐和我的音乐页面切换

* Find.vue 内层的router-view负责发现音乐下的子tab对应的组件切换

4. 运行 - 点击导航观察嵌套路由在哪里展示

> 总结: 嵌套路由, 找准在哪个页面里写router-view和对应规则里写children

### 5.1 声明导航 - 类名区别

> 目标: router-link自带的2个类名的区别是什么

观察路由嵌套导航的样式

* router-link-exact-active  (精确匹配) url中hash值路径, 与href属性值完全相同, 设置此类名

* router-link-active             (模糊匹配) url中hash值,    包含href属性值这个路径

![image-20221006204820197](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204820197.png)

### 5.2 全局前置守卫

> 目标: 路由跳转之前, 先执行一次前置守卫函数, 判断是否可以正常跳转

使用例子: 在跳转路由前, 判断用户登陆了才能去<我的音乐>页面, 未登录弹窗提示回到发现音乐页面

1. 在路由对象上使用固定方法beforeEach

```js
// 目标: 路由守卫
// 场景: 当你要对路由权限判断时
// 语法: router.beforeEach((to, from, next)=>{//路由跳转"之前"先执行这里, 决定是否跳转})
// 参数1: 要跳转到的路由 (路由对象信息)    目标
// 参数2: 从哪里跳转的路由 (路由对象信息)  来源
// 参数3: 函数体 - next()才会让路由正常的跳转切换, next(false)在原地停留, next("强制修改到另一个路由路径上")
// 注意: 如果不调用next, 页面留在原地

// 例子: 判断用户是否登录, 是否决定去"我的音乐"/my
const isLogin = true; // 登录状态(未登录)
router.beforeEach((to, from, next) => {
  if (to.path === "/my" && isLogin === false) {
    alert("请登录")
    next(false) // 阻止路由跳转
  } else {
    next() // 正常放行
  }
})
```

> 总结: next()放行, next(false)留在原地不跳转路由, next(path路径)强制换成对应path路径跳转

## 1. vant组件库

### 1.0 vant组件库-介绍

> 目标: vant是一个轻量、可靠的移动端 Vue 组件库, 开箱即用

[vant官网](https://vant-contrib.gitee.io/vant/#/zh-CN/)

特点:

* 提供 60 多个高质量组件，覆盖移动端各类场景
* 性能极佳，组件平均体积不到 1kb
* 完善的中英文文档和示例
* 支持 Vue 2 & Vue 3
* 支持按需引入和主题定制

### 1.1 全部引入

> 目标: 看官网文档, 下载, 引入vant组件库

全部引入, 快速开始:https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart

1.全部引入, 快速开始: [https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart](https://vant-contrib.gitee.io/vant/)

2.下载Vant组件库到当前项目中

3.在main.js中全局导入所有组件,

4.使用按钮组件 – 作为示范的例子

![image-20221006204832426](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204832426.png)

1. 下载vant组件库到当前项目中

   ```bash
   yarn add vant -D
   ```

2. 导入所有组件, 在main.js中

   ```js
   import Vue from 'vue';
   import Vant from 'vant';
   import 'vant/lib/index.css';
   
   Vue.use(Vant);
   ```

3. 使用按钮组件

   https://vant-contrib.gitee.io/vant/#/zh-CN/button

   ```vue
   <van-button type="primary">主要按钮</van-button>
   <van-button type="info">信息按钮</van-button>
   <van-button type="default">默认按钮</van-button>
   <van-button type="warning">警告按钮</van-button>
   <van-button type="danger">危险按钮</van-button>
   ```

### 1.2 手动按需引入

> 目标: 只引入使用的组件

![image-20221006204838684](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204838684.png)

1.手动单独引入, 快速开始: [https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart](https://vant-contrib.gitee.io/vant/)

```js
// 方式2: 手动 按需引入
// import Button from 'vant/lib/button'; // button组件
// import 'vant/lib/button/style'; // button样式
```

2. 注册

```js
// components: { // 手动注册组件名
//   // VanButton: Button
//   // 等价的
//   [Button.name]: Button
// }
```

3. 使用

```vue
<van-button type="primary">主要按钮</van-button>
<van-button type="info">信息按钮</van-button>
<van-button type="default">默认按钮</van-button>
<van-button type="warning">警告按钮</van-button>
<van-button type="danger">危险按钮</van-button>
```

### 1.3 自动按需引入

> 目标: 按需加载组件

[babel-plugin-import](https://github.com/ant-design/babel-plugin-import) 是一款 babel 插件，它会在编译过程中将 import 的写法自动转换为按需引入的方式。

1. 安装插件

   ```bash
   yarn add babel-plugin-import -D
   ```

2. 在babel配置文件里 (babel.config.js)

   ```js
   module.exports = {
     plugins: [
       ['import', {
         libraryName: 'vant',
         libraryDirectory: 'es',
         style: true
       }, 'vant']
     ]
   };
   ```

3. 全局注册 - 会自动按需引入

   ```js
   // 方式1: 全局 - 自动按需引入vant组件
   // (1): 下载 babel-plugin-import
   // (2): babel.config.js - 添加官网说的配置 (一定要重启服务器)
   // (3): main.js 按需引入某个组件, Vue.use全局注册 - 某个.vue文件中直接使用vant组件
   import { Button } from 'vant';
   Vue.use(Button) // Button组件全局注册, 真正注册的组件名VanButton
   ```

### 1.4 弹出框使用

> 目标: 使用弹出框组件

![image-20221006204848232](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204848232.png)

https://vant-contrib.gitee.io/vant/#/zh-CN/dialog

```vue
<template>
  <div>
    <van-button type="primary" @click="btn">主要按钮</van-button>
    <van-button type="info">信息按钮</van-button>
    <van-button type="default">默认按钮</van-button>
    <van-button type="warning">警告按钮</van-button>
    <van-button type="danger">危险按钮</van-button>
  </div>
</template>

<script>
// 方式2: 手动 按需引入
// import Button from 'vant/lib/button'; // button组件
// import 'vant/lib/button/style'; // button样式

// 目标: 使用弹出框
// 1. 找到vant文档
// 2. 引入
// 3. 在恰当时机, 调用此函数 (还可以用组件的用法)
import { Dialog } from "vant";
export default {
  // components: { // 手动注册组件名
  //   // VanButton: Button
  //   // 等价的
  //   [Button.name]: Button
  // }
  methods: {
    btn() {
      Dialog({ message: "提示", showCancelButton: true }); // 调用执行时, 页面就会出弹出框
    },
  },
};
</script>
```

### 1.5 表单使用

> 目标: 使用vant组件里的表单组件

https://vant-contrib.gitee.io/vant/#/zh-CN/form

![image-20221006204856765](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204856765.png)

表单验证规则:

![image-20221006204904638](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221006204904638.png)

## 今日总结

- [ ] 了解什么是单页面应用, 以及优缺点

- [ ] 了解组件分为哪2类

- [ ] 路由的本质是什么, 就是改变url的hash值, 让js监听到, 根据配置好的映射规则, 显示不同的DOM

- [ ] 声明式导航router-link是vue-router封装注册的全局组件, 比a标签好处是封装了高亮类名等

- [ ] 编程式导航, 用Vue内置的方法改变浏览器url,  this.$router.push() 

- [ ] 路由跳转传参总结

  | 跳转方法                                                     | 传参位置        | 路由规则   | 接收              |
  | ------------------------------------------------------------ | --------------- | ---------- | ----------------- |
  | <router-link to="/path?key=value"></router-link>             | /path?key=value | 无特殊     | $route.query.key  |
  | <router-link to="/path/值"></router-link>                    | /path/值        | /path/:key | $route.params.key |
  | this.$router.push({path: "/path", query: {key: value}}) | query的对象     | 无特殊               | $route.query.key |                 |            |                   |
  | this.$router.push({name: "com", params: {key: value})   | params的对象    | 路由规则需要name属性 | $route.params.key(注意,这种在内存中保存) |                 |            |                   |

  ==无论哪种格式, 声明式和编程式都是通用的, 保证路径和参数格式正确就ok==

- [ ] 路由重定向, 在配置项上使用redirect到目标路由路径

- [ ] 嵌套路由就是在某个一级页面中, 在嵌套一套路由切换系统

  * 在路由规则里找到一级路由, 写children: []  注意: 除了第一层一级路由path写/, 子的开头都无需/
  * 跳转时, 要去的路由路径从一级开始写
  * 心中要做到 浏览器url 路由值  和路由规则里的path 对上即可

- [ ] 全局路由前置守卫 - 可以在跳转路由前进行一些权限判断

- [ ] vant组件库是封装好的组件, 我们拿来即可使用

## 面试题

### 1. 路由之间是怎么跳转的？有哪些方式

1、<router-link to="需要跳转到页面的路径">

 2、this.$router.push()跳转到指定的url，并在history中添加记录，点击回退返回到上一个页面

 3、this.$router.replace()跳转到指定的url，但是history中不会添加记录，点击回退到上上个页面

 4、this.$touter.go(n)向前或者后跳转n个页面，n可以是正数也可以是负数

### 2. vue-router怎么配置路由

在vue中配置路由分为5个步骤，分别是：

1. 引入vue-router.js
2. 配置路由path和组件, 和生成路由对象
3. 把路由对象配置到new Vue中router选项下
4. 页面使用<router-view></router-view> 承载路由
5. <router-link to="要跳转的路径"></router-link> 设置路由导航(声明式导航方式/编程式跳转)

### 3. vue-router的钩子函数都有哪些

关于vue-router中的钩子函数主要分为3类

全局钩子函数要包含beforeEach

  beforeEach函数有三个参数,分别是:

​	to:router即将进入的路由对象
​    from:当前导航即将离开的路由
​    next:function,进行管道中的一个钩子，如果执行完了,则导航的状态就是 confirmed （确认的）否则为false,终止导航。

单独路由独享组件

​	beforeEnter,

组件内钩子

   beforeRouterEnter，
   beforeRouterUpdate,
   beforeRouterLeave

### 4. 路由传值的方式有哪几种

Vue-router传参可以分为两大类，分别是编程式的导航 router.push和声明式的导航

router.push

   字符串：直接传递路由地址，但是不能传递参数

​          this.$router.push("home")

​    对象：

​      命名路由  这种方式传递参数，目标页面刷新会报错 - name+params

​               this.$router.push({name:"news",params:{userId:123})

​      查询参数  和path配对的是query

​               this.$router.push({path:"/news',query:{uersId:123}) 

​      接收参数  this.$route.query

声明式导航

​      字符串 <router-link to:"news"></router-link>

​      命名路由 <router-link :to:"{name:'news',params:{userid:1111}}"></route-link>

​	  还可以to="/path/值" - 需要提前在路由 规则里值 /path/:key

​      查询参数 <router-link :to="{path:'/news',query:{userId:1111}}"></router-link>

​	   还可以to="/path?key=value

### 5. 怎么定义vue-router的动态路由?怎么获取传过来的动态参数?

   动态路由指的就是path路径上传智, 前提需要路由规则了提前配置/path/:key名, 可以写多个用/隔开, 获取使用$route.params.key名来提取对应用路径传过来的值

### 6. Vue的路由实现模式：hash模式和history模式（必会）

hash模式：在浏览器中符号“#”，#以及#后面的字符称之为hash，用 window.location.hash 读取。特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。

history模式：history采用HTML5的新特性；且提供了两个新方法： pushState()， replaceState()可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更

### 7. 请说出路由配置项常用的属性及作用（必会）

​    路由配置参数：  

​      path : 跳转路径
​      component : 路径相对于的组件
​     name:命名路由
​     children:子路由的配置参数(路由嵌套)
​     props:路由解耦
​     redirect : 重定向路由

### 8. 编程式导航使用的方法以及常用的方法（必会）

​    路由跳转 ： this.$router.push()
​    路由替换 : this.$router.replace()
​    后退： this.$router.back()
​    前进 ：this.$router.forward()

### 9. Vue如何去除URL中的#（必会）

​    vue-router 默认使用 hash 模式，所以在路由加载的时候，项目中的 URL 会自带 “#”。如果不想使用 “#”， 可以使用 vue-router 的另一种模式 history：new Router ({ mode : 'history', routes: [ ]})

​    需要注意的是，当我们启用 history 模式的时候，由于我们的项目是一个单页面应用，所以在路由跳转的时候，就会出现访问不到静态资源而出现 “404” 的情况，这时候就需要服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 “index.html” 页面。

### 10. 说一下你在vue中踩过的坑（必会）

​    1、第一个是给对象添加属性的时候，直接通过给data里面的对象添加属性然后赋值，新添加的属性不是响应式的

​    【解决办法】通过Vue.set(对象，属性，值)这种方式就可以达到，对象新添加的属性是响应式的

2、 在created操作dom的时候，是报错的，获取不到dom，这个时候实例vue实例没有挂载

​    【解决办法】通过：Vue.nextTick(回调函数进行获取)

### 11. **$route和$router的区别？**

$route是路由信息对象，包括‘path，hash，query，fullPath，matched，name’等路由信息参数；
$router是路由实例对象，包括了路由的跳转方法，实例对象等

## 附加练习_1.切换页面

> 目的: 点击导航a标签, 实现下面页面内容的切换

建议: 新初始化一个空白的项目来写, 避免新手放到一起, 看的乱

要求: 网页打开默认显示 - 首页部分

规范: 

* views/ 4个页面.vue文件
* router/index.js - 路由配置
* App.vue显示, main.js 注册路由

效果:

![13.5_课上练习_导航切换](https://gitee.com/XXXTENTWXD/pic/raw/master/images/13.5_%E8%AF%BE%E4%B8%8A%E7%BB%83%E4%B9%A0_%E5%AF%BC%E8%88%AA%E5%88%87%E6%8D%A2.gif)



## 附加练习_2.二级路由嵌套

> 目标: 完成git演示的路由切换效果

建议: 再新建一个工程来写

图示:

![14.3_路由嵌套练习](https://gitee.com/XXXTENTWXD/pic/raw/master/images/14.3_%E8%B7%AF%E7%94%B1%E5%B5%8C%E5%A5%97%E7%BB%83%E4%B9%A0.gif)

## 附加练习_3-tabbar切换

把之前写的tabbar案例(原来用动态组件实现)

现在请用路由实现相同的切换效果

## 今日作业

### 三级路由嵌套

要求:

* 默认显示第一个UI_Router路由(一级路由) 3个组件
* 第二个组件需要嵌入导航和二级路由 展示区域
* Bob下才需要第三个路由嵌入

提示:

> 点击按钮使用编程式导航, 可以在2级路由导航路径(带2个/的) 先写成一个数组, 随机取1个然后跳转即可

问题:

可能会爆出警告, 编程式导航如果当前已经在这页, 还想跳转当前路由就会出个警告, 无需关心不影响功能

![Day05_课后作业_嵌套路由作业](https://gitee.com/XXXTENTWXD/pic/raw/master/images/Day05_%E8%AF%BE%E5%90%8E%E4%BD%9C%E4%B8%9A_%E5%B5%8C%E5%A5%97%E8%B7%AF%E7%94%B1%E4%BD%9C%E4%B8%9A.gif)

标签内容(有的短自己手写吧)

```html
<div>
    <h3>Welcome to the UI-Router Demo</h3>
    <p>Use the menu above to navigate. Pay attention to the $state and $stateParams values below.</p>
    <p>Click these links—Alice or Bob—to see a url redirect in action.</p>
</div>



<div>
    <h3>UI-Router Resources</h3>
    <ul>
        <li>Source for this Sample</li>
        <li>GitHub Main Page</li>
        <li>Quick Start</li>
        <li>In-Depth Guide</li>
        <li>API Reference</li>
    </ul>
</div>
```

## 额外扩展

### 路由 - 工作原理手写

> 目标: 了解hash改变, 如何显示不同的组件的过程

基本思路:

1. 用户点击了页面上的a链接
2. 导致了 URL 地址栏中的 Hash 值发生了变化
3. 前端js监听了到 Hash 地址的变化
4. 前端js把当前 Hash 地址对应的组件渲染都浏览器中

实现简单的前端路由:

1. src/views/创建并在App.vue里导入和注册组件

   MyHome.vue

   MyMovie.vue

   MyAbout.vue

```html
<script>
import MyHome from '@/views/MyHome.vue'
import MyMovie from '@/views/MyMovie.vue'
import MyAbout from '@/views/MyAbout.vue'
export default {
  components: {
    MyHome,
    MyMovie,
    MyAbout,
  }
}
</script>
```

2. 通过动态组件, 控制要显示的组件

```vue
<template>
  <div>
    <h1>App组件</h1>
    <component :is="comName"></component>
  </div>
</template>

<script>
export default {
  // ...省略其他
  data () {
    return {
      comName: 'MyHome'
    }
  }
}
</script>
```

3. 声明三个导航链接, 点击时修改地址栏的 hash 值

```jsx
<template>
  <div>
    <h1>App组件</h1>
    <a href="#/home">首页</a>&nbsp;
    <a href="#/movie">电影</a>&nbsp;
    <a href="#/about">关于</a>&nbsp;
    <component :is="comName"></component>
  </div>
</template>
```

4. 在 created 中, 监视地址栏 hash 时的变化, 一旦变化, 动态切换展示的组件

```jsx
created () {
  window.onhashchange = () => {
    switch(location.hash) {
      case '#/home':
        this.comName = 'MyHome'
        break
      case '#/movie':
        this.comName = 'MyMovie'
        break
      case '#/about':
        this.comName = 'MyAbout'
        break
    }
  }
},
```

> 总结: 改变浏览器url的hash值, JS监听到hash值改变, 把对应的组件显示到同一个挂载点



# 第九章 网易云项目

## 知识点自测

- [ ] 知道reset.css和flexible.js的作用
- [ ] 什么是组件库-例如bootstrap作用
- [ ] yarn命令的使用
- [ ] 组件名字用name属性方式注册
- [ ] 如何自定义组件库样式

## 铺垫(自学)

> #### 本地接口项目部署

下载网易云音乐node接口项目, 在本地启动, 为我们vue项目提供数据支持

[项目地址](https://binaryify.github.io/NeteaseCloudMusicApi/#/?id=%e5%ae%89%e8%a3%85)

[备用地址](https://github.com/Binaryify/NeteaseCloudMusicApi/tree/master/docs)

下载后, 安装所有依赖, 在本地启动起来, 测试访问此地址是否有数据

http://localhost:3000, 看到如下页面就成功了 - 等着明天上课启动即可

![image-20221025205737393](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221025205737393.png)

> 总结: 前端请求本地的node项目, node服务器伪装请求去拿网易云音乐服务器数据转发回给自己前端

## 今日学习目标

1. 能够掌握vant组件库的使用
2. 能够掌握vant组件自定义样式能力
3. 能够掌握组件库使用和文档使用能力
4. 能够完成网易云音乐案例

## 1. 案例-网易云音乐

### 1.0 网易云音乐-本地接口

> 目的: 请求网易云音乐服务器API接口-获取数据

![image-20221025205748814](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221025205748814.png)

> 总结: 反向代理就是用本地开启cors的服务器去转发请求拿到数据

### 1.1 网易云音乐-本地接口启动

> 目的: 启动本地网易云音乐API服务

在今天的笔记铺垫中, 大家自学下载了一个项目启动即可

### 1.2 网易云音乐-前端项目初始化

> 目标: 初始化项目, 下载必备包, 引入初始文件, 配置按需自动引入vant, 创建页面组件

1.  初始化工程

   ```bash
   vue create music-demo
   ```

2. 下载需要的所有第三方依赖包

   ```bash
   yarn add axios vant vue-router@3.5.2
   ```

3. 引入笔记代码里准备好的reset.css和flexible.js - 实现样式初始化和适配问题 - 引入到main.js

4. 本次vant使用**自动按需引入**的方式

   文档: https://vant-contrib.gitee.io/vant/#/zh-CN/quickstart

   ```bash
   yarn add babel-plugin-import  -D
   ```

   在babel.config.js - 添加插件配置

   ```js
   plugins: [
       ['import', {
           libraryName: 'vant',
           libraryDirectory: 'es',
           style: true
       }, 'vant']
   ]
   ```


### 1.3 网易云音乐-需求分析

根据需求, 创建路由所需要的5个页面的组件

Layout(布局, 顶部导航和底部导航) > 二级路由 Home 和 Search

Play

![image-20210426212251154](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210426212251154.png)

创建需要的views下的页面组件4个

views/Layout/index.vue  - 负责布局(上下导航 - 中间二级路由切换首页和搜索页面)

```css
/* 中间内容区域 - 容器样式(留好上下导航所占位置) */
.main {
  padding-top: 46px;
  padding-bottom: 50px;
}
```

views/Home/index.vue - 标题和歌名样式

```css
/* 标题 */
.title {
  padding: 0.266667rem 0.24rem;
  margin: 0 0 0.24rem 0;
  background-color: #eee;
  color: #333;
  font-size: 15px;
}
/* 推荐歌单 - 歌名 */
.song_name {
  font-size: 0.346667rem;
  padding: 0 0.08rem;
  margin-bottom: 0.266667rem;
  word-break: break-all;
  text-overflow: ellipsis;
  display: -webkit-box; /** 对象作为伸缩盒子模型显示 **/
  -webkit-box-orient: vertical; /** 设置或检索伸缩盒对象的子元素的排列方式 **/
  -webkit-line-clamp: 2; /** 显示的行数 **/
  overflow: hidden; /** 隐藏超出的内容 **/
}
```

views/Search/index.vue

```css
/* 搜索容器的样式 */
.search_wrap {
  padding: 0.266667rem;
}

/*热门搜索文字标题样式 */
.hot_title {
  font-size: 0.32rem;
  color: #666;
}

/* 热搜词_容器 */
.hot_name_wrap {
  margin: 0.266667rem 0;
}

/* 热搜词_样式 */
.hot_item {
  display: inline-block;
  height: 0.853333rem;
  margin-right: 0.213333rem;
  margin-bottom: 0.213333rem;
  padding: 0 0.373333rem;
  font-size: 0.373333rem;
  line-height: 0.853333rem;
  color: #333;
  border-color: #d3d4da;
  border-radius: 0.853333rem;
  border: 1px solid #d3d4da;
}
```

views/Play/index.vue - 直接从预习资料里复制(节省时间) - 可自己扩展阅读代码

### 1.4 网易云音乐-路由准备

> 目标: 准备路由配置, 显示不同路由页面

router/index.js - 准备路由 - 以及默认显示Layout, 然后Layout默认显示二级路由的首页

```js
// 路由-相关模块
import Vue from 'vue'
import VueRouter from 'vue-router'
import Layout from '@/views/Layout'
import Home from '@/views/Home'
import Search from '@/views/Search'
import Play from '@/views/Play'

Vue.use(VueRouter)
const routes = [
    {
        path: '/',
        redirect: '/layout'
    },
    {
        path: '/layout',
        component: Layout,
        redirect: '/layout/home',
        children: [
            {
                path: 'home',
                component: Home,
                meta: { // meta保存路由对象额外信息的
                    title: "首页"
                }
            },
            {
                path: 'search',
                component: Search,
                meta: {
                    title: "搜索"
                }
            }
        ]
    },
    {
        path: '/play',
        component: Play
    }
]

const router = new VueRouter({
    routes
})

export default router
```

main.js - 引入路由对象, 注册到new Vue中

```js
import router from '@/router'

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
```

App.vue中留好router-view显示路由页面

```vue
<template>
  <div>
    <router-view></router-view>
  </div>
</template>
```

> 总结: 把项目的框搭好, 逐个攻破

### 1.5 网易云音乐-Tabbar组件

> 目标: 点击底部导航, 切换路由页面显示

文档: https://vant-contrib.gitee.io/vant/#/zh-CN/tabbar

1. 注册Tabbar组件, 在main.js中

   ```js
   import { Tabbar, TabbarItem  } from 'vant';
   Vue.use(Tabbar);
   Vue.use(TabbarItem);
   ```

2. 在Layout.vue中使用

   ```vue
   <template>
     <div>
       <div class="main">
         <!-- 二级路由-挂载点 -->
         <router-view></router-view>
       </div>
       <van-tabbar route>
         <van-tabbar-item replace to="/layout/home" icon="home-o"
           >首页</van-tabbar-item
         >
         <van-tabbar-item replace to="/layout/search" icon="search"
           >搜索</van-tabbar-item
         >
       </van-tabbar>
     </div>
   </template>
   
   <script>
   export default {
   }
   </script>
   
   <style scoped>
   /* 中间内容区域 - 容器样式(留好上下导航所占位置) */
   .main {
     padding-top: 46px;
     padding-bottom: 50px;
   }
   </style>
   ```

3. 开启路由模式 route属性, 和to属性指向要切换的路由路径

   ```vue
   <van-tabbar route>
       <van-tabbar-item icon="home-o" to="/layout/home"
                        >首页</van-tabbar-item
           >
       <van-tabbar-item icon="search" to="/layout/search"
                        >搜索</van-tabbar-item
           >
   </van-tabbar>
   ```

> 总结: van-tabbar开启route

### 1.6 网易云音乐-NavBar导航组件

> 目标: 实现顶部标题展示

文档: https://vant-contrib.gitee.io/vant/#/zh-CN/nav-bar

1. main.js - 注册NavBar组件

   ```js
   import { NavBar } from 'vant';
   Vue.use(NavBar);
   ```

2. 复制文档里的, 然后删删只留标题

   ```vue
   <van-nav-bar :title="activeTitle" fixed />
   
   <script>
       export default {
           activeTitle: "首页"
       }
   </script>
   ```

### 1.7 网易云音乐-NavBar标题切换

> 目标: 实现点击底部导航/刷新非第一页面页面, 导航标题正确显示

* 在router/index.js - 给$route里需要导航标题的添加meta元信息属性

  ```js
  {
          path: '/layout',
          component: Layout,
          redirect: '/layout/home',
          children: [
              {
                  path: 'home',
                  component: Home,
                  meta: { // meta保存路由对象额外信息的
                      title: "首页"
                  }
              },
              {
                  path: 'search',
                  component: Search,
                  meta: {
                      title: "搜索"
                  }
              }
          ]
      },
  ```

  Layout.vue中监听$route改变:

  给导航active的值设置$route里的元信息的标题

  ```js
  export default {
    data() {
      return {
        activeTitle: this.$route.meta.title, // "默认"顶部导航要显示的标题 (默认获取当前路由对象里的meta中title值)
      };
    },
    // 路由切换 - 侦听$route对象改变
    watch: {
      $route() {
        this.activeTitle = this.$route.meta.title; // 提取切换后路由信息对象里的title显示
      },
    },
  };
  ```

> 总结: 点击底部导航和刷新当前网页, 都能保证导航标题的正确显示

### 1.8 网易云音乐-网络请求封装

> 目标: 不想把网络请求散落在各个逻辑页面里, 不然以后找起来改起来很麻烦

1. 封装utils/request.js - 基于axios进行二次封装 - 设置基础地址

   ```js
   // 网络请求 - 二次封装
   import axios from 'axios'
   axios.defaults.baseURL = "http://localhost:3000"
   export default axios
   ```

2. 封装src/api/Home.js

   统一封装网络请求方法

   ```js
   // 文件名-尽量和模块页面文件名统一(方便查找)
   import request from '@/utils/request'
   
   // 首页 - 推荐歌单
   export const recommendMusic = params => request({
       url: '/personalized',
       params
       // 将来外面可能传入params的值 {limit: 20}
   })
   ```

3. 在src/api/index.js - 统一导出接口供外部使用

   ```js
   // api文件夹下 各个请求模块js, 都统一来到index.js再向外导出
   import {recommendMusic} from './Home'
   
   export const recommendMusicAPI = recommendMusic // 请求推荐歌单的方法导出
   ```

4. 在main.js - 测试使用一下.

   ```js
   import { recommendMusicAPI } from '@/api/index'
   async function myFn(){
     const res = await recommendMusicAPI({limit: 6});
     console.log(res);
   }
   myFn();
   ```

>  总结: 封装网络请求方法目的, 方便我们统一管理

### 1.9 网易云音乐-首页-推荐歌单

接口地址: /personalized

1. 布局采用van-row和van-col 

   布局文档https://vant-contrib.gitee.io/vant/#/zh-CN/col

2. 使用vant内置的图片组件来显示图片

3. 在main.js注册使用的组件

   ```js
   import { Col, Row, Image as VanImage } from 'vant';
   
   Vue.use(Col);
   Vue.use(Row);
   Vue.use(VanImage);
   ```

4. 在api/index.js下定义推荐歌单的接口方法

   ```js
   // 首页 - 推荐歌单
   export const recommendMusic = params => request({
       url: '/personalized',
       params
       // 将来外面可能传入params的值 {limit: 20}
   })
   ```

5. 把数据请求回来, 用van-image和p标签展示推荐歌单和歌单名字

   ```vue
   <template>
     <div>
      <p class="title">推荐歌单</p>
       <van-row gutter="6">
         <van-col span="8" v-for="obj in reList" :key="obj.id">
           <van-image width="100%" height="3rem" fit="cover" :src="obj.picUrl" />
           <p class="song_name">{{ obj.name }}</p>
         </van-col>
       </van-row>
     </div>
   </template>
   
   <script>
   import { recommendMusicAPI } from "@/api";
   export default {
     data() {
       return {
          reList: [], // 推荐歌单数据
       };
     },
     async created() {
       const res = await recommendMusicAPI({
         limit: 6,
       });
       console.log(res);
       this.reList = res.data.result;
     },
   };
   </script>
   ```

### 1.10 网易云音乐- 首页-最新音乐

> 目标: van-cell单元格使用

请求地址: /personalized/newsong

1. 引入van-cell使用 - 注册组件main.js中

   ```js
   import {Cell} from 'vant';
   Vue.use(Cell);
   ```

2. 定义接口请求方法 - api/index.js

   ```js
   // 首页 - 推荐最新音乐
   export const newMusic = params => request({
       url: "/personalized/newsong",
       params
   })
   ```

3. 列表数据铺设 - 插入自定义标签

   ```vue
   <template>
     <div>
       <p class="title">推荐歌单</p>
       <div>
         <van-row gutter="5">
           <van-col span="8" v-for="obj in recommendList" :key="obj.id">
             <van-image fit="cover" :src="obj.picUrl" />
             <p class="song_name">{{ obj.name }}</p>
           </van-col>
         </van-row>
       </div>
       <p class="title">最新音乐</p>
       <van-cell center v-for="obj in songList" :key="obj.id" :title="obj.name" :label="obj.song.artists[0].name + ' - ' + obj.name">
           <template #right-icon>
             <van-icon name="play-circle-o" size="0.6rem"/>
           </template>
       </van-cell>
     </div>
   </template>
   
   <script>
   import { recommendMusicAPI, newMusicAPI } from "@/api";
   export default {
      data() {
       return {
         reList: [], // 推荐歌单数据
         songList: [], // 最新音乐数据
       };
     },
     async created() {
       const res = await recommendMusicAPI({
         limit: 6,
       });
       console.log(res);
       this.reList = res.data.result;
   
       const res2 = await newMusicAPI({
         limit: 20
       })
       console.log(res2);
       this.songList = res2.data.result
     },
   };
   </script>
   ```

### 1.11 网易云音乐-搜索-热搜关键字

> 目标: 完成热搜关键字铺设

搜索框 – van-search组件

api/Search.js – 热搜关键字 - 接口方法

Search/index.vue引入-获取热搜关键字 - 铺设页面

点击文字填充到输入框

1. 准备搜索界面标签

```vue
<template>
  <div>
    <van-search
      shape="round"
      placeholder="请输入搜索关键词"
    />
    <!-- 搜索下容器 -->
    <div class="search_wrap">
      <!-- 标题 -->
      <p class="hot_title">热门搜索</p>
      <!-- 热搜关键词容器 -->
      <div class="hot_name_wrap">
        <!-- 每个搜索关键词 -->
        <span
          class="hot_item"
          >热搜关键字</span
        >
      </div>
    </div>
  </div>
</template>
<script>
export default {}
</script>

<style scoped>
/* 搜索容器的样式 */
.search_wrap {
  padding: 0.266667rem;
}

/*热门搜索文字标题样式 */
.hot_title {
  font-size: 0.32rem;
  color: #666;
}

/* 热搜词_容器 */
.hot_name_wrap {
  margin: 0.266667rem 0;
}

/* 热搜词_样式 */
.hot_item {
  display: inline-block;
  height: 0.853333rem;
  margin-right: 0.213333rem;
  margin-bottom: 0.213333rem;
  padding: 0 0.373333rem;
  font-size: 0.373333rem;
  line-height: 0.853333rem;
  color: #333;
  border-color: #d3d4da;
  border-radius: 0.853333rem;
  border: 1px solid #d3d4da;
}

/* 给单元格设置底部边框 */
.van-cell {
  border-bottom: 1px solid lightgray;
}
</style>
```

2. api/Search.js - 定义热门搜索接口方法和搜索结果方法

```js
import request from '@/utils/request'

// 热搜关键字
export const hotSearch = () => request({
    url: '/search/hot'
})

// 搜索结果列表
export const searchResult = params => request({
    url: '/cloudsearch',
    params
})
```

3. api/index.js - 导入使用并统一导出

```js
// 统一出口
// 你也可以在逻辑页面里.vue中直接引入@/api/Home下的网络请求工具方法
// 为什么: 我们可以把api所有的方法都统一到一处. 

import {recommendMusic, hotMusic} from '@/api/Home'
import {hotSearch, searchResult} from '@/api/Search'


export const recommendMusicAPI = recommendMusic // 把网络请求方法拿过来 导出
export const hotMusicAPI = hotMusic // 把获取最新音乐的, 网络请求方法导出

export const hotSearchAPI = hotSearch // 热搜
export const searchResultAPI = searchResult // 搜索结果
```

4. created中请求接口-拿到热搜关键词列表

```vue
<!-- 每个搜索关键词 -->
<span
      class="hot_item"
      v-for="(obj, index) in hotArr"
      :key="index"
      >{{ obj.first }}</span>

<script>
    // 目标: 铺设热搜关键字
    // 1. 搜索框van-search组件, 关键词标签和样式
    // 2. 找接口, api/Search.js里定义获取搜索关键词的请求方法
    // 3. 引入到当前页面, 调用接口拿到数据循环铺设页面
    // 4. 点击关键词把值赋予给van-search的v-model变量
    import { hotSearchAPI } from "@/api";
    export default {
        data(){
            return {
                hotArr: [], // 热搜关键字
            }
        },
        async created() {
            const res = await hotSearchAPI();
            console.log(res);
            this.hotArr = res.data.result.hots;
        },
    }
</script>
```

5. 点击热词填充到输入框

```vue
<van-search
            shape="round"
            v-model="value"
            placeholder="请输入搜索关键词"
            />
<!-- 每个搜索关键词 -->
<span
      class="hot_item"
      v-for="(obj, index) in hotArr"
      :key="index"
      @click="fn(obj.first)"
      >{{ obj.first }}</span
    >
</div>

<script>
    export default {
        data(){
            return {
                value: "",
                hotArr: [], // 热搜关键字
            }
        },
        // ...省略了created
        methods: {
            async fn(val) {
                // 点击热搜关键词
                this.value = val; // 选中的关键词显示到搜索框
            },
        }
    }
</script>
```

> 总结: 写好标签和样式, 拿到数据循环铺设, 点击关键词填入到van-search中

### 1.12 网易云音乐-搜索-点击热词-搜索结果

> 目标: 点击热词填充到输入框-出搜索结果

api/Search.js - 搜索结果, 接口方法

Search/index.vue引入-获取搜索结果 - 铺设页面

和热搜关键字容器 – 互斥显示

点击文字填充到输入框, 请求搜索结果铺设

1. 搜索结果显示区域标签+样式(直接复制/vant文档找)

```vue
<!-- 搜索结果 -->
    <div class="search_wrap">
      <!-- 标题 -->
      <p class="hot_title">最佳匹配</p>
      <van-cell
        center
        title='结果名字'
      >
        <template #right-icon>
          <van-icon name="play-circle-o" size="0.6rem"/>
        </template>
      </van-cell>
    </div>
```

2. 点击 - 获取搜索结果 - 循环铺设页面

```vue
<template>
  <div>
    <van-search shape="round" v-model="value" placeholder="请输入搜索关键词" />
    <!-- 搜索下容器 -->
    <div class="search_wrap">
      <!-- 标题 -->
      <p class="hot_title">热门搜索</p>
      <!-- 热搜关键词容器 -->
      <div class="hot_name_wrap">
        <!-- 每个搜索关键词 -->
        <span
          class="hot_item"
          v-for="(obj, index) in hotArr"
          :key="index"
          @click="fn(obj.first)"
          >{{ obj.first }}</span
        >
      </div>
    </div>
    <!-- 搜索结果 -->
    <div class="search_wrap">
      <!-- 标题 -->
      <p class="hot_title">最佳匹配</p>
      <van-cell
        center
        v-for="obj in resultList"
        :key="obj.id"
        :title="obj.name"
        :label="obj.ar[0].name + ' - ' + obj.name"
      >
        <template #right-icon>
          <van-icon name="play-circle-o" size="0.6rem"/>
        </template>
      </van-cell>
    </div>
  </div>
</template>
<script>
// 目标: 铺设热搜关键字
// 1. 搜索框van-search组件, 关键词标签和样式
// 2. 找接口, api/Search.js里定义获取搜索关键词的请求方法
// 3. 引入到当前页面, 调用接口拿到数据循环铺设页面
// 4. 点击关键词把值赋予给van-search的v-model变量

// 目标: 铺设搜索结果
// 1. 找到搜索结果的接口 - api/Search.js定义请求方法
// 2. 再定义methods里getListFn方法(获取数据)
// 3. 在点击事件方法里调用getListFn方法拿到搜索结果数据
// 4. 铺设页面(首页van-cell标签复制过来)
// 5. 把数据保存到data后, 循环van-cell使用即可(切换歌手字段)
// 6. 互斥显示搜索结果和热搜关键词
import { hotSearchAPI, searchResultListAPI } from "@/api";
export default {
  data() {
    return {
      value: "",
      hotArr: [], // 热搜关键字
      resultList: [], // 搜索结果
    };
  },
  async created() {
    const res = await hotSearchAPI();
    console.log(res);
    this.hotArr = res.data.result.hots;
  },
  methods: {
    async getListFn() {
      return await searchResultListAPI({
        keywords: this.value,
        limit: 20,
      }); // 把搜索结果return出去
      // (难点):
      // async修饰的函数 -> 默认返回一个全新Promise对象
      // 这个Promise对象的结果就是async函数内return的值
      // 拿到getListFn的返回值用await提取结果
    },
    async fn(val) {
      // 点击热搜关键词
      this.value = val; // 选中的关键词显示到搜索框
      const res = await this.getListFn();
      console.log(res);
      this.resultList = res.data.result.songs;
    },
  },
};
</script>
```

3. 互斥显示, 热搜关键词和搜索结果列表

![image-20210512142504929](F:\前端\05、阶段五 Vue.js项目实战开发\资料\webpack+Vue基础课程资料\Day08_网易云音乐案例\01_笔记和ppt\images\image-20210512142504929.png)

> 总结: 点击热词后, 调用接口传入关键词, 返回数据铺设

### 1.13 网易云音乐-输入框-搜索结果

> 目标: 监测输入框改变-拿到搜索结果

观察van-search组件是否支持和实现input事件

绑定@input事件和方法

在事件处理方法中获取对应的值使用

如果搜索不存在的数据-要注意接口返回字段不同

1. 绑定@input事件在van-search上

```vue
<van-search shape="round" v-model="value" placeholder="请输入搜索关键词" @input="inputFn"/>
```

2. 实现输入框改变 - 获取搜索结果铺设

```js
async inputFn() {
    // 输入框值改变
    if (this.value.length === 0) {
        // 搜索关键词如果没有, 就把搜索结果清空阻止网络请求发送(提前return)
        this.resultList = [];
        return;
    }
    const res = await this.getListFn();
    console.log(res);
    // 如果搜索结果响应数据没有songs字段-无数据
    if (res.data.result.songs === undefined) {
        this.resultList = [];
        return;
    }
    this.resultList = res.data.result.songs;
},
```

> 总结: 监测输入框改变-保存新的关键词去请求结果回来铺设

### 1.14 网易云音乐-搜索结果-加载更多

> 目标: 触底后, 加载下一页数据

观察接口文档: 发现需要传入offset和分页公式

van-list组件监测触底执行onload事件

配合后台接口, 传递下一页的标识

拿到下一页数据后追加到当前数组末尾即可

1. 设置van-list组件实现相应的属性和方法, 让page++去请求下页数据

```vue
      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="没有更多了"
        @load="onLoad"
      >
        <van-cell
          center
          v-for="obj in resultList"
          :key="obj.id"
          :title="obj.name"
          :label="obj.ar[0].name + ' - ' + obj.name"
        >
          <template #right-icon>
            <van-icon name="play-circle-o" size="0.6rem" />
          </template>
        </van-cell>
      </van-list>
<script>
// 目标: 加载更多
// 1. 集成list组件-定义相关的变量(搞懂变量的作用) -监测触底事件
// 2. 一旦触底, 自动执行onload方法
// 3. 对page++, 给后台传递offset偏移量参数-请求下一页的数据
// 4. 把当前数据和下一页新来的数据拼接起来用在当前 页面的数组里
// (切记) - 加载更多数据后,一定要把loading改成false, 保证下一次还能触发onload方法
export default {
  data() {
    return {
      value: "",
      hotArr: [], // 热搜关键字
      resultList: [], // 搜索结果
      loading: false, // 加载中 (状态) - 只有为false, 才能触底后自动触发onload方法
      finished: false, // 未加载全部 (如果设置为true, 底部就不会再次执行onload, 代表全部加载完成)
      page: 1, // 当前搜索结果的页码
    };
  },
  // ...省略其他
  methods: {
    async getListFn() {
      return await searchResultListAPI({
        keywords: this.value,
        limit: 20,
        offset: (this.page - 1) * 20, // 固定公式
      }); // 把搜索结果return出去
      // (难点):
      // async修饰的函数 -> 默认返回一个全新Promise对象
      // 这个Promise对象的结果就是async函数内return的值
      // 拿到getListFn的返回值用await提取结果
    },
    async onLoad() {
      // 触底事件(要加载下一页的数据咯), 内部会自动把loading改为true
      this.page++;
      const res = await this.getListFn();
      this.resultList = [...this.resultList, ...res.data.result.songs];
      this.loading = false; // 数据加载完毕-保证下一次还能触发onload
    },
  },
};
</script>
```

> 总结: list组件负责UI层监测触底, 执行onload函数, page++, 请求下页数据, 和现在数据合并显示更多, 设置loading为false, 确保下次触底还能执行onLoad

### 1.15 网易云音乐-加载更多-bug修复

> 目标: 如果只有一页数据/无数据判断

无数据/只有一页数据, finished为true

防止list组件触底再加载更多

还要测试-按钮点击/输入框有数据情况的加载更多

正确代码

```diff
 async fn(val) {
      // 点击热搜关键词
+        this.finished = false; // 点击新关键词-可能有新的数据
      this.value = val; // 选中的关键词显示到搜索框
      const res = await this.getListFn();
      console.log(res);
      this.resultList = res.data.result.songs;
+        this.loading = false; // 本次数据加载完毕-才能让list加载更多
    },
    async inputFn() {
+       this.finished = false // 输入框关键字改变-可能有新数据(不一定加载完成了)
      // 输入框值改变
      if (this.value.length === 0) {
        // 搜索关键词如果没有, 就把搜索结果清空阻止网络请求发送(提前return)
        this.resultList = [];
        return;
      }
      const res = await this.getListFn();
      console.log(res);
      
+      // 如果搜索结果响应数据没有songs字段-无数据
+      if (res.data.result.songs === undefined) {
+        this.resultList = [];
+        return;
+      }
      this.resultList = res.data.result.songs;
+        this.loading = false;
    },
    async onLoad() {
      // 触底事件(要加载下一页的数据咯), 内部会自动把loading改为true
      this.page++;
      const res = await this.getListFn();
+        if (res.data.result.songs === undefined) { // 没有更多数据了
+          this.finished = true; // 全部加载完成(list不会在触发onload方法)
+          this.loading = false; // 本次加载完成
+          return;
+        }
      this.resultList = [...this.resultList, ...res.data.result.songs];
+      this.loading = false; // 数据加载完毕-保证下一次还能触发onload
    },
```

> 总结: 在3个函数 上和下, 设置finished还未完成, 最后要把loading改成false, 判断songs字段, 对这里的值要非常熟悉才可以

### 1.16 网易云音乐-输入框-防抖

> 目标: 输入框触发频率过高

输入框输入"asdfghjkl"

​	接着快速的删除

​	每次改变-马上发送网络请求

​	网络请求异步耗时 – 数据回来后还是铺设到页面上

解决:

​	引入防抖功能

```js
async inputFn() {
    // 目标: 输入框改变-逻辑代码-慢点执行
    // 解决: 防抖
    // 概念: 计时n秒, 最后执行一次, 如果再次触发, 重新计时
    // 效果: 用户在n秒内不触发这个事件了, 才会开始执行逻辑代码
    if (this.timer) clearTimeout(this.timer);
    this.timer = setTimeout(async () => {
        this.finished = false; // 输入框关键字改变-可能有新数据(不一定加载完成了)
        // 输入框值改变
        if (this.value.length === 0) {
            // 搜索关键词如果没有, 就把搜索结果清空阻止网络请求发送(提前return)
            this.resultList = [];
            return;
        }
        const res = await this.getListFn();
        console.log(res);
        // 如果搜索结果响应数据没有songs字段-无数据
        if (res.data.result.songs === undefined) {
            this.resultList = [];
            return;
        }
        this.resultList = res.data.result.songs;
        this.loading = false;
    }, 900);
},
```

> 总结: 降低函数执行频率

### 1.17 网易云音乐-页码bug修复

> 目标: 第一个关键词page已经+到了10, 再第二个关键词应该从1开始

加载更多时, page已经往后计数了

重新获取时, page不是从第一页获取的

点击搜索/输入框搜索时, 把page改回1

代码如下:

```diff
 async fn(val) {
      // 点击热搜关键词
+      this.page = 1; // 点击重新获取第一页数据
      this.finished = false; // 点击新关键词-可能有新的数据
      this.value = val; // 选中的关键词显示到搜索框
      const res = await this.getListFn();
      console.log(res);
      this.resultList = res.data.result.songs;
      this.loading = false; // 本次数据加载完毕-才能让list加载更多
 },
 async inputFn() {
      // 目标: 输入框改变-逻辑代码-慢点执行
      // 解决: 防抖
      // 概念: 计时n秒, 最后执行一次, 如果再次触发, 重新计时
      // 效果: 用户在n秒内不触发这个事件了, 才会开始执行逻辑代码
      if (this.timer) clearTimeout(this.timer);
      this.timer = setTimeout(async () => {
+        this.page = 1; // 点击重新获取第一页数据
        this.finished = false; // 输入框关键字改变-可能有新数据(不一定加载完成了)
        // 输入框值改变
        if (this.value.length === 0) {
          // 搜索关键词如果没有, 就把搜索结果清空阻止网络请求发送(提前return)
          this.resultList = [];
          return;
        }
        const res = await this.getListFn();
        console.log(res);
        // 如果搜索结果响应数据没有songs字段-无数据
        if (res.data.result.songs === undefined) {
          this.resultList = [];
          return;
        }
        this.resultList = res.data.result.songs;
        this.loading = false;
      }, 900);
 },
```

> 总结: 切换时, 让page页面回到1

### 1.18 网易云音乐-Layout边距优化

> 目标: 上下导航会盖住中间内容

我们的头部导航和底部导航挡住了中间内容

给中间路由页面设置上下内边距即可

在Layout/index.vue中

```css
/* 中间内容区域 - 容器样式(留好上下导航所占位置) */
.main {
  padding-top: 46px;
  padding-bottom: 50px;
}
```

### 1.19 网易云音乐-SongItem封装

> 目标: 把首页和搜索结果的歌曲cell封装起来

![image-20210512144538038](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210512144538038.png)

创建src/components/SongItem.vue

```vue
<template>
  <van-cell center :title="name" :label="author + ' - ' + name">
    <template #right-icon>
      <van-icon name="play-circle-o" size="0.6rem"/>
    </template>
  </van-cell>
</template>

<script>
export default {
  props: {
    name: String, // 歌名
    author: String, // 歌手
    id: Number, // 歌曲id (标记这首歌曲-为将来跳转播放页做准备)
  }
};
</script>

<style scoped>
/* 给单元格设置底部边框 */
.van-cell {
  border-bottom: 1px solid lightgray;
}
</style>
```

Home/index.vue - 重构

==注意: author字段不同==

```vue
<SongItem v-for="obj in songList"
    :key="obj.id"
    :name="obj.name"
    :author="obj.song.artists[0].name"
    :id="obj.id"
></SongItem>
```

Search/index.vue - 重构

==注意: author字段不同==

```vue
<SongItem
          v-for="obj in resultList"
          :key="obj.id"
          :name="obj.name"
          :author="obj.ar[0].name"
          :id="obj.id"
></SongItem>
```

> 总结: 遇到重复标签要封装

### 1.20 网易云音乐-播放音乐

> 目标: 从预习资料拿到播放的api和页面, 配置好路由规则

==时间关系,这个页面不用写, 直接用, 注释在备课代码里写好了==

组件SongItem里 – 点击事件

api/Play.js – 提前准备好 – 接口方法

跳转到Play页面 – 把歌曲id带过进去

在SongItem.vue - 点击播放字体图标

```js
methods: {
    playFn(){
        this.$router.push({
            path: '/play',
            query: {
                id: this.id // 歌曲id, 通过路由跳转传递过去
            }
        })
    }
}
```

![image-20210512144906051](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20210512144906051.png)

> 总结: 准备好播放页, 点击播放传歌曲id过去, 到播放页-再请求响应数据和歌曲地址用audio标签播放

### 1.21 网易云音乐-vant适配

> 目标: 切换不同机型, ==刷新后==看看标签大小适配吗

* postcss – 配合webpack翻译css代码
* postcss-pxtorem – 配合webpack, 自动把px转成rem
* 新建postcss.config.js – 设置相关配置
* 重启服务器, 再次观察Vant组件是否适配

1. 下载postcss和==postcss-pxtorem@5.1.1==

   postcss作用: 是对css代码做降级处理

   postcss-pxtorem: 自动把所有代码里的css样式的px, 自动转rem

2. src/新建postcss.config.js

```js
module.exports = {
  plugins: {
    'postcss-pxtorem': {
      // 能够把所有元素的px单位转成Rem
      // rootValue: 转换px的基准值。
      // 例如一个元素宽是75px，则换成rem之后就是2rem。
      rootValue: 37.5,
      propList: ['*']
    }
  }
}
```

==以iphone6为基准, 37.5px为基准值换算rem==

## 今日总结

- [ ] 掌握vant组件库的使用 - 找组件, 引组件, 用组件
- [ ] 能够对vant组件自带样式进行覆盖自定义
- [ ] 遇到重复的标签, 自己也封装了一个复用的组件
- [ ] 掌握查询文档和使用每个属性的方式

## 今日作业

把课上的案例-从0再来一遍, 为下阶段移动端项目铺垫



# 第十章 Vuex



## vuex基础-介绍

>  为什么会有Vuex ?

​	Vuex 是一个专为 Vue.js 应用程序开发的**状态管理模式**。它采用**`集中式`**存储管理应用的所有组件的状态，并以相应的规则保证状态以一种**`可预测`**的方式发生变化。

- vuex是采用集中式管理组件依赖的共享数据的一个工具，可以解决不同组件数据共享问题。

![image-20200902235150562](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20200902235150562.png)

**结论**

1. 修改state状态必须通过**`mutations`**
2. **`mutations`**只能执行同步代码，类似ajax，定时器之类的代码不能在mutations中执行
3. 执行异步代码，要通过actions，然后将数据提交给mutations才可以完成
4. state的状态即共享数据可以在组件中引用
5. 组件中可以调用action

## vuex基础-初始化功能

> 建立一个新的脚手架项目, 在项目中应用vuex

```bash
$ vue create  demo
```

> 开始vuex的初始化建立，选择模式时，选择默认模式

初始化：

- 第一步：`npm i vuex --save`  => 安装到**`运行时依赖`**   => 项目上线之后依然使用的依赖 ,开发时依赖  => 开发调试时使用  

> 开发时依赖 就是开开发的时候，需要的依赖，运行时依赖，项目上线运行时依然需要的

- 第二步： **在main.js中** `import Vuex from 'vuex'`
- 第三步：**在main.js中**  `Vue.use(Vuex)`  => 调用了 vuex中的 一个install方法
- 第四步：`const store = new Vuex.Store({...配置项})`
- 第五步：在根实例配置 store 选项指向 store 实例对象

```js
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(vuex)
const store = new Vuex.Store({})
new Vue({
    render: h => h(App),
    store
}).$mount('#app')
```

## vuex基础-state

state是放置所有公共状态的属性，如果你有一个公共状态数据 ， 你只需要定义在 state对象中

**定义state**

```js
// 初始化vuex对象
const store = new Vuex.Store({
  state: {
    // 管理数据
    count: 0
  }
})
```

> 如何在组件中获取count?

**原始形式**- 插值表达式

**`App.vue`**

组件中可以使用  **this.$store** 获取到vuex中的store对象实例，可通过**state**属性属性获取**count**， 如下

```vue
<div> state的数据：{{ $store.state.count }}</div>
```

**计算属性** - 将state属性定义在计算属性中

```js
// 把state中数据，定义在组件内的计算属性中
  computed: {
    count () {
      return this.$store.state.count
    }
  }
```

```vue
 <div> state的数据：{{ count }}</div>
```

**辅助函数**  - mapState

>mapState是辅助函数，帮助我们把store中的数据映射到 组件的计算属性中, 它属于一种方便用法

用法 ： 第一步：导入mapState

```js
import { mapState } from 'vuex'
```

第二步：采用数组形式引入state属性

```js
mapState(['count']) 
```

> 上面代码的最终得到的是 **类似**

```js
count () {
    return this.$store.state.count
}
```

第三步：利用**延展运算符**将导出的状态映射给计算属性

```js
  computed: {
    ...mapState(['count'])
  }
```

```vue
 <div> state的数据：{{ count }}</div>
```

## vuex基础-mutations

> state数据的修改只能通过mutations，并且mutations必须是同步更新，目的是形成**`数据快照`**

数据快照：一次mutation的执行，**立刻**得到一种视图状态，因为是立刻，所以必须是同步

**定义mutations**

```js
const store  = new Vuex.Store({
  state: {
    count: 0
  },
  // 定义mutations
  mutations: {
     
  }
})
```

**格式说明**

mutations是一个对象，对象中存放修改state的方法

```js
mutations: {
    // 方法里参数 第一个参数是当前store的state属性
    // payload 载荷 运输参数 调用mutaiions的时候 可以传递参数 传递载荷
    addCount (state) {
      state.count += 1
    }
  },
```

> 如何在组件中调用mutations

**原始形式**-$store

> 新建组件child-a.vue，内容为一个button按钮，点击按钮调用mutations

``` vue
<template>
  <button @click="addCount">+1</button>
</template>

<script>
export default {
    methods: {
    //   调用方法
      addCount () {
         // 调用store中的mutations 提交给muations
        // commit('muations名称', 2)
        this.$store.commit('addCount', 10)  // 直接调用mutations
    }
  }
}
</script>
```

带参数的传递

```js
    addCount (state, payload) {
        state.count += payload
    }
    this.$store.commit('addCount', 10)
```

**辅助函数** - mapMutations

> mapMutations和mapState很像，它把位于mutations中的方法提取了出来，我们可以将它导入

```js
import  { mapMutations } from 'vuex'
methods: {
    ...mapMutations(['addCount'])
}
```

> 上面代码的含义是将mutations的方法导入了methods中，等同于

```js
methods: {
      // commit(方法名, 载荷参数)
      addCount () {
          this.$store.commit('addCount')
      }
 }
```

此时，就可以直接通过this.addCount调用了

```vue
<button @click="addCount(100)">+100</button>
```

但是请注意： Vuex中mutations中要求不能写异步代码，如果有异步的ajax请求，应该放置在actions中

## vuex基础-actions

> state是存放数据的，mutations是同步更新数据，actions则负责进行异步操作

**定义actions**

```js
 actions: {
  //  获取异步的数据 context表示当前的store的实例 可以通过 context.state 获取状态 也可以通过context.commit 来提交mutations， 也可以 context.diapatch调用其他的action
    getAsyncCount (context) {
      setTimeout(function(){
        // 一秒钟之后 要给一个数 去修改state
        context.commit('addCount', 123)
      }, 1000)
    }
 } 
```

**原始调用** - $store

```js
 addAsyncCount () {
     this.$store.dispatch('getAsyncCount')
 }
```

**传参调用**

```js
 addAsyncCount () {
     this.$store.dispatch('getAsyncCount', 123)
 }
```

**辅助函数** -mapActions

> actions也有辅助函数，可以将action导入到组件中

```js
import { mapActions } from 'vuex'
methods: {
    ...mapActions(['getAsyncCount'])
}
```

直接通过 this.方法就可以调用

```vue
<button @click="getAsyncCount(111)">+异步</button>
```

## vuex基础-getters

> 除了state之外，有时我们还需要从state中派生出一些状态，这些状态是依赖state的，此时会用到getters

例如，state中定义了list，为1-10的数组，

```js
state: {
    list: [1,2,3,4,5,6,7,8,9,10]
}
```

组件中，需要显示所有大于5的数据，正常的方式，是需要list在组件中进行再一步的处理，但是getters可以帮助我们实现它

**定义getters**

```js
  getters: {
    // getters函数的第一个参数是 state
    // 必须要有返回值
     filterList:  state =>  state.list.filter(item => item > 5)
  }
```

使用getters

**原始方式** -$store

```vue
<div>{{ $store.getters.filterList }}</div>
```

**辅助函数** - mapGetters

```js
computed: {
    ...mapGetters(['filterList'])
}
```

```vue
 <div>{{ filterList }}</div>
```

## Vuex中的模块化-Module

### 为什么会有模块化？

> 由于使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿。

这句话的意思是，如果把所有的状态都放在state中，当项目变得越来越大的时候，Vuex会变得越来越难以维护

由此，又有了Vuex的模块化

![image-20200904155846709](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20200904155846709.png)

### 模块化的简单应用

**应用**

定义两个模块   **user** 和  **setting**

user中管理用户的状态  token 

setting中管理 应用的名称 name

```js
const store  = new Vuex.Store({
  modules: {
    user: {
       state: {
         token: '12345'
       }
    },
    setting: {
      state: {
         name: 'Vuex实例'
      }
    }
  })
```

定义child-b组件，分别显示用户的token和应用名称name

```vue
<template>
  <div>
      <div>用户token {{ $store.state.user.token }}</div>
      <div>网站名称 {{ $store.state.setting.name }}</div>
  </div>
</template>
```

请注意： 此时要获取子模块的状态 需要通过 $store.**`state`**.**`模块名称`**.**`属性名`** 来获取

> 看着获取有点麻烦，我们可以通过之前学过的getters来改变一下

```js
 getters: {
   token: state => state.user.token,
   name: state => state.setting.name
 } 
```

请注意：这个getters是根级别的getters哦

**通过mapGetters引用**

```js
 computed: {
       ...mapGetters(['token', 'name'])
 }
```

### 模块化中的命名空间

**命名空间**  **`namespaced`**

> 这里注意理解

默认情况下，模块内部的 action、mutation 和 getter 是注册在**全局命名空间**的——这样使得多个模块能够对同一 mutation 或 action 作出响应。

> 这句话的意思是 刚才的user模块还是setting模块，它的 action、mutation 和 getter 其实并没有区分，都可以直接通过全局的方式调用 如

![image-20200904164007116](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20200904164007116.png)

```js
  user: {
       state: {
         token: '12345'
       },
       mutations: {
        //  这里的state表示的是user的state
         updateToken (state) {
            state.token = 678910
         }
       }
    },
```

**通过mapMutations调用**

```vue
 methods: {
       ...mapMutations(['updateToken'])
  }
 <button @click="updateToken">修改token</button>
```

> 但是，如果我们想保证内部模块的高封闭性，我们可以采用namespaced来进行设置

高封闭性？可以理解成 **一家人如果分家了，此时，你的爸妈可以随意的进出分给你的小家，你觉得自己没什么隐私了，我们可以给自己的房门加一道锁（命名空间 namespaced）,你的父母再也不能进出你的小家了**

如

```js
  user: {
       namespaced: true,
       state: {
         token: '12345'
       },
       mutations: {
        //  这里的state表示的是user的state
         updateToken (state) {
            state.token = 678910
         }
       }
    },
```

使用带命名空间的模块 **`action/mutations`**

方案1：**直接调用-带上模块的属性名路径**

```js
test () {
   this.$store.dispatch('user/updateToken') // 直接调用方法
}
```

方案2：**辅助函数-带上模块的属性名路径**

```vue
  methods: {
       ...mapMutations(['user/updateToken']),
       test () {
           this['user/updateToken']()
       }
   }
  <button @click="test">修改token</button>

```

方案3： **createNamespacedHelpers**  创建基于某个命名空间辅助函数

```vue
import { mapGetters, createNamespacedHelpers } from 'vuex'
const { mapMutations } = createNamespacedHelpers('user')
<button @click="updateToken">修改token2</button>
```

> 关于Vuex的更多用法，后续在项目中讲解

## vuex案例-搭建黑马头条项目

接下来，通过一个案例来使用Vuex介入我们的数据管理

> 通过vue-cli脚手架搭建项目

```bash 
$ vue create toutiao  #创建项目
```

> 选择  vuex / eslint（stanadard） / pre-cssprocesser (less)  确定  

**在main.js中引入样式**(该样式在**资源/vuex样式**中，拷贝到styles目录下)

```js
import './styles/index.css'
```

**拷贝图片资源到assets目录下**（在**资源/vuex样式目录下的图片**）

**在App.vue中拷贝基本结构**

```vue
 <div id="app">
      <ul class="catagtory">
        <li class='select'>开发者资讯</li>
        <li>ios</li>
        <li>c++</li>
        <li>android</li>
        <li>css</li>
        <li>数据库</li>
        <li>区块链</li>
        <li>go</li>
        <li>产品</li>
        <li>后端</li>
        <li>linux</li>
        <li>人工智能</li>
        <li>php</li>
        <li>javascript</li>
        <li>架构</li>
        <li>前端</li>
        <li>python</li>
        <li>java</li>
        <li>算法</li>
        <li>面试</li>
        <li>科技动态</li>
        <li>js</li>
        <li>设计</li>
        <li>数码产品</li>
        <li>html</li>
        <li>软件测试</li>
        <li>测试开发</li>
      </ul>
      <div class="list">
        <div class="article_item">
          <h3 class="van-ellipsis">python数据预处理 ：数据标准化</h3>
          <div class="img_box">
            <img src="@/assets/back.jpg"
            class="w100" />
          </div>
          <!---->
          <div class="info_box">
            <span>13552285417</span>
            <span>0评论</span>
            <span>2018-11-29T17:02:09</span>
          </div>
        </div>
      </div>
    </div>
```

## vuex案例-封装分类组件和频道组件

为了更好的区分组件之间的职责，我们将上方的频道和下方的列表封装成不同的组件

**`components/catagtory.vue`**

```vue
<template>    
   <ul class="catagtory">
        <li class='select'>开发者资讯</li>
        <li>ios</li>
        <li>c++</li>
        <li>android</li>
        <li>css</li>
        <li>数据库</li>
        <li>区块链</li>
        <li>go</li>
        <li>产品</li>
        <li>后端</li>
        <li>linux</li>
        <li>人工智能</li>
        <li>php</li>
        <li>javascript</li>
        <li>架构</li>
        <li>前端</li>
        <li>python</li>
        <li>java</li>
        <li>算法</li>
        <li>面试</li>
        <li>科技动态</li>
        <li>js</li>
        <li>设计</li>
        <li>数码产品</li>
        <li>html</li>
        <li>软件测试</li>
        <li>测试开发</li>
      </ul>
</template>    
```

**`components/new-list.vue`**

```vue
<template> 
  <div class="list">
        <div class="article_item">
          <h3 class="van-ellipsis">python数据预处理 ：数据标准化</h3>
          <div class="img_box">
             <img src="@/assets/back.jpg"
            class="w100" />
          </div>
          <!---->
          <div class="info_box">
            <span>13552285417</span>
            <span>0评论</span>
            <span>2018-11-29T17:02:09</span>
          </div>
        </div>
      </div>
</template>
```

**在App.vue中引入并使用**

```vue
<template>
 <!-- app.vue是根组件 -->
  <div id="app">
    <catagtory />
    <new-list />
  </div>
</template>
<script>
import Catagtory from './components/catagtory'
import NewList from './components/new-list'

export default {
  components: {
    Catagtory, NewList
  }
}
</script>

```

## vuex案例-在vuex中加载分类和频道数据

### 设计categtory和newlist的vuex模块

**安装请求数据的工具 axios**

```bash
$ npm i axios
```

**接口**

​    获取频道列表 

​            http://ttapi.research.itcast.cn/app/v1_0/channels

​    获取频道头条

​          http://ttapi.research.itcast.cn/app/v1_1/articles?channel_id=频道id&timestamp=时间戳&with_top=1

> 我们采用模块化的管理模式，建立一个专门的模块来管理分类和新闻数据

**在store目录下新建目录modules， 新建 catagtory.js和newlist.js**

**模块结构**

```js
export default {
  namespaced: true,
  state: {},
  mutations: {},
  actions: {}
}
```

**在store/index.js中引入定义的两个模块**

```js
import catagtory from './modules/catagtory'
import newlist from './modules/newlist'
 export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
    catagtory,
    newlist
  }
})

```

### 分类模块下设置分类数组和当前激活分类

**在catagtory的 state中定义分类频道列表和当前激活**

```js
state: {
    catagtory: [],
    currentCatagtory: ''
}
```

**定义更新频道列表的mutations**

```js
mutations: {
  updateCatagtory (state, payload) {
      state.catagtory = payload // 更新分类数据
   },
   updateCurrentCatagtory (state, payload) {
      state.currentCatagtory = payload
   }
}
```

**通过getters建立对于分类数据和当前分类的快捷访问**

```js
export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
    catagtory,
    newlist
  },
  getters: {
    catagtory: state => state.catagtory.catagtory, // 建立快捷访问
    currentCatagtory: state => state.catagtory.currentCatagtory
  }
})
```

### 遍历分类数据并判断激活class

**分类组件遍历vuex数据**

```js
import { mapGetters } from 'vuex'
computed: {
    ...mapGetters(['catagtory', 'currentCatagtroy'])
},
```

```vue
 <ul class="catagtory">
    <li :class="{ select: currentCatagtory === item.id }" v-for="item in catagtory"  :key="item.id">{{ item.name }}</li>
 </ul>
```

### 封装调用获取分类action,激活第一个分类

**定义获取频道列表的action,  将第一个频道激活**

```js
  actions: {
    async  getCatagtory (context) {
      const { data: { data: { channels } } } = await                  axios.get('http://ttapi.research.itcast.cn/app/v1_0/channels')
      context.commit('updateCatagtory', channels)
      context.commit('updateCurrentCatagtory', channels[0].id)
    }
  }
```

**初始化catagtory时调用action**

```js
import { mapGetters } from 'vuex'

export default {
  computed: {
    ...mapGetters(['catagtory'])
  },
  created () {
    this.$store.dispatch('catagtory/getCatagtory')
  }
}
```

**点击分类时，触发分类切换**

```vue
 <li @click="$store.commit('catagtory/updateCurrentCatagtory', item.id)" :class="{ select: currentCatagtroy === item.id }" v-for="item in catagtory"  :key="item.id">{{ item.name }}</li>

```

### 定义新闻数据，并封装获取新闻的Action

**在newlist.js中定义获取头条内容的数据**	

```js
state: {
   allData: {}
}
```

**定义更新头条内容的mutations**

```js
  mutations: {
    // payload 载荷  { 1: [], 2: [], 3: [], 4}
    updateList (state, { currentCatagtory, list }) {
      // 不是响应式的
      // state.allData[currentCatagtory] = list // 这样做事大错特错第  感觉不到变化 就不会通知组件
      state.allData = { ...state.allData, [currentCatagtory]: list }
      // 这句代码的含义 就相当于 在一个新的对象后面追加了一个属性  更新某个属性的内容
    }
  },
```

**定义根据分类标识获取新闻的action**

```js
  actions: {
    // 获取新闻列表数据
    // 分类id只能通过传递的方式传进来
    async getNewList (context, cataId) {
      const { data: { data: { results } } } = await axios.get(`http://ttapi.research.itcast.cn/app/v1_1/articles?channel_id=${cataId}&timestamp=${Date.now()}&with_top=1`)
      // results是新闻列表
      context.commit('updateList', { currentCatagtory: cataId, list: results })
    }
  }
```

### 监听激活分类，触发获取新闻Action

**在new-list组件中，引入当前分类的id，监视其改变，一旦改变，触发获取新闻的action** 

```js
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['currentCatagtroy'])
  },
  watch: {
    currentCatagtory (newValue) {
      this.$store.dispatch('newlist/getNewList', newValue)
    }
  }
}
```

### 处理显示新闻内容的数据

**定义当前显示列表的getters**

```js
getters: {
    currentList: state => state.newlist.allData[state.catagtory.currentCatagtory] || []
}
```

**修改new-list内容**

```vue <div class='list'>
<template>
     <div class="list">
        <div class="article_item" v-for="item in currentList" :key="item.art_id">
          <h3 class="van-ellipsis">{{ item.title }}</h3>
          <div class="img_box" v-if="item.cover.type === 1">
            <img :src="item.cover.images[0]"
            class="w100" />
          </div>
          <div class="img_box" v-else-if="item.cover.type === 3">
            <img :src="item.cover.images[0]"
            class="w33" />
             <img :src="item.cover.images[1]"
            class="w33" />
             <img :src="item.cover.images[2]"
            class="w33" />
          </div>
          <!---->
          <div class="info_box">
            <span>{{ item.aut_name }}</span>
            <span>{{ item.comm_count }}评论</span>
            <span>{{ item.pubdate }}</span>
          </div>
        </div>
      </div>
</template>

<script>
// 引入当前激活的分类id
import { mapGetters } from 'vuex'
export default {
  computed: {
    ...mapGetters(['currentCatagtory', 'currentList'])
  },
  watch: {
    currentCatagtory (newValue) {
      // newValue是当前最新的激活的id
      this.$store.dispatch('newlist/getNewList', newValue)
    }
  }
}
</script>

<style>

</style>

```



![image-20201012181147093](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20201012181147093.png)

# 第十一章 Element的表单校验补充

## 表单校验

> 我们尝试通过一个案例对Element的表单校验进行一下补充

### 实现表单基本结构

**创建项目**

```bash
$ vue create login
```

> 选择babel / eslint

**安装Element** 

开发时依赖 ： 开发环境所需要的依赖 ->  devDependencies

运行时依赖: 项目上线依然需要的依赖 -> dependencies

```bash
$ npm i element-ui
```

**在main.js中对ElementUI进行注册**

```js
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
Vue.use(ElementUI);
```

接下来,利用Element组件完成如图的效果

![image-20200906184428291](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20200906184428291.png)

代码如下

```vue
<template>
  <div id="app">
    <!-- 卡片组件 -->
    <el-card class='login-card'>
      <!-- 登录表单 -->
      <el-form style="margin-top: 50px">
        <el-form-item>
          <el-input placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input placeholder="请输入密码"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" style="width: 100%">登录</el-button>
        </el-form-item>

      </el-form>
    </el-card>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {

  }
}
</script>

<style>
 #app {
   width: 100%;
   height: 100vh;
   background-color: pink;
   display: flex;
   justify-content: center;
   align-items: center;
 }
 .login-card {
   width: 440px;
   height: 300px;
 }
</style>

```

### 表单校验的先决条件

接下来，完成表单的校验规则如下几个先决条件

![image-20200906184604982](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20200906184604982.png)

**model属性** (表单数据对象)

```js
  data () {
    // 定义表单数据对象
    return {
      loginForm: {
        mobile: '',
        password: ''
      }
    }
  }
```

**绑定model**

```vue
 <el-form style="margin-top:40px" :model="loginForm" >
```

**rules规则**  先定义空规则，后续再详解

```js
loginRules: {}
<el-form style="margin-top: 50px" model="loginForm" :rules="loginRules">
```

**设置prop属性**

> 校验谁写谁的字段

```vue
<el-form-item prop="mobile">
   ...
<el-form-item prop="password">
   ...
```

**给input绑定字段属性**

```vue
<el-input v-model="loginForm.mobile"></el-input>
<el-input v-model="loginForm.password"></el-input>
```

### 表单校验规则

此时，先决条件已经完成，要完成表单的校验，需要编写规则

> ElementUI的表单校验规则来自第三方校验规则参见 [async-validator](https://github.com/yiminghe/async-validator)

我们介绍几个基本使用的规则

| 规则      | 说明                                                         |
| --------- | ------------------------------------------------------------ |
| required  | 如果为true，表示该字段为必填                                 |
| message   | 当不满足设置的规则时的提示信息                               |
| pattern   | 正则表达式，通过正则验证值                                   |
| min       | 当值为字符串时，min表示字符串的最小长度，当值为数字时，min表示数字的最小值 |
| max       | 当值为字符串时，max表示字符串的最大长度，当值为数字时，max表示数字的最大值 |
| trigger   | 校验的触发方式，change（值改变） / blur （失去焦点）两种，   |
| validator | 如果配置型的校验规则不满足你的需求，你可以通过自定义函数来完成校验 |

校验规则的格式

***{ key(字段名): value(校验规则) => [{}] }***

根据以上的规则，针对当前表单完成如下要求

**手机号**  1.必填 2.手机号格式校验 3. 失去焦点校验

**密码** 1.必填 2.6-16位长度 3. 失去焦点校验

**规则如下**

```js
      loginRules: {
        mobile: [{ required: true, message: '手机号不能为空', trigger: 'blur' },
          { pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号', trigger: 'blur' }],
        password: [{ required: true, message: '密码不能为空', trigger: 'blur' }, {
          min: 6, max: 16, message: '密码应为6-16位的长度', trigger: 'blur'
        }]
      }
```

### 自定义校验规则

> 自定义校验规则怎么用

**`validator`**是一个函数, 其中有三个参数 (**`rule`**(当前规则),`value`(当前值),**`callback`**(回调函数))

```js
var  func = function (rule, value, callback) {
    // 根据value进行进行校验 
    // 如果一切ok  
    // 直接执行callback
    callback() // 一切ok 请继续
    // 如果不ok 
    callback(new Error("错误信息"))
}
```

根据以上要求，增加手机号第三位必须是9的校验规则

如下

```js
// 自定义校验函数
    const checkMobile = function (rule, value, callback) {
      value.charAt(2) === '9' ? callback() : callback(new Error('第三位手机号必须是9'))
    }

 mobile: [
          { required: true, message: '手机号不能为空', trigger: 'blur' },
          { pattern: /^1[3-9]\d{9}$/, message: '请输入正确的手机号', trigger: 'blur' }, {
            trigger: 'blur',
            validator: checkMobile
   }],
```

### 手动校验的实现

>  最后一个问题，如果我们直接点登陆按钮，没有离开焦点，那该怎么校验 ？

此时我们需要用到手动完整校验  [案例](https://element.eleme.cn/#/zh-CN/component/form)

form表单提供了一份API方法，我们可以对表单进行完整和部分校验

| 方法名        | 说明                                                         | 参数                                                         |
| :------------ | :----------------------------------------------------------- | :----------------------------------------------------------- |
| validate      | 对整个表单进行校验的方法，参数为一个回调函数。该回调函数会在校验结束后被调用，并传入两个参数：是否校验成功和未通过校验的字段。若不传入回调函数，则会返回一个 promise | Function(callback: Function(boolean, object))                |
| validateField | 对部分表单字段进行校验的方法                                 | Function(props: array \| string, callback: Function(errorMessage: string)) |
| resetFields   | 对整个表单进行重置，将所有字段值重置为初始值并移除校验结果   | —                                                            |
| clearValidate | 移除表单项的校验结果。传入待移除的表单项的 prop 属性或者 prop 组成的数组，如不传则移除整个表单的校验结果 | Function(props: array \| string)                             |

这些方法是el-form的API，需要获取el-form的实例，才可以调用

**采用ref进行调用**

```vue
<el-form ref="loginForm" style="margin-top:40px" :model="loginForm" :rules="loginRules">

```

**调用校验方法**

```js
  login () {
      // 获取el-form的实例
      this.$refs.loginForm.validate(function (isOK) {
        if (isOK) {
          // 说明校验通过
          // 调用登录接口
        }
      }) // 校验整个表单
    }
```

## Async 和 Await 

 针对异步编程，我们学习过Ajax的回调形式，promise的链式调用形式

**ajax回调模式**

```js
// 回调形式调用
$.ajax({
    url,
    data,
    success:function(result){
        $.ajax({
            data:result,
            success: function(result1){
                $.ajax({
                    url,
                    data: result1
              })
            }
        })
    }
})
```

**promise的链式回调函数**

```js
// 链式调用 没有嵌套
axios({ url, data}).then(result => {
    return  axios({ data:result }) 
}).then(result1 => {
     return  axios({ data:result1 }) 
}).then(result2 => {
   return axios({ data: result2 }) 
}).then(result3 => {
    return axios({ data: result3 }) 
})
```

### 关于Promise你必须知道几件事

> 关于Promise你必须知道几件事

如何声明一个Promise

```js
new Promise(function(resolve, reject){ })
```

如果想让Promise成功执行下去，需要执行resolve，如果让它失败执行下去，需要执行reject

```js
new Promise(function(resolve, reject) { 
    resolve('success')  // 成功执行
}).then(result => {
    alert(result)
})

new Promise(function(resolve, reject) { 
    reject('fail')  // 成功执行
}).then(result => {
    alert(result)
}).catch(error => {
     alert(error)
})
```

如果想终止在某个执行链的位置，可以用**Promise.reject(new Error())**

```js
new Promise(function(resolve, reject) {
    resolve(1)
}).then(result => {
    return result + 1
}).then(result => {
    return result + 1
}).then(result => {
  return  Promise.reject(new Error(result + '失败'))
   // return result + 1
}).then(result => {
    return result + 1
}).catch(error => {	
    alert(error)
})
```

### 异步编程的终极方案  **async /await**

> async 和 await实际上就是让我们像写同步代码那样去完成异步操作

**await** 表示强制等待的意思，**await**关键字的后面要跟一个promise对象，它总是等到该promise对象resolve成功之后执行，并且会返回resolve的结果

```js
 async test () {
      // await总是会等到 后面的promise执行完resolve
      // async /await就是让我们 用同步的方法去写异步
      const result = await new Promise(function (resolve, reject) {
        setTimeout(function () {
          resolve(5)
        }, 5000)
      })
      alert(result)
    }
```

上面代码会等待5秒之后，弹出5

**async 和 await必须成对出现**

由于await的强制等待，所以必须要求使用**await**的函数必须使用**async**标记， async表示该函数就是一个异步函数，不会阻塞其他执行逻辑的执行

```js
async test () {
      const result = await new Promise(function(resolve){  
         setTimeout(function(){
             resolve(5)
         },5000)
       })
       alert(result)
    },
    test1(){
      this.test()
      alert(1)
    }
```

通过上面的代码我们会发现，异步代码总是最后执行，标记了async的函数并不会阻塞整个的执行往下走

> 如果你想让1在5弹出之后再弹出，我们可以这样改造

```js
   async test1(){
     await this.test()
      alert(1)
   }
// 这充分说明 被async标记的函数返回的实际上也是promise对象
```

> 如果promise异常了怎么处理？

 promise可以通过catch捕获，async/ await捕获异常要通过 try/catch

```js
   async  getCatch () {
      try {
        await new Promise(function (resolve, reject) {
          reject(new Error('fail'))
        })
        alert(123)
      } catch (error) {
        alert(error)
      }
   }
```

async / await  用同步的方式 去写异步

# 第十二章 VUE3 新方法
## API风格
官网解释：[API风格](https://cn.vuejs.org/guide/introduction.html)
[响应式基础](https://cn.vuejs.org/guide/essentials/reactivity-fundamentals.html)
1. 选项式API (Options API)
   使用选项式 API，我们可以用包含多个选项的对象来描述组件的逻辑，例如 `data`、`methods` 和 `mounted`。选项所定义的属性都会暴露在函数内部的 `this` 上，它会指向当前的组件实例。就是vue比较常用的方法。
2. 组合式API
   通过组合式 API，我们可以使用导入的 API 函数来描述组件逻辑。在单文件组件中，组合式 API 通常会与 <script setup> 搭配使用。



# 附录(list的各种方法)

![image-20221028235155217](https://gitee.com/XXXTENTWXD/pic/raw/master/images/image-20221028235155217.png)

- 根据条件筛选获得新数组

```js
//选出数组中属性满足isDoneD等于true的并返回一个新的数组
this.list.filter(obj => obj.isDone === true)
```

- 




# 图片

![添加群聊](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E6%B7%BB%E5%8A%A0%E7%BE%A4%E8%81%8A.png)

![24gl-userGroupPlus2](https://gitee.com/XXXTENTWXD/pic/raw/master/images/24gl-userGroupPlus2.png)

![数译_群聊](https://gitee.com/XXXTENTWXD/pic/raw/master/images/%E6%95%B0%E8%AF%91_%E7%BE%A4%E8%81%8A.png)

