# Callback Functions in JavaScript - Interview Preparation Notes

## 📚 Table of Contents
1. [What is a Callback Function?](#what-is-a-callback-function)
2. [Why Use Callback Functions?](#why-use-callback-functions)
3. [Types of Callbacks](#types-of-callbacks)
4. [Synchronous vs Asynchronous Callbacks](#synchronous-vs-asynchronous-callbacks)
5. [Callback Hell (Pyramid of Doom)](#callback-hell-pyramid-of-doom)
6. [Error Handling in Callbacks](#error-handling-in-callbacks)
7. [Callback Patterns](#callback-patterns)
8. [Real-World Test Automation Examples](#real-world-test-automation-examples)
9. [Interview Questions & Answers](#interview-questions--answers)
10. [Best Practices](#best-practices)

---

## What is a Callback Function?

A **callback function** is a function that is passed as an argument to another function and is executed after some operation has been completed. The function that receives the callback is responsible for calling it at the appropriate time.

### Key Concepts

- **Higher-Order Function**: A function that accepts another function as an argument or returns a function
- **Callback Function**: The function passed as an argument
- **Asynchronous Execution**: Callbacks are commonly used for handling asynchronous operations
- **Event Handling**: Callbacks are fundamental to event-driven programming

### Basic Syntax

```javascript
// Function that accepts a callback
function doSomething(callback) {
    // Do some work
    callback(); // Execute the callback
}

// Pass a function as callback
doSomething(function() {
    console.log('Callback executed!');
});
```

### Visual Diagram

```
┌─────────────────────────────────────────────────────┐
│           Function Execution Flow                    │
├─────────────────────────────────────────────────────┤
│                                                      │
│  1. Main Function Called                            │
│     ↓                                                │
│  2. Main Function Executes Logic                    │
│     ↓                                                │
│  3. Callback Function Called                        │
│     ↓                                                │
│  4. Callback Function Executes                      │
│     ↓                                                │
│  5. Control Returns to Main Function                │
│     ↓                                                │
│  6. Main Function Completes                         │
│                                                      │
└─────────────────────────────────────────────────────┘
```

---

## Simple Examples

### Example 1: Basic Callback

```javascript
function greet(name, callback) {
    console.log('Hello, ' + name);
    callback();
}

function sayGoodbye() {
    console.log('Goodbye!');
}

// Pass sayGoodbye as callback
greet('John', sayGoodbye);
// Output:
// Hello, John
// Goodbye!

// Or use anonymous function
greet('Alice', function() {
    console.log('Nice to meet you!');
});
// Output:
// Hello, Alice
// Nice to meet you!

// Or use arrow function
greet('Bob', () => {
    console.log('See you later!');
});
// Output:
// Hello, Bob
// See you later!
```

### Example 2: Callback with Parameters

```javascript
function calculate(a, b, operation) {
    const result = operation(a, b);
    console.log('Result:', result);
    return result;
}

// Different callback functions
function add(x, y) {
    return x + y;
}

function multiply(x, y) {
    return x * y;
}

function subtract(x, y) {
    return x - y;
}

// Use different callbacks
calculate(5, 3, add);       // Result: 8
calculate(5, 3, multiply);  // Result: 15
calculate(5, 3, subtract);  // Result: 2

// Using arrow functions as callbacks
calculate(10, 2, (x, y) => x / y);  // Result: 5
calculate(2, 3, (x, y) => x ** y);  // Result: 8
```

### Example 3: Array Methods with Callbacks

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - executes callback for each element
numbers.forEach(function(num) {
    console.log(num * 2);
});
// Output: 2, 4, 6, 8, 10

// map - creates new array with callback results
const doubled = numbers.map(function(num) {
    return num * 2;
});
console.log(doubled); // [2, 4, 6, 8, 10]

// filter - creates new array with elements that pass the test
const evenNumbers = numbers.filter(function(num) {
    return num % 2 === 0;
});
console.log(evenNumbers); // [2, 4]

// reduce - reduces array to single value
const sum = numbers.reduce(function(accumulator, num) {
    return accumulator + num;
}, 0);
console.log(sum); // 15

// find - returns first element that passes the test
const firstEven = numbers.find(function(num) {
    return num % 2 === 0;
});
console.log(firstEven); // 2

// Using arrow functions (cleaner syntax)
const tripled = numbers.map(num => num * 3);
const odds = numbers.filter(num => num % 2 !== 0);
const product = numbers.reduce((acc, num) => acc * num, 1);
```

---

## Why Use Callback Functions?

### 1. Asynchronous Operations

```javascript
// Without callback - won't work properly
function fetchUserData(userId) {
    let userData;
    setTimeout(() => {
        userData = { id: userId, name: 'John Doe' };
    }, 1000);
    return userData; // Returns undefined! (too early)
}

const user = fetchUserData(1);
console.log(user); // undefined

// With callback - proper async handling
function fetchUserDataWithCallback(userId, callback) {
    setTimeout(() => {
        const userData = { id: userId, name: 'John Doe' };
        callback(userData); // Execute callback with data
    }, 1000);
}

fetchUserDataWithCallback(1, function(user) {
    console.log(user); // { id: 1, name: 'John Doe' }
});
```

### 2. Code Reusability

```javascript
// Generic function that works with different operations
function processArray(array, callback) {
    const results = [];
    for (let i = 0; i < array.length; i++) {
        results.push(callback(array[i], i));
    }
    return results;
}

const numbers = [1, 2, 3, 4, 5];

// Different processing logic via callbacks
const squared = processArray(numbers, num => num ** 2);
console.log(squared); // [1, 4, 9, 16, 25]

const withIndex = processArray(numbers, (num, idx) => `${idx}: ${num}`);
console.log(withIndex); // ["0: 1", "1: 2", "2: 3", "3: 4", "4: 5"]
```

### 3. Event Handling

```javascript
// DOM event listeners use callbacks
document.getElementById('myButton').addEventListener('click', function() {
    console.log('Button clicked!');
});

// Custom event system
class EventEmitter {
    constructor() {
        this.events = {};
    }
    
    on(eventName, callback) {
        if (!this.events[eventName]) {
            this.events[eventName] = [];
        }
        this.events[eventName].push(callback);
    }
    
    emit(eventName, data) {
        if (this.events[eventName]) {
            this.events[eventName].forEach(callback => callback(data));
        }
    }
}

const emitter = new EventEmitter();

emitter.on('userLogin', function(user) {
    console.log('User logged in:', user.name);
});

emitter.on('userLogin', function(user) {
    console.log('Send welcome email to:', user.email);
});

emitter.emit('userLogin', { name: 'John', email: 'john@example.com' });
// Output:
// User logged in: John
// Send welcome email to: john@example.com
```

### 4. Customization

```javascript
function sortUsers(users, compareCallback) {
    return users.sort(compareCallback);
}

const users = [
    { name: 'John', age: 30 },
    { name: 'Alice', age: 25 },
    { name: 'Bob', age: 35 }
];

// Sort by age ascending
const byAge = sortUsers([...users], (a, b) => a.age - b.age);
console.log(byAge);
// [{ name: 'Alice', age: 25 }, { name: 'John', age: 30 }, { name: 'Bob', age: 35 }]

// Sort by name alphabetically
const byName = sortUsers([...users], (a, b) => a.name.localeCompare(b.name));
console.log(byName);
// [{ name: 'Alice', age: 25 }, { name: 'Bob', age: 35 }, { name: 'John', age: 30 }]
```

---

## Types of Callbacks

### 1. Synchronous Callbacks

Executed immediately during the function execution.

```javascript
// Synchronous callback example
function processNumbers(numbers, callback) {
    console.log('Start processing');
    const results = [];
    
    for (const num of numbers) {
        results.push(callback(num)); // Executed immediately
    }
    
    console.log('End processing');
    return results;
}

const numbers = [1, 2, 3];
const squared = processNumbers(numbers, num => num ** 2);

// Output:
// Start processing
// End processing

console.log(squared); // [1, 4, 9]
```

### 2. Asynchronous Callbacks

Executed after an asynchronous operation completes.

```javascript
// Asynchronous callback example
function loadData(callback) {
    console.log('Start loading');
    
    setTimeout(() => {
        const data = { id: 1, name: 'Test Data' };
        callback(data); // Executed after delay
    }, 1000);
    
    console.log('End loading function (but data not loaded yet)');
}

loadData(function(data) {
    console.log('Data loaded:', data);
});

// Output:
// Start loading
// End loading function (but data not loaded yet)
// (after 1 second)
// Data loaded: { id: 1, name: 'Test Data' }
```

---

## Synchronous vs Asynchronous Callbacks

### Synchronous Callbacks

```javascript
// Array methods are synchronous
const numbers = [1, 2, 3, 4, 5];

console.log('Before map');
const doubled = numbers.map(num => {
    console.log('Processing:', num);
    return num * 2;
});
console.log('After map');
console.log('Result:', doubled);

// Output (executes in order):
// Before map
// Processing: 1
// Processing: 2
// Processing: 3
// Processing: 4
// Processing: 5
// After map
// Result: [2, 4, 6, 8, 10]
```

### Asynchronous Callbacks

```javascript
// setTimeout is asynchronous
console.log('Start');

setTimeout(function() {
    console.log('Timeout 1 - 1000ms');
}, 1000);

setTimeout(function() {
    console.log('Timeout 2 - 500ms');
}, 500);

console.log('End');

// Output (async execution):
// Start
// End
// (after 500ms) Timeout 2 - 500ms
// (after 1000ms) Timeout 1 - 1000ms
```

### Visual Comparison

```
SYNCHRONOUS CALLBACKS
┌────────────────────────────────┐
│ 1. Function Call               │
│ 2. Callback Executes ──────┐   │
│ 3. Callback Completes      │   │
│ 4. Function Continues   <──┘   │
│ 5. Function Returns            │
└────────────────────────────────┘
    All happens in sequence


ASYNCHRONOUS CALLBACKS
┌────────────────────────────────┐
│ 1. Function Call               │
│ 2. Async Operation Started     │
│ 3. Function Returns            │
│    (operation continues)       │
│ 4. Callback Executes Later ──┐ │
│ 5. Callback Completes      <──┘│
└────────────────────────────────┘
    Callback executes when ready
```

---

## Callback Hell (Pyramid of Doom)

When callbacks are nested multiple levels deep, code becomes hard to read and maintain.

### Example of Callback Hell

```javascript
// ❌ Bad: Callback Hell
function loginUser(email, password, callback) {
    setTimeout(() => {
        console.log('User logged in');
        callback({ userId: 1, email: email });
    }, 1000);
}

function getUserData(userId, callback) {
    setTimeout(() => {
        console.log('Got user data');
        callback({ userId: userId, name: 'John Doe' });
    }, 1000);
}

function getOrders(userId, callback) {
    setTimeout(() => {
        console.log('Got orders');
        callback([{ orderId: 1, item: 'Book' }]);
    }, 1000);
}

function getOrderDetails(orderId, callback) {
    setTimeout(() => {
        console.log('Got order details');
        callback({ orderId: orderId, price: 29.99 });
    }, 1000);
}

// Nested callbacks - CALLBACK HELL!
loginUser('test@example.com', 'password123', function(user) {
    getUserData(user.userId, function(userData) {
        getOrders(userData.userId, function(orders) {
            getOrderDetails(orders[0].orderId, function(orderDetails) {
                console.log('Final result:', orderDetails);
                // Imagine more levels...
            });
        });
    });
});
```

### Visual Representation of Callback Hell

```
loginUser(credentials, function(user) {
    getUserData(user, function(userData) {
        getOrders(userData, function(orders) {
            getOrderDetails(orders, function(details) {
                processPayment(details, function(payment) {
                    sendConfirmation(payment, function(confirmation) {
                        // Keep going deeper...
                            // This is callback hell!
                                // Hard to read
                                    // Hard to debug
                                        // Hard to maintain
                    });
                });
            });
        });
    });
});
```

### Solutions to Callback Hell

#### Solution 1: Named Functions

```javascript
// ✅ Better: Use named functions
function handleOrderDetails(orderDetails) {
    console.log('Final result:', orderDetails);
}

function handleOrders(orders) {
    getOrderDetails(orders[0].orderId, handleOrderDetails);
}

function handleUserData(userData) {
    getOrders(userData.userId, handleOrders);
}

function handleLogin(user) {
    getUserData(user.userId, handleUserData);
}

// Much flatter and readable
loginUser('test@example.com', 'password123', handleLogin);
```

#### Solution 2: Promises (Modern Approach)

```javascript
// ✅ Best: Use Promises
function loginUserPromise(email, password) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({ userId: 1, email: email });
        }, 1000);
    });
}

function getUserDataPromise(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({ userId: userId, name: 'John Doe' });
        }, 1000);
    });
}

function getOrdersPromise(userId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve([{ orderId: 1, item: 'Book' }]);
        }, 1000);
    });
}

