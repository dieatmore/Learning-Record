## splice()/slice()/split()/map()/find()/filter()/some()/every()/reduce()/keys()/values()/flat()

|类别	|方法列表|
|:--:|:--:|
|数组修改（原数组变）|	splice()|
|数组截取（原数组不变）|	slice()|
|字符串分割（转数组）	|split()|
|数组遍历 / 转换	|map()|
|数组查找（找单个）|	find()|
|数组过滤（找多个）|	filter()|
|数组判断（布尔结果）|	some()、every()|
|数组归并（聚合结果）|	reduce()|
|数组遍历（键 / 值）|	keys()、values()|
|数组扁平化|	flat()|

<hr/>

**splice ()：数组「修改」（增 / 删 / 改）→ 原数组改变**

- 作用：对数组进行 **添加 / 删除 / 替换** 元素，直接修改原数组；
- 语法：**arr.splice(起始索引, 删除数量, 新增元素1, 新增元素2, ...)**
- **返回被删除元素**（无删除则返回空数组）；
- 核心：**会改变原数组**，这是和 slice() 最核心的区别。

```js
const arr = [1, 2, 3, 4, 5];

// 1. 删除：从索引1开始，删除2个元素
const del = arr.splice(1, 2); 
console.log(arr); // [1,4,5]（原数组变）
console.log(del); // [2,3]（返回删除的元素）

// 2. 新增：从索引1开始，删除0个，新增6、7
arr.splice(1, 0, 6, 7);
console.log(arr); // [1,6,7,4,5]（原数组变）

// 3. 替换：从索引3开始，删除1个，替换为8
arr.splice(3, 1, 8);
console.log(arr); // [1,6,7,8,5]（原数组变）
```

<hr/>

**slice ()：数组 / 字符串「截取」→ 原对象不变**

- 作用：**截取** 数组 / 字符串的一部分，**不修改原对象**；
- 语法：**arr.slice(起始索引, 结束索引)**（**结束索引不包含（`[ )`）**，省略则到末尾；支持负数索引，-1 表示最后一个）；
- **返回新值**
- 核心：**原数组 / 字符串不变**

```js
// 数组截取
const arr = [1, 2, 3, 4, 5];
const newArr = arr.slice(1, 4); // 索引1到3（不包含4）
console.log(newArr); // [2,3,4]
console.log(arr); // [1,2,3,4,5]（原数组不变）
console.log(arr.slice(-2)); // [4,5]（负数索引：倒数2个）

// 字符串截取
const str = "hello world";
const newStr = str.slice(0, 5);
console.log(newStr); // "hello"
```

<hr/>

**split ()：字符串「分割」→ 转数组**

- 作用：将字符串 **按指定分隔符** 分割，**不修改原字符串**
- 语法：**str.split(分隔符, 限制返回数组长度)**（分隔符为空字符串 "" 则拆成单个字符）；
- **返回新数组**
- 核心：**仅字符串可用**，是 **字符串转数组** 的核心方法

```js
const str = "苹果,香蕉,橙子";

// 按逗号分割
const arr1 = str.split(",");
console.log(arr1); // ["苹果","香蕉","橙子"]

// 按逗号分割，只取前2个
const arr2 = str.split(",", 2);
console.log(arr2); // ["苹果","香蕉"]

// 拆成单个字符
const arr3 = "abc".split("");
console.log(arr3); // ["a","b","c"]
```

<hr/>

**map ()：数组「遍历转换」→ 返回新数组**

- 作用：遍历数组每个元素，执行回调函数，**不修改原数组**
- 语法：**arr.map((item, index, arr) => { 处理逻辑，返回新值 })**
  <br/>
   - <u>**arr是原数组的引用**，在回调里可以直接访问原数组的完整内容</u>
  <br/>
- **返回回调结果组成的新数组**
- 核心：**一一映射**（原数组有多少元素，新数组就有多少），适合 **数据转换**

```js
const arr = [1, 2, 3];

// 每个元素乘2
const newArr = arr.map(item => item * 2);
console.log(newArr); // [2,4,6]

// 对象数组转换（开发高频）
const users = [{ name: "张三", age: 20 }, { name: "李四", age: 25 }];
const userNames = users.map(user => user.name);
console.log(userNames); // ["张三","李四"]
```

<hr/>

**find ()：数组「查找单个」→ 返回第一个匹配项**

- 作用：遍历数组，无匹配则返回 undefined
- 语法：**arr.find((item, index, arr) => { 条件判断 })**
- **返回「第一个满足回调条件」的元素**
- 核心：找「**单个**」符合条件的元素（比如找 id 为 1 的用户），**找到即停止遍历**

```js
const users = [
  { id: 1, name: "张三" },
  { id: 2, name: "李四" },
  { id: 1, name: "王五" }
];

// 找id为1的第一个用户
const user = users.find(user => user.id === 1);
console.log(user); // { id: 1, name: "张三" }

// 无匹配项
const noUser = users.find(user => user.id === 3);
console.log(noUser); // undefined
```

<hr/>

**filter ()：数组「过滤多个」→ 返回新数组**

