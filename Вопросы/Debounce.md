---
tags:
  - algorithm
links:
  - https://walborn.notion.site/Debounce-73df76233a4d42dca0ce5ce66c20a4d3
---
Пока происходят события - ничего не делаем, но как только прошло некоторое время и не было новых событий - сразу выполним **callback**. Например, пока пользователь вводит что-то в строку поиска, ждём, когда же он закончит. Пользователь перестал - сразу отправляем запрос на бек с введённой строкой

`callback` - функция, которая должна выполниться в конце концов , а время ожидания

`delay` - время бездействия, по окончании которого вызовется колбек, но если за это время произойдёт новое событие, то отсчёт начнётся заново

## `debounced = debounce(callback, delay)` 
новая функция, после применения нашего алгоритма
## Пример

`debounced` функция была вызвана в `60ms, 90ms, и 160ms`.

Если `delay = 100ms`, то первые два вызова будут отменены, а последний выполнится в `260ms`

Если `delay = 50ms`, то первый отменится, а второй выполнится в `140ms`, и третий тоже выполнится, но в `210ms`

```js
const debounce = (callback, delay) => { 
	let timer = null // замыкаем 
	return (...args) => { 
		if (timer) clearTimeout(timer) 
		timer = setTimeout(() => callback(...args), delay) 
	} 
} 

// или короче 

const debounce = (callback, delay, timer) => (...args) => { 
	clearTimeout(timer) 
	timer = setTimeout(() => callback(...args), delay) 
}
// События - ввод пользователя 
// Callback - запрос пользователей 
const debouncedFetchUsers = debounce(fetchUsers, 300)
```

### В React можно написать свой хук

```jsx
const useDebounce = (callback, delay) => { 
	const timer = useRef() 
	const debounce = useCallback((...args) => { 
		if (timer.current) { 
			clearTimeout(timer.current) 
		} 
		timer.current = setTimeout(() => callback(...args), delay) 
	}, [ callback, delay ]) 
	return debounce 
} // usage 

const [ users, setUsers ] = useState([]) 
const [ value, setValue ] = useState('') 
const handleFetchUsers = search => { 
	fetchUsers(search).then(setUsers) 
} 

const debouncedFetchUsers = useDebounce(handleFetchUsers, 500) 

useEffect(() => { 
	debouncedFetchUsers(value) 
}, [ value ])
```

```jsx
function useDebounce(value, delay) { 
	// State and setters for debounced value 
	const [ debounced, setDebounced ] = useState(value) 
	
	useEffect(() => { 
		// Update debounced value after delay 
		const handler = setTimeout(() => setDebounced(value), delay); 
		// Cancel the timeout if value changes (also on delay change or unmount) 
		// This is how we prevent debounced value from updating if value is changed ... 
		// .. within the delay period. Timeout gets cleared and restarted. 
		return () => clearTimeout(handler) 
	}, [ value, delay ] 
	// Only re-call effect if value or delay changes ) 
	return debounced 
} 

// usage 
const [ users, setUsers ] = useState([]) 
const [ value, setValue ] = useState('') 

const debounced = useDebounce(value, 500) 

useEffect(() => { 
	if (!debounced) setValues([]) 
	fetchUsers(debounced).then(seUsers) 
}, [ debounced ])
```
