To master modern JavaScript, developer roadmaps and comprehensive tutorials emphasize building a strong foundation in several core areas before moving on to frameworks. Here are the most important JavaScript topics you must understand:

# 1. Variables, Scope, and Fundamentals

* **Variable Declarations:** Understand the differences between **`var`, `let`, and `const`**.
You should know why `let` and `const` (which are block-scoped)
are preferred over `var` (which is function-scoped), and understand the "temporal dead zone".

## 1.1. 🟢 1. `var` (Function Scoped)

- Scope: Function-scoped
- Can be re-declared and updated
- Hoisted and initialized with `undefined`

### 1.1.1. ✅ Example:

```javascript
function testVar() {
  if (true) {
    var x = 10;
  }
  console.log(x); // accessible outside block
}
```

🔁 Re-declaration allowed:

```javascript
var a = 5;
var a = 10;
console.log(a);
```

## 1.2. 🔵 2. let (Block Scoped)

Scope: Block { }
Cannot be re-declared in same scope
Can be updated
Hoisted but in Temporal Dead Zone (TDZ)

```javascript
function testLet() {
  if (true) {
    let x = 10;
  }
  console.log(x); // not accessible
}
```

🔁 Update allowed:

```javascript
let a = 5;
a = 10;
console.log(a);
```

❌ Re-declaration not allowed:

```javascript
let a = 5;
let a = 10;
```

⚠️ Temporal Dead Zone:

```javascript
console.log(a);
let a = 10;
```

🖨 Output:

ReferenceError: Cannot access 'a' before initialization

## 1.3. 🔴 3. const (Block Scoped, Immutable Binding)

Scope: Block { }
Cannot be re-assigned
Must be initialized at declaration
Also has TDZTemporal Dead Zone

```javascript
const a = 10;
console.log(a);
```

❌ Re-assignment not allowed:

```javascript
const a = 10;
a = 20;
```

🧠 Important: Objects with const

```javascript
const obj = { name: "John" };
obj.name = "Doe";
console.log(obj.name);
```

🖨 Output:

Doe

## ⚡ Key Differences Summary

| Feature        | var                 | let            | const          |
|----------------|---------------------|----------------|----------------|
| Scope          | Function            | Block          | Block          |
| Re-declare     | ✅ Yes              | ❌ No          | ❌ No         |
| Update         | ✅ Yes              | ✅ Yes         | ❌ No         |
| Hoisting       | ✅ Yes (undefined)  | ✅ Yes (TDZ)   | ✅ Yes (TDZ)   |
| Initialization | Optional            | Optional       | Required       |

* **Equality and Coercion:**difference between **strict equality (`===`) and loose equality (`==`)**.

🔹 Strict Equality (===)

Checks both value AND data type
No type conversion (no coercion)

```javascript
5 === 5        // true
5 === "5"      // false (number !== string)
true === 1     // false
null === undefined // false
```

🔹 Loose Equality (==)

Checks only value
Performs type conversion (coercion) if types differ

```javascript
5 == "5"       // true (string converted to number)
true == 1      // true
false == 0     // true
null == undefined // true
"" == 0           // true (empty string coerced to 0)
```

| Expression        | == Result | === Result | Reason freecodecamp+1     |
| ----------------- | --------- | ---------- | ------------------------- |
| 1 == "1"          | true      | false      | String coerced to number  |
| true == 1         | true      | false      | Boolean coerced to number |
| null == undefined | true      | false      | Special loose rule        |
| 0 == false        | true      | false      | Boolean coerced to number |
| "" == 0           | true      | false      | Empty string coerced to 0 |

### why `===` is considered the best practice.

🔹 1. No Unexpected Type Conversion

🔹 2. More Predictable Code

=== behaves consistently because it checks:

Value

Type

🔹 3. Easier Debugging

When bugs happen with ==, they’re often due to silent conversions.

```javascript
if (userInput == 0) {
  // might run even if input is "", false, or "0"
}
-- --
if (userInput === 0) {
  // runs ONLY when it's actually number 0
}
```

🔹 4. Cleaner and Safer Logic

```javascript
Number("5") === 5   // true (you control conversion)
```

# 2. Functions and the `this` Keyword

* **Arrow Functions vs. Ordinary Functions:** Learn the syntax for both, but more importantly, understand how they differ in their handling of the **`this` context**. Arrow functions inherit `this` lexically from their surrounding scope, while ordinary functions have dynamic `this` based on how they are called.

🔹 1. Syntax Difference

✅ Ordinary Function

```javascript
function greet() {
  console.log("Hello");
}
```

✅ Arrow Function

```javascript
const greet = () => {
  console.log("Hello");
};
```

🔹 2. The Key Difference: this Behavior

🔸 Ordinary Function (function)

this is dynamic

Depends on how the function is called

```javascript
const obj = {
  name: "John",
  sayName: function () {
    console.log(this.name);
  }
};
obj.sayName(); // "John"
```

👉 Here, this refers to obj because the function is called using obj.

🔸 Arrow Function (= >)

this is lexical (inherits from surrounding scope)

It does NOT have its own this

```javascript
const obj = {
  name: "John",
  sayName: () => {
    console.log(this.name);
  }
};
obj.sayName(); // undefined ❌
```

👉 Here, this does NOT refer to obj

It takes this from outside (usually global scope)

🔹 3. Why This Matters (Real Example)

❌ Problem with Ordinary Function

```javascript
function Counter() {
  this.count = 0;
  setInterval(function () {
    this.count++;
    console.log(this.count);
  }, 1000);
}
```

👉 this becomes undefined or window, not Counter

✅ Solution with Arrow Function

```javascript
function Counter() {
  this.count = 0;
  setInterval(() => {
    this.count++;
    console.log(this.count);
  }, 1000);
}
```

👉 Arrow function uses parent this, so it works correctly

⚡ When to Use What?

✅ Use Arrow Functions:

Callbacks (map, setTimeout, setInterval)

When you want to preserve this

✅ Use Ordinary Functions:

Object methods

Constructors

When this should be dynamic

---------------

# 3. Arrow Functions vs. Regular Functions in JavaScript

The key difference goes beyond syntax — it's fundamentally about how `this` is resolved.

---

## 3.1. Syntax Comparison

```javascript
// Regular function
function greet(name) {
  return `Hello, ${name}`;
}

// Arrow function
const greet = (name) => `Hello, ${name}`;
```

---

## 3.2. The `this` Difference

**Regular functions** bind `this` *dynamically* — based on **how the function is called**.

**Arrow functions** bind `this` *lexically* — based on **where the function is defined**.

---

## 3.3. Example: The Classic Timer Problem

