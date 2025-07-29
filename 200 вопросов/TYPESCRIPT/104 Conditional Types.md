**Условные типы (Conditional Types)** в TypeScript позволяют выбирать тип на основе условия. Они используют синтаксис:  

```typescript
T extends U ? X : Y
```  

**Пример:**  
```typescript
type IsString<T> = T extends string ? true : false;

type A = IsString<"hello">; // true
type B = IsString<number>;  // false
```  

**Ключевые моменты:**  
- Проверяет, соответствует ли `T` типу `U`.  
- Если да — возвращает `X`, иначе `Y`.  
- Часто используется с `infer` для вывода типов в ветках.  

Полезны для сложных типов, например, в утилитах (`ReturnType`, `Exclude`).