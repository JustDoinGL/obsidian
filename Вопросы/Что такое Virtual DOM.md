---
tags:
  - react
---

`Виртуальный DOM (VDOM)` — это концепция программирования, в которой идеальное или «виртуальное» представление пользовательского интерфейса хранится в памяти и синхронизируется с «настоящим» DOM при помощи библиотеки, такой как `ReactDOM`. Этот процесс называется [[Reconciliation]].

==Такой подход и делает API React **декларативным**: вы указываете, в каком состоянии должен находиться пользовательский интерфейс, а React добивается, чтобы DOM соответствовал этому состоянию.== 

Это абстрагирует манипуляции с атрибутами, обработку событий и ручное обновление DOM, которые в противном случае пришлось бы использовать при разработке приложения.

Поскольку «виртуальный DOM» — это скорее паттерн, чем конкретная технология, этим термином иногда обозначают разные понятия. 

В мире React «виртуальный DOM» обычно ассоциируется с [React-элементами](https://ru.reactjs.org/docs/rendering-elements.html) , поскольку они являются объектами, представляющими пользовательский интерфейс. 

Тем не менее, React также использует внутренние объекты, называемые «волокнами» (**fibers**), чтобы хранить дополнительную информацию о дереве компонентов. Их также можно считать частью реализации «виртуального DOM» в React.

	**Почему используется VDOM для изменения DOM а не DOM API?**

виртуальный DOM — это инструмент, который позволяет нам взаимодействовать с элементами DOM более простым и производительным способом. Это Javascript-объектное представление DOM, которое мы можем изменять так часто, как нам нужно. Изменения, внесенные в этот объект, затем сопоставляются, а изменения в реальном DOM производятся намного реже.

Виртуальный DOM может рассматриваться как копия исходного DOM. Этой копией можно часто манипулировать и обновлять, не используя API DOM. После того как все обновления были внесены в виртуальный DOM, мы можем посмотреть, какие конкретные изменения необходимо внести в исходный DOM, и сделать их целевым и оптимизированным способом.
