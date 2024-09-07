# React-React-Native-Cheat-Sheet

## **React/React Native & TypeScript Advanced Cheat Sheet for Senior/Lead Engineers**

---

### **1. Component Composition and Reusability**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  interface ButtonProps {
    label: string;
    onClick: () => void;
  }

  const Button: React.FC<ButtonProps> = ({ label, onClick }) => (
    <button onClick={onClick}>{label}</button>
  );

  const SubmitButton: React.FC<ButtonProps> = (props) => (
    <Button {...props} label="Submit" />
  );
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React from "react";
  import { Text, TouchableOpacity, StyleSheet } from "react-native";

  interface ButtonProps {
    label: string;
    onPress: () => void;
  }

  const Button: React.FC<ButtonProps> = ({ label, onPress }) => (
    <TouchableOpacity style={styles.button} onPress={onPress}>
      <Text style={styles.label}>{label}</Text>
    </TouchableOpacity>
  );

  const SubmitButton: React.FC<ButtonProps> = (props) => (
    <Button {...props} label="Submit" />
  );

  const styles = StyleSheet.create({
    button: {
      backgroundColor: "#007bff",
      padding: 10,
      borderRadius: 5,
    },
    label: {
      color: "#fff",
      textAlign: "center",
    },
  });
  ```

---

### **2. Higher-Order Components (HOCs)**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  import React, { useEffect } from "react";

  function withLogging<P>(WrappedComponent: React.ComponentType<P>) {
    return (props: P) => {
      useEffect(() => {
        console.log("Component mounted");
      }, []);

      return <WrappedComponent {...props} />;
    };
  }

  const LoggedButton = withLogging(Button);
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React, { useEffect } from "react";
  import { View, Text } from "react-native";

  function withLogging<P>(WrappedComponent: React.ComponentType<P>) {
    return (props: P) => {
      useEffect(() => {
        console.log("Component mounted");
      }, []);

      return (
        <View>
          <WrappedComponent {...props} />
        </View>
      );
    };
  }

  const LoggedButton = withLogging(Button);
  ```

---

### **3. Render Props**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  interface DataFetcherProps<T> {
    render: (data: T | null) => JSX.Element;
  }

  const DataFetcher: React.FC<DataFetcherProps<string[]>> = ({ render }) => {
    const [data, setData] = React.useState<string[] | null>(null);

    React.useEffect(() => {
      fetch("/api/data")
        .then((response) => response.json())
        .then((result) => setData(result));
    }, []);

    return render(data);
  };

  <DataFetcher render={(data) => <div>{data?.join(", ")}</div>} />;
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React, { useState, useEffect } from "react";
  import { View, Text } from "react-native";

  interface DataFetcherProps<T> {
    render: (data: T | null) => JSX.Element;
  }

  const DataFetcher: React.FC<DataFetcherProps<string[]>> = ({ render }) => {
    const [data, setData] = useState<string[] | null>(null);

    useEffect(() => {
      fetch("https://example.com/data")
        .then((response) => response.json())
        .then((result) => setData(result));
    }, []);

    return <View>{render(data)}</View>;
  };

  <DataFetcher render={(data) => <Text>{data?.join(", ")}</Text>} />;
  ```

---

### **4. Controlled vs Uncontrolled Components**

#### **React Version**

- **Controlled Components**:

  ```tsx
  const [value, setValue] = React.useState<string>("");

  return <input value={value} onChange={(e) => setValue(e.target.value)} />;
  ```

- **Uncontrolled Components**:

  ```tsx
  const inputRef = React.useRef<HTMLInputElement>(null);

  return <input ref={inputRef} />;
  ```

#### **React Native Version**

- **Controlled Components in React Native**:

  ```tsx
  import React, { useState } from "react";
  import { TextInput, View } from "react-native";

  const ControlledInput = () => {
    const [value, setValue] = useState<string>("");

    return (
      <TextInput
        value={value}
        onChangeText={setValue}
        placeholder="Type here..."
      />
    );
  };
  ```

- **Uncontrolled Components in React Native**:

  ```tsx
  import React, { useRef } from "react";
  import { TextInput, View } from "react-native";

  const UncontrolledInput = () => {
    const inputRef = useRef<TextInput>(null);

    return <TextInput ref={inputRef} placeholder="Uncontrolled input" />;
  };
  ```

---

### **5. State Management Patterns**

#### **Context API with `useReducer`**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  interface State {
    count: number;
  }

  interface Action {
    type: "INCREMENT" | "DECREMENT";
  }

  const initialState: State = { count: 0 };

  const reducer = (state: State, action: Action): State => {
    switch (action.type) {
      case "INCREMENT":
        return { ...state, count: state.count + 1 };
      case "DECREMENT":
        return { ...state, count: state.count - 1 };
      default:
        throw new Error();
    }
  };

  const CounterContext = React.createContext<{
    state: State;
    dispatch: React.Dispatch<Action>;
  } | null>(null);

  const CounterProvider: React.FC = ({ children }) => {
    const [state, dispatch] = React.useReducer(reducer, initialState);
    return (
      <CounterContext.Provider value={{ state, dispatch }}>
        {children}
      </CounterContext.Provider>
    );
  };

  const useCounter = () => {
    const context = React.useContext(CounterContext);
    if (!context)
      throw new Error("useCounter must be used within CounterProvider");
    return context;
  };
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React, {
    useReducer,
    createContext,
    useContext,
    Dispatch,
  } from "react";
  import { Text, View, Button } from "react-native";

  interface State {
    count: number;
  }

  interface Action {
    type: "INCREMENT" | "DECREMENT";
  }

  const initialState: State = { count: 0 };

  const reducer = (state: State, action: Action): State => {
    switch (action.type) {
      case "INCREMENT":
        return { ...state, count: state.count + 1 };
      case "DECREMENT":
        return { ...state, count: state.count - 1 };
      default:
        throw new Error();
    }
  };

  const CounterContext = createContext<{
    state: State;
    dispatch: Dispatch<Action>;
  } | null>(null);

  const CounterProvider: React.FC = ({ children }) => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
      <CounterContext.Provider value={{ state, dispatch }}>
        {children}
      </CounterContext.Provider>
    );
  };

  const useCounter = () => {
    const context = useContext(CounterContext);
    if (!context)
      throw new Error("useCounter must be used within a CounterProvider");
    return context;
  };

  const Counter = () => {
    const { state, dispatch } = useCounter();
    return (
      <View>
        <Text>Count: {state.count}</Text>
        <Button
          title="Increment"
          onPress={() => dispatch({ type: "INCREMENT" })}
        />
        <Button
          title="Decrement"
          onPress={() => dispatch({ type: "DECREMENT" })}
        />
      </View>
    );
  };
  ```

