## block/inline元素 区别

<br/>

### 1. inline（行内）元素的特点：
- **行内元素不会独占一行，多个行内元素会排成一行**，直到充满整行之后再换行。
- **行内元素不能设置width和height属性**，它的宽和高是元素内容的宽和高。
- **设置margin和padding属性时，margin-top，margin-bottom，padding-top，padding-bottom无效。**
- 行内元素的padding-top和padding-bottom在浏览器中会显示出效果，但是并没有实际作用，对周围的元素没有影响。

<br/>

### 2. block元素的特点
- **块级元素前后会单独换行。**
- **块级元素设置width,height,margin,padding属性有效。**
> 常见的块级元素（div、p、h1~h6、ul、ol、dl、li、dd、table、hr、blockquote、address、table、menu、pre，HTML5新增的header、section、aside、footer）

<br/>

### 3. inline-block的特点
- **设置为inline-block的元素仍然呈现为inline元素，但是其中的内容作为block内容呈现。**
- width，height，margin，padding属性有效。
- **相邻的元素仍然在同一行内排列**

<br/>

### 4. 置换元素特殊情况
`<img>/<input>/<button>` 这类 “置换元素(可替换元素)”，默认是 inline-block（行内块）—— 兼具 inline 和 block 的特性：

- 显示方式：同行排列（inline 特性）；
- 宽高 / 边距：可自由设置（block 特性）；