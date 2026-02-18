## TS

**TypeScript 是 JavaScript 的超集**
简单来说，所有合法的 JS 代码都是合法的 TS 代码，但 TS 给 JS 增加了「类型系统」和一些 ES6+ 之外的新特性，最终 TS 代码会被编译成 JS 代码才能运行。

|特性	|JavaScript (JS)|	TypeScript (TS)|
|:--:|:--:|:--:|
|类型系统|	动态类型（运行时检查）|	静态类型（编译时检查）|
|代码提示 / 报错|	弱，运行时才发现错误|	强，编写时就提示错误|
|学习成本|	低	|中等（在 JS 基础上加类型知识）|
|运行环境|	直接运行	|需编译为 JS 后运行|
|大型项目适配性	|差（易出隐藏 bug）|	优（类型约束降低维护成本）|

<hr/>

### 变量类型可以显示声明

**JS 中变量类型是动态的，TS 则可以显式声明类型，提前规避类型错误** 代码更易维护

```js
// JS 代码（无类型约束，容易出错）
let age = 18;
age = "十八"; // 不会报错，但逻辑上不合理

// TS 代码（显式声明类型，编译时报错）
let age: number = 18; // 声明 age 是数字类型
// age = "十八"; // 编译时直接报错：不能将类型“string”分配给类型“number”
```

> TS可以类型推导，不显示声明也会推导类型，同样会报错

<hr/>

### 函数的类型约束

代码更易维护
```js
// JS 函数（参数/返回值无约束）
function add(a, b) {
  return a + b;
}
add(10, "20"); // 结果是 "1020"，非预期但不报错

// TS 函数（约束参数和返回值类型）
function add(a: number, b: number): number {
  return a + b;
}
// add(10, "20"); // 编译报错：参数“b”的类型不匹配
console.log(add(10, 20)); // 正确输出 30
```

<hr/>

### 接口（TS 特有，约束对象结构）

**TS 的接口可以规范对象的属性和类型，适合复杂对象的定义** 代码更易维护

```js
// TS 接口：约束 User 对象的结构
interface User {
  name: string;
  age: number;
  isAdmin?: boolean; // 可选属性（加 ? 表示可省略）
}

// 符合接口的对象
const user1: User = {
  name: "张三",
  age: 20
};

// 不符合接口的对象（编译报错）
// const user2: User = {
//   name: 123, // 类型错误：name 必须是 string
//   age: "20"  // 类型错误：age 必须是 number
// };
```

<hr/>

### TS 编译为 JS

**TS 不能直接运行，需要通过 tsc（TypeScript 编译器）编译为 JS**

```bash
# 1. 全局安装 TypeScript
npm install -g typescript

# 2. 编写 ts 文件（如 demo.ts）
# 3. 编译 ts 为 js
tsc demo.ts

# 4. 运行编译后的 js 文件
node demo.js
```