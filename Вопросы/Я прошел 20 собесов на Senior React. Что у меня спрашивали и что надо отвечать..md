---
tags:
  - javascript
  - react
links:
  - https://telegra.ph/YA-proshel-20-sobesov-na-Senior-React-CHto-u-menya-sprashivali-i-chto-nado-otvechat-12-10
---
## В useEffect есть колбек, колбек возвращает функцию. Что за функция и какие есть кейсы применения?

Речь идет о функции очистки. Ею, например, можно очистить таймер, чтобы не было утечки памяти. Вызывается перед следующим ререндером, если поменялись зависимости или перед демонтированием. [Документация.](https://react.dev/reference/react/useEffect#useeffect)

## Может ли быть асинхронная setup функция в useEffect? Почему? Как выполнить асинхронные действия внутри него?

Setup функция - первый параметр, что мы прокидываем в хук. То есть код, который вызываем при монтировании и обновлении зависимостей. Она не может быть асинхронной. Асинхронная функция возвращает промис. Колбек useEffect (setup) возвращает или функцию очистки, или ничего, но не промис. Чтобы выполнить асинхронные действия, можно сделать:

```jsx
useEffect(() => {
  // внутри создаем функцию
  const asyncCode = async () => {
    // делаем асинхронные и синхронные действия
    await someAsyncOperation();
    someSyncOperation();
    await anotherAsyncOperation();
  }
 
  // синхронно вызываем асинхронную фукнцию
  asyncCode(); 
})
```
## В чем отличие useEffect от useLayoutEffect? Можешь привести кейс использования useLayoutEffect?

`useEffect` вызывается **в какой-то момент** после того, как браузер все отрисовал. `useLayoutEffect` вызывается **строго перед тем**, как браузер все отрисовал. Кейс использования в [документации](https://react.dev/reference/react/useLayoutEffect).

## Зачем нужны useMemo и useCallback?

- useMemo - не подсчитывать тяжелое значение между ререндерами, либо сохранить ссылку объекта между ререндерами
- useCallback - сохранить ссылку на функцию между ререндерами

Например, у нас есть тяжелый компонент. Когда перерендеривается его родитель, перерендеривается и он. Чтобы избежать ререндеров, мы обернем тяжелый компонент в `memo()`, и обернем передаваемые в него пропсы-объекты (будь то функции, конфиги) в `useCallback` и `useMemo`, чтобы сохранить их ссылку. Таким образом мы избежим ререндеров при обновлении родителя. Почему это надо? Потому что `memo()` сравнивает примитивы через `===`, а объекты - по ссылке, и ссылка на созданные внутри компонента функции и объекты обновляется с каждым ререндером.

## Какие есть кейсы использования useRef?

1. Сохранить значение, которое не будет триггерить ререндеры при изменении. Например, мы хотим трекать, смонтировался ли компонент, и использовать это внутри `useEffect`. Если мы сохраним это в `useState`, нам придется добавить isMounted в его зависимости и мы получим бесконечные ререндеры. [https://www.emgoto.com/storing-values-with-useref/](https://www.emgoto.com/storing-values-with-useref/)

2. Получить доступ к элементу в DOM и управлять им

3. Использовать в связке с `useImperativeHandle`. Про него в следующем вопросе.
## Зачем нужен useImperativeHandle?

Допустим, есть такой код. `useImperativeHandle` позволяет получить доступ к коду другого компонента и императивно (руками)  им управлять. 

Мы создаем реф в `<Outer />`, и в `handleSomething` вызываем код из дочернего компонента `<Inner />`. 

Внутри самого `<Inner />` мы прописываем функцию в рефе `runCodeInChild`, которая модифицирует внутренний стейт. Изменение этого стейта вызывает `console.log` внутри этого же компонента. 

То есть, что мы сделали? 
Мы вызвали `setInnerState` компонента `<Inner />` из компонента `<Outer />` посредством `useRef` + `useImperativeHandle`.

```jsx
function Outer() {
	 const ref = useRef();

	  const handleSomething = () => {
	    ref.runCodeInChild();
	  }

  return <Inner ref={ref} />;
}

const Inner = forwardRef((props, ref) => {
	const [innerState, setInnerState] = useState(0);

	useEffect(() => {
		console.log('inner state changed: ', innerState)
	}, [innerState]);

  useImperativeHandle(ref, () => ({
    // ref.runCodeInChild будет доступен в компоненте <Outer /> 
    runCodeInChild: () => {
      setInnerState(prev => prev + 1);
    }
  }))

  return <div>{innerState}</div>
});
```
**Важное замечание, которые основывается на моем опыте проведения мок-собесов и знаниях учеников.**

Многие считают, что более редкие хуки можно пропустить. **Нет необязательных для знания хуков.** **Вы должны знать каждый хук.** Если вы забьете хоть на один из них, вы становитесь хуже как реакт-разработчик, так как часть задач, для которых придумали эти якобы редкие хуки, вы не сможете решить без говнокода. Поэтому мышление “да ну, useLayoutEffect это какой-то редкий непонятный хук, не буду тратить время” приведет вас к отказам на собесах.

## С точки зрения механизма рендера, в чем отличие классовых компонентов от функциональных?

- Когда монтируется классовый компонент, создается экземпляр. Когда он ререндерится, ссылка на объявленные внутри него методы вроде handleClick не меняется, так как они висят на экземпляре объекта. Поэтому в классовых компонентах нет своего useCallback.
- Когда монтируется функциональный компонент, вызывается, собственно, функция этого компонента, что вы написали. Когда он ререндерится, эта функция тоже вызывается. Когда мы пишем функциональный компонент, мы объявляем handleClick внутри в виде `const handleClick = () => {}`. Если код компонента вызывается заново на каждом ререндере, то все, что создается внутри функции, создается заново, и ссылка на этот самый handleClick будет меняться на каждом ререндере. Поэтому и придумали useCallback, который будет сохранять эту ссылку между ререндерами.
- Есть также и сходства: в классовых компонентах componentDidUpdate отрабатывает тогда же, когда в функциональных useLayoutEffect, то есть перед отрисовкой.
## Для чего нужен key в реакте

[[Reconciliation#Ключи]]

Для правильной работы [[Reconciliation]]. Если ключи не используются, по дефолту будут использовать индексы массива. Если используются индексы и мы поменяем элементы местами, например удалим какой-то из середины, то у части элементов поменяется индекс, и соответственно ключ. Когда меняется ключ, компонент демонтируется и монтируется заново. **НЕ ререндерится,** а демонтируется и монтируется заново.

## Что такое Reconciliation и как он работает?

[[Reconciliation]]
## **Какие изменения были в HTTP с течением времени?**

- 0.9: HTTP-запрос представлял из себя One-liner без хедеров и прочего
- 1.0: Появились хедеры и статус-коды
- 1.1-2.0: Появилась возможность отправлять сразу несколько запросов; content-type; сжатие хедеров; и т.д.
- Если интересно в деталях - [https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP/Basics_of_HTTP/Evolution_of_HTTP)

## **У тебя есть пагинированная таблица и поисковая строка по этой таблице. Твой поиск триггерит запрос на бек. Как будешь реализовывать логику поиска?**

- [[Debounce|Дебаунс]] или [[Throttling|троттл]]. Пользователь закончит ввод, запрос отправится через, условно, 500 мс после этого. Я замечал, что чаще всего на практике использовали кастомный хук [[Debounce#В React можно написать свой хук|useDebounce]]
## В каком случае понадобится использовать `POST` запрос вместо `GET`, даже если мы не хотим ничего менять на сервере?

Берем таблицу с предыдущего вопроса. 
Допустим, это таблица с поиском по сотрудникам. Мы вводим в поисковой строке ФИО и номер телефона. 
Что это? - Персональные данные. 
В чем проблема передавать их по `GET`? 
В `GET` нет тела запроса, которое шифруется по `HTTPS`, соответственно мы можем передать данные только внутри `URL`. В чем проблема передать в виде параметров `URL`, если он шифруется? 
Персональные данные можно будет увидеть в логах сервера. Если будет утечка, утекут и персональные данные. В `POST` же мы передадим их через тело запроса, которое при хороших практиках не будут держать в логах на сервере. Поэтому для обычного получения данных мы будем использовать `POST`, а не `GET`.

## У тебя есть какая-то таска. Как ты будешь подходить к её решению?
[[Рабочий процесс]]

## Какие есть способы оптимизации во фронте?
[[Performance patterns]]

## Если бьешь приложение на чанки, какие могут быть проблемы с кешированием и как их решить?

Допустим, наш чанк назывался feature.js. Мы внесли в него изменения, но браузер пользователя подгружает старый чанк. Почему? Потому что не поменялось название и он тянет его из кеша. Для решения проблемы используем **х**еш из состава файла. Получится что-то вроде feature.ta6f78gesgfh7es.js. После любых изменений хеш будет меняться, так как меняется состав файла, браузер будет думать, что это новый файл и не вытягивать его из **к**еша.


## Расскажи, что такое Core Web Vitals, они же веб-метрики, и как под них можно оптимизировать?
[[Core Web Vitals]]

## Зачем нужен CORS? Как он делается?

[[CORS]]

Чтобы не кинуть запрос с левого сайта, которому запросы кидать нельзя. Браузеры сами не позволяют это сделать, они смотрят на HTTP хедеры, которые это регламентируют.

## Расскажи про SOLID

[[SOLID]]
## Что такое семантические теги?

HTML теги вроде `<section>` и `<header>`. Нужны, чтобы скринридеры и поисковые движки нормально читали страницу. Т.е. для accessibility и SEO.

## В чем безопаснее хранить токен - в локалсторадже или куках? Почему?

В куках, потому что можно запретить доступ к куке из javascript. Таким образом, если кто-то проведет XSS атаку и сможет выполнить js-код у другого пользователя, он не сможет украсть у него токен, так как к куке нет доступа из JS.
## Как работают bind, call и apply
[[Как работают bind, call и apply]]

## Расскажи все, что знаешь про всплытие и погружение события
[[Всплытие и погружение]]

## Как работает замыкание в js? Приведи примеры
[[Closure Замыкание]]
  
## Расскажи про ивент луп

[[Microtasks]]
[[Event loop]]

  

## TODO: Расскажи про *что угодно* в typescript:

- Проходим весь https://learntypescript.dev, если не уверены в знаниях. Если уверены, но по ощущениям есть пробелы - проходим те темы, названия которых не понятны. В обоих случаях обязательно делаем упражнения.