/*
* 浏览器内核：内核就是把按照规范开发的代码按照规范渲染成图形或者页面；
* 常见的浏览器内核：
*   1. webkit: 谷歌浏览器内核（chrome）、国产浏览器的极速模式
*   2. gecko: FireFox(FF)火狐浏览器
*   3. trident: IE 浏览器
*   ....
*  浏览器的兼容问题：
*   1. 浏览器开发商在实现规范的时候，为了突出自己更优秀，采用的方式不一样，导致结果不一致
*   2. 浏览器开发商还会开发出新功能，在这个功能没有成为规范之前，就存在兼容问题
* */

/*
* js有三部分组成：
*   1. ECMAScript：是js语言的规范，规定了js的语法、数据类型、流程控制语句
*   2. DOM：Document Object Model 文档对象模型；提供api（属性和方法）让我们操作页面中的html元素;
*   3. BOM: Browser Object Model 浏览器对象模型；提供api让我们可以操作浏览器；
* */

/*
 * 变量命名规范：
 *   1. js中的变量名严格区分大小写， var a = 1; var A = 1; A和a是两个不同的变量
 *   2. 驼峰命名法：可以使用字母、数字、下划线(_)，但是数字不能作为开头。除第一个单词的首字母不用大写，其余单词首字母都需要大写；例如 var studentInfoGrade
 *   3. 不能使用关键字、保留字；（ECMAScript规定的，如果查看需要查ECMAScript规范）
 *      关键字：有特殊含义的单词或者字母组合， var let function class import ...
 *      保留字：将来可能成为关键字的单词。
 *      建议：变量语义化，用单词尽可能描述这个变量的用途；
 * */

 ### 数据类型

> 基本数据类型
• 数字 number    常规数字和 NaN
• 字符串 string    所有用单引号、双引号、反引号（撇）包起来的都是字符串
• 布尔 boolean    true / false
• 空对象指针 null
• 未定义 undefined
• 唯一值 symbol（）

> 引用数据类型
• 对象数据类型 object
• 普通对象 {} 
• 数组对象 [] 
• 正则对象 /(\d|([1-9]\d+))(.\d+)?$/ 
• 数学函数对象 Math
• 日期对象
• 函数数据类型 function

bigint 语法：在数字后面加个 n；因为 JS 中的 Number 类型只能安全地表示 -9007199254740991 (-(2^53-1)) 和 9007199254740991(2^53-1)之间的整数，任何超出此范围的整数值都可能失去精度。所以有了 bigint；
补充： Infinity 无穷大  -Infinity 无穷小

number： NaN 表示非有效字符，NaN 和任何数都不相等 NaN 永远不等于 NaN
null：空 什么都没有 空对象指针
undefined：未定义
symbol：唯一的值

### 输出方式

> 控制台输出
- console.log() 控制台输出
- console.dir() 控制台详细打印
- console.table() 把一个多维 json 数组在控制台按照表格的形式呈现出来

> 弹窗
- alert 普通弹窗
- confirm 选择型弹窗 确定和取消
- prompt 在 confirm 的基础上多了一个输入框
它们输出的结果都是字符串
这三种方式会阻断 js 代码执行，只有当窗口关掉，js 才会继续执行

### 检测数据类型

1. typeof 运算符
typeof(null) // object 因为 null 是空对象指针
typeof 不能检测引用数据类型中的对象数据类型的具体细分（不能区分数组、对象、正则）一律返回 "object"

```javascript
function sum() {
	let a = 0;
}
typeof typeof typeof sum;
// 'string'
// 执行顺序：先执行最后一个 typeof sum -> "function" -> typeof typeof "function" -> typeof "string" -> "string"
// 只要是两个及以上的typeof 最后返回结果就是 "string"
```

2. constructor 运算符
3. instanceof 运算符
4. Object.prototype.toStrinig.call() 方法

### 其他类型转换为数字类型