```javascript
function Timer() {
  this.seconds = 0;
  // ❌ Regular function — `this` is lost
  setInterval(function () {
    this.seconds++;           // `this` is undefined (or globalThis in non-strict)
    console.log(this.seconds); // NaN
  }, 1000);
}

function Timer() {
  this.seconds = 0;
  // ✅ Arrow function — `this` is inherited from Timer's scope
  setInterval(() => {
    this.seconds++;           // correctly refers to the Timer instance
    console.log(this.seconds); // 1, 2, 3...
  }, 1000);
}
```

The arrow function "closes over" the `this` that was in scope when it was *written*, not when it's *run*.

---

## 3.4. Example: Method on an Object

```javascript
const counter = {
  count: 0,
  // ✅ Regular function — `this` refers to `counter`
  increment() {
    this.count++;
  },
  // ❌ Arrow function — `this` is NOT `counter`, it's the module/global scope
  decrement: () => {
    this.count--; // `this` here is the module/global scope
  }
};

counter.increment(); // works fine
counter.decrement(); // broken — `this.count` is not counter.count
```

This is why **object methods should typically be regular functions**.

---

## 3.5. Quick Reference

| Feature                    | Regular Function         | Arrow Function                   |
|--------------------------- |--------------------------|----------------------------------|
| `this` binding             | Dynamic (call-site)      | Lexical (definition-site)        |
| Can be used as constructor | ✅ `new Foo()`           | ❌ throws error                 |
| Has own `arguments` object | ✅                       | ❌ (use rest params `...args`)  |
| Good for object methods    | ✅                       | ❌                              |
| Good for callbacks/closures| ⚠️ (watch `this`)        | ✅                              |

---

## 3.6. Mental Model

> **Regular function:** *"Who called me? That's my `this`."*

> **Arrow function:** *"Where was I written? That's my `this`."*

* **Context Control:** Learn how to explicitly bind the `this` context using the **`call()`, `apply()`, and `bind()`** methods.

# 4. Context Control: `call()`, `apply()`, and `bind()`

These three methods let you **explicitly set what `this` refers to** when invoking a function, overriding the default dynamic binding.

---

## 4.1. The Problem They Solve

```javascript
const user = { name: "Alice" };
function greet(greeting) {
  console.log(`${greeting}, ${this.name}!`);
}
greet("Hello"); // "Hello, undefined!" — `this` is not `user`
```

All three methods fix this by letting you say: *"Call this function, but use **this object** as `this`."*

---

## 4.2. `call()` — Invoke immediately, args spread out

```javascript
greet.call(user, "Hello"); // "Hello, Alice!"
```

```javascript
// Signature
fn.call(thisArg, arg1, arg2, ...)
```

- Calls the function **immediately**
- Arguments passed **individually**

---

## 4.3. `apply()` — Invoke immediately, args as an array

```javascript
greet.apply(user, ["Hello"]); // "Hello, Alice!"
```

```javascript
// Signature
fn.apply(thisArg, [arg1, arg2, ...])
```

- Calls the function **immediately**
- Arguments passed as an **array**

> **Memory tip:** **A**pply → **A**rray

---

## 4.4. `bind()` — Returns a new function, doesn't call it

```javascript
const boundGreet = greet.bind(user);
boundGreet("Hello"); // "Hello, Alice!"
```

```javascript
// Signature
const newFn = fn.bind(thisArg, arg1, arg2, ...)
```

- Does **not** call the function
- Returns a **new, permanently bound function**
- Bound `this` **cannot be overridden** later, even with `call`/`apply`

---

## 4.5. Side-by-Side Comparison

```javascript
const user = { name: "Alice" };
function introduce(role, company) {
  console.log(`I'm ${this.name}, ${role} at ${company}.`);
}
introduce.call(user, "Engineer", "Acme");
introduce.apply(user, ["Engineer", "Acme"]);
const intro = introduce.bind(user, "Engineer");
intro("Acme"); // can supply remaining args later
```

All three log: `I'm Alice, Engineer at Acme.'

---

## 4.6. Real-World Use Cases

**`call` — Borrowing a method from another object**

```javascript
const dog = { sound: "woof" };
const cat = { sound: "meow" };
function speak() {
  console.log(this.sound);
}
speak.call(dog); // "woof"
speak.call(cat); // "meow"
```

**`apply` — Spreading an array into a function that expects individual args**

```javascript
const numbers = [5, 1, 8, 3];
// Math.max doesn't accept an array — apply unpacks it
const max = Math.max.apply(null, numbers); // 8
// Modern equivalent (prefer this today):
const max = Math.max(...numbers); // 8
```

**`bind` — Fixing `this` for event handlers or callbacks**

```javascript
class Button {
  constructor(label) {
    this.label = label;
    // Without bind, `this` inside handleClick would be the DOM element
    this.handleClick = this.handleClick.bind(this);
  }
  handleClick() {
    console.log(`Clicked: ${this.label}`);
  }
}
```

---

## 4.7. Quick Reference

| Method   | Calls Immediately? | Args Format                       | Returns                 |
|----------|--------------------|-----------------------------------|-------------------------|
| `call()` | ✅ Yes             | Spread: `f.call(ctx, a, b)`       | Function's return value |
| `apply()`| ✅ Yes             | Array: `f.apply(ctx, [a, b])`     | Function's return value |
| `bind()` | ❌ No              | Spread (pre-set): `f.bind(ctx, a)`| New bound function      |

---

## 4.8. Mental Model

> **`call`** and **`apply`** are like saying: *"Run this function now, but pretend it belongs to this object."*

> **`bind`** is like saying: *"Give me a version of this function that **always** belongs to this object."*

* **Closures:** This is a crucial concept where an inner function retains access to the variables of its outer function even after the outer function has finished executing. It is heavily used for data privacy and callbacks.

🔹 What is a Closure?

A closure is when:
An inner function remembers and can access variables from its outer function even after the outer function has finished executing.

🔹 Basic Example

```javascript
function outer() {
  let count = 0;
  function inner() {
    count++;
    console.log(count);
  }
  return inner;
}
const counter = outer();
counter(); // 1
counter(); // 2
counter(); // 3
```

👉 What’s happening?

outer() runs and finishes

But inner() still remembers count

That memory is called a closure

🔹 Why Closures Work

Even after outer() is done

JavaScript keeps variables in memory

Because inner() is still using them

👉 So the function + its remembered variables = closure

🔹 Real Use Case 1: Data Privacy 🔐

```javascript
function createBankAccount() {
  let balance = 0;
  return {
    deposit(amount) {
      balance += amount;
      console.log("Balance:", balance);
    },
    getBalance() {
      return balance;
    }
  };
}
const account = createBankAccount();
account.deposit(100); // Balance: 100
console.log(account.getBalance()); // 100
```

👉 balance is private

👉 You can’t access it directly, only via methods

🔹 Real Use Case 2: Callbacks & Async