---

### **6. Performance Optimization Techniques**

#### **Memoization (`useMemo`, `useCallback`)**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  const expensiveCalculation = (a: number, b: number) => {
    // Some expensive calculation
    return a + b;
  };

  const Component: React.FC = () => {
    const [a, setA] = React.useState(1);
    const [b, setB] = React.useState(2);

    const memoizedValue = React.useMemo(() => expensiveCalculation(a, b), [a, b]);

    const handleClick = React.useCallback(() => {
      console.log(memoizedValue);
    }, [memoizedValue]);

    return <button onClick={handleClick}>Click me</button>;
 };
 ```

#### **React Native Version**

- **React Native Example with TypeScript**:
```tsx
import React, { useState, useMemo, useCallback } from 'react';
import { Button, View, Text } from 'react-native';

const expensiveCalculation = (a: number, b: number) => {
  return a + b;
};

const MyComponent = () => {
  const [a, setA] = useState(1);
  const [b, setB] = useState(2);

  const memoizedValue = useMemo(() => expensiveCalculation(a, b), [a, b]);

  const handleClick = useCallback(() => {
    console.log(memoizedValue);
  }, [memoizedValue]);

  return (
    <View>
      <Button title="Click me" onPress={handleClick} />
      <Text>{memoizedValue}</Text>
    </View>
  );
};
````

---

### **7. Lazy Loading with `React.lazy` and `Suspense`**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  const LazyComponent = React.lazy(() => import("./LazyComponent"));

  const App: React.FC = () => (
    <React.Suspense fallback={<div>Loading...</div>}>
      <LazyComponent />
    </React.Suspense>
  );
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React, { Suspense } from "react";
  import { View, Text } from "react-native";

  const LazyComponent = React.lazy(() => import("./LazyComponent"));

  const App = () => (
    <Suspense fallback={<Text>Loading...</Text>}>
      <LazyComponent />
    </Suspense>
  );
  ```

---

### **8. Error Boundaries**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  class ErrorBoundary extends React.Component {
    state = { hasError: false };

    static getDerivedStateFromError(error: Error) {
      return { hasError: true };
    }

    componentDidCatch(error: Error, info: React.ErrorInfo) {
      console.error("Error caught by ErrorBoundary:", error, info);
    }

    render() {
      if (this.state.hasError) {
        return <h1>Something went wrong.</h1>;
      }

      return this.props.children;
    }
  }

  const App: React.FC = () => (
    <ErrorBoundary>
      <ComponentThatMayFail />
    </ErrorBoundary>
  );
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import React from "react";
  import { View, Text } from "react-native";

  class ErrorBoundary extends React.Component {
    state = { hasError: false };

    static getDerivedStateFromError(error: Error) {
      return { hasError: true };
    }

    componentDidCatch(error: Error, info: React.ErrorInfo) {
      console.error("Error caught by ErrorBoundary:", error, info);
    }

    render() {
      if (this.state.hasError) {
        return <Text>Something went wrong.</Text>;
      }

      return this.props.children;
    }
  }

  const App: React.FC = () => (
    <ErrorBoundary>
      <View>
        <Text>Component content...</Text>
      </View>
    </ErrorBoundary>
  );
  ```

---

### **9. Testing and Code Quality**

#### **Unit Testing with Jest and React Testing Library**

#### **React Version**

- **React Example with TypeScript**:

  ```tsx
  import { render, screen, fireEvent } from "@testing-library/react";

  test("renders a button and checks click event", () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick} label="Click me" />);

    const button = screen.getByText("Click me");
    fireEvent.click(button);

    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  ```

#### **React Native Version**

- **React Native Example with TypeScript**:

  ```tsx
  import { render, fireEvent } from "@testing-library/react-native";
  import { Button, Text } from "react-native";

  test("Button click test", () => {
    const handleClick = jest.fn();
    const { getByText } = render(
      <Button title="Click me" onPress={handleClick} />
    );

    fireEvent.press(getByText("Click me"));
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
  ```

## **Extended React/React Native & TypeScript Cheat Sheet for Advanced Topics**

### **1. Storage Solutions: AsyncStorage, LocalStorage, Secure Storage**

#### **LocalStorage in React**

- **React Example with TypeScript**:

  ```tsx
  const useLocalStorage = <T,>(key: string, initialValue: T) => {
    const [storedValue, setStoredValue] = React.useState<T>(() => {
      try {
        const item = window.localStorage.getItem(key);
        return item ? JSON.parse(item) : initialValue;
      } catch (error) {
        console.error(error);
        return initialValue;
      }
    });

    const setValue = (value: T | ((val: T) => T)) => {
      try {
        const valueToStore =
          value instanceof Function ? value(storedValue) : value;
        setStoredValue(valueToStore);
        window.localStorage.setItem(key, JSON.stringify(valueToStore));
      } catch (error) {
        console.error(error);
      }
    };

    return [storedValue, setValue] as const;
  };

  const App: React.FC = () => {
    const [name, setName] = useLocalStorage<string>("name", "John");

    return (
      <div>
        <input value={name} onChange={(e) => setName(e.target.value)} />
        <p>Hello, {name}</p>
      </div>
    );
  };
  ```