function getOrderDetailsPromise(orderId) {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve({ orderId: orderId, price: 29.99 });
        }, 1000);
    });
}

// Promise chain - much cleaner!
loginUserPromise('test@example.com', 'password123')
    .then(user => getUserDataPromise(user.userId))
    .then(userData => getOrdersPromise(userData.userId))
    .then(orders => getOrderDetailsPromise(orders[0].orderId))
    .then(orderDetails => {
        console.log('Final result:', orderDetails);
    })
    .catch(error => {
        console.error('Error:', error);
    });
```

#### Solution 3: Async/Await (Most Modern)

```javascript
// ✅ Best: Use async/await
async function processUserOrder() {
    try {
        const user = await loginUserPromise('test@example.com', 'password123');
        const userData = await getUserDataPromise(user.userId);
        const orders = await getOrdersPromise(userData.userId);
        const orderDetails = await getOrderDetailsPromise(orders[0].orderId);
        console.log('Final result:', orderDetails);
    } catch (error) {
        console.error('Error:', error);
    }
}

processUserOrder();
```

---

## Error Handling in Callbacks

### Error-First Callback Pattern (Node.js Convention)

```javascript
// Convention: First parameter is error, second is data
function readFile(filename, callback) {
    setTimeout(() => {
        const error = filename ? null : new Error('Filename is required');
        const data = filename ? `Contents of ${filename}` : null;
        
        callback(error, data);
    }, 1000);
}

