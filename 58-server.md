```javascript
// 写一个服务，既可以处理静态资源文件请求，又可以响应 AJAX 请求；

// 根据 pathname 不同，响应不同的内容；
// 静态资源文件请求的 pathname: /(index.html) /a/b/xxx.css xx.js xx.png .. myDream.m4a；静态资源文件请求的 pathname 除了 / ，都带有扩展名；

// ajax 请求 pathname： /home/help/search ；而 ajax 接口是不带扩展名的；

let http = require('http');
let url = require('url');
let fs = require('fs');
let mime = require('mime');

// 1. 创建服务
let server = http.createServer((req, res) => {
  let urlObj = url.parse(req.url, true); // 处理 url
  let {pathname, query} = urlObj; // 从 urlObj 中获取 pathname 和 query（query 是客户端 get 请求时传递的问号传参，经过 url 模块解析后形成的一个对象）

  // 判断请求是静态资源请求 还是 动态的 ajax 请求；
  // 如果是静态资源文件请求 pathname 会带有 .xxx 扩展名或者 pathname 是 /
  if (pathname === '/' || /(\.\w+)$/.test(pathname)) {
    // 如果满足这个条件，说明请求的是静态资源文件
    let filePath = '';
    let contentType = '';
    if (pathname === '/') {
      filePath = __dirname + '/index.html'; // 计算机读取文件时需要一个路径，可以使用绝对路径也可以使用相对路径；
      contentType = 'text/html';
    } else {
      filePath = __dirname + pathname;
      contentType = mime.getType(pathname);
    }
    // 设置响应头
    res.setHeader('Content-Type', contentType);
    fs.readFile(filePath, (err, data) => {
      if (err) {
        res.statusCode = 404;
        res.end('not found');
      } else {
        res.end(data);
      }
    })
  } else {
    // else 的情况就是 ajax 请求
    // 根据不同接口返回不同的内容，即根据 pathname 不同，做不同操作；这种机制叫做路由；
    // 因为现在 ajax 没有特殊情况返回的都是 json，所以在这里设置
    // ajax 请求的接口也是 pathname，因为请求这个 pathname 不再返回静态资源文件，而是根据客户端的请求返回不同的内容；
    res.setHeader('Content-Type', 'application/json;charset=UTF-8;');

    if (pathname === '/api/getBanner') {
      // 这个接口的作用就是返回指定 id 的 banner 图信息；

      // 从 query 中获取客户端传过来的 id，如果获取不到 id 是 undefined；

      let {id} = query;
      // console.log(typeof id);
      // id 存在返回指定 id 的数据，不存在返回全部                                                                              
      if (id) {
        // 如果 id 存在
        let con = fs.readFileSync(__dirname + '/banner.json', 'utf8'); // 读取回来的结果是 JSON 格式的字符串；（现在没有数据库，真实项目中这里要查数据库）
        let dataArr = JSON.parse(con);
        let itemById = dataArr.find(item => +item.id === +id); // 传过来的 id 是 string 类型的，而数组项中的 id 是 number 类型的；需要转一下 number

        // find 的结果有可能不存在，不存在的时候是 undefined
        let d = {
          code: 0,
          data: {
            data: null
          },
          msg: 'ok'
        };
        if (itemById) {
          d.data.data = itemById;
        } else {
          d.data.data = {};
          d.code = -1;
          d.msg = `id为${id}的banner不存在`
        }
        res.end(JSON.stringify(d)); // res.end() 只能接收 Buffer 或者字符串；所以需要把对象 d 转成 JSON 格式的字符串
        // console.log(con);
        // console.log(typeof con);
      } else {
        // 如果 id 不存在
        let conAll = fs.readFileSync(__dirname + '/banner.json', 'utf8');
        let conAllAry = JSON.parse(conAll);
        // 构造接口文档约定的数据结构
        let d2 = {
          code: 0,
          data: {
            data: conAllAry
          },
          msg: 'ok'
        };
        res.end(JSON.stringify(d2));
      }
    }
    
    // 登录接口：根据用户 post 过来的数据去和咱们数据库中的数据比对；首先获取客户端传递的数据中的用户名和密码，然后从数据库中查找和传递用户名一样的数据，如果找到用户名一样的，接着比对密码，用户名和密码都相同才算是登录成功，其他情况都是登录失败
    if (pathname === '/api/login') {
      // post 请求方式：
      // 1. 构造约定的数据结构，默认成功
      let d3 = {
        code: 0,
        data: null,
        msg: 'ok'
      };
      // 2. 获取客户端 post 过来的数据；因为 post 请求传递的数据比较大，客户端会切片，一片一片的传递，而服务器也是一片一片的接收；所以 Node.js 是靠两个事件来接收 post 数据的
      let dataStr = '';
      req.on('data', (chunk) => {
        // 服务端在接收客户端 post 过来的数据的时候，每接收一次会触发一次 data 事件，触发一次 data 事件就会执行一次这个回调函数；
        // console.log(chunk);
        dataStr += chunk;
      });
      req.on('end', () => {
        // 服务端接收 post 数据结束后触发 end 事件，就会执行这个回调
        // console.log('完了');
        // console.log(typeof dataStr); string dataStr 此时已经有了全部的客户端传递过来的数据了，但是是一个字符串类型的；需要转成对象；
        let dataObj = JSON.parse(dataStr);

        // 根据 dataObj 中的用户名去数据库中查找
        let conUser = fs.readFileSync('./user.json', 'utf8');
        let conUserAry = JSON.parse(conUser);
        // console.log(Array.isArray(conUserAry));
        let user = conUserAry.find(item => item.name === dataObj.username);

        // 根据用户名查询的结果有可能不存在，所以需要判断
        if (user) {
          // 说明库里面有这个用户名，但是仍然需要比对密码
          if (user.pwd === dataObj.pwd) {
            res.end(JSON.stringify(d3));
          } else {
            d3.code = 1;
            d3.msg = '用户名或者密码错误';
            res.end(JSON.stringify(d3));
          }
        } else {
          // else 说明库里面没有这个用户
          d3.code = -1;
          d3.msg = `${dataObj.username}不存在，请前往注册`;
          res.end(JSON.stringify(d3));
        }
      })
    }

  }


});

// 2. 监听端口号
server.listen(8000, () => console.log('port 8000 is on'));
```