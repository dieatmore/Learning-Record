## 防抖

### 1. 防抖的本质
当一个事件频繁触发（比如输入框打字、窗口 resize、滚动），**防抖会让函数在最后一次触发后的指定时间才执行，如果在这个时间内再次触发，就重置计时**

> **避免频繁执行** 耗时操作（如输入框实时搜索的接口请求、滚动时的 DOM 计算），减少性能损耗，提升页面流畅度

<hr/>

**基础防抖：**
```js
function debounce(func, delay) {
    let timer = null;
    return function (...args) {
        clearTimeout(timer);
        timer = setTimeout(() => {
        func.apply(this, args);
        }, delay);
    };
}


const input = document.getElementById('searchInput');
// 输入停止500ms后才执行search
input.addEventListener('input', debounce(function(e) {
  search(e.target.value);
}, 500));
```

<br/>

**可立即执行版防抖：**

```js
function debounce(fn, delay, immediate = false) {
  let timer = null;
  
  return function(...args) {
    if (timer) clearTimeout(timer); // 取消定时器任务
    if (immediate && !timer) {
      fn.apply(this, args);
    }

    timer = setTimeout(() => {
      if (!immediate) { 
        fn.apply(this, args);
      }
      timer = null; 
    }, delay);
  };
}


// 使用：按钮点击，立即执行，3秒内重复点击无效
const btn = document.getElementById('btn');
btn.addEventListener('click', debounce(() => {
  console.log('提交');
}, 3000, true));
```