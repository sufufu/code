### 状态码

```javascript
let xhr = new XMLHttpRequest();
xhr.open('GET', 'banner.json', true);
xhr.onreadystatechange = function () {
  if (xhr.readyState === 4 && xhr.status === 200) {
    // xhr.readyState 为4说明 ajax 请求完了
    // xhr.status 是 http-status-code，表示 http 请求的结果状态
  }
};
xhr.send();

// 所有的 http-status-code 都是三位数字，表示 http 请求的结果如何，每个 code 还有一句对应的短语；

// 常用的 http-status-code
// 2xx: 表示成功
// 200 OK 表示所有的东西都正常
// 204 No Content 表示请求成功，但是服务端没有内容给你

// 3xx: 表示重定向
// 301 Moved Permanently 永久重定向，一个域名被指向了一个其他网站，且是永久的；（当访问一个被永久重定向的网址的时候，会收到一个301的响应状态码，浏览器检测到301以后，浏览器会从响应头中找到 Location 的属性对应 url，然后浏览器会重新打开这个 url）；

// 302 Moved Temporarily 临时重定向
// 304 Not Modified 走缓存 （服务端觉得你之前请求过这个东西，而且服务器上的那一份没发生变化，告诉客户端用缓存就行）

// 4xx: 表示客户端错误
// 400 Bad Request 参数传递不当，导致的错误
// 401 Unauthorized 权限不够
// 403 Forbidden 服务端已经理解请求，但是拒绝响应；
// 404 Not Found 客户端请求的资源或者数据不存在（上班后发现请求接口404，有两种情况咱们写错接口或者服务端还没部署）

// 5xx: 表示服务端错误（遇到以5打头的错误去找服务端同事）
// 500 Internal Server Error 服务器内部错误
// 502 Bad Gateway 网关错误

// 这些状态码都是服务端设置的，如果出了问题，找服务端；
```

### ajax同步异步

+ 同步是阻塞的，前一个执行不完，后一个不能开始；异步是非阻塞的，后面的执行不用前面的执行完（异步任务放到等待任务队列中，等待任务队列中的任务谁先达到执行条件谁先执行）

```javascript
// 为什么使用异步？

// 现在有5个接口需要请求
// 1. 1s
// 2. 2s
// 3. 3s
// 4. 4s
// 5. 5s

// 如果使用同步，需要用时15s;因为前一个 ajax 不完，后一个没办法开始；用户等待时间过长；

// 现在改成异步：第一个 ajax 执行，就把它放到等待任务队列中，第二个不用第一个完，第二个就开始了，第三个也不用等第二个。。。这样做每个接口不用互相等；（使用异步就好像同时发了5个请求）然后总的用时只需要5s就可以了。

// 真实的项目中用异步的多；

// 异步有一个麻烦的问题，当接口互相依赖时，代码组织起来会比较麻烦；比如说有两个接口，第二个接口依赖于第一个接口；
let d1 = null;
let x1 = new XMLHttpRequest();
x1.open('GET', '1.json', true);
x1.onreadystatechange = function () {
  if (x1.readyState === 4 && x1.status === 200) {
    d1 = JSON.parse(x1.responseText);

    let id = d1.id;

    let x2 = new XMLHttpRequest();
    x2.open('GET', '2.json?id=' + id, true);
    x2.onreadystatechange = function () {
      if (x2.readyState === 4 && x2.status === 200) {
          
      }
    };
    x2.send();
  }
};
x1.send();
```