#### **AsyncStorage in React Native**

- **React Native Example with TypeScript**:

  ```tsx
  import AsyncStorage from "@react-native-async-storage/async-storage";
  import React, { useEffect, useState } from "react";
  import { View, Text, TextInput, Button } from "react-native";

  const useAsyncStorage = (key: string, initialValue: string) => {
    const [storedValue, setStoredValue] = useState<string>(initialValue);

    useEffect(() => {
      const loadStorageValue = async () => {
        try {
          const item = await AsyncStorage.getItem(key);
          setStoredValue(item ? item : initialValue);
        } catch (error) {
          console.error(error);
        }
      };

      loadStorageValue();
    }, [key, initialValue]);

    const setValue = async (value: string) => {
      try {
        setStoredValue(value);
        await AsyncStorage.setItem(key, value);
      } catch (error) {
        console.error(error);
      }
    };

    return [storedValue, setValue] as const;
  };

  const App = () => {
    const [name, setName] = useAsyncStorage("name", "John");

    return (
      <View>
        <TextInput
          value={name}
          onChangeText={setName}
          placeholder="Enter your name"
        />
        <Text>Hello, {name}</Text>
      </View>
    );
  };

  export default App;
  ```

#### **Secure Storage in React Native**

- **React Native Example with TypeScript** (Using `react-native-keychain` for secure storage):

  ```tsx
  import * as Keychain from "react-native-keychain";
  import React, { useState } from "react";
  import { View, Text, Button, TextInput } from "react-native";

  const App = () => {
    const [username, setUsername] = useState("");
    const [password, setPassword] = useState("");
    const [storedUsername, setStoredUsername] = useState<string | null>(null);
    const [storedPassword, setStoredPassword] = useState<string | null>(null);

    const saveCredentials = async () => {
      try {
        await Keychain.setGenericPassword(username, password);
        console.log("Credentials stored successfully");
      } catch (error) {
        console.log("Could not store credentials", error);
      }
    };

    const loadCredentials = async () => {
      try {
        const credentials = await Keychain.getGenericPassword();
        if (credentials) {
          setStoredUsername(credentials.username);
          setStoredPassword(credentials.password);
        } else {
          console.log("No credentials stored");
        }
      } catch (error) {
        console.log("Could not load credentials", error);
      }
    };

    return (
      <View>
        <TextInput
          placeholder="Username"
          value={username}
          onChangeText={setUsername}
        />
        <TextInput
          placeholder="Password"
          secureTextEntry
          value={password}
          onChangeText={setPassword}
        />
        <Button title="Save" onPress={saveCredentials} />
        <Button title="Load" onPress={loadCredentials} />

        <Text>Stored Username: {storedUsername}</Text>
        <Text>Stored Password: {storedPassword}</Text>
      </View>
    );
  };

  export default App;
  ```

---

### **2. Advanced Error Handling**

#### **Handling Async Errors in React**

- **React Example with TypeScript**:

  ```tsx
  const fetchData = async (url: string) => {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error("Network response was not ok");
      const data = await response.json();
      return data;
    } catch (error) {
      console.error("Fetching data failed:", error);
      throw error;
    }
  };

  const DataComponent: React.FC = () => {
    const [data, setData] = React.useState<string[]>([]);
    const [error, setError] = React.useState<string | null>(null);

    useEffect(() => {
      fetchData("/api/data")
        .then((result) => setData(result))
        .catch((error) => setError(error.message));
    }, []);

    if (error) return <div>Error: {error}</div>;
    return <div>{data.join(", ")}</div>;
  };
  ```

#### **Handling Async Errors in React Native**

- **React Native Example with TypeScript**:

  ```tsx
  import React, { useState, useEffect } from "react";
  import { View, Text } from "react-native";

  const fetchData = async (url: string) => {
    try {
      const response = await fetch(url);
      if (!response.ok) throw new Error("Network error");
      return await response.json();
    } catch (error) {
      throw error;
    }
  };

  const DataComponent: React.FC = () => {
    const [data, setData] = useState<string[]>([]);
    const [error, setError] = useState<string | null>(null);

    useEffect(() => {
      fetchData("https://api.example.com/data")
        .then((response) => setData(response))
        .catch((err) => setError(err.message));
    }, []);

    if (error) return <Text>Error: {error}</Text>;
    return <Text>Data: {data.join(", ")}</Text>;
  };

  export default DataComponent;
  ```

---

### **3. Animation in React Native**

#### **Using `Animated` for Animations**

- **React Native Example with TypeScript**:

  ```tsx
  import React, { useRef, useEffect } from "react";
  import { Animated, View, Button } from "react-native";

  const App: React.FC = () => {
    const fadeAnim = useRef(new Animated.Value(0)).current;

    const fadeIn = () => {
      Animated.timing(fadeAnim, {
        toValue: 1,
        duration: 1000,
        useNativeDriver: true,
      }).start();
    };

    const fadeOut = () => {
      Animated.timing(fadeAnim, {
        toValue: 0,
        duration: 1000,
        useNativeDriver: true,
      }).start();
    };

    return (
      <View>
        <Animated.View
          style={{
            opacity: fadeAnim,
            width: 100,
            height: 100,
            backgroundColor: "blue",
          }}
        />
        <Button title="Fade In" onPress={fadeIn} />
        <Button title="Fade Out" onPress={fadeOut} />
      </View>
    );
  };

  export default App;
  ```

---

### **4. Deep Linking in React Native**

#### **Handling Deep Links with React Navigation**