```javascript
function fetchData() {
  let message = "Data received";
  setTimeout(function () {
    console.log(message);
  }, 1000);
}
fetchData(); // Data received (after 1 sec)
```

👉 Even after fetchData() finishes, message is remembered

🔹 Real Use Case 3: Function Factory

```javascript
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10
```

👉 x is remembered → closure in action

### ⚠️ Common Mistake (Loop Issue)

```javascript
for (var i = 1; i <= 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1000);
}
// Output: 4 4 4 ❌
```

👉 Fix using let:

```javascript
for (let i = 1; i <= 3; i++) {
  setTimeout(function () {
    console.log(i);
  }, 1000);
}
// Output: 1 2 3 ✅
```

### Closures are heavily used in:

1. React hooks

2. Event listeners

3. Async programming

4. Module patterns

* **Higher-Order Functions:** Master functions that accept other functions as arguments or return them, such as the built-in array methods **`.map()`, `.filter()`, and `.reduce()`**.

## 🔹 What is a Higher-Order Function?

1. **Takes another function as an argument**, OR

2. **Returns a function**

## 🔹 1. Function as Argument

```javascript
function greet(name) {
  return "Hello " + name;
}
function processUser(callback) {
  console.log(callback("John"));
}
processUser(greet); // Hello John
```

👉 `processUser` is a **higher-order function**

👉 `greet` is passed as a callback

## 🔹 2. Function Returning Another Function

```javascript
function multiplier(x) {
  return function (y) {
    return x * y;
  };
}
const double = multiplier(2);
console.log(double(5)); // 10
```

## 🔹 Built-in Higher-Order Functions (Very Important)

### ✅ 1. `.map()` → Transform each element

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6]
```

👉 Returns a **new array**

👉 Same length as original

---

### ✅ 2. `.filter()` → Select elements

```javascript
const numbers = [1, 2, 3, 4];
const even = numbers.filter(num => num % 2 === 0);
console.log(even); // [2, 4]
```

👉 Returns elements that match condition

---

### ✅ 3. `.reduce()` → Reduce to single value

```javascript
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 10
```

👉 Converts array → single value

👉 `acc` = accumulator

---

## 🔹 Key Differences

| Method      | Purpose   | Output       |
| ----------- | --------- | ------------ |
| `.map()`    | Transform | New array    |
| `.filter()` | Select    | New array    |
| `.reduce()` | Combine   | Single value |

---

## 🔹 Real-Life Example

```javascript
const users = [
  { name: "Amit", age: 20 },
  { name: "Neha", age: 25 },
  { name: "Rahul", age: 18 }
];
const result = users
  .filter(user => user.age > 18)
  .map(user => user.name);
console.log(result); // ["Amit", "Neha"]
```

---

## 🔹 Why Use Higher-Order Functions?

* ✅ Less code (clean & concise)

* ✅ More readable

* ✅ Reusable logic

* ✅ Functional programming style

---

## 💡 Pro Tip

Try to **avoid loops (`for`, `while`)** when possible and use:

* `.map()` for transformation

* `.filter()` for conditions

* `.reduce()` for calculations

---

# 5. Data Structures and Data Extraction

* **Objects and Arrays:** Go beyond the basics to understand how to iterate over them and manipulate them. Learn about newer collections like **Maps and Sets**, which offer advantages over plain objects for certain dictionary or unique-value use cases.

To really master JavaScript, you need to go beyond just creating **objects and arrays** and learn how to **iterate, transform, and choose the right data structure**—including **`Map`** and **`Set`**.

Let’s break it down clearly 👇

---

## 🔹 1. Iterating Over Arrays

### ✅ Common Methods

```javascript
const numbers = [1, 2, 3];
numbers.forEach(num => console.log(num)); //1
//2
//3
numbers.map(num => num * 2); //[ 2, 4, 6 ]
numbers.filter(num => num % 2 === 0); //[ 2 ]
numbers.reduce((acc, curr) => acc + curr, 0); // 6
```

👉 Arrays are best when:

* Order matters

* You need index-based access

---

## 🔹 2. Iterating Over Objects

### 2.1 ✅ Using `for...in`

```javascript
const user = { name: "Amit", age: 25 };
for (let key in user) {
  console.log(key, user[key]);
}
```

---

### 2.2 ✅ Using `Object.keys`, `values`, `entries`

```javascript
const user = { name: "Amit", age: 25 };
Object.keys(user).forEach(key => console.log(key));
Object.values(user).forEach(value => console.log(value));
Object.entries(user).forEach(([key, value]) => {
  console.log(key, value);
});
```

👉 Objects are best for:

* Key-value pairs

* Structured data

---

## 🔹 3. Manipulating Arrays & Objects

### 3.1 ✅ Array Operations

```javascript
const arr = [1, 2];
arr.push(3);   // add
arr.pop();     // remove last
arr.shift();   // remove first
arr.unshift(0);// add at start
```

### 3.2 ✅ Object Operations

```javascript
const obj = { name: "Amit" };
obj.age = 25;      // add
delete obj.name;   // delete
```

## 🔹 4. `Map` (Better than Object in some cases)

A **`Map`** is a collection of key-value pairs like objects, but more powerful.

```javascript
const map = new Map();
map.set("name", "Amit");
map.set(1, "number key");
console.log(map.get("name")); // Amit
```

## ✅ Why Use `Map` Instead of Object?

1. Keys can be **any type** (not just strings)

2. Maintains **insertion order**

3. Easier size checking (`map.size`)

```javascript
for (let [key, value] of map) {
  console.log(key, value);
}
```

## 🔹 5. `Set` (Unique Values Only)

A **`Set`** stores **only unique values**

```javascript
const set = new Set([1, 2, 2, 3]);
console.log(set); // {1, 2, 3}
```

## ✅ Common Uses of `Set`

### Remove duplicates

```javascript
const nums = [1, 2, 2, 3];
const unique = [...new Set(nums)];
console.log(unique); // [1, 2, 3]
```

### Check existence quickly

```javascript
const set = new Set([1, 2, 3]);
console.log(set.has(2)); // true
```

# 🔹 6. Quick Comparison

| Feature       | Object            | Map      | Set   |
| ------------- | ----------------  | -------- | -----   |
| Key-Value     | ✅ Yes            | ✅ Yes  | ❌ No   |
| Key Types     | String/Symbol     | Any type | N/A      |
| Order         | ❌ Not guaranteed | ✅ Yes  | ✅ Yes   |
| Unique Values | ❌ No             | ❌ No   | ✅ Yes   |
| Iteration     | Medium            | Easy     | Easy     |

# 🔹 When to Use What?

### ✅ Use Array:

* Ordered list

* Looping & transformations

### ✅ Use Object:

* Simple key-value storage

### ✅ Use Map:

* Dynamic keys (any type)

* Frequent additions/removals

### ✅ Use Set:

* Remove duplicates

* Track unique items

## ⚡ Simple Memory Trick

> 🔹 Array → "List"

> 🔹 Object → "Dictionary"

> 🔹 Map → "Advanced Dictionary"

> 🔹 Set → "Unique Collection"

* -----

* **Rest and Spread Operators (`...`):** Understand how to use `...` to spread elements of an array or object into a new one (useful for shallow copying and avoiding mutation), and how to use it to gather remaining arguments in function parameters.

# 🔹 1. Spread Operator (`...`) → "Expand values"

Used to **unpack elements** from arrays/objects.

## ✅ Array Spread

```javascript
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4];
console.log(arr2); // [1, 2, 3, 4]
```

👉 Copies and expands elements

## ✅ Copy Array (Avoid Mutation)

```javascript
const original = [1, 2, 3];
const copy = [...original];
copy.push(4);
console.log(original); // [1, 2, 3]
console.log(copy);     // [1, 2, 3, 4]
```

👉 Prevents modifying the original array

## ✅ Object Spread

```javascript
const user = { name: "Amit" };
const updatedUser = { ...user, age: 25 };
console.log(updatedUser); // { name: "Amit", age: 25 }
```

## ✅ Merge Objects

```javascript
const obj1 = { a: 1 };
const obj2 = { b: 2 };
const merged = { ...obj1, ...obj2 };
```

## ⚠️ Important Note

Spread creates a **shallow copy**:

```javascript
const obj = { nested: { a: 1 } };
const copy = { ...obj };
copy.nested.a = 99;
console.log(obj.nested.a); // 99 ❌ (still affected)
```

# 🔹 2. Rest Operator (`...`) → "Collect values"

Used to **gather remaining elements** into one variable.

## ✅ In Function Parameters

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}
console.log(sum(1, 2, 3, 4)); // 10
```