#### isNaN
NaN：not a number
执行的结果叫做返回值
1. isNaN() 函数：用来检测值是不是 NaN。是就返回 true，不是就返回 false
2. isNaN() 检测的不是 number 类型的也可以得到结果，因为 isNaN() 会首先判断被检测的值是不是 number，如果不是会自动转成 number；内部调用 Number() 函数，把非 number 的转成 number

#### 转换规则
1. Number('12.34'); 字符串使用 Number 转换时，要求引号里面全是数字（可以是小数），就可以成功转换成数字，如果里面有一个非数字字符就会得到 NaN
2. Number(布尔值)，布尔值转成数字：true -> 1, false -> 0
3. Number(null); null 转成数字是 0
4. Number(undefined); undefined 转成数字就是 NaN
5. 引用数据类型转换成数字的时候，先调用引用数据类型的 toString() 函数将引用数据类型转换成字符串，然后再用 Number 转这个字符串。
- isNaN({}); -> 对象转成字符串 ({}).toString() -> "[object Object]"，然后我们Number("[object Object]") -> NaN
- isNaN([12]); -> [12].toString() -> "12" -> Number("12") -> 12
- isNaN([12, 23]); -> [12, 23].toString() -> "12,23" -> Number('12,23') -> NaN
6. parseInt() / parseFloat() 把字符串转成数字的，和 Number 类似，Number 是强制转化，字符串里面有一个不是数字字符，就会得到 NaN；
7. parseInt(字符串) 函数：从字符串左侧开始查找，把找到的整数部分返回。如果第一个字符就不是数字，直接返回 NaN
- parseFloat() 函数：从字符串左侧开始查找，把找到的整数和小数一起返回，只能识别一个小数点。如果字符串第一个字符就不是数字，就直接返回 NaN

### 对象

> 声明一个对象
let obj = {}
属性名可以是数字字母下划线的组合

> 对象.属性名
获取对象的属性高值：对象.属性名 或者 对象['属性名']；返回属性名对应的属性值。访问不存在的属性名会返回 undefined

> 增加或者修改
增加或者修改对象的属性名: 对象的属性名具有唯一性，不能有有重复的属性名存在。增加或者修改都是通过赋值的方式完成的；如果这个属性名在之前的对象中已经存在了，就是修改这个属性；如果这个属性名在之前的对象中不存在，就是为这个对象增加一个属性；

> 删除
软删除：将属性值修改为 null。属性名还存在，只不过属性值是 null
彻底删除：delete 对象.属性名 或者 delete 对象['属性名'] ;删除后，属性名和属性值都不存在了

### 数组

第一项是的索引是0； length 代表数组项个数；最后一项的索引是 length - 1；
数组的键都是数字，所以只能用方括号的方式操作；
访问一个数组不存在的索引，返回undefined

### ！和 ！！

！转成布尔值 然后取反
！！ 转成布尔值

### 判断语句

1. if else
2. a > b ? 1 : 0;
3. switch case
switch (值)，和 case 的值是绝对比较，值要相同、类型也要相同
每个 case 后面都需要加 break，如果不加就会不管后面条件是否成立，把后面的 case 及 default 都执行，直到遇到下一个 break 停下来。
4. 逻辑运算符 || 和 &&

> 条件怎么写
1. 比较运算符：<、<=、>、>=、!= 不等于、这些都会返回布尔值
2. == 相对比较：不比较类型，只比较值，如果值相同就是 true，所以 1 == '1' true
3. === 绝对比较，值不但要相同、类型也得相同。所以 1 === '1' false
4. 数学表达式：+ - * / %，如果条件里面是一个数学表达式，会等着计算出结果，然后再把这个结果转换成布尔值；数学运算符除 + 外，和非数字运算，会把两边都转成数字，在计算。

### null 和 undefined 的区别

首先 mull 和 undefined 是两种数据类型
typeof null -> "object" 因为 null 空对象指针；
typeof undefined -> "undefined"
Number(null) -> 0 
Number(undefined) -> NaN
访问对象、数组不存在的键或者索引返回undefined