- **React Native Example with TypeScript**:

  ```tsx
  import React from "react";
  import { NavigationContainer } from "@react-navigation/native";
  import { createStackNavigator } from "@react-navigation/stack";
  import { Linking } from "react-native";

  const Stack = createStackNavigator();

  const config = {
    screens: {
      Home: "home",
      Profile: "profile/:id",
    },
  };

  const linking = {
    prefixes: ["myapp://", "https://myapp.com"],
    config,
  };

  const HomeScreen = () => <Text>Home Screen</Text>;
  const ProfileScreen = ({ route }: { route: any }) => (
    <Text>Profile: {route.params.id}</Text>
  );

  const App = () => (
    <NavigationContainer linking={linking}>
      <Stack.Navigator>
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Profile" component={ProfileScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );

  export default App;
  ```

---

### **5. React Native Geolocation**

#### **Fetching Location Data**

- **React Native Example with TypeScript** (Using `react-native-geolocation-service`):

```tsx
import React, { useState, useEffect } from "react";
import { Text, View, Button } from "react-native";
import Geolocation from "react-native-geolocation-service";
import { check, request, PERMISSIONS, RESULTS } from "react-native-permissions";

const App: React.FC = () => {
  const [location, setLocation] = useState<{
    latitude: number;
    longitude: number;
  } | null>(null);

  const requestLocationPermission = async () => {
    const result = await request(PERMISSIONS.ANDROID.ACCESS_FINE_LOCATION);
    if (result === RESULTS.GRANTED) {
      Geolocation.getCurrentPosition(
        (position) => {
          setLocation({
            latitude: position.coords.latitude,
            longitude: position.coords.longitude,
          });
        },
        (error) => {
          console.log(error.code, error.message);
        },
        { enableHighAccuracy: true, timeout: 15000, maximumAge: 10000 }
      );
    }
  };

  useEffect(() => {
    requestLocationPermission();
  }, []);

  return (
    <View>
      {location ? (
        <Text>
          Latitude: {location.latitude}, Longitude: {location.longitude}
        </Text>
      ) : (
        <Text>Fetching location...</Text>
      )}
    </View>
  );
};

export default App;
```

---

### **6. Advanced Security Practices**

#### **Best Practices for Secure Data Handling in React Native**

1. **Use Secure Storage for sensitive data** (as shown above).
2. **Encrypt sensitive data at rest** using libraries like `react-native-encrypted-storage`.
3. **Implement biometric authentication** using libraries like `react-native-touch-id` or `react-native-fingerprint-scanner`.
4. **Use HTTPS for all network requests** and verify SSL certificates.

---

Certainly! Let's **dive deeper into these advanced topics** and provide more comprehensive examples and explanations. This will further enhance your preparation for a **senior/lead React/React Native developer interview**.

---

## **Extended Advanced Topics in React and React Native with TypeScript**

### **1. Advanced Storage Solutions**

#### **1.1. LocalStorage Enhancements in React**

- **Using Custom Hooks for Synchronization Across Tabs**

  ```tsx
  import { useState, useEffect } from "react";

  const useSyncedLocalStorage = <T,>(key: string, initialValue: T) => {
    const [value, setValue] = useState<T>(() => {
      const jsonValue = localStorage.getItem(key);
      return jsonValue !== null ? JSON.parse(jsonValue) : initialValue;
    });

    useEffect(() => {
      const handleStorageChange = (event: StorageEvent) => {
        if (event.key === key) {
          setValue(event.newValue ? JSON.parse(event.newValue) : initialValue);
        }
      };
      window.addEventListener("storage", handleStorageChange);
      return () => window.removeEventListener("storage", handleStorageChange);
    }, [key, initialValue]);

    const setItem = (newValue: T) => {
      setValue(newValue);
      localStorage.setItem(key, JSON.stringify(newValue));
    };

    return [value, setItem] as const;
  };

  // Usage
  const App: React.FC = () => {
    const [theme, setTheme] = useSyncedLocalStorage<"light" | "dark">(
      "theme",
      "light"
    );

    return (
      <div className={`app ${theme}`}>
        <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
          Toggle Theme
        </button>
      </div>
    );
  };
  ```

- **Persisting Complex Data Structures**

  When dealing with complex data structures, consider serializing data efficiently.

  ```tsx
  interface UserPreferences {
    theme: "light" | "dark";
    language: string;
    notificationsEnabled: boolean;
  }

  const initialPreferences: UserPreferences = {
    theme: "light",
    language: "en",
    notificationsEnabled: true,
  };

  // Custom hook similar to useLocalStorage, handling complex data structures
  ```

#### **1.2. AsyncStorage Advanced Usage in React Native**

- **Handling JSON Data**

  ```tsx
  import AsyncStorage from "@react-native-async-storage/async-storage";

  const storeData = async (key: string, value: any) => {
    try {
      const jsonValue = JSON.stringify(value);
      await AsyncStorage.setItem(key, jsonValue);
    } catch (e) {
      console.error("Error storing data", e);
    }
  };

  const getData = async (key: string) => {
    try {
      const jsonValue = await AsyncStorage.getItem(key);
      return jsonValue != null ? JSON.parse(jsonValue) : null;
    } catch (e) {
      console.error("Error retrieving data", e);
    }
  };
  ```

- **Migrating from AsyncStorage to Secure Storage**

  To enhance security, consider migrating sensitive data from AsyncStorage to secure storage solutions.

#### **1.3. Secure Storage Best Practices**

- **Using `react-native-encrypted-storage`**

  ```tsx
  import EncryptedStorage from "react-native-encrypted-storage";

  const storeSecureData = async (key: string, value: any) => {
    try {
      await EncryptedStorage.setItem(key, JSON.stringify(value));
    } catch (e) {
      console.error("Error storing secure data", e);
    }
  };

  const getSecureData = async (key: string) => {
    try {
      const jsonValue = await EncryptedStorage.getItem(key);
      return jsonValue != null ? JSON.parse(jsonValue) : null;
    } catch (e) {
      console.error("Error retrieving secure data", e);
    }
  };
  ```

