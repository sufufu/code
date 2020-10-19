this 是一个 js 的关键字，代表当前代码【执行】的环境对象，一般在函数中使用，在函数执行时确定，根据函数执行的方式不同，代表的值不同。目前有以下情况

1. 事件函数中的 this 是绑定当前事件的元素
2. 自执行函数中的 this 是 window
3. 定时器的回调函数中的 this 是 window
4. 方法调用时，看方法前面有没有 [.] 如果有；点前面是谁，方法中的 this 就是谁，如果没有点，方法中的 this 就是 window
5. 箭头函数没有 this ，只能指向箭头函数声明时所在的作用域中的 this 
6. 全局作用域中的 this 时window
7. this 在运行时不能直接通过赋值的方式修改

那么怎么修改 this 指向呢？？？

```javascript
function sum(a, b) {
    console.log(this);
    console.log(a, b);
    return a + b;
}

var obj = {
  id: '0511120117'
};
// sum(1, 2); // window

// 1. call()
// 作用: 修改函数中的 this 指向，并且把修改 this 后的函数执行
// 语法：函数名.call(ctx, 实参1, 实参2.....)
// 参数：ctx 就是用来替换函数中this的对象，从第二个参数起，都是传递给函数的实参
// sum.call(obj, 2, 3); // call 之后，sum 中的 this 就变成了 obj

// 用 call 指定 undefined 和 null 作为 this 无效；
// sum.call(undefined, 3, 4);
// sum.call(null, 1, 3);
// sum.call();

// 2. apply()
// apply 方法和 call 方法作用一样，修改函数中的 this，并且让这个修改 this 之后的函数执行；
// 但是传参方式不同；call 方法是一个一个的传递实参给 sum 的，apply 是把实参都放到一个数组中，数组项是传递给 sum 的实参；

// sum.apply(obj, [11, 12]);
// var ary = [1, 2, 3, 4, 5];
// sum.apply(obj, ary);


// 3. bind() 方法：
// 作用：修改函数中的 this，返回一个修改 this 后的新函数；不会让函数执行。
let sum2 = sum.bind(obj, 1, 2); // 传参的时候和 call 一样，需要一个一个的传
console.log(sum2);
console.log(sum === sum2); // false 因为 sum2 是一个新函数
sum2(); // 修改 this 后，需要自己执行一次这个函数

// call 和 apply 是修改 this 并且让函数执行，call 是一个一个的传参，apply 传递一个数组
// bind 是只修改 this 返回修改 this 后的新函数，并不会让函数执行；

```