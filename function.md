# 9 Ways to Declare JavaScript Functions - Complete Guide

## Table of Contents
1. [Function Declaration](#1-function-declaration)
2. [Function Expression](#2-function-expression)
3. [Arrow Function](#3-arrow-function)
4. [Function Constructor](#4-function-constructor)
5. [IIFE (Immediately Invoked Function Expression)](#5-iife-immediately-invoked-function-expression)
6. [Generator Function](#6-generator-function)
7. [Anonymous Function](#7-anonymous-function)
8. [Recursive Function](#8-recursive-function)
9. [Higher-Order Function](#9-higher-order-function)

---

## 1. Function Declaration

### What is it?
A function declaration defines a named function using the `function` keyword. It's the most traditional and straightforward way to create functions in JavaScript.

### Key Characteristics:
- **Hoisted**: Can be called before it's defined in the code
- **Named**: Must have a function name
- **Creates its own `this` context**

### Syntax:
```javascript
function functionName(parameters) {
  // function body
  return value;
}
```

### Examples:

#### Basic Example:
```javascript
// Function declaration
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet('Alice'));
// Output: Hello, Alice!

console.log(greet('Bob'));
// Output: Hello, Bob!
```

**Note:** The function is declared with a name (`greet`) and can be called anywhere in the scope, even before its declaration.

#### Hoisting Example:
```javascript
// Calling function before declaration - this works!
console.log(add(5, 3));
// Output: 8

function add(a, b) {
  return a + b;
}

console.log(add(10, 20));
// Output: 30
```

**Note:** Function declarations are **hoisted**, meaning JavaScript moves them to the top of their scope during compilation. You can call them before they appear in your code.

#### Real-World Example:
```javascript
function calculateDiscount(price, discountPercent) {
  const discount = (price * discountPercent) / 100;
  const finalPrice = price - discount;
  return finalPrice;
}

console.log(calculateDiscount(100, 20));
// Output: 80

console.log(calculateDiscount(500, 15));
// Output: 425
```

**Note:** Function declarations are ideal for utility functions that you'll use throughout your code.

---

## 2. Function Expression

### What is it?
A function expression defines a function as part of a larger expression, typically by assigning it to a variable. The function can be named or anonymous.

### Key Characteristics:
- **NOT hoisted**: Cannot be called before definition
- **Can be named or anonymous**
- **Often assigned to variables**
- **Can be passed as arguments**

### Syntax:
```javascript
const functionName = function(parameters) {
  // function body
  return value;
};
```

### Examples:

#### Basic Example:
```javascript
// Function expression
const multiply = function(a, b) {
  return a * b;
};

console.log(multiply(4, 5));
// Output: 20

console.log(multiply(7, 3));
// Output: 21
```

**Note:** The function is assigned to a variable. You must define it before calling it.

#### NOT Hoisted:
```javascript
// This will cause an error!
// console.log(subtract(10, 5));
// Output: ReferenceError: Cannot access 'subtract' before initialization

const subtract = function(a, b) {
  return a - b;
};

console.log(subtract(10, 5));
// Output: 5
```

**Note:** Unlike function declarations, function expressions are **not hoisted**. You'll get an error if you try to call them before definition.

#### Named Function Expression:
```javascript
// Named function expression (useful for recursion and debugging)
const factorial = function fact(n) {
  if (n <= 1) return 1;
  return n * fact(n - 1); // Can reference itself by name
};

console.log(factorial(5));
// Output: 120 (5 * 4 * 3 * 2 * 1)

console.log(factorial(3));
// Output: 6 (3 * 2 * 1)
```

**Note:** You can give function expressions names. This is useful for recursion and makes debugging easier (the name appears in stack traces).

#### Function as Argument:
```javascript
// Passing function expression as callback
const numbers = [1, 2, 3, 4, 5];

const double = function(num) {
  return num * 2;
};

const doubled = numbers.map(double);
console.log(doubled);
// Output: [2, 4, 6, 8, 10]
```

**Note:** Function expressions can be passed as arguments to other functions, making them perfect for callbacks.

---

## 3. Arrow Function

### What is it?
Arrow functions are a concise syntax for writing function expressions, introduced in ES6. They have a shorter syntax and lexically bind the `this` value.

### Key Characteristics:
- **Concise syntax**
- **Lexically binds `this`** (inherits from parent scope)
- **No `arguments` object**
- **Cannot be used as constructors**
- **Implicit return** for single expressions

### Syntax:
```javascript
const functionName = (parameters) => {
  // function body
  return value;
};

// Implicit return (one-liner)
const functionName = (parameters) => value;
```

### Examples:

#### Basic Example:
```javascript
// Traditional function expression
const addOld = function(a, b) {
  return a + b;
};

// Arrow function equivalent
const add = (a, b) => a + b;

console.log(add(5, 3));
// Output: 8

console.log(add(12, 8));
// Output: 20
```

**Note:** Arrow functions provide a much cleaner syntax for simple operations.

#### Different Syntax Variations:
```javascript
// No parameters
const greet = () => 'Hello World!';
console.log(greet());
// Output: Hello World!

// Single parameter (parentheses optional)
const square = x => x * x;
console.log(square(5));
// Output: 25

// Multiple parameters
const multiply = (a, b) => a * b;
console.log(multiply(3, 4));
// Output: 12

// Multiple lines with explicit return
const calculateArea = (length, width) => {
  const area = length * width;
  console.log(`Calculating area: ${length} x ${width}`);
  return area;
};
console.log(calculateArea(5, 10));
// Output: 
// Calculating area: 5 x 10
// 50
```

**Note:** Arrow functions have flexible syntax based on the number of parameters and complexity of the function body.

#### Lexical `this` Binding:
```javascript
// Arrow function inherits 'this' from parent scope
const person = {
  name: 'Alice',
  hobbies: ['reading', 'coding', 'gaming'],
  
  // Regular function for method
  showHobbies: function() {
    console.log(`${this.name}'s hobbies:`);
    
    // Arrow function inherits 'this' from showHobbies
    this.hobbies.forEach(hobby => {
      console.log(`${this.name} likes ${hobby}`);
    });
  }
};

person.showHobbies();
// Output:
// Alice's hobbies:
// Alice likes reading
// Alice likes coding
// Alice likes gaming
```

**Note:** Arrow functions don't have their own `this`. They inherit it from the enclosing scope, which is perfect for callbacks.

#### Array Operations:
```javascript
const numbers = [1, 2, 3, 4, 5];

// Map
const doubled = numbers.map(n => n * 2);
console.log('Doubled:', doubled);
// Output: Doubled: [2, 4, 6, 8, 10]

// Filter
const evens = numbers.filter(n => n % 2 === 0);
console.log('Evens:', evens);
// Output: Evens: [2, 4]

// Reduce
const sum = numbers.reduce((acc, n) => acc + n, 0);
console.log('Sum:', sum);
// Output: Sum: 15
```

**Note:** Arrow functions make array operations much more readable and concise.

---

## 4. Function Constructor

### What is it?
The Function Constructor creates functions dynamically at runtime using the `new Function()` syntax. It takes string arguments that become the function's parameters and body.

### Key Characteristics:
- **Creates functions from strings**
- **Evaluated at runtime**
- **Always creates functions in global scope**
- **Less performant** (no optimization)
- **Security risk** (similar to `eval()`)
- **Rarely used in modern code**

### Syntax:
```javascript
const functionName = new Function('param1', 'param2', 'function body as string');
```

### Examples:

#### Basic Example:
```javascript
// Creating a function using Function constructor
const add = new Function('a', 'b', 'return a + b');

console.log(add(5, 3));
// Output: 8

console.log(add(10, 20));
// Output: 30
```

**Note:** The last argument is the function body, and all previous arguments are parameter names (as strings).

#### Multiple Statements:
```javascript
// Function with multiple statements
const greet = new Function('name', `
  const message = 'Hello, ' + name + '!';
  console.log(message);
  return message;
`);

const result = greet('Alice');
// Output: Hello, Alice!
console.log('Returned:', result);
// Output: Returned: Hello, Alice!
```

**Note:** You can include multiple statements in the function body string, but this becomes hard to read and maintain.

#### Dynamic Function Creation:
```javascript
// Creating functions dynamically based on user input
function createMathOperation(operation) {
  if (operation === 'add') {
    return new Function('a', 'b', 'return a + b');
  } else if (operation === 'multiply') {
    return new Function('a', 'b', 'return a * b');
  }
}

const addFunc = createMathOperation('add');
console.log(addFunc(5, 3));
// Output: 8

const multiplyFunc = createMathOperation('multiply');
console.log(multiplyFunc(4, 7));
// Output: 28
```

**Note:** Function constructors can create functions dynamically, but this is rarely needed and poses security risks.

#### ⚠️ Security Warning:
```javascript
// DANGEROUS: Never use with user input!
const userInput = "return 1"; // Safe
// const userInput = "alert('hacked'); return 1"; // Malicious!

const dangerousFunc = new Function(userInput);
console.log(dangerousFunc());
// Output: 1
```

**Note:** Function constructors execute arbitrary code strings, making them vulnerable to code injection attacks. **Avoid using them with user input!**

#### Why NOT to Use:
```javascript
// ❌ Bad: Function constructor
const subtract1 = new Function('a', 'b', 'return a - b');

// ✅ Good: Regular function or arrow function
const subtract2 = (a, b) => a - b;

console.log(subtract1(10, 5));
// Output: 5
console.log(subtract2(10, 5));
// Output: 5

// Same result, but subtract2 is:
// - More readable
// - More performant
// - More secure
// - Easier to debug
```

**Note:** In modern JavaScript, you should almost never use Function constructors. Use regular functions or arrow functions instead.

---

## 5. IIFE (Immediately Invoked Function Expression)

### What is it?
An IIFE is a function that runs immediately after it's defined. It's commonly used to create a private scope and avoid polluting the global namespace.

### Key Characteristics:
- **Executes immediately** after definition
- **Creates private scope** (encapsulation)
- **Prevents global namespace pollution**
- **Can return values**
- **Used for initialization code**

### Syntax:
```javascript
(function() {
  // function body
})();

// Or with arrow function
(() => {
  // function body
})();
```

### Examples:

#### Basic Example:
```javascript
// IIFE with regular function
(function() {
  console.log('This runs immediately!');
})();
// Output: This runs immediately!

// IIFE with arrow function
(() => {
  console.log('This also runs immediately!');
})();
// Output: This also runs immediately!
```

**Note:** The function is wrapped in parentheses and immediately invoked with `()` at the end.

#### With Parameters:
```javascript
// IIFE with parameters
(function(name, age) {
  console.log(`Name: ${name}, Age: ${age}`);
})('Alice', 25);
// Output: Name: Alice, Age: 25

// Arrow function IIFE with parameters
((x, y) => {
  const sum = x + y;
  console.log(`${x} + ${y} = ${sum}`);
})(10, 20);
// Output: 10 + 20 = 30
```

**Note:** You can pass arguments to IIFEs just like regular functions.

#### Creating Private Scope:
```javascript
// Variables inside IIFE are private
(function() {
  const privateVar = 'I am private!';
  const privateFunc = () => console.log(privateVar);
  
  privateFunc();
})();
// Output: I am private!

// These will cause errors (variables are not accessible outside)
// console.log(privateVar); // ReferenceError
// privateFunc(); // ReferenceError
```

**Note:** IIFEs create a private scope. Variables declared inside are not accessible from outside, preventing global namespace pollution.

#### Returning Values (Module Pattern):
```javascript
// IIFE returning an object (Module Pattern)
const calculator = (function() {
  // Private variables
  let result = 0;
  
  // Private function
  const log = (operation, value) => {
    console.log(`${operation}: result is now ${value}`);
  };
  
  // Public API
  return {
    add: function(num) {
      result += num;
      log('Add', result);
      return this; // Enable chaining
    },
    subtract: function(num) {
      result -= num;
      log('Subtract', result);
      return this;
    },
    multiply: function(num) {
      result *= num;
      log('Multiply', result);
      return this;
    },
    getResult: function() {
      return result;
    },
    reset: function() {
      result = 0;
      log('Reset', result);
      return this;
    }
  };
})();

// Using the module
calculator.add(10).multiply(2).subtract(5);
// Output:
// Add: result is now 10
// Multiply: result is now 20
// Subtract: result is now 15

console.log('Final result:', calculator.getResult());
// Output: Final result: 15

calculator.reset();
// Output: Reset: result is now 0
```

**Note:** This is the Module Pattern - using an IIFE to create private variables and expose only specific methods. Very useful for encapsulation.

#### Avoiding Global Pollution:
```javascript
// Without IIFE - pollutes global scope
var name = 'Global';
var age = 100;
console.log('Before IIFE:', name, age);
// Output: Before IIFE: Global 100

// With IIFE - doesn't pollute global scope
(function() {
  var name = 'Local';
  var age = 25;
  console.log('Inside IIFE:', name, age);
})();
// Output: Inside IIFE: Local 25

console.log('After IIFE:', name, age);
// Output: After IIFE: Global 100
```

**Note:** IIFEs prevent variable name conflicts by creating isolated scopes.

#### Real-World Use Case - Configuration:
```javascript
const app = (function() {
  // Private configuration
  const config = {
    apiUrl: 'https://api.example.com',
    apiKey: 'secret-key-12345',
    timeout: 5000
  };
  
  // Private helper
  const validateConfig = () => {
    console.log('Configuration validated');
    return config.apiKey && config.apiUrl;
  };
  
  // Initialize
  if (validateConfig()) {
    console.log('App initialized successfully');
  }
  
  // Public API
  return {
    getApiUrl: () => config.apiUrl,
    setTimeout: (time) => {
      config.timeout = time;
      console.log(`Timeout set to ${time}ms`);
    }
  };
})();

// Output:
// Configuration validated
// App initialized successfully

console.log(app.getApiUrl());
// Output: https://api.example.com

app.setTimeout(3000);
// Output: Timeout set to 3000ms

// Cannot access private config
// console.log(app.config); // undefined
```

**Note:** IIFEs are perfect for initialization code and creating modules with private and public members.

---

## 6. Generator Function

### What is it?
Generator functions are special functions that can pause execution and resume later, yielding multiple values over time. They're defined using `function*` syntax and use the `yield` keyword.

### Key Characteristics:
- **Can pause and resume** execution
- **Defined with `function*`** syntax
- **Returns a Generator object**
- **Uses `yield`** to produce values
- **Useful for iterators** and lazy evaluation
- **Controls execution flow**

### Syntax:
```javascript
function* generatorName() {
  yield value1;
  yield value2;
  // ...
}
```

### Examples:

#### Basic Example:
```javascript
// Simple generator function
function* simpleGenerator() {
  console.log('First yield');
  yield 1;
  
  console.log('Second yield');
  yield 2;
  
  console.log('Third yield');
  yield 3;
  
  console.log('Done!');
}

const gen = simpleGenerator();

console.log(gen.next()); 
// Output: 
// First yield
// { value: 1, done: false }

console.log(gen.next());
// Output:
// Second yield
// { value: 2, done: false }

console.log(gen.next());
// Output:
// Third yield
// { value: 3, done: false }

console.log(gen.next());
// Output:
// Done!
// { value: undefined, done: true }
```

**Note:** Each call to `next()` runs the generator until it hits a `yield`, then pauses. The returned object has `value` (the yielded value) and `done` (whether generator is finished).

#### Infinite Sequence Generator:
```javascript
// Generator for infinite sequence of numbers
function* infiniteNumbers() {
  let num = 1;
  while (true) {
    yield num++;
  }
}

const numbers = infiniteNumbers();

console.log(numbers.next().value); // Output: 1
console.log(numbers.next().value); // Output: 2
console.log(numbers.next().value); // Output: 3
console.log(numbers.next().value); // Output: 4
```

**Note:** Generators can create infinite sequences without consuming infinite memory because values are generated on-demand (lazy evaluation).

#### ID Generator:
```javascript
// Practical example: Unique ID generator
function* idGenerator() {
  let id = 1;
  while (true) {
    yield `ID-${id++}`;
  }
}

const generateId = idGenerator();

console.log(generateId.next().value); // Output: ID-1
console.log(generateId.next().value); // Output: ID-2
console.log(generateId.next().value); // Output: ID-3

// Create another independent generator
const generateUserId = idGenerator();
console.log(generateUserId.next().value); // Output: ID-1
```

**Note:** Each generator instance maintains its own state independently.

#### Using Generators with for...of:
```javascript
// Generator for range of numbers
function* range(start, end) {
  console.log(`Generating range from ${start} to ${end}`);
  for (let i = start; i <= end; i++) {
    yield i;
  }
}

// Use with for...of loop
console.log('Using for...of:');
for (const num of range(1, 5)) {
  console.log(num);
}
// Output:
// Generating range from 1 to 5
// 1
// 2
// 3
// 4
// 5
```

**Note:** Generators work seamlessly with `for...of` loops, which automatically call `next()` until `done` is true.

#### Generator with yield*:
```javascript
// Delegating to another generator
function* generator1() {
  yield 'A';
  yield 'B';
}

function* generator2() {
  yield 1;
  yield 2;
}

function* combinedGenerator() {
  yield* generator1(); // Delegate to generator1
  yield* generator2(); // Delegate to generator2
  yield 'Done';
}

const combined = combinedGenerator();

console.log([...combined]);
// Output: ['A', 'B', 1, 2, 'Done']
```

**Note:** `yield*` delegates to another generator, yielding all its values before continuing.

#### Passing Values to Generator:
```javascript
// Generator that receives values
function* twoWayGenerator() {
  const first = yield 'What is your name?';
  console.log(`Name received: ${first}`);
  
  const second = yield 'What is your age?';
  console.log(`Age received: ${second}`);
  
  return `${first} is ${second} years old`;
}

const gen = twoWayGenerator();

console.log(gen.next());
// Output: { value: 'What is your name?', done: false }

console.log(gen.next('Alice'));
// Output:
// Name received: Alice
// { value: 'What is your age?', done: false }

console.log(gen.next(25));
// Output:
// Age received: 25
// { value: 'Alice is 25 years old', done: true }
```

**Note:** You can pass values back into a generator using `next(value)`. The value becomes the result of the `yield` expression.

#### Real-World Example - Pagination:
```javascript
// Generator for paginated data
function* paginatedData(data, pageSize) {
  let page = 1;
  for (let i = 0; i < data.length; i += pageSize) {
    const pageData = data.slice(i, i + pageSize);
    console.log(`--- Page ${page} ---`);
    yield pageData;
    page++;
  }
}

const allUsers = [
  'User1', 'User2', 'User3', 'User4', 'User5',
  'User6', 'User7', 'User8', 'User9', 'User10'
];

const pages = paginatedData(allUsers, 3);

console.log('Page 1:', pages.next().value);
// Output:
// --- Page 1 ---
// Page 1: ['User1', 'User2', 'User3']

console.log('Page 2:', pages.next().value);
// Output:
// --- Page 2 ---
// Page 2: ['User4', 'User5', 'User6']

console.log('Page 3:', pages.next().value);
// Output:
// --- Page 3 ---
// Page 3: ['User7', 'User8', 'User9']
```

**Note:** Generators are excellent for handling large datasets in chunks, making them memory-efficient.

---

## 7. Anonymous Function

### What is it?
An anonymous function is a function without a name. These functions are typically used as arguments to other functions (callbacks) or immediately invoked.

### Key Characteristics:
- **No function name**
- **Often used as callbacks**
- **Can be assigned to variables**
- **Used in IIFEs**
- **Harder to debug** (no name in stack trace)

### Syntax:
```javascript
function(parameters) {
  // function body
}

// Or as arrow function
(parameters) => {
  // function body
}
```

### Examples:

#### Basic Example:
```javascript
// Anonymous function assigned to variable
const greet = function(name) {
  return `Hello, ${name}!`;
};

console.log(greet('Alice'));
// Output: Hello, Alice!
```

**Note:** This is an anonymous function (no name after `function` keyword) stored in a variable.

#### As Callback Function:
```javascript
// Anonymous function as callback in setTimeout
console.log('Start');

setTimeout(function() {
  console.log('This runs after 2 seconds');
}, 2000);

console.log('End');

// Output (immediately):
// Start
// End
// Output (after 2 seconds):
// This runs after 2 seconds
```

**Note:** Anonymous functions are commonly used as callbacks. They're defined inline where they're needed.

#### With Array Methods:
```javascript
const numbers = [1, 2, 3, 4, 5];

// Anonymous function in map
const doubled = numbers.map(function(num) {
  return num * 2;
});
console.log('Doubled:', doubled);
// Output: Doubled: [2, 4, 6, 8, 10]

// Anonymous function in filter
const evens = numbers.filter(function(num) {
  return num % 2 === 0;
});
console.log('Evens:', evens);
// Output: Evens: [2, 4]

// Anonymous arrow function in reduce
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log('Sum:', sum);
// Output: Sum: 15
```

**Note:** Arrow functions are also anonymous functions. They're the modern, concise way to write anonymous functions.

#### Event Handlers:
```javascript
// Simulating event handling
const button = {
  addEventListener: function(event, callback) {
    console.log(`Event listener added for: ${event}`);
    callback(); // Simulate event
  }
};

// Anonymous function as event handler
button.addEventListener('click', function() {
  console.log('Button was clicked!');
});

// Output:
// Event listener added for: click
// Button was clicked!

// Anonymous arrow function
button.addEventListener('hover', () => {
  console.log('Button was hovered!');
});

// Output:
// Event listener added for: hover
// Button was hovered!
```

**Note:** Event handlers often use anonymous functions because the callback is only needed in that specific context.

#### IIFE with Anonymous Function:
```javascript
// Immediately invoked anonymous function
(function() {
  const message = 'This is private';
  console.log(message);
})();
// Output: This is private

// With parameters
(function(name, age) {
  console.log(`${name} is ${age} years old`);
})('Alice', 25);
// Output: Alice is 25 years old
```

**Note:** IIFEs always use anonymous functions because they're invoked immediately and don't need a name.

#### Multiple Anonymous Functions:
```javascript
const operations = {
  // Anonymous functions as object methods
  add: function(a, b) {
    return a + b;
  },
  
  subtract: function(a, b) {
    return a - b;
  },
  
  // Arrow function (also anonymous)
  multiply: (a, b) => a * b,
  
  divide: (a, b) => {
    if (b === 0) {
      console.log('Cannot divide by zero!');
      return null;
    }
    return a / b;
  }
};

console.log('10 + 5 =', operations.add(10, 5));
// Output: 10 + 5 = 15

console.log('10 - 5 =', operations.subtract(10, 5));
// Output: 10 - 5 = 5

console.log('10 * 5 =', operations.multiply(10, 5));
// Output: 10 * 5 = 50

console.log('10 / 5 =', operations.divide(10, 5));
// Output: 10 / 5 = 2

operations.divide(10, 0);
// Output: Cannot divide by zero!
```

**Note:** Anonymous functions are commonly used as object methods, especially when using arrow functions for concise syntax.

#### Disadvantage - Debugging:
```javascript
// Named function - easier to debug
function namedFunction() {
  console.trace('Named function called');
}

// Anonymous function - harder to debug
const anonymousFunction = function() {
  console.trace('Anonymous function called');
};

namedFunction();
// Output shows: "namedFunction" in stack trace

anonymousFunction();
// Output shows: "anonymousFunction" (from variable name) in stack trace

// Truly anonymous - hardest to debug
setTimeout(function() {
  console.trace('Timeout callback');
}, 100);
// Output shows: "anonymous" or no name in stack trace
```

**Note:** Anonymous functions can make debugging harder because they don't have meaningful names in stack traces. For complex callbacks, consider using named function expressions.

---

## 8. Recursive Function

### What is it?
A recursive function is a function that calls itself during its execution. It's used to solve problems that can be broken down into smaller, similar subproblems.

### Key Characteristics:
- **Calls itself**
- **Must have a base case** (stopping condition)
- **Can be any type of function** (declaration, expression, arrow)
- **Used for tree traversal, sorting, mathematical problems**
- **Risk of stack overflow** if no proper base case

### Syntax:
```javascript
function recursiveFunction(parameters) {
  // Base case - stops recursion
  if (condition) {
    return value;
  }
  
  // Recursive case - calls itself
  return recursiveFunction(modifiedParameters);
}
```

### Examples:

#### Basic Example - Countdown:
```javascript
// Simple countdown using recursion
function countdown(num) {
  // Base case: stop when num is 0
  if (num <= 0) {
    console.log('Done!');
    return;
  }
  
  console.log(num);
  // Recursive case: call itself with num - 1
  countdown(num - 1);
}

countdown(5);
// Output:
// 5
// 4
// 3
// 2
// 1
// Done!
```

**Note:** Every recursive function needs a **base case** (stopping condition) to prevent infinite recursion. Here, it's `num <= 0`.

#### Factorial:
```javascript
// Calculate factorial using recursion
function factorial(n) {
  // Base case
  if (n <= 1) {
    console.log(`Base case: factorial(${n}) = 1`);
    return 1;
  }
  
  // Recursive case
  console.log(`Calculating: ${n} * factorial(${n - 1})`);
  return n * factorial(n - 1);
}

const result = factorial(5);
console.log(`Result: ${result}`);

// Output:
// Calculating: 5 * factorial(4)
// Calculating: 4 * factorial(3)
// Calculating: 3 * factorial(2)
// Calculating: 2 * factorial(1)
// Base case: factorial(1) = 1
// Result: 120
```

**Note:** Factorial of 5 is 5 × 4 × 3 × 2 × 1 = 120. Each recursive call solves a smaller version of the problem.

#### Fibonacci Sequence:
```javascript
// Calculate Fibonacci number using recursion
function fibonacci(n) {
  console.log(`fibonacci(${n}) called`);
  
  // Base cases
  if (n <= 0) return 0;
  if (n === 1) return 1;
  
  // Recursive case: fib(n) = fib(n-1) + fib(n-2)
  return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log('Result:', fibonacci(5));

// Output:
// fibonacci(5) called
// fibonacci(4) called
// fibonacci(3) called
// fibonacci(2) called
// fibonacci(1) called
// fibonacci(0) called
// fibonacci(1) called
// fibonacci(2) called
// fibonacci(1) called
// fibonacci(0) called
// fibonacci(3) called
// fibonacci(2) called
// fibonacci(1) called
// fibonacci(0) called
// fibonacci(1) called
// Result: 5
```

**Note:** Fibonacci sequence: 0, 1, 1, 2, 3, 5, 8... Each number is the sum of the previous two. Notice how the same values are calculated multiple times (inefficient, but demonstrates recursion).

#### Sum of Array Elements:
```javascript
// Sum array elements using recursion
function sumArray(arr, index = 0) {
  // Base case: reached end of array
  if (index >= arr.length) {
    console.log('Base case reached');
    return 0;
  }
  
  console.log(`Adding arr[${index}] = ${arr[index]}`);
  // Recursive case: current element + sum of rest
  return arr[index] + sumArray(arr, index + 1);
}

const numbers = [1, 2, 3, 4, 5];
const total = sumArray(numbers);
console.log(`Total sum: ${total}`);

// Output:
// Adding arr[0] = 1
// Adding arr[1] = 2
// Adding arr[2] = 3
// Adding arr[3] = 4
// Adding arr[4] = 5
// Base case reached
// Total sum: 15
```

**Note:** Recursion processes the array one element at a time, adding each to the sum of the remaining elements.

#### Reverse a String:
```javascript
// Reverse string using recursion
function reverseString(str) {
  // Base case: empty or single character
  if (str.length <= 1) {
    console.log(`Base case: "${str}"`);
    return str;
  }
  
  console.log(`Processing: "${str}"`);
  // Recursive case: last char + reverse of rest
  return str[str.length - 1] + reverseString(str.slice(0, -1));
}

const reversed = reverseString('hello');
console.log(`Result: ${reversed}`);

// Output:
// Processing: "hello"
// Processing: "hell"
// Processing: "hel"
// Processing: "he"
// Base case: "h"
// Result: olleh
```

**Note:** Takes the last character and adds it to the reversed version of the remaining string.

#### Flatten Nested Array:
```javascript
// Flatten nested array using recursion
function flattenArray(arr) {
  let result = [];
  
  for (let item of arr) {
    if (Array.isArray(item)) {
      console.log(`Found nested array: ${JSON.stringify(item)}`);
      // Recursive case: flatten nested array
      result = result.concat(flattenArray(item));
    } else {
      console.log(`Adding element: ${item}`);
      result.push(item);
    }
  }
  
  return result;
}

const nested = [1, [2, 3], [4, [5, 6]], 7];
const flat = flattenArray(nested);
console.log('Flattened:', flat);

// Output:
// Adding element: 1
// Found nested array: [2,3]
// Adding element: 2
// Adding element: 3
// Found nested array: [4,[5,6]]
// Adding element: 4
// Found nested array: [5,6]
// Adding element: 5
// Adding element: 6
// Adding element: 7
// Flattened: [1, 2, 3, 4, 5, 6, 7]
```

**Note:** Recursion is perfect for handling nested structures of unknown depth.

#### Power Function:
```javascript
// Calculate power using recursion
function power(base, exponent) {
  // Base case
  if (exponent === 0) {
    console.log(`Base case: ${base}^0 = 1`);
    return 1;
  }
  
  console.log(`Calculating: ${base}^${exponent} = ${base} * ${base}^${exponent - 1}`);
  // Recursive case
  return base * power(base, exponent - 1);
}

const result = power(2, 4);
console.log(`Result: ${result}`);

// Output:
// Calculating: 2^4 = 2 * 2^3
// Calculating: 2^3 = 2 * 2^2
// Calculating: 2^2 = 2 * 2^1
// Calculating: 2^1 = 2 * 2^0
// Base case: 2^0 = 1
// Result: 16
```

**Note:** 2^4 = 2 × 2 × 2 × 2 = 16. Each recursive call reduces the exponent by 1.

#### ⚠️ Stack Overflow Warning:
```javascript
// Dangerous: No base case or wrong base case
function infiniteRecursion(n) {
  console.log(n);
  return infiniteRecursion(n + 1); // Never stops!
}

// Don't run this! It will crash
// infiniteRecursion(1);
// Output: RangeError: Maximum call stack size exceeded

// Safe version with proper base case
function safeRecursion(n, limit = 10) {
  if (n >= limit) {
    console.log('Stopped at limit');
    return;
  }
  console.log(n);
  safeRecursion(n + 1, limit);
}

safeRecursion(1, 5);
// Output:
// 1
// 2
// 3
// 4
// Stopped at limit
```

**Note:** Always ensure your recursive function has a reachable base case, or it will cause a stack overflow error.

---

## 9. Higher-Order Function

### What is it?
A higher-order function is a function that either:
1. Takes one or more functions as arguments (callbacks)
2. Returns a function as its result
3. Or both

### Key Characteristics:
- **Accepts functions as parameters**
- **Returns functions**
- **Enables functional programming**
- **Common in JavaScript** (map, filter, reduce)
- **Creates function factories**
- **Enables function composition**

### Syntax:
```javascript
// Function that takes a function as argument
function higherOrder(callback) {
  return callback();
}

// Function that returns a function
function higherOrder() {
  return function() {
    // ...
  };
}
```

### Examples:

#### Basic Example - Function as Argument:
```javascript
// Higher-order function that takes a callback
function greet(name, formatter) {
  const message = `Hello, ${name}!`;
  return formatter(message);
}

// Different formatter functions
function uppercase(str) {
  return str.toUpperCase();
}

function addExclamation(str) {
  return str + '!!!';
}

function addEmoji(str) {
  return str + ' 😊';
}

console.log(greet('Alice', uppercase));
// Output: HELLO, ALICE!

console.log(greet('Bob', addExclamation));
// Output: Hello, Bob!!!!

console.log(greet('Charlie', addEmoji));
// Output: Hello, Charlie! 😊
```

**Note:** `greet` is a higher-order function because it takes another function (`formatter`) as an argument.

#### Function Returning Function:
```javascript
// Higher-order function that returns a function
function createMultiplier(multiplier) {
  console.log(`Creating multiplier function for: ${multiplier}`);
  
  return function(number) {
    return number * multiplier;
  };
}

// Create specialized functions
const double = createMultiplier(2);
const triple = createMultiplier(3);
const quadruple = createMultiplier(4);

// Output:
// Creating multiplier function for: 2
// Creating multiplier function for: 3
// Creating multiplier function for: 4

console.log(double(5));     // Output: 10
console.log(triple(5));     // Output: 15
console.log(quadruple(5));  // Output: 20
```

**Note:** `createMultiplier` returns a new function. Each returned function "remembers" its multiplier value (closure).

#### Built-in Higher-Order Functions:
```javascript
const numbers = [1, 2, 3, 4, 5];

// map - transforms each element
const doubled = numbers.map(function(num) {
  console.log(`Doubling ${num}`);
  return num * 2;
});
console.log('Doubled:', doubled);

// Output:
// Doubling 1
// Doubling 2
// Doubling 3
// Doubling 4
// Doubling 5
// Doubled: [2, 4, 6, 8, 10]

// filter - keeps elements that pass test
const evens = numbers.filter(function(num) {
  const isEven = num % 2 === 0;
  console.log(`${num} is even: ${isEven}`);
  return isEven;
});
console.log('Evens:', evens);

// Output:
// 1 is even: false
// 2 is even: true
// 3 is even: false
// 4 is even: true
// 5 is even: false
// Evens: [2, 4]

// reduce - accumulates values
const sum = numbers.reduce(function(acc, num) {
  console.log(`Accumulator: ${acc}, Current: ${num}`);
  return acc + num;
}, 0);
console.log('Sum:', sum);

// Output:
// Accumulator: 0, Current: 1
// Accumulator: 1, Current: 2
// Accumulator: 3, Current: 3
// Accumulator: 6, Current: 4
// Accumulator: 10, Current: 5
// Sum: 15
```

**Note:** `map`, `filter`, and `reduce` are higher-order functions because they take callback functions as arguments.

#### Creating Custom Higher-Order Functions:
```javascript
// Custom forEach implementation
function customForEach(array, callback) {
  console.log('Starting custom forEach...');
  for (let i = 0; i < array.length; i++) {
    callback(array[i], i, array);
  }
  console.log('Finished custom forEach');
}

customForEach([10, 20, 30], function(item, index) {
  console.log(`Index ${index}: ${item}`);
});

// Output:
// Starting custom forEach...
// Index 0: 10
// Index 1: 20
// Index 2: 30
// Finished custom forEach

// Custom filter implementation
function customFilter(array, predicate) {
  console.log('Starting custom filter...');
  const result = [];
  for (let item of array) {
    if (predicate(item)) {
      console.log(`Keeping: ${item}`);
      result.push(item);
    } else {
      console.log(`Filtering out: ${item}`);
    }
  }
  return result;
}

const filtered = customFilter([1, 2, 3, 4, 5], num => num > 2);
console.log('Filtered result:', filtered);

// Output:
// Starting custom filter...
// Filtering out: 1
// Filtering out: 2
// Keeping: 3
// Keeping: 4
// Keeping: 5
// Filtered result: [3, 4, 5]
```

**Note:** Understanding how built-in higher-order functions work helps you create your own reusable utilities.

#### Function Composition:
```javascript
// Higher-order function for composing functions
function compose(...functions) {
  return function(input) {
    console.log(`Starting with input: ${input}`);
    return functions.reduceRight((acc, fn) => {
      const result = fn(acc);
      console.log(`Applied ${fn.name}: ${result}`);
      return result;
    }, input);
  };
}

// Simple functions to compose
function addTen(x) {
  return x + 10;
}

function multiplyByTwo(x) {
  return x * 2;
}

function subtractFive(x) {
  return x - 5;
}

// Compose functions (right to left)
const calculate = compose(subtractFive, multiplyByTwo, addTen);

const result = calculate(5);
console.log(`Final result: ${result}`);

// Output:
// Starting with input: 5
// Applied addTen: 15
// Applied multiplyByTwo: 30
// Applied subtractFive: 25
// Final result: 25
```

**Note:** Composition allows you to combine simple functions into complex operations. Functions are applied right to left: (5 + 10) × 2 - 5 = 25.

#### Memoization (Caching):
```javascript
// Higher-order function for memoization
function memoize(fn) {
  const cache = {};
  
  return function(...args) {
    const key = JSON.stringify(args);
    
    if (key in cache) {
      console.log(`Cache hit for ${key}`);
      return cache[key];
    }
    
    console.log(`Cache miss for ${key}, calculating...`);
    const result = fn(...args);
    cache[key] = result;
    return result;
  };
}

// Expensive function
function slowSquare(n) {
  console.log(`Computing square of ${n}`);
  return n * n;
}

const memoizedSquare = memoize(slowSquare);

console.log(memoizedSquare(5));
// Output:
// Cache miss for [5], calculating...
// Computing square of 5
// 25

console.log(memoizedSquare(5));
// Output:
// Cache hit for [5]
// 25

console.log(memoizedSquare(10));
// Output:
// Cache miss for [10], calculating...
// Computing square of 10
// 100

console.log(memoizedSquare(5));
// Output:
// Cache hit for [5]
// 25
```

**Note:** Memoization is a higher-order function pattern that caches results for performance. Repeated calls with same arguments return cached results instantly.

#### Partial Application:
```javascript
// Higher-order function for partial application
function partial(fn, ...fixedArgs) {
  return function(...remainingArgs) {
    const allArgs = [...fixedArgs, ...remainingArgs];
    console.log(`Calling with args: ${allArgs}`);
    return fn(...allArgs);
  };
}

// Original function
function greetPerson(greeting, name, punctuation) {
  return `${greeting}, ${name}${punctuation}`;
}

// Create specialized versions
const sayHello = partial(greetPerson, 'Hello');
const sayHelloExcited = partial(greetPerson, 'Hello', 'World');

console.log(sayHello('Alice', '!'));
// Output:
// Calling with args: Hello,Alice,!
// Hello, Alice!

console.log(sayHello('Bob', '.'));
// Output:
// Calling with args: Hello,Bob,.
// Hello, Bob.

console.log(sayHelloExcited('!!!'));
// Output:
// Calling with args: Hello,World,!!!
// Hello, World!!!
```

**Note:** Partial application creates specialized functions by pre-filling some arguments. It's a powerful pattern for creating reusable functions.

#### Debounce (Real-World Example):
```javascript
// Higher-order function for debouncing
function debounce(func, delay) {
  let timeoutId;
  
  return function(...args) {
    console.log(`Debounce called, resetting timer`);
    
    clearTimeout(timeoutId);
    
    timeoutId = setTimeout(() => {
      console.log(`Executing function after ${delay}ms delay`);
      func(...args);
    }, delay);
  };
}

// Function to debounce
function search(query) {
  console.log(`Searching for: "${query}"`);
}

// Create debounced version
const debouncedSearch = debounce(search, 500);

// Simulate rapid calls (like user typing)
console.log('User typing...');
debouncedSearch('a');
debouncedSearch('ap');
debouncedSearch('app');
debouncedSearch('appl');
debouncedSearch('apple');

// Output (immediately):
// User typing...
// Debounce called, resetting timer
// Debounce called, resetting timer
// Debounce called, resetting timer
// Debounce called, resetting timer
// Debounce called, resetting timer

// Output (after 500ms):
// Executing function after 500ms delay
// Searching for: "apple"
```

**Note:** Debouncing delays function execution until after a pause in calls. Perfect for search boxes, window resize handlers, etc.

---

## Comparison Table

| Function Type | Hoisted | Has `this` | Has `arguments` | Can be Constructor | Use Case |
|--------------|---------|------------|-----------------|-------------------|----------|
| **Function Declaration** | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes | General purpose functions |
| **Function Expression** | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes | Callbacks, conditional definitions |
| **Arrow Function** | ❌ No | ❌ No (lexical) | ❌ No | ❌ No | Short callbacks, array methods |
| **Function Constructor** | ❌ No | ✅ Yes | ✅ Yes | ✅ Yes | Dynamic code (rarely used) |
| **IIFE** | N/A | ✅ Yes | ✅ Yes | ❌ No | Encapsulation, initialization |
| **Generator Function** | ✅ Yes | ✅ Yes | ✅ Yes | ❌ No | Iterators, lazy evaluation |
| **Anonymous Function** | ❌ No | ✅ Yes | ✅ Yes | Depends | Callbacks, one-time use |
| **Recursive Function** | Depends | Depends | Depends | Depends | Tree traversal, divide-conquer |
| **Higher-Order Function** | Depends | Depends | Depends | Depends | Functional programming, utilities |

---

## When to Use Each Type

### Use Function Declaration when:
- You need hoisting
- Creating utility functions
- Function is used throughout the file
- Need clear, named functions for debugging

### Use Function Expression when:
- You want to prevent hoisting
- Conditionally creating functions
- Assigning functions to object properties
- Need control over when function is defined

### Use Arrow Function when:
- Writing short, concise functions
- Working with array methods (map, filter, reduce)
- Need lexical `this` binding
- Creating inline callbacks

### Use Function Constructor when:
- Almost never (security and performance issues)
- Generating functions from user input (with extreme caution)

### Use IIFE when:
- Need private scope/variables
- Initializing code once
- Creating modules
- Avoiding global namespace pollution

### Use Generator Function when:
- Creating custom iterators
- Handling large datasets lazily
- Implementing pausable operations
- Managing complex asynchronous flows

### Use Anonymous Function when:
- One-time callbacks
- Inline event handlers
- Function won't be reused
- Quick utility operations

### Use Recursive Function when:
- Tree/graph traversal
- Divide and conquer algorithms
- Processing nested structures
- Mathematical computations (factorial, fibonacci)

### Use Higher-Order Function when:
- Building reusable abstractions
- Functional programming patterns
- Creating function factories
- Composing complex operations

---

## Best Practices

1. **Choose the right tool**: Use the function type that best fits your use case
2. **Prefer arrow functions** for simple callbacks and array methods
3. **Use function declarations** for top-level, reusable functions
4. **Avoid Function Constructor** due to security and performance concerns
5. **Always include base cases** in recursive functions
6. **Use IIFEs** for module patterns and initialization
7. **Leverage higher-order functions** for cleaner, more maintainable code
8. **Name your functions** when possible for better debugging
9. **Avoid deep recursion** without tail call optimization
10. **Understand `this` binding** differences between function types

---

## Summary

JavaScript offers multiple ways to declare and use functions, each with specific purposes:

- **Function Declarations** and **Function Expressions** are traditional, versatile options
- **Arrow Functions** provide modern, concise syntax with lexical scoping
- **Function Constructors** should be avoided in most cases
- **IIFEs** are perfect for encapsulation and avoiding global pollution
- **Generator Functions** enable powerful iteration and lazy evaluation
- **Anonymous Functions** are great for quick, inline operations
- **Recursive Functions** solve problems by breaking them into smaller pieces
- **Higher-Order Functions** enable functional programming and code reusability

Master these different function types to write cleaner, more efficient, and more maintainable JavaScript code!

---

**Happy Coding! 🚀**