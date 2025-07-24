# Senior React Native Engineer Interview Questions - Funding Circle

**Duration:** Up to 1 hour  
**Target Level:** Senior Engineer  
**Focus Areas:** React Native, React, JavaScript/TypeScript, Software Engineering Practices

---

## React - Reconciliation (Advanced Level)

### 1. Fiber Architecture Deep Dive
Explain React's Fiber architecture and how it differs from the previous Stack reconciler. How does Fiber enable time-slicing and concurrent rendering?

explains old reconsiler.
mentions prioritization.
mentions useTransition.
Suspense.


**Evaluation Criteria:**
- [ ] Explains that Fiber is a data structure representing a unit of work
- [ ] Mentions that Stack reconciler was synchronous and blocking
- [ ] Describes how Fiber enables incremental rendering
- [ ] Explains work prioritization and scheduling
- [ ] Mentions the ability to pause, abort, and resume work
- [ ] Discusses the "work-in-progress" tree concept
- [ ] Explains how time-slicing works (yielding to main thread)
- [ ] Connects to concurrent features like Suspense and useTransition

### 2. Reconciliation Algorithm
Walk me through React's reconciliation algorithm. How does React decide whether to update, replace, or reorder elements during a re-render?



**Evaluation Criteria:**
- [ ] Explains the diffing process between virtual DOM trees
- [ ] Mentions the importance of element type comparison
- [ ] Describes how React handles element type changes (unmount/remount)
- [ ] Explains the role of keys in list reconciliation
- [ ] Discusses the heuristics React uses for performance
- [ ] Mentions that React assumes elements at same position are related
- [ ] Explains how props comparison works for updates
- [ ] Describes the recursive nature of the algorithm

// would it destroy whole siblings because index is chagned? 

### 3. Key Optimization
```javascript
// What's wrong with this implementation and how would you fix it?
const TodoList = ({ todos, onToggle }) => {
  return todos.map((todo, index) => (
    <TodoItem 
      key={index} 
      todo={todo} 
      onToggle={() => onToggle(todo.id)}
    />
  ));
};
```

### 4. Work Loop Interruption
How does React's work loop handle interruption for higher priority updates? Explain the concept of "yielding to the main thread."

**Evaluation Criteria:**
- [ ] Explains the scheduler's role in managing work priorities
- [ ] Describes how React checks for higher priority work
- [ ] Mentions the time slicing mechanism (5ms chunks)
- [ ] Explains yielding to browser for painting/user interactions
- [ ] Discusses different priority levels (Immediate, UserBlocking, Normal, Low, Idle)
- [ ] Describes how interrupted work can be resumed or discarded
- [ ] Mentions the requestIdleCallback concept (or similar mechanisms)
- [ ] Explains how this prevents blocking the main thread

### 5. Concurrent Features
How do `useTransition` and `useDeferredValue` leverage Fiber's architecture to improve user experience?

**Evaluation Criteria:**
- [ ] Explains that useTransition marks updates as non-urgent
- [ ] Describes how it prevents UI from blocking during expensive operations
- [ ] Mentions the isPending state for loading indicators
- [ ] Explains useDeferredValue for deferring expensive computations
- [ ] Describes how these hooks work with Fiber's priority system
- [ ] Mentions the concept of "urgent vs non-urgent" updates
- [ ] Explains how this improves perceived performance
- [ ] Connects to React 18's concurrent rendering

---

## React - Components (Advanced Level)

### 6. Component Lifecycle vs Hooks
Compare the component lifecycle methods with their Hook equivalents. What are the advantages and limitations of each approach?

**Evaluation Criteria:**
- [ ] Maps useEffect to componentDidMount, componentDidUpdate, componentWillUnmount
- [ ] Explains that useEffect runs after render (vs some lifecycle methods running before)
- [ ] Mentions that useEffect can have multiple instances vs single lifecycle methods
- [ ] Describes useLayoutEffect for synchronous operations (like componentDidMount)
- [ ] Explains advantages: better code organization, logic reuse, simpler patterns
- [ ] Mentions limitations: learning curve, potential for infinite loops
- [ ] Discusses how custom hooks enable better abstraction than HOCs/render props
- [ ] Explains the mental model shift from lifecycle thinking to effect thinking

### 7. Error Boundaries
```javascript
// Implement a comprehensive error boundary that:
// 1. Catches errors in child components
// 2. Logs errors to an external service
// 3. Provides fallback UI
// 4. Allows retry functionality
```

### 8. Higher-Order Components vs Render Props vs Hooks
When would you choose each pattern? Provide examples of scenarios where one approach is clearly superior.

