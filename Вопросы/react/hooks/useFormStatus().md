---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react-dom/hooks/useFormStatus
  - https://youtu.be/_tUdtt6H5CE?si=zQM3XgRGokRnLoI2
  - https://www.youtube.com/watch?v=ffveQQtxNgM
---
`useFormStatus` is a Hook that gives you status information of the last form submission.

```jsx
const { pending, data, method, action } = useFormStatus();
```
## Reference

### `useFormStatus()`

The `useFormStatus` Hook provides status information of the last form submission.

```jsx
import { useFormStatus } from "react-dom";  

import action from './actions';  

function Submit() {  
	const status = useFormStatus();  

	return <button disabled={status.pending}>Submit</button>  
}

export default function App() {  
	return (  
		<form action={action}>  
			<Submit />  
		</form>  
	);  
}
```

#### Parameters

`useFormStatus` does not take any parameters.

#### Returns

A `status` object with the following properties:

- `pending`: A boolean. If `true`, this means the parent `<form>` is pending submission. Otherwise, `false`.

- `data`: An object implementing the [`FormData interface`](https://developer.mozilla.org/en-US/docs/Web/API/FormData) that contains the data the parent `<form>` is submitting. If there is no active submission or no parent `<form>`, it will be `null`.

- `method`: A string value of either `'get'` or `'post'`. This represents whether the parent `<form>` is submitting with either a `GET` or `POST` [HTTP method](https://developer.mozilla.org/en-US/docs/Web/HTTP/Methods). By default, a `<form>` will use the `GET` method and can be specified by the [`method`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form#method) property.

- `action`: A reference to the function passed to the `action` prop on the parent `<form>`. If there is no parent `<form>`, the property is `null`. If there is a URI value provided to the `action` prop, or no `action` prop specified, `status.action` will be `null`.

#### Caveats

- The `useFormStatus` Hook must be called from a component that is rendered inside a `<form>`.
- `useFormStatus` will only return status information for a parent `<form>`. It will not return status information for any `<form>` rendered in that same component or children components.
## Usage

### Display a pending state during form submission
### Read the form data being submitted
## Troubleshooting
### `status.pending` is never `true`

`useFormStatus` will only return status information for a parent `<form>`.

If the component that calls `useFormStatus` is not nested in a `<form>`, `status.pending` will always return `false`. Verify `useFormStatus` is called in a component that is a child of a `<form>` element.

`useFormStatus` will not track the status of a `<form>` rendered in the same component. See [Pitfall](https://react.dev/reference/react-dom/hooks/useFormStatus#useformstatus-will-not-return-status-information-for-a-form-rendered-in-the-same-component) for more details.