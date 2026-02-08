## BFC

**BFC**（Block Formatting Context，**块级格式化上下文**）**本质是一个独立的 “布局容器”**：<u>容器内的块级元素按自己的规则排版，完全不会影响容器外的元素，容器外的元素也无法影响容器内的元素。</u>

### 如何触发 BFC
- 根元素（`<html>`）	页面最外层天然是 BFC
- overflow: hidden/auto/scroll	最常用
- float: left/right	浮动元素自动成为 BFC	
- position: absolute/fixed	绝对 / 固定定位元素是 BFC
- display: inline-block	行内块元素是 BFC	
- display: flex/grid	flex/grid 容器是 BFC

### BFC应用场景
**1、解决当父级元素没有高度时，子级元素浮动会使父级元素高度塌陷的问题** —— 给父元素触发 BFC（overflow: hidden）：
 
**2、解决子级元素外边距会使父级元素塌陷的问题** —— 给父元素触发 BFC（overflow: hidden）：

**3、解决两栏布局（左侧元素浮动后，右侧元素会 “钻” 到左侧下面，无法自适应剩余宽度）** —— 给右侧元素触发 BFC（overflow: hidden）：