### 什么是 node

+ Nodejs 不是一门新的语言，而是一个可以让 js 运行在服务端的运行环境；Nodejs 基于 v8 引擎；在服务器上安装 nodejs 以后，就可以用 js 开发服务端程序了，所以 js 现在是全栈开发语言

+ 前后端通过 http 协议通信；前端通过 http 协议把请求发送给服务器，服务器接收客户端的请求，准备客户端需要的数据，最后通过 HTTP 协议把数据响应给客户端；

+ 服务器的作用
1. 存储前后端的代码（js html css 图片等）
2. 处理客户端的请求
3. 响应客户端的请求（把数据发送给客户端）
4. 服务器上还可以安装数据库保存用户数据

+ node.js 的优势
1. Nodejs 可以写任何最新的技术、ES6 / ES7 任意用，没有兼容性问题
2. 单线程异步，事件驱动
3. 事件驱动的非阻塞 I / O，轻量高效；（采用异步的方式处理 Input / Output 输入 / 输出）
4. npm 是包管理器，npmjs.com 是全球最大的 node 开源生态社区
5. npm 是随着 node 一起安装的，装完 node，npm 就自动安装了
6. npm 包是别人做好的库，我们只需要安装，然后导入后就可以使用了
7. yarn 也是 node 包管理器，yarn 的特点就是速度快

```javascript
// 装 nrm ：npm install nrm -g
// nrm 用来切换源（源就是从哪里下载）
// nrm ls 查看所有的源
// nrm use taobao
```

### nodejs 的模块

1. 在 nodejs 中没有 html 了，js 之间引用就需要模块化
2. 在 nodejs 中，所有的 js 文件都是模块，所有的方法和逻辑都要写在模块内；

+ node 的模块分为三种
1. 内置模块：安装 node 时，随着 node 一起安装的模块，这些模块都是 nodejs 的核心模块；如 http 模块，fs 模块
2. 自定义模块：自己封装的
3. 第三方模块：通过 npm 或者 yarn 安装的模块如 nrm yarn lessc 等

### text

+ 因为 node 没有 html 了，如果 js 文件互相依赖，就要使用模块化把这个 js 文件中的内容导出，需要这些内容的模块导入；
+ 所谓模块化：把别人需要的东西导出去，如果不导出，别人拿不到；把自己需要的东西导入进来；别人的模块导出什么，我们导入的时候就能拿到什么，别人没导出的我们拿不到；

```javascript
function fn() {
  return 1 + 1;
}
let xxxx = 12;

console.log(fn());

// 导出方式：
// 1. 直接导出 fn
// module.exports = fn;
// module.exports = xxxx;

// 2. exports.fn = fn; 导出一个对象，对象中有一个属性 fn，属性值是 fn 这个函数;
// 如果要导出多个，使用 exports.属性名 = 属性值 形式；其中属性名自定义；但是你导出的时候叫什么，导入时还需要使用同名的属性；

exports.xyz = fn;
exports.abc = xxxx;

// 在 nodejs 中运行 js
// 1. 找到你要运行的 js 文件的目录
// 2. 在 js 文件所在的目录中，按住 shift 键 点击鼠标右键 选择 在此处打开 powershell 窗口（win7 在此处打开命令窗口）
// 3. 在命令行界面中输入：node 文件名 回车

// 在 webstorm / vscode 中使用 terminal（终端运行）
// 1. 在项目中找到 js 所在的文件夹
// 2. 在 js 所在文件夹右键，选择 Open in terminal （在终端中打开）
// 3. node 文件名 回车

// 插件运行：
// 1. 安装 code runner 插件，然后 右键 Run code
// 2. webstorm 在 js 文件中右键 Run 文件名
```

### 导入模块

导入模块：使用 require('被依赖 js 文件的路径')方法

```javascript
// 因为 4-使用模块.js 需要使用 3-test.js 中的 fn 方法；所以需要导入 3-test.js

let obj = require('./3-test'); // 在导入 3-test.js 时，3-test.js 中的代码就会执行

console.log(obj.xyz());
console.log(obj.abc);
```

### node 模块原理

+ 目录中不能有汉字，文件夹和文件不能叫 node / npm
+ 在浏览器中，json setTimeout 等都是定义在window 对象中
+ node 中没有 window 对象
+ node 中的全局对象是 global
+ Node 天生自带模块化，node 会给 js 文件外面套一个自执行函数，并且给自执行函数传入几个实参：exports, module, require, __dirname, __filename

