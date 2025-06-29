---
tags:
  - redux
  - redux-saga
---
## Какую проблему решает redux?
Управление состоянием (данными) всего вашего приложения.
## Что такое экшены и экшен креэйтеры?

Экшены (Action) — это структуры, которые передают данные из вашего приложения в стор. 
Они являются единственными источниками информации для стора. Вы отправляете их в стор, используя метод `store.dispatch(). useDispatch()`

**Генераторы экшенов** (Action Creators) — функции, которые создают экшены

### Есть ли хук позволяющий отправить экшен?

`const dispatch = useDispatch()`

## Redux-Saga 

### Чем отличается fork и call?
`fork`, как и `call`, может использоваться для вызова как обычных функций, так и функций генератора, но `fork` является не блокирующим генеретор не ждет выполнение `fork`.

### Для чего нужны саги?
==Саги позволяют писать сложный асинхронный код в синхронном стиле упростить работу с сайд ефектами==

Мы можем:
- приостановить выполнение воркера 
- закрыть его 
- форкнуть
дает больше гибкости для реализации сложной логики

Можно представить это так, что `saga` — это как отдельный поток в вашем приложении, который отвечает за работу с сайд эффектами. `redux-saga` — это мидлвар `redux`, что означает, что этот поток может запускаться, останавливаться и отменяться из основного приложения с помощью экшенов redux, оно имеет доступ к полному состоянию redux приложения и также может диспатчить действия redux.

Библиотека использует концепцию ES6, под названием генераторы, для того, чтобы сделать эти асинхронные потоки легкими для чтения, написания и тестирования. (если вы не знакомы с этим, здесь есть некоторые ссылки для ознакомления) Тем самым, эти асинхронные потоки выглядят, как ваш стандартный синхронный JavaScript-код. (наподобие async/await, но генераторы имеют несколько отличных возможностей, необходимых нам)