// Using error-first callback
readFile('test.txt', function(error, data) {
    if (error) {
        console.error('Error:', error.message);
        return;
    }
    console.log('Data:', data);
});

// With no filename - triggers error
readFile(null, function(error, data) {
    if (error) {
        console.error('Error:', error.message); // Error: Filename is required
        return;
    }
    console.log('Data:', data);
});
```

### Try-Catch with Callbacks

```javascript
function riskyOperation(callback) {
    try {
        // Simulate risky operation
        const random = Math.random();
        if (random < 0.5) {
            throw new Error('Operation failed');
        }
        callback(null, 'Success!');
    } catch (error) {
        callback(error, null);
    }
}

// Handle both success and error
riskyOperation(function(error, result) {
    if (error) {
        console.error('Caught error:', error.message);
    } else {
        console.log('Result:', result);
    }
});
```

### Multiple Callbacks (Success & Error)

```javascript
function fetchData(successCallback, errorCallback) {
    setTimeout(() => {
        const success = Math.random() > 0.5;
        
        if (success) {
            successCallback({ data: 'Fetched successfully' });
        } else {
            errorCallback(new Error('Fetch failed'));
        }
    }, 1000);
}

// Separate callbacks for success and error
fetchData(
    function(data) {
        console.log('Success:', data);
    },
    function(error) {
        console.error('Error:', error.message);
    }
);
```

---

## Callback Patterns

### 1. Once Callback Pattern

```javascript
function createOnce(callback) {
    let called = false;
    
    return function(...args) {
        if (!called) {
            called = true;
            callback(...args);
        } else {
            console.log('Callback already executed');
        }
    };
}

const onceCallback = createOnce(function(message) {
    console.log('Message:', message);
});

onceCallback('First call');  // Message: First call
onceCallback('Second call'); // Callback already executed
onceCallback('Third call');  // Callback already executed
```

### 2. Debounce Pattern

```javascript
function debounce(callback, delay) {
    let timeoutId;
    
    return function(...args) {
        clearTimeout(timeoutId);
        timeoutId = setTimeout(() => {
            callback(...args);
        }, delay);
    };
}

// Usage example
const searchInput = debounce(function(query) {
    console.log('Searching for:', query);
}, 500);

searchInput('a');    // Won't execute
searchInput('ab');   // Won't execute
searchInput('abc');  // Executes after 500ms: "Searching for: abc"
```

### 3. Throttle Pattern

```javascript
function throttle(callback, limit) {
    let inThrottle;
    
    return function(...args) {
        if (!inThrottle) {
            callback(...args);
            inThrottle = true;
            setTimeout(() => {
                inThrottle = false;
            }, limit);
        }
    };
}

