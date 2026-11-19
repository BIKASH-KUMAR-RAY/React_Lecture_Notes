# React DOM, Virtual DOM, Diffing Algorithm & React Fiber

## 1. How does the browser render a webpage?

```text
HTML  ---> DOM Tree  ----\
                         \
                          ---> Render Tree ---> Reflow ---> Repaint
                         /
CSS   ---> CSSOM Tree ---/
```

### Explanation

- **HTML -> DOM Tree:** The browser converts HTML elements into a DOM (Document Object Model) tree.
- **CSS -> CSSOM Tree:** The browser converts CSS rules into a CSSOM (CSS Object Model) tree.
- **Render Tree:** DOM and CSSOM are combined to create the render tree. It contains the elements that need to be displayed.
- **Reflow:** The browser calculates the position, height, width, and layout of elements.
- **Repaint:** The browser paints the pixels on the screen, including colors, borders, backgrounds, etc.

**Remember:**

- DOM = Structure of the webpage.
- CSSOM = Styling information.
- Render Tree = Elements that need to be displayed along with their styles.
- Reflow = Calculation of layout.
- Repaint = Painting pixels on the screen.

---

## 2. How is React able to manipulate the DOM so efficiently?

-> Pehle React real DOM mai changes nahi karta hai directly. Woh pehle detect karta hai ki kin elements pe change karna hai and then real DOM mai change karta hai.

React uses the concept of the **Virtual DOM** and a process called **Reconciliation** to determine what changes need to be made to the real DOM.

### Detection

-> React creates a Virtual DOM (that is a representation of the real DOM but consists of lightweight elements).

**Virtual DOM:**
- It is a lightweight JavaScript representation of the UI.
- It contains information about elements, their properties, and children.
- It helps React calculate what changes are required.

### Analogy

Imagine you have written a document containing 100 lines.

You change only line number 50.

Instead of rewriting the entire document, you compare the old document with the new document and update only the changed part.

Similarly, React compares its previous and new UI representations to determine what needs to change.

### How does it work?

**## Whenever there is a change in any of the values, one more Virtual DOM is created. After that, both the Virtual DOM trees are compared. Whenever there is a change in any of the elements in React, it is detected and then sent to ReactDOM in order to change the real DOM, and the new VDOM is kept and the unchanged one is deleted.**

### Flow

1. Initially, React creates the UI.
2. A state or prop changes.
3. React renders the component again and creates a new UI representation.
4. React compares the new representation with the previous one.
5. The required changes are committed to the real DOM.

**Important terms:**

- **Rendering:** React calls the component function to calculate the next UI.
- **Reconciliation:** React compares the previous UI representation with the new one.
- **Commit phase:** React applies the necessary changes to the real DOM.

**Whenever a new VDOM is created, only the parts in which changes occur are updated in the real DOM and not the whole DOM.**

Note: React may execute a component function again during rendering, but that does not mean the entire real DOM is recreated.

---

## 3. Primitive vs Non-Primitive Data Types in React

React uses state update comparisons to determine whether a state value has changed. It uses `Object.is()` to compare the previous and next state values.

### Example 1: Primitive Data Type

```jsx
function App() {
  const [count, setCount] = useState(0);

  function handleChange() {
    setCount(0);
  }
}
```

Here, the value didn't change and hence the real DOM doesn't change because primitive data types are detected on the basis of their values.

**Explanation:**

- Initially, `count = 0`.
- We call `setCount(0)` again.
- The previous and new values are the same.
- React can skip the re-render because the state hasn't changed.

### Example 2: Non-Primitive Data Type

```jsx
function App() {
  const [count, setCount] = useState([10, 20, 30]);

  function handleChange() {
    count.push(99);
    setCount(count);
  }
}
```

Whereas non-primitive data type changes are detected on the basis of their references.

Here, the values don't change their reference since the reference remains the same.

**Explanation:**

- Initially, `count` points to an array in memory.
- `count.push(99)` modifies the existing array.
- The reference of the array remains the same.
- `setCount(count)` receives the same reference.
- React may skip the re-render because the reference hasn't changed.

### To reflect changes, do this:

```jsx
setCount([...count, 40]);
```

Since the reference changes and the array changes, React detects the new state.

**Important:** We use the spread operator (`...`) to create a new array instead of modifying the existing array.

### Memory Analogy

Imagine an array is a house.

- Reference = Address of the house.
- Values = Things inside the house.

If you change the furniture inside the same house, the address remains the same.

If you create a new house with the updated furniture, the address changes.

Similarly, React detects changes in arrays and objects using their references.

**Remember:**

Never directly modify state arrays or objects. Always create a new array or object when updating state.

---

## 4. Diffing Algorithm

**## diffing algo --> Difference between the VDOM before changes and the new VDOM.**

The children are differentiated on the basis of their respective positions.

A child consists of two subparts which have dynamic as well as static data. It detects both the values and only detects in the dynamic part that the values have changed.

### Explanation

The Diffing Algorithm is the process React uses to compare the previous and new UI representations.

For example:

Previous UI:

```jsx
<div>
  <h1>Hello</h1>
  <p>Welcome</p>
</div>
```