👉 Collects all arguments into an array

## ✅ In Array Destructuring

```javascript
const [first, ...rest] = [1, 2, 3, 4];
console.log(first); // 1
console.log(rest);  // [2, 3, 4]
```

## ✅ In Object Destructuring

```javascript
const user = { name: "Amit", age: 25, city: "Pune" };
const { name, ...others } = user;
console.log(name);   // Amit
console.log(others); // { age: 25, city: "Pune" }
```

# 🔹 Key Difference

| Feature  | Spread (`...`)  | Rest (`...`)             |
| -------- | --------------- | ------------------------ |
| Purpose  | Expand values   | Collect values           |
| Usage    | Arrays, Objects | Functions, Destructuring |
| Position | Right side      | Left side                |

# 🔹 Real-Life Use Cases

### ✅ Avoid Mutation (React, state updates)

```javascript
const newState = { ...state, updated: true };
```

### ✅ Flexible Function Inputs

```javascript
function logAll(...args) {
  console.log(args);
}
```

# ⚡ Easy Memory Trick

> 🔹 Spread = “Spread out”

> 🔹 Rest = “Rest together”

## 💡 Pro Tip

* Use **spread** for copying & merging

* Use **rest** for flexible arguments

* Always remember: **shallow copy only**

* -----

# 6. Asynchronous JavaScript (Crucial for Modern Web Apps)

* **The Event Loop and Call Stack:** Understand JavaScript's single-threaded nature and how the event loop processes asynchronous microtasks and macrotasks without blocking the main thread.

# 🔹 6.1. JavaScript is Single-Threaded

* Only **one task runs at a time**

* Uses a **Call Stack** to manage execution

👉 But still handles async tasks (like API calls, timers) using the **event loop**

# 🔹 6.2. Call Stack (Execution Context)

> Think of it as a **stack (LIFO)** — Last In, First Out

```javascript
function first() {
  console.log("First");
}
function second() {
  first();
  console.log("Second");
}
second();
```

### 👉 Flow:

1. `second()` pushed

2. `first()` pushed → executed → removed

3. back to `second()` → removed

# 🔹 6.3. Web APIs (Browser/Node Helpers)

Async operations don’t run in the stack:

* `setTimeout`

* `fetch`

* DOM events

👉 They go to **Web APIs**, then to queues

# 🔹 6.5. Two Queues (Very Important)

## 🔸 Microtask Queue (HIGH priority 🚀)

* `Promise.then()`

* `catch`, `finally`

* `queueMicrotask`

## 🔸 Macrotask Queue (Lower priority)

* `setTimeout`

* `setInterval`

* Events (click, etc.)

# 🔹 6.6. Execution Priority

👉 Order is always:

> **Call Stack → Microtasks → Macrotasks**

# 🔹 6.7. Classic Example

```javascript
console.log("Start");
setTimeout(() => {
  console.log("Timeout");
}, 0);
Promise.resolve().then(() => {
  console.log("Promise");
});
console.log("End");
```

## ✅ Output:

```javascript
Start
End
Promise
Timeout
```

## 🔍 Step-by-Side-Step:

1. `Start` → runs immediately

2. `setTimeout` → goes to **Web API → Macrotask queue**

3. `Promise.then` → goes to **Microtask queue**

4. `End` → runs

Now stack is empty 👇

1. Event loop picks **Microtask first** → `Promise`

2. Then **Macrotask** → `Timeout`

---

# 🔹 6.8. Important Rule

👉 After every task:

* Event loop **clears all microtasks first**

* Then takes **one macrotask**

---

# 🔹 6.9. Another Tricky Example

```javascript
setTimeout(() => console.log("A"), 0);
Promise.resolve().then(() => {
  console.log("B");
  return Promise.resolve();
}).then(() => {
  console.log("C");
});
console.log("D");
```

### ✅ Output:

```javascript
D
B
C
A
```

---

# 🔹 6.10. Why This Matters

* Prevents UI blocking

* Enables async behavior

* Helps debug timing issues

* Crucial for:

  * APIs (`fetch`)

  * React rendering

  * Performance optimization

# 🔹 Visual Flow

```javascript
Call Stack
↓
Is empty?
↓
Microtask Queue (ALL tasks)
↓
Macrotask Queue (ONE task)
```

# ⚡ Easy Memory Trick

> 🔹 “Stack → Microtask → Macrotask”

> 🔹 “Promises first, Timers later”

# 💡 Pro Tips

* `Promise` always runs before `setTimeout`

* Even `setTimeout(fn, 0)` is **NOT immediate**

* Too many microtasks can delay macrotasks

* **Promises:** Learn how to handle operations that take time (like fetching data) using Promises to avoid "callback hell".

# 🔹 1. What is a Promise?

> A **Promise** represents a value that will be available **now, later, or never**

### 📌 States of a Promise:

* **Pending** → still running

* **Fulfilled** → success (`resolve`)

* **Rejected** → error (`reject`)

# 🔹 2. Creating a Promise

