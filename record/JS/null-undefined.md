## null/undefined

- **undefined 是 JS 自动赋予的「未定义值」，表示 “值缺失”**
- **null 是开发者手动赋值的「空值」，表示 “有意识的空”**

|类型	|核心定义	|本质特性|
|:--:|:--:|:--:|
|undefined|	表示「未定义」，即变量 / 属性「存在但未赋值」，是 JS 自动赋予的默认值|	被动的、无意识的空（JS 引擎自动生成）|
|null|	表示「空值」，即变量 / 属性「存在且手动赋值为空」，是程序员主动设置的空引用 |	主动的、有意识的空（开发者手动赋值）|

> **1. typeof null === 'object'（JS 历史 bug）**
> **2. undefined == null 为 true，但语义不同，实际开发中优先用严格相等（===）**

<br/>

**为什么 null + 1 和 undefined + 1 表现不同？**

```js
let a = undefined + 1  // NaN
let b = null + 1  // 1
Number(undefined)  // NaN
Number(null)  // 0
```

这涉及到 JavaScript 中的隐式类型转换，在执行 加法运算 前，隐士类型转换会尝试将表达式中的变量转换为 number 类型。如：'1' + 1 会得到结果 11。

- null 转化为 number 时，会转换成 0

- undefined 转换为 number 时，会转换为 NaN
