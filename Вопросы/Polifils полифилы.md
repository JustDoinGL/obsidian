---
tags:
  - javascript
  - codetask
---
[[Promise implementation]]
[[Promise#Полифиллы]]
[[Как работают bind, call и apply#Polyfill]]

## map
```js
Array.prototype.pMap = function(cb, context) {
  cb = cb.bind(context)
  r = []
  for (let i = 0; i < this.length; i++) {
    r.push(cb(this[i], i, this))
  }
  return r
}
```
## indexOf
```js
if (typeof Array.prototype.indexOf !== 'function') {
  Array.prototype.indexOf = function (item) {
    for (let i = 0; i < this.length; i++)
      if (this[i] === item) return i
    return -1
  }
}
```

## forEachRight
```js
Array.prototype.forEachRight = function (callback) {
  for (let i = this.length - 1; i > -1; i--)
    callback.call(this, this[i], i, this)
  return this
}
```