- 作用：遍历数组，**不修改原数组**
- 语法：**arr.filter((item, index, arr) => { 条件判断 })**
- 返回「**所有满足回调条件**」的元素组成的新数组
- 核心：找 **多个符合条件** 的元素，遍历全部元素。

```js
const arr = [1, 2, 3, 4, 5];

// 过滤出偶数
const evenArr = arr.filter(item => item % 2 === 0);
console.log(evenArr); // [2,4]

// 过滤年龄≥20的用户
const users = [
  { name: "张三", age: 18 }, 
  { name: "李四", age: 22 }, 
  { name: "王五", age: 25 }
];
const adultUsers = users.filter(user => user.age >= 20);
console.log(adultUsers); // [{name:"李四",age:22}, {name:"王五",age:25}]
```

<hr/>

**some ()：数组「判断是否存在」→ 返回布尔值**

- 作用：判断数组中「**是否存在至少一个**」满足回调条件的元素，**找到即停止遍历**
- 语法：**arr.some((item, index, arr) => { 条件判断 })**
- **返回布尔值**
- 核心：「**只要有一个满足**」就返回 true，否则 false

```js
const arr = [1, 2, 3, 4];

// 是否有偶数
const hasEven = arr.some(item => item % 2 === 0);
console.log(hasEven); // true

// 是否有年龄≥30的用户
const users = [{ age: 18 }, { age: 25 }];
const hasAdult = users.some(user => user.age >= 30);
console.log(hasAdult); // false
```

<hr/>

**every ()：数组「判断是否全部满足」→ 返回布尔值**

- 作用：判断数组中「**所有元素**」是否都满足回调条件，有一个不满足就停止遍历；
- 语法：**arr.every((item, index, arr) => { 条件判断 })**
- **返回布尔值**
- 核心：「**全部满足**」才返回 true，否则 false

```js
const arr = [2, 4, 6];

// 是否全是偶数
const allEven = arr.every(item => item % 2 === 0);
console.log(allEven); // true

// 是否所有用户年龄≥18
const users = [{ age: 18 }, { age: 20 }, { age: 17 }];
const allAdult = users.every(user => user.age >= 18);
console.log(allAdult); // false
```

<hr/>

**reduce ()：数组「归并聚合」→ 返回任意类型结果**

- 作用：遍历数组，将每个元素和上一次的结果「**累加 / 聚合**」，返回最终聚合值（万能方法，可替代 map/filter/some 等）；
- 语法：**arr.reduce((累计值, 当前项, 索引, 原数组) => { 聚合逻辑 }, 初始值)**
- **返回最终聚合值**
- 核心：<u>灵活度最高，适合求和、求最大值、数组转对象、扁平数组等场景</u>

```js
// 场景1：数组求和（最基础）
const arr = [1, 2, 3, 4];
const sum = arr.reduce((total, item) => total + item, 0);
console.log(sum); // 10

// 场景2：数组转对象（开发高频）
const users = [{ id: 1, name: "张三" }, { id: 2, name: "李四" }];
const userObj = users.reduce((obj, user) => {
  obj[user.id] = user; // 以id为键，用户对象为值
  return obj;
}, {});
console.log(userObj); // { 1: {id:1,name:"张三"}, 2: {id:2,name:"李四"} }

// 场景3：求数组最大值
const max = arr.reduce((prev, curr) => prev > curr ? prev : curr, arr[0]);
console.log(max); // 4
```

<hr/>

**keys ()/values ()：数组「遍历键 / 值」→ 返回迭代器**

- 作用：
   - **keys() 返回数组索引的迭代器**
   - **values() 返回数组元素的迭代器**
- 语法：**arr.keys() / arr.values()**；
- 核心：**需配合 for...of 遍历**，或转数组（Array.from()）

```js
const arr = ["a", "b", "c"];

// keys()：遍历索引
const keyIter = arr.keys();
for (const key of keyIter) {
  console.log(key); // 0, 1, 2
}

// values()：遍历元素
const valIter = arr.values();
for (const val of valIter) {
  console.log(val); // a, b, c
}

// 转数组
console.log(Array.from(arr.keys())); // [0,1,2]
console.log(Array.from(arr.values())); // ["a","b","c"]
```

<hr/>

**flat ()：数组「扁平化」→ 返回新数组**

- 作用：将多维数组 **拍平** 为一维 / 指定维度的数组，**不修改原数组**
- 语法：**arr.flat(深度)**（深度默认 1，Infinity 表示拍平所有维度）；
- 核心：**处理嵌套数组**（比如接口返回的多维数据）

```js
// 二维数组拍平（默认深度1）
const arr1 = [1, [2, 3], [4, [5]]];
const flat1 = arr1.flat();
console.log(flat1); // [1,2,3,4,[5]]

// 拍平所有维度
const flat2 = arr1.flat(Infinity);
console.log(flat2); // [1,2,3,4,5]

// 空元素会被移除
const arr2 = [1, , 3, [4, , 6]];
console.log(arr2.flat()); // [1,3,4,6]
```