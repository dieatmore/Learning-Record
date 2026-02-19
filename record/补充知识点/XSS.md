## XSS 跨站脚本攻击

就是攻击者往你的网页里注入了可执行的 JavaScript，然后浏览器傻乎乎地执行了这段恶意代码

- 偷走用户的 cookie / token
- 冒充用户发请求、删数据、改资料
- 跳转到钓鱼网站
- 窃取输入的密码、手机号
- 监控用户所有操作
- 控制整个页面

#### 分三类

1. **存储型 XSS（最危险）**
恶意代码存到数据库 → 所有人访问都中招
评论、留言、昵称、简介 → 最容易出问题

2. **反射型 XSS**
恶意代码放在 URL 里 → 点链接就执行
常用于钓鱼

3. **DOM 型 XSS**
前端直接把不可信数据插入 DOM

<hr/>

#### 前端容易犯的 XSS 错误
你写代码时，只要出现下面这些，99% 有 XSS 风险：
innerHTML = 用户输入
outerHTML
document.write
setTimeout(用户输入)
eval(用户输入)
把用户数据放进 `<a href="javascript:…">`
把用户数据放进 onclick onload 等事件

<hr/>

#### 要点

- **永远不要直接用 innerHTML 插入用户数据** ——用 textContent 代替，它不会执行脚本。
- **现代框架（Vue/React）默认防 XSS**
因为它们默认用 textContent 逻辑，不会执行 HTML。
只有你主动用 v-html / dangerouslySetInnerHTML 才会有风险。