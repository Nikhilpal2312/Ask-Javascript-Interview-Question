# React.js and Redux Interview Questions & Answers

## Table of Contents
1. [React.js Interview Questions](#reactjs-interview-questions)
2. [Redux Interview Questions](#redux-interview-questions)
3. [React + Redux Integration Questions](#react--redux-integration-questions)
4. [Practical Coding Examples](#practical-coding-examples)

---

## React.js Interview Questions

### 1. What is React.js and what are its key features?

**Answer:** React.js is a JavaScript library for building user interfaces, particularly web applications. It was developed by Facebook.

**Key Features:**
- **Virtual DOM**: Efficient rendering through virtual representation of the real DOM
- **Component-Based**: Build encapsulated components that manage their own state
- **JSX**: JavaScript syntax extension that allows HTML-like syntax
- **One-way Data Binding**: Data flows down from parent to child components
- **Lifecycle Methods**: Control component behavior during mounting, updating, and unmounting

**Example:**
```jsx
import React, { useState } from 'react';

function WelcomeComponent({ name }) {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <h1>Hello, {name}!</h1>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}

export default WelcomeComponent;
```

### 2. What is the difference between functional and class components?

**Answer:**

**Class Components:**
- Use ES6 class syntax
- Have lifecycle methods
- Use `this.state` for state management
- More verbose

**Functional Components:**
- Use function syntax
- Use React Hooks for state and lifecycle
- Simpler and more concise
- Better performance

**Example:**

```jsx
// Class Component
class ClassComponent extends React.Component {
  constructor(props) {
    super(props);
    this.state = { count: 0 };
  }
  
  componentDidMount() {
    console.log('Component mounted');
  }
  
  render() {
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

// Functional Component
import React, { useState, useEffect } from 'react';

function FunctionalComponent() {
  const [count, setCount] = useState(0);
  
  useEffect(() => {
    console.log('Component mounted');
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

### 3. What are React Hooks? Explain useState and useEffect.

**Answer:** Hooks are functions that let you use state and other React features in functional components.

**useState:** Manages local state in functional components
**useEffect:** Handles side effects (API calls, subscriptions, timers)

**Example:**
```jsx
import React, { useState, useEffect } from 'react';

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);
  
  useEffect(() => {
    const fetchUser = async () => {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const userData = await response.json();
        setUser(userData);
      } catch (err) {
        setError(err.message);
      } finally {
        setLoading(false);
      }
    };
    
    fetchUser();
  }, [userId]); // Dependency array - effect runs when userId changes
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h2>{user?.name}</h2>
      <p>{user?.email}</p>
    </div>
  );
}
```

### 4. What is the Virtual DOM and how does it work?

**Answer:** The Virtual DOM is a JavaScript representation of the real DOM. React creates a virtual copy of the DOM in memory and uses a diffing algorithm to determine the minimal set of changes needed to update the real DOM.

**Process:**
1. When state changes, React creates a new Virtual DOM tree
2. Compares (diffs) the new tree with the previous Virtual DOM tree
3. Calculates the minimum changes needed
4. Updates only the changed elements in the real DOM

**Example:**
```jsx
function TodoList() {
  const [todos, setTodos] = useState([
    { id: 1, text: 'Learn React', completed: false },
    { id: 2, text: 'Build an app', completed: false }
  ]);
  
  const toggleTodo = (id) => {
    setTodos(todos.map(todo => 
      todo.id === id ? { ...todo, completed: !todo.completed } : todo
    ));
    // React will only update the specific todo item that changed
    // instead of re-rendering the entire list
  };
  
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <input 
            type="checkbox" 
            checked={todo.completed}
            onChange={() => toggleTodo(todo.id)}
          />
          {todo.text}
        </li>
      ))}
    </ul>
  );
}
```

### 5. What is prop drilling and how can you avoid it?

**Answer:** Prop drilling occurs when you pass data through multiple component layers that don't need the data, just to get it to a deeply nested component.

**Solutions:**
- Context API
- Redux
- Component composition

**Example:**

```jsx
// Prop Drilling Problem
function App() {
  const [user, setUser] = useState({ name: 'John', theme: 'dark' });
  
  return <Layout user={user} />;
}

function Layout({ user }) {
  return <Header user={user} />; // Passing user just to pass it down
}

