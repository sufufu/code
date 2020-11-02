### class

class 关键字：ES6 新增的 用于声明一个类

ES5 声明类
```javascript
function Teacher(name, age, subject, from) {
  this.name = name;
  this.age = age;
  this.subject = subject;
  this.from = from;
}

Teacher.prototype.teach = function () {
  console.log(`${this.name} 教 ${this.subject}`);
};

Teacher.motto = '传道受业解惑'; // 把 Teacher 当成一个普通对象，并且添加一个属性 motto；motto 是静态属性，和实例无关，和原型无关；
Teacher.getX = function () { // 静态方法
  console.log('四十米大刀');
};

let m = new Teacher('mabin', 18, 'JS', 'zf');
m.teach(); // mabin 教 JS；

console.log(Teacher.motto);*/
```

ES6 声明类

```javascript
class Teacher {
  constructor (name, age, subject, from) {
    // 在 constructor（构造函数中）通过 this.xxx = xxx 是在给实例添加私有属性
    let x = 100; // x 不是私有属性
    this.name = name;
    this.age = age;
    this.subject = subject;
    this.from = from;
  }
  
  // 添加公有方法
  teach () {
    console.log(`${this.name} 教 ${this.subject}`);
  }
  getY () {
    console.log('hello');
  }

  // 添加静态属性方法，用 static 声明静态的属性和方法；静态属性和方法只能类自身访问，和实例、原型都没有关系；
  static motto = '传道受业解惑';
  static getX () {
    console.log('四十米大刀');
  }
}

// 没有兼容性问题添加静态属性和方法
// Teacher.motto = 'xxx';
// Teacher.getX = function () {};

console.log(Teacher.motto);
Teacher.getX();

let n = new Teacher('牛晓鑫', 19, 'JS', 'zf');
console.log(n);
n.teach();

let m = new Teacher('马宾', 18, 'JS', 'zf');
console.log(m);
m.teach();

console.log(n.teach === m.teach);
```

### class 继承

```javascript
// ES6的继承 extends 关键字

class A {
  constructor (a, b) {
    // 给 A 类的实例添加私有属性
    this.text = 'A类的text';
  }
  // 添加公有方法
  say () {
    console.log('hello world');
  }
}

class B extends A { // class B extends A 让 B 类继承 A 类
  constructor () {
    super('x', 'y'); // 在这里调用 super(); super 就是父类; super() 这一行代码要写在使用 this 关键字之前，如果不这样就会报错；super 执行时还可以传递实参，实参是传递给父类的构造函数的（父类的constructor）；
    this.x = 100;
    this.y = 200;
  }
}

let b = new B();
console.log(b);
b.say(); // hello world 说明，extends 关键字继承把父类的公有的变成了子类公有的；

/*
*  {
*   text: "A 类的 text", text 属性时继承的 A 类的，extends 可以把父类私有的变为子类私有的
*   x: 100,
*   y: 200
*  }
*
* */

// extends 关键字继承：
// 1. 声明子类时，就需要 extends 父类，形如 class B extends A；
// 2. 在子类的构造函数中使用 this 关键字之前，调用 super()；

// extends 关键字的原理使用的寄生组合式继承（原型式继承 +call 继承）

// 用ES6声明一个类：Student
// 要求：这个类的实例，有 name、age、sex（一般1表示男性，2表示女性）score 这些私有属性；
// 还要有 learn 公有方法，learn 方法执行输出：'我热爱编程，因为编程使我快乐';

// 继承就是为了让子类的实例能够使用父类上的属性或者方法；

class Human {
  constructor (name, age, sex) {
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
  eat () {
    console.log('美食');
  }
  drink () {
    console.log('干杯');
  }
  sleep () {
    console.log('睡觉');
  }
  think () {
    console.log('我是谁，我在哪儿，我要去哪儿');
  }
}

class Student extends Human {
  constructor (name, age, sex) {
    super(name, age, sex);
  }
  learn () {
    console.log('You see see u, one day day 的');
  }
}

let stu = new Student('马宾', 18, '男');

stu.learn();
stu.eat();
stu.drink();
stu.think();

class Assistant extends Human {
  constructor (name, age, sex) {
    super(name, age, sex);
  }
  lock () {
    console.log('锁门');
  }
}

let assis = new Assistant('王五', 40, '男');
assis.lock();
assis.drink();
```