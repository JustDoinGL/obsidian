---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useCallback
  - https://youtu.be/2Wp7QPTkpms?si=l0bSbM3AtamSavmo
---
`useCallback` is a React Hook that lets you cache a function definition between re-renders.

```js
const cachedFn = useCallback(fn, dependencies)
```

## Reference
### `useCallback(fn, dependencies)`

Call `useCallback` at the top level of your component to cache a function definition between re-renders:

#### Parameters

- `fn`: The function value that you want to cache. It can take any arguments and return any values. React will return (not call!) your function back to you during the initial render. On next renders, React will give you the same function again if the `dependencies` have not changed since the last render. Otherwise, it will give you the function that you have passed during the current render, and store it in case it can be reused later. React will not call your function. The function is returned to you so you can decide when and whether to call it.

- `dependencies`: The list of all reactive values referenced inside of the `fn` code. Reactive values include props, state, and all the variables and functions declared directly inside your component body. If your linter is [configured for React](https://react.dev/learn/editor-setup#linting), it will verify that every reactive value is correctly specified as a dependency. The list of dependencies must have a constant number of items and be written inline like `[dep1, dep2, dep3]`. React will compare each dependency with its previous value using the [[Object.is()]] comparison algorithm.


#### Returns
On the initial render, `useCallback` returns the `fn` function you have passed.

During subsequent renders, it will either return an already stored `fn` function from the last render (if the dependencies haven’t changed), or return the `fn` function you have passed during this render.

#### Caveats

- `useCallback` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a new component and move the state into it.
- React **will not throw away the cached function unless there is a specific reason to do that.** For example, in development, React throws away the cache when you edit the file of your component. Both in development and in production, React will throw away the cache if your component suspends during the initial mount. In the future, React may add more features that take advantage of throwing away the cache—for example, if React adds built-in support for virtualized lists in the future, it would make sense to throw away the cache for items that scroll out of the virtualized table viewport. This should match your expectations if you rely on `useCallback` as a performance optimization. Otherwise, a [state variable](https://react.dev/reference/react/useState#im-trying-to-set-state-to-a-function-but-it-gets-called-instead) or a [ref](https://react.dev/reference/react/useRef#avoiding-recreating-the-ref-contents) may be more appropriate.
## Usage

### Skipping re-rendering of components
### Updating state from a memoized callback
> Был вопрос на собесе: https://youtu.be/I5vdydjkmAE?si=2NuQ2qjOLmZsfyBy

Sometimes, you might need to update state based on previous state from a memoized callback.

This `handleAddTodo` function specifies `todos` as a dependency because it computes the next todos from it:

```jsx
function TodoList() {  
	const [todos, setTodos] = useState([]);  
	
	const handleAddTodo = useCallback((text) => {    
		const newTodo = { 
			id: nextId++, text 
		};    		
		setTodos([...todos, newTodo]);  }, [todos]);  
	// ...
```

You’ll usually want memoized functions to have as few dependencies as possible. When you read some state only to calculate the next state, you can remove that dependency by passing an [updater function](https://react.dev/reference/react/useState#updating-state-based-on-the-previous-state) instead:

```jsx
function TodoList() {  
	const [todos, setTodos] = useState([]);  
	const handleAddTodo = useCallback((text) => {    
		const newTodo = { id: nextId++, text };    
		setTodos(todos => [...todos, newTodo]);  
	}, []); 
	// ✅ No need for the todos dependency  // ...
```

Here, instead of making `todos` a dependency and reading it inside, you pass an instruction about _how_ to update the state (`todos => [...todos, newTodo]`) to React. [Read more about updater functions.](https://react.dev/reference/react/useState#updating-state-based-on-the-previous-state)

### Preventing an Effect from firing too often
To solve this, you can wrap the function you need to call from an Effect into `useCallback`:

```jsx
function ChatRoom({ roomId }) {  
	const [message, setMessage] = useState('');  
	const createOptions = useCallback(() => {    
		return {     
			serverUrl: 'https://localhost:1234',      
			roomId: roomId   
		};  
	}, [roomId]); 
	// ✅ Only changes when roomId changes  
	useEffect(() => {    
		const options = createOptions();   
		const connection = createConnection();    
		connection.connect();    
		return () => connection.disconnect();  
	}, [createOptions]); 
	// ✅ Only changes when createOptions changes  // ...
```

This ensures that the `createOptions` function is the same between re-renders if the `roomId` is the same. **However, it’s even better to remove the need for a function dependency.** Move your function _inside_ the Effect:

```jsx
function ChatRoom({ roomId }) {  
	const [message, setMessage] = useState('');  
	useEffect(() => {    
		function createOptions() { 
		// ✅ No need for useCallback or function dependencies!      
			return {        
				serverUrl: 'https://localhost:1234',        
				roomId: roomId      
			}; 
		}    
		const options = createOptions();    
		const connection = createConnection();    
		connection.connect();    
		return () => connection.disconnect();  
	}, [roomId]); 
	// ✅ Only changes when roomId changes  // ...
```

Now your code is simpler and doesn’t need `useCallback`. [Learn more about removing Effect dependencies.](https://react.dev/learn/removing-effect-dependencies#move-dynamic-objects-and-functions-inside-your-effect)

### Optimizing a custom Hook

If you’re writing a [custom Hook,](https://react.dev/learn/reusing-logic-with-custom-hooks) it’s recommended to wrap any functions that it returns into `useCallback`:

```jsx
function useRouter() {  
	const { dispatch } = useContext(RouterStateContext);  
	
	const navigate = useCallback((url) => {  
		dispatch({ type: 'navigate', url });  
	}, [dispatch]);  

	const goBack = useCallback(() => {  
		dispatch({ type: 'back' });  
	}, [dispatch]);  

	return {  
		navigate,  
		goBack,  
	};  
}
```

This ensures that the consumers of your Hook can optimize their own code when needed.