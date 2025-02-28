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

1. What is TypeScript and why is it used?