**Evaluation Criteria:**
- [ ] Explains HOCs are good for cross-cutting concerns (auth, logging, analytics)
- [ ] Mentions HOC limitations: wrapper hell, prop name collisions, static composition
- [ ] Describes render props as good for dynamic composition and flexible UI patterns
- [ ] Mentions render prop limitations: callback hell, performance issues with inline functions
- [ ] Explains hooks as the modern solution for stateful logic sharing
- [ ] Describes hook advantages: no wrapper components, easier testing, better tree structure
- [ ] Mentions when to still use HOCs: when you need to modify component behavior declaratively
- [ ] Discusses when render props are still useful: when you need to share render logic
- [ ] Explains the evolution from mixins → HOCs → render props → hooks

### 9. Component Composition
Design a flexible Modal component that supports:
- Custom content
- Different sizes and positions
- Animation configurations
- Backdrop handling
- Accessibility features

### 10. Performance Optimization
```javascript
// Optimize this component for performance
const ExpensiveList = ({ items, searchTerm, sortBy }) => {
  const filteredItems = items.filter(item => 
    item.name.toLowerCase().includes(searchTerm.toLowerCase())
  );
  
  const sortedItems = filteredItems.sort((a, b) => {
    return a[sortBy] > b[sortBy] ? 1 : -1;
  });
  
  return (
    <div>
      {sortedItems.map(item => (
        <ExpensiveItem key={item.id} item={item} />
      ))}
    </div>
  );
};
```

---

## React - Hooks (Intermediate Level)

### 11. Custom Hook Design
Create a custom hook `useApi` that handles:
- Loading states
- Error handling
- Automatic retries
- Request cancellation
- Caching

### 12. Hook Rules Violations
```javascript
// Identify and fix the hook rule violations
const Component = ({ condition, items }) => {
  if (condition) {
    const [state, setState] = useState(false);
  }
  
  const [count, setCount] = useState(0);
  
  for (let i = 0; i < items.length; i++) {
    useEffect(() => {
      console.log(items[i]);
    }, [items[i]]);
  }
  
  return <div>{count}</div>;
};
```

### 13. useEffect Dependencies
Explain the difference between these useEffect implementations:
```javascript
// Version A
useEffect(() => {
  fetchData(userId);
}, []);

// Version B
useEffect(() => {
  fetchData(userId);
}, [userId]);

// Version C
useEffect(() => {
  fetchData(userId);
}, [userId, fetchData]);
```

### 14. Memory Leaks Prevention
How do you prevent memory leaks in React hooks? Provide examples of common scenarios and solutions.

**Evaluation Criteria:**
- [ ] Explains useEffect cleanup function (return statement)
- [ ] Identifies event listener cleanup scenarios
- [ ] Describes timer/interval clearing (clearTimeout, clearInterval)
- [ ] Explains async operation cancellation (AbortController, cancellation flags)
- [ ] Mentions WebSocket/subscription cleanup
- [ ] Describes closure capture issues with stale state
- [ ] Explains functional state updates to avoid stale closures
- [ ] Mentions React StrictMode for detecting missing cleanups
- [ ] Discusses memory profiling tools and detection methods

### 15. Hook Optimization
When and how would you optimize these hooks: `useCallback`, `useMemo`, `useRef`?

**Evaluation Criteria:**
- [ ] **useCallback**: Memoizes functions to prevent child re-renders, dependency array usage
- [ ] **useMemo**: Memoizes expensive computations, when to use vs when it's overkill
- [ ] **useRef**: Persisting values across renders, avoiding re-renders, DOM references
- [ ] Explains when optimization is unnecessary (premature optimization)
- [ ] Describes dependency array importance and ESLint rules
- [ ] Mentions performance profiling before optimizing
- [ ] Explains React.memo vs hook optimizations
- [ ] Discusses referential equality and when it matters

---

## Redux (Intermediate Level)

### 16. Redux Architecture
Explain the unidirectional data flow in Redux. How does it compare to other state management solutions like Context API or Zustand?

**Evaluation Criteria:**
- [ ] Explains the Redux cycle: Action → Dispatcher → Reducer → Store → View
- [ ] Describes immutability requirements and benefits
- [ ] Mentions single source of truth principle
- [ ] Explains predictable state changes through pure functions
- [ ] Compares to Context API: Redux has better DevTools, middleware, performance optimizations
- [ ] Mentions Context API limitations: no time-travel debugging, provider hell, re-render issues
- [ ] Compares to Zustand: Redux is more opinionated, has larger ecosystem, better debugging
- [ ] Discusses when to choose each solution based on app complexity and team needs
- [ ] Mentions Redux DevTools and middleware ecosystem as key advantages

### 17. Middleware Implementation
```javascript
// Implement a Redux middleware that:
// 1. Logs all actions and state changes
// 2. Measures action performance
// 3. Handles async actions with loading states
```

### 18. Selector Optimization
```javascript
// Optimize these selectors for performance
const selectUserPosts = state => 
  state.posts.filter(post => post.userId === state.currentUser.id);

const selectPostsWithComments = state =>
  state.posts.map(post => ({
    ...post,
    comments: state.comments.filter(comment => comment.postId === post.id)
  }));
```

