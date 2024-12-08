# **JavaScript Paradigms Overview**

A **paradigm** is a style or way of writing code. JavaScript is a multi-paradigm language, meaning it supports different styles of programming. Understanding paradigms helps you write better, more organized, and maintainable code.

---

### **Main Programming Paradigms in JavaScript**

1.  #### Imperative Programming:
    - Focuses on how things should happen.
    - You write step-by-step instructions to tell the computer what to do.
    - Example:

```javascript
let sum = 0;
for (let i = 1; i <= 5; i++) {
  sum += i;
}
console.log(sum); // Output: 15
```

---

2.  #### Declarative Programming:
    - Focuses on what you want to achieve, not how to do it.You don't need to write step by step instructions to execute the code.
    - You describe the logic and let JavaScript handle the underlying steps.
    - Example:

```javascript
const numbers = [1, 2, 3, 4, 5];
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // Output: 15
```

---

3. #### Object-Oriented Programming (OOP):
   - Organizes code into objects that contain data (properties) and behavior (methods).
   - Example:

```javascript
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
  start() {
    console.log(`${this.make} ${this.model} is starting...`);
  }
}
const myCar = new Car("Toyota", "Corolla");
myCar.start(); // Output: Toyota Corolla is starting...
```

---

4. #### Functional Programming (FP):
   - Focuses on writing pure functions that avoid changing state and side effects.
   - JavaScript supports FP through functions like map, reduce, and filter.

---

## Deep Dive into Functional Programming (FP)

**Functional programming (FP)** treats computation as the evaluation of mathematical functions. It emphasizes immutability, avoiding side effects, and using higher-order functions.

---

### Key Concepts of Functional Programming

1. #### Pure Functions:
   - A function is pure if:
     - It always returns the same output for the same input.
     - It has no side effects (it doesn't modify external state).
   - Example :

```javascript
function add(a, b) {
  return a + b; // Pure: No external effect, just returns sum.
}
```

---

2. #### Immutability
   - In functional programming, original data should not be modified. Instead, new copies of data are created.
   - Example :

```javascript
const numbers = [1, 2, 3];
const newNumbers = numbers.map((num) => num * 2);
console.log(newNumbers); // [2, 4, 6]
console.log(numbers); // [1, 2, 3] (original array unchanged)
```

---

3. #### Higher-Order Functions (HOFs):
   - Functions that either:
     - Take other functions as arguments.
     - Return a function as a result.
     - Example :

```javascript
function greet(name) {
  return function (message) {
    return `${message}, ${name}!`;
  };
}
const greetJohn = greet("John");
console.log(greetJohn("Hello")); // Output: Hello, John!
```

---

4. #### First-Class Functions:
   - Functions in JavaScript are first-class citizens, meaning:
     - They can be assigned to variables.
     - They can be passed as arguments.
     - They can be returned from other functions.
     - Example :

```javascript
const sayHello = () => console.log("Hello!");
const executeFunction = (fn) => fn();
executeFunction(sayHello); // Output: Hello!
```

---

5. #### Function Composition:
   - Combining multiple functions to create a new function.
   - Example :

```javascript
const multiplyBy2 = (x) => x * 2;
const add3 = (x) => x + 3;
const combined = (x) => add3(multiplyBy2(x));
console.log(combined(5)); // Output: 13
```

---

6. #### Avoiding Side Effects:
   - Functions should not change variables or objects outside their scope.
   - Side effects make debugging harder and reduce code predictability.
   - Example of side effect (to avoid)!!!:

```javascript
let count = 0;
function increment() {
  count++; // Modifies external variable (side effect)
}
```

---

## Key Methods in JavaScript Functional Programming

1. #### map():
   - Creates a new array by applying a function to each element of the original array.
   - The original array doesn't change and shouldn't be changed to avoid side effects.
   - Example:

```javascript
const numbers = [1, 2, 3];
const doubled = numbers.map((num) => num * 2);
console.log(doubled); // [2, 4, 6]
console.log(numbers); // [1, 2, 3]
```

---

2. #### filter():
   - Creates a new array with elements that pass a certain condition.
   - The original array doesn't change and shouldn't be changed to avoid side effects.
   - Example :

```javascript
const numbers = [1, 2, 3, 4];
const evens = numbers.filter((num) => num % 2 === 0);
console.log(evens); // [2, 4]
console.log(numbers); // [1, 2, 3, 4]
```

---

3. #### reduce():
   - Reduces an array to a single value(sum total) by applying a function repeatedly.
   - Example :

```javascript
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 10
console.log(numbers) // [1, 2, 3, 4]
```

---

4. #### forEach():
   - Executes a function on each element of an array.
   - Example :

```javascript
const numbers = [1, 2, 3];
numbers.forEach((num) => console.log(num));
// Output : 1 
         // 2
         // 3
```

---

## Advantages of Functional Programming in JavaScript

- Predictable and Easier to Debug: Pure functions make debugging easier since you know the function’s output depends only on its inputs.
- Reusability: Functions can be reused in different parts of your code.
- Modularity: Code is broken into smaller, manageable pieces.
- Concurrency: FP makes it easier to write concurrent code since it avoids shared state.

---

## Summary:

- JavaScript supports multiple paradigms, including imperative, declarative, object-oriented, and functional programming.
- Functional programming focuses on pure functions, immutability, and avoiding side effects.
- Important concepts: pure functions, immutability, higher-order functions, function composition, and avoiding side effects.
- Use built-in methods like map(), filter(), and reduce() to write cleaner, functional code.
