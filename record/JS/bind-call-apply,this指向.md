## bind/call/apply，this指向

**this 表示「函数执行时的上下文对象」**，它的指向只和函数「怎么调用」有关，和「在哪定义」无关。先记住 4 条基础规则（**优先级从低到高**）

|调用方式	|this 指向	|示例|
|:--:|:--:|:--:|
|普通函数调用|	全局对象（浏览器：window，Node：global）/ 严格模式下 undefined|	function fn() { console.log(this) } fn()
|对象方法调用	|调用该方法的对象|	obj.fn() → this 指向 obj|
|构造函数调用（new）|	新创建的实例对象|	new Person() → this 指向新实例|
|事件 / 定时器回调	|浏览器：window；严格模式下 undefined	|setTimeout(fn, 100) → this 指向 window|

```js
// 1. 普通函数调用（全局上下文）
function normalFn() {
  console.log(this); // 浏览器：window，Node：global
}
normalFn();

// 2. 对象方法调用（指向调用对象）
const person = {
  name: "张三",
  sayHi() {
    console.log(this.name); // this 指向 person
  }
};
person.sayHi(); // 输出 张三

// 3. 构造函数调用（指向新实例）
function Person(name) {
  this.name = name; // this 指向 new 创建的实例
}
const p1 = new Person("李四");
console.log(p1.name); // 输出 李四

// 4. 定时器回调（全局上下文）
setTimeout(person.sayHi, 100); // 输出 undefined（this 指向 window，window 没有 name 属性）
```

<hr/>

### bind/call/apply：显式绑定 this 指向

**当默认的 this 指向不符合预期时（比如上面的定时器示例），就需要用 bind/call/apply 手动绑定 this**。三者的核心作用都是 **改变函数的 this 指向**，区别仅在于 **参数传递方式** 和 **是否立即执行**

|方法	|是否立即执行|	参数传递方式|	返回值|
|:--:|:--:|:--:|:--:|
|call	|是|	逐个传参（fn.call (thisObj, arg1, arg2)）|	函数执行结果|
|apply	|是|	数组传参（fn.apply (thisObj, [arg1, arg2])）	|函数执行结果|
|bind	|否|	逐个传参（可后续补传）|	绑定 this 后的新函数|

<br/>

**（1）call：立即执行 + 逐个传参**

> **语法：函数.call(绑定的this对象, 参数1, 参数2, ...)**

```js
// 一个独立函数（和任何对象都没关系）
function sayHi(age, gender) {
  console.log(`我是${this.name}，${age}岁，${gender}`);
}

// 一个独立对象
const person = { name: "张三" };

// 问题：直接调用 sayHi(20, "男") → this 指向 window，输出 undefined
sayHi(20, "男"); // 我是undefined，20岁，男

// 解决：用 call 强制让 sayHi 的 this 指向 person
sayHi.call(person, 20, "男"); // 我是张三，20岁，男
```

<br/>

**（2）apply：立即执行 + 数组传参**

> **语法：函数.apply(绑定的this对象, [参数1, 参数2, ...])**

```js
// 接上面的 person 示例
const args = [20, "男"];
// apply 接收数组参数，自动拆解
person.sayHi.apply(person, args); // 输出：我是张三，20岁，男

// 经典场景：求数组最大值（借用 Math.max）
const arr = [1, 5, 3, 9, 2];
// Math.max 不支持数组，但 apply 可以传数组
const max = Math.max.apply(null, arr); // null 表示 this 指向全局（Math.max 不依赖 this）
console.log(max); // 输出 9
```

<br/>

**（3）bind：不立即执行 + 绑定 this 后返回新函数**

> **语法：const 新函数 = 函数.bind(绑定的this对象, 参数1, 参数2, ...)**
   - 适用场景：需要「延迟执行」的函数（比如定时器、事件回调）

```js
// 修复定时器的核心方案：用 bind 绑定 this，返回新函数
const person = {
  name: "张三",
  sayHi(age) {
    console.log(`我是${this.name}，${age}岁`);
  }
};

// bind 绑定 this 为 person，并预设参数 20，返回新函数
const bindFn = person.sayHi.bind(person, 20);
setTimeout(bindFn, 100); // 100ms 后输出：我是张三，20岁

// bind 可后续补传参数（预设的参数在前，补传的在后）
const bindFn2 = person.sayHi.bind(person);
bindFn2(25); // 输出：我是张三，25岁
```

<hr/>

**特殊场景：箭头函数的 this 指向**

箭头函数 **没有自己的 this**，它的 this 继承自「定义时的外层普通函数 / 全局上下文」，且 **无法通过 bind/call/apply 修改**

```js
const person = {
  name: "张三",
  // 普通函数：this 指向 person
  normalFn() {
    console.log(this.name);
  },
  // 箭头函数：this 继承外层（全局），而非 person
  arrowFn: () => {
    console.log(this.name); // this 指向 window，无 name 属性
  }
};

person.normalFn(); // 输出 张三
person.arrowFn(); // 输出 undefined

// 箭头函数无法通过 bind 修改 this
const bindArrow = person.arrowFn.bind(person);
bindArrow(); // 依然输出 undefined
```