function Header({ user }) {
  return <UserProfile user={user} />; // Still passing it down
}

function UserProfile({ user }) {
  return <div>Welcome, {user.name}!</div>; // Finally using it
}

// Solution using Context API
const UserContext = React.createContext();

function App() {
  const [user, setUser] = useState({ name: 'John', theme: 'dark' });
  
  return (
    <UserContext.Provider value={user}>
      <Layout />
    </UserContext.Provider>
  );
}

function Layout() {
  return <Header />; // No prop passing needed
}

function Header() {
  return <UserProfile />; // No prop passing needed
}

function UserProfile() {
  const user = useContext(UserContext); // Direct access to user
  return <div>Welcome, {user.name}!</div>;
}
```

---

## Redux Interview Questions

### 1. What is Redux and what are its core principles?

**Answer:** Redux is a predictable state container for JavaScript apps. It helps manage application state in a consistent way.

**Three Core Principles:**
1. **Single Source of Truth**: The global state is stored in a single store
2. **State is Read-Only**: State can only be changed by dispatching actions
3. **Changes are Made with Pure Functions**: Reducers are pure functions that specify how the state changes

**Example:**
```javascript
// Action Types
const INCREMENT = 'INCREMENT';
const DECREMENT = 'DECREMENT';

// Action Creators
const increment = () => ({ type: INCREMENT });
const decrement = () => ({ type: DECREMENT });

// Reducer (Pure Function)
const counterReducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case INCREMENT:
      return { ...state, count: state.count + 1 };
    case DECREMENT:
      return { ...state, count: state.count - 1 };
    default:
      return state;
  }
};

// Store
import { createStore } from 'redux';
const store = createStore(counterReducer);

// Usage
store.dispatch(increment()); // { count: 1 }
store.dispatch(increment()); // { count: 2 }
store.dispatch(decrement()); // { count: 1 }
```

### 2. What are Actions, Reducers, and Store in Redux?

**Answer:**

**Actions:** Plain JavaScript objects that describe what happened
**Reducers:** Pure functions that specify how the state changes in response to actions
**Store:** Holds the application state and provides methods to access and update it

**Example:**
```javascript
// Actions
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
  payload: { id }
});

const deleteTodo = (id) => ({
  type: DELETE_TODO,
  payload: { id }
});

// Reducer
const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
      
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload.id
          ? { ...todo, completed: !todo.completed }
          : todo
      );
      
    case DELETE_TODO:
      return state.filter(todo => todo.id !== action.payload.id);
      
    default:
      return state;
  }
};

// Store
import { createStore } from 'redux';
const store = createStore(todosReducer);

// Usage
store.dispatch(addTodo('Learn Redux'));
store.dispatch(addTodo('Build a project'));
store.dispatch(toggleTodo(store.getState()[0].id));
```

### 3. What is middleware in Redux? Give an example of Redux Thunk.

**Answer:** Middleware provides a way to extend Redux with custom functionality. It sits between dispatching an action and the moment it reaches the reducer.

**Redux Thunk** allows you to dispatch functions (thunks) instead of plain action objects, enabling asynchronous actions.

**Example:**
```javascript
// Without Thunk (synchronous)
const setUser = (user) => ({
  type: 'SET_USER',
  payload: user
});

// With Thunk (asynchronous)
const fetchUser = (userId) => {
  return async (dispatch, getState) => {
    dispatch({ type: 'FETCH_USER_START' });
    
    try {
      const response = await fetch(`/api/users/${userId}`);
      const user = await response.json();
      
      dispatch({
        type: 'FETCH_USER_SUCCESS',
        payload: user
      });
    } catch (error) {
      dispatch({
        type: 'FETCH_USER_ERROR',
        payload: error.message
      });
    }
  };
};

// Store setup with Thunk
import { createStore, applyMiddleware } from 'redux';
import thunk from 'redux-thunk';

const store = createStore(
  userReducer,
  applyMiddleware(thunk)
);

// Usage
store.dispatch(fetchUser(123)); // This now works with async function
```

### 4. What's the difference between Redux and Context API?

**Answer:**

| Redux | Context API |
|-------|-------------|
| External library | Built into React |
| Global state management | Component tree state sharing |
| Time travel debugging | No built-in debugging |
| Middleware support | No middleware |
| Better for complex state | Better for simple state |
| Predictable state updates | Direct state updates |

**Example:**

```jsx
// Redux approach
const store = createStore(reducer);

