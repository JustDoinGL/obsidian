---
tags:
  - algorithm
links:
  - https://walborn.notion.site/Throttling-e5101dd0b1b344de9f1f710d127a1b0c
---
Тротлинг не обновляет таймер каждый раз, в отличие от [[Debounce]]

1. Если таймер не активный Выполнить `callback`. Запустить новый таймер.
2. Если таймер работает Запомнить аргументы на будущее, перезаписав старые, если были. Это может происходить много раз. Как только таймер сработает, выполнить с последними аргументами.

Мы двигаем мышью по экрану и хотим видеть всю траекторию движения, а не только, когда её остановили (как было бы при [[Debounce]]). Нам, конечно, кажется, что мышь двигается плавно, но на самом деле тут просто значение `delay` очень маленькое, поряядка `16.7ms`. Нам станет заметно подёргивание мыши только тогда, когда что-то начнёт грузить основной поток js. 
## **Пример**

**Сверху** - все события

**Снизу** - когда сработал debounce

🥶 😱 🫥 🫥 🥹 🫥 🥵 🥳 🫥

🥶 🫥 🫥 🫥 😱 🫥 🫥 🫥 🥳

## Аналогия из жизни

Это похоже на измерение веса по утрам, когда хотим похудеть. Каждое утро мы встаём на весы, чтоб записать новый вес. Понятное дело, что в течение дня вес может сильно меняться, но нас это не сильно волнует, нам важна динамика на длинной дистанции. Как только перестали хотеть похудеть - считайте, что события перестали появляться. События типа “Что-то живот какой-то большой, нужно бы завтра взвеситься, проверить вес”
### vanila
```js
const throttle = (callback, delay) => {
	let pending = false
	let args = undefined

	const wrapper = (...nextArgs) => {
		args = nextArgs
		if (pending) return

		callback(...args)

		pending = true
		args = undefined

		setTimeout(() => {
			pending = false
			if (args) wrapper(...args)
		}, delay)
	}
	return wrapper
}

// usage
const throttled = throttle(console.log, 100)
throttled('log') // logged immediately
throttled('log') // logged at t = 100ms
```
### React
```jsx
const useThrottle = (callback, delay) => {
	const pending = useRef()
	const args = useRef()

	const throttled = useCallback((...nextArgs) => {
		args.current = nextArgs
		if (pending.current) return
		callback(...args.current)
		pending.current = true

		setTimeout(() => {
			pending.current = false
			if (args.current) wrapper(...args.current)
		}, delay)
	}, [ callback, delay ])
	
	return throttled
}

// usage
const searchCharactersDebounced = useThrottle(searchCharacters, 500)
```

### TS React
```tsx
function useThrottle<T>(value: T, interval = 500): T {
  const [ throttled, setThrottled ] = useState<T>(value)
  const prev = useRef<number>(Date.now())

  useEffect(() => {
    if (Date.now() >= prev.current + interval) {
      prev.current = Date.now()
      setThrottled(value)
    } else {
      const timer = setTimeout(() => {
        prev.current = Date.now()
        setThrottled(value)
      }, interval)

      return () => clearTimeout(timer)
    }
  }, [ value, interval ])

  return throttled
}
```