### sort 按照某个属性排序

```javascript
// var ary5 = [1, 3, 5];
var ary6 = [ // 数组项可以是任意数据类型的
  {
    name: '张三',
    age: 15
  },
  {
    name: '李四',
    age: 18
  },
  {
    name: '王五',
    age: 14
  },
  {
    name: '麻六',
    age: 19
  }
]; 

// 按照年龄（按照某个对象的属性）进行升序排序
// 如果数组项是是对象，我们按照对象的某个属性排序时，在回调函数中return a.属性名 - b.属性名；就可以实现数组中的项按照这个属性升序排序；（不是只把这个属性值排序，而是数组项整体排序）
ary6.sort(function (a, b) {
  // console.log(a - b);
  return a.age - b.age
});
// console.log(ary6);
```

### 数组复制

```javascript
var ary = [1, 2, 3, 5, 6];

// 复制一个数组：
// slice()
// concat()
// ... 扩展运算符
var a1 = ary.slice();
var a2 = ary.concat();
var a3 = [...ary];

var ary2 = [
  {
    name: '张三',
    age: 10
  },
  {
    name: '李四',
    age: 15
  },
  {
    name: '王五',
    age: 40
  }
];
var ary3 = ary2.slice(); // ary3 是复制出来的数组
console.log(ary3);
console.log(ary3 === ary2);

// a1[1] = 100;
// console.log(ary);
// console.log(a1);

// ary3[1].age = 51; // 修改的是复制出来的数组 ary3 的第二项的 age 属性
// console.log(ary2);
// console.log(ary3);

// ? 原数组 ary2 和复制出来的数组 ary3 的第二项的 age 属性都被修改了成 51？为啥呢？

// 当我们数组项是一个基本数据类型值，数组存储这一项时存储的就是这个基本数据类型值本身；
// 如果数组项是引用数据类型值，数组存储这一项的时候存储的引用数据类型的堆内存地址；ary2 存储的结果是类似这样：
// var ary2 = [aaafff000, aaafff111, aaafff222]

// 所以在复制数组时，如果复制的项是基本数据类型的值，复制出来的就是基本数据类型值本身（相当于克隆了一个这个值）；
// 如果复制的项引用数据类型时，被复制的是堆内存地址。所在新数组中存储的也是堆内存地址；所以这个操作新数组中的项，操作的就是这个内存地址，所以原数组中的这一项也会跟着改变；这种复制称为浅复制、浅拷贝；

// 深拷贝：要求我们复制出来的数组，和原数组没有关系；

// 深拷贝ary2
var aryBak = [];
for (let i = 0; i < ary2.length; i++) {
  let obj = {}; // 这个是一个新的对象和数组项没有关系
  obj.name = ary2[i].name; // 把数组项中 name 和 age 属性添加到新对象中
  obj.age = ary2[i].age;
  aryBak.push(obj);
}
aryBak[1].age = 51;
console.log(ary2);
console.log(aryBak);

// 思考题？如何深复制这样一个数组，写一个方法
var ary5 = [
  {
    name: '张三',
    age: 15,
    ex: [
      {
        name: '李四',
        age: 15
      },
      {
        name: '王五',
        age: 18
      }
    ]
  },
  {
    name: '马六',
    age: 19,
    ex: [
      {
        name: '李四',
        age: 15
      },
      {
        name: '王五',
        age: 18
      }
    ]
  }
];

// 类数组转数组时？
var box2 = document.getElementById('box2');
var lis = box2.getElementsByTagName('li');
console.log(lis); // DOM 元素集合都是类数组

lis[1].style.backgroundColor = 'red'; // lis类数组集合中都是li元素对象

var lisAry = utils.arrLikeToAry(lis);
console.log(lisAry);
// ? 数组中数组项是什么？
lisAry[0].style.backgroundColor = '#00b38a'; // 操作lisAry，liAry 是通过类数组 lis 转成的数组；

// lis集合中每个 li 元素对象，对应的都是页面中一个 li；而元素对象也是对象，所以 lis 这个集合中存储的li元素对象的堆内存地址：
// lis = {0: aaafff000, 1: aaafff222, 2: ....length: 4}
// 然后类数组转数组，只不过是把类数组集合中存储的堆内存地址复制到了数组中。并没有重新再造一份 li 元素对象，这个数组中元素就是页面中的元素对象。
// 转成的数组：[aaafff000, aaafff222, ....]
```