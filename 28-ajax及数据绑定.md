### ajax

```javascript
// AJAX : Asynchronous Javascript And XML (异步的javascript和xml)

// AJAX 四步：

// 1. 第一创建ajax实例对象
let xhr = new XMLHttpRequest();

// 2. 调用 xhr 的 open 方法配置请求信息
// HTTP METHOD 请求方式：GET / POST / PUT / DELETE / OPTIONS... 
// URL: 请求的服务端的接口地址 是一个地址
// 异步或者同步：false 表示同步，true 表示异步
xhr.open('GET', './js/data.json', false);

// 3. 监听 xhr 的 onreadystatechange 事件
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    // xhr.readyState 为4， xhr.status 为200 时表示我们的请求成功
    // 请求成功后，我们可以拿到服务端返回的数据，这些数据挂载到 xhr.responseText 上；
    // console.log(xhr.responseText); // 我们拿到的数据时 JSON 格式的字符串，所以需要处理成对象；
    // console.log(typeof xhr.responseText); // string
    let data = JSON.parse(xhr.responseText);
    // console.log(data);
    // 拿到数据后，我们需要把这些数据绑定到页面中
  }
};

// 4. 发送请求
xhr.send();
```

### 数据绑定

数据绑定：通过某种方式把数据绑定到页面的html中

```javascript
var stus = [
  {
    name: '张三',
    age: 18
  },
  {
    name: '李四',
    age: 19
  },
  {
    name: '王五',
    age: 16
  }
];

// 常见的方式：
// 1. 动态创建添加DOM
let box = document.getElementById('box');
for (let i = 0; i < stus.length; i++) {
  let item = stus[i];
  let li = document.createElement('li');
  // name: 张三, age: 18
  li.innerHTML = `name: ${item.name}, age: ${item.age}`;
  // box.appendChild(li);
}
// DOM回流和重绘：

// 2. 字符串拼接+innerHTML

// 2.1 传统字符串拼接

// 2.2 模板字符串拼接；
let str = ``;
for (let i = 0; i < stus.length; i++) {
  let item = stus[i];
  str += `<li>name: ${item.name}, age: ${item.age}</li>`;
}
// console.log(str);
box.innerHTML = str;
```