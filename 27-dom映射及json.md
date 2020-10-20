### dom 映射

+ DOM映射：页面中的 html 元素和我们通过 js 的相关方法（getElementById / getElementsByTagName / getElementsByClassName / getElementsByName...）获取来的元素对象或者集合存在映射关系（一个改另一个跟着改）

```javascript
// DOM 映射的常见情形：
// 1.
let box3 = document.getElementById('box3');
box3.style.backgroundColor = 'red'; 
// ? 为啥我修改一个对象的属性，页面中的元素的背景色就会变成红色？
// box3.style.backgroundColor = 'red'; 这行代码本质上讲 box3 对象对应的堆内存空间中的 style 属性下的 backgroundColor 属性值修改为 'red' ；但是因为 DOM 映射机制，页面中的 id 为 box3 的 div 元素和 box3 对象存在映射关系，我们修改这个对象，浏览器就用根据最新的值重新渲染这个元素；

var obj = {
  name: '100',
  backgroundColor: 'blue'
};
obj.backgroundColor = 'red'; // 修改 obj 页面中不会有任何动静，因为页面中没有一个元素和 obj 存在这样的映射关系；

// 2.
let box2 = document.getElementById('box2');
let hwList = box2.getElementsByTagName('li'); // hwList 是我们通过 dom 相关方法获取来的元素集合

let hwAry = utils.arrLikeToAry(hwList); // 把li集合转成一个数组
hwAry.sort((a, b) => a.getAttribute('price') - b.getAttribute('price'));

hwAry.forEach((item, index) => {
  // item 就是 hwAry 中的每个li元素
  // debugger; // 断点
  box2.appendChild(item); // item 不是新创建出来的，而是之前存在于页面中的 li 元素；
});

// ? 为啥页面中还是4个，不是8个呢？
// hwList 这个集合是通过 DOM 相关方法获取来的元素集合，集合的每一项都是页面中的 li 元素对象，而元素对象也是对象。其实集合存储 li 元素的堆内存地址，所以我们在类数组转数组的时候，只不过是把 li 的堆内存地址存储到了数组中（数组中的 li 元素不是新造的，就是原来类数组中的 li，也就是页面中已经存在的4个 li）。appendChild 方法是向容器末尾追加元素的，在追加时，如果发现这个元素是已经存在于容器中的元素，此时 appendChild 不会重新克隆一份新的，而是把原来的元素移动到容器末尾。 
```

+ DOM 回流、重绘
- DOM 回流（reflow）：页面中的元素增加、删除、大小、位置的改变，会引起浏览器重新计算其他元素的位置，这种现象称为 DOM 回流。DOM 回流非常消耗性能，尽量避免 DOM 回流；
- DOM 重绘：元素的某些 css 样式如背景色、字体颜色等发生改变时，浏览器需要重新描绘渲染这个元素，这种现象称为 DOM 重绘。

文档碎片

```javascript
// 文档碎片：存放 DOM 元素的临时容器；专门用来减少 DOM 回流次数的
// document.createDocumentFragment() 创建一个文档碎片
let frg = document.createDocumentFragment();
listBox.appendChild(frg); // 把文档碎片插入 ul 中，一次性把10个 li 插入页面中；这样做只会引发一次 DOM 回流
frg = null; // 临时对象用完之后记得手动释放内存（手动释放内存是良好代码习惯之一）。
```

### JSON 

JSON 数据：不是数据类型，是一种常用于前后端交互的数据格式。

JSON格式的对象：把属性名和属性值都用字符串包起来，如果属性值数字可以不包（如果用引号包了，就是字符串）

```javascript
var obj = {
  "name": "张三"
};

var ary = [
  {
    "name": "张三",
    "age": 15
  },
  {
    "name": "李四",
    "age": 18
  }
];
```

JSON 格式的字符串：长得像极了 JSON 对象的字符串，属性名和属性值都是用双引号包着

```javascript
// js 中引号在字符串中单双循环着用；如果最外层是单引号，字符串中不能再用单引号了，如果一定要用要使用 \' (\叫做转义符); 如果最外层是双引号，字符串中不能再用双引号，如果一定要用必须使用 \" 转义

let str1 = "{\"name\": \"张三\"}";
let str2 = '[{"name": "张三"}, {"name": "李四"}]';

// JSON 格式的对象和 JSON 格式的字符串互转
// 在 window 上面有一个 JSON 的对象，其中有两个方法；

// window.JSON.parse() 把 JSON 格式的字符串转成对象
let ary2 = JSON.parse(str2);
// console.log(Array.isArray(ary2)); // true
// console.log(ary2); // 数组

// window.JSON.stringify() 把对象转成 JSON 格式的字符串
let str3 = JSON.stringify({name: '马六'});
let str4 = JSON.stringify([{name: 'zs'}, {name: 'lisi'}]);
// console.log(str3);
// console.log(typeof str3);
// console.log(typeof str4);
```

### 使用JSON的方法实现深复制

```javascript
let ary3 = [{name: 'zs'}, {name: 'lisi'}];
let ary4 = ary3.slice();

let ary5 = JSON.parse(JSON.stringify(ary3)); // 先把对象转成 JSON 格式的字符串，再把 JSON 格式的字符串转成一个对象，这个对象和原来的没有关系；

ary5[1].name = '李四';
// console.log(ary3);
// console.log(ary5);

// IE 6/7 没有 JSON 对象，无法使用 JSON.parse() 把字符串转成对象
// 在IE 6/7中使用 eval 方法

var str6 = '[{"name": "张三"}, {"name": "李四"}]';
var obj2 = eval(str6);
console.log(obj2);

var str7 =  '{"name": "mabin"}';
// var obj3 = eval(str7);
// console.log(obj3); // {"name": "mabin"} 这个花括号会被 js 解析引擎理解成代码块，所以为了避免这种情况，需要在字符串外侧拼接一个小括号

var obj3 = eval('(' + str7 + ')'); // ({"name": "mabin"}) 加小括号后，js 解析器认为这是一个整体的值，里面的花括号就会处理成对象
console.log(obj3);


/**
 * @desc JSON格式字符串转对象
 * @param jsonstr JSON格式字符串
 * @returns {Object} 对象
 */
function toJSON(jsonstr) {
  if ('JSON' in window) { // 'JSON' in window 返回false表示JSON的方法不可以用
    return JSON.parse(jsonstr);
  } else {
    return eval('(' + jsonstr + ')');
  }
}
```