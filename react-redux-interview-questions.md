# React.js and Redux Interview Questions & Answers

## React.js Fundamentals

### Q1. What is React.js?
**Answer:** React.js is a JavaScript library for building user interfaces, particularly single-page applications. It's used for handling the view layer and can be used for developing both web and mobile applications.

**Key Features:**
- Virtual DOM
- Component-based architecture
- Unidirectional data flow
- JSX support

**Example:**
```jsx
import React from 'react';

function App() {
  return (
    <div className="App">
      <h1>Hello, React!</h1>
    </div>
  );
}

export default App;
```

### Q2. What are Components in React?
**Answer:** Components are the building blocks of React applications. They are reusable pieces of UI that can be either functional (using hooks) or class-based.

**Example:**
```jsx
// Functional Component
function Welcome(props) {
  return <h1>Hello, {props.name}!</h1>;
}

// Class Component
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}

// Usage
<Welcome name="John" />
```

### Q3. What is JSX?
**Answer:** JSX is a syntax extension for JavaScript that allows you to write HTML-like code in JavaScript. It gets transformed into regular JavaScript function calls.

**Example:**
```jsx
// JSX
const element = (
  <div className="greeting">
    <h1>Hello, World!</h1>
    <p>Welcome to React</p>
  </div>
);

// Transpiled to:
const element = React.createElement(
  'div',
  { className: 'greeting' },
  React.createElement('h1', null, 'Hello, World!'),
  React.createElement('p', null, 'Welcome to React')
);
```

### Q4. What are Props in React?
**Answer:** Props (properties) are read-only components that are passed from parent to child components. They are used to pass data and event handlers down the component tree.

**Example:**
```jsx
// Parent Component
function Parent() {
  const user = { name: 'Alice', age: 25 };
  
  return (
    <Child 
      name={user.name} 
      age={user.age}
      onUserClick={() => console.log('User clicked')}
    />
  );
}

// Child Component
function Child(props) {
  return (
    <div onClick={props.onUserClick}>
      <h2>Name: {props.name}</h2>
      <p>Age: {props.age}</p>
    </div>
  );
}
```

### Q5. What is State in React?
**Answer:** State is a built-in object that contains data or information about the component. When state changes, the component re-renders.

**Example:**
```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
      <button onClick={() => setCount(count - 1)}>
        Decrement
      </button>
    </div>
  );
}
```

### Q6. What are React Hooks?
**Answer:** Hooks are functions that allow you to use state and other React features in functional components. They were introduced in React 16.8.

**Common Hooks:**
```jsx
import React, { useState, useEffect, useContext, useRef } from 'react';

function ExampleComponent() {
  // useState - for state management
  const [count, setCount] = useState(0);
  
  // useEffect - for side effects
  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);
  
  // useRef - for accessing DOM elements
  const inputRef = useRef(null);
  
  // useContext - for consuming context
  const theme = useContext(ThemeContext);
  
  return (
    <div>
      <input ref={inputRef} />
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
    </div>
  );
}
```

### Q7. What is the Virtual DOM?
**Answer:** The Virtual DOM is a lightweight copy of the actual DOM. React uses it to improve performance by minimizing direct manipulation of the DOM.

**How it works:**
1. When state changes, React creates a new Virtual DOM tree
2. Compares it with the previous Virtual DOM tree (diffing)
3. Updates only the changed parts in the real DOM

**Example:**
```jsx
// React automatically handles Virtual DOM updates
function TodoList() {
  const [todos, setTodos] = useState(['Learn React', 'Build App']);
  
  const addTodo = () => {
    setTodos([...todos, 'New Todo']);
    // React will efficiently update only the new list item
  };
  
  return (
    <ul>
      {todos.map((todo, index) => (
        <li key={index}>{todo}</li>
      ))}
    </ul>
  );
}
```

### Q8. What is the difference between State and Props?
**Answer:**

| State | Props |
|-------|-------|
| Managed within component | Passed from parent component |
| Can be modified | Read-only |
| Component re-renders when changed | Component re-renders when changed |
| Used for component's internal data | Used for passing data between components |

**Example:**
```jsx
// Parent component with state
function Parent() {
  const [parentData, setParentData] = useState('Parent State');
  
  return <Child data={parentData} />; // data is a prop
}

// Child component
function Child(props) {
  const [childState, setChildState] = useState('Child State'); // internal state
  
  return (
    <div>
      <p>Props: {props.data}</p>
      <p>State: {childState}</p>
    </div>
  );
}
```

