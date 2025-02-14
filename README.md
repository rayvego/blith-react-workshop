# What on earth is React?

A popular open-source JavaScript library for building user interfaces, primarily for web applications.

# What's the Need for React / What Problem Does it Solve?

- The primary motivation behind React's creation was to solve performance and maintainability issues that Facebook was facing with its growing application.
- It **improved performance**, **code maintainability** and **efficient updates**.

How is it better than normal HTML CSS JS?

- Easier for the developer to make large applications.
- Component based architecture allows for reusability.
- Efficient and faster updates - only updates the parts of the UI that have changed rather than rendering whole page again
- JS included to make dynamic user intferfaces - no more use of handlebars or embedded JS (EJS)
- Scalable, SEO friendly, rich ecosystem of libraries and tools, cross-platform development

# Setting up a React Project with Vite

## Vite

Vite is a modern, fast build tool and development server for web applications. It’s designed to enhance the efficiency and speed of building web projects, particularly for JavaScript frameworks like React, Vue, and Angular.

Optimized builds, fast development servers, HMR, and many more!

## Create a Project Using Vite

```shell
npm create vite@latest // create project

npm install // install dependencies

npm run dev // run development server
```

## Prelude

*Typical Folder Structure*

```plaintext
task-manager/
├── node_modules/
├── public/
│   └── favicon.ico
├── src/
│   ├── assets/
│   │   └── logo.svg         # App logo and static images
│   ├── components/
│   │   ├── common/          # Reusable UI components
│   │   │   ├── Button/
│   │   │   │   ├── Button.jsx
│   │   │   │   └── Button.module.css
│   │   │   ├── Modal/
│   │   │   └── Header/
│   │   ├── tasks/           # Task-related components
│   │   │   ├── TaskList/
│   │   │   ├── TaskItem/
│   │   │   └── TaskForm/
│   ├── context/             # React Context providers
│   │   └── TaskContext.jsx
│   ├── hooks/               # Custom hooks
│   │   └── useLocalStorage.js
│   ├── pages/               # Page components
│   │   ├── Home/
│   │   │  ├── Home.jsx
│   │   │  └── Home.module.css
│   │   ├── About/
│   │   │  ├── About.jsx
│   │   │  └── About.module.css
│   ├── services/            # API services
│   │   └── taskService.js
│   ├── styles/              # Global styles
│   │   ├── index.css        # Main CSS file
│   │   └── variables.css    # CSS variables
│   ├── utils/               # Helper functions
│   │   └── helpers.js
│   ├── App.jsx              # Root component
│   └── main.jsx             # Entry point
├── .gitignore
├── index.html               # Main HTML template
├── package.json
├── vite.config.js           # Vite configuration
└── README.md
```

## What's JSX and TSX? A New Language!?!

JSX and TSX are syntax extensions that let you write HTML-like code directly in JavaScript/TypeScript files, mainly used in React for building user interfaces. While compiling, it gets converted to JS and TS through tools like Babel/

*Example*
```jsx
function Greeting() {
	return (
		<div>
			<h1 className={"container"}>Hello, World!</h1>
			<h1 style={{color: "red", fontSize: "20px"}}>2 + 3 = {2 + 3}</h1>
		</div>
	);
}
```

# Components and Props

React components and props work together to create reusable, dynamic user interfaces.

Components are the building blocks of React. They are self-contained UI elements that contain:

- JSX/TSX
- JS/TS Logic
- Styling (optional)

Once created - they can be reused and nested.

```jsx
function App() {
  return (
    <Header>
      <Navigation />
      <SearchBar />
    </Header>
  )
}
```

Props are a way to communicate between components - data passed from parent to child essentially

- One-way only: Parent --> Child only
- Read-only: Child can't modify props but you pass functions as props that can be called to change state.

```jsx
// Parent component
function App() {
  const user = { name: "Bob", role: "Developer" }
  return <ProfileCard user={user} />
}

// Child component
function ProfileCard({ user }) {
  return (
    <div className="card">
      <h2>{user.name}</h2>
      <p>{user.role}</p>
    </div>
  )
}
```

# State