// Usage example
const scrollHandler = throttle(function() {
    console.log('Scroll event handled');
}, 1000);

// Even if called multiple times, executes at most once per second
window.addEventListener('scroll', scrollHandler);
```

### 4. Retry Pattern

```javascript
function retry(operation, maxAttempts, callback) {
    let attempts = 0;
    
    function attempt() {
        attempts++;
        
        operation(function(error, result) {
            if (error) {
                if (attempts < maxAttempts) {
                    console.log(`Attempt ${attempts} failed, retrying...`);
                    setTimeout(attempt, 1000);
                } else {
                    callback(new Error(`Failed after ${maxAttempts} attempts`), null);
                }
            } else {
                callback(null, result);
            }
        });
    }
    
    attempt();
}

// Simulated flaky operation
function flakyOperation(callback) {
    const success = Math.random() > 0.7;
    setTimeout(() => {
        if (success) {
            callback(null, 'Success!');
        } else {
            callback(new Error('Operation failed'), null);
        }
    }, 500);
}

// Retry up to 3 times
retry(flakyOperation, 3, function(error, result) {
    if (error) {
        console.error('Final error:', error.message);
    } else {
        console.log('Final result:', result);
    }
});
```

---

## Real-World Test Automation Examples

### Example 1: Page Object with Callbacks

```javascript
class LoginPage {
    constructor(page) {
        this.page = page;
        this.usernameField = '#username';
        this.passwordField = '#password';
        this.loginButton = '#login-btn';
    }
    
    login(username, password, callback) {
        this.page.fill(this.usernameField, username)
            .then(() => this.page.fill(this.passwordField, password))
            .then(() => this.page.click(this.loginButton))
            .then(() => this.page.waitForSelector('#dashboard'))
            .then(() => {
                console.log('Login successful');
                callback(null, true);
            })
            .catch((error) => {
                console.error('Login failed:', error);
                callback(error, null);
            });
    }
    
    loginWithRetry(username, password, maxRetries, callback) {
        let attempts = 0;
        
        const attemptLogin = () => {
            attempts++;
            this.login(username, password, (error, success) => {
                if (error && attempts < maxRetries) {
                    console.log(`Retry attempt ${attempts}...`);
                    setTimeout(attemptLogin, 2000);
                } else {
                    callback(error, success);
                }
            });
        };
        
        attemptLogin();
    }
}

// Usage
const loginPage = new LoginPage(page);

loginPage.login('testuser', 'password123', function(error, success) {
    if (error) {
        console.error('Login error:', error);
    } else {
        console.log('Logged in successfully');
    }
});

// With retry
loginPage.loginWithRetry('testuser', 'password123', 3, function(error, success) {
    if (error) {
        console.error('Login failed after retries:', error);
    } else {
        console.log('Logged in successfully');
    }
});
```

### Example 2: Test Data Generator with Callbacks

```javascript
class TestDataGenerator {
    generateUser(callback) {
        setTimeout(() => {
            const user = {
                id: Math.floor(Math.random() * 10000),
                username: `user_${Date.now()}`,
                email: `test_${Date.now()}@example.com`,
                password: this.generatePassword()
            };
            callback(null, user);
        }, 100);
    }
    
    generatePassword() {
        return Math.random().toString(36).slice(-10);
    }
    
    generateMultipleUsers(count, callback) {
        const users = [];
        let generated = 0;
        
        for (let i = 0; i < count; i++) {
            this.generateUser((error, user) => {
                if (error) {
                    callback(error, null);
                    return;
                }
                
                users.push(user);
                generated++;
                
                if (generated === count) {
                    callback(null, users);
                }
            });
        }
    }
    
    generateUserWithValidation(callback) {
        this.generateUser((error, user) => {
            if (error) {
                callback(error, null);
                return;
            }
            
            // Validate generated user
            if (this.validateUser(user)) {
                callback(null, user);
            } else {
                callback(new Error('User validation failed'), null);
            }
        });
    }
    
    validateUser(user) {
        return user.email.includes('@') && 
               user.username.length > 5 && 
               user.password.length >= 8;
    }
}

// Usage
const generator = new TestDataGenerator();

generator.generateUser(function(error, user) {
    if (error) {
        console.error('Error:', error);
    } else {
        console.log('Generated user:', user);
    }
});

generator.generateMultipleUsers(5, function(error, users) {
    if (error) {
        console.error('Error:', error);
    } else {
        console.log('Generated users:', users);
    }
});

generator.generateUserWithValidation(function(error, user) {
    if (error) {
        console.error('Validation error:', error);
    } else {
        console.log('Valid user:', user);
    }
});
```

### Example 3: API Testing with Callbacks

```javascript
class APIClient {
    constructor(baseURL) {
        this.baseURL = baseURL;
    }
    
    request(endpoint, options, callback) {
        const url = `${this.baseURL}${endpoint}`;
        
        fetch(url, options)
            .then(response => {
                if (!response.ok) {
                    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
                }
                return response.json();
            })
            .then(data => callback(null, data))
            .catch(error => callback(error, null));
    }
    
    get(endpoint, callback) {
        this.request(endpoint, { method: 'GET' }, callback);
    }
    
    post(endpoint, body, callback) {
        this.request(endpoint, {
            method: 'POST',
            headers: { 'Content-Type': 'application/json' },
            body: JSON.stringify(body)
        }, callback);
    }
    
