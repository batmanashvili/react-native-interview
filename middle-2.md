### Javascript

1. "use strict" and Variable Hoisting

What are the main restrictions imposed by "use strict" in JavaScript? How does variable hoisting work differently under strict mode compared to non-strict mode?

Expected Answer:
* "use strict" prevents implicit global variable declarations.
* It disallows duplicate parameter names in functions.
* Throws an error when trying to assign values to read-only properties.
* Hoisting behavior:
```javascript
function test() {
  "use strict";
  x = 10; // ReferenceError: x is not defined
}
Without "use strict", x would become a global variable.
```


2. what would be the output of the following code?
```javascript
const initialState = {
  user: { details: { name: "Alice", age: 25 } },
};

const updatedState = {
  ...initialState,
};

updatedState.user.details.age = 26;

console.log(initialState);
console.log(updatedState);
```

3. what is difference between Object and Map? 

Expected Answer:
* Objects are key-value pairs, while Maps can have any type of key.
* Objects have built-in methods like hasOwnProperty, while Maps have a size property and specific methods like set, get, delete, and has.
* Objects are not iterable, while Maps are iterable.

4. What is difference between Map and WeakMap?

Expected Answer:
* WeakMap keys are only objects, not primitive values.
* WeakMap does not have a size property.
* WeakMap does not have a clear method.
* WeakMap does not have a forEach method.
* Keys are not strongly referenced, they can be garbage collected if there are no other references to them.

```javascript
let obj = { name: "React" };
let weakMap = new WeakMap();
weakMap.set(obj, "value");

obj = null; // The key-value pair is garbage-collected
```

5. What is the type Symbol and what will be the output of the following code? (Data Types)

```javascript
const sym1 = Symbol("test");
const sym2 = Symbol("test");

const obj = {
  [sym1]: "value1",
  [sym2]: "value2"
};

console.log(obj[sym1]); // ?
console.log(obj[sym2]); // ?
console.log(obj["test"]); // ?
console.log(Object.keys(obj)); // ?
```

6. "this" keyword and "use strict"
```javascript

"use strict";

const value = 1;

function createObject(){
  const value = 2;
  var obj = {
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
};

createObject();
```

7. Asynchronous Programming 

```javascript
try {
  setTimeout(() => {
    Promise.reject(console.log('reject'));
  }, 0);
} catch {
  console.error('error');
}
```

8. Functions, Lexical Environement and Closures

```javascript
function outerFunction() {
  let count = 0;
  
  return function innerFunction() {
    count++;
    console.log(count);
  };
}

const firstCall = outerFunction();
const secondCall = outerFunction();

firstCall(); // ?
firstCall(); // ?
secondCall(); // ?
```

9. Generators:

```javascript
function* simpleGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = simpleGenerator();

console.log(gen.next()); // ?
console.log(gen.next()); // ?
console.log(gen.next()); // ?
console.log(gen.next()); // ?
```

10. Generators

```javascript
function* advancedGenerator() {
  try {
    yield 1;
    yield 2;
    return 3;
    yield 4;
  } finally {
    console.log("Cleanup code executed");
  }
}

const gen = advancedGenerator();

console.log(gen.next()); // ?
console.log(gen.next()); // ?
console.log(gen.next()); // ?
console.log(gen.next()); // ?

const gen2 = advancedGenerator();
console.log(gen2.throw(new Error("Manual error"))); // ?
console.log(gen2.next()); // ?
```

### TypeScript

1. What is difference between "any" and "unknown" ?
The any type is a catch-all type that disables TypeScript's type checking for a variable.

On "unknown" TypeScript forces you to perform some type of checking before performing operations on it.
  
```javascript
let value: unknown | any = "John";
value.toUpperCase();
```

2. What is type casting ?

3. What is generic type and when do we need to use it?

4. 



### React

Reconcilation 
1. How does React determine if a component should be updated or replaced during reconciliation?

React compares the virtual DOM of the previous and current render. If the element type (tag name or component type) remains the same, React updates the component instead of unmounting and re-mounting it. Otherwise, it replaces the entire subtree.

2. What is fiber?
 * Sync vs new prioritization renders 
 * Fiber Tree: Fiber maintains two trees: the "current" tree (what’s on screen) and the "work-in-progress" tree (what’s being built). Each fiber node represents a piece of work (e.g., a component render).
 * Scheduling: Fiber assigns a priority level to each update, determining when and how it gets processed. This is managed by the React Scheduler (a separate package, scheduler, integrated into React).

3. How prioritization works in fiber?

React uses a set of internal priority levels (exposed conceptually in APIs like useTransition). These include:

* Immediate (Synchronous): For urgent updates, like typing in an input field. These are processed right away and block other work.
* User-Blocking: High-priority updates that should feel instant (e.g., button clicks). These have a short timeout (e.g., 250ms).
* Normal: Default priority for most updates (e.g., fetching data after a click). These can be deferred slightly (e.g., 5 seconds).
* Low: Background tasks (e.g., preloading data). These can be delayed significantly.
* Idle: Work that happens when the app is idle (e.g., analytics logging).


4. Rules of hooks?

* Only Call Hooks at the Top Level
* Only Call Hooks from React Functional Components (or Custom Hooks)
* 

5. How are the hooks registered? 

React hooks (like useState, useEffect, etc.) are registered and managed through the Fiber reconciliation system.

Every functional component in React has an associated Fiber node in the Fiber tree.
When a functional component renders, React keeps track of its hooks in a linked list attached to that Fiber node.
This linked list is stored in the memoizedState property of the Fiber node for the component.

Hooks are registered in the order they are called during a component’s render phase.

React relies on the call order of hooks being consistent across renders.

React relies on the call order of hooks being consistent across renders. This is why you can’t put hooks inside conditionals or loops—React wouldn’t know which hook belongs to which state if the order changed.


6. Explain why setState is asynchronous and how it can be used.

7. What is Batching?

```javascript

import { useState } from 'react';
import { Button, Text } from 'react-native';

function Counter() {
  const [state, setState] = useState<number>(0);

  const handlePress = () => {
    setState(state + 1);
    setState(state + 1);
    setState(state + 1);
    console.log(state); // ?
  };

  console.log(state) // ?

  // How many rerenders ? 
  return (
    <>
      <Text>Count: {state}</Text>
      <Button title="Increment" onPress={handlePress} />
    </>
  );
}

export default Counter;
```


8. Does this cause extra renders?

```javascript
const user = useSelector((state) => state.user);
```

9. What is the difference between a simple selector and a memoized selector?

```javascript
const getUser = (state) => state.user;

const getUserFullName = createSelector(
  [getUser],
  (user) => `${user.firstName} ${user.lastName}`
);
```

10. 