### for in

用来遍历普通对象的。可以在循环时把对象的每个键取出来，只能把对象的可枚举属性取出来（浏览器添加的一般都是不可枚举属性，可枚举的一般是我们自定义的属性）

```javascript
for (var key in obj) {
  // key 是每次循环时对象中的一个键
  // 对象有多少个键，for in 循环就执行多少回
  console.log(key);
  console.log(obj[key]); // 因为 key 是一个变量，所以只能通过 对象[key] 的方式获取每次循环时对象的值
}
```
continue: 结束本次循环
break: 退出循环

### 基本数据类型和引用数据类型的区别

基本数据类型的操作是值类型的操作，就是说变量就真的代表它本身，let a = 12；12是基本数据类型，所以变量 a 就真的代表12这个值；引用数据类型操作的不是值本身，是这个引用数据类型存储的堆内存空间地址，代码以字符串形式存储在该堆内存空间地址中，执行时再一行一行的执行

### 获取元素

> 直接获取的是节点，可以直接操作

通过 id 获取    document.getelementById("id名")；

> 获取节点集合，必须加上 [] 才能操作节点

1. 通过标签名获取
document.getElementsByTagName("标签名")；
2. 通过 name 属性获取 
document.getElementsByName("name");
3. 通过类名获取 
document.getElementsByClassName("类名")；

> 通过选择器获取

1. 获取节点集合 nodelist
document.querySelectorAll("选择器")
2. 获取单个节点而且只能获取一个，如果选择器能选到多个，那么默认拿第一个
document.querySelector("选择器")

> 改变节点内容
document.querySelector("选择器").innerHTML = "需要改变的内容"

### 函数

```javascript
function fun(a, b) {
	// 函数声明
	// 使用 function 关键字声明函数
	// fun 叫做函数名
	// (a, b) 叫做形参入口，a b 叫做形参，形参是非必须的 可以设置默认值
	// 函数中的 {} 叫做函数体。把我们需要的功能和逻辑写在函数体中
	
	// 函数执行
	// 函数声明后并不会发挥作用，即函数体中的代码不会执行。所以要想发挥作用，我们还需要让函数执行。
	// 函数执行语法：函数名(实参); 实参也是非必须的
	// 函数执行表示让函数体里面的代码执行，此外函数名() 这个表达式还表示函数执行后留下的结果；
	// 函数可以多次的执行，每次执行都是独立的，不存在互相影响。
	
	// 函数参数   =>【连接词】：对比我们写的函数和 isNaN，我们也想要我们给它什么，函数帮我们处理什么。
	// 参数就是函数的入口，当我们在函数中封装一个功能，我们希望我给它什么，它帮我们处理什么。配合函数的声明和执行两部分，参数对应这两部分也有两个部分：
	// 函数声明阶段: 形参，形参是函数内部的变量，它也是用来代表和存储值的；
	// 函数执行阶段：实参，实参是给形参赋值的具体值。就是说函数执行时，形参所代表的具体的值。
}

function sum(a, b) {
	console.log(a, b);
	var total = 0;
	total = a + b;
	console.log(total)
}

sum(1, 2); // a = 1, b =2
sum(10); // a = 10, b = undefined 函数定义了形参，但是执行时没有传递形参时，该形参值是 undefined
sum(20); // a = 20， b = undefined 没办法绕过 a 只传给 b
sum(11, 22, 33); // a = 11， b = 22， 没有人接受33
```

### 闭包

闭包(closure)：函数会保护函数体内部的变量不被外界所干扰的机制成为闭包；

/*
* 1. return 用于指定函数返回值，那么 return 啥函数返回值是啥
* 2. 如果函数内部没有 return 或者 return 后面啥也没写，那么函数的返回值是 undefined
* 3. return 关键字还有一个重要作用————强制结束 return 后面的代码。（return 后面的代码不执行）
* 4. return 永远返回一个值，如果是一个表达式，return 会等着表达式求值完成，然后再把值返回。
* */