    // Test helper with assertions
    testEndpoint(endpoint, expectedStatus, callback) {
        this.get(endpoint, (error, data) => {
            if (error) {
                callback({
                    passed: false,
                    error: error.message
                });
                return;
            }
            
            const passed = data.status === expectedStatus;
            callback({
                passed: passed,
                data: data,
                error: passed ? null : `Expected status ${expectedStatus}, got ${data.status}`
            });
        });
    }
    
    // Sequential requests with callbacks
    createAndVerifyUser(userData, callback) {
        // Step 1: Create user
        this.post('/users', userData, (error, createdUser) => {
            if (error) {
                callback(error, null);
                return;
            }
            
            console.log('User created:', createdUser);
            
            // Step 2: Verify user was created
            this.get(`/users/${createdUser.id}`, (error, fetchedUser) => {
                if (error) {
                    callback(error, null);
                    return;
                }
                
                console.log('User verified:', fetchedUser);
                callback(null, {
                    created: createdUser,
                    verified: fetchedUser,
                    match: createdUser.id === fetchedUser.id
                });
            });
        });
    }
}

// Usage
const api = new APIClient('https://api.example.com');

api.get('/users/1', function(error, data) {
    if (error) {
        console.error('GET Error:', error);
    } else {
        console.log('User data:', data);
    }
});

api.post('/users', { name: 'John Doe', email: 'john@example.com' }, function(error, data) {
    if (error) {
        console.error('POST Error:', error);
    } else {
        console.log('Created user:', data);
    }
});

api.testEndpoint('/health', 200, function(result) {
    if (result.passed) {
        console.log('✅ Health check passed');
    } else {
        console.log('❌ Health check failed:', result.error);
    }
});

api.createAndVerifyUser({ name: 'Test User', email: 'test@example.com' }, function(error, result) {
    if (error) {
        console.error('Error:', error);
    } else {
        console.log('Result:', result);
        console.log('User verified:', result.match ? 'Yes' : 'No');
    }
});
```

### Example 4: Test Reporter with Callbacks

```javascript
class TestReporter {
    constructor() {
        this.tests = [];
        this.currentTest = null;
    }
    
    startTest(testName, callback) {
        this.currentTest = {
            name: testName,
            startTime: Date.now(),
            endTime: null,
            status: 'running',
            steps: []
        };
        
        console.log(`\n▶ Starting test: ${testName}`);
        callback();
    }
    
    logStep(step, callback) {
        this.currentTest.steps.push({
            description: step,
            timestamp: Date.now()
        });
        console.log(`  ✓ ${step}`);
        
        if (callback) {
            setTimeout(callback, 10); // Small delay for async
        }
    }
    
    endTest(status, callback) {
        this.currentTest.endTime = Date.now();
        this.currentTest.status = status;
        
        const duration = this.currentTest.endTime - this.currentTest.startTime;
        const symbol = status === 'passed' ? '✅' : '❌';
        
        console.log(`${symbol} Test ${status}: ${this.currentTest.name} (${duration}ms)`);
        
        this.tests.push(this.currentTest);
        this.currentTest = null;
        
        if (callback) {
            callback();
        }
    }
    
    generateReport(callback) {
        const total = this.tests.length;
        const passed = this.tests.filter(t => t.status === 'passed').length;
        const failed = total - passed;
        
        const report = {
            total: total,
            passed: passed,
            failed: failed,
            passRate: ((passed / total) * 100).toFixed(2) + '%',
            tests: this.tests
        };
        
        console.log('\n📊 Test Report:');
        console.log(`   Total: ${total}`);
        console.log(`   Passed: ${passed}`);
        console.log(`   Failed: ${failed}`);
        console.log(`   Pass Rate: ${report.passRate}`);
        
        callback(null, report);
    }
}

// Usage
const reporter = new TestReporter();

reporter.startTest('Login Test', function() {
    reporter.logStep('Navigate to login page', function() {
        reporter.logStep('Enter credentials', function() {
            reporter.logStep('Click login button', function() {
                reporter.endTest('passed', function() {
                    console.log('Login test completed');
                });
            });
        });
    });
});

// Flattened with named functions
function step1() {
    reporter.logStep('Open application', step2);
}

function step2() {
    reporter.logStep('Verify homepage', step3);
}

function step3() {
    reporter.endTest('passed', function() {
        reporter.generateReport(function(error, report) {
            if (!error) {
                console.log('Full report:', report);
            }
        });
    });
}

reporter.startTest('Homepage Test', step1);
```

### Example 5: Wait Helper with Callbacks

```javascript
class WaitHelper {
    waitFor(condition, timeout, callback) {
        const startTime = Date.now();
        const interval = 100;
        
        const checkCondition = () => {
            const elapsed = Date.now() - startTime;
            
            if (elapsed >= timeout) {
                callback(new Error(`Timeout after ${timeout}ms`), false);
                return;
            }
            
            const result = condition();
            
            if (result) {
                callback(null, true);
            } else {
                setTimeout(checkCondition, interval);
            }
        };
        
        checkCondition();
    }
    
    waitForElement(selector, timeout, callback) {
        console.log(`Waiting for element: ${selector}`);
        
        this.waitFor(
            () => document.querySelector(selector) !== null,
            timeout,
            (error, found) => {
                if (error) {
                    callback(error, null);
                } else {
                    const element = document.querySelector(selector);
                    console.log(`Element found: ${selector}`);
                    callback(null, element);
                }
            }
        );
    }
    
    waitForText(text, timeout, callback) {
        console.log(`Waiting for text: "${text}"`);
        
        this.waitFor(
            () => document.body.textContent.includes(text),
            timeout,
            (error, found) => {
                if (error) {
                    callback(error, null);
                } else {
                    console.log(`Text found: "${text}"`);
                    callback(null, true);
                }
            }
        );
    }
    
