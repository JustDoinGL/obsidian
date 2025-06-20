---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react-dom/hooks/useFormState
  - https://www.youtube.com/watch?v=ffveQQtxNgM
---

`useFormState` is a Hook that allows you to update state based on the result of a form action.

```jsx
const [state, formAction] = useFormState(fn, initialState, permalink?);
```
## Reference

### `useFormState(action, initialState, permalink?)`

#### Parameters
- `fn`: The function to be called when the form is submitted or button pressed. When the function is called, it will receive the previous state of the form (initially the `initialState` that you pass, subsequently its previous return value) as its initial argument, followed by the arguments that a form action normally receives.
- `initialState`: The value you want the state to be initially. It can be any serializable value. This argument is ignored after the action is first invoked.
- **optional** `permalink`: A string containing the unique page URL that this form modifies. For use on pages with dynamic content (eg: feeds) in conjunction with progressive enhancement: if `fn` is a [server action](https://react.dev/reference/react/use-server) and the form is submitted before the JavaScript bundle loads, the browser will navigate to the specified permalink URL, rather than the current page’s URL. Ensure that the same form component is rendered on the destination page (including the same action `fn` and `permalink`) so that React knows how to pass the state through. Once the form has been hydrated, this parameter has no effect.

#### Returns

`useFormState` returns an array with exactly two values:

1. The current state. During the first render, it will match the `initialState` you have passed. After the action is invoked, it will match the value returned by the action.
2. A new action that you can pass as the `action` prop to your `form` component or `formAction` prop to any `button` component within the form.

#### Caveats

- When used with a framework that supports React Server Components, `useFormState` lets you make forms interactive before JavaScript has executed on the client. When used without Server Components, it is equivalent to component local state.
- The function passed to `useFormState` receives an extra argument, the previous or initial state, as its first argument. This makes its signature different than if it were used directly as a form action without using `useFormState`.

## Usage

### Using information returned by a form action
## Troubleshooting
### My action can no longer read the submitted form data
When you wrap an action with `useFormState`, it gets an extra argument _as its first argument_. The submitted form data is therefore its _second_ argument instead of its first as it would usually be. The new first argument that gets added is the current state of the form.

```js
function action(currentState, formData) {
	// ...
}
```