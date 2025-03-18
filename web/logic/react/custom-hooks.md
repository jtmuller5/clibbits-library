# Creating Custom Hooks in React

## Introduction

Custom hooks are a powerful feature in React that allow you to extract component logic into reusable functions. Custom hooks let you reuse stateful logic between components without duplicating code or adding unnecessary complexity.

## Basic Principles

- Custom hooks are JavaScript functions that start with the word "use" (e.g., `useCustomHook`)
- They can call other hooks (built-in or custom)
- They allow you to share logic across components
- They don't share state between components - each call to a hook creates isolated state

## Creating Your First Custom Hook

Let's create a simple custom hook that manages a counter:

```jsx
// useCounter.js
import { useState } from 'react';

function useCounter(initialValue = 0) {
  const [count, setCount] = useState(initialValue);
  
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  const reset = () => setCount(initialValue);
  
  return { count, increment, decrement, reset };
}

export default useCounter;
```

## Using Your Custom Hook

Here's how to use your new custom hook in a component:

```jsx
// CounterComponent.jsx
import React from 'react';
import useCounter from './useCounter';

function CounterComponent() {
  const { count, increment, decrement, reset } = useCounter(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

export default CounterComponent;
```

## Custom Hook with API Data Fetching

Let's create a more useful hook for fetching data from an API:

```jsx
// useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState(null);

  useEffect(() => {
    const fetchData = async () => {
      try {
        setLoading(true);
        const response = await fetch(url);
        
        if (!response.ok) {
          throw new Error(`HTTP error! Status: ${response.status}`);
        }
        
        const result = await response.json();
        setData(result);
        setError(null);
      } catch (error) {
        setError(error.message);
        setData(null);
      } finally {
        setLoading(false);
      }
    };

    fetchData();
  }, [url]);

  return { data, loading, error };
}

export default useFetch;
```

## Using the Fetch Hook

```jsx
// DataComponent.jsx
import React from 'react';
import useFetch from './useFetch';

function DataComponent() {
  const { data, loading, error } = useFetch('https://api.example.com/data');

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  
  return (
    <div>
      <h2>Data Retrieved:</h2>
      <pre>{JSON.stringify(data, null, 2)}</pre>
    </div>
  );
}

export default DataComponent;
```

## Custom Hook with LocalStorage

Here's a hook for persisting state to localStorage:

```jsx
// useLocalStorage.js
import { useState, useEffect } from 'react';

function useLocalStorage(key, initialValue) {
  // Get from local storage then parse stored json or return initialValue
  const readValue = () => {
    if (typeof window === 'undefined') {
      return initialValue;
    }

    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      console.warn(`Error reading localStorage key "${key}":`, error);
      return initialValue;
    }
  };

  // State to store our value
  const [storedValue, setStoredValue] = useState(readValue);

  // Return a wrapped version of useState's setter function that persists the new value to localStorage
  const setValue = (value) => {
    try {
      // Allow value to be a function so we have the same API as useState
      const valueToStore =
        value instanceof Function ? value(storedValue) : value;
      
      // Save state
      setStoredValue(valueToStore);
      
      // Save to local storage
      if (typeof window !== 'undefined') {
        window.localStorage.setItem(key, JSON.stringify(valueToStore));
      }
    } catch (error) {
      console.warn(`Error setting localStorage key "${key}":`, error);
    }
  };

  useEffect(() => {
    setStoredValue(readValue());
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  return [storedValue, setValue];
}

export default useLocalStorage;
```

## Best Practices for Custom Hooks

1. **Naming Convention**: Always start custom hook names with "use" (e.g., `useFormInput`, `useWindowSize`)

2. **Single Responsibility**: Each hook should focus on one specific functionality

3. **Composability**: Smaller hooks can be combined to create more complex ones

4. **Testing**: Custom hooks can be tested independently from components using libraries like `@testing-library/react-hooks`

5. **Documentation**: Document the hook's purpose, parameters, return values, and provide examples

## Example: Composing Multiple Hooks

```jsx
// useUserData.js
import { useState } from 'react';
import useFetch from './useFetch';
import useLocalStorage from './useLocalStorage';

function useUserData(userId) {
  const [savedUserData, setSavedUserData] = useLocalStorage(`user-${userId}`, null);
  const { data, loading, error } = useFetch(`https://api.example.com/users/${userId}`);
  const [isEditing, setIsEditing] = useState(false);
  
  // Use the cached data if available, otherwise use the fetched data
  const userData = savedUserData || data;
  
  const saveUserData = (newData) => {
    setSavedUserData(newData);
    setIsEditing(false);
  };
  
  return {
    userData,
    loading,
    error,
    isEditing,
    setIsEditing,
    saveUserData
  };
}

export default useUserData;
```

## Conclusion

Custom hooks are a powerful pattern in React that enable cleaner, more reusable code. By extracting component logic into custom hooks, you can simplify your components, avoid duplication, and create a library of reusable functionality for your application.

Remember that custom hooks don't share state - each component that uses a hook gets its own isolated state. This allows for maximum flexibility while maintaining React's compositional model.