- **Best Practices**

  - **Avoid Storing Sensitive Data in Plain Text**: Always encrypt sensitive information.
  - **Regularly Clear Sensitive Data**: Especially on logout or session timeout.
  - **Use Biometric Authentication**: For accessing highly sensitive data.

### **2. Advanced Error Handling**

#### **2.1. Global Error Handling**

- **React Version**

  ```tsx
  import React from "react";

  interface ErrorInfo {
    hasError: boolean;
    error: Error | null;
  }

  class GlobalErrorBoundary extends React.Component<{}, ErrorInfo> {
    constructor(props: {}) {
      super(props);
      this.state = { hasError: false, error: null };
    }

    static getDerivedStateFromError(error: Error) {
      return { hasError: true, error };
    }

    componentDidCatch(error: Error, info: React.ErrorInfo) {
      // Log error to monitoring service
      console.error("Global error:", error, info);
    }

    render() {
      if (this.state.hasError) {
        return <h1>An unexpected error occurred.</h1>;
      }
      return this.props.children;
    }
  }

  // Usage in root component
  const App: React.FC = () => (
    <GlobalErrorBoundary>
      <MainApp />
    </GlobalErrorBoundary>
  );
  ```

- **React Native Version**

  Similar to React, but consider integrating with native modules to catch native errors.

#### **2.2. Error Handling in Async/Await**

- **Using try-catch Blocks in Async Functions**

  ```tsx
  const fetchData = async () => {
    try {
      const response = await fetch("/api/data");
      if (!response.ok)
        throw new Error(`HTTP error! status: ${response.status}`);
      const data = await response.json();
      return data;
    } catch (error) {
      // Handle error appropriately
      console.error("Error fetching data:", error);
      throw error; // Re-throw if necessary
    }
  };
  ```

- **Centralized Error Handling with Axios Interceptors**

  ```tsx
  import axios from "axios";

  const apiClient = axios.create({
    baseURL: "https://api.example.com",
  });

  // Add a response interceptor
  apiClient.interceptors.response.use(
    (response) => response,
    (error) => {
      // Handle errors globally
      console.error("API error:", error);
      return Promise.reject(error);
    }
  );

  export default apiClient;
  ```

### **3. Advanced Animations**

#### **3.1. Animations in React with Framer Motion**

- **React Example with TypeScript**

  ```tsx
  import React from "react";
  import { motion } from "framer-motion";

  const AnimatedComponent: React.FC = () => {
    return (
      <motion.div
        initial={{ opacity: 0, y: -20 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.5 }}
      >
        <h1>Welcome to the App</h1>
      </motion.div>
    );
  };

  export default AnimatedComponent;
  ```

#### **3.2. Advanced Animations in React Native with Reanimated 2**

- **React Native Example with TypeScript**

  ```tsx
  import React from "react";
  import Animated, {
    useSharedValue,
    useAnimatedStyle,
    withTiming,
    Easing,
  } from "react-native-reanimated";
  import { View, Button } from "react-native";

  const ReanimatedExample: React.FC = () => {
    const animation = useSharedValue(0);

    const animatedStyle = useAnimatedStyle(() => {
      return {
        transform: [{ translateY: animation.value }],
      };
    });

    const startAnimation = () => {
      animation.value = withTiming(100, {
        duration: 500,
        easing: Easing.out(Easing.exp),
      });
    };

    return (
      <View>
        <Animated.View
          style={[
            { width: 100, height: 100, backgroundColor: "blue" },
            animatedStyle,
          ]}
        />
        <Button title="Animate" onPress={startAnimation} />
      </View>
    );
  };

  export default ReanimatedExample;
  ```

- **Advantages of Reanimated 2**

  - **Native Performance**: Animations run on the UI thread.
  - **Advanced Gesture Handling**: Works seamlessly with gesture handlers.
  - **Declarative API**: More intuitive and maintainable.

### **4. Deep Linking and Universal Links**

#### **4.1. Setting Up Universal Links and App Links**

- **Configuring in React Native**

  - **iOS (Universal Links)**:

    - Set up Associated Domains in Xcode (`applinks:yourdomain.com`).
    - Create an `apple-app-site-association` file on your server.

  - **Android (App Links)**:
    - Add intent filters in `AndroidManifest.xml`.
    - Create a `assetlinks.json` file on your server.

- **Handling Links in App**

  ```tsx
  import { Linking } from "react-native";

  useEffect(() => {
    const handleDeepLink = (event: { url: string }) => {
      const route = event.url.replace(/.*?:\/\//g, "");
      // Navigate based on route
    };

    Linking.addEventListener("url", handleDeepLink);

    return () => {
      Linking.removeEventListener("url", handleDeepLink);
    };
  }, []);
  ```

#### **4.2. Advanced Configuration with React Navigation**

- **React Navigation's Linking Configuration**

  ```tsx
  const linking = {
    prefixes: ["https://myapp.com", "myapp://"],
    config: {
      screens: {
        Home: "",
        Profile: "user/:id",
        Settings: "settings",
      },
    },
  };

  const App = () => (
    <NavigationContainer linking={linking}>
      {/* Rest of your app code */}
    </NavigationContainer>
  );
  ```

- **Handling Nested Navigators**

  Ensure that your linking configuration accounts for nested navigators.

### **5. Advanced Geolocation**

#### **5.1. Real-Time Location Tracking**

