## flex/flex: 1

### flex
**flex 是 flex-grow、flex-shrink、flex-basis 三个属性的简写**

- flex-grow(扩张因子)：**父容器有剩余空间时，子元素按比例瓜分剩余空间（0 = 不瓜分，正数 = 按比例分）**—— 默认0
- flex-shrink(收缩因子)：**父容器空间不足时，子元素按比例收缩（0 = 不收缩，正数 = 按比例缩）** —— 默认1
- flex-basis(初始基准尺寸)：**子元素在分配空间前的默认尺寸（可以是 px、%、auto 等）**c —— 默认auto, 即本来大小

### flex: 1

**flex: 1 是一个简写值，等价于 flex: 1 1 0%;**

- flex-grow: 1 父容器有剩余空间时，该元素会按比例（和其他 flex-grow>0 的元素）瓜分剩余空间；
- flex-shrink: 1 父容器空间不足时，该元素会按比例收缩，避免溢出；
- flex-basis: 0% 该元素的初始基准尺寸为 0（不占默认空间，完全按比例分配）。

> flex:1 
> - **父容器必须设置 display: flex**（或 inline-flex），否则 flex 属性完全无效；
> - **子元素不要同时设置 width: 100%（会覆盖 flex 的比例分配逻辑）；**
> - flex:1 是 “主轴方向” 的分配（**默认主轴是水平方向，控制宽度；如果设置 flex-direction: column，则控制高度**）。

### flex:1 vs flex:auto

flex:auto 等价于 flex: 1 1 auto

> **核心区别：**
> - flex:1：初始尺寸 0，剩余空间全按比例分（等分 / 占满剩余）；
> - flex:auto：初始尺寸为内容宽度，剩余空间再按比例分（先占内容宽，再分剩余）。