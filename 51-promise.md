### 什么是 promise

+ Promise 是 ES6 新增的类，用来管理异步，Promise 是同步的，new Promise 时传递的回调函数会同步执行

```javascript
let p = new Promise(function (resolve, reject) {
  // 这个函数是同步执行
  // 函数里面放的是异步处理的任务
});

p.then(function (result) {
  // 异步处理成功后执行的操作写在 then 的第一个回调中
  console.log(result);
}, function (err) {
  // 异步处理失败后执行的操作写在 then 的第二个回调中
  console.log(err);
});

// Promise 实例有三种状态：
// pending: Promise 实例初始化成功，正在处理异步
// fulfilled: 异步处理成功
// rejected: 异步处理失败

// Promise 对象的状态只能从 pending 变成 fulfilled 或者变成 rejected；一旦发生改变，状态就会凝固，不能再发生变化；
```

```javascript
let p2 = new Promise(function (resolve, reject) {
  // 这个回调是同步执行的，里面放的都是异步处理的任务
  // resolve 是一个函数，在异步处理成功后执行，有两个作用，第一把 promise 实例的状态从 pending 变为 fulfilled；第二，执行一个事件池，这个事件池中收集了所有 then 方法的第一个回调函数；
  // reject 是一个函数，在异步处理失败后执行；有两个作用，第一个把 Promise 实例的状态从 pending 变成 rejected；第二，执行一个事件池，这个事件池中收集所有的 then 方法的第二个回调函数；

  // resolve 或者 reject 在执行的时候收到的实参会传递给第一个 then 方法的回调函数；

  setTimeout(function () {
    // reject('xyz');
    resolve('abc'); // 因为 promise 的状态一旦发生变化，就凝固了，不能再变化；
  }, 1000);
});

p2.then(function (result) {
  // then 的回调函数的执行规律：
  // 第一个 then 比较特殊，它的两个回调函数中哪个能执行，取决于创建 Promise 实例时到底是 resolve 还是 reject 了。如果是 resolve 了，第一个 then 的第一个回调函数会执行；如果是 reject 了，那么第二个回调执行；

  // 后面的 then 方法中哪个回调能执行，由前面 then 方法中执行情况决定：
  // 1. 当前面 then 的回调函数没有返回 Promise 实例时，无论前面的 then 是哪个回调函数执行，都会执行第一个回调函数；前面 then 的回调函数的返回值会传给下一个 then 回调函数；如果前面 then 回调执行时报错了，就会执行第二个，并且报错信息会传递给第二个回调；

  // 2. 当前面 then 的回调返回了 Promise 实例时；后面 then 方法的哪个回调能执行，取决于前面 then 方法返回的 Promise 实例的状态；如果前面返回的实例 resolve 了，后面 then 的第一个回调就会执行；如果前面的实例 reject 了，后面 then 的第二个回调执行；并且前面 then 的回调返回的实例 resolve 或者 reject 执行时收到的实参，会传给下一个 then 要执行的回调；


  // throw '我就是想报错';
  // console.log(1);
  // console.log(result);
  // return 'ok'
  return new Promise(function (resolve, reject) {
    // resolve('前面实例 resolve');
    reject('前面实例reject');
  })
}, function (err) {
  console.log(2);
  console.log(err);
  return '不ok'
}).then(function (result2) {
  // result2 是上面 then 回调的返回值
  console.log(3);
  console.log(result2);
}, function (err2) {
  console.log(4);
  console.log(err2);
});
```

### Promise-all

```javascript
// 有两个接口，login.json 和 banner.json；这两个接口并没有什么联系，但是需要都请求完才能渲染数据；

// 同时请求多个接口，这些接口都请求完才能做某件事情；这个时候要用到一个方法：

// Promise.all([promise实例1, promise实例2.....]) 是 Promise 的静态方法，
// 参数接收一个数组，数组项都是 promise 实例；
// 返回一个新的 Promise 实例，如果数组中所有的 Promise 都 resolve 了，新返回的 Promise 实例就是 resolve。如果数组中只要有一个 reject了，新返回的 Promise实例就是 reject 的；

let p1 = new Promise((resolve, reject) => {
  $.ajax({
    url: 'login.json',
    type: 'GET',
    cache: false,
    dataType: 'json',
    error(err) {
      reject(err)
    },
    success(data) {
      resolve('abc')
    }
  })
});
let p2 = new Promise((resolve, reject) => {
  $.ajax({
    url: 'banner.json',
    type: 'GET',
    cache: false,
    dataType: 'json',
    error(err) {
      reject(err)
    },
    success(data) {
      resolve('xyz');
    }
  })
});

Promise.all([p2, p1]).then(function (result) {
  console.log(result); // result 是一个数组，是 all 方法的数组中所有 promise 实例在 resolve 的时候传入的实参，而且是按照顺序的；
}).catch((err) => console.log(err));

// 通常 then 中只写成功的回调；再最后写一个 catch 方法，这里面用于处理异常和 reject；
```