### 19. Redux Toolkit vs Classic Redux
What are the benefits of Redux Toolkit? How does `createSlice` improve developer experience?

**Evaluation Criteria:**
- [ ] Explains that RTK reduces boilerplate code significantly
- [ ] Mentions built-in Immer for immutable updates (allows "mutative" logic)
- [ ] Describes createSlice combining actions and reducers in one place
- [ ] Explains automatic action creator generation
- [ ] Mentions built-in Redux DevTools Extension support
- [ ] Describes createAsyncThunk for handling async logic
- [ ] Explains RTK Query for data fetching and caching
- [ ] Mentions better TypeScript support out of the box
- [ ] Discusses performance optimizations and good defaults
- [ ] Explains the opinionated approach reducing decision fatigue

### 20. Normalized State Structure
Design a normalized state structure for a social media app with users, posts, comments, and likes.

**Evaluation Criteria:**
- [ ] Understands normalization principles (entities by ID, no nested duplicates)
- [ ] Designs separate entities: users, posts, comments, likes with IDs as keys
- [ ] Uses arrays of IDs for relationships (post.commentIds, user.postIds)
- [ ] Avoids deeply nested data structures
- [ ] Implements proper foreign key relationships
- [ ] Considers entity lookup efficiency (O(1) by ID)
- [ ] Mentions tools like normalizr for API response transformation
- [ ] Discusses denormalization for frequently accessed data
- [ ] Considers cache invalidation strategies

---

## React Native - Environment and Expo (Intermediate Level)

### 21. Expo vs Bare Workflow
When would you choose Expo managed workflow vs bare workflow? What are the trade-offs?

**Evaluation Criteria:**
- [ ] Explains managed workflow: Expo handles build process, limited to Expo SDK
- [ ] Describes bare workflow: full access to native code, manual build management
- [ ] Mentions managed benefits: faster development, easier setup, automatic updates
- [ ] Explains managed limitations: limited native modules, larger bundle size, Expo dependencies
- [ ] Describes bare workflow benefits: full flexibility, custom native modules, smaller bundles
- [ ] Mentions bare workflow complexity: requires native development knowledge, manual setup
- [ ] Discusses ejecting from managed to bare workflow
- [ ] Explains when to choose each based on project requirements
- [ ] Mentions Expo Dev Build as a middle ground option

### 22. Metro Configuration
```javascript
// Configure Metro bundler for:
// 1. Custom file extensions
// 2. Alias resolution
// 3. Performance optimization
// 4. Monorepo setup
```

### 23. Development Environment Setup
Describe your ideal React Native development environment setup. What tools and configurations do you use?

**Evaluation Criteria:**
- [ ] Mentions Node.js version management (nvm, volta)
- [ ] Describes IDE setup (VS Code extensions, Android Studio, Xcode)
- [ ] Explains simulator/emulator configuration
- [ ] Discusses development servers and hot reloading
- [ ] Mentions debugging tools (Flipper, React DevTools)
- [ ] Describes version control setup and hooks
- [ ] Explains environment variable management
- [ ] Mentions package management (npm, yarn, dependency management)
- [ ] Discusses team consistency (EditorConfig, ESLint, Prettier)

### 24. Debugging Tools
Compare the debugging capabilities of Flipper, React Native Debugger, and Expo DevTools.

**Evaluation Criteria:**
- [ ] **Flipper**: Native debugging, network inspection, layout inspector, crash reports
- [ ] **React Native Debugger**: React DevTools integration, Redux DevTools, network tab
- [ ] **Expo DevTools**: Expo-specific features, performance monitoring, remote debugging
- [ ] Compares network debugging capabilities across tools
- [ ] Discusses performance profiling features
- [ ] Explains when to use each tool based on project setup
- [ ] Mentions limitations and compatibility issues
- [ ] Describes integration with CI/CD and automated testing

### 25. Build Optimization
How do you optimize React Native bundle size and startup performance?

**Evaluation Criteria:**
- [ ] Mentions bundle splitting and code splitting techniques
- [ ] Describes tree shaking and dead code elimination
- [ ] Explains image optimization and asset management
- [ ] Discusses ProGuard/R8 for Android optimization
- [ ] Mentions RAM bundle and inline requires
- [ ] Describes font optimization and custom fonts
- [ ] Explains startup performance metrics and measurement
- [ ] Discusses lazy loading and dynamic imports
- [ ] Mentions platform-specific optimizations

---

## React Native - Publishing and Distribution (Intermediate Level)

### 26. CI/CD Pipeline
Design a CI/CD pipeline for React Native that handles:
- Automated testing
- Code signing
- App store deployment
- Different environments (staging, production)

### 27. Code Push Strategy
When and how would you implement CodePush? What are the limitations and best practices?