```javascript
const myPromise = new Promise((resolve, reject) => {
  let success = true;
  if (success) {
    resolve("Data fetched");
  } else {
    reject("Error occurred");
  }
});
```

# 🔹 3. Consuming a Promise

```javascript
myPromise
  .then(result => console.log(result))   // success
  .catch(error => console.log(error))   // error
  .finally(() => console.log("Done"));  // always runs
```

# 🔹 4. Real Example (Async Task)

```javascript
function fetchData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("Data received");
    }, 1000);
  });
}
fetchData().then(data => console.log(data));
```

# 🔹 5. Callback Hell ❌

```javascript
setTimeout(() => {
  console.log("Step 1");
  setTimeout(() => {
    console.log("Step 2");
    setTimeout(() => {
      console.log("Step 3");
    }, 1000);
  }, 1000);
}, 1000);
```

👉 Hard to read, maintain, and debug

# 🔹 6. Promises Fix This ✅

```javascript
function step1() {
  return Promise.resolve("Step 1");
}
function step2() {
  return Promise.resolve("Step 2");
}
function step3() {
  return Promise.resolve("Step 3");
}
step1()
  .then(res => {
    console.log(res);
    return step2();
  })
  .then(res => {
    console.log(res);
    return step3();
  })
  .then(res => console.log(res));
```

👉 Clean and readable chaining

# 🔹 7. Error Handling

```javascript
fetchData()
  .then(data => {
    throw new Error("Something went wrong");
  })
  .catch(err => console.log(err.message));
```

👉 Errors automatically move to `.catch()`

# 🔹 8. Promise Chaining Rules

* `.then()` returns a new Promise

* You can chain multiple `.then()`

* Errors skip directly to `.catch()`

# 🔹 9. Promise Methods (Important)

### ✅ `Promise.all()`

```javascript
Promise.all([promise1, promise2])
  .then(results => console.log(results));
```

👉 Runs in parallel, fails if any fails

### ✅ `Promise.allSettled()`

```javascript
Promise.allSettled([promise1, promise2]);
```

👉 Waits for all (success or fail)

### ✅ `Promise.race()`

```javascript
Promise.race([promise1, promise2]);
```

👉 Returns first completed

# 🔹 10. Modern Alternative → `async/await`

```javascript
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.log(err);
  }
}
```

👉 Cleaner, looks like synchronous code

# 🔹 Why Promises Matter

* ✅ Avoid callback hell

* ✅ Better readability

* ✅ Built-in error handling

* ✅ Works perfectly with `async/await`

# ⚡ Simple Memory Trick

> 🔹 Promise = “I’ll give result later”

## 💡 Pro Tip

Best practice:

* Use `async/await` for readability

* Use `Promise.all()` for parallel tasks

* ** `async` and `await`:** Master this modern syntax that allows you to write asynchronous code that looks and behaves like synchronous top-to-bottom code.

**`async` / `await`** is modern JavaScript syntax built on top of Promises that lets you write **asynchronous code in a clean, top-to-bottom (synchronous-looking) way**.

# 🔹 1. What is `async`?

> An `async` function **always returns a Promise**

```javascript
async function greet() {
  return "Hello";
}
greet().then(console.log); // Hello
```

👉 Even though it returns a string, it’s wrapped in a Promise

# 🔹 2. What is `await`?

> `await` pauses execution **until a Promise resolves**

```javascript
function fetchData() {
  return new Promise(resolve => {
    setTimeout(() => resolve("Data received"), 1000);
  });
}
async function getData() {
  const data = await fetchData();
  console.log(data);
}
getData(); // Data received
```

👉 Code looks synchronous, but it’s async behind the scenes

# 🔹 3. Before vs After

### ❌ Using `.then()`

```javascript
fetchData()
  .then(data => {
    console.log(data);
  });
```

### ✅ Using `async/await`

```javascript
async function getData() {
  const data = await fetchData();
  console.log(data);
}
```

👉 Cleaner and easier to read

# 🔹 4. Error Handling (Very Important)

Use `try...catch`

```javascript
async function getData() {
  try {
    const data = await fetchData();
    console.log(data);
  } catch (err) {
    console.log("Error:", err);
  }
}
```

👉 Handles rejected promises

# 🔹 5. Multiple `await`

```javascript
async function process() {
  const a = await Promise.resolve(1);
  const b = await Promise.resolve(2);
  console.log(a + b); // 3
}
```

## ⚠️ Sequential vs Parallel

### ❌ Sequential (slow)

```javascript
const a = await fetchA();
const b = await fetchB();
```

### ✅ Parallel (fast)

```javascript
const [a, b] = await Promise.all([fetchA(), fetchB()]);
```

👉 Runs both at the same time 🚀

# 🔹 6. `await` Rules

* Only works inside **`async` functions**

* Pauses only that function (not whole program)

* Non-blocking for the event loop

# 🔹 7. Event Loop Connection

👉 Even with `await`:

* It uses **Promises internally**

* Goes to **microtask queue**

```javascript
console.log("Start");
async function test() {
  console.log("Inside");
  await Promise.resolve();
  console.log("After await");
}
test();
console.log("End");
```

### ✅ Output:

```javascript
Start
Inside
End
After await
```

# 🔹 8. Common Mistakes

### ❌ Forgetting `await`

```javascript
const data = fetchData(); // returns Promise, not actual data
```

### ❌ Using `await` outside async

```javascript
await fetchData(); // ❌ Error
```

# 🔹 9. Real-Life Example (API)

```javascript
async function getUsers() {
  try {
    const res = await fetch("https://api.example.com/users");
    const data = await res.json();
    console.log(data);
  } catch (err) {
    console.log("Failed:", err);
  }
}
```

# 🔹 Why Use `async/await`?

* ✅ Cleaner than `.then()`

* ✅ Easier error handling

* ✅ Better readability

* ✅ Looks synchronous but is async

---

# ⚡ Memory Trick

> 🔹 `async` = returns Promise

> 🔹 `await` = “wait here, but don’t block everything”

## 💡 Pro Tip

Best practice:

* Use `async/await` for readability

* Use `Promise.all()` for parallel tasks

---

# 7. Object-Oriented Programming (OOP)

* **Prototypes and Inheritance:** Understand **Prototypal Inheritance**, which is JavaScript's fundamental mechanism for sharing methods and data between objects.

# 🔹 1. What is a Prototype?

> Every JavaScript object has a hidden property called **`[[Prototype]]`** (accessible via `__proto__`)

👉 It links to another object from which it can **inherit properties and methods**

## ✅ Example

```javascript
const obj = {};
console.log(obj.__proto__); // Object prototype
```

👉 This links to:

```javascript
Object.prototype
```

# 🔹 2. Prototype Chain

> JavaScript looks for properties **up the chain**

```javascript
const user = {
  name: "Amit"
};
console.log(user.toString()); // works!
```

👉 Why?

* `toString()` is not in `user`

