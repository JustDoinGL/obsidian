---
tags:
  - javascript
---
`this` - Ссылка на обьект в контексте которого вызвана функция.

не является фиксированным. Значение `this` вычисляется во время выполнения кода и зависит от контекста.

```js
let user = { name: "Джон" };
let admin = { name: "Админ" };

function sayHi() {
  alert( this.name );
}
// используем одну и ту же функцию в двух объектах
user.f = sayHi;
admin.f = sayHi;
// вызовы функции, приведённые ниже, имеют разное значение this
// "this" внутри функции является ссылкой на объект, который указан "перед точкой"
user.f(); // Джон  (this == user)
admin.f(); // Админ  (this == admin)
```

Свойство контекста выполнения кода (global, function или eval), которое в нестрогом режиме всегда является ссылкой на объект, а в строгом режиме может иметь любое значение.

### Global контекст

В глобальном контексте выполнения (за пределами каких-либо функций) `this` ссылается на глобальный объект вне зависимости от режима (строгий или нестрогий).

```js
// В браузерах, объект window также является объектом global:
console.log(this === window); // true

a = 37;
console.log(window.a); // 37

this.b = "MDN";
console.log(window.b)  // "MDN"
console.log(b)         // "MDN"
```

### Function контекст

#### Простой вызов

Поскольку следующий код не в [[‘use strict’|строгом режиме]], и значение `this` не устанавливается вызовом, по умолчанию будет использоваться объект `global`, которым в браузере является [window](https://developer.mozilla.org/ru/docs/Web/API/Window).

```js
function f1(){
  return this;
}

// В браузере:
f1() === window; // window - глобальный объект в браузере

// В Node:
f1() === global; // global - глобальный объект в Node
```


В строгом режиме, если значение `this` не установлено в контексте выполнения, оно остаётся `undefined`, как показано в следующем примере:

```js
function f2(){
  "use strict"; // см. strict mode
  return this;
}

f2() === undefined; // true
```

#### Стрелочные функции

В [стрелочных функциях](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Functions/Arrow_functions), `this` привязан к окружению, в котором была создана функция. В глобальной области видимости this будет указывать на глобальный объект.

```js
var globalObject = this;
var foo = (() => this);
console.log(foo() === globalObject); // true




var objReg = {
  hello: function() {
    return this;
  }
};
 
var objArrow = {
    hello: () => this
};
 
objReg.hello(); // возвращает, как и ожидается, объект objReg 
objArrow.hello(); // возвращает объект Window!
```

​
## This и вложенные объекты

`this` относиться к тому объекту, в методе которого оно используется. Рассмотрим пример. (смотрим на крайнюю правую точку) 

```js
var obj1 = {
  hello: function() {
    console.log('Hello world');
    return this;
  },
  obj2: {
      breed: 'dog',
      speak: function(){
            console.log('woof!');
            return this;
        }
    }
};

console.log(obj1);
console.log(obj1.hello());// выводит 'Hello world' и возвращает obj1console.log(obj1.obj2);
console.log(obj1.obj2.speak());// выводит 'woof!' и возвращает obj2
```
​

### C `new`

при использовании `new F()` сначала происходит создание контекста как при замыкании и поэтому this не теряется для стрелочных

```js
class Person {
    
    constructor(name) {
        this.name = name;
    }

  //c с методом не сработает
    greet = () => {
        console.log(`Hello, my name is ${this?.name}.`);
    };
//c с методом не сработает
  setName = (n) => {
    this.name = n;
  }
}

const p = new Person("Ron")
const greet = p.greet;
const setName = p.setName

// We don't want this to fail!
greet();
setName('Vera')
greet();

function Person2(name){
  this.name = name;
      this.greet = () => {
        console.log(`Hello, my name is ${this?.name}.`);
    };
}

const greet2 = new Person2("Bob").greet;
greet2()
```

### Привязка контекста к функции

1 Самый простой вариант решения – это обернуть вызов в анонимную функцию, создав замыкание

```js
setTimeout(function() { 
	user.sayHi(); // Привет, Вася! 
}, 1000)
```

привязать контекст с помощью [[Как работают bind, call и apply#bind|bind]]

```js
const a = {
		t: this, // ссылка на глобальный обьект см Global контекст
    n: 'name',
    fn() {
        console.log('fn', this) //ссылка на обьект a

        function fn2() {
            console.log('fn2', this) //ссылка на глобальный обьект/undefined так как вызвана не в контексте
        }
        fn2();
        const sf = () => console.log('sf', this); //ссылка на обьект a берет контекст выше
        sf();
    },
    sfg: () => console.log('sfg', this)//ссылка на глобальный обьект  берет контекст выше
}

a.fn();
a.sfg();
```