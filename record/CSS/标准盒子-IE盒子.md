## 标准盒子/IE盒子

CSS中存在两种盒子模型：
- **标准盒子** (外扩模型)：**box-sizing: content-box（默认）**
- **IE盒子**(怪异盒模型/内扩模型/边框盒模型)：**box-sizing: border-box**

<br/>

标准盒子设置width和height时，**仅是给content设置宽高**，不包括padding和border

IE盒子的width和height则是 **content+padding+border**，更符合现实直觉。

实际开发中一般都使用border-box。