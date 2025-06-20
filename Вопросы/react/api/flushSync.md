---
tags:
  - react
---

`flushSync` позволяет заставить React синхронно сбрасывать любые обновления внутри предоставленного обратного вызова. Это гарантирует, что DOM будет обновлен немедленно.

## Reference

### `flushSync(callback)`

Call `flushSync` to force React to flush any pending work and update the DOM synchronously.

```jsx
import { flushSync } from 'react-dom';

flushSync(() => {  setSomething(123);});
```

Most of the time, `flushSync` can be avoided. Use `flushSync` as last resort.


#### Parameters

- `callback`: A function. React will immediately call this callback and flush any updates it contains synchronously. It may also flush any pending updates, or Effects, or updates inside of Effects. If an update suspends as a result of this `flushSync` call, the fallbacks may be re-shown.

#### Returns

`flushSync` returns `undefined`.

#### Caveats

- `flushSync` can significantly hurt performance. Use sparingly.
- `flushSync` may force pending Suspense boundaries to show their `fallback` state.
- `flushSync` may run pending effects and synchronously apply any updates they contain before returning.
- `flushSync` may flush updates outside the callback when necessary to flush the updates inside the callback. For example, if there are pending updates from a click, React may flush those before flushing the updates inside the callback.