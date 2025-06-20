---
tags:
  - react
links:
  - https://react.dev/reference/react/memo
---
`memo` lets you skip re-rendering a component when its props are unchanged

## Reference 
### `memo(Component, arePropsEqual?)`

Wrap a component in `memo` to get a _memoized_ version of that component. This memoized version of your component will usually not be re-rendered when its parent component is re-rendered as long as its props have not changed. But React may still re-render it: memoization is a performance optimization, not a guarantee.

```jsx
import { memo } from 'react';

const SomeComponent = memo(function SomeComponent(props) {  
	// ...
});
```
#### Parameters
- `Component`: The component that you want to memoize. The `memo` does not modify this component, but returns a new, memoized component instead. Any valid React component, including functions and [`forwardRef`](https://react.dev/reference/react/forwardRef) components, is accepted.
- **optional** `arePropsEqual`: A function that accepts two arguments: the component’s previous props, and its new props. It should return `true` if the old and new props are equal: that is, if the component will render the same output and behave in the same way with the new props as with the old. Otherwise it should return `false`. Usually, you will not specify this function. By default, React will compare each prop with [[Object.is()]]

#### Returns

`memo` returns a new React component. It behaves the same as the component provided to `memo` except that React will not always re-render it when its parent is being re-rendered unless its props have changed.