function App() {
  return (
    <Provider store={store}>
      <TodoApp />
    </Provider>
  );
}

// Context API approach
const TodoContext = createContext();

function TodoProvider({ children }) {
  const [todos, setTodos] = useState([]);
  
  const addTodo = (text) => {
    setTodos([...todos, { id: Date.now(), text, completed: false }]);
  };
  
  return (
    <TodoContext.Provider value={{ todos, addTodo }}>
      {children}
    </TodoContext.Provider>
  );
}

function App() {
  return (
    <TodoProvider>
      <TodoApp />
    </TodoProvider>
  );
}
```

---

## React + Redux Integration Questions

### 1. How do you connect React components to Redux store?

**Answer:** Use React-Redux library with `Provider`, `useSelector`, and `useDispatch` hooks (or legacy `connect` function).

**Example:**
```jsx
// Store setup
import { createStore } from 'redux';
import { Provider } from 'react-redux';

const store = createStore(todosReducer);

// App component
function App() {
  return (
    <Provider store={store}>
      <TodoApp />
    </Provider>
  );
}

// Connected component using hooks
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
      <AddTodoForm onAdd={handleAddTodo} />
      <ul>
        {todos.map(todo => (
          <li key={todo.id}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => handleToggleTodo(todo.id)}
            />
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

// Legacy connect approach
import { connect } from 'react-redux';

const mapStateToProps = (state) => ({
  todos: state.todos
});

const mapDispatchToProps = {
  addTodo,
  toggleTodo
};

export default connect(mapStateToProps, mapDispatchToProps)(TodoList);
```

### 2. What is Redux Toolkit and why should you use it?

**Answer:** Redux Toolkit (RTK) is the official, opinionated, batteries-included toolset for efficient Redux development. It simplifies Redux usage and includes best practices by default.

**Benefits:**
- Reduces boilerplate code
- Built-in Immer for immutable updates
- Built-in Redux Thunk
- DevTools integration
- TypeScript support

**Example:**
```javascript
// Traditional Redux
const ADD_TODO = 'ADD_TODO';
const TOGGLE_TODO = 'TOGGLE_TODO';

const addTodo = (text) => ({
  type: ADD_TODO,
  payload: { id: Date.now(), text, completed: false }
});

const todosReducer = (state = [], action) => {
  switch (action.type) {
    case ADD_TODO:
      return [...state, action.payload];
    case TOGGLE_TODO:
      return state.map(todo =>
        todo.id === action.payload.id
          ? { ...todo, completed: !todo.completed }
          : todo
      );
    default:
      return state;
  }
};

// Redux Toolkit
import { createSlice, configureStore } from '@reduxjs/toolkit';

const todosSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      // Immer allows "mutation" syntax
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
    }
  }
});

export const { addTodo, toggleTodo } = todosSlice.actions;

const store = configureStore({
  reducer: {
    todos: todosSlice.reducer
  }
});
```

---

## Practical Coding Examples

### Complete Todo App with React and Redux

```jsx
// store/todosSlice.js
import { createSlice } from '@reduxjs/toolkit';

const todosSlice = createSlice({
  name: 'todos',
  initialState: {
    items: [],
    filter: 'all' // 'all', 'active', 'completed'
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
    setFilter: (state, action) => {
      state.filter = action.payload;
    }
  }
});

export const { addTodo, toggleTodo, deleteTodo, setFilter } = todosSlice.actions;
export default todosSlice.reducer;

// store/index.js
import { configureStore } from '@reduxjs/toolkit';
import todosReducer from './todosSlice';

export const store = configureStore({
  reducer: {
    todos: todosReducer
  }
});

// components/TodoApp.jsx
import React from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { addTodo, toggleTodo, deleteTodo, setFilter } from '../store/todosSlice';

