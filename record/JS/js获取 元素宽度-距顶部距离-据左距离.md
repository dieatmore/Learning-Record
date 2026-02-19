## js获取，元素宽度/距顶部距离/据左距离

在 JS 中获取元素尺寸 / 位置，主要分两类：

1. **CSS 样式值**：通过 style 属性获取（**仅能拿到行内样式，不推荐**）

2. **布局实际值**：通过 offsetXXX/clientXXX/getBoundingClientRect() 获取（**推荐，能拿到渲染后的真实尺寸 / 位置**）

<hr/>

### 1. 获取元素宽度

|方法|	含义	|包含边框 / 内边距 / 滚动条？	|示例|
|:--:|:--:|:--:|:--:|
|element.style.width	|行内样式设置的宽度|	仅行内样式值，无单位|	box.style.width → "200px"（仅行内设置时）|
|element.offsetWidth	|布局宽度（含边框 + 内边距 + 内容宽度 + 滚动条）|	包含 border + padding + 内容宽度 + 滚动条（若有）|	box.offsetWidth → 220（数值，无单位）|
|element.clientWidth	|可视宽度（含内边距，不含边框 / 滚动条）	|包含 padding + 内容宽度，不含 border / 滚动条	|box.clientWidth → 200|
|getBoundingClientRect().width	|布局宽度（同 offsetWidth，浮点数）|	包含 border + padding + 内容宽度 + 滚动条|	box.getBoundingClientRect().width → 220.0|

<hr/>

### 2. 获取元素距离顶部 / 左侧的距离（分两种场景）

##### (1) 距离视口顶部 / 左侧（相对浏览器窗口）

**使用 element.getBoundingClientRect()（最推荐，返回精准的浮点数）**

- top：元素上边缘到视口顶部的距离（滚动时会变化）；
- left：元素左边缘到视口左侧的距离（滚动时会变化）；

> **特点：若页面滚动，值会实时变化（相对视口）**

<br/>

##### (2) 距离文档顶部 / 左侧（相对整个网页，不受滚动影响）

**需要结合 getBoundingClientRect() + 页面滚动距离**

- 文档顶部 = element.getBoundingClientRect().top + window.scrollY；
- 文档左侧 = element.getBoundingClientRect().left + window.scrollX；

> 补充：旧方法 **offsetTop/offsetLeft 也能获取，但需递归计算**（父元素有定位时不准确）, **不推荐**

```js
  <script>
    const box = document.getElementById('box');

    // ========== 场景1：相对视口的距离（滚动时变化） ==========
    const rect = box.getBoundingClientRect();
    console.log('距离视口顶部：', rect.top); // 如 70.0（px）
    console.log('距离视口左侧：', rect.left); // 如 30.0（px）

    // 滚动页面后重新获取（值会变化）
    window.addEventListener('scroll', () => {
      const newRect = box.getBoundingClientRect();
      console.log('滚动后距离视口顶部：', newRect.top);
    });

    // ========== 场景2：相对文档的距离（不受滚动影响） ==========
    // 方法1：推荐（精准）
    const docTop = rect.top + window.scrollY;
    const docLeft = rect.left + window.scrollX;
    console.log('距离文档顶部：', docTop); // 固定值，如 70.0
    console.log('距离文档左侧：', docLeft); // 固定值，如 30.0

    // 方法2：offsetTop（需处理父元素定位，不推荐）
    function getOffsetTop(element) {
      let top = 0;
      // 递归累加父元素的offsetTop
      while (element) {
        top += element.offsetTop;
        element = element.offsetParent; // 取定位的父元素
      }
      return top;
    }
    console.log('offsetTop计算文档顶部：', getOffsetTop(box));
  </script>
```

<hr/>

### 补充：其他常用尺寸 / 位置属性

|属性|	作用|
|:--:|:--:|
|element.offsetHeight|	元素布局高度（含边框 + 内边距 + 内容高度 + 滚动条）|
|element.clientHeight|	元素可视高度（含内边距，不含边框 / 滚动条）|
|element.scrollTop	|元素内容滚动的垂直距离（如滚动容器滚了多少）|
|window.scrollY|	页面垂直滚动距离（文档顶部到视口顶部）|
|window.innerWidth	|视口宽度（含滚动条）|

