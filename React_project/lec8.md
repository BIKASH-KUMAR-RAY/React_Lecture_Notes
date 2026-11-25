# USEREF HOOKS

`useRef` is a React Hook that allows us to store values that persist between renders without causing a re-render when their values change.

**Important points:**
- These are created only once, not again and again when re-rendered.
- When the function is re-rendered, the ref object is not created again. Hamesha purana ref object hi use hoga.
- It is initialized only once for a particular component instance.
- It stores its value inside the `.current` property.
- Changing the `.current` value does not trigger a re-render.
- Unlike state, refs are mainly used for values that don't need to be displayed directly in the UI.

## 1. Using useRef to Store an Interval ID

**Example ->**

```jsx
import { useRef, useState } from "react";

function Timer() {
    const [time, settime] = useState(0);

    let intervalidRef = useRef(null);

    function handleStart() {
        intervalidRef.current = setInterval(() => {
            settime(prevTime => prevTime + 1);
        }, 1000);
    }

    function handleStop() {
        clearInterval(intervalidRef.current);
    }

    function handlereset() {
        clearInterval(intervalidRef.current);
        settime(0);
    }

    return (
        <>
            <h1>{time}</h1>
            <button onClick={handleStart}>Start</button>
            <button onClick={handleStop}>Stop</button>
            <button onClick={handlereset}>Reset</button>
        </>
    );
}

export default Timer;
```

**Explanation:**

- Initially, `intervalidRef.current` contains `null`.
- When `handleStart()` is called, `setInterval()` returns an interval ID.
- We store that ID inside `intervalidRef.current`.
- Even when the component re-renders, the ref object is preserved.
- Therefore, we can access the previously stored interval ID instead of getting `null` again.
- `handleStop()` uses that ID to stop the interval.

**Important logic:**

The interval ID is stored in the ref because we need to access it later, but changing the interval ID should not cause a re-render.

**One important thing:**

In the interval callback, we use:

```jsx
settime(prevTime => prevTime + 1);
```

Instead of:

```jsx
settime(time + 1);
```

This is because the callback created by `setInterval` can capture an old value of `time` (stale closure).

Using the functional updater ensures that we always get the latest state value.

**Note:** In this example, clicking Start multiple times can create multiple intervals. We can prevent this by checking whether an interval is already running.

---

## 2. Login.jsx – Accessing Input Values Using useRef

Here, we see another application of `useRef`.

```jsx
import { useRef } from "react";

function Login() {
    // const [email,setEmail] = useState("");
    // const [password,setpassword] = useState("");

    const emailRef = useRef(null);
    const passref = useRef(null);

    console.log("render");

    function handleSubmit(e) {
        e.preventDefault();

        console.log(emailRef.current.value);
        console.log(passref.current.value);
    }

    return (
        <>
            <form onSubmit={handleSubmit}>
                <input type="email" ref={emailRef}></input>

                <input type="password" ref={passref}></input>

                <button type="submit">Submit</button>
            </form>
        </>
    );
}

export default Login;
```

Here it shows another application of `useRef`.

**Explanation:**

The problem here is that whenever we insert new values into an input field, if we use `useState` with `onChange`, it renders again and again.

So, instead, we give it to the browser so that it can handle the input values, and the function doesn't re-render with every new input value.

Here, we give it to the real DOM (browser), which handles this. We point to those DOM elements using the variables created by `useRef` in order to get their values.

**How does it work?**

1. We create references using `useRef(null)`.
2. We attach those references to the input fields using the `ref` attribute.
3. React connects the references to the actual DOM elements.
4. We access the input values using `.current.value`.
5. Whenever the user submits the form, we get the latest input values.

**Important logic:**

```jsx
emailRef.current.value
```

- `emailRef` is the reference object.
- `.current` points to the actual input DOM element.
- `.value` gives us the value entered by the user.

**Note:** Using `useRef` here means we don't need React state to track every keystroke. The browser handles the input values directly.

However, if we need to display live input values or perform validation while typing, `useState` is generally more suitable.

---

## 3. Creating a Video Player (Video.jsx)