State in React is a built-in memory system for components that lets them track and update dynamic information over time.

*What it does*
- Stores data that changes during component use (e.g., form inputs, counters)
- Triggers automatic re-renders when updated
- Keeps UI in sync with user interactions or data changes

It is local to components - Each component instance has its own isolated state. Two `<Counter />` components won’t interfere with each other’s counts - its not like a database.

```jsx
function Counter() {
  const [count, setCount] = useState(0); // Initial value: 0

  return (
    <button onClick={() => setCount(count => count + 1)}>
      Clicks: {count}
    </button>
  );
}
```

```jsx
function LoginForm() {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  return (
    <form>
      <input 
        type="text" 
        value={username} 
        onChange={(e) => setUsername(e.target.value)} 
      />
      <input
        type="password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />
    </form>
  );
}
```

# Hooks

They are just functions introduced in React 16 that allow components (functional) to manage state, lifecycle methods and other React features previously only supported in class components. They let you “hook into” React’s core features like state, context, and lifecycle methods from functional components like:

- State management (with `useState` or `useReducer`).
- Side-effect handling (with `useEffect`).
- Context access (with `useContext`).

Some common ones are - `useState`, `useEffect`, `useContext`, `useRefs`, `useCallback`, `useMemo`, `useReducer`.

We can also have custom hooks like:

```jsx
function useToggle(initial = false) {
  const [state, setState] = useState(initial);
  const toggle = () => setState(!state);
  return [state, toggle];
}
```

## useState

Track dynamic values in components

```jsx
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(prev => prev + 1)}>
        Increment
      </button>
    </div>
  );
}
```

## useEffect

Handle operations outside React’s render cycle.

```jsx
import { useState, useEffect } from 'react';

function UserData() {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Fetch data on mount
    fetch('/api/user')
      .then(res => res.json())
      .then(data => setUser(data));

    // Cleanup function
    return () => {
      console.log('Component unmounted');
    };
  }, []); // Dependency array, empty => run once initially

  return <div>{user?.name}</div>;
}
```

## useContext

Global State - Access context values without prop drilling.

```jsx
// 1. Create Theme Context
import { createContext, useState, useContext } from 'react';

const ThemeContext = createContext();

export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  const toggleTheme = () => {
    setTheme(prevTheme => prevTheme === 'light' ? 'dark' : 'light');
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook for easy access
export function useTheme() {
  return useContext(ThemeContext);
}
```

```jsx
// 2. Wrap your app with ThemeProvider
import { ThemeProvider } from './ThemeContext';

function App() {
  return (
    <ThemeProvider>
      <MainContent />
    </ThemeProvider>
  );
}
```

```jsx
// 3. Use theme in components
function Header() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <header style={{ 
      backgroundColor: theme === 'light' ? '#f8f9fa' : '#212529',
      color: theme === 'light' ? '#212529' : '#f8f9fa'
    }}>
      <button onClick={toggleTheme}>
        Switch to {theme === 'light' ? 'Dark' : 'Light'} Mode
      </button>
    </header>
  );
}

function Content() {
  const { theme } = useTheme();
  
  return (
    <div style={{
      backgroundColor: theme === 'light' ? '#ffffff' : '#343a40',
      color: theme === 'light' ? '#343a40' : '#ffffff',
      minHeight: '100vh',
      padding: '20px'
    }}>
      <h1>{theme === 'light' ? 'Light' : 'Dark'} Theme Active</h1>
      <p>This content automatically adapts to the selected theme.</p>
    </div>
  );
}

function MainContent() {
  return (
    <>
      <Header />
      <Content />
    </>
  );
}

```

## useRef (not useRefs!)

Suppose you want to track and display how many times a component was re-rendered.

First thought that comes to mind is to use state to manage the count and `useEffect` to update that count. Do you see the problem here? We are running into an *infinite loop*. When we update the state, useEffect is going to be fired which will update the state, which will again fire the useEffect… and this goes on.

