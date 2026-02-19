## new创建对象过程

1. **创建空对象**：new 首先会在内存中创建一个全新的、空的普通对象（{}）。
   
2. **绑定原型**：将这个空对象的 __proto__ 指向构造函数的 prototype 属性，让新对象能继承构造函数原型上的属性和方法。
   
3. **执行构造函数**：将构造函数的 this 绑定到这个新创建的空对象上，然后执行构造函数内部的代码（目的是给新对象添加专属的属性 / 方法）。
   
4. **返回对象**：
     - 如果构造函数没有显式返回对象 / 函数，则自动返回这个新创建的对象；
     - 如果构造函数显式返回了一个对象 / 函数，则 new 操作的结果会变成这个返回值（而非第一步创建的空对象）。

```js
// 定义一个构造函数
function Person(name, age) {
  // 步骤3：this 指向新创建的空对象，给对象添加属性
  this.name = name;
  this.age = age;
  // 注意：这里没有显式返回值，会自动返回新对象
}

// 给原型添加共享方法（步骤2会让新对象继承这个方法）
Person.prototype.sayHi = function() {
  console.log(`你好，我是${this.name}，今年${this.age}岁`);
};

// 使用 new 创建对象
const person1 = new Person("张三", 20);

// 验证结果
console.log(person1.name); // 输出：张三（构造函数给对象加的属性）
person1.sayHi(); // 输出：你好，我是张三，今年20岁（继承原型的方法）
console.log(person1.__proto__ === Person.prototype); // 输出：true（步骤2的原型绑定）




// 构造函数显式返回,new 操作的结果会变成这个返回值
function Person(name) {
  this.name = name;
  // 显式返回一个新对象
  return { nickname: "小李" };
}

const person2 = new Person("李四");
console.log(person2.name); // 输出：undefined（原新对象被覆盖）
console.log(person2.nickname); // 输出：小李（返回的是显式的对象）
```