### Q9. What are Lifecycle Methods in React?
**Answer:** Lifecycle methods are special methods that are called at different stages of a component's life. In functional components, useEffect replaces most lifecycle methods.

**Class Component Lifecycle:**
```jsx
class LifecycleExample extends React.Component {
  constructor(props) {
    super(props);
    this.state = { data: null };
  }

  componentDidMount() {
    // Called after component is mounted
    this.fetchData();
  }

  componentDidUpdate(prevProps, prevState) {
    // Called after component updates
    if (prevProps.id !== this.props.id) {
      this.fetchData();
    }
  }

  componentWillUnmount() {
    // Called before component unmounts
    this.cleanup();
  }

  render() {
    return <div>{this.state.data}</div>;
  }
}
```

**Functional Component with useEffect:**
```jsx
function LifecycleExample({ id }) {
  const [data, setData] = useState(null);

  // componentDidMount equivalent
  useEffect(() => {
    fetchData();
  }, []);

  // componentDidUpdate equivalent
  useEffect(() => {
    if (id) {
      fetchData();
    }
  }, [id]);

  // componentWillUnmount equivalent
  useEffect(() => {
    return () => {
      cleanup();
    };
  }, []);

  return <div>{data}</div>;
}
```

### Q10. What is Context API?
**Answer:** Context API provides a way to pass data through the component tree without having to pass props down manually at every level.

**Example:**
```jsx
// Create Context
const ThemeContext = React.createContext();

// Provider Component
function App() {
  const [theme, setTheme] = useState('light');

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <Header />
      <Main />
    </ThemeContext.Provider>
  );
}

// Consumer Component
function Header() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <header className={theme}>
      <h1>My App</h1>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </header>
  );
}
```

## Redux Fundamentals

### Q11. What is Redux?
**Answer:** Redux is a predictable state container for JavaScript applications. It helps manage application state in a predictable way using a unidirectional data flow.

**Core Concepts:**
- Store: Single source of truth
- Actions: Describe what happened
- Reducers: Specify how state changes

**Example:**
```jsx
// Action
const addTodo = (text) => ({
  type: 'ADD_TODO',
  payload: { id: Date.now(), text, completed: false }
});

// Reducer
const todoReducer = (state = [], action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return [...state, action.payload];
    case 'TOGGLE_TODO':
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
};

// Store
import { createStore } from 'redux';
const store = createStore(todoReducer);
```

### Q12. What are Actions in Redux?
**Answer:** Actions are plain JavaScript objects that describe what happened. They are the only way to send data to the Redux store.

**Example:**
```jsx
// Action Types (constants)
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const DELETE_TODO = 'DELETE_TODO';

// Action Creators
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: {
    id: Date.now(),
    text,
    completed: false
  }
});

const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id
});

// Usage
store.dispatch(addTodo('Learn Redux'));
store.dispatch(toggleTodo(123));
store.dispatch(deleteTodo(123));
```

### Q13. What are Reducers in Redux?
**Answer:** Reducers are pure functions that take the current state and an action, and return a new state. They specify how the application's state changes in response to actions.

**Example:**
```jsx
// Initial State
const initialState = {
  todos: [],
  filter: 'all'
};

// Reducer
const todoReducer = (state = initialState, action) => {
  switch (action.type) {
    case 'ADD_TODO':
      return {
        ...state,
        todos: [...state.todos, action.payload]
      };
    
    case 'TOGGLE_TODO':
      return {
        ...state,
        todos: state.todos.map(todo =>
          todo.id === action.payload
            ? { ...todo, completed: !todo.completed }
            : todo
        )
      };
    
    case 'DELETE_TODO':
      return {
        ...state,
        todos: state.todos.filter(todo => todo.id !== action.payload)
      };
    
    case 'SET_FILTER':
      return {
        ...state,
        filter: action.payload
      };
    
    default:
      return state;
  }
};
```

### Q14. What is the Redux Store?
**Answer:** The store is the object that brings together actions and reducers. It holds the application state and provides methods to access and update it.

**Example:**
```jsx
import { createStore, combineReducers } from 'redux';

// Multiple reducers
const todoReducer = (state = [], action) => {
  // ... todo logic
};

const userReducer = (state = null, action) => {
  switch (action.type) {
    case 'SET_USER':
      return action.payload;
    default:
      return state;
  }
};

// Combine reducers
const rootReducer = combineReducers({
  todos: todoReducer,
  user: userReducer
});

// Create store
const store = createStore(rootReducer);

// Store methods
console.log(store.getState()); // Get current state
store.dispatch(addTodo('New todo')); // Dispatch action
store.subscribe(() => console.log('State changed:', store.getState())); // Subscribe to changes
```

