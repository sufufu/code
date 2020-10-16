### 动态操作 dom

```javascript
/*
* 目标：
*   1. 掌握动态DOM增删改查方法及返回值
*   2. 联系动态DOM增删改查
* */

// -> 1. 动态创建dom
// document.createElement()
// 语法：document.createElement('div')
// 参数：html 标签名 div、img、span、p....
// 返回值：新创建的 dom 对象
// 注意：新建 dom 元素对象的操作和操作页面中存在的 dom 元素对象的方式一样

var newDiv = document.createElement('div');
newDiv.style.width = '100px';
newDiv.style.height = '100px';
newDiv.style.backgroundColor = 'red';
newDiv.innerHTML = '这是一个新建的DIV';
newDiv.onclick = function () {
  alert('双击 666');
};
console.log(newDiv);
// ? 为什么页面中没有？因为我们只是动态创建了，并没有插入到 html 中。

// -> 2. appendChild() 向容器元素末尾追加元素
// 语法：容器元素.appendChild()
// 参数：元素对象（可以是新建的，也可以是页面之前就存在的）
var wrapper = document.querySelector('#wrapper');
wrapper.appendChild(newDiv);

// -> 3. insertBefore() 把元素插入到容器指定标签之前
// 语法：容器.insertBefore(newEle, oldEle);
// 参数：新元素, 作为参照物的老元素

var newP = document.createElement('p');
newP.style.backgroundColor = 'green';
newP.innerHTML = '这是p标签';

// 获取参照物
var originExist = document.querySelector('#originExist');
wrapper.insertBefore(newP, originExist);

// -> 3. removeChild() 移除元素
// 语法：父级容器.removeChild()
// 参数：要移除的元素
// 返回值：返回被删掉的元素

wrapper.removeChild(originExist);
setTimeout(function () {
  wrapper.removeChild(newDiv);
}, 3000)

// -> cloneNode() 克隆节点
// 语法：curEle.cloneNode()
// 参数：是否深度克隆; 默认是 false，表示只克隆自己；true：表示克隆自己和子孙代。
// 返回值：是克隆出来的元素对象
var forClone = document.querySelector('#for-clone');
forClone.onclick = function () {
  alert('for clone ')
};
var cloneNode = forClone.cloneNode(false);
var cloneNode2 = forClone.cloneNode(true);
console.log(cloneNode);
console.log(cloneNode2);
console.log(cloneNode === forClone); // false 表示克隆出来的副本和原版是两个不同的对象

wrapper.appendChild(cloneNode2); // 注意：克隆出来的元素对象不会带有原版的事件。使用克隆节点时，需要给克隆出来的节点重新绑定事件
```

### 属性

```javascript
/*
* 目标：
*   1. 掌握常用属性操作方法
*   2. 练习自定义属性保存保存数据
* */

// -> 这里的属性专只 html 行内属性
// -> 1. 动态设置行内属性: setAttribute(attr, value)
// 语法：元素对象.setAttribute(attr, value)
// 参数：属性名, 属性值 (字符串)
var attributeBox = document.getElementsByClassName('attr-box');
console.log(attributeBox);
// attributeBox.setAttribute('today', '2019-5-24'); // attributeBox 是元素集合，我们要操作元素对象，所以需要[索引]的方式把元素取出来
attributeBox[0].setAttribute('today', '2019-05-24');
attributeBox[0].setAttribute('name', '珠峰培训');
attributeBox[0].setAttribute('age', '10');

// -> 2. 获取属性值 ：getAttribute()
// 语法：元素对象.getAttribute(attr) attr 是属性名
var courses = attributeBox[0].getAttribute('courses');
console.log(courses);

// -> 3. 删除行内属性 removeAttribute()
// 语法：元素对象.removeAttribute(attr)
attributeBox[0].removeAttribute('end');

// 学习这个属性的目的在于可以通过自定义属性保存数据。而且这些数据是在 html 行内可见的（是通过控制台可以看到）

```

### 操作类名

classList 返回所选元素的类名 是一个数组，一个类名就是一个索引
classList.add 添加一个类名
classList.remove 移除一个类名
classList.toggle 用来切换类名，有就删除，没有就添加
classList.contains 判断一个类型是不是存在，返回 true 或者 false