New UI:

```jsx
<div>
  <h1>Hello</h1>
  <p>Good Morning</p>
</div>
```

React detects that only the text inside the paragraph has changed.

It updates that part instead of recreating the entire DOM structure.

### Important Rules of Diffing Algorithm

1. If the element types are different, React generally replaces the old element with a new one.
2. If the element types are the same, React updates the changed properties.
3. For children, React uses their positions and keys to identify corresponding elements.

---

## 5. Key in React

**## Key -> It is given to the dynamic child and is unique (don't use index number).**

Keys help React identify which child has been added, removed, or changed.

### Example

```jsx
function List() {
  const [foodItems, setFoodItems] = useState([
    "Apple",
    "Banana",
    "Orange"
  ]);

  function handleChange() {
    setFoodItems(["Mango", ...foodItems]);
  }

  return (
    <>
      <button onClick={handleChange}>Increment</button>
      <Food foods={foodItems} />
    </>
  );
}
```

### Another file: Food.jsx

```jsx
function Food({ foods }) {
  return (
    <>
      <ul>
        {foods.map(food => <li key={food}>{food}</li>)}
      </ul>
    </>
  );
}
```

**All the children are compared on the basis of their respective positions and that may cause several more changes in the real DOM, so instead we can use keys which can then be used to compare on their respective keys and which reduces the number of changes.**

### Why do we need keys?

Imagine the previous list is:

```text
Apple
Banana
Orange
```

Now we add Mango at the beginning:

```text
Mango
Apple
Banana
Orange
```

Without keys, React may associate children based on their positions and do unnecessary updates.

With keys, React can identify the existing items and understand that Mango is a new item added at the beginning.

### Important: Why should we not use index as a key?

Example:

```jsx
{foods.map((food, index) => (
  <li key={index}>{food}</li>
))}
```

Suppose the original list is:

```text
Index 0 -> Apple
Index 1 -> Banana
Index 2 -> Orange
```

After adding Mango at the beginning:

```text
Index 0 -> Mango
Index 1 -> Apple
Index 2 -> Banana
Index 3 -> Orange
```

Now the indexes of the existing elements have changed.

React may associate the wrong items with the previous elements, which can cause problems, especially when list items contain input fields or their own state.

**Therefore, use a unique and stable ID whenever possible.**

Example:

```jsx
{foods.map(food => (
  <li key={food.id}>{food.name}</li>
))}
```

**Note:** Using an array index is acceptable for a static list that never changes order and never has items inserted or removed. But stable unique IDs are generally preferred.

---

## 6. React Fiber

**## React Fiber: Optimized VDOM. Why?**

React Fiber is the internal reconciliation architecture of React.

It was introduced to make React's rendering process more flexible and efficient.

### Why do we need React Fiber?

Earlier, React's reconciliation process was more synchronous.

If React had a large number of components to process, it could take a long time to complete the rendering work, potentially making the UI less responsive.

React Fiber helps solve this problem.

### Main Features of React Fiber

1. **Incremental Rendering:** React can divide rendering work into smaller units.
2. **Prioritization:** React can prioritize important updates over less important updates.
3. **Pause and Resume:** React can pause some rendering work and continue it later.
4. **Better Responsiveness:** It helps keep the UI responsive during complex rendering tasks.

### Analogy

Imagine a teacher has 100 answer sheets to check.

Instead of checking all 100 sheets continuously without stopping, the teacher checks a few sheets, handles an urgent task, and then continues checking.

Similarly, React Fiber allows React to divide rendering work into smaller units and manage the priority of that work.

### Important Concept

React Fiber uses two important phases:

**1. Render Phase**

- React calculates what changes are required.
- It performs reconciliation.
- This phase can be interrupted and restarted.

**2. Commit Phase**

- React applies the necessary changes to the real DOM.
- This phase is synchronous and cannot be interrupted in the middle.

**Remember:**

React Fiber is not another Virtual DOM. It is the architecture that helps React manage and schedule its rendering and reconciliation work.

---

## 7. Quick Revision

| Concept | Meaning |
|---|---|
| DOM | Tree representation of HTML elements |
| CSSOM | Tree representation of CSS styles |
| Render Tree | Combines DOM and CSSOM information for rendering |
| Reflow | Calculates layout and positions |
| Repaint | Paints pixels on the screen |
| Virtual DOM | Lightweight JavaScript representation of UI |
| Reconciliation | Compares previous and new UI representations |
| Diffing Algorithm | Determines the differences between UI representations |
| Key | Uniquely identifies list children |
| React Fiber | Architecture for managing and scheduling rendering work |

### Remember the Overall Flow

```text
State / Props Change
        |
        v
Component Re-renders
        |
        v
New UI Representation
        |
        v
Reconciliation
        |
        v
Diffing Algorithm
        |
        v
Required Changes Identified
        |
        v
Commit Phase
        |
        v
Real DOM Updated
```

**One-line summary:**

React uses Virtual DOM, reconciliation, keys, and Fiber architecture to efficiently calculate, identify, and apply UI changes without unnecessarily updating the entire real DOM.