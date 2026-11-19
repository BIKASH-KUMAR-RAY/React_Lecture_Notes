# React `useEffect` Hook

## What is useEffect Hook?

-> `useEffect` hook is used when we want to fetch values and we dont want to re-render our functions again and again.

`useEffect` is used to perform side effects in React, such as:

- Fetching data from an API.
- Updating the document title.
- Setting timers.
- Adding event listeners.

### Analogy

Imagine you are ordering food from a restaurant.

- React component is the customer.
- API is the restaurant.
- `useEffect` is the waiter who performs the task after the component renders.

We don't need to order food every time we sit down. Similarly, `useEffect` allows us to perform tasks at specific times instead of executing them on every render.

### Example: Fetching GitHub Users

```jsx
import { useEffect, useState } from "react";

function App() {
  let [users, setUsers] = useState([]);

  useEffect(() => {
    async function GithubProfile() {
      const response = await fetch("https://api.github.com/users");
      const data = await response.json();

      setUsers(data);
      console.log("Hello");
    }

    GithubProfile();
  }, []);

  return (
    <>
      {users.map((user) => (
        <p key={user.id}>{user.login}</p>
      ))}
    </>
  );
}

export default App;
```

### Explanation

- `useState([])` creates a state variable called `users`.
- `useEffect()` is used to perform the API call.
- `fetch()` requests data from the GitHub API.
- `response.json()` converts the response into JavaScript data.
- `setUsers(data)` updates the state.
- When the state changes, React re-renders the component to display the users.

**Important:** `useEffect` itself does not prevent re-rendering. It controls when the side effect executes.

---

## # Whatever is written in useEffect hook gets executed in the last and not in the beginning as it is used for api callings and fetching data from outside server and all.

-> `useEffect` executes after React has rendered the component and updated the DOM.

### Flow

1. React component renders.
2. React updates the DOM.
3. `useEffect` executes.
4. API data is fetched.
5. State changes using `setUsers(data)`.
6. React renders again to display the updated data.

### Analogy

Think of building a house:

- First, the house is built (render).
- Then, furniture is arranged (effect).
- If new furniture arrives, the house interior is updated (re-render).

---

## # useEffect Syntax

```jsx
useEffect(() => {

}, []);
```

**# The empty array here in the last ensures that useEffect runs only once and not again and again and use Effect runs only once when this empty array is given.**

### Dependency Array

**# []-> The empty array here is called the dependency array here which means that useEffect depends on it for how many time it will run .**

The dependency array determines when the effect should execute again.

### Three Types of useEffect

#### 1. Without Dependency Array

```jsx
useEffect(() => {
  console.log("Hello");
});
```

-> Runs after every render.

#### 2. With Empty Dependency Array

```jsx
useEffect(() => {
  console.log("Hello");
}, []);
```

-> Runs after the initial render (once per mount in normal production behavior).

#### 3. With Dependency Array Containing a Variable

```jsx
useEffect(() => {
  console.log("Hello");
}, [count]);
```

-> Runs after the initial render and whenever `count` changes.

---

## # If we want to re render the function using useEffect then we have to insert the value of of the state changing variable inside the dependency array.

### Example

```jsx
const [count, setCount] = useState(0);

useEffect(() => {

}, [count]);
```

-> Here, `count` is inserted inside the dependency array.

Whenever the value of `count` changes, `useEffect` executes again.

### Complete Example: Counter

```jsx
import { useState, useEffect } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return (
    <>
      <h1>Count: {count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </>
  );
}

export default App;
```

### Dry Run

| Action | Count | useEffect |
|---|---|---|
| Initial render | 0 | Executes |
| Click Increase | 1 | Executes |
| Click Increase | 2 | Executes |
| Click Increase | 3 | Executes |

**Explanation:**

- Initially, `count = 0`.
- `useEffect` executes after the initial render.
- When we click the button, `setCount()` updates the state.
- React re-renders the component.
- Since `count` has changed, `useEffect` executes again.

---

## # Important Concept: useEffect vs useState

| useState | useEffect |
|---|---|
| Used to manage state | Used to perform side effects |
| State changes cause re-render | Executes based on dependencies |
| Example: Counter value | Example: API fetching |

**Remember:**

- `useState` -> Stores and updates data.
- `useEffect` -> Performs tasks after rendering.
- Dependency array -> Controls when the effect executes.

---

## # Cleanup Function in useEffect

-> Sometimes, we need to clean up an effect when a component is removed or before an effect runs again.

For example:
- Clearing a timer.
- Removing event listeners.
- Cancelling or ignoring outdated requests.

### Example: setInterval

```jsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);

    return () => {
      clearInterval(timer);
    };
  }, []);

  return <h1>{count}</h1>;
}

export default App;
```

### Explanation

- `setInterval()` executes the callback every 1 second.
- `clearInterval()` stops the timer.
- The cleanup function is returned from `useEffect`.
- React calls the cleanup function when the component unmounts.

### Syntax

```jsx
useEffect(() => {
  // Side effect

  return () => {
    // Cleanup
  };
}, []);
```

**Analogy:**

When you leave a room, you switch off the lights.

Similarly, when a component is removed, we clean up the tasks that were started by that component.

---

## # Important Note: React Strict Mode

In React development mode, when `StrictMode` is enabled, React may execute your effect twice during the initial mount to help identify bugs.

```jsx
useEffect(() => {
  console.log("Hello");
}, []);
```

You may see `"Hello"` printed twice in the console during development.

This is normal. It generally does not happen in the same way in production.

---

## # Quick Revision

- `useEffect` is used for side effects such as API calls, timers, subscriptions, and event listeners.
- `useEffect` executes after rendering.
- `useEffect(() => {})` -> Runs after every render.
- `useEffect(() => {}, [])` -> Runs after initial mount.
- `useEffect(() => {}, [count])` -> Runs after initial mount and when `count` changes.
- Updating state inside an effect can cause another render.
- Always include the reactive values used inside the effect in its dependency array.
- Cleanup functions are used to stop or remove effects when they are no longer needed.

### One-Line Analogy

`useEffect` is like a watchman who checks whether a particular thing has changed and performs a task whenever that condition is met.
