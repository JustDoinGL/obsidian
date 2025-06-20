---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useReducer
---

`useReducer` is a React Hook that lets you add a [[Редюсер (reducer)]] to your component.

```js
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

## Reference
### `useReducer(reducer, initialArg, init?)`

Call `useReducer` at the top level of your component to manage its state with a [reducer.](https://react.dev/learn/extracting-state-logic-into-a-reducer)

```js
import { useReducer } from 'react';

function reducer(state, action) {  
	// ...
}

function MyComponent() {  
	const [state, dispatch] = useReducer(reducer, { age: 42 });  
	// ...
```

#### Parameters

- `reducer`: The reducer function that specifies how the state gets updated. It must be pure, should take the state and action as arguments, and should return the next state. State and action can be of any types.
- `initialArg`: The value from which the initial state is calculated. It can be a value of any type. How the initial state is calculated from it depends on the next `init` argument.
- **optional** `init`: The initializer function that should return the initial state. If it’s not specified, the initial state is set to `initialArg`. Otherwise, the initial state is set to the result of calling `init(initialArg)`.

#### Returns

`useReducer` returns an array with exactly two values:

1. The current state. During the first render, it’s set to `init(initialArg)` or `initialArg` (if there’s no `init`).
2. The [`dispatch` function](https://react.dev/reference/react/useReducer#dispatch) that lets you update the state to a different value and trigger a re-render.

#### Caveats
- `useReducer` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a new component and move the state into it.
- In Strict Mode, React will **call your reducer and initializer twice** in order to [help you find accidental impurities.](https://react.dev/reference/react/useReducer#my-reducer-or-initializer-function-runs-twice) This is development-only behavior and does not affect production. If your reducer and initializer are pure (as they should be), this should not affect your logic. The result from one of the calls is ignored.
### `dispatch` function

The `dispatch` function returned by `useReducer` lets you update the state to a different value and trigger a re-render. You need to pass the action as the only argument to the `dispatch` function:

```JS
const [state, dispatch] = useReducer(reducer, { age: 42 });

function handleClick() {  
	dispatch({ type: 'incremented_age' });  
	// ...
```

React will set the next state to the result of calling the `reducer` function you’ve provided with the current `state` and the action you’ve passed to `dispatch`.

#### Parameters

- `action`: The action performed by the user. It can be a value of any type. ==By convention, an action is usually an object with a `type` property identifying it and, optionally, other properties with additional information.==

#### Returns 

`dispatch` functions do not have a return value.

#### Caveats 
- The `dispatch` function **only updates the state variable for the _next_ render**. If you read the state variable after calling the `dispatch` function, [you will still get the old value](https://react.dev/reference/react/useReducer#ive-dispatched-an-action-but-logging-gives-me-the-old-state-value) that was on the screen before your call.

- If the new value you provide is identical to the current `state`, as determined by an [[Object.is()]] comparison, React will **skip re-rendering the component and its children.** This is an optimization. React may still need to call your component before ignoring the result, but it shouldn’t affect your code.

- React [batches state updates.](https://react.dev/learn/queueing-a-series-of-state-updates) It updates the screen **after all the event handlers have run** and have called their `set` functions. This prevents multiple re-renders during a single event. In the rare case that you need to force React to update the screen earlier, for example to access the DOM, you can use [[flushSync]]


---