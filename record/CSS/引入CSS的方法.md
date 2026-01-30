## 引入CSS的方法

### 一、引入 CSS 的三种核心方法

<br/>
<br/>

**样式优先级：行内样式>内联样式>外联样式**

<br/>

##### 1. 行内样式（内联样式）
**直接在 HTML 元素的style属性中编写 CSS 规则**

预览：`<div style="width: 100px; color: red;"></div>`。

核心特点：样式作用域仅限当前元素，无法复用；是最 “贴近元素” 的引入方式，优先级最高。

##### 2. 内联样式（嵌入式样式）
**在 HTML 文档的`<head>`标签内添加`<style>`标签，标签内编写 CSS 规则**

<u>样式作用域为当前整个 HTML 文档，可在文档内复用，但无法跨文档使用</u>

预览：
```html
<head>
  <style>
    div { width: 100px; color: red; }
  </style>
</head>
```


##### 3. 外联样式（引用外部 CSS 文件）
**将 CSS 规则写在独立的.css文件中，通过 HTML/ CSS 语法引入，是开发中最常用的方式**

<u>样式可跨多个 HTML 文档复用，便于代码维护和缓存（浏览器可缓存外部 CSS 文件）</u>

分两种方法实现：

- link 标签（HTML 标签）
- @import 规则（CSS 语法）

### 二、link/import

<br/>

##### 1. link 标签（HTML 标签）

**在`<head>`内通过`<link>`标签引入**

预览：`<link rel="stylesheet" href="style.css">；`

<br/>

##### 2. @import 规则（CSS 语法）

**可在`<style>`标签内或外部 CSS 文件中使用**

预览：`@import url("style.css");。`

<br/>

##### 3. link 与 @import 区别

- link 是 HTML 标签 —— @import 是 CSS 语法提供的
- link是HTML标签，无兼容问题 —— @import只在IE5以上才能识别
- link 的扩展能力远强于 @import，更能适配复杂的开发场景
- link 缓存独立 —— @import 缓存依赖父 CSS 文件，可控性差
- **link的并行加载能减少 CSS 加载耗时，降低页面无样式闪烁风险** —— **@import的串行加载易导致渲染延迟，多层嵌套时性能问题更突出**
- link 标签支持 JS 动态操作, 更加灵活 —— @import 规则：无原生动态操作能力
   > 示例（动态加载主题 CSS）:
   > -  link 标签可通过 DOM API 自由创建、删除、修改属性（如href、media），是动态切换样式（如夜间模式）、按需加载 CSS 的核心方案
   > <br/>
   > - @import 因为是 CSS 语法，JS 无法直接访问或修改 CSS 文件中的@import规则；若要调整，只能间接操作（如替换`<style>`标签的全部内容、重新加载整个 CSS 文件），操作复杂且性能差，几乎无法实现灵活的动态样式切换。

<br/>

##### 4. link 和 @import 本身无 “样式优先级高低”

link 和 @import 只是CSS 的引入方式，不会改变选择器的权重，也没有官方定义的 “link 优先级＞@import” 的规则

<u>加载优先级：link 是 HTML 核心资源，浏览器会给 link 分配更高的加载优先级（优先下载），而 @import 是 CSS 内部的资源，加载优先级低；</u>

