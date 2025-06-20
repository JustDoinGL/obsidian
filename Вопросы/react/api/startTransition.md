---
tags:
  - react
---


`startTransition` lets you update the state without blocking the UI.

## Reference 

### `startTransition(scope)`

The `startTransition` function lets you mark a state update as a transition.

```jsx
import { startTransition } from 'react';

function TabContainer() {  
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
- `scope`: A function that updates some state by calling one or more [`set` functions.](https://react.dev/reference/react/useState#setstate)React immediately calls `scope` with no arguments and marks all state updates scheduled synchronously during the `scope` function call as transitions. They will be [non-blocking](https://react.dev/reference/react/useTransition#marking-a-state-update-as-a-non-blocking-transition) and [will not display unwanted loading indicators.](https://react.dev/reference/react/useTransition#preventing-unwanted-loading-indicators)

#### Returns 

`startTransition` does not return anything.

#### Caveats

- `startTransition` does not provide a way to track whether a transition is pending. To show a pending indicator while the transition is ongoing, you need [[useTransition()]] instead.
    
- You can wrap an update into a transition only if you have access to the `set` function of that state. If you want to start a transition in response to some prop or a custom Hook return value, try [[useDeferredValue()]] instead.
    
- The function you pass to `startTransition` must be synchronous. React immediately executes this function, marking all state updates that happen while it executes as transitions. If you try to perform more state updates later (for example, in a timeout), they won’t be marked as transitions.
    
- A state update marked as a transition will be interrupted by other state updates. For example, if you update a chart component inside a transition, but then start typing into an input while the chart is in the middle of a re-render, React will restart the rendering work on the chart component after handling the input state update.
    
- Transition updates can’t be used to control text inputs.
    
- If there are multiple ongoing transitions, React currently batches them together. This is a limitation that will likely be removed in a future release.