- **React Native Example with TypeScript**

  ```tsx
  import React, { useEffect, useState } from "react";
  import { View, Text } from "react-native";
  import Geolocation from "react-native-geolocation-service";

  const RealTimeLocation: React.FC = () => {
    const [position, setPosition] = useState<{
      latitude: number;
      longitude: number;
    } | null>(null);

    useEffect(() => {
      const watchId = Geolocation.watchPosition(
        (pos) => {
          setPosition({
            latitude: pos.coords.latitude,
            longitude: pos.coords.longitude,
          });
        },
        (error) => {
          console.error("Error watching position:", error);
        },
        {
          enableHighAccuracy: true,
          distanceFilter: 0,
          interval: 5000,
          fastestInterval: 2000,
        }
      );

      return () => {
        Geolocation.clearWatch(watchId);
      };
    }, []);

    if (!position) {
      return <Text>Waiting for position...</Text>;
    }

    return (
      <View>
        <Text>Latitude: {position.latitude}</Text>
        <Text>Longitude: {position.longitude}</Text>
      </View>
    );
  };

  export default RealTimeLocation;
  ```

- **Handling Permissions with `react-native-permissions`**

  Ensure you request and handle permissions appropriately for both iOS and Android.

### **6. Advanced Security Practices**

#### **6.1. Implementing Biometric Authentication**

- **Using `react-native-touch-id` or `react-native-fingerprint-scanner`**

  ```tsx
  import React from "react";
  import { Button, Alert } from "react-native";
  import TouchID from "react-native-touch-id";

  const optionalConfigObject = {
    title: "Authentication Required",
    color: "#e00606",
    fallbackLabel: "Use Passcode",
  };

  const BiometricAuth: React.FC = () => {
    const handleAuthentication = () => {
      TouchID.authenticate("Authenticate with Touch ID", optionalConfigObject)
        .then((success) => {
          Alert.alert("Authenticated Successfully");
        })
        .catch((error) => {
          Alert.alert("Authentication Failed", error.message);
        });
    };

    return <Button title="Authenticate" onPress={handleAuthentication} />;
  };

  export default BiometricAuth;
  ```

#### **6.2. Secure Network Communication**

- **Certificate Pinning**

  Use libraries like `react-native-cert-pinner` to prevent man-in-the-middle attacks.

  ```tsx
  import axios from "axios";
  import RNCertPinner from "react-native-cert-pinner";

  RNCertPinner.pinCertificates([
    {
      host: "api.example.com",
      hashes: ["sha256/AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA="],
    },
  ]);

  const apiClient = axios.create({
    baseURL: "https://api.example.com",
    httpsAgent: new https.Agent({ rejectUnauthorized: false }), // Required for custom SSL handling
  });
  ```

- **Avoiding Vulnerable Dependencies**

  Regularly audit your dependencies using tools like `npm audit` or `yarn audit`.

#### **6.3. Data Encryption**

- **Encrypting Data at Rest**

  Use encryption libraries to encrypt data before storing it, even in secure storage.

  ```tsx
  import CryptoJS from "crypto-js";

  const secretKey = "your-secret-key";

  const encryptData = (data: any) => {
    return CryptoJS.AES.encrypt(JSON.stringify(data), secretKey).toString();
  };

  const decryptData = (cipherText: string) => {
    const bytes = CryptoJS.AES.decrypt(cipherText, secretKey);
    return JSON.parse(bytes.toString(CryptoJS.enc.Utf8));
  };

  // Usage with AsyncStorage or Secure Storage
  ```

### **7. Offline Synchronization**

#### **7.1. Using Realm or WatermelonDB**

- **Setting Up Realm**

  ```tsx
  import Realm from "realm";

  class TaskSchema extends Realm.Object<TaskSchema> {
    _id!: Realm.BSON.ObjectId;
    name!: string;
    status!: string;

    static schema = {
      name: "Task",
      primaryKey: "_id",
      properties: {
        _id: "objectId",
        name: "string",
        status: "string",
      },
    };
  }

  const realm = await Realm.open({
    path: "myrealm",
    schema: [TaskSchema],
  });

  // CRUD operations
  realm.write(() => {
    realm.create("Task", {
      _id: new Realm.BSON.ObjectId(),
      name: "Do the dishes",
      status: "Open",
    });
  });
  ```

- **Synchronization Strategies**

  Implement conflict resolution strategies when syncing data with the server.

### **8. Advanced State Management with Redux Toolkit**

#### **8.1. Setting Up Redux Toolkit**

- **React/React Native Example with TypeScript**

  ```tsx
  import { configureStore, createSlice, PayloadAction } from "@reduxjs/toolkit";
  import { Provider, useDispatch, useSelector } from "react-redux";

  interface CounterState {
    value: number;
  }

  const initialState: CounterState = { value: 0 };

  const counterSlice = createSlice({
    name: "counter",
    initialState,
    reducers: {
      increment: (state) => {
        state.value += 1;
      },
      decrement: (state) => {
        state.value -= 1;
      },
      incrementByAmount: (state, action: PayloadAction<number>) => {
        state.value += action.payload;
      },
    },
  });

  const store = configureStore({
    reducer: {
      counter: counterSlice.reducer,
    },
  });

  type RootState = ReturnType<typeof store.getState>;
  type AppDispatch = typeof store.dispatch;

  const CounterComponent: React.FC = () => {
    const dispatch = useDispatch<AppDispatch>();
    const count = useSelector((state: RootState) => state.counter.value);

    return (
      <View>
        <Text>{count}</Text>
        <Button
          title="Increment"
          onPress={() => dispatch(counterSlice.actions.increment())}
        />
        <Button
          title="Decrement"
          onPress={() => dispatch(counterSlice.actions.decrement())}
        />
      </View>
    );
  };

  // In your App component
  const App = () => (
    <Provider store={store}>
      <CounterComponent />
    </Provider>
  );

  export default App;
  ```

#### **8.2. Async Operations with Redux Toolkit**