    sleep(duration, callback) {
        console.log(`Sleeping for ${duration}ms`);
        setTimeout(() => {
            console.log('Sleep completed');
            callback();
        }, duration);
    }
}

// Usage
const waiter = new WaitHelper();

waiter.waitForElement('#myButton', 5000, function(error, element) {
    if (error) {
        console.error('Element not found:', error.message);
    } else {
        console.log('Found element:', element);
        element.click();
    }
});

waiter.waitForText('Success', 3000, function(error, found) {
    if (error) {
        console.error('Text not found:', error.message);
    } else {
        console.log('Success message appeared!');
    }
});

waiter.sleep(1000, function() {
    console.log('Continuing after sleep');
});
```

---

## Interview Questions & Answers

### Q1: What is a callback function in JavaScript?

**Answer**: A callback function is a function passed as an argument to another function, which is then executed at a later time or after a specific event occurs. Callbacks are fundamental to asynchronous programming in JavaScript and enable functions to be more flexible and reusable.

```javascript
// Simple callback example
function greet(name, callback) {
    console.log('Hello, ' + name);
    callback();
}

greet('John', function() {
    console.log('Callback executed!');
});
```

---

### Q2: What is the difference between synchronous and asynchronous callbacks?

**Answer**: 
- **Synchronous callbacks** execute immediately during the function execution and block further code execution until complete.
- **Asynchronous callbacks** execute later, typically after an I/O operation or timer completes, and don't block code execution.

```javascript
// Synchronous callback
const numbers = [1, 2, 3];
numbers.forEach(num => console.log(num)); // Executes immediately
console.log('Done'); // Waits for forEach to complete

// Asynchronous callback
setTimeout(() => {
    console.log('Async callback');
}, 1000);
console.log('Done'); // Executes before setTimeout callback
```

---

### Q3: What is callback hell and how can you avoid it?

**Answer**: Callback hell (or "Pyramid of Doom") occurs when callbacks are nested multiple levels deep, making code difficult to read and maintain.

**Solutions**:
1. Use named functions instead of anonymous functions
2. Use Promises with `.then()` chains
3. Use async/await (modern best practice)
4. Modularize code into smaller functions

```javascript
// Callback hell
getData(function(a) {
    getMoreData(a, function(b) {
        getMoreData(b, function(c) {
            console.log(c); // Hard to read
        });
    });
});

// Solution with Promises
getData()
    .then(a => getMoreData(a))
    .then(b => getMoreData(b))
    .then(c => console.log(c));

// Solution with async/await
async function fetchData() {
    const a = await getData();
    const b = await getMoreData(a);
    const c = await getMoreData(b);
    console.log(c);
}
```

---

### Q4: What is the error-first callback pattern?

**Answer**: The error-first callback pattern (popularized by Node.js) is a convention where the first parameter of a callback is reserved for an error object, and subsequent parameters are for successful results. If there's no error, the first parameter is `null`.

```javascript
function readFile(filename, callback) {
    // Simulated file read
    if (!filename) {
        callback(new Error('Filename required'), null);
    } else {
        callback(null, 'File contents');
    }
}

readFile('test.txt', function(error, data) {
    if (error) {
        console.error('Error:', error.message);
        return;
    }
    console.log('Data:', data);
});
```

---

### Q5: How do callbacks enable asynchronous programming?

**Answer**: Callbacks enable asynchronous programming by allowing functions to execute code after an operation completes without blocking the main thread. The callback is registered and executed later when the async operation finishes.

```javascript
// Without callback - blocks execution
function fetchDataSync() {
    // This would block for 2 seconds
    const data = waitTwoSeconds(); // Hypothetical blocking function
    return data;
}

// With callback - non-blocking
function fetchDataAsync(callback) {
    setTimeout(() => {
        const data = { id: 1, name: 'Data' };
        callback(data); // Execute when ready
    }, 2000);
}

console.log('Start');
fetchDataAsync(function(data) {
    console.log('Data:', data); // Executes after 2 seconds
});
console.log('End'); // Executes immediately

// Output:
// Start
// End
// (2 seconds later) Data: { id: 1, name: 'Data' }
```

---

### Q6: Can you explain higher-order functions and their relationship to callbacks?

**Answer**: A higher-order function is a function that either accepts another function as an argument or returns a function. Callbacks are functions passed to higher-order functions.

```javascript
// Higher-order function that accepts a callback
function processArray(arr, callback) {
    const results = [];
    for (const item of arr) {
        results.push(callback(item));
    }
    return results;
}

// Using the higher-order function with different callbacks
const numbers = [1, 2, 3, 4];

const squared = processArray(numbers, num => num ** 2);
console.log(squared); // [1, 4, 9, 16]

const doubled = processArray(numbers, num => num * 2);
console.log(doubled); // [2, 4, 6, 8]

// Higher-order function that returns a function
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

---

### Q7: What are some common use cases for callbacks in test automation?

**Answer**: Common use cases in test automation include:

1. **Async operations**: Waiting for elements, API responses
2. **Event handling**: Click events, page loads
3. **Retry logic**: Retrying failed operations
4. **Test lifecycle**: Before/after hooks
5. **Reporting**: Collecting and formatting test results
6. **Data generation**: Creating test data asynchronously

```javascript
// Test automation example
class TestRunner {
    beforeEach(callback) {
        console.log('Setup test');
        callback();
    }
    
    test(testName, testCallback) {
        this.beforeEach(() => {
            console.log(`Running: ${testName}`);
            testCallback((error) => {
                if (error) {
                    console.log(`❌ ${testName} failed`);
                } else {
                    console.log(`✅ ${testName} passed`);
                }
            });
        });
    }
}

const runner = new TestRunner();

runner.test('Login Test', function(done) {
    // Simulate async test
    setTimeout(() => {
        const success = true;
        done(success ? null : new Error('Login failed'));
    }, 1000);
});
```