### Q15. How to Connect React with Redux?
**Answer:** React-Redux provides the `connect` function and `useSelector`/`useDispatch` hooks to connect React components to the Redux store.

**Using Hooks (Modern approach):**
```jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';

function TodoList() {
  const todos = useSelector(state => state.todos);
  const dispatch = useDispatch();

  const handleAddTodo = (text) => {
    dispatch(addTodo(text));
  };

  const handleToggleTodo = (id) => {
    dispatch(toggleTodo(id));
  };

  return (
    <div>
      {todos.map(todo => (
        <div key={todo.id}>
          <span
            style={{ textDecoration: todo.completed ? 'line-through' : 'none' }}
            onClick={() => handleToggleTodo(todo.id)}
          >
            {todo.text}
          </span>
        </div>
      ))}
    </div>
  );
}
```

**Using connect (Class components):**
```jsx
import { connect } from 'react-redux';

class TodoList extends React.Component {
  render() {
    const { todos, dispatch } = this.props;
    
    return (
      <div>
        {todos.map(todo => (
          <div key={todo.id}>
            <span onClick={() => dispatch(toggleTodo(todo.id))}>
              {todo.text}
            </span>
          </div>
        ))}
      </div>
    );
  }
}

const mapStateToProps = (state) => ({
  todos: state.todos
});

export default connect(mapStateToProps)(TodoList);
```

### Q16. What is Redux Toolkit?
**Answer:** Redux Toolkit is the official, opinionated, batteries-included toolset for efficient Redux development. It simplifies Redux setup and reduces boilerplate code.

**Example:**
```jsx
import { createSlice, configureStore } from '@reduxjs/toolkit';

// Create slice (combines actions and reducers)
const todoSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      state.push({
        id: Date.now(),
        text: action.payload,
        completed: false
      });
    },
    toggleTodo: (state, action) => {
      const todo = state.find(todo => todo.id === action.payload);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    deleteTodo: (state, action) => {
      return state.filter(todo => todo.id !== action.payload);
    }
  }
});

// Extract actions and reducer
export const { addTodo, toggleTodo, deleteTodo } = todoSlice.actions;
export default todoSlice.reducer;

// Create store
const store = configureStore({
  reducer: {
    todos: todoSlice.reducer
  }
});
```

### Q17. What are Redux Middleware?
**Answer:** Middleware provides a way to interact with actions that have been dispatched to the store before they reach the reducer. Common use cases include logging, async actions, and side effects.

**Example with Redux Thunk:**
```jsx
import { createAsyncThunk } from '@reduxjs/toolkit';

// Async action creator
export const fetchTodos = createAsyncThunk(
  'todos/fetchTodos',
  async () => {
    const response = await fetch('/api/todos');
    return response.json();
  }
);

// Slice with async reducer
const todoSlice = createSlice({
  name: 'todos',
  initialState: { items: [], loading: false, error: null },
  reducers: {
    // ... other reducers
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchTodos.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchTodos.fulfilled, (state, action) => {
        state.loading = false;
        state.items = action.payload;
      })
      .addCase(fetchTodos.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  }
});

// Usage in component
function TodoList() {
  const dispatch = useDispatch();
  const { items, loading, error } = useSelector(state => state.todos);

  useEffect(() => {
    dispatch(fetchTodos());
  }, [dispatch]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      {items.map(todo => (
        <div key={todo.id}>{todo.text}</div>
      ))}
    </div>
  );
}
```

### Q18. What is Redux DevTools?
**Answer:** Redux DevTools is a browser extension that provides debugging capabilities for Redux applications. It allows you to inspect state changes, time-travel debugging, and more.

**Setup:**
```jsx
import { configureStore } from '@reduxjs/toolkit';

const store = configureStore({
  reducer: rootReducer,
  devTools: process.env.NODE_ENV !== 'production'
});
```

**Features:**
- State inspection
- Action history
- Time-travel debugging
- State diffs
- Action replay

### Q19. What are Selectors in Redux?
**Answer:** Selectors are functions that extract specific pieces of data from the Redux state. They help with performance optimization and code organization.

