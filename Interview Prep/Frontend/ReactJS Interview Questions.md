# ReactJS Interview Questions

**[Read this first](../../Development/ReactJS.md)**

- [ReactJS Interview Questions](#reactjs-interview-questions)
  - [Hooks with examples](#hooks-with-examples)
    - [useState](#usestate)
    - [useEffect](#useeffect)
    - [useRef](#useref)
      - [Accessing DOM Elements](#accessing-dom-elements)
    - [useMemo](#usememo)
    - [Custom Hooks](#custom-hooks)
  - [Component lifecycle in functional components](#component-lifecycle-in-functional-components)
  - [Create folder ui structure](#create-folder-ui-structure)
  - [React Router, Context API, Redux](#react-router-context-api-redux)

## Hooks with examples

### useState

The React `useState` Hook allows us to track state in a function component. State generally refers to data or properties that need to be tracking in an application.

ex.

```jsx
import { useState } from "react";

export default function App() {
	const [count, setCount] = useState(0);

	return (
		<div>
			{count}
			<button onClick={() => setCount((count) => count + 1)}>
				Increase Count{" "}
			</button>
		</div>
	);
}
```

### useEffect

-   The `useEffect` Hook allows you to perform side effects in your components.

-   Some examples of side effects are: fetching data, directly updating the DOM, and timers.

-   `useEffect` accepts two arguments. The second argument is optional.
    -   `useEffect(callbackFunction, [dependencyArray])`

ex.

```jsx
useEffect(() => {
	axios
		.get("http://localhost:8000/blogs")
		.then((res) => {
			setBlogs(res.data);
		})
		.catch((err) => {
			console.log(err);
		});
	// will only run once because of this dependency array
	// fetch data only once
}, []);
```

### useRef

-   The `useRef` Hook allows you to persist values between renders.

-   It can be used to store a mutable value that does not cause a re-render when updated.

-   It can be used to access a DOM element directly.

ex.

```jsx
import { useState, useEffect, useRef } from "react";
import ReactDOM from "react-dom/client";

function App() {
	const [inputValue, setInputValue] = useState("");
	const count = useRef(0);

	useEffect(() => {
		count.current = count.current + 1;
	});

	return (
		<>
			<input
				type="text"
				value={inputValue}
				onChange={(e) => setInputValue(e.target.value)}
			/>
			<h1>Render Count: {count.current}</h1>
		</>
	);
}
```

#### Accessing DOM Elements

```jsx
import { useRef } from "react";
import ReactDOM from "react-dom/client";

function App() {
	const inputElement = useRef();

	const focusInput = () => {
		inputElement.current.focus();
	};

	return (
		<>
			<input type="text" ref={inputElement} />
			<button onClick={focusInput}>Focus Input</button>
		</>
	);
}
```

### useMemo

-   The React `useMemo` Hook returns a memoized value.

-   The `useMemo` Hook only runs when one of its dependencies update.

ex. of a poor performing function

```jsx
import { useState } from "react";
import ReactDOM from "react-dom/client";

const App = () => {
	const [count, setCount] = useState(0);
	const [todos, setTodos] = useState([]);
	const calculation = expensiveCalculation(count);

	const increment = () => {
		setCount((c) => c + 1);
	};
	const addTodo = () => {
		setTodos((t) => [...t, "New Todo"]);
	};

	return (
		<div>
			<div>
				<h2>My Todos</h2>
				{todos.map((todo, index) => {
					return <p key={index}>{todo}</p>;
				})}
				<button onClick={addTodo}>Add Todo</button>
			</div>
			<hr />
			<div>
				Count: {count}
				<button onClick={increment}>+</button>
				<h2>Expensive Calculation</h2>
				{calculation}
			</div>
		</div>
	);
};

const expensiveCalculation = (num) => {
	console.log("Calculating...");
	for (let i = 0; i < 1000000000; i++) {
		num += 1;
	}
	return num;
};
```

To fix this performance issue, we can use the `useMemo` Hook to memoize the `expensiveCalculation` function. This will cause the function to only run when needed. We can wrap the expensive function call with `useMemo`.

The `useMemo`Hook accepts a second parameter to declare dependencies. The expensive function will only run when its dependencies have changed.

In the following example, the expensive function will only run when `count` is changed and not when todo's are added.

```jsx
import { useState, useMemo } from "react";
import ReactDOM from "react-dom/client";

const App = () => {
	const [count, setCount] = useState(0);
	const [todos, setTodos] = useState([]);
	const calculation = useMemo(() => expensiveCalculation(count), [count]);

	const increment = () => {
		setCount((c) => c + 1);
	};
	const addTodo = () => {
		setTodos((t) => [...t, "New Todo"]);
	};

	return (
		<div>
			<div>
				<h2>My Todos</h2>
				{todos.map((todo, index) => {
					return <p key={index}>{todo}</p>;
				})}
				<button onClick={addTodo}>Add Todo</button>
			</div>
			<hr />
			<div>
				Count: {count}
				<button onClick={increment}>+</button>
				<h2>Expensive Calculation</h2>
				{calculation}
			</div>
		</div>
	);
};

const expensiveCalculation = (num) => {
	console.log("Calculating...");
	for (let i = 0; i < 1000000000; i++) {
		num += 1;
	}
	return num;
};
```

### Custom Hooks

```js
//useFetch.js
import { useState, useEffect } from "react";

// this hooks does is to declare initial state and whenever the url updates it sets the state to the new one
const useFetch = (url) => {
	const [data, setData] = useState(null);
	const [isPending, setIsPending] = useState(true);
	const [error, setError] = useState(null);

	useEffect(() => {
		// only using setTimeout to simulate a real request
		setTimeout(() => {
			fetch(url)
				.then((res) => {
					if (!res.ok) {
						throw Error(
							"Could not fetch the data for that resource"
						);
					}
					return res.json();
				})
				.then((data) => {
					setData(data);
					setIsPending(false);
					setError(null);
				})
				.catch((err) => {
					setIsPending(false);
					setError(err.message);
				});
		}, 500);
	}, [url]); // will need to pass url as a dependency since whenever it changes it should re-run

	// when using hooks generally return an array but by using object we don't have to worry about the order in which we get stuff back
	return { data, isPending, error };
};

export default useFetch;
```

## Component lifecycle in functional components

**NOTE : If StrictMode is on then react renders components twice in dev mode and that would lead to 2 console logs when showing the example.**

```jsx
// App.js file
import { useState } from "react";
import Counter from "./Counter";

export default function App() {
	const [count, setCount] = useState(0);

	return (
		<div>
			<Counter number={count} />
			<button onClick={() => setCount((count) => count + 1)}>
				Increase Count{" "}
			</button>
		</div>
	);
}
```

```jsx
// Counter.js file
import { useEffect } from "react";

const Counter = ({ number }) => {
	useEffect(() => {
		console.log("on mount");
		return () => {
			console.log("on unmount");
		};
	}, []);

	useEffect(() => {
		console.log("on update");
	}, [number]);

	return <div>{number}</div>;
};

export default Counter;
```

## Create folder ui structure

Create dummy folder data

```jsx
// folderData/folderData.js
const fileExplorerData = {
	name: "root",
	isFolder: true,
	items: [
		{
			name: "public",
			isFolder: true,
			items: [
				{
					name: "index.html",
				},
				{
					name: "robots.txt",
				},
			],
		},
		{
			name: "src",
			isFolder: true,
			items: [
				{
					name: "folderData",
					isFolder: true,
					items: [
						{
							name: "folderData.js",
						},
					],
				},
				{
					name: "App.js",
				},
				{
					name: "index.js",
				},
				{
					name: "styles.css",
				},
			],
		},
		{
			name: "package.json",
		},
	],
};

export default fileExplorerData;
```

```jsx
// App.js file
import fileExplorerData from "../src/folderData/folderData";
import Explorer from "../src/components/Explorer";

const App = () => {
	return (
		<div>
			<Explorer fileExplorerData={fileExplorerData} />
		</div>
	);
};

export default App;
```

Create a component Explorer

```jsx
// components/Explorer.js
import { useState } from "react";

const Explorer = ({ fileExplorerData }) => {
	const [displayFiles, setDisplayFiles] = useState(false);
	const { items } = fileExplorerData;

	if (fileExplorerData.isFolder) {
		return (
			<div>
				<span onClick={() => setDisplayFiles(!displayFiles)}>
					{fileExplorerData.name}
				</span>

				{/* {displayFiles ? (
          <div style={{ paddingLeft: "5px" }}>
            {items.map((item) => {
              return <Explorer fileExplorerData={item} />;
            })}
          </div>
        ) : null} */}
				<div
					style={
						displayFiles
							? { display: "block", paddingLeft: "5px" }
							: { display: "none" }
					}
				>
					{items.map((item, idx) => {
						return <Explorer fileExplorerData={item} key={idx} />;
					})}
				</div>
			</div>
		);
	}
	return (
		<div>
			<span>{fileExplorerData.name}</span>
		</div>
	);
};

export default Explorer;
```

## React Router, Context API, Redux

Refer notes.
