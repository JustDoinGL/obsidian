---
tags:
  - patterns
---
Добовление функцианольности в обьект или класс ==без использования механизмов наследования.== Примеси невозможно использовать сами по себе их цель это добовление функцианальности к обьектам или классам.

```js
class Dog {
  constructor(name) {
    this.name = name;
  }
}

const dogFunctionality = {
  bark: () => console.log("Woof!"),
  wagTail: () => console.log("Wagging my tail!"),
  play: () => console.log("Playing!")
};

Object.assign(Dog.prototype, dogFunctionality);

const pet1 = new Dog("Daisy");

pet1.name; // Daisy
pet1.bark(); // Woof!
pet1.play(); // Playing!
```