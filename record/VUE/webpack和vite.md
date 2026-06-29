## webpack/vite

> Webpack：支持 CommonJS、AMD、ESM、assets 全能处理
Vite：只原生支持 ESM，CommonJS 需要转译

#### 缓存策略
- Webpack：缓存依赖,减少打包时间，提升二次构建速度
- Vite：
第三方库 强缓存
源码 协商缓存
热更新时几乎无开销

#### Webpack 开发环境冷启动流程
启动时，解析所有依赖
把所有模块编译、打包成一个或多个 bundle 文件
dev server 接管所有bundle文件
浏览器加载 bundle 文件，运行应用
特点：
冷启动速度 和项目规模强相关
项目越大，启动越慢
优点：对模块控制力极强，生态成熟

#### Webpack 热更新（HMR）
找到变化的模块
重新编译这个模块及其相关依赖并生成 描述信息文件 和 update 文件
dev server 通过 WebSocket 通知浏览器更新
浏览器请求下载 描述信息文件（包含更新信息、哈希） 以及 update 补丁文件 
之后替换旧模块实现热更新

#### Vite 开发环境冷启动流程
启动 dev server 以及 记录依赖关系生成 ModuleGraph，不打包、不编译
但是 esbuild 会做第三方依赖预构建
浏览器直接请求入口处的 原生 ES 模块（import/export）
执行时遇到 import 时，按需去请求加载对应的文件
Vite 实时编译该文件，结果返回给浏览器进行渲染
特点：
冷启动速度 几乎和项目规模无关
启动极快，改代码热更新也极快
利用浏览器原生 ES 模块，省去全量打包步骤

#### Vite 热更新
只编译 修改文件
dev server 通过 WebSocket 通知浏览器哪个文件更新了
浏览器请求下载 修改文件
替换旧组件实现热更新

<hr/>

#### Esbuild
开发环境（主要）：
   - **第三方库预构建**：
把 vue/react/lodash 等 CommonJS 转 ES模块，并且合并小模块
   - **快速转译**(Vue速度快核心)：
浏览器请求 .ts/.vue 等文件时，esbuild 快速转译为 JS

生产环境（辅助）：
- esbuild 不再负责打包，只做 代码转译 和 代码压缩

<hr/>

#### Rollup

Rollup 是一个专门 **用于生产环境的 JS 模块打包器**，主打：打包干净、体积小、Tree-shaking 强

- **Tree-shaking(主要)**
  `import { debounce } from 'lodash-es'`
  **只保留 debounce 相关代码,其他没用的全部删掉**

- 打包时会自动代码切割和chunk切分

<hr/>

- Rollup 为什么不适合开发环境？

     <br/>  
      因为它必须全量打包，和 Webpack 一样：
      启动要遍历所有依赖
      要打包完整 bundle
      热更新慢
      项目越大越卡