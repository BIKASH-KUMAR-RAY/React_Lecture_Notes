# React Hooks – `useState`

## 1. Why Should We Avoid Manual DOM Manipulation in React?

**Hame khudse kuch bhi manipulate nahi karna DOM mai, React DOM karega.**

```jsx
count++;
const para = document.querySelector('p');
para.textContent = `Counter:${count}`;
```

**Explanation:**

Yaha hm khudse change kar rahe hai values `count` ka, joki hmko manually nahi karna hai. React ko batana hai bas, baaki woh React DOM se change karwaye jaha bhi pure files mai `count` variable ka value change ho raha hai.

In the above code, we are manually selecting a paragraph using `document.querySelector()` and changing its content using `textContent`.

React mein humein generally manually DOM manipulate karne ki zaroorat nahi hoti. We update the state, and React updates the UI accordingly.

## 2. What Are Hooks?

**The above thing is done by Hooks.**

Hooks are nothing but the functions given by React.

Hooks allow us to use React features inside functional components, such as:

- `useState` – to manage state.
- `useEffect` – to perform side effects.
- `useRef` – to store references to DOM elements or values.
- `useContext` – to access context values.

## 3. How to Use Hooks?

### Step 1: Import `useState`

```jsx
import { useState } from "react";
```

### Step 2: Initialize the State Variable

```jsx
let [count, setCount] = useState(0);
```

- `useState(0)` initializes the state with the value `0`.
- `count` stores the current value of the state.
- `setCount` is the function used to update the state.

`useState` ek array deta hai jismai variable likho pehle mai and then dusre mai koi bhi naam ka function.

`count` mai `0` chala jayega jo `useState` mai daala hua hai, and then hm jaha bhi changes karenge waha yeh function `setCount` mai count ka value daal dena hai taaki React ko indication mile ki har value jaha change ho raha hai kar do.

More precisely, `useState` returns an array containing two values:

1. The current state value.
2. A function to update that state.

We usually use array destructuring to access these values.

### Step 3: Update the State

```jsx
count++;
setCount(count);
```

`setCount` is used to update the state and trigger a re-render of the component.

However, instead of modifying `count` directly, it is better to update the state like this:

```jsx
setCount(count + 1);
```

This tells React to update the state to the new value.

## 4. Complete Example of `useState`

```jsx
import { useState } from "react";

function App() {
    const [count, setCount] = useState(0);

    return (
        <div>
            <p>Counter: {count}</p>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>
        </div>
    );
}

export default App;
```

### Explanation

1. Initially, `count` is `0`.
2. React displays `Counter: 0`.
3. When the button is clicked, `setCount(count + 1)` is called.
4. The state updates from `0` to `1`.
5. React re-renders the component and displays `Counter: 1`.

Every time we click the button, the counter increases by one.

**The main idea:** We update the state, and React takes care of updating the UI.

## 5. Important Logic: State Update and Re-rendering

### What is re-rendering?

Re-rendering means React runs the component function again to calculate what the UI should look like with the updated state.

For example:

```jsx
const [count, setCount] = useState(0);

return (
    <button onClick={() => setCount(count + 1)}>
        {count}
    </button>
);
```

Initially, the button displays `0`.

After clicking it, the state becomes `1`, and the button displays `1`.

React updates the UI based on the new state.

### Why not use a normal variable?

```jsx
let count = 0;

function increase() {
    count++;
}
```

A normal variable does not automatically tell React to re-render the component when its value changes.

Therefore, changing a normal variable alone is not sufficient when we want the UI to reflect that change.

Using `useState` allows React to track the state and update the UI when necessary.

## 6. Functional State Updates

When the new state depends on the previous state, we can use a function inside the state setter.

```jsx
setCount(prevCount => prevCount + 1);
```

Here, `prevCount` represents the previous state value.

This approach is useful when multiple state updates may be queued before the component re-renders.

For example:

```jsx
const [count, setCount] = useState(0);

function increase() {
    setCount(prevCount => prevCount + 1);
}
```

Each call calculates the next value from the previous state.

## 7. Important Points to Remember

- `useState` is a React Hook.
- `count` stores the current state value.
- `setCount` updates the state and can trigger a re-render.
- `useState(0)` initializes the state with `0`.
- Do not directly modify the state variable using operations such as `count++`.
- Use `setCount(count + 1)` or `setCount(prevCount => prevCount + 1)` to update the counter.
- React handles the UI update based on the new state; manual DOM manipulation is generally unnecessary in React.
- Hooks such as `useState` must be called at the top level of a React function component or a custom Hook, not inside loops, conditions, or nested functions.

## Quick Revision

```jsx
import { useState } from "react";

const [count, setCount] = useState(0);

// Update using the current state value
setCount(count + 1);

// Update using the previous state
setCount(prevCount => prevCount + 1);
```

**Remember:**

`useState` → stores state.

`setCount` → updates state.

State update → React re-renders the component when needed.

Re-render → React updates the UI to reflect the new state.
