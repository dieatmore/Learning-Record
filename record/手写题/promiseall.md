### Promise.all

```js
const myAll = function (promises) {
  return new Promise((resolve, reject) => {
    if (promises === null || typeof promises[Symbol.iterator] !== 'function') {
      return reject(new TypeError('argument is not iterable.'))
    }

    const results = []
    const pArray = Array.from(promises)
    if (pArray.length === 0) {
      return resolve(results)
    }
    let count = 0
    pArray.forEach((pro, index) => {
      Promise.resolve(pro)
        .then((r) => {
          results[index] = r
          count++
          if (count === promises.length) {
            resolve(results)
          }
        })
        .catch(reject)
    })
  })
}
```