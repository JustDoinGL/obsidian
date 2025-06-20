---
tags:
  - react
links:
  - https://react.dev/reference/react/createContext
---
`createContext` lets you create a [context](https://react.dev/learn/passing-data-deeply-with-context) that components can provide or read.

```js
const SomeContext = createContext(defaultValue)
```
## Reference

### `createContext(defaultValue)`

Call `createContext` outside of any components to create a context.

```js
import { createContext } from 'react';

const ThemeContext = createContext('light');
```

#### Parameters
`defaultValue`: The value that you want the context to have when there is no matching context provider in the tree above the component that reads context. If you don’t have any meaningful default value, specify `null`. The default value is meant as a “last resort” fallback. It is static and never changes over time.

#### Returns

`createContext` returns a context object.

**The context object itself does not hold any information.** It represents _which_ context other components read or provide. Typically, you will use `SomeContext.Provider` in components above to specify the context value, and call [[useContext()]] in components below to read it. The context object has a few properties:

- `SomeContext.Provider` lets you provide the context value to components.
- `SomeContext.Consumer` is an alternative and rarely used way to read the context value.

---

### `SomeContext.Provider`

Wrap your components into a context provider to specify the value of this context for all components inside:

```jsx
function App() {  
	const [theme, setTheme] = useState('light');  
	// ...  
	return (    
		<ThemeContext.Provider value={theme}>      
			<Page />    
		</ThemeContext.Provider>  
	);
}
```

#### Props
 `value`: The value that you want to pass to all the components reading this context inside this provider, no matter how deep. The context value can be of any type. A component calling [[useContext()]] inside of the provider receives the `value` of the innermost corresponding context provider above it.

---

### `SomeContext.Consumer`

Before `useContext` existed, there was an older way to read context:

```jsx
function Button() {  
	// 🟡 Legacy way (not recommended)  
	return (    
		<ThemeContext.Consumer>      
			{theme => (       
				 <button className={theme} />      
			)} 
		</ThemeContext.Consumer>  
	);
}
```

Although this older way still works, but **newly written code should read context with [[useContext()]] instead:**

```jsx
function Button() {
	// ✅ Recommended way 
	const theme = useContext(ThemeContext);  
	return <button className={theme} />;
}
```

#### Props

 `children`: A function. React will call the function you pass with the current context value determined by the same algorithm as [[useContext()]] does, and render the result you return from this function. React will also re-run this function and update the UI whenever the context from the parent components changes.

---

## Usage

### Creating context

### Importing and exporting context from a file
