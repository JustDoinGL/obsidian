---
tags:
  - patterns
---
Compound components - это подход, в котором вы объединяете несколько компонентов одной общей сущностью и общим состоянием. 
Отдельно от этой сущности вы их использовать не можете, тк они являются единым целым.

Самый наглядный пример такого подхода, который знают все фронты - это select с его option в обычном HTML.

```html
<select name="meals"> 
	<option value="pizza">Pizza</option>
	<option value="pasta">Pasta</option> 
	<option value="borsch">Borsch</option> 
	<option value="fries">Fries</option> 
</select>
```

## React
```jsx
import React from "react";
import { FlyOut } from "./FlyOut";

// готовый компонент меню
export default function FlyoutMenu() {
  return (
    <FlyOut>// обертка
      <FlyOut.Toggle /> // кнопка переключателя
      <FlyOut.List> // список итемов
        <FlyOut.Item>Edit</FlyOut.Item>
        <FlyOut.Item>Delete</FlyOut.Item>
      </FlyOut.List>
    </FlyOut>
  );
}
```
### реализация Context API
```jsx
importReactfrom "react";
import Icon from "./Icon";

const FlyOutContext =React.createContext();

export function FlyOut(props) {
		// будет служить в качестве стата открытия закрытия меню
    const [open, toggle] =React.useState(false);

    return (
        <div className={`flyout`}>
            <FlyOutContext.Provider value={{ open, toggle }}> //создаем контекст в который будем передавать стат закрытия открытия меню
                {props.children}
            </FlyOutContext.Provider>
        </div>
    );
}

function Toggle() {
// читаем значения с контекста
    const { open, toggle } =React.useContext(FlyOutContext);

    return (
        <div className="flyout-btn" onClick={() => toggle(!open)}>
            <Icon />
        </div>
    );
}

function List({children}) {
    const { open } =React.useContext(FlyOutContext);
	// если меню открыто показываем список меню
    return open && <ul className="flyout-list">{children}</ul>;
}

function Item({children}) {
    return <li className="flyout-item">{children}</li>;
}

FlyOut.Toggle = Toggle;
FlyOut.List = List;
FlyOut.Item = Item;

```
### React.Children
`React.Children` предоставляет функции для работы с непрозрачной структурой данных `this.props.children`.

### `React.Children.map`

`React.Children.map(children, function[(thisArg)])`

Вызывает функцию для каждого непосредственного потомка, содержащегося в `children` передавая их по очереди в `thisArg`. Если `children` — это массив, он будет пройден, и функция будет вызвана для каждого потомка в массиве. Если `children` равен `null` или `undefined`, этот метод вернёт `null` или `undefined`, а не массив.

> Примечание
> 
> Если `children` — это `Fragment`, он будет рассматриваться как целый потомок, а элементы внутри не будут пройдены.

```jsx
import React from "react";
import Icon from "./Icon";

export function FlyOut(props) {
// будет служить в качестве стата открытия закрытия меню
  const [open, toggle] = React.useState(false);

  return (
    <div className={`flyout`}>
// для каждого непосредсвенного ребенка(Toggle,List) вызывает функцию React.cloneElement
      {React.Children.map(props.children, child =>
//клонирует элемент и возврощает новый с пропсами open, toggle
        React.cloneElement(child, { open, toggle })
      )}
    </div>
  );
}

function Toggle({ open, toggle }) {
  return (
    <div className="flyout-btn" onClick={() => toggle(!open)}>
      <Icon />
    </div>
  );
}

function List({ children, open }) {
  return open && <ul className="flyout-list">{children}</ul>;
}

function Item({ children }) {
  return <li className="flyout-item">{children}</li>;
}

FlyOut.Toggle = Toggle;
FlyOut.List = List;
FlyOut.Item = Item;
```