**Example:**
```jsx
// Basic selectors
const selectTodos = (state) => state.todos;
const selectUser = (state) => state.user;

// Memoized selectors with reselect
import { createSelector } from 'reselect';

const selectTodos = (state) => state.todos;

const selectCompletedTodos = createSelector(
  [selectTodos],
  (todos) => todos.filter(todo => todo.completed)
);

const selectActiveTodos = createSelector(
  [selectTodos],
  (todos) => todos.filter(todo => !todo.completed)
);

const selectTodoCount = createSelector(
  [selectTodos],
  (todos) => todos.length
);

// Usage in component
function TodoStats() {
  const completedTodos = useSelector(selectCompletedTodos);
  const activeTodos = useSelector(selectActiveTodos);
  const totalTodos = useSelector(selectTodoCount);

  return (
    <div>
      <p>Total: {totalTodos}</p>
      <p>Completed: {completedTodos.length}</p>
      <p>Active: {activeTodos.length}</p>
    </div>
  );
}
```

### Q20. What is the difference between Redux and Context API?
**Answer:**

| Redux | Context API |
|-------|-------------|
| External library | Built into React |
| More boilerplate | Less boilerplate |
| Better for large applications | Good for small to medium apps |
| Built-in dev tools | No built-in dev tools |
| Predictable state updates | Can be unpredictable |
| Middleware support | No middleware |
| Time-travel debugging | No time-travel debugging |

**When to use Redux:**
- Large applications with complex state
- Need for time-travel debugging
- Multiple teams working on the same codebase
- Need for middleware (async actions, logging, etc.)

**When to use Context API:**
- Small to medium applications
- Simple state management needs
- Want to avoid external dependencies
- Simple theme or user authentication state

## Advanced React Concepts

### Q21. What are React Fragments?
**Answer:** Fragments allow you to group multiple elements without adding extra nodes to the DOM.

**Example:**
```jsx
// Without Fragment
function App() {
  return (
    <div>
      <h1>Title</h1>
      <p>Paragraph</p>
    </div>
  );
}

// With Fragment
function App() {
  return (
    <>
      <h1>Title</h1>
      <p>Paragraph</p>
    </>
  );
}

// Or with explicit Fragment
import React from 'react';

function App() {
  return (
    <React.Fragment>
      <h1>Title</h1>
      <p>Paragraph</p>
    </React.Fragment>
  );
}
```

### Q22. What are React Portals?
**Answer:** Portals provide a way to render children into a DOM node that exists outside the parent component's DOM hierarchy.

**Example:**
```jsx
import ReactDOM from 'react-dom';

function Modal({ children, isOpen }) {
  if (!isOpen) return null;

  return ReactDOM.createPortal(
    <div className="modal">
      <div className="modal-content">
        {children}
      </div>
    </div>,
    document.getElementById('modal-root')
  );
}

// Usage
function App() {
  const [isModalOpen, setIsModalOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsModalOpen(true)}>
        Open Modal
      </button>
      <Modal isOpen={isModalOpen}>
        <h2>Modal Content</h2>
        <button onClick={() => setIsModalOpen(false)}>
          Close
        </button>
      </Modal>
    </div>
  );
}
```

### Q23. What are React Error Boundaries?
**Answer:** Error boundaries are React components that catch JavaScript errors anywhere in their child component tree and display a fallback UI.

**Example:**
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, errorInfo) {
    console.log('Error:', error);
    console.log('Error Info:', errorInfo);
  }

  render() {
    if (this.state.hasError) {
      return <h1>Something went wrong.</h1>;
    }

    return this.props.children;
  }
}

// Usage
function App() {
  return (
    <ErrorBoundary>
      <MyComponent />
    </ErrorBoundary>
  );
}
```

### Q24. What are React Memo and useMemo?
**Answer:** These are performance optimization techniques to prevent unnecessary re-renders.

**React.memo:**
```jsx
const MyComponent = React.memo(function MyComponent(props) {
  return <div>{props.name}</div>;
});

// Only re-renders if props change
<MyComponent name="John" />
```

**useMemo:**
```jsx
function ExpensiveComponent({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]); // Only recalculates when items change

  return <div>Total: {expensiveValue}</div>;
}
```

### Q25. What is useCallback?
**Answer:** useCallback returns a memoized callback function that only changes if one of its dependencies has changed.

**Example:**
```jsx
function ParentComponent() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []); // Empty dependency array means callback never changes

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>
        Count: {count}
      </button>
      <ChildComponent onButtonClick={handleClick} />
    </div>
  );
}

const ChildComponent = React.memo(function ChildComponent({ onButtonClick }) {
  return <button onClick={onButtonClick}>Click me</button>;
});
```

## Common Interview Scenarios

### Q26. How would you implement a search functionality with debouncing?
**Answer:**
```jsx
import React, { useState, useEffect, useCallback } from 'react';

