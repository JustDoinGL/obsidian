---
tags:
  - typescript
---
```ts
// Есть функция как получить тип ее параметров
function test(a:string,b:number,c:boolean):void{
  console.log({a,b,c})
}

// вариант первый утилитарный тип
type T1 = Parameters<typeof test>;

// вариант второй
type MyParameters<T extends (...arg: any[]) => void> = T extends (...arg: infer P) => void ? P : never;

type T2 = MyParameters<typeof test>
```

## какая есть особенность у ключевого слова `infer` ?
Ключевое слово `infer` можно использовать только в условных типах.