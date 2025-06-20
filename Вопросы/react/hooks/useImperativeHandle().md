---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useImperativeHandle
---
`useImperativeHandle` is a React Hook that lets you customize the handle exposed as a [ref.](https://react.dev/learn/manipulating-the-dom-with-refs)

```
useImperativeHandle(ref, createHandle, dependencies?)
```

## Reference

### `useImperativeHandle(ref, createHandle, dependencies?)`

Call `useImperativeHandle` at the top level of your component to customize the ref handle it exposes:

```jsx
import { forwardRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {  
	useImperativeHandle(ref, () => {    
		return {      
			// ... your methods ...    
		};  
	}, []);  
	
	// ...
```

#### Parameters
- `ref`: The `ref` you received as the second argument from the [`forwardRef` render function.](https://react.dev/reference/react/forwardRef#render-function)
- `createHandle`: A function that takes no arguments and returns the ref handle you want to expose. That ref handle can have any type. Usually, you will return an object with the methods you want to expose.

- **optional** `dependencies`: The list of all reactive values referenced inside of the `createHandle` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison. If a re-render resulted in a change to some dependency, or if you omitted this argument, your `createHandle` function will re-execute, and the newly created handle will be assigned to the ref.
#### Returns

`useImperativeHandle` returns `undefined`.