**Evaluation Criteria:**
- [ ] Explains CodePush for over-the-air updates without app store approval
- [ ] Describes when to use: bug fixes, content updates, feature flags
- [ ] Mentions limitations: no native code changes, binary size limits
- [ ] Explains deployment strategies: staged rollouts, A/B testing
- [ ] Discusses rollback capabilities and monitoring
- [ ] Mentions security considerations and code signing
- [ ] Describes integration with CI/CD pipelines
- [ ] Explains version compatibility and targeting
- [ ] Discusses user experience during updates

### 28. App Store Optimization
What steps do you take to ensure smooth App Store and Play Store submissions?

**Evaluation Criteria:**
- [ ] Explains app store guidelines compliance (content, technical requirements)
- [ ] Describes testing procedures: device testing, edge cases, accessibility
- [ ] Mentions metadata optimization: descriptions, keywords, screenshots
- [ ] Explains build preparation: signing, permissions, configurations
- [ ] Discusses review process management and common rejection reasons
- [ ] Mentions phased rollouts and monitoring post-release
- [ ] Describes ASO (App Store Optimization) techniques
- [ ] Explains compliance with privacy policies and data handling
- [ ] Discusses version management and backward compatibility

### 29. Version Management
How do you handle versioning in React Native apps? Explain semantic versioning strategy.

**Evaluation Criteria:**
- [ ] Explains semantic versioning: MAJOR.MINOR.PATCH format
- [ ] Describes version bumping strategies for different change types
- [ ] Mentions platform-specific versioning (iOS build numbers, Android version codes)
- [ ] Explains coordination between app version and API versions
- [ ] Describes version compatibility and minimum supported versions
- [ ] Mentions automated versioning in CI/CD pipelines
- [ ] Discusses version tracking and release notes
- [ ] Explains hotfix versioning and emergency releases
- [ ] Describes version deprecation strategies

### 30. Distribution Certificates
Explain the difference between development, distribution, and ad-hoc provisioning profiles in iOS.

**Evaluation Criteria:**
- [ ] **Development**: For testing on registered devices during development
- [ ] **Distribution**: For App Store submissions and enterprise distribution
- [ ] **Ad-hoc**: For testing with limited devices outside App Store
- [ ] Explains certificate vs provisioning profile differences
- [ ] Describes device registration and UDID management
- [ ] Mentions enterprise certificates and their use cases
- [ ] Explains certificate expiration and renewal processes
- [ ] Describes code signing and entitlements
- [ ] Mentions security implications and best practices

---

## React Native - Native Modules (Advanced Level)

### 31. Native Module Creation
```javascript
// Create a native module that:
// 1. Accesses device battery information
// 2. Handles callbacks and promises
// 3. Supports both iOS and Android
```

### 32. Bridge Communication
Explain how the React Native bridge works. What are the performance implications of frequent bridge communication?

**Evaluation Criteria:**
- [ ] Explains that bridge is asynchronous communication layer between JS and native
- [ ] Describes serialization/deserialization of data across bridge
- [ ] Mentions that bridge communication is batched for performance
- [ ] Explains performance implications: latency, serialization overhead, main thread blocking
- [ ] Describes when bridge becomes a bottleneck (animations, frequent updates)
- [ ] Mentions strategies to minimize bridge traffic (batching, caching, native solutions)
- [ ] Explains the JSON serialization limitation
- [ ] Discusses the single-threaded nature of the bridge

### 33. Turbo Modules
What are Turbo Modules and how do they improve upon legacy native modules?

**Evaluation Criteria:**
- [ ] Explains that Turbo Modules enable synchronous native calls
- [ ] Describes lazy loading of native modules (load on demand)
- [ ] Mentions better TypeScript support with code generation
- [ ] Explains direct JSI (JavaScript Interface) usage bypassing the bridge
- [ ] Describes performance improvements: no serialization, direct memory access
- [ ] Mentions that they're part of the New Architecture (with Fabric)
- [ ] Explains backward compatibility considerations
- [ ] Discusses the migration path from legacy modules

### 34. Platform-Specific Code
How do you handle platform-specific implementations while maintaining code reusability?

**Evaluation Criteria:**
- [ ] Explains Platform.OS for simple platform checks
- [ ] Describes platform-specific file extensions (.ios.js, .android.js)
- [ ] Mentions Platform.select() for object-based selection
- [ ] Explains when to create separate components vs shared logic
- [ ] Describes native module platform implementations
- [ ] Mentions conditional imports and dynamic loading
- [ ] Explains maintaining shared interfaces across platforms
- [ ] Discusses testing strategies for platform-specific code
- [ ] Mentions design system considerations for platform differences

### 35. Native Module Debugging
What techniques do you use to debug native modules on iOS and Android?

