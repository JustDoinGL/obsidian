---
tags:
  - javascript
links:
  - https://walborn.notion.site/Promise-2e7695ce722e493ebf434552d01d5ba2
---
# История

Промисы появились с выходом ES6 - в 2015 году. Но пользоваться ими стали ещё раньше, потому что существовали специальные библиотеки с полифиллами.

Да вы и сами можете написать свою реализацию, если захотите, это не так сложно. К тому же реализацию промиса или полифилла его метода очень любят спрашивать на собесах. Например, реализуйте свой `Promise.all`
# Определение

`Promise` — специальный объект, ожидающий какого-то условия. Как только промис создан, он находится в состоянии ожидания `pending`, после чего он может выполниться успешно - состояние `fulfilled`, или неуспешно - `rejected`

- **pending** - ожидание
- **fulfilled** - исполнено успешно
- **rejected** - исполнено с ошибкой

## Жизненный пример

В жизни нам часто приходится иметь дело с промисами. 

Помолвка - кольцо, библиотека - читательский билет, поездка - билет на самолёт, мобилизация - повестка (часто заканчивается режектом)

Если вы были в макдаке и заказывали через терминал, то знаете как это происходит

1. `pending` Вы выбираете “Картофель фри по деревенски и Капучино биг”, оплачиваете 169 тугриков и получаете чек с номером 33. Поздравляю! 🥳 Вы только что создали промис и находитесь в состоянии `pending`. Можете выбрать куда сесть и полистать ленту телеграмма.
2. `fulfilled` Через некоторое время ты слышишь “Заказ 33 готов!”. И бежишь к стойке, так как изрядно проголодался. Заказ выполнился успешно, `fulfilled!`
3. `then` Можно идти за столик, откушать кофий со свободным картофелем. Но!
4. `rejected` Возможен и другой исход. Кассир с горечью сообщает: “У нас картошка закончилась, последнюю съел вон тот толстый мальчик в кепке, а кофемашина вообще сломалась ещё вчера”. О чём вы думаете в этот момент? О том, чтоб вернули деньги и дали хоть что-нибудь взамен. Запрос в состоянии `rejected` .
5. `catch` Вы можете отловить этот момент и попробовать сходить в `kfc` или помереть голодной смертью.
6. `finally` В любом случае после всего вы пойдёте домой смотреть ютубчик

## В коде
```js
const promise = new Promise((resolve, reject) => {
  // Здесь можно выполнять любые действия
  // вызов resolve(result) переведёт промис в состояние fulfilled
  // вызов reject(error) переведёт промис в состояние rejected
})

// Можно создать сразу «готовый» промис
const fulfilled = Promise.resolve(result)
// const fulfilled = new Promise((resolve) => resolve(result))
const rejected = Promise.reject(error)
// const rejected = new Promise((_, reject) => reject(error))

const promise = new Promise( ... )

// Можно навесить их одновременно
promise.then(onFulfilled, onRejected)

// Можно по отдельности
// Только обработчик onFulfilled
promise.then(onFulfilled)
// Только обработчик onRejected
promise.then(null, onRejected)
promise.catch(onRejected)
```
## Полифиллы

[[Promise implementation]]

#### Promise.all

```js
const promiseAll = (promises) => {
  const results = []
  
  // to track how many promises have left
  let left = promises.length

  return new Promise((resolve, reject) => {
    promises.forEach((promise, index) => {
      // if promise passes
      promise
        .then((result) => {
          // store its outcome and increment the count 
          results[index] = result
          
          // if all the promises are completed, 
          // resolve and return the result
          if (--left) resolve(results)
        })
        .catch(reject) // if any promise fails, reject
    })
  })
}
```

### Promise.allSettled
```js
const promiseAllSettled = (promises) => {
  const settle = (promise) => promise
    .then((value) => ({ status: 'fulfilled', value }))
    .catch((reason) => ({ status: 'rejected', reason }))
  
  return Promise.all(promises.map(settle))
}

// или
const promiseAllSettled = (promises) => {
  const results = []
  
  // to track how many promises have left
  let left = promises.length

  return new Promise((resolve) => {
    promises.forEach((promise, index) => {
      promise
        .then((result) => {
          results[index] = ({ status: 'fulfilled', value }))
        })
        .catch((reason) => {
					results[index] = ({ status: 'rejected', reason }))
				})
				.finally(() => {
					// if all the promises are completed, 
          // resolve and return the result
          if (--left) resolve(results)
		})
    })
  })
}
```
#### Promise.finally
```js
Promise.prototype.finally = function (callback) {
  const resolve = (res) => Promise.resolve(callback()).then(() => res)
  const reject = (e) => Promise.resolve(callback()).then(() => { throw e })
  return this.then(resolve, reject)
}
```

### Promise.trying
```js
const trying = (fetch, cnt, delay = 200) => {
  let p = fetch()
  for (let i = 0; i < cnt; i++) {
    p = p.catch(() => new Promise((resolve, reject) =>
      setTimeout(() => {
        console.log(`trying ${i}`)
        fetch().then(resolve).catch(reject)
      }, delay),
    ),
    )
  }
  return p
}
```

### Promise.delay
```js
Promise.prototype.delay = (timeout) => new Promise((resolve) => setTimeout(resolve, timeout))
```

## Задачи
```js
console.log(1)

setTimeout(() => console.log(2))

Promise.reject(3).catch(console.log)

new Promise(resolve => setTimeout(resolve))
	.then(() => console.log(4))

Promise.resolve(5).then(console.log)

console.log(6)

setTimeout(() => console.log(7), 0)
```

```js
/*
1 6 3 5 2 4 7 
*/
1 _ _ _ _ 6 _ // call stack
_ _ 3 s 5 _ _ // microtasks
_ 2 _ 4 _ _ 7 // macrotasks

// заметьте, что setTimeout сразу создался
// и был добавлен в очередь макртозадач
// до последнего setTimeout'a
```

```js
const myPromise = (delay) => new Promise((rs) => setTimeout(rs, delay))
setTimeout(() => console.log('1 in setTimeout1'), 1000)
myPromise(1000).then(res => console.log('2 in Promise 1'))
setTimeout(() => console.log('3 in setTimeout2'), 100);
myPromise(2000).then(res => console.log('4 in Promise 2'))
setTimeout(() => console.log('5 in setTimeout3'), 2000)
myPromise(1000).then(res => console.log('6 in Promise 3'))
setTimeout(() => console.log('7 in setTimeout4'), 1000);
myPromise(5000).then(res => console.log('8 in Promise '))
```

```js
1000 1000
100  2000
2000 1000
1000 5000

s2 s1 p1 p3 s4 p2 s3 p4
```