```javascript
(function (exports, module, requrie, __dirname, __filename) {
  // 这里面才是咱们写的 js 代码
  
})(exports, module, require, __dirname, __filename);

// console.log(exports); // 导出
// console.log(module); // 模块
// console.log(require); // 导入模块的方法
console.log(__dirname); // 是当前 js 文件所在的目录的绝对路径
console.log(__filename); // 当前 js 的文件名，带绝对路径和扩展名
```

### 第三方模块

```javascript
// 第三方的模块：
// 我们使用的第三方的模块也称为依赖包。需要使用包管理器 npm 或者 yarn 安装（下载）；

// 通过 npm 在没有依赖包的目录中安装依赖的步骤：
// 1. npm init -y
// npm 生成一个 package.json 文件，这个 package.json 是用来记录当前项目中的依赖包的；

// 2. 安装项目依赖（根据你的项目需要安装依赖）
// 生产依赖和开发依赖：
// 生产依赖：项目在运行的时候需要的包，比如 jquery
// 开发依赖：只在开发的过程中有用的依赖包（大多数都是工具性的包，比如 gulp、webpack 等打包工具）

// 3. 安装依赖包的命令：npm install 依赖包名称 参数

// npm install 参数：
// 1. 无参数 安装到 node_modules 中，默认当做生产依赖；
// 2. -g 全局安装，全局安装后在命令行里面可以使用；
// 3. -save 安装生产依赖，把依赖包下载 node_modules 下，并且把这个包的信息添加到 package.json 下的 dependencies 中
// 4. -save-dev 安装开发依赖，把依赖包下载到 node_modules 下，并且把这个包的信息添加到 package.json 下的 devDependencies

// package.json
// dependencies 记录的当前项目所需要的生产依赖包；
// devDependencies 记录的是当前项目所需要的开发依赖；

// node_modules: 安装依赖的时候会自动生成这样一个文件夹；所有通过 npm 或者 yarn 安装的依赖都会放在这个里面；

// package.json 记录了当前项目所需的依赖包和版本；
// node_modules 文件夹不会来回传递，如果上线并不会把 node_modules 传到服务器上；
// 因为 package.json 文件记录了所有的依赖包，我们只需要在 package.json 所在的项目目录执行 npm install
// npm install 根据 package.json 中记录的开发依赖和生产依赖去安装对应的依赖包；

// 清除 NPM 的缓存：
//  npm cache clean --force

// MAC：sudo 以管理员身份运行该命令
// sudo npm install
```

### 内置模块

内置模块：Node.js 内置模块是随着 Node 一起安装的，都是 Node 核心模块

fs 模块：fs（file system）是用来读写文件的，专门用来处理文件的读写；

```javascript
// 1. 使用 fs 要先导入 fs 模块，其他内置模块也需要在使用前导入；
let fs = require('fs'); // 导入内置模块和第三方的模块时都不需要写路径；导入自定义模块时必须写路径；

// 1. 异步读取文件
// fs.readFile(fileName, option, callback);
// fileName: 文件名
// option: 设置读取的内容为哪种编码，option 的默认值是 buffer，存储二进制的数据，一般在机器之间传递可以直接传二进制；
// callback: 回调函数 读取文件后异步执行的回调函数
fs.readFile('./1.txt', function (err, data) {
  // 当读取失败时 err 是一个对象，读取成功时是 null

  if (err) {
    console.log(err);
  } else {
    console.log(data); // 读取时默认使用 buffer，buffer 存储二进制数据
    console.log(data.toString()); // buffer.toString() 就可以变成汉字
  }
});

fs.readFile('./1.txt', 'utf8',function (err, data) {

  // 当读取失败时 err 是一个对象，读取成功的是 null

  if (err) {
    console.log(err);
  } else {
    console.log(data); // 设置为 utf8 以后自动把读取到的内容变成 utf8 编码的
  }
});

// 2. 同步读取文件
// fs.readFileSync(fileName, options);
// fileName: 文件名
// options: 编码
// 这个方法会返回读取到的内容；如果读取失败直接报错
let data = fs.readFileSync('./1.txt', 'utf8');
console.log(data);
```