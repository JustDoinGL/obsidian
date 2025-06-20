---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useInsertionEffect
---
>`useInsertionEffect` is for CSS-in-JS library authors. Unless you are working on a CSS-in-JS library and need a place to inject the styles, you probably want [`useEffect`](https://react.dev/reference/react/useEffect) or [[useLayoutEffect()]] instead.

`useInsertionEffect` allows inserting elements into the DOM before any layout effects fire.

```jsx
useInsertionEffect(setup, dependencies?)
```

## Reference
 ### `useInsertionEffect(setup, dependencies?)` 

Call `useInsertionEffect` to insert styles before any effects fire that may need to read layout:

```js
import { useInsertionEffect } from 'react';
// Inside your CSS-in-JS library
function useCSS(rule) {  
	useInsertionEffect(() => {    
		// ... inject <style> tags here ...  
	});  
	return rule;
}
```
#### Parameters

- `setup`: The function with your Effect’s logic. Your setup function may also optionally return a _cleanup_ function. When your component is added to the DOM, but before any layout effects fire, React will run your setup function. After every re-render with changed dependencies, React will first run the cleanup function (if you provided it) with the old values, and then run your setup function with the new values. When your component is removed from the DOM, React will run your cleanup function.
    
- **optional** `dependencies`: The list of all reactive values referenced inside of the `setup` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is) comparison algorithm. If you don’t specify the dependencies at all, your Effect will re-run after every re-render of the component.
#### Returns

`useInsertionEffect` returns `undefined`.

#### Caveats [](https://react.dev/reference/react/useInsertionEffect#caveats "Link for Caveats")

- Effects only run on the client. They don’t run during server rendering.
- You can’t update state from inside `useInsertionEffect`.
- By the time `useInsertionEffect` runs, refs are not attached yet.
- `useInsertionEffect` may run either before or after the DOM has been updated. You shouldn’t rely on the DOM being updated at any particular time.
- Unlike other types of Effects, which fire cleanup for every Effect and then setup for every Effect, `useInsertionEffect` will fire both cleanup and setup one component at a time. This results in an “interleaving” of the cleanup and setup functions.