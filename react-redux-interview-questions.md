# JavaScript React.js & Redux Interview Questions & Answers

## Table of Contents
1. [React.js Fundamentals](#reactjs-fundamentals)
2. [React Hooks](#react-hooks)
3. [React Component Lifecycle](#react-component-lifecycle)
4. [React State Management](#react-state-management)
5. [Redux Fundamentals](#redux-fundamentals)
6. [Redux Toolkit](#redux-toolkit)
7. [Advanced React Concepts](#advanced-react-concepts)
8. [Performance Optimization](#performance-optimization)
9. [Testing](#testing)
10. [Practical Examples](#practical-examples)

---

## React.js Fundamentals

### Q1. What is React.js?
**Answer:** React.js is a JavaScript library for building user interfaces, particularly single-page applications. It's used for handling the view layer and can be used for developing both web and mobile applications.

**Key Features:**
- Virtual DOM
- Component-based architecture
- Unidirectional data flow
- JSX syntax

**Example:**
```jsx
import React from 'react';

function App() {
  return (
    <div className="App">
      <h1>Hello, React!</h1>
      <p>Welcome to my first React component</p>
    </div>
  );
}

export default App;
```

### Q2. What is JSX?
**Answer:** JSX is a syntax extension for JavaScript that allows you to write HTML-like code in JavaScript. It gets transformed into regular JavaScript function calls.

**Example:**
```jsx
// JSX
const element = (
  <div className="container">
    <h1>Hello, {name}!</h1>
    <p>Welcome to React</p>
  </div>
);

// Transpiled to:
const element = React.createElement(
  'div',
  { className: 'container' },
  React.createElement('h1', null, 'Hello, ', name, '!'),
  React.createElement('p', null, 'Welcome to React')
);
```

### Q3. What are Components in React?
**Answer:** Components are the building blocks of React applications. They are reusable pieces of UI that can be either function components or class components.

**Example:**
```jsx
// Function Component
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

### Q4. What is the difference between Props and State?
**Answer:** 
- **Props** are read-only and passed from parent to child components
- **State** is mutable and managed within the component itself

**Example:**
```jsx
// Parent Component
function Parent() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <Child name="John" count={count} />
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

// Child Component
function Child(props) {
  const [localState, setLocalState] = useState(0);
  
  return (
    <div>
      <p>Name: {props.name}</p> {/* Props - read-only */}
      <p>Parent Count: {props.count}</p> {/* Props */}
      <p>Local Count: {localState}</p> {/* State */}
      <button onClick={() => setLocalState(localState + 1)}>
        Increment Local
      </button>
    </div>
  );
}
```

---

## React Hooks

### Q5. What are React Hooks?
**Answer:** Hooks are functions that allow you to use state and other React features in function components. They were introduced in React 16.8.

**Common Hooks:**
- `useState` - for state management
- `useEffect` - for side effects
- `useContext` - for consuming context
- `useReducer` - for complex state logic
- `useCallback` - for memoizing functions
- `useMemo` - for memoizing values

**Example:**
```jsx
import React, { useState, useEffect } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  const [name, setName] = useState('');

  useEffect(() => {
    document.title = `Count: ${count}`;
  }, [count]);

  return (
    <div>
      <input 
        value={name} 
        onChange={(e) => setName(e.target.value)} 
        placeholder="Enter name"
      />
      <p>Hello, {name}!</p>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

### Q6. Explain useState Hook
**Answer:** `useState` is a Hook that lets you add state to function components.

**Example:**
```jsx
import React, { useState } from 'react';

function UserForm() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    age: ''
  });

  const handleChange = (e) => {
    const { name, value } = e.target;
    setFormData(prevState => ({
      ...prevState,
      [name]: value
    }));
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log('Form submitted:', formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Name"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Email"
      />
      <input
        type="number"
        name="age"
        value={formData.age}
        onChange={handleChange}
        placeholder="Age"
      />
      <button type="submit">Submit</button>
    </form>
  );
}
```

### Q7. Explain useEffect Hook
**Answer:** `useEffect` lets you perform side effects in function components. It serves the same purpose as `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` in class components.

**Example:**
```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        setUser(userData);
      } catch (error) {
        console.error('Error fetching user:', error);
      } finally {
        setLoading(false);
      }
    };

    fetchUser();
  }, [userId]); // Dependency array

  useEffect(() => {
    // Cleanup function
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  if (loading) return <div>Loading...</div>;
  if (!user) return <div>User not found</div>;

  return (
    <div>
      <h2>{user.name}</h2>
      <p>Email: {user.email}</p>
      <p>Age: {user.age}</p>
    </div>
  );
}
```

---

## React Component Lifecycle

### Q8. Explain React Component Lifecycle
**Answer:** React components have a lifecycle that consists of three main phases: Mounting, Updating, and Unmounting.

**Class Component Lifecycle:**
```jsx
class LifecycleDemo extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
    console.log('1. Constructor');
  }

  static getDerivedStateFromProps(props, state) {
    console.log('2. getDerivedStateFromProps');
    return null;
  }

  componentDidMount() {
    console.log('4. Component Did Mount');
  }

  shouldComponentUpdate(nextProps, nextState) {
    console.log('5. Should Component Update');
    return true;
  }

  getSnapshotBeforeUpdate(prevProps, prevState) {
    console.log('7. Get Snapshot Before Update');
    return null;
  }

  componentDidUpdate(prevProps, prevState, snapshot) {
    console.log('8. Component Did Update');
  }

  componentWillUnmount() {
    console.log('9. Component Will Unmount');
  }

  render() {
    console.log('3. Render');
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Increment
        </button>
      </div>
    );
  }
}
```

**Function Component with Hooks:**
```jsx
function LifecycleDemo() {
  const [count, setCount] = useState(0);

  // Equivalent to componentDidMount
  useEffect(() => {
    console.log('Component mounted');
  }, []);

  // Equivalent to componentDidUpdate
  useEffect(() => {
    console.log('Count updated:', count);
  }, [count]);

  // Equivalent to componentWillUnmount
  useEffect(() => {
    return () => {
      console.log('Component will unmount');
    };
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

---

## React State Management

### Q9. What is Context API?
**Answer:** Context API is React's built-in state management solution that allows you to share data between components without prop drilling.

**Example:**
```jsx
import React, { createContext, useContext, useState } from 'react';

// Create Context
const ThemeContext = createContext();
const UserContext = createContext();

// Provider Component
function AppProvider({ children }) {
  const [theme, setTheme] = useState('light');
  const [user, setUser] = useState({ name: 'John', email: 'john@example.com' });

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      <UserContext.Provider value={{ user, setUser }}>
        {children}
      </UserContext.Provider>
    </ThemeContext.Provider>
  );
}

// Custom Hooks
function useTheme() {
  const context = useContext(ThemeContext);
  if (!context) {
    throw new Error('useTheme must be used within ThemeProvider');
  }
  return context;
}

function useUser() {
  const context = useContext(UserContext);
  if (!context) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}

// Components
function Header() {
  const { theme, setTheme } = useTheme();
  const { user } = useUser();

  return (
    <header style={{ background: theme === 'light' ? '#fff' : '#333' }}>
      <h1>Welcome, {user.name}!</h1>
      <button onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}>
        Toggle Theme
      </button>
    </header>
  );
}

function App() {
  return (
    <AppProvider>
      <Header />
      <Main />
    </AppProvider>
  );
}
```

---

## Redux Fundamentals

### Q10. What is Redux?
**Answer:** Redux is a predictable state container for JavaScript applications. It helps manage application state in a predictable way using a unidirectional data flow.

**Core Concepts:**
- **Store**: The single source of truth for your application state
- **Actions**: Plain objects that describe what happened
- **Reducers**: Pure functions that specify how state changes in response to actions

**Example:**
```jsx
// Action Types
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';
const DELETE_TODO = 'DELETE_TODO';

// Action Creators
const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text, completed: false }
});

const toggleTodo = (id) => ({
  type: TOGGLE_TODO,
  payload: id
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: id
});

// Reducer
const todoReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    
    case DELETE_TODO:
      return state.filter(todo => todo.id !== action.payload);
    
    default:
      return state;
  }
};

// Store
import { createStore } from 'redux';
const store = createStore(todoReducer);

// Component
function TodoApp() {
  const [todos, setTodos] = useState(store.getState());
  const [inputValue, setInputValue] = useState('');

  useEffect(() => {
    const unsubscribe = store.subscribe(() => {
      setTodos(store.getState());
    });

    return unsubscribe;
  }, []);

  const handleAddTodo = () => {
    if (inputValue.trim()) {
      store.dispatch(addTodo(inputValue));
      setInputValue('');
    }
  };

  const handleToggleTodo = (id) => {
    store.dispatch(toggleTodo(id));
  };

  const handleDeleteTodo = (id) => {
    store.dispatch(deleteTodo(id));
  };

  return (
    <div>
      <div>
        <input
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add a todo"
        />
        <button onClick={handleAddTodo}>Add</button>
      </div>
      
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => handleToggleTodo(todo.id)}
            />
            <span style={{ 
              textDecoration: todo.completed ? 'line-through' : 'none' 
            }}>
              {todo.text}
            </span>
            <button onClick={() => handleDeleteTodo(todo.id)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

### Q11. What is Redux Middleware?
**Answer:** Redux middleware provides a way to intercept and modify actions before they reach the reducer. It's useful for logging, async operations, and other side effects.

**Example:**
```jsx
// Custom Middleware
const loggerMiddleware = store => next => action => {
  console.log('Previous State:', store.getState());
  console.log('Action:', action);
  
  const result = next(action);
  
  console.log('Next State:', store.getState());
  return result;
};

// Async Middleware (Redux Thunk)
const thunkMiddleware = store => next => action => {
  if (typeof action === 'function') {
    return action(store.dispatch, store.getState);
  }
  return next(action);
};

// Async Action Creator
const fetchTodos = () => {
  return async (dispatch) => {
    dispatch({ type: 'FETCH_TODOS_START' });
    
    try {
      const response = await fetch('/api/todos');
      const todos = await response.json();
      dispatch({ type: 'FETCH_TODOS_SUCCESS', payload: todos });
    } catch (error) {
      dispatch({ type: 'FETCH_TODOS_ERROR', payload: error.message });
    }
  };
};

// Apply Middleware
import { createStore, applyMiddleware } from 'redux';
const store = createStore(
  todoReducer,
  applyMiddleware(loggerMiddleware, thunkMiddleware)
);
```

---

## Redux Toolkit

### Q12. What is Redux Toolkit?
**Answer:** Redux Toolkit is the official, opinionated, batteries-included toolset for efficient Redux development. It simplifies Redux setup and reduces boilerplate code.

**Example:**
```jsx
import { createSlice, configureStore } from '@reduxjs/toolkit';

// Create Slice
const todoSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    loading: false,
    error: null
  },
  reducers: {
    addTodo: (state, action) => {
      state.items.push({
        id: Date.now(),
        text: action.payload,
        completed: false
      });
    },
    toggleTodo: (state, action) => {
      const todo = state.items.find(todo => todo.id === action.payload);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    deleteTodo: (state, action) => {
      state.items = state.items.filter(todo => todo.id !== action.payload);
    },
    setLoading: (state, action) => {
      state.loading = action.payload;
    },
    setError: (state, action) => {
      state.error = action.payload;
    }
  }
});

// Export actions and reducer
export const { addTodo, toggleTodo, deleteTodo, setLoading, setError } = todoSlice.actions;
export default todoSlice.reducer;

// Configure Store
const store = configureStore({
  reducer: {
    todos: todoSlice.reducer
  }
});

// Async Thunk
import { createAsyncThunk } from '@reduxjs/toolkit';

export const fetchTodos = createAsyncThunk(
  'todos/fetchTodos',
  async () => {
    const response = await fetch('/api/todos');
    return response.json();
  }
);

// Updated Slice with Extra Reducers
const todoSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    loading: false,
    error: null
  },
  reducers: {
    // ... existing reducers
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

// Component with Redux Toolkit
import { useSelector, useDispatch } from 'react-redux';

function TodoApp() {
  const dispatch = useDispatch();
  const { items, loading, error } = useSelector(state => state.todos);
  const [inputValue, setInputValue] = useState('');

  useEffect(() => {
    dispatch(fetchTodos());
  }, [dispatch]);

  const handleAddTodo = () => {
    if (inputValue.trim()) {
      dispatch(addTodo(inputValue));
      setInputValue('');
    }
  };

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div>
      <div>
        <input
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add a todo"
        />
        <button onClick={handleAddTodo}>Add</button>
      </div>
      
      <ul>
        {items.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => dispatch(toggleTodo(todo.id))}
            />
            <span style={{ 
              textDecoration: todo.completed ? 'line-through' : 'none' 
            }}>
              {todo.text}
            </span>
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </div>
  );
}
```

---

## Advanced React Concepts

### Q13. What are Higher-Order Components (HOCs)?
**Answer:** HOCs are functions that take a component and return a new component with additional props or behavior.

**Example:**
```jsx
// HOC for authentication
function withAuth(WrappedComponent) {
  return function AuthenticatedComponent(props) {
    const [isAuthenticated, setIsAuthenticated] = useState(false);
    const [user, setUser] = useState(null);

    useEffect(() => {
      // Check authentication status
      const checkAuth = async () => {
        try {
          const response = await fetch('/api/auth/status');
          const authData = await response.json();
          setIsAuthenticated(authData.isAuthenticated);
          setUser(authData.user);
        } catch (error) {
          setIsAuthenticated(false);
        }
      };
      
      checkAuth();
    }, []);

    if (!isAuthenticated) {
      return <div>Please log in to access this page.</div>;
    }

    return <WrappedComponent {...props} user={user} />;
  };
}

// HOC for loading states
function withLoading(WrappedComponent) {
  return function LoadingComponent(props) {
    const [loading, setLoading] = useState(true);
    const [data, setData] = useState(null);

    useEffect(() => {
      const fetchData = async () => {
        try {
          setLoading(true);
          const response = await fetch(props.dataUrl);
          const result = await response.json();
          setData(result);
        } catch (error) {
          console.error('Error fetching data:', error);
        } finally {
          setLoading(false);
        }
      };

      fetchData();
    }, [props.dataUrl]);

    if (loading) {
      return <div>Loading...</div>;
    }

    return <WrappedComponent {...props} data={data} />;
  };
}

// Usage
const AuthenticatedUserProfile = withAuth(withLoading(UserProfile));
```

### Q14. What are Render Props?
**Answer:** Render props is a technique for sharing code between components using a prop whose value is a function.

**Example:**
```jsx
// Render Prop Component
class MouseTracker extends React.Component {
  constructor(props) {
    super(props);
    this.state = { x: 0, y: 0 };
  }

  handleMouseMove = (event) => {
    this.setState({
      x: event.clientX,
      y: event.clientY
    });
  }

  render() {
    return (
      <div style={{ height: '100vh' }} onMouseMove={this.handleMouseMove}>
        {this.props.render(this.state)}
      </div>
    );
  }
}

// Usage
function App() {
  return (
    <div>
      <MouseTracker
        render={({ x, y }) => (
          <h1>The mouse position is ({x}, {y})</h1>
        )}
      />
      
      <MouseTracker
        render={({ x, y }) => (
          <div style={{ position: 'absolute', left: x, top: y }}>
            🐭
          </div>
        )}
      />
    </div>
  );
}

// Function Component with Hooks
function MouseTracker({ render }) {
  const [position, setPosition] = useState({ x: 0, y: 0 });

  const handleMouseMove = useCallback((event) => {
    setPosition({
      x: event.clientX,
      y: event.clientY
    });
  }, []);

  return (
    <div style={{ height: '100vh' }} onMouseMove={handleMouseMove}>
      {render(position)}
    </div>
  );
}
```

### Q15. What is React.memo?
**Answer:** `React.memo` is a higher-order component that memoizes your component, preventing unnecessary re-renders when props haven't changed.

**Example:**
```jsx
import React, { useState, useCallback } from 'react';

// Memoized Component
const ExpensiveComponent = React.memo(({ data, onItemClick }) => {
  console.log('ExpensiveComponent rendered');
  
  return (
    <div>
      {data.map(item => (
        <div key={item.id} onClick={() => onItemClick(item.id)}>
          {item.name}
        </div>
      ))}
    </div>
  );
});

// Parent Component
function ParentComponent() {
  const [count, setCount] = useState(0);
  const [items, setItems] = useState([
    { id: 1, name: 'Item 1' },
    { id: 2, name: 'Item 2' },
    { id: 3, name: 'Item 3' }
  ]);

  // Memoized callback to prevent unnecessary re-renders
  const handleItemClick = useCallback((id) => {
    console.log('Item clicked:', id);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment Count
      </button>
      
      <ExpensiveComponent 
        data={items} 
        onItemClick={handleItemClick} 
      />
    </div>
  );
}
```

---

## Performance Optimization

### Q16. How to optimize React performance?
**Answer:** React performance can be optimized using various techniques:

**1. React.memo for Component Memoization:**
```jsx
const OptimizedComponent = React.memo(({ data }) => {
  return <div>{data.map(item => <span key={item.id}>{item.name}</span>)}</div>;
});
```

**2. useMemo for Expensive Calculations:**
```jsx
function ExpensiveCalculation({ items }) {
  const expensiveValue = useMemo(() => {
    return items.reduce((sum, item) => sum + item.value, 0);
  }, [items]);

  return <div>Total: {expensiveValue}</div>;
}
```

**3. useCallback for Function Memoization:**
```jsx
function ParentComponent() {
  const [count, setCount] = useState(0);
  
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <ChildComponent onButtonClick={handleClick} />
    </div>
  );
}
```

**4. Virtual Scrolling for Large Lists:**
```jsx
import { FixedSizeList as List } from 'react-window';

function VirtualizedList({ items }) {
  const Row = ({ index, style }) => (
    <div style={style}>
      {items[index].name}
    </div>
  );

  return (
    <List
      height={400}
      itemCount={items.length}
      itemSize={35}
      width={300}
    >
      {Row}
    </List>
  );
}
```

**5. Code Splitting:**
```jsx
import React, { lazy, Suspense } from 'react';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}
```

---

## Testing

### Q17. How to test React components?
**Answer:** React components can be tested using Jest and React Testing Library.

**Example:**
```jsx
// Component to test
function Counter({ initialValue = 0 }) {
  const [count, setCount] = useState(initialValue);

  return (
    <div>
      <p data-testid="count">Count: {count}</p>
      <button 
        data-testid="increment" 
        onClick={() => setCount(count + 1)}
      >
        Increment
      </button>
      <button 
        data-testid="decrement" 
        onClick={() => setCount(count - 1)}
      >
        Decrement
      </button>
    </div>
  );
}

// Test file
import { render, screen, fireEvent } from '@testing-library/react';
import '@testing-library/jest-dom';
import Counter from './Counter';

describe('Counter Component', () => {
  test('renders with initial count', () => {
    render(<Counter initialValue={5} />);
    expect(screen.getByTestId('count')).toHaveTextContent('Count: 5');
  });

  test('increments count when increment button is clicked', () => {
    render(<Counter />);
    
    const incrementButton = screen.getByTestId('increment');
    const countElement = screen.getByTestId('count');
    
    fireEvent.click(incrementButton);
    expect(countElement).toHaveTextContent('Count: 1');
  });

  test('decrements count when decrement button is clicked', () => {
    render(<Counter initialValue={5} />);
    
    const decrementButton = screen.getByTestId('decrement');
    const countElement = screen.getByTestId('count');
    
    fireEvent.click(decrementButton);
    expect(countElement).toHaveTextContent('Count: 4');
  });
});
```

---

## Practical Examples

### Q18. Complete Todo Application with React and Redux
**Answer:** Here's a complete Todo application demonstrating React and Redux concepts:

**Redux Store:**
```jsx
// store/todoSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchTodos = createAsyncThunk(
  'todos/fetchTodos',
  async () => {
    const response = await fetch('/api/todos');
    return response.json();
  }
);

export const addTodoAsync = createAsyncThunk(
  'todos/addTodo',
  async (text) => {
    const response = await fetch('/api/todos', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ text })
    });
    return response.json();
  }
);

const todoSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    loading: false,
    error: null,
    filter: 'all'
  },
  reducers: {
    toggleTodo: (state, action) => {
      const todo = state.items.find(todo => todo.id === action.payload);
      if (todo) {
        todo.completed = !todo.completed;
      }
    },
    deleteTodo: (state, action) => {
      state.items = state.items.filter(todo => todo.id !== action.payload);
    },
    setFilter: (state, action) => {
      state.filter = action.payload;
    }
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
      })
      .addCase(addTodoAsync.fulfilled, (state, action) => {
        state.items.push(action.payload);
      });
  }
});

export const { toggleTodo, deleteTodo, setFilter } = todoSlice.actions;
export default todoSlice.reducer;
```

**Components:**
```jsx
// components/TodoApp.jsx
import React, { useState, useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchTodos, addTodoAsync, toggleTodo, deleteTodo, setFilter } from '../store/todoSlice';
import TodoList from './TodoList';
import TodoForm from './TodoForm';
import TodoFilter from './TodoFilter';

function TodoApp() {
  const dispatch = useDispatch();
  const { items, loading, error, filter } = useSelector(state => state.todos);
  const [inputValue, setInputValue] = useState('');

  useEffect(() => {
    dispatch(fetchTodos());
  }, [dispatch]);

  const handleAddTodo = async () => {
    if (inputValue.trim()) {
      await dispatch(addTodoAsync(inputValue));
      setInputValue('');
    }
  };

  const handleToggleTodo = (id) => {
    dispatch(toggleTodo(id));
  };

  const handleDeleteTodo = (id) => {
    dispatch(deleteTodo(id));
  };

  const handleFilterChange = (newFilter) => {
    dispatch(setFilter(newFilter));
  };

  const filteredTodos = items.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;

  return (
    <div className="todo-app">
      <h1>Todo App</h1>
      
      <TodoForm
        value={inputValue}
        onChange={setInputValue}
        onSubmit={handleAddTodo}
      />
      
      <TodoFilter
        currentFilter={filter}
        onFilterChange={handleFilterChange}
      />
      
      <TodoList
        todos={filteredTodos}
        onToggle={handleToggleTodo}
        onDelete={handleDeleteTodo}
      />
      
      <div className="todo-stats">
        <p>Total: {items.length}</p>
        <p>Completed: {items.filter(todo => todo.completed).length}</p>
        <p>Active: {items.filter(todo => !todo.completed).length}</p>
      </div>
    </div>
  );
}

export default TodoApp;
```

```jsx
// components/TodoForm.jsx
import React from 'react';

function TodoForm({ value, onChange, onSubmit }) {
  const handleSubmit = (e) => {
    e.preventDefault();
    onSubmit();
  };

  return (
    <form onSubmit={handleSubmit} className="todo-form">
      <input
        type="text"
        value={value}
        onChange={(e) => onChange(e.target.value)}
        placeholder="Add a new todo..."
        className="todo-input"
      />
      <button type="submit" className="todo-button">
        Add Todo
      </button>
    </form>
  );
}

export default TodoForm;
```

```jsx
// components/TodoList.jsx
import React from 'react';
import TodoItem from './TodoItem';

function TodoList({ todos, onToggle, onDelete }) {
  if (todos.length === 0) {
    return <p>No todos found.</p>;
  }

  return (
    <ul className="todo-list">
      {todos.map(todo => (
        <TodoItem
          key={todo.id}
          todo={todo}
          onToggle={onToggle}
          onDelete={onDelete}
        />
      ))}
    </ul>
  );
}

export default TodoList;
```

```jsx
// components/TodoItem.jsx
import React from 'react';

function TodoItem({ todo, onToggle, onDelete }) {
  return (
    <li className={`todo-item ${todo.completed ? 'completed' : ''}`}>
      <input
        type="checkbox"
        checked={todo.completed}
        onChange={() => onToggle(todo.id)}
        className="todo-checkbox"
      />
      <span className="todo-text">{todo.text}</span>
      <button
        onClick={() => onDelete(todo.id)}
        className="todo-delete"
      >
        Delete
      </button>
    </li>
  );
}

export default TodoItem;
```

```jsx
// components/TodoFilter.jsx
import React from 'react';

function TodoFilter({ currentFilter, onFilterChange }) {
  const filters = [
    { key: 'all', label: 'All' },
    { key: 'active', label: 'Active' },
    { key: 'completed', label: 'Completed' }
  ];

  return (
    <div className="todo-filter">
      {filters.map(filter => (
        <button
          key={filter.key}
          onClick={() => onFilterChange(filter.key)}
          className={`filter-button ${currentFilter === filter.key ? 'active' : ''}`}
        >
          {filter.label}
        </button>
      ))}
    </div>
  );
}

export default TodoFilter;
```

This comprehensive guide covers the most important React.js and Redux concepts with practical examples. Each question includes detailed explanations and working code examples that you can use for interview preparation or as a reference for your projects.