- **Using createAsyncThunk**

  ```tsx
  import { createAsyncThunk } from "@reduxjs/toolkit";

  export const fetchUserById = createAsyncThunk(
    "users/fetchByIdStatus",
    async (userId: string, thunkAPI) => {
      const response = await fetch(`https://api.example.com/users/${userId}`);
      return (await response.json()) as User;
    }
  );

  // Handle in slice
  const userSlice = createSlice({
    name: "user",
    initialState: { entities: [], loading: "idle" } as UserState,
    reducers: {},
    extraReducers: (builder) => {
      builder
        .addCase(fetchUserById.pending, (state) => {
          state.loading = "loading";
        })
        .addCase(fetchUserById.fulfilled, (state, action) => {
          state.loading = "idle";
          state.entities.push(action.payload);
        })
        .addCase(fetchUserById.rejected, (state) => {
          state.loading = "failed";
        });
    },
  });
  ```

### **9. WebSockets and Real-Time Data**

#### **9.1. Using Socket.IO in React/React Native**

- **Setting Up Socket.IO Client**

  ```tsx
  import io from "socket.io-client";
  import React, { useEffect } from "react";

  const socket = io("https://yourserver.com");

  const RealTimeComponent: React.FC = () => {
    useEffect(() => {
      socket.on("connect", () => {
        console.log("Connected to server");
      });

      socket.on("message", (data) => {
        console.log("Received message:", data);
      });

      return () => {
        socket.disconnect();
      };
    }, []);

    return <View>{/* Your UI components */}</View>;
  };

  export default RealTimeComponent;
  ```

- **Handling Reconnection and Error Events**

  Implement event listeners for `reconnect`, `disconnect`, and `error` to enhance reliability.

### **10. Accessibility Best Practices**

#### **10.1. Enhancing Accessibility in React**

- **Using Semantic HTML Elements**

  ```tsx
  return (
    <header>
      <nav>
        <ul>
          <li>
            <a href="/home">Home</a>
          </li>
          {/* Other navigation items */}
        </ul>
      </nav>
    </header>
  );
  ```

- **Adding ARIA Attributes**

  ```tsx
  <button aria-label="Close" onClick={handleClose}>
    ×
  </button>
  ```

#### **10.2. Accessibility in React Native**

- **Using Accessibility Props**

  ```tsx
  import { TouchableOpacity, Text } from "react-native";

  <TouchableOpacity
    accessible={true}
    accessibilityLabel="Close Button"
    accessibilityHint="Closes the dialog"
    onPress={handleClose}
  >
    <Text>Close</Text>
  </TouchableOpacity>;
  ```

- **Testing Accessibility**

  Utilize tools like **Accessibility Inspector** for iOS and **Accessibility Scanner** for Android.

---

## **Encryption, biometrics and authentication React/React Native & TypeScript Cheat Sheet for Advanced Topics**

## **1. Encryption in React Native**

Encryption ensures that sensitive data is secure at rest and during transmission. In React Native, we can use libraries like **crypto-js** for encryption and **react-native-encrypted-storage** for securely storing encrypted data.

### **1.1. Encrypting Data Using `crypto-js`**

```tsx
import CryptoJS from "crypto-js";

// Encrypt a string
const encryptData = (data: string, secretKey: string): string => {
  return CryptoJS.AES.encrypt(data, secretKey).toString();
};

// Decrypt the encrypted string
const decryptData = (ciphertext: string, secretKey: string): string => {
  const bytes = CryptoJS.AES.decrypt(ciphertext, secretKey);
  return bytes.toString(CryptoJS.enc.Utf8);
};

// Example usage
const secretKey = "my-secret-key";
const encrypted = encryptData("Sensitive data", secretKey);
const decrypted = decryptData(encrypted, secretKey);

console.log("Encrypted:", encrypted);
console.log("Decrypted:", decrypted);
```

**Use Cases**:

- Encrypting sensitive data (e.g., API keys, tokens) before storing in local storage or AsyncStorage.
- Decrypting the data only when required to minimize exposure of sensitive information.

### **1.2. Secure Storage Using `react-native-encrypted-storage`**

This library provides secure and encrypted storage solutions for both iOS and Android.

#### **Setting up Secure Storage**

1. Install the library:

   ```bash
   npm install react-native-encrypted-storage
   ```

2. Implementing Secure Storage:

```tsx
import EncryptedStorage from "react-native-encrypted-storage";

// Save data securely
const storeSecureData = async (key: string, value: any) => {
  try {
    await EncryptedStorage.setItem(key, JSON.stringify(value));
    console.log("Data stored successfully");
  } catch (error) {
    console.error("Failed to store data securely:", error);
  }
};

// Retrieve data securely
const getSecureData = async (key: string) => {
  try {
    const storedValue = await EncryptedStorage.getItem(key);
    return storedValue ? JSON.parse(storedValue) : null;
  } catch (error) {
    console.error("Failed to retrieve data securely:", error);
  }
};

// Example usage
const saveToken = async () => {
  await storeSecureData("authToken", { token: "my-secure-token" });
};

const loadToken = async () => {
  const token = await getSecureData("authToken");
  console.log("Retrieved Token:", token);
};
```

**Advantages**:

- Secure storage that is encrypted and protected by the system’s security mechanisms (Keychain on iOS, Keystore on Android).
- Prevents unauthorized access to sensitive data.

---

## **2. Biometric Authentication in React Native**

Biometric authentication (e.g., Face ID, Touch ID) provides a secure and user-friendly way to authenticate users without requiring passwords. In React Native, libraries like **react-native-keychain** and **react-native-touch-id** enable biometric authentication.

### **2.1. Biometric Authentication Using `react-native-keychain`**

1. Install the package:

   ```bash
   npm install react-native-keychain
   ```

2. Set up biometric authentication:

```tsx
import * as Keychain from "react-native-keychain";

