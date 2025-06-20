---
tags:
  - javascript
links:
  - https://walborn.notion.site/AbortController-db8e28449b2846b4a5d13fe0a71fd953
---

Как мы знаем, метод `fetch` возвращает промис. А в JavaScript в целом нет понятия «отмены» промиса. Как же прервать запрос `fetch`?

Для таких целей существует специальный встроенный объект: `AbortController`, который можно использовать для отмены не только `fetch`, но и других асинхронных задач.

Использовать его достаточно просто:
##  Шаг 1: создаём контроллер:
```js
let controller = new AbortController()
```

Контроллер `controller` – чрезвычайно простой объект.
- Он имеет единственный метод `abort()` и единственное свойство `signal`.
- При вызове `abort()`:
	- генерируется событие с именем `abort` на объекте `controller.signal`  
	- свойство `controller.signal.aborted` становится равным `true`.

Все, кто хочет узнать о вызове `abort()`, ставят обработчики на `controller.signal`, чтобы отслеживать его.
Вот так (пока без `fetch`):
```js
const controller = new AbortController() // срабатывает при вызове 
controller.abort() 
controller.signal.addEventListener('abort', () => alert('отмена!')) controller.abort() // отмена! 
alert(controller.signal.aborted) // true
```
## Шаг 2: передайте свойство `signal` опцией в метод `fetch`:
```js
    const controller = new AbortController()
    const url = '<https://jsonplaceholder.typicode.com/users>'
    fetch(url, { signal: controller.signal })
```
Метод `fetch` умеет работать с `AbortController`, он слушает событие `abort` на `signal`.    
## Шаг 3: чтобы прервать выполнение `fetch`, вызовите `controller.abort()`:
```js
controller.abort()
```
Вот и всё: `fetch` получает событие из `signal` и прерывает запрос.

Когда fetch отменяется, его промис завершается с ошибкой AbortError, поэтому мы должны обработать её, например, в try..catch:
```js
// прервать через 1 секунду 
let controller = new AbortController() 

setTimeout(() => controller.abort(), 1000) 

try { 
	let response = 
		await fetch('/article/fetch-abort/demo/hang', 
			{ signal: controller.signal }) 
} catch(err) { 
	if (err.name === 'AbortError') { // обработать ошибку от вызова abort() 
		alert('Прервано!') 
	} else { 
		throw err; 
	} 
}
```
**`AbortController` – масштабируемый, он позволяет отменить несколько вызовов `fetch` одновременно.**

Например, здесь мы запрашиваем много URL параллельно, и контроллер прерывает их все:

```jsx
const urls = [...] // список URL для параллельных fetch

const controller = new AbortController()

const fetchJobs = urls.map(url => fetch(url, {
  signal: controller.signal
}))

const results = await Promise.all(fetchJobs)

// если откуда-то вызвать controller.abort(),
// то это прервёт все вызовы fetch
```

Если у нас есть собственные асинхронные задачи, отличные от `fetch`, мы можем использовать один `AbortController` для их остановки вместе с `fetch`.

Нужно лишь слушать его событие `abort`:
```js
const urls = [ ... ]
const controller = new AbortController()

const proimiseJob = new Promise((resolve, reject) => { // наша задача
  ...
  controller.signal.addEventListener('abort', reject)
})

const fetchJobs = urls.map(url => fetch(url, { // запросы fetch
  signal: controller.signal
}))

// ожидать выполнения нашей задачи и всех запросов
let results = await Promise.all([ ...fetchJobs, proimiseJob ])

// вызов откуда-нибудь ещё:
// controller.abort() прервёт все вызовы fetch и наши задачи
```
Так что AbortController существует не только для fetch, это универсальный объект для отмены асинхронных задач, в fetch встроена интеграция с ним.
## В React

Представим ситуацию. Слабый интернет и пользователь никак не может дождаться загрузки всех постов. Он переходит на другую страницу, а запрос то уже запущен и тянет на себя так необходимые ресурсы.

А вот если бы разработчик поставил отмену запроса в useEffect, внутри функции, которую он возвращает из себя, то тогда запрос оборвался бы и лишняя работа не делалась! В маленьких проектах на это все закрывают глаза, потому что на производительности почти не сказывается. А вот когда пользователей миллион, то лишние запросы выливаются в приличные деньги (за энергию и сервера) и недовольных клиентов (увеличивается время ожидания страницы)

```jsx
useEffect(() => { 
	const abortController = new AbortController() 
	
	fetch( `https://jsonplaceholder.typicode.com/users?search=${search}`, 
		{ signal: abortController.signal } ) 
	.then(response => response.json()) 
	.then(setUsers) 
	
	return () => abortController.abort() 
}, [ search ])
```