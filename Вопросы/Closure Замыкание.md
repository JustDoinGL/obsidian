---
tags:
  - "#javascript"
---
**Замыкание** — это механизм, возможный только благодаря тому, что функция ==знает своё лексическое окружение==, в котором она была объявлена. 

И через это окружение она способна получать доступ и даже изменять внешние переменные


В коде это будет выглядеть так:

```js
const outer = () => {
	let value = 'Yay'
	const inner = () => {
		value = 'Ooh'
	}
	const print = () => console.log(value)
	return { inner, print }
}

cosnt { inner, print } = outer()
print() // Yay
inner()
print() // Ooh
```

## Пример. Фабрика счётчиков.
Создадим функцию - фабрику счётчиков, внутри которой объявили переменную `let count = 0` . Фабрика на то и фабрика, что возвращает другие функции, и эти функции могут что-то делать с `count`, который у них внутри не объявлен
```js
function makeCounter() {
  let count = 0
  return () => ++counter
}

// или const counter = (count = 0) => () => ++count

const counterOne = makeCounter()
const counterTwo = makeCounter()
console.log(counterOne()) // 1
console.log(counterOne()) // 2

console.log(counterTwo()) // 1
console.log(counterTwo()) // 2
```
## Лексическое окружение

Можно называть замыканием **функцию с внешними переменными**, но это совсем не говорит, как вообще такое возможно!

Когда интерпретатор видит переменную, то сначала пытается найти ее в текущем [[Функции Function#Cтадии создания контекста выполнения функции|LexicalEnvironment]], но если ничего не нашёл, то идёт выше, во внешнее лексическое окружение, если и там не нашёл, то ещё выше, и ещё выше, пока не дойдёт до глобального окружения `window`

Таким образом `замыкание` - это связь функции с лексическим окружением, в котором она была объявлена, через которое она дотягивается до внешних переменных

То есть заметили, что мы можем двигаться только вверх по дереву, от ребёнка к родителю. Посмотреть, а какие же переменные находятся внутри вложенной функции, то есть ребёнке, или в соседе, мы не можем.

```js
function canGetCount(n) {
  let counter = n
  return () => (counter-- > 0 ? 'yes' : 'no')
}

// или
const canGetCount = (n) => () => n-- > 0 ? 'yes' : 'no'

const getOne = canGetCount(2)

```

```js
let name = 'John'

function sayHi() {
  console.log(`Hi, ${name}`)
}

name = 'Pete'

sayHi() // что будет показано: John или Pete?
```

## АХУЕТЬ С подвохом
```js
var a = 5

(function() {
  console.log(a)
})()
```

## sum(a)(b)(c)
Написать функцию sum чтобы выражение sum(1)(2)(5)(10) возвращало 18. sum(1)(2)(5)(10) = 18

```js
/**
 Храним результат в переменной замыкания `r`
 Функция `fn` не вызывает сама себя, а возвращает ссылку на себя
 Функция sum срабатывает только один раз. Она возвращает функцию fn
*/

const sum = (x) => {
  let r = x
  const fn = (y) => { r += y; return fn}
  fn.toString = fn.valueOf = () => r
  return fn
}

// usage
console.log(+sum(1)(2)(5)(10)) // 18
```
## Make Donkey
1. Будет ли работать доступ к переменной `name` через замыкание в примере ниже?
2. Удалится ли переменная `name` из памяти при выполнении `delete donkey.sayHi` ? Если нет — можно ли к `name` как-то обратиться после удаления `donkey.sayHi` ?
3. А если присвоить `donkey.sayHi = donkey.yell = null` — останется ли name в памяти?
```js
const makeDonkey = () => {
 const name = 'Ослик Иа'

 return {
   sayHi() {
     console.log(name)
   },
   yell() {
     console.log('И-а, и-а!')
   }
 }
}
const donkey = makeDonkey()
donkey.sayHi()

```
1. **Да** Будет работать благодаря ссылке `[[Scope]]` на внешний объект переменных, которая будет присвоена функциям `sayHi` и `yell` при создании объекта.
2. **Нет** `name` не удалится из памяти, поскольку несмотря на то, что `sayHi` больше нет, есть ещё функция `yell` , которая также ссылается на внешний объект переменных. Этот объект хранится целиком, вместе со всеми свойствами. При этом, так как функция `sayHi` удалена из объекта и ссылок на нее нет, то больше к переменной `name` обращаться некому. Получилось, что она «застряла» в памяти, хотя, по сути, никому не нужна.
3. Если и `sayHi` и `yell` удалить, тогда, так как больше внутренних функций не останется, удалится и объект переменных вместе с `name`
## Restore encapsulation
```js
function createStack() {
  return {
    items: [],
    push(item) {
      this.items.push(item);
    },
    pop() {
      return this.items.pop();
    }
  }
}

const stack = createStack()
stack.push(10)
stack.push(5)
stack.pop() // 5

stack.items // [10]
stack.items = [10, 100, 1000] // Encapsulation broken!
```

```js
const createStack = () => {
	const items = []
  return {
    push: item => items.push(item),
    pop: () => items.pop(),
		get: () => structuredClone(items),
  }
}

const stack = createStack()
stack.push(10)
stack.push(5)
stack.pop() // 5

stack.get() // [10]
stack.items = [10, 100, 1000] // Encapsulation broken!
```
## Revolut
```js
// Q: What is "closure"?

for (var i = 0; i < 5; i++) {
  setTimeout(() => {
    // Q: What would be the output and why?
    console.log('[Closure]', i + i + '' + i)
  }, i * 10)
}
```
[Closure] 105

Починить можно разными способами, например, обернуть в замыкание и сразу вызвать его

```js
for (var i = 0; i < 5; i++) {
  ((i) => setTimeout(() => {
    // Q: What would be the output and why?
    console.log('[Closure]', i + i + '' + i)
  }, i * 10))(i)
}

```