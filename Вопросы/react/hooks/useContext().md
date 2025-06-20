---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useContext
---
`useContext` is a React Hook that lets you read and subscribe to [context](https://react.dev/learn/passing-data-deeply-with-context) from your component.

```js
const value = useContext(SomeContext)
```
## Reference

### `useContext(SomeContext)`

Call `useContext` at the top level of your component to read and subscribe to [context.](https://react.dev/learn/passing-data-deeply-with-context)

```jsx
import { useContext } from 'react';
	
function MyComponent() {  
	const theme = useContext(ThemeContext);  
	// ...
```

#### Parameters
`SomeContext`: The context that you’ve previously created with [[createContext()]]. The context itself does not hold the information, it only represents the kind of information you can provide or read from components.

#### Returns

`useContext` returns the context value for the calling component. It is determined as the `value` passed to the closest `SomeContext.Provider` above the calling component in the tree. If there is no such provider, then the returned value will be the `defaultValue` you have passed to [[useContext()]] for that context. The returned value is always up-to-date. React automatically re-renders components that read some context if it changes.

#### Caveats

- `useContext()` call in a component is not affected by providers returned from the _same_ component. The corresponding `<Context.Provider>` **needs to be _above_** the component doing the `useContext()` call.
- React **automatically re-renders** all the children that use a particular context starting from the provider that receives a different `value`. The previous and the next values are compared with the [[Object.is()]] comparison. Skipping re-renders with [[memo]] does not prevent the children receiving fresh context values.
- If your build system produces duplicates modules in the output (which can happen with symlinks), this can break context. Passing something via context only works if `SomeContext` that you use to provide context and `SomeContext` that you use to read it are **_exactly_ the same object**, as determined by a `===` comparison.

---

## Usage
### Passing data deeply into the tree
### Updating data passed via context
### Specifying a fallback default value
### Overriding context for a part of the tree
### Optimizing re-renders when passing objects and functions
```jsx
import { useCallback, useMemo } from 'react';  

function MyApp() {
	const [currentUser, setCurrentUser] = useState(null); 

	const login = useCallback((response) => {  
		storeCredentials(response.credentials);  
		setCurrentUser(response.user);  
	}, []);  

	const contextValue = useMemo(() => ({  
		currentUser,  
		login  
	}), [currentUser, login]);  

	return (  
		<AuthContext.Provider value={contextValue}>  
			<Page />  
		</AuthContext.Provider>  
	);  
}
```