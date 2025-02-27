Javascript J3 questions:

1. null vs undefined;

2. Strict vs non-strict comparison


3. Object as a reference type;
What happens when you assign one object to another variable? Does it create a copy?
What is the difference between shallow copy and deep copy of an object?



```
const obj1 = { name: "Alice", age: 25 };
const obj2 = obj1;
obj2.age = 30;
console.log(obj1.age); // What will be the output and why?
```


4. What are different ways to create an object in JavaScript?


5. "this" definition; Global context adn this, how is "this" keyword different in arrow functions and noraml functions. 

```
const obj = {
  value: 10,
  regularFn: function () {
    return this.value;
  },
  arrowFn: () => {
    return this.value;
  },
};

console.log(obj.regularFn()); // ?
console.log(obj.arrowFn()); // ?
```


6. Handling Errors in Async/Await


7. Promises vs Callbacks
What are the main differences between Promises and callbacks?
Why are Promises considered a better alternative to callbacks?
Can you convert a callback-based function into a Promise-based function? How?

What are the three states of a Promise?

8. object methods
How do you check if an object has a specific property?
How do you get all the keys of an object?
How do you merge two objects?
How do you delete a property from an object?
How do you loop through an object’s properties?
How do you convert an object into an array of key-value pairs? obj.entries()

9. What is the difference between __proto__ and prototype in JavaScript?

__proto__ is an instance property that points to the prototype of the object it was created from.
prototype is a property of constructor functions that is used to set up the prototype of new instances.



10. How do you define a generic interface?


11. How do you use multiple generic types in a function?


React Reconciliation:

---

### **1. Can we use indexes as keys in lists, and what are the potential issues with doing so?**  
**Explanation:**  
Using indexes as keys can lead to performance issues and unexpected UI behavior. When an item is inserted or removed, React may re-render components incorrectly because their keys have changed. This can cause issues like losing component state, unnecessary re-renders, and animation glitches.

**Example of incorrect key usage:**  
```jsx
{items.map((item, index) => (
  <Item key={index} data={item} />
))}
```
If an item is removed from the middle of the list, all subsequent items will shift, and their indexes (keys) will change, forcing unnecessary re-renders.

**Best practice:**  
Use a **stable, unique identifier** instead of an index:  
```jsx
{items.map((item) => (
  <Item key={item.id} data={item} />
))}
```

---

### **2. Should a component’s `key` be globally unique across the entire application or just within its sibling list?**  
**Explanation:**  
A component’s `key` only needs to be unique within its sibling list. React uses keys to differentiate elements **within the same parent** during reconciliation.

**Correct example:**  
```jsx
<ul>
  {items.map((item) => (
    <li key={item.id}>{item.name}</li>
  ))}
</ul>
```
Each `key` here should be unique only **within this `<ul>` list**.

However, if a `key` is not unique among siblings, React may incorrectly reuse elements, leading to bugs.

---

### **3. How does React determine if a component should be updated or replaced during reconciliation?**  
**Explanation:**  
React compares the virtual DOM of the previous and current render. If the element type (tag name or component type) remains the same, React updates the component instead of unmounting and re-mounting it. Otherwise, it replaces the entire subtree.

**Example:**  
```jsx
function App() {
  const [isBox, setIsBox] = useState(true);
  
  return isBox ? <div>Hello</div> : <span>Hello</span>;
}
```
Since `<div>` and `<span>` are different types, React will **replace** the element instead of updating it.

---

### **4. What happens if a component’s `key` changes between renders?**  
**Explanation:**  
If a component's `key` changes, React will **unmount the old component and mount a new one** instead of reusing the previous instance.

**Example:**  
```jsx
<Item key={user.id} />
```
If `user.id` changes, React will destroy the old component and create a new one, resetting its state.

This behavior is useful when forcing a full re-render but can lead to performance issues if keys change unnecessarily.

---

### **5. Why does React use a "two-pass" diffing algorithm, and how does it optimize rendering performance?**  
**Explanation:**  
React's reconciliation follows these two steps:  
1. **First pass:** Compares element types and reorders nodes if necessary.  
2. **Second pass:** Recursively updates attributes and properties.  