// Store login credentials securely with biometric authentication
const storeCredentials = async (username: string, password: string) => {
  try {
    await Keychain.setGenericPassword(username, password, {
      accessControl: Keychain.ACCESS_CONTROL.BIOMETRY_ANY, // Face ID or Touch ID
      accessible: Keychain.ACCESSIBLE.WHEN_UNLOCKED_THIS_DEVICE_ONLY,
    });
    console.log("Credentials stored securely with biometric authentication");
  } catch (error) {
    console.error("Could not store credentials:", error);
  }
};

// Retrieve login credentials with biometric authentication
const loadCredentials = async () => {
  try {
    const credentials = await Keychain.getGenericPassword({
      accessControl: Keychain.ACCESS_CONTROL.BIOMETRY_ANY, // Face ID or Touch ID
    });

    if (credentials) {
      console.log("Credentials loaded:", credentials);
      return credentials;
    } else {
      console.log("No credentials stored");
      return null;
    }
  } catch (error) {
    console.error("Could not retrieve credentials:", error);
    return null;
  }
};

// Example usage
const authenticateUser = async () => {
  const credentials = await loadCredentials();
  if (credentials) {
    console.log(`Authenticated! Username: ${credentials.username}`);
  }
};
```

**Key Options**:

- **`ACCESS_CONTROL.BIOMETRY_ANY`**: Allows biometric authentication like Face ID or Touch ID.
- **`ACCESSIBLE.WHEN_UNLOCKED_THIS_DEVICE_ONLY`**: Ensures credentials are accessible only when the device is unlocked.

---

## **3. Authentication Solutions**

In modern applications, authentication is commonly handled with OAuth, JWT (JSON Web Tokens), or other token-based systems. Let's explore setting up authentication with JWT and handling it securely in **React** and **React Native**.

### **3.1. Token-Based Authentication (JWT)**

JWT tokens are commonly used for securing API requests. They should be stored securely to prevent unauthorized access.

#### **Using JWT with Secure Storage**

1. **Login and Token Handling**:

```tsx
import axios from "axios";
import EncryptedStorage from "react-native-encrypted-storage";

const login = async (username: string, password: string) => {
  try {
    // Send login request to the API
    const response = await axios.post("https://api.example.com/login", {
      username,
      password,
    });
    const { token } = response.data;

    // Store the token securely
    await EncryptedStorage.setItem("jwtToken", token);
    console.log("Token stored successfully");
  } catch (error) {
    console.error("Login failed:", error);
  }
};

// Example usage
const handleLogin = async () => {
  await login("john.doe", "mypassword");
};
```

2. **Using JWT for Secured API Requests**:

```tsx
import axios from "axios";
import EncryptedStorage from "react-native-encrypted-storage";

// Function to retrieve token and make API request
const fetchProtectedData = async () => {
  try {
    // Retrieve token from secure storage
    const token = await EncryptedStorage.getItem("jwtToken");

    // Set token in Authorization header
    const response = await axios.get("https://api.example.com/protected-data", {
      headers: {
        Authorization: `Bearer ${token}`,
      },
    });

    // Handle the protected data response
    console.log("Protected Data:", response.data);
  } catch (error) {
    console.error("Error fetching protected data:", error);
  }
};

// Example usage
const loadData = async () => {
  await fetchProtectedData();
};
```

#### **Secure Token Refresh Handling**

JWT tokens are often short-lived for security reasons, requiring a refresh token mechanism.

```tsx
const refreshAccessToken = async () => {
  try {
    const refreshToken = await EncryptedStorage.getItem("refreshToken");
    const response = await axios.post("https://api.example.com/refresh-token", {
      token: refreshToken,
    });

    const { newToken } = response.data;

    // Store the new token securely
    await EncryptedStorage.setItem("jwtToken", newToken);
    console.log("Token refreshed successfully");
  } catch (error) {
    console.error("Token refresh failed:", error);
  }
};
```

---

### **4. Multi-Factor Authentication (MFA)**

Multi-factor authentication (MFA) adds an additional layer of security by requiring not only a password but also a second factor, such as an OTP (One-Time Password).

#### **4.1. Setting Up MFA with an External Service (e.g., Auth0)**

1. **Request MFA Code After Login**:

   - After a successful login, the backend can return an OTP or challenge the user for MFA.

2. **Verify MFA Code**:

```tsx
const verifyMfaCode = async (userId: string, mfaCode: string) => {
  try {
    const response = await axios.post("https://api.example.com/mfa/verify", {
      userId,
      code: mfaCode,
    });

    if (response.data.success) {
      console.log("MFA verification successful");
      // Proceed with authenticated operations
    } else {
      console.error("MFA verification failed");
    }
  } catch (error) {
    console.error("MFA verification error:", error);
  }
};
```

#### **4.2. Using Time-Based OTP (TOTP) for MFA**

You can integrate TOTP (Time-Based One-Time Password) using external services or libraries like **speakeasy**.

---

### **5. Best Practices for Secure Authentication and Data Handling**

- **Always use HTTPS**: Ensure all network communications, especially those involving sensitive data like login credentials, are secured over HTTPS.
- **Use short-lived JWT tokens**: Tokens should have short expiration times, and refresh tokens should be securely stored for renewing access.
- **Biometric fallback**: In addition to biometrics, provide a fallback authentication mechanism (e.g., PIN code or password).

- **Monitor security**: Implement server-side monitoring and alerting for unusual login attempts, token misuse, or suspicious activity.

- \*\*

Encrypt sensitive data\*\*: Always encrypt tokens, passwords, and any personally identifiable information (PII) before storing or transmitting it.

- **Regular security audits**: Ensure you perform regular audits of your application dependencies and codebase for known security vulnerabilities.

---