**Evaluation Criteria:**
- [ ] **iOS**: Xcode debugger, console logs, breakpoints in Objective-C/Swift
- [ ] **Android**: Android Studio debugger, ADB logs, breakpoints in Java/Kotlin
- [ ] Explains bridge communication debugging techniques
- [ ] Mentions logging strategies across JS/Native boundary
- [ ] Describes crash log analysis and symbolication
- [ ] Explains performance profiling for native code
- [ ] Mentions simulator vs device debugging differences
- [ ] Describes testing strategies for native modules
- [ ] Explains integration testing with JavaScript layer

---

## React Native - Debugging (Intermediate Level)

### 36. Performance Debugging
```javascript
// Debug and optimize this slow scrolling list
const SlowList = ({ data }) => {
  return (
    <FlatList
      data={data}
      renderItem={({ item }) => (
        <View style={styles.item}>
          <Image source={{ uri: item.image }} style={styles.image} />
          <Text>{item.title}</Text>
          <Text>{new Date(item.createdAt).toLocaleDateString()}</Text>
        </View>
      )}
    />
  );
};
```

### 37. Memory Leak Detection
How do you identify and fix memory leaks in React Native apps?

**Evaluation Criteria:**
- [ ] Mentions profiling tools: Xcode Instruments, Android Studio Profiler
- [ ] Describes heap snapshot analysis and memory growth patterns
- [ ] Explains common leak sources: listeners, timers, closures, images
- [ ] Mentions React DevTools Profiler for component analysis
- [ ] Describes automated leak detection in CI/CD
- [ ] Explains memory pressure testing and stress testing
- [ ] Mentions platform-specific tools and techniques
- [ ] Describes fixing techniques: cleanup functions, weak references
- [ ] Explains monitoring memory usage in production

### 38. Crash Reporting
Set up comprehensive crash reporting and error tracking for a React Native app.

**Evaluation Criteria:**
- [ ] Mentions crash reporting services: Crashlytics, Sentry, Bugsnag
- [ ] Explains JavaScript error boundaries and native crash handling
- [ ] Describes symbolication for meaningful stack traces
- [ ] Mentions custom error reporting and user context
- [ ] Explains crash grouping and prioritization
- [ ] Describes integration with CI/CD and alerting systems
- [ ] Mentions privacy considerations and data handling
- [ ] Explains crash reproduction and debugging workflows
- [ ] Describes performance monitoring alongside crash reporting

### 39. Network Debugging
What tools and techniques do you use to debug network requests in React Native?

**Evaluation Criteria:**
- [ ] Mentions network inspection tools: Flipper, Charles Proxy, Wireshark
- [ ] Explains React Native Debugger network tab usage
- [ ] Describes request/response logging and interception
- [ ] Mentions platform-specific debugging (iOS Network Link Conditioner)
- [ ] Explains SSL/TLS debugging and certificate issues
- [ ] Describes timeout and connectivity issue debugging
- [ ] Mentions performance analysis and optimization
- [ ] Explains debugging in different environments (dev, staging, production)
- [ ] Describes API mocking and testing strategies

### 40. Production Debugging
How do you debug issues that only occur in production builds?

**Evaluation Criteria:**
- [ ] Explains production build differences: optimizations, minification, bundling
- [ ] Mentions remote debugging limitations and alternatives
- [ ] Describes log aggregation and monitoring systems
- [ ] Explains reproducing production issues in development
- [ ] Mentions feature flags and gradual rollouts for debugging
- [ ] Describes user feedback collection and bug reporting
- [ ] Explains crash analytics and performance monitoring
- [ ] Mentions A/B testing for issue isolation
- [ ] Describes rollback strategies and hotfix deployment

---

## JavaScript - Common/Objects/Functions (Advanced Level)

### 41. Prototypal Inheritance
```javascript
// Implement a proper inheritance chain
function Animal(name) {
  this.name = name;
}

function Dog(name, breed) {
  // Implement proper inheritance
}

// Ensure instanceof works correctly
// Add method inheritance
// Handle property shadowing
```

### 42. Object Manipulation
```javascript
// What's the output and why?
const obj = { a: 1, b: 2 };
const newObj = { ...obj, a: 3, c: 4 };
delete newObj.b;
Object.freeze(newObj);
newObj.a = 5;
newObj.d = 6;
console.log(newObj);
```

### 43. Function Composition
Implement a `compose` function that allows: `compose(f, g, h)(x) === f(g(h(x)))`

### 44. This Binding
```javascript
// Explain the output
const obj = {
  name: 'Object',
  getName: function() { return this.name; },
  getNameArrow: () => this.name,
  getNameBound: function() { return this.name; }.bind(this)
};

const getName = obj.getName;
console.log(getName());
console.log(obj.getName());
console.log(obj.getNameArrow());
```

### 45. Closures and Memory
Identify and fix the memory leak:
```javascript
function createHandlers() {
  const largeData = new Array(1000000).fill('data');
  
  return {
    handleClick: function() {
      console.log('clicked');
    },
    handleHover: function() {
      console.log('hovered');
    }
  };
}
```