This method prevents unnecessary updates, making rendering more efficient. By prioritizing updates over full re-renders, React achieves better performance.

---

### **6. How does React handle the reconciliation of deeply nested component trees, and what optimizations does it use?**  
**Explanation:**  
For deeply nested components, React optimizes reconciliation by:  
- **Skipping subtrees if the parent has not changed.**  
- **Using `PureComponent` or `React.memo()`** to prevent re-renders for unchanged props.  
- **Using `shouldComponentUpdate()`** to control updates manually.  

Example:
```jsx
const MemoizedComponent = React.memo(MyComponent);
```
This ensures that `MyComponent` **only re-renders if props change**.

---

### **7. What impact does moving components within a list have on reconciliation, and how can it be optimized?**  
**Explanation:**  
Moving components within a list (e.g., reordering items) can cause React to **recreate instances unnecessarily** unless proper `keys` are used.

**Example of bad key usage:**
```jsx
{items.map((item, index) => (
  <Item key={index} data={item} />
))}
```
If the list order changes, React **incorrectly reuses elements**, leading to potential UI glitches.

**Optimization:**  
Use stable keys:
```jsx
{items.map((item) => (
  <Item key={item.id} data={item} />
))}
```

---

### **8. How does React differentiate between new elements and updated elements in a list when reconciling?**  
**Explanation:**  
React identifies elements based on their **keys**. If the `key` remains the same, React assumes it is the **same element** and updates it. If the `key` changes or disappears, React treats it as a new element.

**Example:**
```jsx
<ul>
  <li key="1">Apple</li>
  <li key="2">Banana</li>
</ul>
```
If we change the order:
```jsx
<ul>
  <li key="2">Banana</li>
  <li key="1">Apple</li>
</ul>
```
React **correctly reorders the elements** instead of replacing them.

---

### **9. What are the limitations of React’s diffing algorithm when detecting changes in the virtual DOM?**  
**Explanation:**  
React’s diffing algorithm **assumes elements at the same position in a tree are related** unless keys are used.

**Limitations:**
- Struggles with **deeply nested structures**.
- Inefficient when elements frequently change order.
- Cannot detect **granular changes within complex components** (e.g., inside a canvas or text editor).

**Solution:**  
Use stable `keys`, `shouldComponentUpdate()`, and memoization to optimize updates.

---

### **10. In what scenarios might React unnecessarily re-render components, and how can developers prevent this?**  
**Explanation:**  
React might re-render components unnecessarily in these cases:
- **Parent component re-renders** → Child re-renders even if props didn’t change.
- **State updates with the same value** → Still triggers a re-render.
- **Anonymous functions in props** → Creates a new reference each render.

**How to prevent unnecessary re-renders:**
- **Use `React.memo()`** for functional components:
  ```jsx
  const MemoizedComponent = React.memo(MyComponent);
  ```
- **Use `useCallback()` to memoize functions:**
  ```jsx
  const handleClick = useCallback(() => { ... }, []);
  ```
- **Use `useMemo()` for expensive calculations:**
  ```jsx
  const computedValue = useMemo(() => expensiveFunction(data), [data]);
  ```
- **Use immutable state updates** to prevent unnecessary re-renders.

---


Redux:

Here are some **Redux**, **Selectors**, **HOC (Higher-Order Components)**, **Reducers**, and **Rendering Logic with Redux** questions:  

---

HOC - now now, 

### **Redux Questions**  

#### **1. What are the key differences between Redux `dispatch` and `useReducer` in React?**  
Example:  
```tsx
const [state, dispatch] = useReducer(reducer, initialState);
dispatch({ type: "ADD_ITEM", payload: { name: "Item 1" } });

// How does this compare to Redux's dispatch method?
```

---

#### **2. How does Redux handle state immutability in reducers?**  
Example:  
```ts
const reducer = (state = { count: 0 }, action) => {
  switch (action.type) {
    case "INCREMENT":
      state.count += 1; // Is this correct? Why or why not?
      return state;
    default:
      return state;
  }
};
```
> Why does this code break Redux’s rules?