```jsx
import React, { useState, useEffect ) from 'react'

export default function App() {
	const [name, setName] = useState("")
	const [renderCount, setRenderCount] = useState(0)
	
	useEffect(() => {
		setRenderCount(prevRenderCount => prevRenderCount + 1)
	}) // no dependency array so runs after every render
	
	return (
		<>
			<input value={name} onChange={e => setName(e.target.value)}/>
			<div> My name is {name} </div>
			<div> I rendered {renderCount} times </div>
		</>
	)
}
```

The solution is to use something called **useRef**. It is very similar to state but it persists between renders of the components and it doesn’t cause the component to re-update when it changes!

It returns an object called **current** which is the value stored in it.

```jsx
import React, { useState, useEffect ) from 'react'

export default function App() {
	const [name, setName] = useState("")
	const renderCount = useRef(1)
	
	useEffect(() => {
		renderCount.current = renderCount.current + 1
	}) // no dependency array so runs after every render
	
	return (
		<>
			<input value={name} onChange={e => setName(e.target.value)}/>
			<div> My name is {name} </div>
			<div> I rendered {renderCount.current} times </div>
		</>
	)
}
```

# Example of a React Application

- `index.html` - Browser’s first loaded file, contains the `div` where React mounts and loads global CSS and entry script
- `main.tsx` - Initializes React application, finds DOM container (`#root`) and renders root `<App>` component
- `App.tsx` - Root component, starting point for component tree, your main UI component, edit this to build your interface
- `vite.config.ts` - Build configuration, plugin integration, dev server behavior, build process configs.
- `package.json` - Metadata about the project

# Bonus! React Query - Fetching Done Correctly

React TanStack Query (formerly React Query) is a powerful data-fetching library that simplifies server-state management in React applications by handling caching, synchronization, and background updates.

**Why Use TanStack Query?**
Key advantages over manual state management:
- *Automatic caching & deduplication*: Reduces redundant API calls by reusing cached data
- *Background refetching*: Keeps data fresh with smart stale-while-revalidate strategies
- *Optimized performance*: Minimizes re-renders through granular state updates
- *Boilerplate reduction*: Eliminates 40-90% of state management code compared to useEffect-based solutions
- *Error/loading state handling*: Built-in mechanisms for unified error handling and loading states

`useQuery` - Manages data fetching, caching, background refetching and automatic revalidation.

```jsx
const { 
  data,          // Successful response
  error,         // Error object
  isLoading,     // Initial load state
  isFetching,    // Background refresh state
  status         // 'loading' | 'error' | 'success'
} = useQuery({
  queryKey: ['todos', { userId: 1 }], // Cache key (array)
  queryFn: ({ signal }) =>            // Fetch function
    axios.get('/api/todos', { 
      params: { userId: 1 },
      signal // Auto-cancellation
    }).then(res => res.data),
  staleTime: 1000 * 60 * 5, // 5 minutes until stale
  refetchOnWindowFocus: true,
  retry: (failureCount, error) => 
    error.status !== 404 && failureCount < 3
});

```

`useMutation` - For changing data on your server (like creating/updating/deleting records). Think of it for actions that modify data - POST/PUT/PATCH/DELETE

```jsx
const addTodoMutation = useMutation({
  mutationFn: (newTodo) => 
    axios.post('/api/todos', newTodo), // Your API call
  onSuccess: () => {
    // Refresh todos list after success
    queryClient.invalidateQueries(['todos']);
  }
});

// Trigger when needed:
addTodoMutation.mutate({ text: "Buy milk" });

// Or

<button onClick={() => addTodoMutation.mutate({ text: "New task" })}>
  Add Task
</button>

// Usage

addTodoMutation.isPending // Loading state
addTodoMutation.isError   // If request failed
addTodoMutation.isSuccess // If request worked
```

```jsx
const queryClient = new QueryClient()

function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <TodoApp />
    </QueryClientProvider>
  )
}
```

- *Complex Sequences*: Multi-step writes requiring atomicity
- *Real-Time Updates*: Chat messages, live collaborations
- *Undo/Redo Systems*: Operations needing rollback capabilities
- *Optimistic UI*: Instant feedback systems (likes, favorites)

# Bonus! TailwindCSS - Better Styling

# Bonus! Introduction to NextJS
