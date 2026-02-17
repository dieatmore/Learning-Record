## settimeout/setinterval

|API	|作用|	基础语法|
|:--:|:--:|:--:|
|setTimeout|	延迟指定毫秒后，执行一次回调函数|	const timerId = setTimeout(回调函数, 延迟毫秒数, 回调参数1, 回调参数2...)|
|setInterval|	每隔指定毫秒，重复执行回调函数|	const timerId = setInterval(回调函数, 间隔毫秒数, 回调参数1, 回调参数2...)|


 **“延迟 / 间隔毫秒数” 不是 “绝对执行时间”，而是 “回调进入宏任务队列的最早时间”。**
> - setTimeout(fn, 1000)：告诉 JS 引擎 “**至少** 1000ms 后，把 fn 推入宏任务队列”；
> - 只有当执行栈为空（同步代码 + 微任务都执行完），且宏任务队列中没有更早的任务时，才会执行这个 fn；
> - 如果主线程被阻塞（比如同步代码执行了 2 秒），那么 fn 的实际执行时间会远大于 1000ms

### setInterval 的 “堆积执行” 问题

```js
// setInterval 回调执行时间超过间隔，会堆积
let i = 0;
setInterval(() => {
  console.log(`第${++i}次执行`);
  // 回调执行时间2秒，间隔1秒 → 会堆积
  const start = Date.now();
  while (Date.now() - start < 2000) {}
}, 1000);
```

> 现象：第一次执行耗时 2 秒，期间 setInterval 会在 1 秒时尝试推入第二个回调，但因第一个还在执行，会堆积；第一个执行完后，第二个立即执行，没有间隔。

### **解决方案：用 setTimeout 递归实现稳定间隔**

```js
// 递归setTimeout：间隔=延迟+回调执行时间，更稳定
let i = 0;
function repeatFn() {
  console.log(`第${++i}次执行`);
  // 回调执行2秒
  const start = Date.now();
  while (Date.now() - start < 2000) {}
  // 执行完后，再延迟1秒入队（总间隔3秒，稳定）
  setTimeout(repeatFn, 1000);
}
repeatFn();
```

### 常见坑点与注意事项

##### this 指向问题
定时器回调中的 this 默认指向 window（浏览器）/ global（Node），而非定义时的上下文：
```js
const obj = {
  name: '小明',
  sayHi: function() {
    setTimeout(function() {
      console.log(this.name); // undefined（this指向window）
    }, 0);
  }
};
obj.sayHi();

// 解决：用箭头函数/绑定this
setTimeout(() => {
  console.log(this.name); // 小明（箭头函数继承外层this）
}, 0);
```

**延迟最小值限制**
**浏览器对定时器的延迟有最小限制（约 4ms）**，即使设置 setTimeout(fn, 0)，实际入队时间也会延迟约 4ms（Node 无此限制）。

**清除定时器的注意事项**
- 必须保存 timerId，否则无法清除；
- clearTimeout 和 clearInterval 可以混用（底层都是清除定时器），但建议按语义使用；
- 清除已执行 / 已清除的定时器，不会报错（安全操作）。

**定时器不能保证异步操作的执行顺序**