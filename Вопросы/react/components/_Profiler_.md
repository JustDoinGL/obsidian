---
tags:
  - react
links:
  - https://react.dev/reference/react/Profiler
---
`<Profiler>` lets you measure rendering performance of a React tree programmatically.

```jsx
<Profiler id="App" onRender={onRender}> 
	<App />
</Profiler>
```
## Reference

### `<Profiler>`

Wrap a component tree in a `<Profiler>` to measure its rendering performance.

```jsx
<Profiler id="App" onRender={onRender}>  
	<App />
</Profiler>
```

#### Props
- `id`: A string identifying the part of the UI you are measuring.
- `onRender`: An `onRender` that React calls every time components within the profiled tree update. It receives information about what was rendered and how much time it took.

#### Caveats
Profiling adds some additional overhead, so **it is disabled in the production build by default.** To opt into production profiling, you need to enable a [special production build with profiling enabled.](https://fb.me/react-profiling)

---

### `onRender` callback 
React will call your `onRender` callback with information about what was rendered.

```js
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {  // Aggregate or log render timings...}
```

#### Parameters

- `id`: The string `id` prop of the `<Profiler>` tree that has just committed. This lets you identify which part of the tree was committed if you are using multiple profilers.
- `phase`: `"mount"`, `"update"` or `"nested-update"`. This lets you know whether the tree has just been mounted for the first time or re-rendered due to a change in props, state, or hooks.
- `actualDuration`: The number of milliseconds spent rendering the `<Profiler>` and its descendants for the current update. This indicates how well the subtree makes use of memoization (e.g. [[memo]] and [[useMemo()]]). Ideally this value should decrease significantly after the initial mount as many of the descendants will only need to re-render if their specific props change.
- `baseDuration`: The number of milliseconds estimating how much time it would take to re-render the entire `<Profiler>` subtree without any optimizations. It is calculated by summing up the most recent render durations of each component in the tree. This value estimates a worst-case cost of rendering (e.g. the initial mount or a tree with no memoization). Compare `actualDuration` against it to see if memoization is working.
- `startTime`: A numeric timestamp for when React began rendering the current update.
- `commitTime`: A numeric timestamp for when React committed the current update. This value is shared between all profilers in a commit, enabling them to be grouped if desirable.
## Usage

### Measuring rendering performance programmatically
### Measuring different parts of the application