---
tags:
  - javascript
links:
  - https://learn.javascript.ru/call-apply-decorators
  - https://learn.javascript.ru/bind
---
## В чем разница между  `Function.prototype.call` и `Function.prototype.apply`?

Оба метода вызывают исходный метод с подменённым контекстом, но `.call` принимает параметры через запятую, а `.apply` - массивом.

`.bind` возвращает новую функцию с контекстом, который передали первым параметром. 
Перепривязать контекст уже нельзя будет
```js
fn.bind(user).bind(admin) // будет все равно user
```

## bind

Метод `bind` возвращает «привязанный вариант» функции `func`, фиксируя контекст `this` и первые аргументы `arg1`, `arg2`…, если они заданы.

Обычно `bind` применяется для фиксации `this` в методе объекта, чтобы передать его в качестве колбэка. Например, для `setTimeout`.

Когда мы привязываем аргументы, такая функция называется [[#Частичное применение|«частично применённой»]] или «частичной».

Частичное применение удобно, когда мы не хотим повторять один и тот же аргумент много раз. Например, если у нас есть функция `send(from, to)` и `from` всё время будет одинаков для нашей задачи, то мы можем создать частично применённую функцию и дальше работать с ней.
### Частичное применение

Мы можем привязать не только `this`, но и аргументы. Это делается редко, но иногда может быть полезно.

```javascript
function mul(a, b) {
	return a * b; 
}  

let triple = mul.bind(null, 3);

alert( triple(3) ); // = mul(3, 3) = 9 
alert( triple(4) ); // = mul(3, 4) = 12 
alert( triple(5) ); // = mul(3, 5) = 15
```
### Частичное применение без контекста

Что если мы хотим зафиксировать некоторые аргументы, но не контекст `this`? Например, для метода объекта.

Встроенный `bind` не позволяет этого. Мы не можем просто опустить контекст и перейти к аргументам.

К счастью, легко создать вспомогательную функцию `partial`, которая привязывает только аргументы.

```js
function partial(func, ...argsBound) {
	return function(...args) { // (*)     
		return func.call(this, ...argsBound, ...args);   
	} 
}  
// использование: 
let user = {   
	firstName: "John",   
	say(time, phrase) {
		alert(`[${time}] ${this.firstName}: ${phrase}!`);   
	} 
};  

// добавляем частично применённый метод с фиксированным временем
user.sayNow = partial(
	user.say, 
	new Date().getHours() + ':' + new Date().getMinutes()
);
	
user.sayNow("Hello"); 
// Что-то вроде этого: 
// [10:00] John: Hello!
```
### Polyfill
```js
// Please write your own "bind" implementation (polyfill): "fbind"

Function.prototype.fbind = function (ctx, ...bindArgs) {
  // Accepting arguments passed to newFunc
  return (...args) => this.apply(ctx, [ ...bindArgs, ...args ])
}

// Usage
const fn = function (id, city) {
  console.log(`${this.name}, ${id}, ${city}`) // id will be undefined
}

const context = { name: 'Jack' }
const bindedFn = fn.fbind(context, 'a_random_id')
bindedFn('New York') //
```
## call
[[Декораторы. Прозрачное кеширование|Кеширующий декоратор]] не подходит для работы с методами объектов.

В нашем случае мы можем использовать `call` в обёртке для передачи контекста в исходную функцию:
```js
let worker = {   
	someMethod() {     
		return 1;   
	},    
	slow(x) {     
		alert("Called with " + x);     
		return x * this.someMethod(); // (*)   
	} 
};  

function cachingDecorator(func) {   
	let cache = new Map();   
	
	return function(x) {     
		if (cache.has(x)) {       
			return cache.get(x);     
		}     
		let result = func.call(this, x); // теперь 'this' передаётся правильно     
		cache.set(x, result);     
		return result;   
	}; 
}  

worker.slow = cachingDecorator(worker.slow); // теперь сделаем её кеширующей  
alert( worker.slow(2) ); // работает 
alert( worker.slow(2) ); // работает, не вызывая первоначальную функцию (кешируется)
```

## apply

Вместо `func.call(this, ...arguments)` мы можем написать `func.apply(this, arguments)`.

Единственная разница в синтаксисе между `call` и `apply` состоит в том, что `call` ожидает список аргументов, в то время как `apply` принимает псевдомассив.

Вот более мощный `cachingDecorator`:

```js
let worker = { 
	slow(min, max) {     
		alert(`Called with ${min},${max}`);     
		return min + max;   
	} 
};  

function cachingDecorator(func, hash) {   
	let cache = new Map();   
	return function() {     
		let key = hash(arguments); // (*)     
		if (cache.has(key)) {       
			return cache.get(key);     
		}      
		let result = func.apply(this, arguments); // (**)     
		cache.set(key, result);     
		return result;  
	}; 
}  

function hash(args) {   
	return args[0] + ',' + args[1]; 
}  

worker.slow = cachingDecorator(worker.slow, hash);  
alert( worker.slow(3, 5) ); // работает 
alert( "Again " + worker.slow(3, 5) ); // аналогично (из кеша)
```

## Заимствование метода
Теперь давайте сделаем ещё одно небольшое улучшение функции хеширования:

```js
function hash() {   
	alert( [].join.call(arguments) ); // 1,2 
}  

hash(1, 2);
```

Этот трюк называется _заимствование метода_.

Мы берём (заимствуем) метод `join` из обычного массива `[].join`. И используем `[].join.call`, чтобы выполнить его в контексте `arguments`.

TODO: 
- debounce 
- throttle