---

### Q8: How do you handle errors in callbacks?

**Answer**: There are several approaches to error handling in callbacks:

1. **Error-first pattern**: First parameter is error
2. **Try-catch blocks**: For synchronous code within callbacks
3. **Separate callbacks**: One for success, one for error
4. **Error parameter checking**: Always check for errors before proceeding

```javascript
// Method 1: Error-first pattern
function operation1(callback) {
    const error = null; // or new Error('Failed')
    const result = 'Success';
    callback(error, result);
}

operation1((error, result) => {
    if (error) {
        console.error('Error:', error);
        return;
    }
    console.log('Result:', result);
});

// Method 2: Try-catch
function operation2(callback) {
    try {
        const result = riskyOperation();
        callback(null, result);
    } catch (error) {
        callback(error, null);
    }
}

// Method 3: Separate callbacks
function operation3(successCallback, errorCallback) {
    const success = true;
    if (success) {
        successCallback('Data');
    } else {
        errorCallback(new Error('Failed'));
    }
}

operation3(
    (data) => console.log('Success:', data),
    (error) => console.error('Error:', error)
);
```

---

### Q9: What is the difference between callbacks and Promises?

**Answer**:

| Feature | Callbacks | Promises |
|---------|-----------|----------|
| **Syntax** | Function as parameter | `.then()` and `.catch()` chains |
| **Readability** | Can lead to callback hell | More readable, chainable |
| **Error Handling** | Manual error checking | Built-in `.catch()` |
| **Composition** | Difficult to compose | Easy with `.then()` chains |
| **Return Values** | Not returned, passed to callback | Returns Promise object |

```javascript
// Using callbacks
function fetchDataCallback(callback) {
    setTimeout(() => {
        callback(null, 'Data');
    }, 1000);
}

fetchDataCallback((error, data) => {
    if (error) {
        console.error(error);
    } else {
        console.log(data);
    }
});

// Using Promises
function fetchDataPromise() {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            resolve('Data');
        }, 1000);
    });
}

fetchDataPromise()
    .then(data => console.log(data))
    .catch(error => console.error(error));

// Or with async/await
async function getData() {
    try {
        const data = await fetchDataPromise();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}
```

---

### Q10: How do you ensure a callback is only executed once?

**Answer**: Use a flag or wrapper function to track if the callback has been called.

```javascript
// Method 1: Manual flag
function once(callback) {
    let called = false;
    
    return function(...args) {
        if (!called) {
            called = true;
            callback.apply(this, args);
        }
    };
}

const onceCallback = once(function(message) {
    console.log('Message:', message);
});

onceCallback('First'); // Message: First
onceCallback('Second'); // (nothing happens)

// Method 2: Built-in with closure
function doSomethingOnce(callback) {
    let executed = false;
    
    return function() {
        if (!executed) {
            executed = true;
            callback();
        } else {
            console.log('Already executed');
        }
    };
}

const myCallback = doSomethingOnce(() => {
    console.log('Executing task');
});

myCallback(); // Executing task
myCallback(); // Already executed
```

---

### Q11: How would you convert a callback-based function to a Promise?

**Answer**: Wrap the callback-based function in a new Promise, calling `resolve` on success and `reject` on error.

```javascript
// Original callback-based function
function fetchDataCallback(url, callback) {
    setTimeout(() => {
        if (url) {
            callback(null, { data: 'Result' });
        } else {
            callback(new Error('URL required'), null);
        }
    }, 1000);
}

// Convert to Promise
function fetchDataPromise(url) {
    return new Promise((resolve, reject) => {
        fetchDataCallback(url, (error, data) => {
            if (error) {
                reject(error);
            } else {
                resolve(data);
            }
        });
    });
}

// Usage
fetchDataPromise('https://api.example.com')
    .then(data => console.log(data))
    .catch(error => console.error(error));

// Or with async/await
async function getData() {
    try {
        const data = await fetchDataPromise('https://api.example.com');
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

// Generic utility function
function promisify(callbackFunction) {
    return function(...args) {
        return new Promise((resolve, reject) => {
            callbackFunction(...args, (error, data) => {
                if (error) {
                    reject(error);
                } else {
                    resolve(data);
                }
            });
        });
    };
}

// Usage
const fetchDataPromisified = promisify(fetchDataCallback);
fetchDataPromisified('https://api.example.com')
    .then(data => console.log(data));
```

---

### Q12: What is the `this` keyword behavior in callbacks?

**Answer**: The value of `this` inside a callback depends on how the callback is invoked. Regular functions have their own `this`, while arrow functions inherit `this` from their enclosing scope.

```javascript
const obj = {
    name: 'Test Object',
    
    // Regular function callback
    methodWithRegular: function() {
        setTimeout(function() {
            console.log(this.name); // undefined (this refers to global/window)
        }, 100);
    },
    
    // Arrow function callback
    methodWithArrow: function() {
        setTimeout(() => {
            console.log(this.name); // "Test Object" (inherits this from methodWithArrow)
        }, 100);
    },
    
    // Bind solution
    methodWithBind: function() {
        setTimeout(function() {
            console.log(this.name); // "Test Object" (this is bound)
        }.bind(this), 100);
    },
    
    // Self/that pattern
    methodWithSelf: function() {
        const self = this;
        setTimeout(function() {
            console.log(self.name); // "Test Object" (using closure)
        }, 100);
    }
};

obj.methodWithRegular(); // undefined
obj.methodWithArrow();   // Test Object
obj.methodWithBind();    // Test Object
obj.methodWithSelf();    // Test Object
```

