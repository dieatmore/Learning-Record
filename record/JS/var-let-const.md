## var/let/const

|特性|	var | let|	const|
|:--:|:--:|:--:|:--:|
|作用域|	函数作用域 / 全局作用域|	块级作用域|	块级作用域|
|变量提升	|有（可先使用后声明）|	有（暂时性死区）|	有（暂时性死区）|
|重复声明	|允许	|不允许	|不允许|
|重新赋值|	允许|	允许	|不允许（引用类型可修改内部）|
|必须初始化	|否	|否|	是|

<hr/>

### 作用域差异

```js
// 1. var 的函数作用域（无块级作用域）
if (true) {
  var a = 10; // 全局作用域
}
console.log(a); // 输出 10（能访问，因为 var 无视 if 的 {} 块）

// 2. let 的块级作用域
if (true) {
  let b = 20; // 块级作用域（仅 if 内部可用）
}
// console.log(b); // 报错：b is not defined

// 3. 经典的 for 循环问题（var 导致的坑）
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i); // 输出 3、3、3（var 是全局的，循环结束后 i=3）
  }, 100);
}

// let 解决 for 循环问题
for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log(j); // 输出 0、1、2（每次循环都创建新的 j）
  }, 100);
}
```

<hr/>

### 变量提升与暂时性死区

- 变量提升：JS 引擎会把变量声明「提升」到当前作用域顶部，但赋值留在原地；
- var：提升后默认值为 undefined，可以「先使用后声明」；
- let/const：虽也有提升，但存在「暂时性死区（TDZ）」—— 声明前使用会报错，避免了不合理的提前使用。

```js
// var 的变量提升（无 TDZ）
console.log(c); // 输出 undefined（提升了声明，未提升赋值）
var c = 30;

// let 的暂时性死区
// console.log(d); // 报错：Cannot access 'd' before initialization
let d = 40;

// const 同理
// console.log(e); // 报错：Cannot access 'e' before initialization
const e = 50;
```

<hr/> 

###  重复声明与重新赋值

- var：允许同一作用域内重复声明同一变量，后声明的会覆盖前一个；
- let/const：同一作用域内禁止重复声明，避免变量覆盖的坑；
- const：声明时必须初始化，且不能重新赋值（但引用类型如对象 / 数组，可修改内部属性 / 元素）

<hr/>

TS 完全继承了 JS 中三者的特性，且官方 *推荐优先使用 const，其次 let，避免使用 var**：
- 声明后不需要修改的变量（如常量、固定引用的对象 / 数组）：用 const（结合类型注解更规范）；
- 声明后需要重新赋值的变量：用 let + 类型注解；
- 绝对不要用 var：TS 会提示警告，且容易引发作用域相关的 bug。