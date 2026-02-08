## transition/transform/animation

- **transform**：负责元素的变形 / 位移（比如移动、旋转、缩放），是 “**做什么变化**”；
- **transition**：负责让样式变化从 “瞬间跳变” 变成 “平滑过渡”，是 “**怎么平滑变**”；
- **animation**：负责自定义多步骤、可循环的关键帧动画，是 “**复杂自动变**”。

### 一、transform：元素的 “变形 / 位移”（无动画，只是视觉变化）

##### 1. 旋转(rotate)
**主要分为2D旋转和3D旋转。**
- rotate(angle)，2D 旋转，参数为角度，如45deg
- rotate(x,y,z,angle)，3D旋转，围绕原地到(x,y,z)的直线进行3D旋转
- rotateX(angle)，沿着X轴进行3D旋转
- rotateY(angle)
- rotateZ(angle)

##### 2. 缩放(scale)
**一般用于元素的大小收缩设定。**
- scale(x, y)
- scale3d(x, y, z)
- scaleX(x)
- scaleY(y)
- scaleZ(z)
  > 其中x、y、z为收缩比例。

##### 3. 倾斜(skew)

**主要用于对元素的样式倾斜。**
- skew(x-angle, y-angle)，沿着x和y轴的2D倾斜转换
- skewX(angle)，沿着x轴的2D倾斜转换
- skew(angle)，沿着y轴的2D倾斜转换

##### 4. 移动(translate)
**主要用于将元素移动。**
- translate(x, y)，定义向x和y轴移动的像素点
- translate(x, y, z)，定义像x、y、z轴移动的像素点
- translateX(x)
- translateY(y)
- translateZ(z)

### 二、transition：样式变化的 “平滑过渡”（需要触发条件）

transition 不能单独产生动效，它的作用是给 “元素的样式变化”（比如 transform、width、background 等）添加 “平滑过渡动画”，让变化从 “瞬间跳变” 变成 “慢慢过渡”。

> **transition: 要过渡的属性 过渡时长 缓动函数 延迟时间;**

- 要过渡的属性：比如 transform、background、width（all 表示所有样式变化都过渡）；
- 过渡时长：比如 0.3s（必须写，否则没动画）；
- 缓动函数：动画的运动速度曲线。比如 ease（默认，先慢后快再慢）、linear（匀速）；
- 延迟时间：设置动画在开始之前的延迟。比如 0.1s（多久后开始过渡）。

### 三、animation：自定义 “关键帧动画”（自动、多步骤、可循环）

animation 是自定义多步骤动画，可以定义 “从开始到结束的多个关键状态”，无需触发条件（页面加载就可自动执行），还能控制循环、播放方向、暂停等，适合复杂动效。

<br/>

用法（两步走）
1. 用 @keyframes 定义 “关键帧”（动画的各个状态）；
2. 用 animation 属性调用这个关键帧动画。

<br/>

> **animation: 动画名 过渡时长 缓动函数 延迟时间 循环次数 播放方向;**

(1) 动画名：需要绑定到选择器的keyframe名称。

(2) 过渡时长：完成该动画需要花费的时间，秒或毫秒。

(3) 缓动函数：动画的运动速度曲线。比如 ease（默认，先慢后快再慢）、linear（匀速）；

(4) 延迟时间：设置动画在开始之前的延迟。

(5) 循环次数：设置动画执行的次数。infinite 表示无限循环；

(6) 播放方向：normal，默认值，动画应该正常播放；alternate 表示往返播放（比如去→回→去→回）

