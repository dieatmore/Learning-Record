## 手撕new

```javascript
function myNew(constructor, ...args) {
  // 1. 创建空对象
  const obj = {};

  // 2. 链接原型
  obj.__proto__ = constructor.prototype;

  // 3. 执行构造函数，this 指向 obj
  const res = constructor.apply(obj, args);

  // 4. 如果构造函数返回对象，就返回它；否则返回 obj
  return typeof res === 'object' && res !== null ? res : obj;
}
```

test
```javascript
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.say = function () {
  console.log(this.name);
};

// 使用我们自己的 new
const p = myNew(Person, '张三', 18);
p.say(); // 张三
console.log(p.age); // 18
```
