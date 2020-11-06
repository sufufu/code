### cookie

+ cookie 可以和 localStorage、sessionStorage 一并用作本地存储；但是 cookie 主业不是做本地存储，它是用来保存 http 状态

- HTTP 协议是无状态的，即服务器向客户端发送数据结束后，连接就会关闭，而服务器不会记录发生的这一切，这就会有一个问题，如果我们登陆后向服务器请求了一次资源，等服务器响应结束就会链接断开，但是我们第二次请求一个求他的资源，但是服务器已经忘记了已经登陆过，所以服务器要求你重新登陆，这就导致每次请求东西都要登陆

- 为了解决这个东西，http 协议发明了 cookie，cookie 是 http 协议的一部分，即不属于客户端技术也不属于服务端技术，但是客户端和服务端都有操作 cookie 的技术

- 在客户端发起请求的时候，http 协议会从客户端把所有的 cookie 读取出来，然后带着这些 cookie 去请求服务器；服务器收到请求后，服务器收到请求里面是包含 cookie 的，服务器就可以任意操作 cookie 了。服务器响应的时候会让 http 协议把 cookie 再带给客户端（浏览器检测到响应中有 cookie，浏览器会自动把 cookie 保存起来）；

-  以登录为例，第一次我们带着用户的用户名和密码去请求服务器，服务端拿到用户名和密码去数据库中匹配，如果匹配成功就成功登录，于此同时服务端会在 cookie 中设置一个值表示登录成功，等响应的时候这个 cookie 就会带到客户端，浏览器会自动把它存起来；下一次再请求的时候，http 协议会自动带着 cookie 去请求；


### 浏览器操作 cookie：
```javascript
// 1. js 获取 cookie -> document.cookie
console.log(document.cookie);

// 2. js 设置 cookie -> document.cookie = 'key=value;配置属性'
// document.cookie = 'name=mabin;';
document.cookie = 'title=宇宙集团军总司令;';
console.log(document.cookie);

// document.cookie = 'name=bingo;';

// 设置 cookie 时的注意事项：
// 1. 如果设置多个，就需要多次给 document.cookie 赋值
// 2. 如果同名 cookie，后面的会覆盖前面的；

// cookie 的配置属性：
// domain 可以访问这个 cookie 的域是哪个
// path 可以访问当前 cookie 的路径，一般设置为 / 表示根目录；所有根目录下面的路径都能访问这个 cookie；
document.cookie = 'name=bingo;path=/'; // 如果同名 cookie 配置属性不同不能互相覆盖；
// expires cookie 的过期时间；cookie 是有时效性的，如果过期，浏览器就会删除这个 cookie；expires 的值是一个 GMT 时间；
// * 删除 cookie 的原理：把 cookie 的 expires 设置为一个过去的时间，浏览器会自动删了它；
document.cookie = 'age=18;expires=Sat, 13 Jul 2019 08:53:00 GMT;';
// maxAge: cookie 的有效期，表示 cookie 在多长时间之内是有效的，如1小时，1分钟；单位是 ms；（服务端设置的）

// * http-only: 只能给 http 协议使用，前端不能获取也不能修改；（服务端设置）
```

### session

session 是服务端技术，意思是会话控制

+ 和 cookie 不同，cookie 是存在客户端的，而 session 是存在服务器上，并且不会随着 http 传递

cookie 可以在客户端随意被更改，服务器为了杜绝这种事情，服务器也搞了一个存储用户信息的东西，并且这个东西只在服务器放着，不给别人看；这个东西就是 session

+ session 的原理

- 以登录为例，登录时客户端会把用户名和密码发给服务器，服务端收到请求后会去数据库中匹配，如果匹配成功，就可以登录成功了。

- 接着服务端会在 cookie 中设置一个表示登录状态的值，例如 isLogin=true; 同时服务器会生成一份 session 文件，这个 session 文件有一个 id，这个 id 叫做 session-id。这个 session 文件中一般会存储用户的 id，登录时间等信息；然后把 session-id 会写到 cookie 中；然后 http 协议会带着这些 cookie 响应给客户端；

- 客户端收到响应后，会自动把响应中的 cookie 存储在浏览器中，这些 cookie 就包含了 session-id；

- 等再次发起请求时，http 协议会带着所有的 cookie 去请求，服务端收到请求后从 cookie 中找到 session-id，然后根据 session-id 去查找 session 文件；然后从 session 文件中获取用户的信息，如果信息有效就继续响应请求，否则认为登录失效；

+ session 和 cookie 的区别

1. session 是服务端的技术，session 存在服务器上
2. cookie 是 http 协议的一部分，存在客户端

session 和 cookie 的联系：session-id 是存在 cookie 中的，会随着 http 通信时在客户端和服务端来回传递；

### token

+ token 叫做令牌，不是和 cookie 或者 session 一样的一种技术，是一种用于身份校验机制；

sign：签名

- token: 一般用户登录的时候，客户端会传递用户的用户名和密码给服务端，服务端匹配后，会根据用户 id、登录时间等信息生成一个字符串，并且还要给这个字符串加密，甚至还需要签名；
- 生成 token 以后会返回给客户端，客户端下次再请求的时候要带着这个 token 来，服务器就会认识这个 token，接着对 token 进行校验，如果通过继续响应，如果不通过就返回错误，要求用户重新登录；

+ 常见的 token 使用方式
1. 把 token 放到 cookie 中，http 请求时会自动带着 token
2. 服务端把 token 作为数据返回客户端，客户端需要手动保存 token；可以存在 localStorage 中，下次请求的时候要把 token 从 ls 中取出来，再作为参数发送给服务端
3. 服务端返回 token 后，可以吧 token 放到请求头里面，让服务端从请求头里面取 