# Creating a Custom React Context to Pass Data

React Context provides a way to share data between components without having to explicitly pass props through every level of the component tree. Here's a comprehensive guide to creating and using a custom React Context.

## Basic Principles of React Context

- Context provides a way to pass data through the component tree without manually passing props
- It's designed to share data that is considered "global" for a tree of React components
- Context helps avoid "prop drilling" (passing props through many levels of components)

## Creating a Custom Context

Let's create a simple theme context that will allow components to access and update theme information:

```jsx
// ThemeContext.js
import { createContext, useState, useContext } from 'react';

// Create the context with a default value
const ThemeContext = createContext(null);

// Create a provider component
export function ThemeProvider({ children }) {
  const [theme, setTheme] = useState('light');

  // Value object that will be shared
  const value = {
    theme,
    toggleTheme: () => setTheme(theme === 'light' ? 'dark' : 'light')
  };

  return (
    <ThemeContext.Provider value={value}>
      {children}
    </ThemeContext.Provider>
  );
}

// Custom hook for using this context
export function useTheme() {
  const context = useContext(ThemeContext);
  if (context === null) {
    throw new Error('useTheme must be used within a ThemeProvider');
  }
  return context;
}
```

## Using Your Custom Context

Here's how to use your new context in your application:

```jsx
// App.js
import React from 'react';
import { ThemeProvider } from './ThemeContext';
import ThemedButton from './ThemedButton';
import ThemedHeader from './ThemedHeader';

function App() {
  return (
    <ThemeProvider>
      <div className="app">
        <ThemedHeader />
        <ThemedButton />
      </div>
    </ThemeProvider>
  );
}

export default App;
```

And then in your components:

```jsx
// ThemedButton.js
import React from 'react';
import { useTheme } from './ThemeContext';

function ThemedButton() {
  const { theme, toggleTheme } = useTheme();
  
  return (
    <button
      onClick={toggleTheme}
      style={{
        background: theme === 'light' ? '#fff' : '#333',
        color: theme === 'light' ? '#333' : '#fff',
        padding: '10px 15px',
        border: '1px solid #ccc',
        borderRadius: '4px'
      }}
    >
      Toggle Theme (Current: {theme})
    </button>
  );
}

export default ThemedButton;
```

## Context with Multiple Values

Let's create a more complex user context that manages user authentication:

```jsx
// UserContext.js
import { createContext, useState, useContext, useEffect } from 'react';

// Create context with a default value
const UserContext = createContext(null);

export function UserProvider({ children }) {
  const [user, setUser] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  // Simulate checking for a logged-in user
  useEffect(() => {
    const checkLoggedInUser = async () => {
      try {
        setLoading(true);
        // Simulate API call to get user data
        const storedUser = localStorage.getItem('user');
        
        if (storedUser) {
          setUser(JSON.parse(storedUser));
        }
      } catch (err) {
        setError('Failed to fetch user');
        console.error(err);
      } finally {
        setLoading(false);
      }
    };

    checkLoggedInUser();
  }, []);

  const login = async (credentials) => {
    try {
      setLoading(true);
      // Simulate API call for login
      const response = await mockLoginAPI(credentials);
      setUser(response.user);
      localStorage.setItem('user', JSON.stringify(response.user));
      return true;
    } catch (err) {
      setError('Login failed');
      console.error(err);
      return false;
    } finally {
      setLoading(false);
    }
  };

  const logout = () => {
    setUser(null);
    localStorage.removeItem('user');
  };

  // Helper function to simulate API
  const mockLoginAPI = (credentials) => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (credentials.username === 'demo' && credentials.password === 'password') {
          resolve({
            user: {
              id: 1,
              username: 'demo',
              name: 'Demo User',
              email: 'demo@example.com'
            }
          });
        } else {
          reject(new Error('Invalid credentials'));
        }
      }, 1000);
    });
  };

  const value = {
    user,
    loading,
    error,
    login,
    logout
  };

  return <UserContext.Provider value={value}>{children}</UserContext.Provider>;
}

// Custom hook to use the user context
export function useUser() {
  const context = useContext(UserContext);
  if (context === null) {
    throw new Error('useUser must be used within a UserProvider');
  }
  return context;
}
```

## Using the User Context

```jsx
// App.js
import React from 'react';
import { UserProvider } from './UserContext';
import { ThemeProvider } from './ThemeContext';
import Dashboard from './Dashboard';
import LoginPage from './LoginPage';

function App() {
  return (
    <ThemeProvider>
      <UserProvider>
        <AppContent />
      </UserProvider>
    </ThemeProvider>
  );
}

function AppContent() {
  const { user, loading } = useUser();

  if (loading) return <div>Loading...</div>;
  
  return user ? <Dashboard /> : <LoginPage />;
}

export default App;
```

Login component:

