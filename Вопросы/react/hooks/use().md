---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useId
---
`use` is a React Hook that lets you read the value of a resource like a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) or [context](https://react.dev/learn/passing-data-deeply-with-context).
```js
const value = use(resource);
```

## Reference

### `use(resource)`

Call `use` in your component to read the value of a resource like a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) or [context](https://react.dev/learn/passing-data-deeply-with-context).

```jsx
import { use } from 'react';

function MessageComponent({ messagePromise }) { 
	const message = use(messagePromise); 
	const theme = use(ThemeContext);  
	// ...
```

Unlike all other React Hooks, `use` can be called within loops and conditional statements like `if`
Like other React Hooks, the function that calls `use` must be a Component or Hook.

When called with a Promise, the `use` Hook integrates with [`Suspense`](https://react.dev/reference/react/Suspense) and [error boundaries](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary). The component calling `use` _suspends_ while the Promise passed to `use` is pending. If the component that calls `use` is wrapped in a Suspense boundary, the fallback will be displayed. Once the Promise is resolved, the Suspense fallback is replaced by the rendered components using the data returned by the `use` Hook. If the Promise passed to `use` is rejected, the fallback of the nearest Error Boundary will be displayed.
#### Parameters

- `resource`: this is the source of the data you want to read a value from. A resource can be a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) or a [context](https://react.dev/learn/passing-data-deeply-with-context).

#### Returns

The `use` Hook returns the value that was read from the resource like the resolved value of a [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) or [context](https://react.dev/learn/passing-data-deeply-with-context).

#### Caveats

- The `use` Hook must be called inside a Component or a Hook.
- When fetching data in a [Server Component](https://react.dev/reference/react/use-server), prefer `async` and `await` over `use`. `async` and `await` pick up rendering from the point where `await` was invoked, whereas `use` re-renders the component after the data is resolved.
- Prefer creating Promises in [Server Components](https://react.dev/reference/react/use-server) and passing them to [Client Components](https://react.dev/reference/react/use-client) over creating Promises in Client Components. Promises created in Client Components are recreated on every render. Promises passed from a Server Component to a Client Component are stable across re-renders. [See this example](https://react.dev/reference/react/use#streaming-data-from-server-to-client).

## Usage

### Reading context with `use`
When a [context](https://react.dev/learn/passing-data-deeply-with-context) is passed to `use`, it works similarly to [`useContext`](https://react.dev/reference/react/useContext). While `useContext` must be called at the top level of your component, `use` can be called inside conditionals like `if` and loops like `for`. `use` is preferred over `useContext` because it is more flexible.

```jsx
import { use } from 'react';

function Button() {  
	const theme = use(ThemeContext);  
	// ...
```

### Streaming data from the server to the client
### ! Dealing with rejected Promises 
>`use` cannot be called in a try-catch block. Instead of a try-catch block [wrap your component in an Error Boundary](https://react.dev/reference/react/use#displaying-an-error-to-users-with-error-boundary), or [provide an alternative value to use with the Promise’s `.catch` method](https://react.dev/reference/react/use#providing-an-alternative-value-with-promise-catch).