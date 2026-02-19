## RequestAnimationFrame，实现平滑滚动到顶部

requestAnimationFrame 是浏览器提供的 **「动画专用 API」**，相比定时器：

- **自动对齐浏览器的刷新频率**（通常 60Hz，即 16.7ms / 帧），避免卡顿；
- **页面隐藏时自动暂停，节省性能**
- **动画更平滑，无定时器的 “跳帧” 问题**

```js
function scrollToTop(duration = 500) {
  // 1. 获取当前滚动位置（距离文档顶部的距离）
  const startPosition = window.scrollY;
  // 2. 计算滚动总距离（就是当前位置，因为要到顶部）
  const distance = startPosition;
  // 3. 记录动画开始时间（用于计算进度）
  let startTime = null;

  // 4. 动画核心函数（由RAF调用）
  function animation(currentTime) {
    // 初始化开始时间
    if (!startTime) startTime = currentTime;
    // 计算已过去的时间（毫秒）
    const timePass = currentTime - startTime;
    // 计算滚动进度（0~1），用缓动函数让滚动更自然
    const progress = Math.min(timePass / duration, 1);
    // 缓动公式：easeOutCubic（先快后慢，更符合用户体验）
    const easeProgress = 1 - Math.pow(1 - progress, 3);
    
    // 5. 计算当前要滚动到的位置
    const currentPosition = startPosition - (distance * easeProgress);
    // 6. 执行滚动
    window.scrollTo(0, currentPosition);

    // 7. 未完成时，继续调用RAF
    if (timePass < duration) {
      requestAnimationFrame(animation);
    }
  }

  // 8. 启动动画
  requestAnimationFrame(animation);
}

// ========== 使用示例 ==========
// 给按钮绑定点击事件，点击后500ms平滑滚到顶部
document.getElementById('toTopBtn').addEventListener('click', () => {
  scrollToTop(500); // 可自定义时长，比如800ms更慢
});
```

> **currentTime: 浏览器自动传递给 RAF 回调函数的「时间戳参数」** —— 表示当前浏览器执行这一帧的时间（基于系统时间，精确到微秒级别）