---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useRef
---
`useRef` is a React Hook that lets you reference a value that’s not needed for rendering.

```js
const ref = useRef(initialValue)
```

## Reference

### `useRef(initialValue)`

Call `useRef` at the top level of your component to declare a [ref.](https://react.dev/learn/referencing-values-with-refs)

```js
import { useRef } from 'react';

function MyComponent() {  
	const intervalRef = useRef(0);  
	const inputRef = useRef(null);  
	// ...
```


#### Parameters
 `initialValue`: The value you want the ref object’s `current` property to be initially. It can be a value of any type. This argument is ignored after the initial render.

#### Returns 

`useRef` returns an object with a single property:

 `current`: Initially, it’s set to the `initialValue` you have passed. You can later set it to something else. If you pass the ref object to React as a `ref` attribute to a JSX node, React will set its `current` property.

On the next renders, `useRef` will return the same object.
#### Caveats 

- You can mutate the `ref.current` property. Unlike state, it is mutable. However, if it holds an object that is used for rendering (for example, a piece of your state), then you shouldn’t mutate that object.
- When you change the `ref.current` property, React does not re-render your component. React is not aware of when you change it because a ref is a plain JavaScript object.
- Do not write _or read_ `ref.current` during rendering, except for [initialization.](https://react.dev/reference/react/useRef#avoiding-recreating-the-ref-contents) This makes your component’s behavior unpredictable.
- In Strict Mode, React will **call your component function twice** in order to [help you find accidental impurities.](https://react.dev/reference/react/useState#my-initializer-or-updater-function-runs-twice)This is development-only behavior and does not affect production. Each ref object will be created twice, but one of the versions will be discarded. If your component function is pure (as it should be), this should not affect the behavior.

---
## Usage

### Referencing a value with a ref
### Manipulating the DOM with a ref
### Избегающий повторного создания содержимого ссылки