---
tags:
  - hooks
  - react
---
`useSyncExternalStore` is a React Hook that lets you subscribe to an external store.

```js
const snapshot = useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)
```
## Reference

### `useSyncExternalStore(subscribe, getSnapshot, getServerSnapshot?)`

Call `useSyncExternalStore` at the top level of your component to read a value from an external data store.

```js
import { useSyncExternalStore } from 'react';
import { todosStore } from './todoStore.js';

function TodosApp() {  
	const todos = useSyncExternalStore(todosStore.subscribe, todosStore.getSnapshot);  
	// ...}
```

It returns the snapshot of the data in the store. You need to pass two functions as arguments:

1. The `subscribe` function should subscribe to the store and return a function that unsubscribes.
2. The `getSnapshot` function should read a snapshot of the data from the store.

#### Parameters

- `subscribe`: A function that takes a single `callback` argument and subscribes it to the store. When the store changes, it should invoke the provided `callback`. This will cause the component to re-render. The `subscribe` function should return a function that cleans up the subscription.

- `getSnapshot`: A function that returns a snapshot of the data in the store that’s needed by the component. While the store has not changed, repeated calls to `getSnapshot` must return the same value. If the store changes and the returned value is different (as compared by [[Object.is()]], React re-renders the component.

- **optional** `getServerSnapshot`: A function that returns the initial snapshot of the data in the store. It will be used only during server rendering and during hydration of server-rendered content on the client. The server snapshot must be the same between the client and the server, and is usually serialized and passed from the server to the client. If you omit this argument, rendering the component on the server will throw an error.

#### Returns 

The current snapshot of the store which you can use in your rendering logic.
#### Caveats 

- The store snapshot returned by `getSnapshot` must be immutable. If the underlying store has mutable data, return a new immutable snapshot if the data has changed. Otherwise, return a cached last snapshot.

- If a different `subscribe` function is passed during a re-render, React will re-subscribe to the store using the newly passed `subscribe` function. You can prevent this by declaring `subscribe` outside the component.

- If the store is mutated during a [non-blocking transition update](https://react.dev/reference/react/useTransition), React will fall back to performing that update as blocking. Specifically, for every transition update, React will call `getSnapshot` a second time just before applying changes to the DOM. If it returns a different value than when it was called originally, React will restart the update from scratch, this time applying it as a blocking update, to ensure that every component on screen is reflecting the same version of the store.

- It’s not recommended to _suspend_ a render based on a store value returned by `useSyncExternalStore`. The reason is that mutations to the external store cannot be marked as [non-blocking transition updates](https://react.dev/reference/react/useTransition), so they will trigger the nearest [`Suspense` fallback](https://react.dev/reference/react/Suspense), replacing already-rendered content on screen with a loading spinner, which typically makes a poor UX.

For example, the following are discouraged:

```js
const LazyProductDetailPage = lazy(() => 
	import('./ProductDetailPage.js'));

function ShoppingApp() {  
	const selectedProductId = useSyncExternalStore(...);  
	// ❌ Calling `use` with a Promise dependent on `selectedProductId`  
	const data = use(fetchItem(selectedProductId))  
	// ❌ Conditionally rendering a lazy component based on `selectedProductId`  
	return selectedProductId != null 
		? <LazyProductDetailPage /> 
		: <FeaturedProducts />;
}
```

## Usage
### Subscribing to an external store
### Subscribing to a browser API 
### Extracting the logic to a custom Hook
### Adding support for server rendering