---
know immutability of --------------

#### **3. Can a Redux reducer have side effects? If not, how should side effects be handled in Redux?**  
Example:  
```ts
const reducer = (state = { count: 0 }, action) => {
  if (action.type === "INCREMENT") {
    console.log("Increment action triggered"); // Is this allowed inside a reducer?
  }
  return state;
};
```
> How should logging or API calls be handled instead?

---

#### **4. How do you structure Redux for large applications?**  
Example:  
> When should you use feature-based reducers vs. splitting by data type?

---

redux thunk know 


### **Selectors Questions**  

#### **5. Why are selectors preferred over directly accessing the Redux state?**  
Example:  
```tsx
const user = useSelector((state) => state.auth.user);
```
> Why is using **reselect** or memoized selectors beneficial?

---

#### **6. What is the difference between a simple selector and a memoized selector?**  
Example:  
```ts
const getUser = (state) => state.user;

const getUserFullName = createSelector(
  [getUser],
  (user) => `${user.firstName} ${user.lastName}`
);
```
> When does `createSelector` improve performance?

---

### **HOC (Higher-Order Components) Questions**  

#### **7. How does a Higher-Order Component (HOC) work with Redux?**  
Example:  
```tsx
const withAuth = (Component) => (props) => {
  const user = useSelector((state) => state.auth.user);
  return user ? <Component {...props} /> : <Redirect to="/login" />;
};
```
> What are the advantages of using HOCs over hooks like `useSelector`?

---

#### **8. How do you connect a component to Redux using `connect()` HOC?**  
Example:  
```tsx
const mapStateToProps = (state) => ({ user: state.auth.user });

export default connect(mapStateToProps)(Profile);
```
> How does `connect()` improve testability compared to `useSelector()`?

---

### **Reducers Questions**  

#### **9. Why should Redux reducers be pure functions?**  
Example:  
```ts
const reducer = (state = { count: 0 }, action) => {
  state.count += 1; // Is this correct?
  return state;
};
```
> How does mutating state cause unexpected behavior?

---

#### **10. How do you handle deeply nested state updates in Redux reducers?**  
Example:  
```ts
const initialState = {
  user: { details: { name: "Alice", age: 25 } },
};

const reducer = (state = initialState, action) => {
  if (action.type === "UPDATE_AGE") {
    return {
      ...state,
      user: { ...state.user, details: { ...state.user.details, age: action.payload } },
    };
  }
  return state;
};
```
> Why is spreading objects necessary in Redux?

---

### **Rendering Logic with Redux Questions**  

#### **11. When does a React component re-render when using `useSelector()`?**  
Example:  
```tsx
const count = useSelector((state) => state.counter.value);
```
> What happens if `state.counter` updates but `value` remains the same?

---

#### **12. How does `useSelector` cause unnecessary re-renders? How do you prevent it?**  
Example:  
```tsx
const user = useSelector((state) => state.user); // Why might this cause excessive re-renders?
```
> How can you optimize selectors to avoid this?

---

#### **13. Why should derived state be computed using selectors instead of in `useEffect`?**  
Example:  
```tsx
useEffect(() => {
  setFullName(`${user.firstName} ${user.lastName}`);
}, [user]);
```
> How can this be rewritten using selectors for better performance?

---

lifecycle methods - not knows very well but know how hooks work.





#### **14. How do you prevent components from re-rendering when unrelated Redux state updates?**  
Example:  
```tsx
const user = useSelector((state) => state.user);
const theme = useSelector((state) => state.theme); // Does this cause extra renders?
```
> Should we use `shallowEqual` or separate components for each slice of state?

---

#### **15. What happens when you dispatch an action inside `useEffect`?**  
Example:  
```tsx
useEffect(() => {
  dispatch(fetchUser());
}, []);
```
> How can this cause infinite loops, and how do you prevent it?

---
know the event loop


React Native 

What is different between bare and expo managed workflows

Why do we need metro bundler? 

Can we animate everything with React Native provided Animation API? and what is different between Reanimated?
 
what is provvisioning profiles used for? 

what is discribution certificated used for? 











