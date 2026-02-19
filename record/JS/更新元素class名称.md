## 更新元素class名称

### 1. 旧方法：直接操作 className（不推荐）

className 是元素的字符串属性，直接赋值会 **覆盖所有现有 class**，**适合一次性替换全部类名**

>  缺点：追加 / 移除时需 **手动处理** 空格和字符串，**容易出错，代码冗余**

```js
// 获取元素
const box = document.getElementById('box');

// 1. 覆盖所有class（原有class会被清空）
box.className = 'active'; // 此时元素只有 active 类

// 2. 追加class（需手动拼接，容易出错）
box.className = box.className + ' bg-red'; // 注意前面的空格，否则会变成 activebg-red

// 3. 移除class（需手动拆分、过滤，非常麻烦）
const classArr = box.className.split(' ');
const newClassArr = classArr.filter(cls => cls !== 'bg-red');
box.className = newClassArr.join(' ');
```

<hr/>

### 2. 推荐方法：使用 classList API

**classList 是元素的只读对象**，提供了一系列方法来 **精准操作单个 class**，不会影响其他类名 , 是目前的最佳实践

|方法	|作用	|示例|
|:--:|:--:|:--:|
|add(cls1, cls2...)|	添加一个 / 多个 class（重复添加无效果）	|box.classList.add('active', 'bg-blue')|
|remove(cls1, cls2...)	|移除一个 / 多个 class（移除不存在的类无报错）	|box.classList.remove('bg-red', 'hidden')|
|toggle(cls, force?)	|切换 class：存在则移除，不存在则添加； —— force 为 true 强制添加，false 强制移除	|box.classList.toggle('active') —— box.classList.toggle('active', true)（强制添加）|
|contains(cls)	|判断是否包含某个 class，返回布尔值|	box.classList.contains('active')|
|replace(oldCls, newCls)	|替换指定 class	|box.classList.replace('bg-red', 'bg-green')|

<hr/>

> classList 是伪数组，可以用 ... 展开为真数组，但不能直接用 forEach（部分浏览器支持，建议转数组后遍历）：

```js
// 遍历所有class
[...box.classList].forEach(cls => {
  console.log('当前类名：', cls);
});
```