* It is found in `Object.prototype`

## 🔗 Chain Flow:

```javascript
user → Object.prototype → null
```

# 🔹 3. Prototypal Inheritance

> Objects can **inherit from other objects**

## ✅ Using `Object.create()`

```javascript
const animal = {
  speak() {
    console.log("Animal speaks");
  }
};
const dog = Object.create(animal);
dog.speak(); // Animal speaks
```

👉 `dog` inherits from `animal`

# 🔹 4. Constructor Functions + Prototype

Before classes, inheritance was done like this:

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.sayHello = function () {
  console.log("Hello " + this.name);
};
const p1 = new Person("Rahul");
p1.sayHello(); // Hello Rahul
```

👉 `sayHello` is shared via prototype (not duplicated)

# 🔹 5. Why Prototypes Matter

Without prototypes:

* Each object would have its own copy of methods ❌

* More memory usage

With prototypes:

* Methods are **shared** ✅

# 🔹 6. Modern Way → `class` (Syntax Sugar)

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHello() {
    console.log("Hello " + this.name);
  }
}
const p1 = new Person("Neha");
p1.sayHello();
```

👉 Under the hood → still uses **prototypes**

# 🔹 7. Inheritance with Classes

```javascript
class Animal {
  speak() {
    console.log("Animal speaks");
  }
}
class Dog extends Animal {}
const d = new Dog();
d.speak(); // Animal speaks
```

# 🔹 8. Key Concepts Summary

| Concept              | Meaning                |
| -------------------- | ---------------------- |
| Prototype            | Object to inherit from |
| Prototype Chain      | Lookup path            |
| Inheritance          | Sharing properties     |
| `Object.create()`    | Link objects           |
| `prototype` property | Used in constructors   |

# 🔹 9. Real-Life Analogy

> 👨 Parent → 👶 Child

> Child inherits traits from parent

```javascript
Dog → Animal → Object
```

# 🔹 10. Important Notes

* Everything in JS is based on **prototypes**

* Even arrays and functions use prototypes

* `class` is just a cleaner syntax (not a new system)

# ⚡ Memory Trick

> 🔹 “If not found, go up the chain”

## 💡 Pro Tip

Common interview focus:

* Difference between `__proto__` and `prototype`

* How method lookup works

* Constructor vs class

-----------------------

Great choice—this topic becomes **very clear with diagrams + tricky questions**. Let’s do both 👇

---

# 🔹 1. Prototype Chain (Diagram)

### ✅ Example Code

```javascript
const animal = {
  eats: true
};
const dog = Object.create(animal);
dog.barks = true;
```

### 🧠 Diagram

```javascript
dog
├── barks: true
└── [[Prototype]] ─────▶ animal
                              ├── eats: true
                              └── [[Prototype]] ───▶ Object.prototype
                                                      └── [[Prototype]] → null
```

### 👉 How Lookup Works

```javascript
console.log(dog.eats); // true
```

Steps:

1. Check `dog` → ❌ not found

2. Go to `animal` → ✅ found

3. Return value

# 🔹 2. Constructor Function Diagram

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  console.log("Hi " + this.name);
};
const p1 = new Person("Amit");
```

### 🧠 Diagram

```javascript
p1
├── name: "Amit"
└── [[Prototype]] ─────▶ Person.prototype
                           ├── sayHi()
                           └── [[Prototype]] ───▶ Object.prototype
```

### 👉 Key Idea

* `p1.__proto__ === Person.prototype` ✅

* Methods live in **prototype**, not inside object

# 🔹 3. Class (Same Under the Hood)

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    console.log("Hi " + this.name);
  }
}
```

👉 Internally:

```javascript
Person.prototype.sayHi = function() {}
```

* **ES6 Classes:** Learn the modern `class` syntax, how to use `constructor` functions, how to subclass using `extends` and `super()`, and how to implement **static methods and private fields** (using the `#` prefix).

# 🔹 1. Basic Class Syntax

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
}
const p1 = new Person("Amit", 25);
p1.greet(); // Hello, I'm Amit
```

👉 `constructor` runs when you create an object using `new`

# 🔹 2. Behind the Scenes (Important)

```javascript
console.log(typeof Person); // function
```

👉 Classes are just **syntactic sugar over prototypes**

# 🔹 3. Inheritance with `extends`

```javascript
class Animal {
  speak() {
    console.log("Animal speaks");
  }
}
class Dog extends Animal {}
const d = new Dog();
d.speak(); // Animal speaks
```

👉 `Dog` inherits from `Animal`

# 🔹 4. Using `super()`

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}
class Dog extends Animal {
  constructor(name, breed) {
    super(name); // call parent constructor
    this.breed = breed;
  }
}
const d = new Dog("Tommy", "Labrador");
console.log(d.name); // Tommy
```

👉 `super()` is required in child constructors

# 🔹 5. Method Overriding

```javascript
class Animal {
  speak() {
    console.log("Animal speaks");
  }
}
class Dog extends Animal {
  speak() {
    console.log("Dog barks");
  }
}
new Dog().speak(); // Dog barks
```

👉 Child method overrides parent

# 🔹 6. Static Methods

> Belong to the class, not instances

```javascript
class MathUtil {
  static add(a, b) {
    return a + b;
  }
}
console.log(MathUtil.add(2, 3)); // 5
```

👉 Cannot be called on object:

```javascript
const m = new MathUtil();
// m.add(2,3); ❌ Error
```

# 🔹 7. Private Fields (`#`)

> Truly private variables (ES2022+)

```javascript
class BankAccount {
  #balance = 0;
  deposit(amount) {
    this.#balance += amount;
  }
  getBalance() {
    return this.#balance;
  }
}
const acc = new BankAccount();
acc.deposit(100);
console.log(acc.getBalance()); // 100
```

## ❌ Not Accessible Outside

```javascript
console.log(acc.#balance); // ❌ Error
```

👉 Ensures **data privacy**

# 🔹 8. Getters & Setters

```javascript
class User {
  constructor(name) {
    this._name = name;
  }
  get name() {
    return this._name;
  }
  set name(value) {
    this._name = value;
  }
}
const u = new User("Rahul");
console.log(u.name); // Rahul
```

# 🔹 9. Full Example

```javascript
class Vehicle {
  constructor(type) {
    this.type = type;
  }
  start() {
    console.log(`${this.type} started`);
  }
}
class Car extends Vehicle {
  #speed = 0;
  constructor(type, brand) {
    super(type);
    this.brand = brand;
  }
  accelerate() {
    this.#speed += 10;
    console.log(this.#speed);
  }
  static info() {
    console.log("Cars are vehicles");
  }
}
const car = new Car("Car", "Toyota");
car.start();
car.accelerate();
Car.info();
```

# 🔹 10. Key Concepts Summary

