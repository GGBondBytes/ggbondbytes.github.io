---
title: debouncePromise
date: 2022-07-14 11:39:20
top: false
tags:
- React
---

```javascript
// 中断当前promise
function abortPromise(promise1) {
  let abort;
  const promise2 = new Promise((resolve, reject) => {
    abort = reject;
  });
  const p = Promise.race([promise1, promise2]);
  p.abort = abort;
  return p;
}
// promise防抖
export function debouncePromise(fn, time) {
  let promise;
  return (...rest) => {
    if (promise && typeof promise.abort === 'function') {
      promise.abort();
    }
    const timeoutPromise = new Promise(resolve => {
      setTimeout(() => {
        resolve(undefined);
      }, time);
    });
    promise = abortPromise(timeoutPromise);
    return promise.then(() => fn(...rest));
  };
}
```
