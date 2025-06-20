---
tags:
  - javascript
  - object
links:
  - https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is
---
```js
console.log(Object.is('1', 1));
// Expected output: false

console.log(Object.is(NaN, NaN));
// Expected output: true

console.log(Object.is(-0, 0));
// Expected output: false

const obj = {};
console.log(Object.is(obj, {}));
// Expected output: false
```

## Description
- both [`undefined`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)
- both [`null`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/null)
- both `true` or both `false`
- both strings of the same length with the same characters in the same order
- both the same object (meaning both values reference the same object in memory)
- both [BigInts](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt) with the same numeric value
- both [symbols](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol) that reference the same symbol value
- both numbers and
    - both `+0`
    - both `-0`
    - both [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN)
    - or both non-zero, not [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN), and have the same value

`Object.is()` is not equivalent to the [`==`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Equality) operator. The `==` operator applies various coercions to both sides (if they are not the same type) before testing for equality (resulting in such behavior as `"" == false` being `true`), but `Object.is()` doesn't coerce either value.

`Object.is()` is also _not_ equivalent to the [`===`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) operator. The only difference between `Object.is()`and `===` is in their treatment of signed zeros and `NaN` values. The `===` operator (and the `==`operator) treats the number values `-0` and `+0` as equal, but treats [`NaN`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/NaN) as not equal to each other.

```js
// Case 1: Evaluation result is the same as using ===
Object.is(25, 25); // true
Object.is("foo", "foo"); // true
Object.is("foo", "bar"); // false
Object.is(null, null); // true
Object.is(undefined, undefined); // true
Object.is(window, window); // true
Object.is([], []); // false
const foo = { a: 1 };
const bar = { a: 1 };
const sameFoo = foo;
Object.is(foo, foo); // true
Object.is(foo, bar); // false
Object.is(foo, sameFoo); // true

// Case 2: Signed zero
Object.is(0, -0); // false
Object.is(+0, -0); // false
Object.is(-0, -0); // true

// Case 3: NaN
Object.is(NaN, 0 / 0); // true
Object.is(NaN, Number.NaN); // true

```