### arguments

arguments 是函数内置的实参集合。arguments 里面包含了所有函数执行时传入的实参（内置的意思就是浏览器天生自带的，不管你设置形参与否，也不管你传了实参与否，arguments 都存在）
arguments 是类数组是一个对象

```javascript
// 需求：求任意数字之和
function anySum2() {
  var total = 0;
  for (var i = 0; i < arguments.length; i++) {
    var item = arguments[i];
    if (typeof item !== "number") {
      // 这个条件用来判断每个实参的类型是不是数字，满足这个条件时表示不是数字，就需要先尝试转换
      item = parseFloat(item) // 用 parseFloat 方法把实参转成数字
    }
    if (!isNaN(item)) {
      // 这个条件是用来判断实参或者被转换后的实参是不是一个 NaN，如果不是 NaN 再加
      total += item;
    }
  }
  // 最后返回计算结果
  return total;
}

var r4 = anySum2('1', '2', 3, '5', 'USA', '13.5px', NaN);
console.log(r4);
// 代码的健壮性：允许一些意外情况的发生时，程序还可以正常工作，并且功能正常。为了增强代码的健壮性，我们需要在程序中额外增加处理意外情况的逻辑。anySum 现在需要增加检测实参是否是数字，如果不是就要转成数字，但是转完之后的结果还需要判断是不是一个有效数字，如果是有效数字，再加。
```

### 匿名函数 && 自执行函数

匿名函数：没有名字的函数	
函数表达式：把函数当成一个值赋值给变量或者对象的属性（包含元素的事件属性）
自执行函数：声明完成后立即执行；

```javascript
(function (a, b) {return a + b})(1,2);
~function (a, b) {return a + b}(1,2);
+function (a, b) {return a + b}(1,2);
!function (a, b) {return a + b}(1,2);
```

### 递归

```javascript
function rOneToTen(num) {
  if (num === 10) {
    // 使用递归时一定要考虑清楚何时终止递归
    return 10
  }
  return num + rOneToTen(num + 1)
  // return 1 + rOneToTen(2)
  // return 1 + 2 + rOneToTen(3)
  // return 1 + 2 + 3 + rOneToTen(4)
  // return 1 + 2 + 3 + 4 + rOneToTen(5)
  // return 1 + 2 + 3 + 4 + 5 + rOneToTen(6)
  // return 1 + 2 + 3 + 4 + 5 + 6 + rOneToTen(7)
  // return 1 + 2 + 3 + 4 + 5 + 6 + 7 + rOneToTen(8)
  // return 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + rOneToTen(9)
  // return 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + rOneToTen(10)
  // return 1 + 2 + 3 + 4 + 5 + 6 + 7 + 8 + 9 + 10 // 55
}
console.log(rOneToTen(1));

// 需求：求出1-10中是3的倍数的所有数字之和
function timesOfThree() {
  var total = 0;
  for (var i = 1; i <= 10; i++) {
    if (i % 3 === 0) {
      total += i;
    }
  }
  return total;
}
function rTimesOfThree(num) {
  if (num === 10) {
    return 0
  }
  if (num % 3 === 0) {
    return num + rTimesOfThree(num + 1);
  } else {
    return rTimesOfThree(num + 1)
  }
  // return rTimesOfThree(2)
  // return rTimesOfThree(3)
  // return 3 + rTimesOfThree(4)
  // return 3 + rTimesOfThree(5)
  // return 3 + 6 + rTimesOfThree(7)
  // return 3 + 6 + rTimesOfThree(8)
  // return 3 + 6 + rTimesOfThree(9)
  // return 3 + 6 + 9 + rTimesOfThree(10)
  // return 3 + 6 + 9 + 0 // 18
}
console.log(rTimesOfThree(1));
```