function TodoApp() {
  const { items, filter } = useSelector(state => state.todos);
  const dispatch = useDispatch();
  
  const [inputValue, setInputValue] = React.useState('');
  
  const filteredTodos = items.filter(todo => {
    if (filter === 'active') return !todo.completed;
    if (filter === 'completed') return todo.completed;
    return true;
  });
  
  const handleSubmit = (e) => {
    e.preventDefault();
    if (inputValue.trim()) {
      dispatch(addTodo(inputValue.trim()));
      setInputValue('');
    }
  };
  
  return (
    <div className="todo-app">
      <h1>Todo App</h1>
      
      <form onSubmit={handleSubmit}>
        <input
          type="text"
          value={inputValue}
          onChange={(e) => setInputValue(e.target.value)}
          placeholder="Add a new todo..."
        />
        <button type="submit">Add</button>
      </form>
      
      <div className="filters">
        <button 
          className={filter === 'all' ? 'active' : ''}
          onClick={() => dispatch(setFilter('all'))}
        >
          All
        </button>
        <button 
          className={filter === 'active' ? 'active' : ''}
          onClick={() => dispatch(setFilter('active'))}
        >
          Active
        </button>
        <button 
          className={filter === 'completed' ? 'active' : ''}
          onClick={() => dispatch(setFilter('completed'))}
        >
          Completed
        </button>
      </div>
      
      <ul className="todo-list">
        {filteredTodos.map(todo => (
          <li key={todo.id} className={todo.completed ? 'completed' : ''}>
            <input
              type="checkbox"
              checked={todo.completed}
              onChange={() => dispatch(toggleTodo(todo.id))}
            />
            <span>{todo.text}</span>
            <button onClick={() => dispatch(deleteTodo(todo.id))}>
              Delete
            </button>
          </li>
        ))}
      </ul>
      
      <div className="stats">
        Total: {items.length} | 
        Active: {items.filter(t => !t.completed).length} | 
        Completed: {items.filter(t => t.completed).length}
      </div>
    </div>
  );
}

export default TodoApp;

// App.js
import React from 'react';
import { Provider } from 'react-redux';
import { store } from './store';
import TodoApp from './components/TodoApp';
import './App.css';

function App() {
  return (
    <Provider store={store}>
      <TodoApp />
    </Provider>
  );
}

export default App;
```

### Async Data Fetching Example

```jsx
// store/usersSlice.js
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

// Async thunk for fetching users
export const fetchUsers = createAsyncThunk(
  'users/fetchUsers',
  async (_, { rejectWithValue }) => {
    try {
      const response = await fetch('/api/users');
      if (!response.ok) {
        throw new Error('Failed to fetch users');
      }
      return await response.json();
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

const usersSlice = createSlice({
  name: 'users',
  initialState: {
    data: [],
    loading: false,
    error: null
  },
  reducers: {
    clearError: (state) => {
      state.error = null;
    }
  },
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
        state.error = null;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.payload;
      });
  }
});

export const { clearError } = usersSlice.actions;
export default usersSlice.reducer;

// components/UserList.jsx
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { fetchUsers, clearError } from '../store/usersSlice';

function UserList() {
  const { data: users, loading, error } = useSelector(state => state.users);
  const dispatch = useDispatch();
  
  useEffect(() => {
    dispatch(fetchUsers());
  }, [dispatch]);
  
  if (loading) return <div>Loading users...</div>;
  
  if (error) {
    return (
      <div>
        <p>Error: {error}</p>
        <button onClick={() => dispatch(clearError())}>
          Clear Error
        </button>
        <button onClick={() => dispatch(fetchUsers())}>
          Retry
        </button>
      </div>
    );
  }
  
  return (
    <div>
      <h2>Users</h2>
      <button onClick={() => dispatch(fetchUsers())}>
        Refresh
      </button>
      <ul>
        {users.map(user => (
          <li key={user.id}>
            <strong>{user.name}</strong> - {user.email}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default UserList;
```

## Additional Interview Topics

### Performance Optimization
- React.memo()
- useMemo() and useCallback()
- Code splitting with React.lazy()
- Redux selector optimization

### Testing
- Testing React components with React Testing Library
- Testing Redux reducers and actions
- Mocking API calls in tests

### Best Practices
- Component composition vs inheritance
- When to use Redux vs Context API vs local state
- Folder structure and organization
- Error boundaries

This guide covers the most common React and Redux interview questions with practical examples that demonstrate real-world usage patterns.