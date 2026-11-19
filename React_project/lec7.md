# React.memo, useMemo & useCallback Hooks

## 1. React.memo Hook

**# React memo hook??**

Whenever a new VDOM is created because of the increment in the values of the count, all the elements in the Sum component also get created in the new VDOM, which is not required.

In order to handle this, we use `React.memo`.

It memorizes whether a component has to render along with its parent or not.

If it is not using any props from its parent, it will not re-render along with its parent (provided its props remain unchanged).

Also, it compares the props received from the parent and checks whether the props are constant. If yes, then it doesn't re-render.

### Analogy

Imagine you are working on a project with two sections:

- Counter section
- Mathematics section

Whenever you increase the counter, only the counter section needs to change.

There is no need to calculate the mathematical section again and again if its data hasn't changed.

`React.memo` helps us avoid these unnecessary component re-renders.

### First file: App.jsx

```jsx
import { useState } from "react";
import Sum from "./Sum";

function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <h1>Counter: {count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <Sum />
    </>
  );
}

export default App;
```

### Second file: Sum.jsx

```jsx
import React from "react";

const Sum = React.memo(() => {
  function calculateSum() {
    let sum = 0;

    for (let i = 1; i <= 1000; i++) {
      sum += i;
    }

    return sum;
  }

  let total = calculateSum();

  console.log("Sum renderer");

  return (
    <>
      <h1>This is our math library</h1>
      <h2>Sum: {total}</h2>
    </>
  );
});

export default Sum;
```

### Explanation

- Initially, both App and Sum components render.
- When we click the Increment button, the count changes.
- The App component re-renders.
- But Sum does not re-render because its props haven't changed.
- `React.memo` remembers the previous result of the component rendering and skips unnecessary re-renders when props are unchanged.

**Important:** `React.memo` does not stop the parent from re-rendering. It only helps prevent unnecessary re-renders of the memoized child component.

---

## 2. When do we need to render a memoized component again?

**# Whenever props from the parent are dynamic and are dependent on the value of some state variable, the Sum function gets re-rendered due to a change in its value.**

### Example

```jsx
function App() {
  const [count, setCount] = useState(0);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <Sum number={count} />
    </>
  );
}
```

Now the Sum component receives `count` as a prop.

Whenever `count` changes:

1. App re-renders.
2. The `number` prop changes.
3. React.memo detects that the prop has changed.
4. Sum re-renders.

**Remember:**

`React.memo` compares the previous and new props using shallow comparison.

- If props are unchanged, it skips the re-render.
- If props change, it allows the component to re-render.

Note: `React.memo` also doesn't prevent re-renders caused by the component's own state changes or consumed context changes.

---

## 3. useMemo Hook

**# useMemo -> Memorizes the previous result so that the calculation is not required again and again until the value of the dependency array changes.**

`useMemo` is used to memorize the result of an expensive calculation.

### Analogy

Imagine you are solving a mathematical problem.

You calculate the answer once and write it down.

If the input values haven't changed, there is no need to solve the same problem again.

Similarly, `useMemo` stores the previous calculated result and reuses it until its dependencies change.

### Syntax of useMemo

```jsx
useMemo(() => {

}, []);
```

- First argument: A function that performs the calculation.
- Second argument: Dependency array that determines when the calculation should execute again.

### Example

```jsx
const [number, setNumber] = useState(100000);

const prime = useMemo(() => {

  let total = 0;

  if (number > 1)
    total++;

  for (let i = 3; i <= number; i++) {
    total++;

    for (let j = 2; j < i; j++) {
      if (i % j == 0) {
        total--;
        break;
      }
    }
  }

  return total;

}, [number]);
```

### Explanation

- Initially, `number = 100000`.
- The function calculates the number of prime numbers up to `number`.
- The result is stored in `prime`.
- If some other state variable changes, the calculation is not repeated.
- If `number` changes, the calculation executes again.

**Important:** This example uses a basic prime-counting algorithm and is computationally expensive for large numbers. The logic is kept simple for understanding `useMemo`.

### Another Simple Example

```jsx
const [count, setCount] = useState(0);
const [name, setName] = useState("Rohit");

const result = useMemo(() => {
  console.log("Calculating...");
  return count * 10;
}, [count]);
```

**Dry Run:**

| Action | count | name | Calculation |
|---|---:|---|---|
| Initial render | 0 | Rohit | Executes |
| Change name | 0 | Rahul | Doesn't execute |
| Increment count | 1 | Rahul | Executes |
| Change name | 1 | Amit | Doesn't execute |

This is because `useMemo` depends only on `count`.

---

## 4. useCallback Hook

**# useCallback hook ->**

Whenever a block re-renders, many functions are executed again and again. Their function objects are recreated, and since they are objects, they take some memory. So we use `useCallback` throughout the code so that the functions could be memorized.

