## ajax，原生fetch()

- AJAX：不是具体技术，是 **「异步请求数据、不刷新页面」** 的统称，原生实现靠 XMLHttpRequest（XHR）；
- fetch()：ES6+ 新增的原生请求 API，替代 XHR，基于 Promise，无封装（是 “原生底层”）；
- axios：你熟悉的第三方库，底层封装了 XHR，补充了超时、拦截器、自动解析 JSON 等功能，比原生更易用

<hr/>

**1. 原生 AJAX（XHR）- GET 请求（核心步骤）**

```js
// 1. 创建XHR对象
const xhr = new XMLHttpRequest();
// 2. 配置请求（方法、URL、异步）
xhr.open('GET', 'https://xxx.com/api', true);
// 3. 监听状态（核心：readyState=4 表示请求完成）
xhr.onreadystatechange = function() {
  if (xhr.readyState === 4) {
    if (xhr.status >= 200 && xhr.status < 300) {
      // 手动解析JSON
      const data = JSON.parse(xhr.responseText);
      console.log(data);
    } else {
      console.error('请求失败：', xhr.status);
    }
  }
};
// 4. 发送请求
xhr.send(null);
```

**2. 原生 fetch () - GET 请求**

```js
fetch('https://xxx.com/api')
  .then(res => {
    // 核心坑点：fetch不识别HTTP错误（404/500），需手动判断
    if (!res.ok) throw new Error(`HTTP错误：${res.status}`);
    // 手动解析JSON（fetch不会自动解析）
    return res.json();
  })
  .then(data => console.log(data))
  .catch(err => console.error(err));
```

**3. axios**

```js
// axios 帮你封装了「错误判断 + 自动解析JSON」，更简洁
axios.get('https://xxx.com/api')
  .then(res => console.log(res.data)) // 自动解析好的data
  .catch(err => console.error(err)); // 404/500都会自动catch
```

<hr/>

|特性	|原生 XHR（AJAX）|	原生 fetch ()	|axios|
|:--:|:--:|:--:|:--:|
|错误处理	|404/500 触发错误|	仅网络错（断网）reject，HTTP 错需手动判断|	自动捕获所有错误（HTTP + 网络）|
|JSON 解析|	手动 JSON.parse()	|手动 res.json()	|自动解析（res.data）|
|超时处理	|原生支持 xhr.timeout	|无原生超时，需手动封装|	内置 timeout 配置|
|拦截器	|无（需自己写）	|无（需自己写）|	内置请求 / 响应拦截器（核心优势）|
|Cookie 携带|	默认携带	|需加 credentials: 'include'	|默认携带（可配置）|