```jsx
// LoginPage.js
import React, { useState } from 'react';
import { useUser } from './UserContext';
import { useTheme } from './ThemeContext';

function LoginPage() {
  const [username, setUsername] = useState('');
  const [password, setPassword] = useState('');
  const { login, error } = useUser();
  const { theme } = useTheme();

  const handleSubmit = async (e) => {
    e.preventDefault();
    await login({ username, password });
  };

  const styles = {
    container: {
      maxWidth: '400px',
      margin: '0 auto',
      padding: '20px',
      backgroundColor: theme === 'light' ? '#fff' : '#333',
      color: theme === 'light' ? '#333' : '#fff',
      borderRadius: '5px',
      boxShadow: '0 2px 5px rgba(0, 0, 0, 0.1)'
    },
    input: {
      width: '100%',
      padding: '10px',
      marginBottom: '15px',
      borderRadius: '4px',
      border: '1px solid #ddd',
      backgroundColor: theme === 'light' ? '#fff' : '#555',
      color: theme === 'light' ? '#333' : '#fff'
    },
    button: {
      padding: '10px 15px',
      backgroundColor: '#0066ff',
      color: 'white',
      border: 'none',
      borderRadius: '4px',
      cursor: 'pointer'
    }
  };

  return (
    <div style={styles.container}>
      <h2>Login</h2>
      {error && <p style={{ color: 'red' }}>{error}</p>}
      <form onSubmit={handleSubmit}>
        <div>
          <input
            type="text"
            placeholder="Username"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
            style={styles.input}
          />
        </div>
        <div>
          <input
            type="password"
            placeholder="Password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            style={styles.input}
          />
        </div>
        <button type="submit" style={styles.button}>
          Login
        </button>
      </form>
    </div>
  );
}

export default LoginPage;
```

## Best Practices for React Context

1. **Don't Overuse Context**: 
   - Context is primarily designed for data that needs to be accessed by many components
   - For simple prop passing, regular props might be more appropriate

2. **Split Contexts by Domain**:
   - Create separate contexts for unrelated parts of your application
   - This prevents unnecessary re-renders when only one piece of context changes

3. **Provide Default Values**:
   - Always give your context a meaningful default value
   - This helps with type checking and avoids null checking everywhere

4. **Create Custom Hooks**:
   - Wrap the `useContext` call in a custom hook (like `useTheme` or `useUser`)
   - This adds better error messages and makes context usage more explicit

5. **Structure Provider Components**:
   - Keep your provider components clean and focused
   - Separate complex logic into helper functions or custom hooks

6. **Optimize for Performance**:
   - Use React's memoization techniques to prevent unnecessary re-renders
   - Consider using `React.memo`, `useMemo`, and `useCallback` with context values

## Context with useReducer for Complex State

For more complex state management, combine context with useReducer:

```jsx
// ShoppingCartContext.js
import { createContext, useContext, useReducer } from 'react';

// Initial state
const initialState = {
  items: [],
  total: 0
};

// Reducer function
function cartReducer(state, action) {
  switch (action.type) {
    case 'ADD_ITEM':
      const existingItemIndex = state.items.findIndex(
        item => item.id === action.payload.id
      );

      if (existingItemIndex >= 0) {
        // Item exists, increase quantity
        const newItems = [...state.items];
        newItems[existingItemIndex] = {
          ...newItems[existingItemIndex],
          quantity: newItems[existingItemIndex].quantity + 1
        };

        return {
          ...state,
          items: newItems,
          total: state.total + action.payload.price
        };
      } else {
        // New item
        return {
          ...state,
          items: [...state.items, { ...action.payload, quantity: 1 }],
          total: state.total + action.payload.price
        };
      }

    case 'REMOVE_ITEM':
      const itemToRemove = state.items.find(item => item.id === action.payload);
      const newItems = state.items.filter(item => item.id !== action.payload);
      
      return {
        ...state,
        items: newItems,
        total: state.total - (itemToRemove.price * itemToRemove.quantity)
      };

    case 'CLEAR_CART':
      return initialState;

    default:
      throw new Error(`Unhandled action type: ${action.type}`);
  }
}

// Create context
const ShoppingCartContext = createContext(null);

// Provider component
export function ShoppingCartProvider({ children }) {
  const [state, dispatch] = useReducer(cartReducer, initialState);

  // Actions
  const addItem = (item) => dispatch({ type: 'ADD_ITEM', payload: item });
  const removeItem = (itemId) => dispatch({ type: 'REMOVE_ITEM', payload: itemId });
  const clearCart = () => dispatch({ type: 'CLEAR_CART' });

  const value = {
    items: state.items,
    total: state.total,
    addItem,
    removeItem,
    clearCart
  };

  return (
    <ShoppingCartContext.Provider value={value}>
      {children}
    </ShoppingCartContext.Provider>
  );
}

// Custom hook
export function useShoppingCart() {
  const context = useContext(ShoppingCartContext);
  if (context === null) {
    throw new Error('useShoppingCart must be used within a ShoppingCartProvider');
  }
  return context;
}
```

## Conclusion

React Context is a powerful feature for sharing state across your application without prop drilling. By creating custom contexts for different parts of your application, you can create a clean architecture that's easy to maintain and understand.

Remember these key points:
- Create context with `createContext()`
- Provide values using `<Context.Provider value={value}>`
- Access values in components with `useContext()` or a custom hook
- Organize related state and functions in a single context
- Split unrelated state into separate contexts
- Use `useReducer` with context for complex state management

When used correctly, context can greatly simplify your React applications and make them more maintainable.