function SearchComponent() {
  const [searchTerm, setSearchTerm] = useState('');
  const [results, setResults] = useState([]);
  const [loading, setLoading] = useState(false);

  // Debounced search function
  const debouncedSearch = useCallback(
    debounce(async (term) => {
      if (!term) {
        setResults([]);
        return;
      }

      setLoading(true);
      try {
        const response = await fetch(`/api/search?q=${term}`);
        const data = await response.json();
        setResults(data);
      } catch (error) {
        console.error('Search error:', error);
      } finally {
        setLoading(false);
      }
    }, 300),
    []
  );

  useEffect(() => {
    debouncedSearch(searchTerm);
  }, [searchTerm, debouncedSearch]);

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={(e) => setSearchTerm(e.target.value)}
        placeholder="Search..."
      />
      {loading && <div>Loading...</div>}
      <ul>
        {results.map(item => (
          <li key={item.id}>{item.name}</li>
        ))}
      </ul>
    </div>
  );
}

// Debounce utility function
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}
```

### Q27. How would you implement infinite scrolling?
**Answer:**
```jsx
import React, { useState, useEffect, useRef } from 'react';

function InfiniteScrollList() {
  const [items, setItems] = useState([]);
  const [loading, setLoading] = useState(false);
  const [page, setPage] = useState(1);
  const [hasMore, setHasMore] = useState(true);
  const observer = useRef();

  const lastItemRef = useCallback(node => {
    if (loading) return;
    if (observer.current) observer.current.disconnect();
    observer.current = new IntersectionObserver(entries => {
      if (entries[0].isIntersecting && hasMore) {
        setPage(prevPage => prevPage + 1);
      }
    });
    if (node) observer.current.observe(node);
  }, [loading, hasMore]);

  useEffect(() => {
    const fetchItems = async () => {
      setLoading(true);
      try {
        const response = await fetch(`/api/items?page=${page}`);
        const data = await response.json();
        
        if (data.items.length === 0) {
          setHasMore(false);
        } else {
          setItems(prev => [...prev, ...data.items]);
        }
      } catch (error) {
        console.error('Error fetching items:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchItems();
  }, [page]);

  return (
    <div>
      {items.map((item, index) => (
        <div
          key={item.id}
          ref={index === items.length - 1 ? lastItemRef : null}
        >
          {item.name}
        </div>
      ))}
      {loading && <div>Loading...</div>}
      {!hasMore && <div>No more items</div>}
    </div>
  );
}
```

### Q28. How would you implement a custom hook for API calls?
**Answer:**
```jsx
import { useState, useEffect } from 'react';

function useApi(url, options = {}) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        setError(null);
        
        const response = await fetch(url, options);
        if (!response.ok) {
          throw new Error(`HTTP error! status: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  const refetch = () => {
    setLoading(true);
    setError(null);
    fetchData();
  };

  return { data, loading, error, refetch };
}

// Usage
function UserProfile({ userId }) {
  const { data: user, loading, error, refetch } = useApi(`/api/users/${userId}`);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  if (!user) return <div>No user found</div>;

  return (
    <div>
      <h1>{user.name}</h1>
      <p>{user.email}</p>
      <button onClick={refetch}>Refresh</button>
    </div>
  );
}
```

### Q29. How would you implement a form with validation?
**Answer:**
```jsx
import React, { useState } from 'react';

function ContactForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    message: ''
  });
  const [errors, setErrors] = useState({});
  const [isSubmitting, setIsSubmitting] = useState(false);

  const validateForm = () => {
    const newErrors = {};

    if (!formData.name.trim()) {
      newErrors.name = 'Name is required';
    }

    if (!formData.email.trim()) {
      newErrors.email = 'Email is required';
    } else if (!/\S+@\S+\.\S+/.test(formData.email)) {
      newErrors.email = 'Email is invalid';
    }

    if (!formData.message.trim()) {
      newErrors.message = 'Message is required';
    } else if (formData.message.length < 10) {
      newErrors.message = 'Message must be at least 10 characters';
    }

    setErrors(newErrors);
    return Object.keys(newErrors).length === 0;
  };

  const handleSubmit = async (e) => {
    e.preventDefault();
    
    if (!validateForm()) {
      return;
    }

    setIsSubmitting(true);
    try {
      const response = await fetch('/api/contact', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(formData),
      });

      if (response.ok) {
        alert('Message sent successfully!');
        setFormData({ name: '', email: '', message: '' });
      } else {
        throw new Error('Failed to send message');
      }
    } catch (error) {
      alert('Error sending message: ' + error.message);
    } finally {
      setIsSubmitting(false);
    }
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prev => ({
      ...prev,
      [name]: value
    }));
    
    // Clear error when user starts typing
    if (errors[name]) {
      setErrors(prev => ({
        ...prev,
        [name]: ''
      }));
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <div>
        <label htmlFor="name">Name:</label>
        <input
          type="text"
          id="name"
          name="name"
          value={formData.name}
          onChange={handleChange}
          className={errors.name ? 'error' : ''}
        />
        {errors.name && <span className="error-text">{errors.name}</span>}
      </div>

      <div>
        <label htmlFor="email">Email:</label>
        <input
          type="email"
          id="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
          className={errors.email ? 'error' : ''}
        />
        {errors.email && <span className="error-text">{errors.email}</span>}
      </div>

      <div>
        <label htmlFor="message">Message:</label>
        <textarea
          id="message"
          name="message"
          value={formData.message}
          onChange={handleChange}
          className={errors.message ? 'error' : ''}
        />
        {errors.message && <span className="error-text">{errors.message}</span>}
      </div>

      <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Sending...' : 'Send Message'}
      </button>
    </form>
  );
}
```

### Q30. How would you implement a shopping cart with Redux?
**Answer:**
```jsx
// Redux Toolkit Slice
import { createSlice } from '@reduxjs/toolkit';

const cartSlice = createSlice({
  name: 'cart',
  initialState: {
    items: [],
    total: 0
  },
  reducers: {
    addToCart: (state, action) => {
      const existingItem = state.items.find(item => item.id === action.payload.id);
      if (existingItem) {
        existingItem.quantity += 1;
      } else {
        state.items.push({ ...action.payload, quantity: 1 });
      }
      state.total = state.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    },
    removeFromCart: (state, action) => {
      state.items = state.items.filter(item => item.id !== action.payload);
      state.total = state.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    },
    updateQuantity: (state, action) => {
      const { id, quantity } = action.payload;
      const item = state.items.find(item => item.id === id);
      if (item) {
        item.quantity = quantity;
        if (item.quantity <= 0) {
          state.items = state.items.filter(item => item.id !== id);
        }
      }
      state.total = state.items.reduce((sum, item) => sum + (item.price * item.quantity), 0);
    },
    clearCart: (state) => {
      state.items = [];
      state.total = 0;
    }
  }
});

export const { addToCart, removeFromCart, updateQuantity, clearCart } = cartSlice.actions;
export default cartSlice.reducer;

// React Component
function ShoppingCart() {
  const { items, total } = useSelector(state => state.cart);
  const dispatch = useDispatch();

  const handleAddToCart = (product) => {
    dispatch(addToCart(product));
  };

  const handleRemoveFromCart = (productId) => {
    dispatch(removeFromCart(productId));
  };

  const handleUpdateQuantity = (productId, quantity) => {
    dispatch(updateQuantity({ id: productId, quantity }));
  };

  const handleClearCart = () => {
    dispatch(clearCart());
  };

  return (
    <div>
      <h2>Shopping Cart</h2>
      {items.length === 0 ? (
        <p>Your cart is empty</p>
      ) : (
        <>
          {items.map(item => (
            <div key={item.id} className="cart-item">
              <img src={item.image} alt={item.name} />
              <div>
                <h3>{item.name}</h3>
                <p>${item.price}</p>
                <div>
                  <button
                    onClick={() => handleUpdateQuantity(item.id, item.quantity - 1)}
                    disabled={item.quantity <= 1}
                  >
                    -
                  </button>
                  <span>{item.quantity}</span>
                  <button
                    onClick={() => handleUpdateQuantity(item.id, item.quantity + 1)}
                  >
                    +
                  </button>
                  <button
                    onClick={() => handleRemoveFromCart(item.id)}
                  >
                    Remove
                  </button>
                </div>
              </div>
            </div>
          ))}
          <div className="cart-total">
            <h3>Total: ${total.toFixed(2)}</h3>
            <button onClick={handleClearCart}>Clear Cart</button>
            <button>Checkout</button>
          </div>
        </>
      )}
    </div>
  );
}
```

This comprehensive guide covers the most important React.js and Redux concepts that are commonly asked in interviews, with practical examples for each topic. The examples demonstrate real-world usage patterns and best practices that you can reference during your interview preparation.