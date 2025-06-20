---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useTransition
---
`useTransition` is a React Hook that lets you update the state without blocking the UI.

```js
const [isPending, startTransition] = useTransition()
```
## Reference

### `useTransition()`

Call `useTransition` at the top level of your component to mark some state updates as transitions.

```js
import { useTransition } from 'react';

function TabContainer() { 
	const [isPending, startTransition] = useTransition();  
	// ...
}
```


#### Parameters
`useTransition` does not take any parameters.

#### Returns

`useTransition` returns an array with exactly two items:

1. The `isPending` flag that tells you whether there is a pending transition.
2. The [[startTransition]] that lets you mark a state update as a transition.

---

### `startTransition` function

The `startTransition` function returned by `useTransition` lets you mark a state update as a transition.

```js
function TabContainer() {  
	const [isPending, startTransition] = useTransition();  
	const [tab, setTab] = useState('about');  
	
	function selectTab(nextTab) {    
		startTransition(() => {      
			setTab(nextTab);    
		});  
	}  
	// ...
}
```

#### Parameters
 `scope`: A function that updates some state by calling one or more [`set` functions.](https://react.dev/reference/react/useState#setstate) React immediately calls `scope` with no parameters and marks all state updates scheduled synchronously during the `scope` function call as transitions. They will be [non-blocking](https://react.dev/reference/react/useTransition#marking-a-state-update-as-a-non-blocking-transition) and [will not display unwanted loading indicators.](https://react.dev/reference/react/useTransition#preventing-unwanted-loading-indicators)

#### Returns
`startTransition` does not return anything.

#### Caveats 

- `useTransition` is a Hook, so it can only be called inside components or custom Hooks. If you need to start a transition somewhere else (for example, from a data library), call the standalone [[startTransition]] instead.

- You can wrap an update into a transition only if you have access to the `set` function of that state. If you want to start a transition in response to some prop or a custom Hook value, try [[useDeferredValue()]] instead.

- The function you pass to `startTransition` must be synchronous. React immediately executes this function, marking all state updates that happen while it executes as transitions. If you try to perform more state updates later (for example, in a timeout), they won’t be marked as transitions.

- A state update marked as a transition will be interrupted by other state updates. For example, if you update a chart component inside a transition, but then start typing into an input while the chart is in the middle of a re-render, React will restart the rendering work on the chart component after handling the input update.

- Transition updates can’t be used to control text inputs.

- If there are multiple ongoing transitions, React currently batches them together. This is a limitation that will likely be removed in a future release.

## Usage

### Marking a state update as a non-blocking transition
### Updating the parent component in a transition 
### Displaying a pending visual state during the transition
### Preventing unwanted loading indicators
### Building a Suspense-enabled router 
### Displaying an error to users with an error boundary