---

## JavaScript - Asynchronous Programming (Advanced Level)

### 46. Promise Implementation
Implement a basic Promise from scratch with `then`, `catch`, and `finally` methods.

**Evaluation Criteria:**
- [ ] Understands the three states: pending, fulfilled, rejected
- [ ] Implements state transitions correctly (only from pending)
- [ ] Handles synchronous and asynchronous resolution/rejection
- [ ] Implements proper callback queuing for then/catch handlers
- [ ] Ensures handlers are called asynchronously (microtask)
- [ ] Implements promise chaining correctly
- [ ] Handles error propagation through the chain
- [ ] Implements finally method that doesn't change the promise value
- [ ] Considers edge cases: multiple then calls, immediate resolution

### 47. Async/Await Error Handling
```javascript
// Improve this error handling
async function fetchUserData(userId) {
  try {
    const user = await fetch(`/users/${userId}`);
    const posts = await fetch(`/users/${userId}/posts`);
    return { user: user.json(), posts: posts.json() };
  } catch (error) {
    console.error(error);
  }
}
```

### 48. Event Loop Understanding
Predict the output:
```javascript
console.log('1');

setTimeout(() => console.log('2'), 0);

Promise.resolve().then(() => console.log('3'));

queueMicrotask(() => console.log('4'));

console.log('5');
```

**Evaluation Criteria:**
- [ ] Correctly predicts output: 1, 5, 3, 4, 2
- [ ] Explains synchronous code executes first (1, 5)
- [ ] Describes microtask queue vs macrotask queue difference
- [ ] Explains that microtasks have higher priority than macrotasks
- [ ] Mentions that Promise.then and queueMicrotask are microtasks
- [ ] Explains that setTimeout is a macrotask
- [ ] Describes the event loop cycle: call stack → microtasks → macrotasks
- [ ] Understands that microtask queue is drained completely before macrotasks

### 49. Async Iterator
Implement an async iterator that fetches paginated data:
```javascript
class PaginatedAPI {
  constructor(baseUrl) {
    this.baseUrl = baseUrl;
  }
  
  // Implement async iterator
  async *[Symbol.asyncIterator]() {
    // Implementation here
  }
}
```

**Evaluation Criteria:**
- [ ] Understands async generator syntax (async function*)
- [ ] Implements Symbol.asyncIterator correctly
- [ ] Handles async operations within the generator (await fetch)
- [ ] Implements pagination logic (page tracking, hasNext checks)
- [ ] Uses yield for each page of data
- [ ] Handles errors appropriately within async iterator
- [ ] Implements termination condition for pagination
- [ ] Understands for-await-of loop consumption
- [ ] Considers memory efficiency and lazy loading

---

## TypeScript - Types/Classes (Advanced Level)

### 50. Advanced Type System
```typescript
// Create a type that extracts the return type of async functions
type AsyncReturnType<T> = /* implementation */;

// Usage
async function fetchUser() {
  return { id: 1, name: 'John' };
}

type User = AsyncReturnType<typeof fetchUser>; // Should be { id: number; name: string; }
```

**Evaluation Criteria:**
- [ ] Understands that async functions return Promise<T>
- [ ] Uses conditional types to extract the Promise type
- [ ] Implements: `T extends (...args: any[]) => Promise<infer R> ? R : never`
- [ ] Explains the `infer` keyword for type inference
- [ ] Handles the function type constraint properly
- [ ] Understands template literal types and utility types
- [ ] Can explain how conditional types work in TypeScript
- [ ] Mentions potential edge cases (non-async functions, overloaded functions)

### 51. Conditional Types
```typescript
// Implement a Pick type for nested objects
type DeepPick<T, K extends string> = /* implementation */;

interface User {
  profile: {
    personal: {
      name: string;
      age: number;
    };
    settings: {
      theme: string;
    };
  };
}

type UserName = DeepPick<User, 'profile.personal.name'>;
```

**Evaluation Criteria:**
- [ ] Understands conditional types with extends keyword
- [ ] Uses template literal types for path parsing
- [ ] Implements recursive type traversal for nested objects
- [ ] Uses infer keyword for type extraction
- [ ] Handles dot notation parsing ('profile.personal.name')
- [ ] Implements proper type constraints and validations
- [ ] Understands mapped types and keyof operator
- [ ] Considers edge cases: invalid paths, primitive types
- [ ] Explains the utility and use cases of such advanced types

### 52. Generic Constraints
Design a type-safe event emitter with strongly typed events:
```typescript
interface Events {
  'user:login': { userId: string; timestamp: Date };
  'user:logout': { userId: string };
  'post:created': { postId: string; authorId: string };
}

class TypedEventEmitter<T extends Record<string, any>> {
  // Implementation
}
```

