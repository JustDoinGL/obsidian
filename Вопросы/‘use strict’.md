---
tags:
  - javascript
---
## Зачем нужен `use strict`?

### Строгий режим делает следующее:

- **Выбрасывает ошибки**, когда в коде используются некоторые небезопасные конструкции.
- Выключает функции языка, которые запутывают код и потому не должны использоваться.
- Предотвращает использование слов, которые могут быть использованы в качестве ключевых в будущем.

### Важные ограничения, которые накладывает включение строгого режима.

#### Нельзя использовать переменные без объявления
```jsx
'use strict'

const name = 'Anna'
console.log(name)
// Anna

age = 24
console.log(age)
// Uncaught ReferenceError: age is not defined
```
#### Явная ошибка если значение поля нельзя изменить или удалить

С помощью методов `Object.defineProperty()` или `Object.preventExtensions()` в JavaScript можно запретить перезаписывать поля [объекта](https://doka.guide/js/object/). При включённом строгом режиме попытка перезаписать поле приведёт к ошибке.

```jsx
'use strict'

const obj = {}

Object.defineProperty(obj, 'someProp', { value: 'Alex', writable:false })

console.log(obj.someProp)
// Alex

obj.someProp = 'James'
// Uncaught TypeError: Cannot assign to read only property 'someProp' of object #<Object>
```

Ошибка будет выброшена в строгом режиме и при попытке удаления поля из объекта, когда это сделать нельзя

```jsx
const obj = {}

Object.defineProperty(obj, 'someProp', {
	value: 'Anna',
	configurable: false
})

delete obj.someProp
// Uncaught TypeError: Cannot delete property 'someProp' of #<Object>
```
#### Параметры функции не могут иметь одинаковые имена

Если в [функции](https://doka.guide/js/function/) объявить два параметра с одинаковым именем, то строгий режим выбросит ошибку выполнения.
```jsx
'use strict'

// Uncaught SyntaxError: Duplicate parameter name not allowed in this context
function sum(a, b, a) {
// ...
}
```

Без `use strict` интерпретатор выполнит код без ошибок, но обратиться к переопределённому параметру будет невозможно.

```jsx
function sum(a, b, a) {
  console.log(a)
  console.log(b)
}

sum(1, 2, 3) // Выведет 3 и 2, первый аргумент потерян навсегда
```

#### Другое поведение [[this Context контекст]]    

При включённом строгом режиме [[this Context контекст]] больше не будет по умолчанию ссылаться на глобальный объект.

```jsx
'use strict'

function logThis() {
  console.log(this)
}

logThis()
// Выведет undefined
```

Без `use strict` если вызывать функцию в глобальном контексте (например в консоли браузера), то [[this Context контекст]] всегда будет ссылаться на глобальный объект.    
#### Запрещено использовать зарезервированные слова

В строгом в режиме запрещено использовать в коде некоторые слова, которые были специально зарезервированы для того, чтобы использовать их в будущем. Это слова `implements`, `interface`, `let`, `package`, `private`, `protected`, `public`, `static`, `yield`. Некоторые из этих слов уже используется в данный момент: например объявление переменных через `let` или возвращение значения из генератора с помощью `yield`.

#### Ограничение небезопасных конструкций

Дополнительно включение строгого режима не позволяет использовать в коде конструкцию `with` и очищает переменные, созданные с помощью `eval()`. Обе эти конструкции устарели и являются потенциально опасными, потому не используются в современном JavaScript.

```jsx
function foo() {
    const x = 10;

    return {
        x: 20,
        bar: () => {
            console.log(this.x);
        },
        baz: function () {
            console.log(this.x);
        },
    };
}

const obj1 = foo();
obj1.bar(); // undefined
obj1.baz(); // 20

const obj2 = foo.call({ x: 30 });
let x = obj2.bar;
let y = obj2.baz;
x(); // 30
y(); // 30

obj2.bar(); // 30
obj2.baz(); // 20

```