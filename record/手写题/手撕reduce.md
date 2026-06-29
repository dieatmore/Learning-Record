## 手撕reduce

```javascript
Array.prototype.myReduce = function (callback, initialValue) {
  const array = this;
  let acc;
  let startIndex = 0;

  // 有初始值
  if (arguments.length >= 2) {
    acc = initialValue;
  } else {
    // 无初始值，取第一项
    acc = array[0];
    startIndex = 1;
  }

  // 遍历执行
  for (let i = startIndex; i < array.length; i++) {
    acc = callback(acc, array[i], i, array);
  }

  return acc;
};
```

test
```javascript
// 测试1：求和
const arr = [1, 2, 3, 4];
console.log(arr.myReduce((a, b) => a + b, 0)); // 10

// 测试2：扁平化
const arr2 = [[1], [2], [3]];
console.log(arr2.myReduce((a, b) => a.concat(b), [])); // [1,2,3]

// 测试3：统计次数
const arr3 = ['a', 'b', 'a'];
console.log(arr3.myReduce((map, item) => {
  map[item] = (map[item] || 0) + 1;
  return map;
}, {}));
```