`useCallback` memorizes the function definition itself instead of its result.

### Syntax

```jsx
const handleClick = useCallback(() => {
  console.log("Handle Click", count);
}, [count]);
```

### Explanation

- `useCallback` memorizes the function.
- If the dependencies remain unchanged, React returns the same function reference.
- If `count` changes, React creates and returns a new function.

### Analogy

Imagine you have a phone number saved in your contacts.

Instead of creating a new contact every time, you reuse the same saved contact.

Similarly, `useCallback` helps reuse the same function reference when its dependencies haven't changed.

### Example

```jsx
import { useState, useCallback, memo } from "react";

const Child = memo(({ handleClick }) => {
  console.log("Child rendered");

  return (
    <button onClick={handleClick}>
      Click Me
    </button>
  );
});

function App() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState("Rohit");

  const handleClick = useCallback(() => {
    console.log("Handle Click", count);
  }, [count]);

  return (
    <>
      <h1>{count}</h1>

      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <button onClick={() => setName("Rahul")}>
        Change Name
      </button>

      <p>{name}</p>

      <Child handleClick={handleClick} />
    </>
  );
}

export default App;
```

**Explanation:**

- When the name changes, App re-renders.
- Since `count` hasn't changed, `handleClick` retains the same reference.
- React.memo sees that the function prop hasn't changed.
- Therefore, Child doesn't need to re-render.
- When count changes, the function reference changes and Child re-renders.

---

## 5. useMemo with Objects and React.memo

**# When two objects are under two variables but they are different and React.memo thinks their reference is different and then allows them to re-render but the objects are same.**

In order to solve this problem, we use `useMemo`.

### Example

```jsx
const obj = useMemo(() => {
  return { name: "Rohit", age: 20 };
}, []);

<Post value={obj}></Post>
```

### Explanation

Consider this example without useMemo:

```jsx
function App() {
  const [count, setCount] = useState(0);

  const obj = {
    name: "Rohit",
    age: 20
  };

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <Post value={obj} />
    </>
  );
}
```

Whenever App re-renders:

- A new object is created.
- The new object has a different reference.
- Even though the values are the same, the reference is different.
- React.memo detects the changed prop and re-renders Post.

### Solution using useMemo

```jsx
const obj = useMemo(() => {
  return {
    name: "Rohit",
    age: 20
  };
}, []);
```

Now:

- The object is created during the initial render.
- Its reference is preserved between renders.
- When the parent re-renders, the object reference remains the same.
- React.memo can skip the unnecessary re-render of Post.

**Remember:** Objects and arrays are compared using their references during shallow prop comparison, not by comparing all their internal values.

---

## 6. Difference Between React.memo, useMemo and useCallback

| Feature | What does it memorize? | Used for |
|---|---|---|
| React.memo | Component rendering result | Preventing unnecessary component re-renders |
| useMemo | Calculated value | Avoiding expensive recalculations |
| useCallback | Function reference | Avoiding unnecessary function recreation |

### Easy Way to Remember

- `React.memo` -> Memorizes a component.
- `useMemo` -> Memorizes a value.
- `useCallback` -> Memorizes a function.

### Quick Comparison

```jsx
// React.memo
const Child = React.memo(({ name }) => {
  return <h1>{name}</h1>;
});

// useMemo
const result = useMemo(() => {
  return a + b;
}, [a, b]);

// useCallback
const handleClick = useCallback(() => {
  console.log("Hello");
}, []);
```

---

## 7. Important Notes

1. `React.memo` is useful when a component re-renders frequently with the same props.
2. `useMemo` is useful when a calculation is expensive.
3. `useCallback` is useful when passing functions to memoized child components or when a stable function reference is required.
4. All three are performance optimization tools.
5. They should not be used everywhere unnecessarily because memoization also has its own overhead.
6. `useMemo` and `useCallback` are performance optimizations, not guarantees that React will preserve cached values forever.
7. Always include the reactive values used inside their callbacks in the dependency array.

---

## 8. Quick Revision

```text
React.memo
    |
    ---> Memorizes component rendering result
    |
    ---> Compares previous and new props
    |
    ---> Skips rendering if props are unchanged


useMemo
    |
    ---> Memorizes calculated values
    |
    ---> Recalculates when dependencies change


useCallback
    |
    ---> Memorizes function references
    |
    ---> Returns a new function when dependencies change
```

### Final Analogy

Imagine a restaurant:

- `React.memo` -> Doesn't prepare the same dish again if the order hasn't changed.
- `useMemo` -> Remembers the result of a calculation instead of calculating it again.
- `useCallback` -> Reuses the same saved instruction instead of creating a new one.

**The main purpose of all three is to avoid unnecessary work and improve React application performance.**