---

## Best Practices

### ✅ DO

**1. Use error-first callbacks**
```javascript
// Good
function operation(callback) {
    if (error) {
        callback(new Error('Failed'), null);
    } else {
        callback(null, result);
    }
}
```

**2. Always check for errors**
```javascript
// Good
operation((error, data) => {
    if (error) {
        console.error('Error:', error);
        return;
    }
    // Process data
});
```

**3. Use named functions for complex callbacks**
```javascript
// Good - readable
function handleSuccess(data) {
    console.log('Success:', data);
}

function handleError(error) {
    console.error('Error:', error);
}

asyncOperation(handleSuccess, handleError);
```

**4. Keep callbacks focused and single-purpose**
```javascript
// Good
function processData(callback) {
    const data = fetchData();
    callback(data);
}

function validateData(callback) {
    const isValid = validate();
    callback(isValid);
}
```

**5. Document callback parameters**
```javascript
/**
 * Fetches user data from API
 * @param {number} userId - The user ID
 * @param {function(Error, Object)} callback - Callback with error and user data
 */
function getUserData(userId, callback) {
    // Implementation
}
```

---

### ❌ DON'T

**1. Don't create deeply nested callbacks**
```javascript
// Bad - callback hell
operation1(function(result1) {
    operation2(result1, function(result2) {
        operation3(result2, function(result3) {
            // Too deep!
        });
    });
});

// Good - use Promises or async/await
async function operations() {
    const result1 = await operation1();
    const result2 = await operation2(result1);
    const result3 = await operation3(result2);
}
```

**2. Don't forget to handle errors**
```javascript
// Bad
operation((error, data) => {
    console.log(data); // What if error exists?
});

// Good
operation((error, data) => {
    if (error) {
        console.error(error);
        return;
    }
    console.log(data);
});
```

**3. Don't call callbacks multiple times**
```javascript
// Bad
function operation(callback) {
    callback('First call');
    callback('Second call'); // Wrong!
}

// Good
function operation(callback) {
    let called = false;
    if (!called) {
        called = true;
        callback('Only call');
    }
}
```

**4. Don't block the event loop with synchronous operations**
```javascript
// Bad - blocks event loop
function processLargeArray(arr, callback) {
    for (let i = 0; i < arr.length; i++) {
        // Long synchronous operation
    }
    callback();
}

// Good - allow event loop to breathe
function processLargeArray(arr, callback) {
    let index = 0;
    function processNext() {
        if (index < arr.length) {
            // Process item
            index++;
            setImmediate(processNext);
        } else {
            callback();
        }
    }
    processNext();
}
```

**5. Don't mix callbacks and Promises**
```javascript
// Bad - confusing
function getData(callback) {
    return new Promise((resolve) => {
        // Don't mix!
        callback(data);
        resolve(data);
    });
}

// Good - choose one approach
function getDataCallback(callback) {
    callback(null, data);
}

function getDataPromise() {
    return Promise.resolve(data);
}
```

---

## Summary

### Key Takeaways

1. ✅ Callbacks are functions passed as arguments to other functions
2. ✅ Enable asynchronous programming and event-driven code
3. ✅ Can be synchronous or asynchronous
4. ✅ Error-first callback pattern: `callback(error, data)`
5. ✅ Callback hell can be avoided with named functions, Promises, or async/await
6. ✅ Common in array methods, event handlers, and async operations
7. ✅ Higher-order functions accept or return functions
8. ✅ Always handle errors properly
9. ✅ Modern alternatives: Promises and async/await
10. ✅ Essential for test automation frameworks

### When to Use Callbacks

- ✅ Simple async operations
- ✅ Event handlers
- ✅ Array manipulation methods
- ✅ Custom higher-order functions
- ✅ Legacy code compatibility

### When to Avoid Callbacks

- ❌ Complex async flows (use Promises/async-await)
- ❌ Deep nesting scenarios
- ❌ Need for better error handling
- ❌ Sequential async operations
- ❌ Modern codebases (prefer Promises)

---

## Migration Path

```
EVOLUTION OF ASYNC JAVASCRIPT

1. Callbacks (Traditional)
   ↓
2. Promises (ES6/2015)
   ↓
3. Async/Await (ES2017)

All three are valid, but async/await is preferred for new code.
Callbacks are still used in many libraries and legacy code.
```

### Same Operation - Three Approaches

```javascript
// 1. Callbacks
function fetchDataCallback(callback) {
    setTimeout(() => {
        callback(null, 'Data');
    }, 1000);
}

fetchDataCallback((error, data) => {
    if (error) console.error(error);
    else console.log(data);
});

// 2. Promises
function fetchDataPromise() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve('Data');
        }, 1000);
    });
}

fetchDataPromise()
    .then(data => console.log(data))
    .catch(error => console.error(error));

// 3. Async/Await
async function fetchDataAsync() {
    try {
        const data = await fetchDataPromise();
        console.log(data);
    } catch (error) {
        console.error(error);
    }
}

fetchDataAsync();
```

---

## Additional Resources

- [MDN - Callback function](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)
- [MDN - Asynchronous JavaScript](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous)
- [JavaScript.info - Callbacks](https://javascript.info/callbacks)
- [Node.js - Error-first callbacks](https://nodejs.org/en/knowledge/errors/what-are-the-error-conventions/)

---

**Created for Interview Preparation**  
*Last Updated: January 2026*

**Note**: While callbacks are fundamental to JavaScript, modern development favors Promises and async/await for better readability and error handling. However, understanding callbacks is crucial as they're still widely used in many libraries, event handlers, and legacy codebases. In test automation, you'll encounter callbacks in testing frameworks, page object models, and async operations.