**Evaluation Criteria:**
- [ ] Uses generic constraints (T extends Record<string, any>)
- [ ] Implements type-safe emit method with event names and payloads
- [ ] Creates strongly typed on/off methods with proper event typing
- [ ] Uses keyof T for event name restrictions
- [ ] Implements proper method signatures with conditional types
- [ ] Handles listener function typing correctly
- [ ] Uses method overloading or union types appropriately
- [ ] Considers type inference and IntelliSense experience
- [ ] Explains benefits of compile-time type safety for events

---

## Software Engineering - OOP/Functional Programming (Advanced Level)

### 53. Design Patterns
Implement the Observer pattern for a React Native app's state management without using external libraries.

**Evaluation Criteria:**
- [ ] Implements Subject (Observable) with subscribe/unsubscribe methods
- [ ] Implements Observer interface with update/notify method
- [ ] Maintains list of observers and notifies them on state changes
- [ ] Handles subscription cleanup to prevent memory leaks
- [ ] Considers React-specific integration (useEffect for subscription)
- [ ] Implements proper event types and payload structure
- [ ] Handles error cases (null observers, failed notifications)
- [ ] Discusses when Observer pattern is appropriate vs alternatives
- [ ] Mentions performance considerations (large observer lists)

### 54. SOLID Principles
```javascript
// Refactor this class to follow SOLID principles
class UserService {
  constructor() {
    this.db = new Database();
    this.email = new EmailService();
    this.logger = new Logger();
  }
  
  createUser(userData) {
    // Validates data
    if (!userData.email || !userData.name) {
      throw new Error('Invalid data');
    }
    
    // Saves to database
    const user = this.db.save(userData);
    
    // Sends email
    this.email.send(userData.email, 'Welcome!');
    
    // Logs action
    this.logger.log(`User created: ${user.id}`);
    
    return user;
  }
}
```

**Evaluation Criteria:**
- [ ] **S** - Single Responsibility: Separates validation, persistence, notification, logging
- [ ] **O** - Open/Closed: Uses interfaces/abstractions for extensibility
- [ ] **L** - Liskov Substitution: Ensures interfaces can be substituted
- [ ] **I** - Interface Segregation: Creates focused interfaces (IValidator, IUserRepository, etc.)
- [ ] **D** - Dependency Inversion: Injects dependencies rather than creating them
- [ ] Implements constructor injection for dependencies
- [ ] Creates separate classes: UserValidator, UserRepository, NotificationService
- [ ] Uses interfaces/abstractions for loose coupling
- [ ] Demonstrates understanding of why each principle matters

### 55. Functional Programming
Implement these functional programming utilities:
- `curry`: Transform a function to be curried
- `pipe`: Compose functions left-to-right
- `debounce`: Functional debouncing without side effects

**Evaluation Criteria:**
- [ ] **Curry**: Implements partial application, handles variable arity functions
- [ ] **Pipe**: Composes functions left-to-right (f(g(h(x))))
- [ ] **Debounce**: Creates pure function without shared state, returns new debounced function
- [ ] Understands higher-order functions and closures
- [ ] Implements proper TypeScript typing for generic functions
- [ ] Explains functional programming principles: immutability, pure functions
- [ ] Discusses advantages: testability, predictability, composition
- [ ] Mentions functional libraries: Ramda, Lodash/FP, fp-ts
- [ ] Explains when to use functional vs object-oriented approaches

---

## Software Engineering - Network/Git (Advanced Level)

### 56. HTTP Optimization
Design a caching strategy for a React Native app that handles:
- Network-first vs cache-first strategies
- Offline functionality
- Cache invalidation
- Background sync

**Evaluation Criteria:**
- [ ] **Network-first**: Fresh data priority, fallback to cache on failure
- [ ] **Cache-first**: Performance priority, background refresh strategies
- [ ] **Offline**: Local storage solutions, data synchronization on reconnect
- [ ] **Cache invalidation**: TTL, versioning, manual invalidation, dependency tracking
- [ ] **Background sync**: Queue management, conflict resolution, batch operations
- [ ] Mentions HTTP caching headers (ETag, Cache-Control, Last-Modified)
- [ ] Describes storage solutions: AsyncStorage, SQLite, Realm
- [ ] Explains cache layers: memory, disk, network
- [ ] Discusses cache size management and cleanup strategies

### 57. Git Workflow
Describe your preferred Git workflow for a team of 8 developers working on a React Native app. How do you handle:
- Feature branches
- Code reviews
- Release management
- Hotfixes

**Evaluation Criteria:**
- [ ] Describes a clear branching strategy (GitFlow, GitHub Flow, or similar)
- [ ] Explains feature branch naming conventions and lifecycle
- [ ] Details code review process: PR templates, required approvers, CI checks
- [ ] Describes release branch management and versioning strategy
- [ ] Explains hotfix process: emergency branches, fast-track reviews
- [ ] Mentions protection rules for main/master branch
- [ ] Discusses merge vs rebase strategies and when to use each
- [ ] Explains conflict resolution processes
- [ ] Mentions automated testing and CI/CD integration
- [ ] Considers mobile-specific concerns: app store releases, binary artifacts

