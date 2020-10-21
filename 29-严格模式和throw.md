### 严格模式 松散模式

+ 一般情况下，js 运行在松散模式下。为了让语言越来越规范，引入了严格模式
- 使用严格模式：在脚本的第一行增加一个字符串 'use strict'

```javascript
"use strict"
// ??松散模式和严格模式不同
// 1. 严格模式下，函数形参和 arguments 不再存在映射关系
function sum(a, b) {
  b = 15; // 松散模式下，形参和 arguments 存在映射关系，如果在函数中修改形参的值，arguments 中对应的值也会跟着修改
  console.log(arguments);// [1, 15]
}
sum(1, 2);

// webpack 打包时，会把js文件默认添加 "use strict"

// 2. 严格模式下，call 不传参时，函数中的 this 是 undefined；严格模式下，call 谁，函数中的 this 就是谁
function sum2(a, b) {
  console.log(this);
}
var obj = {
  name: '珠峰'
};
sum2(); // window

// 严格模式下
sum2.call(obj); // obj
sum2.call(); // -> undefined
sum2.call(null); // null
sum2.call(undefined); // undefined

// 3. 严格模式不能给未声明的变量赋值，如果赋值就会报错；
// aa = 1; 严格模式下会报错
// console.log(aa);
```

### throw 手动抛出错误

+ throw 关键字：用来手动抛出异常，抛出异常后，后面的代码就不会执行（同步的操作）throw 后面不论写什么，都会当成错误抛出

- 常见的内置错误类型
1. ReferenceError 引用错误，一般由于引用未声明的变量导致
throw new ReferenceError('引用错误');
2. TypeError 类型错误，一般由于搞错类型（不是函数的变量或者值后面跟一个小括号）
 throw new TypeError('你搞错类型了');
3. SyntaxError 语法错误，一般因为语法写的不对
throw new SyntaxError('你的语法真差啊');
4. Error 普通错误
throw Error('这是普通错误'); // 可以不用 new

结合 try-catch 根据不同报错类型 采取不同措施

```javascript
try {
  throw new SyntaxError('类型错误');
} catch (e) {
  // 根据报错类型不同，采取不同的措施
  if (e instanceof TypeError) {
    console.log(1);
  } else if (e instanceof SyntaxError) {
    console.log(2);
  }
}
```