**Important note ->** Always keep the video and big files in the `public` folder in React when you want to access them directly using a URL.

Here, `useRef` gives us control over this video element.

```jsx
import { useRef } from "react";

function Video() {
    const videoRef = useRef(null);

    function handleStart() {
        videoRef.current.play();
    }

    function handleStop() {
        videoRef.current.pause();
    }

    function handleRestart() {
        videoRef.current.currentTime = 0;
    }

    return (
        <>
            <video
                ref={videoRef}
                src="/strike.mp4"
                width="600"
                height="400"
            ></video>

            <div>
                <button onClick={handleStart}>Start</button>
                <button onClick={handleStop}>Pause</button>
                <button onClick={handleRestart}>Restart</button>
            </div>
        </>
    );
}

export default Video;
```

**Explanation:**

- Initially, `videoRef.current` is `null`.
- We attach the reference to the video element using `ref={videoRef}`.
- React connects the reference to the actual video DOM element.
- Now, `videoRef.current` gives us direct access to that video element.
- We can use JavaScript's built-in video methods and properties to control it.

**Important methods and properties:**

| Method / Property | Purpose |
|---|---|
| `videoRef.current.play()` | Plays the video |
| `videoRef.current.pause()` | Pauses the video |
| `videoRef.current.currentTime = 0` | Restarts the video from the beginning |
| `videoRef.current.duration` | Gives the total duration of the video |
| `videoRef.current.volume = 0.5` | Sets the volume to 50% |
| `videoRef.current.muted = true` | Mutes the video |

**Note:** In React, event names are case-sensitive. Always use `onClick`, not `onclick`.

---

## 4. Difference Between useState and useRef

| useState | useRef |
|---|---|
| Stores values between renders | Stores values between renders |
| Updating state triggers a re-render | Updating `.current` does not trigger a re-render |
| Used when UI needs to update | Used when UI does not need to update |
| Returns a state value and setter function | Returns an object with a `.current` property |
| React manages state updates | We can directly modify the `.current` value |

**Remember:**

- `useState` → When changing a value should update the UI.
- `useRef` → When we want to preserve a value or access a DOM element without triggering a re-render.

---

## 5. Difference Between useRef and Normal Variables

Consider the following example:

```jsx
import { useRef, useState } from "react";

function Example() {
    let count = 0;

    const countRef = useRef(0);

    const [stateCount, setStateCount] = useState(0);

    function handleClick() {
        count++;
        countRef.current++;
        setStateCount(stateCount + 1);
    }

    console.log(count, countRef.current, stateCount);

    return (
        <>
            <h2>Normal Variable: {count}</h2>
            <h2>Ref: {countRef.current}</h2>
            <h2>State: {stateCount}</h2>

            <button onClick={handleClick}>Click</button>
        </>
    );
}

export default Example;
```

**Explanation:**

### Normal Variable
- A normal variable is initialized again whenever the component re-renders.
- Its previous value is not preserved between renders.

### useRef
- Its value is preserved between renders.
- Changing `.current` does not trigger a re-render.
- Therefore, the updated ref value will not automatically appear in the UI.

### useState
- Its value is preserved between renders.
- Updating it triggers a re-render.
- Therefore, the updated state value appears in the UI.

**Important:** If we click the button, `setStateCount()` triggers a re-render. During that render, the normal variable `count` is initialized again with `0`, but `countRef.current` retains its updated value.

---

## 6. Important Points to Remember

1. `useRef` returns an object containing a `.current` property.
2. The same ref object is preserved between renders of the same component instance.
3. Updating `.current` does not trigger a re-render.
4. We can use `useRef` to access DOM elements directly.
5. We can use it to store values such as interval IDs, timeout IDs, and previous values.
6. We should not use refs when we want changes to automatically update the UI.
7. React assigns the DOM element to `.current` after the element is mounted.
8. When a component is unmounted, React resets DOM refs to `null`.

**Final takeaway:**

`useRef` is mainly used for two purposes:

- **Storing values:** Values that should persist between renders without causing a re-render.
- **DOM manipulation:** Accessing DOM elements directly to perform operations such as playing videos, focusing inputs, scrolling, etc.