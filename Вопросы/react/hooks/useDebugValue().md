---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useDebugValue
---

`useDebugValue` is a React Hook that lets you add a label to a custom Hook in [React DevTools.](https://react.dev/learn/react-developer-tools)

```jsx
useDebugValue(value, format?)
```
## Reference

### `useDebugValue(value, format?)`

Call `useDebugValue` at the top level of your [custom Hook](https://react.dev/learn/reusing-logic-with-custom-hooks) to display a readable debug value:

#### Parameters

- `value`: The value you want to display in React DevTools. It can have any type.
- **optional** `format`: A formatting function. When the component is inspected, React DevTools will call the formatting function with the `value` as the argument, and then display the returned formatted value (which may have any type). If you don’t specify the formatting function, the original `value` itself will be displayed.

#### Returns

`useDebugValue` does not return anything.

## Usage

### Adding a label to a custom Hook
### Deferring formatting of a debug value [](https://react.dev/reference/react/useDebugValue#deferring-formatting-of-a-debug-value "Link for Deferring formatting of a debug value")

You can also pass a formatting function as the second argument to `useDebugValue`:

```jsx
useDebugValue(date, date => date.toDateString());
```

Your formatting function will receive the debug value as a parameter and should return a formatted display value. When your component is inspected, React DevTools will call this function and display its result.

This lets you avoid running potentially expensive formatting logic unless the component is actually inspected. For example, if `date` is a Date value, this avoids calling `toDateString()` on it for every render.