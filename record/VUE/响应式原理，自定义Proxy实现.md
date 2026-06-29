## 响应式原理，自定义Proxy实现

```js
// ------------------- 核心变量：存储依赖关系 -------------------
// 格式：target → key → Set<依赖函数>
// 比如：pestData → name → [updateView, logName]
const targetMap = new WeakMap();
// 当前正在执行的依赖函数（比如渲染视图的函数）
let activeEffect: Function | null = null;

// ------------------- 1. 依赖收集：track -------------------
/**
 * 读取数据时，收集依赖
 * @param target 被代理的对象（比如 pestData）
 * @param key 被读取的属性（比如 name）
 */
function track(target: object, key: string | symbol) {
  if (!activeEffect) return; // 没有正在执行的函数，无需收集

  // 1. 找 target 对应的依赖表（没有则创建）
  let depsMap = targetMap.get(target);
  if (!depsMap) {
    depsMap = new Map();
    targetMap.set(target, depsMap);
  }

  // 2. 找 key 对应的依赖集合（没有则创建）
  let deps = depsMap.get(key);
  if (!deps) {
    deps = new Set();
    depsMap.set(key, deps);
  }

  // 3. 将当前执行的函数加入依赖集合
  deps.add(activeEffect);
}

// ------------------- 2. 触发更新：trigger -------------------
/**
 * 修改数据时，触发依赖函数执行
 * @param target 被代理的对象
 * @param key 被修改的属性
 */
function trigger(target: object, key: string | symbol) {
  // 1. 找 target 对应的依赖表
  const depsMap = targetMap.get(target);
  if (!depsMap) return;

  // 2. 找 key 对应的所有依赖函数
  const deps = depsMap.get(key);
  if (!deps) return;

  // 3. 执行所有依赖函数
  deps.forEach((effect) => effect());
}

// ------------------- 3. 自定义 Proxy 实现响应式 -------------------
/**
 * 将对象转为响应式对象
 * @param target 原始对象
 * @returns 代理后的响应式对象
 */
function reactive<T extends object>(target: T): T {
  return new Proxy(target, {
    // 拦截属性读取（比如 obj.name、obj['name']）
    get(target, key, receiver) {
      const res = Reflect.get(target, key, receiver);
      // 读取时收集依赖
      track(target, key);
      return res;
    },

    // 拦截属性赋值（比如 obj.name = '新值'）
    set(target, key, value, receiver) {
      const oldValue = Reflect.get(target, key, receiver);
      const res = Reflect.set(target, key, value, receiver);
      // 只有值变化时，才触发更新
      if (oldValue !== value) {
        trigger(target, key);
      }
      return res;
    },

    // 可选：拦截删除属性（比如 delete obj.name）
    deleteProperty(target, key) {
      const res = Reflect.deleteProperty(target, key);
      trigger(target, key);
      return res;
    },
  });
}

// ------------------- 4. 注册依赖函数：effect -------------------
/**
 * 注册“依赖响应式数据的函数”，并立即执行一次
 * @param fn 依赖函数（比如更新视图的函数）
 */
function effect(fn: Function) {
  // 将 fn 设为当前活跃的依赖函数
  activeEffect = fn;
  // 立即执行一次，触发 get 拦截，收集依赖
  fn();
  // 执行完后重置
  activeEffect = null;
}

// ------------------- 测试：验证响应式 -------------------
// 1. 定义原始数据（你的病虫害数据）
const pestData = {
  name: '松材线虫',
  area: '江苏省',
  level: '高危',
};

// 2. 转为响应式对象
const reactivePest = reactive(pestData);

// 3. 注册依赖函数（模拟更新视图）
effect(() => {
  console.log(`【视图更新】病虫害名称：${reactivePest.name}，区域：${reactivePest.area}`);
});

// 4. 修改数据，触发响应式更新
console.log('===== 修改名称 =====');
reactivePest.name = '美国白蛾'; // 自动执行 effect 函数，打印新名称

console.log('===== 修改区域 =====');
reactivePest.area = '山东省'; // 再次触发 effect 函数

console.log('===== 删除属性 =====');
delete reactivePest.level; // 拦截删除操作（可选）
```