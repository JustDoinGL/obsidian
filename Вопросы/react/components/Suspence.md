---
tags:
  - react
links:
  - https://react.dev/reference/react/Suspense
---
`<Suspense>` lets you display a fallback until its children have finished loading.

## Reference

### `<Suspense>`

#### Props

- `children`: The actual UI you intend to render. If `children` suspends while rendering, the Suspense boundary will switch to rendering `fallback`.
- `fallback`: An alternate UI to render in place of the actual UI if it has not finished loading. Any valid React node is accepted, though in practice, a fallback is a lightweight placeholder view, such as a loading spinner or skeleton. Suspense will automatically switch to `fallback` when `children` suspends, and back to `children` when the data is ready. If `fallback` suspends while rendering, it will activate the closest parent Suspense boundary.

#### Caveats

- React не сохраняет никакого состояния для рендеров, которые были приостановлены до того, как их удалось смонтировать в первый раз. Когда компонент загрузится, React повторит рендеринг приостановленного дерева с нуля.
- Если Suspense отображал содержимое для дерева, но затем оно снова приостановилось, "резервный вариант" будет показан снова, если только обновление, вызвавшее его, не было вызвано [[startTransition]] or [[useDeferredValue()]].
- If React needs to hide the already visible content because it suspended again, it will clean up [[useLayoutEffect()]] in the content tree. When the content is ready to be shown again, React will fire the layout Effects again. This ensures that Effects measuring the DOM layout don’t try to do this while the content is hidden.
- React includes under-the-hood optimizations like _Streaming Server Rendering_ and _Selective Hydration_ that are integrated with Suspense. Read [an architectural overview](https://github.com/reactwg/react-18/discussions/37) and watch [a technical talk](https://www.youtube.com/watch?v=pj5N-Khihgc) to learn more.