- [深入理解 DOM 和 CSSOM：网页渲染的核心](https://blog.csdn.net/u012446963/article/details/145808318?ops_request_misc=&request_id=&biz_id=102&utm_term=DOM/CSSOM&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-145808318.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)

## DOM/CSSOM

- **DOM 是 HTML 的树形对象（结构），CSSOM 是 CSS 的树形对象（样式），两者结合生成渲染树；**
- **DOM 让 JS 操作 HTML，CSSOM 让 JS 操作 CSS，同时为页面渲染提供依据；**
- **修改 DOM/CSSOM 会触发回流（布局）或重绘（样式），优化核心是减少回流次数，优先用 GPU 加速属性（如transform）**

### DOM（文档对象模型）

DOM
本质：浏览器将 HTML 文档解析成的树形结构对象，是 JS 操作 HTML 的接口。
核心作用：**让 JS 能访问、修改、添加、删除 HTML 元素（比如用document.getElementById获取元素）**

结构示例：
html
预览

```html
<html>
  <body>
    <div class="box">Hello</div>
  </body>
</html>
```
解析后的 DOM 树：

```markdown
plaintext
Document
└── html
    └── body
        └── div (class="box")
            └── 文本节点：Hello
常用操作（JS 操作 DOM）：
javascript
运行
// 获取DOM节点
const box = document.querySelector('.box');
// 修改内容
box.textContent = 'Hello DOM';
// 修改属性
box.classList.add('active');
// 删除节点
box.remove();
```

<br/>

###  CSSOM（CSS 对象模型）

本质：**浏览器将 CSS 样式（内联 / 外联 / 内嵌）解析成的树形结构对象，是 JS 操作 CSS 的接口**
核心作用：让 JS 能访问、修改 CSS 样式，同时为浏览器计算元素最终样式（布局渲染）提供依据。

结构示例：
css

```css
.box {
  width: 100px;
  height: 100px;
  background: red;
}
```
解析后的 CSSOM 树（简化）：
```markdown
plaintext
CSSStyleSheet
└── .box
    ├── width: 100px
    ├── height: 100px
    └── background: red
常用操作（JS 操作 CSSOM）：
javascript
运行
// 获取元素的计算样式（最终生效的样式）
const style = window.getComputedStyle(box);
console.log(style.width); // "100px"

// 直接修改行内样式（修改CSSOM）
box.style.background = 'blue';

// 添加样式表
const styleSheet = document.createElement('style');
styleSheet.textContent = '.box { border: 1px solid black; }';
document.head.appendChild(styleSheet);
```

<br/>

### DOM + CSSOM = 渲染树（Render Tree）

浏览器不会单独使用 DOM 或 CSSOM，而是将两者结合生成渲染树，这是页面渲染的核心步骤：
- 构建 DOM 树：解析 HTML，生成节点树；
- 构建 CSSOM 树：解析所有 CSS（包括外链、内联、内嵌），计算样式优先级；
- 生成渲染树：
   - 遍历 DOM 树，只保留可见元素（隐藏元素如display: none会被排除，visibility: hidden仍保留）；
   - 为每个可见 DOM 节点匹配 CSSOM 中的样式，形成 “DOM 节点 + 样式” 的渲染树。
  
<br/>

### Problem

<br/>

**为什么 CSS 会阻塞渲染？**
浏览器为了避免 “样式闪烁”（先渲染无样式的 DOM，再渲染有样式的 DOM），会等待 CSSOM 构建完成后，再结合 DOM 生成渲染树，因此 CSS 解析会阻塞渲染（但不会阻塞 DOM 解析）

<br/>

**JS 和 DOM/CSSOM 的执行顺序？**
浏览器先解析 HTML，构建 DOM 树；
遇到 CSS 时 **并行解析** ，构建 CSSOM 树；
遇到`<script>`标签时，暂停 DOM 构建，执行 JS（JS 可能操作 DOM/CSSOM）；
所有 DOM/CSSOM 构建完成后，生成渲染树，开始布局和绘制。