| Feature       | Purpose             |
| ------------- | ------------------- |
| `class`       | Blueprint           |
| `constructor` | Initialize object   |
| `extends`     | Inherit             |
| `super()`     | Call parent         |
| `static`      | Class-level methods |
| `#private`    | Hidden data         |

# ⚡ Memory Trick

> 🔹 Class = Blueprint

> 🔹 `extends` = inherit

> 🔹 `super()` = parent call

> 🔹 `#` = private

# 8. Modularity

* **ES Modules (ESM):** Understand how to split your code into multiple files using **`import` and `export`** statements. You should know the difference between named exports, namespace imports, and default exports.

# 🔹 1. Why Use Modules?

* ✅ Better code organization

* ✅ Reusability

* ✅ Maintainability

* ✅ Avoid global scope pollution

# 🔹 2. Named Exports

> Export multiple values by name

### 📄 `math.js`

```javascript
export const add = (a, b) => a + b;
export const sub = (a, b) => a - b;
```

### 📄 `app.js`

```javascript
import { add, sub } from "./math.js";
console.log(add(2, 3)); // 5
```

## ✅ Rename While Importing

```javascript
import { add as sum } from "./math.js";
```

# 🔹 3. Default Export

> Only **one default export per file**

### 📄 `greet.js`

```javascript
export default function greet() {
  console.log("Hello");
}
```

### 📄 `app.js`

```javascript
import greet from "./greet.js";
greet();
```

👉 No `{}` needed

# 🔹 4. Named + Default Together

```javascript
// file.js
export const x = 10;
export default function hello() {}
```

```javascript
// app.js
import hello, { x } from "./file.js";
```

# 🔹 5. Namespace Import (`*`)

> Import everything as an object

```javascript
import * as math from "./math.js";
console.log(math.add(2, 3));
console.log(math.sub(5, 2));
```

👉 Access via `math.methodName`

# 🔹 6. Export All

```javascript
export * from "./math.js";
```

👉 Re-export everything

# 🔹 7. Important Rules

* File must use:

  * `"type": "module"` in `package.json` OR

  * `.mjs` extension

* Use **relative paths (`./`)**

* Modules are **strict mode by default**

# 🔹 8. Common Mistakes

### ❌ Wrong Import

```javascript
import add from "./math.js"; // ❌ if it's named export
```

### ❌ Missing Extension

```javascript
import { add } from "./math"; // ❌ in browser
```

# 🔹 9. Quick Comparison

| Type             | Syntax            | Notes             |
| ---------------- | ----------------- | ----------------- |
| Named Export     | `export const x`  | Multiple allowed  |
| Default Export   | `export default`  | One per file      |
| Namespace Import | `import * as obj` | All in one object |

# 🔹 10. Real-Life Example Structure

```

project/

├── utils.js

├── api.js

└── app.js

```

👉 Helps scale large apps

# ⚡ Memory Trick

> 🔹 Named = `{}`

> 🔹 Default = no `{}`

> 🔹 `*` = everything

## 💡 Pro Tip

* Prefer **named exports** in large projects (better autocomplete & refactoring)

* Use **default export** for main functionality of a file

# 9. Browser Interaction (The DOM)

* **DOM Manipulation** is how JavaScript interacts with your web page—letting you **select, create, update, and delete HTML elements dynamically**.

# 🔹 1. What is the DOM?

> The **DOM (Document Object Model)** represents your HTML as a **tree of objects**

```html
<html>
├── <body>
      ├── <h1>
      └── <p>
```

👉 JavaScript can read & modify this structure

# 🔹 2. Selecting Elements

### ✅ Common Methods

```javascript
// By ID
const title = document.getElementById("title");
// By class
const items = document.getElementsByClassName("item");
// By tag
const divs = document.getElementsByTagName("div");
// Modern (recommended)
const el = document.querySelector(".item");
const all = document.querySelectorAll(".item");
```

👉 `querySelector` = first match

👉 `querySelectorAll` = all matches (NodeList)

# 🔹 3. Modifying Content

```javascript
const el = document.querySelector("#title");
el.textContent = "New Title";
el.innerHTML = "<span>Styled Title</span>";
```

👉 `textContent` → safe text

👉 `innerHTML` → allows HTML (be careful ⚠️)

# 🔹 4. Changing Styles

```javascript
el.style.color = "red";
el.style.fontSize = "20px";
```

## ✅ Better Approach → Classes

```javascript
el.classList.add("active");
el.classList.remove("active");
el.classList.toggle("active");
```

# 🔹 5. Creating Elements

```javascript
const newDiv = document.createElement("div");
newDiv.textContent = "Hello World";
```

# 🔹 6. Adding to DOM

```javascript
document.body.appendChild(newDiv);
```

## ✅ Modern Methods

```javascript
parent.append(newDiv);
parent.prepend(newDiv);
parent.before(newDiv);
parent.after(newDiv);
```

# 🔹 7. Removing Elements

```javascript
el.remove();
```

# 🔹 8. Event Handling (Very Important)

```javascript
const btn = document.querySelector("button");
btn.addEventListener("click", () => {
  console.log("Button clicked!");
});
```

# 🔹 9. Real Example

```javascript
const list = document.querySelector("#list");
const btn = document.querySelector("#addBtn");
btn.addEventListener("click", () => {
  const li = document.createElement("li");
  li.textContent = "New Item";
  list.append(li);
});
```

👉 Adds items dynamically

# 🔹 10. Traversing DOM

```javascript
el.parentElement
el.children
el.firstElementChild
el.nextElementSibling
```

# 🔹 11. Best Practices

* ✅ Use `querySelector` (modern)

* ✅ Prefer `textContent` over `innerHTML`

* ✅ Use `classList` for styling

* ✅ Avoid too many DOM updates (performance)

* **Events:** Understand how to listen for user interactions (clicks, keyboard input) and grasp concepts like **event bubbling, capturing, and delegation**.

# **Events in JavaScript (User Interactions)**

Events are actions that happen in the browser, usually triggered by the user.

### Common user interaction events:

| Event Type               | Description                          |
|--------------------------|--------------------------------------|
| **click**                | User clicks a button/div/link        |
| **dblclick**             | Double click                         |
| **keydown**              | Key is pressed                       |
| **keyup**                | Key is released                      |
| **input**                | Typing inside an input field         |
| **submit**                | Form is submitted                    |
| **mouseover / mouseout** | Mouse enters/leaves element          |

# **1) Listening for Events**

You can listen to events using **`addEventListener()`**.

## Example: Click event

```javascript
const button = document.getElementById("myBtn");
button.addEventListener("click", function () {
  console.log("Button clicked!");
});
```

## Example: Keyboard event

```javascript
document.addEventListener("keydown", function (event) {
  console.log("Key pressed:", event.key);
});
```

* `event.key` tells which key was pressed (`a`, `Enter`, `Escape`, etc.)