### 58. API Design
Design a RESTful API for a mobile app that needs to:
- Handle offline synchronization
- Support pagination
- Provide real-time updates
- Handle authentication and authorization

**Evaluation Criteria:**
- [ ] Designs proper REST endpoints with clear resource naming
- [ ] Implements conflict resolution for offline sync (timestamps, version vectors)
- [ ] Describes pagination strategies: cursor-based or offset-based
- [ ] Explains real-time options: WebSockets, Server-Sent Events, or polling
- [ ] Implements proper authentication: JWT, OAuth2, or similar
- [ ] Describes authorization patterns: RBAC, ABAC, or resource-based
- [ ] Handles data versioning and migration strategies
- [ ] Considers mobile constraints: bandwidth, battery, intermittent connectivity
- [ ] Implements proper error handling and status codes
- [ ] Discusses caching strategies and ETags for efficiency

### 59. Security Best Practices
What security measures do you implement in React Native apps regarding:
- API communication
- Local data storage
- Authentication tokens
- Code obfuscation

**Evaluation Criteria:**
- [ ] **API Communication**: HTTPS/TLS, certificate pinning, request signing, rate limiting
- [ ] **Local Storage**: Keychain/Keystore for sensitive data, avoid AsyncStorage for secrets
- [ ] **Authentication**: Secure token storage, refresh token rotation, biometric authentication
- [ ] **Code Obfuscation**: Bundle obfuscation, ProGuard/R8, remove debug info in production
- [ ] Mentions additional security measures: jailbreak/root detection, anti-tampering
- [ ] Describes secure coding practices: input validation, XSS prevention
- [ ] Explains security testing: penetration testing, static analysis tools
- [ ] Discusses compliance requirements: GDPR, CCPA, industry standards
- [ ] Mentions security monitoring and incident response plans

### 60. Performance Monitoring
How do you implement comprehensive performance monitoring for a React Native app in production?

**Evaluation Criteria:**
- [ ] Mentions performance monitoring tools: Firebase Performance, New Relic, Datadog
- [ ] Explains key metrics: app startup time, screen load time, memory usage, CPU usage
- [ ] Describes custom performance tracking and business metrics
- [ ] Mentions React Native performance profiling tools and techniques
- [ ] Explains network performance monitoring and API response times
- [ ] Describes crash monitoring integration with performance data
- [ ] Mentions user experience metrics: frame drops, ANR detection
- [ ] Explains performance budgets and alerting thresholds
- [ ] Describes A/B testing for performance optimization
- [ ] Mentions battery usage and thermal performance monitoring

---

## Scenario-Based Questions

### 61. Architecture Decision
You're tasked with building a React Native app that needs to work offline and sync when online. Design the architecture including data flow, state management, and synchronization strategy.

### 62. Legacy Code Migration
How would you approach migrating a large React Native codebase from:
- Class components to functional components
- JavaScript to TypeScript
- Redux to a modern state management solution

### 63. Performance Crisis
A React Native app is experiencing severe performance issues on Android devices. Walk me through your debugging and optimization process.

### 64. Team Scalability
How would you structure a React Native codebase to support a growing team and multiple feature teams working simultaneously?

### 65. Production Emergency
A critical bug is discovered in production that causes the app to crash for users on iOS 15+. Describe your immediate response and long-term prevention strategy.

---

---

## Interview Evaluation Guide

**Note:** This interview format allows for flexible selection of questions based on candidate responses and time constraints. Focus on 8-12 questions that best evaluate the candidate's expertise in the most critical areas for the role.

### Using the Evaluation Criteria

**For Theoretical Questions:**
- Use the provided checklists to assess depth of understanding
- A senior candidate should cover 70-80% of the listed points
- Look for clear explanations, not just buzzword mentions
- Value practical experience over textbook knowledge

**Scoring Guidance:**
- **Excellent (4/5)**: Covers most criteria points with clear examples and deep understanding
- **Good (3/5)**: Covers key points but may lack depth in some areas
- **Satisfactory (2/5)**: Basic understanding but missing important concepts
- **Needs Improvement (1/5)**: Limited knowledge with significant gaps

**Red Flags:**
- Cannot explain fundamental concepts for their claimed expertise level
- Gives outdated information without acknowledging evolution
- Shows no practical experience with technologies they claim to know
- Cannot connect theoretical knowledge to real-world applications

**Positive Indicators:**
- Provides concrete examples from real projects
- Acknowledges trade-offs and limitations
- Asks clarifying questions about requirements
- Demonstrates continuous learning and adaptation to new technologies