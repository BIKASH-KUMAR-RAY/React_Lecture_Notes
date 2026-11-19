JSX (JavaScript XML)
Concept: JSX looks like HTML, but it allows us to write HTML-like structures directly inside JavaScript.

Flow of Execution:

JSX 
Babel

​
 React.createElement 
React

​
 React Element (JS Object) 
ReactDOM

​
 REAL DOM (HTML Element)
Babel (JavaScript Compiler)
Babel is a JavaScript compiler. It takes our JSX code and converts it into standard JavaScript code using React.createElement:

JSX Code:

```JavaScript
<h1 id="title">Hello Coder army</h1>
```
Babel Compilation:

JavaScript
const element = React.createElement('h1', { id: 'title' }, "Hello Coder Army");
Execution Process:

Babel compiles JSX into React.createElement(...).

React creates a React Element (which is a JavaScript object).

React passes this object to ReactDOM.

ReactDOM converts it into a Real DOM element (HTML Element) on the webpage.

JSX Rules & Syntax Notes
className instead of class:
In JSX, we write className instead of class because class is a reserved keyword in JavaScript.

JavaScript
const element = <h1 id="title" className="first">Hello Coder army</h1>;
Single Root Element Requirement:
Instead of writing multiple top-level elements like <h1> and <h2> separately, wrap them inside a single root tag such as a <div> (or React Fragment <>...</>).

```JavaScript
const element3 = (
  <div>
    <h1>Hello Coder army</h1>
    <h2>Kaise ho </h2>
  </div>
);
```
React Components
A React Component is simply a JavaScript function that returns JSX.

```JavaScript
function App() {
  return (
    <h1>hello coder army</h1> // JSX
  );
}
```

const root = ReactDOM.createRoot(document.getElementById('root'));

// Render either by calling the function or using JSX component syntax:
root.render(App());
// OR
root.render(<App />);
Embedding JavaScript Expressions in JSX
You can write any valid JavaScript expression inside JSX using curly braces {}.

1. Passing Parameters to Functions:
```JavaScript
function App(name) {
  return (
    <h1>hello coder army {name}</h1>
  );
}

const a = App("Rohit");

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(a);
```
2. Ternary Operators / Conditional Logic:
```JavaScript
const age = 10;

const element = <h1>Hello Coder {age > 12 ? "Adult" : "Kid"}</h1>;

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(element);
```
3. Rendering Arrays using .map():
```JavaScript
const courses = ["HTML", "CSS", "JS", "React"];

const element = (
  <ul>
    {courses.map((course) => (
      <li key={course}>{course}</li>
    ))}
  </ul>
);

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(element);
```
Props in React
Whatever attributes we pass as arguments to a component in JSX get converted into a JavaScript object. We receive this object in the function parameter as props.

```JavaScript
function App(props) {
  return (
    <div>
      <h1>Hello Coder Army</h1>
      <p>Name: {props.name}</p>
      <p>Age: {props.age}</p>
    </div>
  );
}

// Passed attributes are converted into an object: { name: "Rohit", age: 30 }
const element = <App name="Rohit" age={30} />;

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(element);
Additional Example — Props Destructuring (Bonus Reference):
JavaScript
function UserCard({ name, age }) {
  return (
    <div>
      <h2>User: {name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}

const element = <UserCard name="Rohit" age={30} />;
```