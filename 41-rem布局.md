```html
<!doctype html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport"
    content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
  <!--viewport: 视口-->
  <!--device-width 设备宽度-->
  <!--initial-scale 初始缩放比例1.0-->
  <!--user-scalable=no 不允许用户缩放-->
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
  <style>
    * {
      margin: 0;
      padding: 0;
    }

    html {
      font-size: 100px;
    }

    /*默认情况下，html 的 font-size 属性是 12px，也就是默认 1rem 是 12px*/
    /*一般为了好计算，都把 html 字体设置成 100px，此时 1rem 就是 100px*/
    .box1 {
      margin: .2rem;
      width: 1rem;
      height: 1rem;
      background: lightcoral;
    }

    .box2 {
      margin: .2rem;
      width: 2rem;
      height: 2rem;
      background: lightgreen;
    }

    .box3 {
      margin: .2rem;
      width: 2.5rem;
      height: 2.5rem;
      background: lightseagreen;
    }
  </style>
</head>

<body>

  <div class="box1"></div>
  <div class="box2"></div>
  <div class="box3"></div>

  <script>
    // 写 rem 布局首先要在 head 标签中加一个 meta 标签：
    /*<meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">*/

    // rem 是什么？
    // rem 和 px 一样是个单位。px 是绝对单位，而 rem 是个相对单位，rem 是相对于 html 标签的字体大小，html 标签的 font-size 就是1rem；html 元素默认的 font-size 是 12px，也是浏览器所能显示的最小字体（不做特殊处理）；

    // 为什么要用 rem 布局？
    // 如果页面用 px 来写，页面中元素的尺寸都是写死的。现在改成用 rem 来写，rem 是一个相对单位，用 rem 设定元素的尺寸，只要修改 html 的 font-size，这些用 rem 设定的尺寸都会自动跟着缩放，而且缩放比例相同。我们使用 px 按照某一个手机的屏幕尺寸切图，但是换到另一个手机上，这两个手机的屏幕尺寸不同，原来的样式就不能用了。但是现在换成 rem 来设定元素尺寸，当切换手机后，根据手机的屏幕尺寸修改 html 的 font-size，页面中所有的使用 rem 的元素的尺寸都会自动按照最新的比例进行缩放。

    // rem 布局的思路：
    // 首先给 html 的 font-size 设置为 100px（此时页面中的 1rem 就是 100px），接下来我们写样式时，把所有的带 px 的单位都改用 rem 来设定（测量出来的 px / 100 就是需要的 rem 数）。但是如果 html 标签的 font-size 不变，那和直接写 px 没什么区别；
    // 但是如果我们修改了 html 的 font-size，就是修改了 rem 和 px 的换算比例，浏览器就会按照最新的比例把页面中使用 rem 的元素进行缩放。使用 rem 布局，就可以通过简单设置 html 的 font-size 来实现整个页面的缩放。

    // html {font-size: 100px}
    // 原来在750的设备上有一个1rem的盒子，100 / 750
    // 375 设备上， 求这个盒子的宽 x / 375  = 100 / 750
    // (375 / 750) * 100 -> 50px

    // 真实项目中设计师会提供设计稿，常用设计稿宽度 640（320）和 750（375）两种；接下来我们根据设计稿切图，首先设置 html {font-size: 100px} ，此时 1rem 就是 100px；
    // 接下来使用 rem 设定页面中元素的尺寸。

    // 如果现在是 750px 宽的设计稿，相当于页面运行在 750px 的设备上；
    // 如果现在页面运行在 375px 的设备上，此时我们修改 html 的 font-size 修改为：375 / 750 * 100 px。此时页面中 1rem 现在是50px；

    (function (designWidth) { // 设计稿的宽度 designWidth
      function computedFont() {
        // 当前视口的宽度
        let winW = document.documentElement.clientWidth || document.body.clientWidth;
        // 计算页面中的 html 的 font-size
        document.documentElement.style.fontSize = (winW / designWidth) * 100 + 'px';
      }
      computedFont();
      // 一般监听页面的 resize 事件，以便当页面的尺寸发生改变时重新计算 html 的字体大小
      window.addEventListener('resize', computedFont);

      // 手机横屏时也要重新计算这个 font-size
      window.addEventListener('orientationchange', computedFont);
    })(750);

  // 如果有需求需要你判断，如果是访问的 www 的网站，就跳到对应的 m 站。例如在手机上访问 wwww.dahai.com ,就要跳到 m.dahai.com。
  // 有一个插件 device.js 可以判断设备类型。https://www.npmjs.com/package/device.js

  // window.location.href = 'm.dahai.com' 跳转到指定页面

  </script>
</body>

</html>
```