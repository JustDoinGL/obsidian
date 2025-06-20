---
tags:
  - react
  - hooks
links:
  - https://react.dev/reference/react/useLayoutEffect
---
>`useLayoutEffect` can hurt performance. Prefer [`useEffect`](https://react.dev/reference/react/useEffect) when possible.

```jsx
useLayoutEffect(setup, dependencies?)
```

## Reference
Вызовите `useLayoutEffect` для выполнения измерений макета перед тем, как браузер перерисует экран:
```jsx
useLayoutEffect(setup, dependencies?)
```
 
 #### Parameters:
 - `setup`: The function with your Effect’s logic. Your setup function may also optionally return a **_cleanup_** function. Before your component is added to the DOM, React will run your setup function. After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values. Before your component is removed from the DOM, React will run your cleanup function.
- **optional** `dependencies`: The list of all reactive values referenced inside of the `setup` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [[Object.is()]] comparison. If you omit this argument, your Effect will re-run after every re-render of the component.
### Returns
`useLayoutEffect` returns `undefined`.

## Usage
### Measuring layout before the browser repaints the screen
**All of this needs to happen before the browser repaints the screen.** You don’t want the user to see the tooltip moving. Call `useLayoutEffect` to perform the layout measurements before the browser repaints the screen:

Here’s how this works step by step:

1. `Tooltip` renders with the initial `tooltipHeight = 0` (so the tooltip may be wrongly positioned).
2. React places it in the DOM and runs the code in `useLayoutEffect`.
3. Your `useLayoutEffect` [measures the height](https://developer.mozilla.org/en-US/docs/Web/API/Element/getBoundingClientRect) of the tooltip content and triggers an immediate re-render.
4. `Tooltip` renders again with the real `tooltipHeight` (so the tooltip is correctly positioned).
5. React updates it in the DOM, and the browser finally displays the tooltip.