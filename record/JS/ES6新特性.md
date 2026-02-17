## ES6新特性

#### 1. let/const 替代 var

<br/>

#### 2. 解构赋值：简化数据提取

- 数组解构
```js
// 基础解构
const [a, b, c] = [1, 2, 3];
console.log(a, b, c); // 1 2 3

// 跳过元素
const [x, , y] = [1, 2, 3];
console.log(x, y); // 1 3

// 默认值
const [m, n = 10] = [5];
console.log(m, n); // 5 10

// 剩余参数
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest); // [2,3,4]
```

- 对象解构
```js
// 基础解构（属性名匹配）
const { name, age } = { name: '小明', age: 18 };
console.log(name, age); // 小明 18

// 重命名
const { name: userName, age: userAge } = { name: '小明', age: 18 };
console.log(userName); // 小明

// 默认值
const { gender = '男' } = { name: '小明' };
console.log(gender); // 男

// 嵌套解构
const obj = { info: { id: 1, score: 90 } };
const { info: { id, score } } = obj;
console.log(id, score); // 1 90
```

<br/>

#### 3. 函数增强：默认参数 / 剩余参数 / 箭头函数

###### 默认参数

```js
// ES5 写法（有坑：0/'' 会被判定为 false）
function fn1(a) {
  a = a || 10;
  console.log(a);
}
fn1(0); // 10（不符合预期）

// ES6 写法（仅参数为 undefined 时生效）
function fn2(a = 10) {
  console.log(a);
}
fn2(0); // 0（符合预期）
fn2(); // 10（未传参）
```

###### 剩余参数

```js
// ES5：arguments 是类数组，需转换
function sum1() {
  return Array.prototype.slice.call(arguments).reduce((a, b) => a + b);
}

// ES6：剩余参数直接是数组
function sum2(...nums) {
  return nums.reduce((a, b) => a + b);
}
console.log(sum2(1,2,3)); // 6s
```

###### 箭头函数（=>）：箭头函数继承外层 this

```js
// 基础写法
const add = (a, b) => a + b; // 单行返回，省略 {} 和 return

// 多行逻辑需加 {}
const fn = (a) => {
  a++;
  return a;
};

// 解决 this 指向问题（高频场景）
const obj = {
  name: '小明',
  fn1: function() {
    // ES5：需保存 this
    const that = this;
    setTimeout(function() {
      console.log(that.name); // 小明
    }, 0);
  },
  fn2: function() {
    // ES6：箭头函数继承外层 this
    setTimeout(() => {
      console.log(this.name); // 小明
    }, 0);
  }
};
```

<br/>

#### 4. 新数据结构：Set/Map（解决数组 / 对象痛点）

###### Set：无重复值的集合

- 存储唯一值（自动去重），支持所有数据类型；
- 类数组结构，可通过 Array.from() 转为数组；
- 常用方法：add()/delete()/has()/clear()/size

###### Map：键值对集合（替代普通对象）

- 键可以是任意类型（对象 / 函数 / 基本类型），普通- 对象键只能是字符串 / Symbol；
- 可通过 size 获取长度，普通对象需手动遍历；
- 遍历更便捷，支持 forEach()/for...of。

<br/>

#### 5. 新运算符：扩展运算符 / 可选链 / 空值合并（ES6+ 扩展）

<br/>

###### 扩展运算符（...）：展开可迭代对象

**数组展开 / 合并**
```js
const arr1 = [1, 2];
const arr2 = [3, 4];
// 合并数组
const arr3 = [...arr1, ...arr2]; // [1,2,3,4]
// 展开数组作为函数参数
console.log(Math.max(...arr1)); // 2
```

**对象展开 / 合并**
```js
const obj1 = { name: '小明' };
const obj2 = { age: 18 };
// 合并对象（后面对象覆盖前面）
const obj3 = { ...obj1, ...obj2, age: 20 };
console.log(obj3); // {name: '小明', age: 20}
```

<br/>

###### 可选链运算符（?.）：ES2020 新增，避免属性不存在报错

**语法：obj?.a?.b，某一级不存在则返回 undefined**
```js
const obj = { info: { name: '小明' } };
// ES5：多层判断，避免报错
console.log(obj && obj.info && obj.info.age); // undefined

// ES6+：可选链，简洁安全
console.log(obj?.info?.age); // undefined
console.log(obj?.data?.list); // undefined（无报错）
```

<br/>

###### 逻辑赋值运算符（??=/&&=/||=）：ES2021 新增
```js
let a = undefined;
a ??= '默认值'; // 等价于 a = a ?? '默认值'
console.log(a); // '默认值'

let b = true;
b &&= '值'; // 等价于 b = b && '值'
console.log(b); // '值'
```

<br/>

#### 6. 其它特性

###### 模板字符串（`）：支持换行 / 变量插入
```js
const name = '小明';
const age = 18;
// ES5：拼接繁琐
const str1 = '姓名：' + name + '\n年龄：' + age;

// ES6：模板字符串，简洁
const str2 = `姓名：${name}
年龄：${age}
年龄+1：${age + 1}`;
console.log(str2);
```

###### 类（class）：简化构造函数写法
```js
// ES5：构造函数
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function() {
  console.log(`你好，${this.name}`);
};

// ES6：class 语法糖
class Person {
  constructor(name) {
    this.name = name; // 构造方法
  }

  sayHi() { // 原型方法
    console.log(`你好，${this.name}`);
  }

  static staticFn() { // 静态方法（类本身的方法）
    console.log('静态方法');
  }
}

const p = new Person('小明');
p.sayHi(); // 你好，小明
Person.staticFn(); // 静态方法
```

###### 模块化（import/export）：解决全局作用域污染
```js
// 导出模块（module.js）
export const name = '小明';
export function fn() { return 1; }
export default { age: 18 };

// 导入模块
import { name, fn } from './module.js'; // 导入具名导出
import obj from './module.js'; // 导入默认导出
```