---

# **2) The Event Object**

Whenever an event happens, JavaScript gives you an **event object**.

```javascript
button.addEventListener("click", function (event) {
  console.log(event);
});
```

Useful properties:

* `event.target` → the actual element clicked

* `event.currentTarget` → the element the listener is attached to

* `event.key` → pressed key (for keyboard events)

* `event.type` → type of event (`click`, `keydown`, etc.)

# **3) Event Bubbling**

**Bubbling** means:

When you click a child element, the event first happens on the child, then it **bubbles up** to its parent, then grandparent, and so on.

## Example:

```html
<div id="parent">
  <button id="child">Click Me</button>
</div>
```

```javascript
document.getElementById("child").addEventListener("click", () => {
  console.log("Child clicked");
});
document.getElementById("parent").addEventListener("click", () => {
  console.log("Parent clicked");
});
```

### If you click the button:

Output:

```javascript
Child clicked
Parent clicked
```

✅ Event travels **upward** = **bubbling**

# **4) Event Capturing (Trickling)**

**Capturing** is the opposite of bubbling.

The event goes:

* from **outer parent → inner child**

By default, event listeners use **bubbling phase**.

To use **capturing**, pass `true` as the 3rd argument:

```javascript
document.getElementById("parent").addEventListener("click", () => {
  console.log("Parent capture");
}, true);
document.getElementById("child").addEventListener("click", () => {
  console.log("Child clicked");
});
```

### Clicking the button:

Output:

```javascript
Parent capture
Child clicked
```

✅ Event travels **downward first** = **capturing**

# **5) Event Flow (Important Interview Concept)**

An event generally has **3 phases**:

1. **Capturing phase** → top to target

2. **Target phase** → event reaches clicked element

3. **Bubbling phase** → target back up

So if you click a button inside a div:

**Window → Document → HTML → Body → Div → Button → Div → Body → HTML → Document → Window**

# **6) Stop Bubbling / Stop Propagation**

If you don’t want the event to continue to parent elements:

```javascript
document.getElementById("child").addEventListener("click", (event) => {
  event.stopPropagation();
  console.log("Child only");
});
```

Now clicking child will **not trigger parent click handler**.

# **7) Event Delegation (Very Important)**

**Event delegation** means:

Instead of adding event listeners to many child elements,

you add **one listener to the parent** and use bubbling to detect which child was clicked.

This is:

* more efficient

* useful for dynamic elements added later

## Example:

```html
<ul id="list">
  <li>Item 1</li>
  <li>Item 2</li>
  <li>Item 3</li>
</ul>
```

```javascript
document.getElementById("list").addEventListener("click", function (event) {
  if (event.target.tagName === "LI") {
    console.log("Clicked:", event.target.textContent);
  }
});
```

### Why use delegation?

Instead of:

```javascript
// Bad for many items
document.querySelectorAll("li").forEach(li => {
  li.addEventListener("click", ...);
});
```

Use:

```javascript
// Better: one listener on parent
list.addEventListener("click", ...);
```

# **8) Real Benefit of Event Delegation with Dynamic Elements**

If new list items are added later:

```javascript
const newItem = document.createElement("li");
newItem.textContent = "Item 4";
document.getElementById("list").appendChild(newItem);
```

You **do not need to attach a new listener**.

The parent listener still works.

✅ This is why delegation is very useful.

# **9) Quick Comparison**

## **Bubbling**

* Event moves **child → parent**

* Default behavior

## **Capturing**

* Event moves **parent → child**

* Enable with `true` in `addEventListener`

## **Delegation**

* Put one listener on **parent**

* Use `event.target` to handle child actions

* Works because of **bubbling**

# **10) Best Practice Example**

```javascript
document.addEventListener("click", function (event) {
  if (event.target.matches(".delete-btn")) {
    console.log("Delete button clicked");
  }
});
```

This is powerful for:

* buttons in tables

* dynamically rendered lists

* menus

* Salesforce LWC / Aura DOM interaction concepts too (similar event flow thinking)

# **11) Easy Memory Trick**

* **Bubbling** = **inside → outside**

* **Capturing** = **outside → inside**

* **Delegation** = **listen on parent, handle child**

# **12) Interview-Ready Short Answer**

> In JavaScript, events are user or browser actions like clicks or key presses. We listen to them using `addEventListener()`. By default, events follow **bubbling**, where the event moves from the target element up to its ancestors. In **capturing**, the event moves from ancestors down to the target. **Event delegation** uses bubbling by attaching a single listener to a parent element and checking `event.target`, which improves performance and supports dynamically added elements.

* **Browser Storage:** Learn how to persist data on the client side using Local Storage and Session Storage.

Here’s a clear and simple explanation of **Browser Storage (Local Storage & Session Storage)** 👇

## 📦 What is Browser Storage?

Browser storage lets you **save data directly in the user’s browser** so it can be used later without needing a server.

Common types:

* **Local Storage**

* **Session Storage**

Both are part of the **Web Storage API**

## 🟢 Local Storage

Stores data **permanently (until manually cleared)**

### 🔹 Key Features:

* Data persists even after closing the browser

* Shared across tabs of the same site

* Storage limit ~5–10MB

### 🔹 Example:

```javascript
// Store data
localStorage.setItem("username", "Salaka");
// Get data
let user = localStorage.getItem("username");
// Remove data
localStorage.removeItem("username");
// Clear all
localStorage.clear();
```

### ✅ Use Cases:

* Remember login preferences

* Save theme (dark/light mode)

* Store user settings

## 🟡 Session Storage

Stores data **only for one browser session**

### 🔹 Key Features:

* Data is deleted when tab is closed

* Not shared across tabs

* Same storage limit as localStorage

### 🔹 Example:

```javascript
// Store data
sessionStorage.setItem("sessionUser", "Salaka");
// Get data
let user = sessionStorage.getItem("sessionUser");
// Remove data
sessionStorage.removeItem("sessionUser");
```

### ✅ Use Cases:

* Temporary form data

* OTP/session-related info

* One-time user actions

## ⚡ Key Differences

| Feature     | Local Storage          | Session Storage  |
| ----------- | ---------------------- | ---------------- |
| Lifetime    | Permanent              | Until tab closes |
| Scope       | All tabs (same origin) | Single tab       |
| Persistence | Yes                    | No               |
| Use Case    | Long-term data         | Temporary data   |

## ⚠️ Important Notes

* Data is stored as **strings only**

* Not secure → ❌ don’t store passwords or sensitive data

* Same-origin policy applies (only your site can access it)

## 💡 Pro Tip

If you want to store objects:

```javascript
let user = { name: "Salaka", age: 30 };
// Store
localStorage.setItem("user", JSON.stringify(user));
// Retrieve
let data = JSON.parse(localStorage.getItem("user"));
```
