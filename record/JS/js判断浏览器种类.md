## js判断浏览器种类

### 思路：解析 User-Agent 字符串

**浏览器每次请求都会携带 User-Agent（UA）字符串，包含浏览器类型、版本、内核等信息，JS 中可通过**

**`navigator.userAgent`** 获取，核心是通过 **正则匹配 UA 中的特征关键词**

> 注意：UA 可被手动修改，因此判断结果 **并非 100% 绝对准确**，但能满足 99% 的常规场景

<hr/>

单独判断某类浏览器:

```js
// 1. 判断是否为 Chrome 浏览器
function isChrome() {
  const ua = navigator.userAgent.toLowerCase();
  return ua.includes('chrome/') && !ua.includes('edg/');
}

// 2. 判断是否为 Firefox 浏览器
function isFirefox() {
  return navigator.userAgent.toLowerCase().includes('firefox/');
}

// 3. 判断是否为 Safari 浏览器
function isSafari() {
  const ua = navigator.userAgent.toLowerCase();
  return ua.includes('safari/') && !ua.includes('chrome/');
}

// 4. 判断是否为 Edge 浏览器（Chromium 内核）
function isEdge() {
  return navigator.userAgent.toLowerCase().includes('edg/');
}

// 5. 判断是否为 IE 浏览器（兼容 IE11）
function isIE() {
  const ua = navigator.userAgent.toLowerCase();
  return ua.includes('msie') || ua.includes('trident/');
}

// 使用示例
if (isChrome()) {
  console.log('当前是 Chrome 浏览器');
} else if (isIE()) {
  console.log('当前是 IE 浏览器，建议升级');
}
```


<hr/>

完整的浏览器判断函数:
```js
/**
 * 判断浏览器类型和版本
 * @returns {Object} { name: 浏览器名称, version: 版本号 }
 */
function getBrowserInfo() {
  const ua = navigator.userAgent.toLowerCase();
  let name = 'unknown';
  let version = '0';

  // 匹配 Edge（Chromium 内核）
  if (ua.includes('edg/')) {
    name = 'Microsoft Edge';
    version = ua.match(/edg\/([\d.]+)/)[1];
  }
  // 匹配 Chrome（排除 Edge 干扰）
  else if (ua.includes('chrome/') && !ua.includes('edg/')) {
    name = 'Google Chrome';
    version = ua.match(/chrome\/([\d.]+)/)[1];
  }
  // 匹配 Firefox
  else if (ua.includes('firefox/')) {
    name = 'Mozilla Firefox';
    version = ua.match(/firefox\/([\d.]+)/)[1];
  }
  // 匹配 Safari（排除 Chrome 干扰，Chrome 含 safari 关键词）
  else if (ua.includes('safari/') && !ua.includes('chrome/')) {
    name = 'Apple Safari';
    version = ua.match(/version\/([\d.]+)/)[1];
  }
  // 匹配 IE（仅 IE11 及以下，Edge 已替代 IE）
  else if (ua.includes('msie') || ua.includes('trident/')) {
    name = 'Microsoft Internet Explorer';
    version = ua.match(/msie ([\d.]+)/)?.[1] || ua.match(/rv:([\d.]+)/)[1];
  }
  // 匹配 Opera
  else if (ua.includes('opr/')) {
    name = 'Opera';
    version = ua.match(/opr\/([\d.]+)/)[1];
  }

  return { name, version };
}

// 使用示例
const browser = getBrowserInfo();
console.log(`当前浏览器：${browser.name}，版本：${browser.version}`);
// 输出示例：当前浏览器：Google Chrome，版本：120.0.0.0
```
