## JS创建dom节点添加元素

### 创建 & 添加 DOM 节点

##### 1. 创建 DOM 节点

最常用的是 `document.createElement(tagName)`，用于创建指定标签的元素节点

```js
// 创建一个 <div> 节点
const div = document.createElement('div');
// 创建一个 <p> 节点
const p = document.createElement('p');
// 创建一个 <img> 节点
const img = document.createElement('img');
```

<br/>

##### 2. 给节点设置属性 / 内容

创建节点后，通常需要设置文本、样式、属性等

```js
// 1. 设置文本内容（推荐，避免XSS）
div.textContent = '这是一个新的div';
// 2. 设置HTML内容（慎用，有XSS风险）
p.innerHTML = '<span style="color: red;">这是带样式的p标签</span>';
// 3. 设置属性
img.src = 'https://example.com/photo.jpg';
img.alt = '示例图片';
// 4. 设置样式（行内样式）
div.style.width = '200px';
div.style.height = '100px';
div.style.backgroundColor = 'lightblue';
// 5. 添加类名
div.classList.add('box', 'container'); // 可添加多个类名
```

<br/>

##### 3. 将节点添加到页面

创建的节点默认只存在于内存中，必须通过以下方法添加到 DOM 树才会显示

|方法|	作用|	示例|
|:--:|:--:|:--:|
|parent.appendChild(child)	|把节点添加到父元素的最后一个子元素后面|	document.body.appendChild(div)|
|parent.prepend(child)	|把节点添加到父元素的第一个子元素前面	|document.body.prepend(p)|
|parent.insertBefore(newNode, referenceNode)|	把节点插入到参考节点前面	|document.body.insertBefore(img, div)|
|parent.replaceChild(newNode, oldNode)|	用新节点替换旧节点	|document.body.replaceChild(div, p)|

> parent（必须是 referenceNode 或 oldNode 的直接父节点）；

<hr/>

### 进阶场景

##### 1. 批量创建 / 添加节点（性能优化）

**如果需要添加大量节点，直接循环 appendChild 会频繁触发重排 / 重绘，建议先用 DocumentFragment 暂存**

```js
const fragment = document.createDocumentFragment(); // 虚拟容器，不触发重排

// 批量创建10个li节点
for (let i = 1; i <= 10; i++) {
  const li = document.createElement('li');
  li.textContent = `列表项 ${i}`;
  fragment.appendChild(li); // 先添加到虚拟容器
}

// 一次性添加到页面，仅触发1次重排
const ul = document.createElement('ul');
ul.appendChild(fragment);
document.body.appendChild(ul);
```

<br/>

##### 2. 创建文本节点（单独）

如果只需要添加纯文本，也可以直接创建文本节点

```js
const textNode = document.createTextNode('纯文本内容');
document.body.appendChild(textNode);
```

<br/>

##### 3. 移除节点（补充）

创建后如需移除，使用 `node.remove()` 或 `parent.removeChild(child)`

```js
// 方式1：节点自身调用remove（推荐）
newDiv.remove();
// 方式2：通过父元素移除
container.removeChild(oldP);
```

<hr/>

### 注意事项

- **节点只能存在一个位置**：如果把已添加到 DOM 的节点再次 appendChild，它会从原位置移除，移动到新位置；

```js
const div = document.createElement('div');
document.body.appendChild(div);
document.getElementById('container').appendChild(div); // div 会从body移动到container
```