---
tags:
  - hooks
  - react
links:
  - https://react.dev/reference/react/useId
  - https://youtu.be/GNVI9Pr_RKQ?si=vkkzq6XA6zh7wwnk
---
`useId` is a React Hook for generating unique IDs that can be passed to accessibility attributes.

```js
const id = useId()
```
## Reference
### `useId()`
Call `useId` at the top level of your component to generate a unique ID:

```jsx
import { useId } from 'react';

function PasswordField() {  
	const passwordHintId = useId();  
	// ...
```

#### Parameters
`useId` does not take any parameters.

#### Returns

`useId` returns a unique ID string associated with this particular `useId` call in this particular component.

#### Caveats 
- `useId` is a Hook, so you can only call it **at the top level of your component** or your own Hooks. You can’t call it inside loops or conditions. If you need that, extract a new component and move the state into it.
    
- `useId` **should not be used to generate keys** in a list. [Keys should be generated from your data.](https://react.dev/learn/rendering-lists#where-to-get-your-key)
## Usage
### Generating unique IDs for accessibility attributes
### Generating IDs for several related elements
### Specifying a shared prefix for all generated IDs
```jsx
import { createRoot } from 'react-dom/client';
import App from './App.js';
import './styles.css';

const root1 = createRoot(document.getElementById('root1'), {
  identifierPrefix: 'my-first-app-'
});
root1.render(<App />);

const root2 = createRoot(document.getElementById('root2'), {
  identifierPrefix: 'my-second-app-'
});
root2.render(<App />);
```
### Using the same ID prefix on the client and the server