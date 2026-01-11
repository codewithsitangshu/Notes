# JavaScript Inheritance

## Table of Contents
1. [What is Inheritance in JavaScript?](#what-is-inheritance-in-javascript)
2. [Types of Inheritance](#types-of-inheritance)
3. [Is Multi-Level Inheritance Possible?](#is-multi-level-inheritance-possible)
4. [Is Multiple Inheritance Possible?](#is-multiple-inheritance-possible)
5. [Prototypal Inheritance](#prototypal-inheritance)
6. [Inheritance Best Practices](#inheritance-best-practices)
7. [Real-World Test Automation Examples](#real-world-test-automation-examples)

---

## What is Inheritance in JavaScript?

**Inheritance** is an Object-Oriented Programming (OOP) concept where one class (child/subclass) can inherit properties and methods from another class (parent/superclass). This promotes **code reusability** and establishes a hierarchical relationship between classes.

### Key Concepts

- **Parent Class (Superclass/Base Class)**: The class being inherited from
- **Child Class (Subclass/Derived Class)**: The class that inherits from the parent
- **`extends` keyword**: Used to create a child class
- **`super` keyword**: Used to call parent class constructor and methods

### Basic Syntax

```javascript
// Parent class
class ParentClass {
    constructor(property) {
        this.property = property;
    }
    
    parentMethod() {
        console.log('Method from parent');
    }
}

// Child class inherits from Parent
class ChildClass extends ParentClass {
    constructor(property, childProperty) {
        super(property);  // Call parent constructor
        this.childProperty = childProperty;
    }
    
    childMethod() {
        console.log('Method from child');
    }
}
```

### Simple Example

```javascript
// Parent class
class Animal {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    
    eat() {
        console.log(`${this.name} is eating`);
    }
    
    sleep() {
        console.log(`${this.name} is sleeping`);
    }
    
    getInfo() {
        return `${this.name} is ${this.age} years old`;
    }
}

// Child class inherits from Animal
class Dog extends Animal {
    constructor(name, age, breed) {
        super(name, age);  // Call parent constructor
        this.breed = breed;
    }
    
    bark() {
        console.log(`${this.name} says: Woof! Woof!`);
    }
    
    // Override parent method
    getInfo() {
        return `${super.getInfo()} and is a ${this.breed}`;
    }
}

// Usage
const dog = new Dog('Buddy', 3, 'Golden Retriever');
dog.eat();        // Inherited: Buddy is eating
dog.sleep();      // Inherited: Buddy is sleeping
dog.bark();       // Own method: Buddy says: Woof! Woof!
console.log(dog.getInfo());  // Overridden: Buddy is 3 years old and is a Golden Retriever
```

### Benefits of Inheritance

1. ✅ **Code Reusability** - Avoid duplicating code
2. ✅ **Maintainability** - Changes in parent affect all children
3. ✅ **Logical Hierarchy** - Represents real-world relationships
4. ✅ **Extensibility** - Easy to add new features
5. ✅ **Polymorphism** - Child classes can override parent methods

---

## Types of Inheritance

### 1. Single Inheritance

One child class inherits from one parent class.

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
    
    displayName() {
        console.log(`Parent: ${this.name}`);
    }
}

class Child extends Parent {
    constructor(name, age) {
        super(name);
        this.age = age;
    }
    
    displayAge() {
        console.log(`Age: ${this.age}`);
    }
}

const child = new Child('John', 25);
child.displayName();  // Parent: John (inherited)
child.displayAge();   // Age: 25 (own method)
```

### 2. Multi-Level Inheritance

A child class becomes a parent for another class (inheritance chain).

```javascript
// Grandparent class
class Vehicle {
    constructor(brand) {
        this.brand = brand;
    }
    
    start() {
        console.log(`${this.brand} vehicle is starting`);
    }
}

// Parent class (inherits from Vehicle)
class Car extends Vehicle {
    constructor(brand, model) {
        super(brand);
        this.model = model;
    }
    
    drive() {
        console.log(`${this.brand} ${this.model} is driving`);
    }
}

// Child class (inherits from Car)
class ElectricCar extends Car {
    constructor(brand, model, batteryCapacity) {
        super(brand, model);
        this.batteryCapacity = batteryCapacity;
    }
    
    charge() {
        console.log(`Charging ${this.brand} ${this.model} with ${this.batteryCapacity}kWh battery`);
    }
}

const tesla = new ElectricCar('Tesla', 'Model 3', 75);
tesla.start();   // Inherited from Vehicle: Tesla vehicle is starting
tesla.drive();   // Inherited from Car: Tesla Model 3 is driving
tesla.charge();  // Own method: Charging Tesla Model 3 with 75kWh battery
```

### 3. Hierarchical Inheritance

Multiple child classes inherit from one parent class.

```javascript
class Shape {
    constructor(color) {
        this.color = color;
    }
    
    displayColor() {
        console.log(`Color: ${this.color}`);
    }
}

class Circle extends Shape {
    constructor(color, radius) {
        super(color);
        this.radius = radius;
    }
    
    area() {
        return Math.PI * this.radius * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(color, width, height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    area() {
        return this.width * this.height;
    }
}

class Triangle extends Shape {
    constructor(color, base, height) {
        super(color);
        this.base = base;
        this.height = height;
    }
    
    area() {
        return 0.5 * this.base * this.height;
    }
}

const circle = new Circle('red', 5);
const rectangle = new Rectangle('blue', 10, 20);
const triangle = new Triangle('green', 8, 6);

circle.displayColor();     // Inherited: Color: red
rectangle.displayColor();  // Inherited: Color: blue
triangle.displayColor();   // Inherited: Color: green

console.log(circle.area());     // 78.54
console.log(rectangle.area());  // 200
console.log(triangle.area());   // 24
```

---

## Is Multi-Level Inheritance Possible?

### ✅ YES - Multi-Level Inheritance is FULLY Supported

JavaScript supports multi-level inheritance where a class can inherit from another class, which in turn inherits from another class, creating an inheritance chain.

### Detailed Example

```javascript
// Level 1: Grandparent
class BasePage {
    constructor(page) {
        this.page = page;
        this.timeout = 30000;
    }
    
    async navigate(url) {
        await this.page.goto(url, { timeout: this.timeout });
        console.log(`Navigated to: ${url}`);
    }
    
    async getTitle() {
        return await this.page.title();
    }
    
    async takeScreenshot(filename) {
        await this.page.screenshot({ path: filename });
        console.log(`Screenshot saved: ${filename}`);
    }
}

// Level 2: Parent (inherits from BasePage)
class FormPage extends BasePage {
    constructor(page, formSelector) {
        super(page);
        this.formSelector = formSelector;
    }
    
    async fillField(selector, value) {
        await this.page.fill(selector, value);
        console.log(`Filled ${selector} with: ${value}`);
    }
    
    async submitForm() {
        await this.page.click(`${this.formSelector} button[type="submit"]`);
        console.log('Form submitted');
    }
    
    async getFieldValue(selector) {
        return await this.page.inputValue(selector);
    }
}

// Level 3: Child (inherits from FormPage)
class LoginPage extends FormPage {
    constructor(page) {
        super(page, '#login-form');
        this.url = 'https://example.com/login';
        this.usernameField = '#username';
        this.passwordField = '#password';
        this.loginButton = '#login-btn';
    }
    
    async login(username, password) {
        await this.navigate(this.url);           // From BasePage (Level 1)
        await this.fillField(this.usernameField, username);  // From FormPage (Level 2)
        await this.fillField(this.passwordField, password);  // From FormPage (Level 2)
        await this.page.click(this.loginButton);
        console.log(`Login attempt with username: ${username}`);
    }
    
    async getLoginTitle() {
        return await this.getTitle();            // From BasePage (Level 1)
    }
}

// Usage - Child has access to all parent and grandparent methods
const loginPage = new LoginPage(page);
await loginPage.login('testuser', 'password123');
await loginPage.takeScreenshot('login.png');  // From BasePage
const title = await loginPage.getLoginTitle(); // From BasePage
```

### Multi-Level Inheritance Chain

```javascript
// 5 Levels of Inheritance
class Level1 {
    method1() { return 'Level 1'; }
}

class Level2 extends Level1 {
    method2() { return 'Level 2'; }
}

class Level3 extends Level2 {
    method3() { return 'Level 3'; }
}

class Level4 extends Level3 {
    method4() { return 'Level 4'; }
}

class Level5 extends Level4 {
    method5() { return 'Level 5'; }
}

const obj = new Level5();
console.log(obj.method1());  // Level 1 (from Level1)
console.log(obj.method2());  // Level 2 (from Level2)
console.log(obj.method3());  // Level 3 (from Level3)
console.log(obj.method4());  // Level 4 (from Level4)
console.log(obj.method5());  // Level 5 (own method)
```

### Checking Inheritance Chain

```javascript
class Grandparent {}
class Parent extends Grandparent {}
class Child extends Parent {}

const child = new Child();

// instanceof checks entire chain
console.log(child instanceof Child);        // true
console.log(child instanceof Parent);       // true
console.log(child instanceof Grandparent);  // true
console.log(child instanceof Object);       // true

// Check prototype chain
console.log(Child.prototype instanceof Parent);       // true
console.log(Child.prototype instanceof Grandparent);  // true
console.log(Parent.prototype instanceof Grandparent); // true
```

### Best Practices for Multi-Level Inheritance

✅ **Keep inheritance depth reasonable** (3-4 levels max)
✅ **Each level should add meaningful functionality**
✅ **Use composition when inheritance chain gets too deep**
✅ **Document the inheritance hierarchy clearly**
❌ **Avoid deep inheritance chains** (6+ levels)

---

## Is Multiple Inheritance Possible?

### ❌ NO - JavaScript Does NOT Support Multiple Inheritance Directly

JavaScript does **NOT** allow a class to inherit from multiple classes simultaneously (like C++ or Python). A class can only `extend` one parent class.

### What Doesn't Work

```javascript
// ❌ This will cause a SyntaxError
class Parent1 {
    method1() { return 'Parent1'; }
}

class Parent2 {
    method2() { return 'Parent2'; }
}

// ❌ SyntaxError: Classes can only extend a single class
class Child extends Parent1, Parent2 {
    // This won't work!
}
```

### Why Multiple Inheritance is Not Supported

1. **Diamond Problem** - Ambiguity when multiple parents have the same method
2. **Complexity** - Makes inheritance chain difficult to understand and maintain
3. **Design Decision** - JavaScript's prototype chain is linear (single inheritance)

### ✅ Alternatives to Achieve Multiple Inheritance Behavior

While JavaScript doesn't support true multiple inheritance, there are several patterns to achieve similar functionality:

---

### Method 1: Object Composition (RECOMMENDED)

Instead of inheriting from multiple classes, compose objects with multiple capabilities.

```javascript
// Separate capabilities
const CanLogin = {
    login(username, password) {
        console.log(`${username} logged in with password: ${password}`);
        this.isLoggedIn = true;
    },
    
    logout() {
        console.log('User logged out');
        this.isLoggedIn = false;
    }
};

const CanNavigate = {
    navigate(url) {
        console.log(`Navigating to: ${url}`);
        this.currentUrl = url;
    },
    
    goBack() {
        console.log('Going back');
    }
};

const CanTakeScreenshot = {
    screenshot(filename) {
        console.log(`Screenshot saved: ${filename}`);
    }
};

// Compose a class with multiple capabilities
class TestPage {
    constructor(page) {
        this.page = page;
        this.isLoggedIn = false;
        
        // Compose multiple behaviors
        Object.assign(this, CanLogin, CanNavigate, CanTakeScreenshot);
    }
}

// Usage
const testPage = new TestPage(page);
testPage.login('user', 'pass');        // From CanLogin
testPage.navigate('https://test.com'); // From CanNavigate
testPage.screenshot('test.png');       // From CanTakeScreenshot
```

---

### Method 2: Mixins (POPULAR PATTERN)

Mixins allow you to "mix in" functionality from multiple sources.

```javascript
// Mixin 1: Logging functionality
const LoggerMixin = (Base) => class extends Base {
    log(message) {
        console.log(`[LOG] ${new Date().toISOString()} - ${message}`);
    }
    
    error(message) {
        console.error(`[ERROR] ${new Date().toISOString()} - ${message}`);
    }
    
    warn(message) {
        console.warn(`[WARN] ${new Date().toISOString()} - ${message}`);
    }
};

// Mixin 2: Validation functionality
const ValidationMixin = (Base) => class extends Base {
    validateEmail(email) {
        const regex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
        return regex.test(email);
    }
    
    validatePassword(password) {
        return password.length >= 8;
    }
    
    validateUsername(username) {
        return username.length >= 3 && username.length <= 20;
    }
};

// Mixin 3: Retry functionality
const RetryMixin = (Base) => class extends Base {
    async retry(operation, maxAttempts = 3, delay = 1000) {
        for (let attempt = 1; attempt <= maxAttempts; attempt++) {
            try {
                return await operation();
            } catch (error) {
                if (attempt === maxAttempts) throw error;
                this.warn(`Attempt ${attempt} failed, retrying...`);
                await this.sleep(delay);
            }
        }
    }
    
    sleep(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
};

// Base class
class TestRunner {
    constructor(name) {
        this.name = name;
    }
    
    run() {
        console.log(`Running test: ${this.name}`);
    }
}

// Apply multiple mixins to create enhanced class
class EnhancedTestRunner extends RetryMixin(ValidationMixin(LoggerMixin(TestRunner))) {
    async runWithRetry(testFunction) {
        this.log(`Starting test: ${this.name}`);
        
        await this.retry(async () => {
            await testFunction();
            this.log('Test passed!');
        });
    }
    
    validateTestData(data) {
        if (!this.validateEmail(data.email)) {
            this.error('Invalid email');
            return false;
        }
        if (!this.validatePassword(data.password)) {
            this.error('Invalid password');
            return false;
        }
        this.log('Test data validated successfully');
        return true;
    }
}

// Usage - Has functionality from all mixins!
const runner = new EnhancedTestRunner('Login Test');
runner.log('Test initialized');                        // From LoggerMixin
runner.validateEmail('test@example.com');              // From ValidationMixin
await runner.runWithRetry(async () => {                // From RetryMixin
    // Test logic here
});
```

---

### Method 3: Class Factory Pattern

Create classes dynamically with multiple behaviors.

```javascript
// Behavior 1
class Clickable {
    async click(selector) {
        console.log(`Clicking: ${selector}`);
        await this.page.click(selector);
    }
    
    async doubleClick(selector) {
        console.log(`Double clicking: ${selector}`);
        await this.page.dblclick(selector);
    }
}

// Behavior 2
class Typable {
    async type(selector, text) {
        console.log(`Typing "${text}" into: ${selector}`);
        await this.page.type(selector, text);
    }
    
    async clear(selector) {
        console.log(`Clearing: ${selector}`);
        await this.page.fill(selector, '');
    }
}

// Behavior 3
class Waitable {
    async waitForElement(selector, timeout = 30000) {
        console.log(`Waiting for: ${selector}`);
        await this.page.waitForSelector(selector, { timeout });
    }
    
    async waitForText(text, timeout = 30000) {
        console.log(`Waiting for text: ${text}`);
        await this.page.waitForSelector(`text=${text}`, { timeout });
    }
}

// Factory function to combine behaviors
function createPageClass(...behaviors) {
    class Page {
        constructor(page) {
            this.page = page;
        }
    }
    
    // Copy methods from all behaviors
    behaviors.forEach(Behavior => {
        Object.getOwnPropertyNames(Behavior.prototype)
            .filter(name => name !== 'constructor')
            .forEach(name => {
                Page.prototype[name] = Behavior.prototype[name];
            });
    });
    
    return Page;
}

// Create class with multiple behaviors
const InteractivePage = createPageClass(Clickable, Typable, Waitable);

const page = new InteractivePage(browserPage);
await page.click('#button');              // From Clickable
await page.type('#input', 'test');        // From Typable
await page.waitForElement('#result');     // From Waitable
```

---

### Method 4: Aggregation Pattern

Aggregate multiple objects into one.

```javascript
class Logger {
    log(message) {
        console.log(`[LOG] ${message}`);
    }
}

class Validator {
    validate(data) {
        return data !== null && data !== undefined;
    }
}

class Reporter {
    report(status) {
        console.log(`Test Status: ${status}`);
    }
}

// Main class that aggregates others
class TestSuite {
    constructor() {
        this.logger = new Logger();
        this.validator = new Validator();
        this.reporter = new Reporter();
    }
    
    runTest(data) {
        this.logger.log('Starting test...');
        
        if (this.validator.validate(data)) {
            this.logger.log('Data is valid');
            this.reporter.report('PASSED');
        } else {
            this.logger.log('Data is invalid');
            this.reporter.report('FAILED');
        }
    }
}

// Usage
const suite = new TestSuite();
suite.runTest({ username: 'test' });
```

---

### Method 5: Interface Implementation (TypeScript-style)

JavaScript doesn't have interfaces, but we can simulate them:

```javascript
// Define "interfaces" as objects with methods
const IClickable = {
    click: function() { throw new Error('Must implement click()'); },
    doubleClick: function() { throw new Error('Must implement doubleClick()'); }
};

const ITypable = {
    type: function() { throw new Error('Must implement type()'); },
    clear: function() { throw new Error('Must implement clear()'); }
};

// Class implements multiple "interfaces"
class InteractivePage {
    constructor(page) {
        this.page = page;
        
        // Ensure all interface methods are implemented
        this.validateInterface(IClickable);
        this.validateInterface(ITypable);
    }
    
    validateInterface(interfaceObj) {
        Object.keys(interfaceObj).forEach(method => {
            if (typeof this[method] !== 'function') {
                throw new Error(`Must implement ${method}()`);
            }
        });
    }
    
    // Implement IClickable
    async click(selector) {
        await this.page.click(selector);
    }
    
    async doubleClick(selector) {
        await this.page.dblclick(selector);
    }
    
    // Implement ITypable
    async type(selector, text) {
        await this.page.type(selector, text);
    }
    
    async clear(selector) {
        await this.page.fill(selector, '');
    }
}
```

---

## Prototypal Inheritance

JavaScript uses **prototypal inheritance** under the hood. Understanding this is crucial for interviews.

### Prototype Chain

```javascript
class Animal {
    eat() {
        console.log('Eating...');
    }
}

class Dog extends Animal {
    bark() {
        console.log('Barking...');
    }
}

const dog = new Dog();

// Prototype chain:
// dog -> Dog.prototype -> Animal.prototype -> Object.prototype -> null

console.log(dog.__proto__ === Dog.prototype);                    // true
console.log(Dog.prototype.__proto__ === Animal.prototype);       // true
console.log(Animal.prototype.__proto__ === Object.prototype);    // true
console.log(Object.prototype.__proto__ === null);                // true
```

### Understanding Prototypes

```javascript
class Vehicle {
    constructor(brand) {
        this.brand = brand;
    }
    
    start() {
        console.log(`${this.brand} is starting`);
    }
}

class Car extends Vehicle {
    constructor(brand, model) {
        super(brand);
        this.model = model;
    }
}

const car = new Car('Toyota', 'Camry');

// Check prototype chain
console.log(car.hasOwnProperty('brand'));      // true (own property)
console.log(car.hasOwnProperty('model'));      // true (own property)
console.log(car.hasOwnProperty('start'));      // false (inherited from prototype)

// Method lookup in prototype chain
console.log('start' in car);                   // true (found in prototype)
console.log(car.start === Car.prototype.start); // false
console.log(car.start === Vehicle.prototype.start); // true
```

### Constructor vs Prototype Methods

```javascript
class TestCase {
    constructor(name) {
        this.name = name;
        
        // Instance method (created for each object)
        this.instanceMethod = function() {
            console.log('Instance method');
        };
    }
    
    // Prototype method (shared across all instances)
    prototypeMethod() {
        console.log('Prototype method');
    }
}

const test1 = new TestCase('Test 1');
const test2 = new TestCase('Test 2');

// Instance methods are different objects
console.log(test1.instanceMethod === test2.instanceMethod);  // false

// Prototype methods are the same object
console.log(test1.prototypeMethod === test2.prototypeMethod); // true
```

---

# Method Overriding

**Insert this section after "Prototypal Inheritance" and before "Inheritance Best Practices"**

---

## Method Overriding

**Method Overriding** is a fundamental OOP concept where a child class provides its own implementation of a method that already exists in the parent class. The child's version "overrides" the parent's version.

### Key Concepts

- Child class redefines a method from the parent class
- The overridden method in the child class has the **same name** and **signature** as the parent method
- When called on a child instance, the child's version executes (not the parent's)
- You can still call the parent's version using `super.methodName()`
- Enables **polymorphism** - different behavior for different classes

---

### Basic Method Overriding

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }
    
    speak() {
        return `${this.name} makes a sound`;
    }
    
    move() {
        return `${this.name} is moving`;
    }
}

class Dog extends Animal {
    // Override speak() method
    speak() {
        return `${this.name} barks: Woof! Woof!`;
    }
    
    // Override move() method
    move() {
        return `${this.name} runs on four legs`;
    }
}

class Cat extends Animal {
    // Override speak() method
    speak() {
        return `${this.name} meows: Meow! Meow!`;
    }
    
    // Override move() method
    move() {
        return `${this.name} walks gracefully`;
    }
}

// Usage
const animal = new Animal('Generic Animal');
const dog = new Dog('Buddy');
const cat = new Cat('Whiskers');

console.log(animal.speak()); // Generic Animal makes a sound
console.log(dog.speak());    // Buddy barks: Woof! Woof!
console.log(cat.speak());    // Whiskers meows: Meow! Meow!

console.log(animal.move());  // Generic Animal is moving
console.log(dog.move());     // Buddy runs on four legs
console.log(cat.move());     // Whiskers walks gracefully
```

---

### Method Overriding with `super`

You can call the parent's method from within the overridden method using `super.methodName()`.

```javascript
class Vehicle {
    constructor(brand) {
        this.brand = brand;
    }
    
    start() {
        return `${this.brand} engine is starting`;
    }
    
    getInfo() {
        return `Vehicle: ${this.brand}`;
    }
}

class Car extends Vehicle {
    constructor(brand, model) {
        super(brand);
        this.model = model;
    }
    
    // Override start() and extend parent functionality
    start() {
        const parentStart = super.start(); // Call parent method
        return `${parentStart}, car ${this.model} is ready`;
    }
    
    // Override getInfo() and add more details
    getInfo() {
        const parentInfo = super.getInfo(); // Call parent method
        return `${parentInfo}, Model: ${this.model}`;
    }
}

class ElectricCar extends Car {
    constructor(brand, model, batteryCapacity) {
        super(brand, model);
        this.batteryCapacity = batteryCapacity;
    }
    
    // Override start() from Car
    start() {
        const carStart = super.start(); // Call Car's start()
        return `${carStart}, charging ${this.batteryCapacity}kWh battery`;
    }
    
    // Override getInfo() from Car
    getInfo() {
        const carInfo = super.getInfo(); // Call Car's getInfo()
        return `${carInfo}, Battery: ${this.batteryCapacity}kWh`;
    }
}

// Usage
const vehicle = new Vehicle('Generic');
const car = new Car('Toyota', 'Camry');
const tesla = new ElectricCar('Tesla', 'Model 3', 75);

console.log(vehicle.start());
// Generic engine is starting

console.log(car.start());
// Generic engine is starting, car Camry is ready

console.log(tesla.start());
// Generic engine is starting, car Model 3 is ready, charging 75kWh battery

console.log(vehicle.getInfo()); // Vehicle: Generic
console.log(car.getInfo());     // Vehicle: Toyota, Model: Camry
console.log(tesla.getInfo());   // Vehicle: Tesla, Model: Model 3, Battery: 75kWh
```

---

### Constructor Overriding

Constructors can also be overridden. The child class constructor **must** call `super()` before accessing `this`.

```javascript
class User {
    constructor(username, email) {
        this.username = username;
        this.email = email;
        this.createdAt = new Date();
        console.log('User constructor called');
    }
    
    displayInfo() {
        return `User: ${this.username}, Email: ${this.email}`;
    }
}

class Admin extends User {
    constructor(username, email, permissions) {
        // Must call super() first!
        super(username, email);
        this.permissions = permissions;
        this.role = 'admin';
        console.log('Admin constructor called');
    }
    
    // Override displayInfo()
    displayInfo() {
        const userInfo = super.displayInfo();
        return `${userInfo}, Role: ${this.role}, Permissions: ${this.permissions.join(', ')}`;
    }
}

// Usage
const admin = new Admin('john_admin', 'john@example.com', ['read', 'write', 'delete']);
// Output:
// User constructor called
// Admin constructor called

console.log(admin.displayInfo());
// User: john_admin, Email: john@example.com, Role: admin, Permissions: read, write, delete
```

---

### Test Automation Examples

#### Example 1: Page Object Pattern with Method Overriding

```javascript
class BasePage {
    constructor(page) {
        this.page = page;
        this.timeout = 30000;
    }
    
    async navigate(url) {
        await this.page.goto(url, { timeout: this.timeout });
        console.log(`Navigated to: ${url}`);
    }
    
    async waitForPageLoad() {
        await this.page.waitForLoadState('networkidle');
        console.log('Base page loaded');
    }
    
    async getPageTitle() {
        return await this.page.title();
    }
}

class LoginPage extends BasePage {
    constructor(page) {
        super(page);
        this.url = 'https://example.com/login';
        this.usernameField = '#username';
        this.passwordField = '#password';
        this.loginButton = '#login-btn';
    }
    
    // Override navigate to use predefined URL
    async navigate() {
        await super.navigate(this.url); // Call parent navigate
        await this.waitForLoginForm();
    }
    
    // Override waitForPageLoad with login-specific logic
    async waitForPageLoad() {
        await super.waitForPageLoad(); // Call parent method
        await this.page.waitForSelector(this.loginButton);
        console.log('Login page fully loaded');
    }
    
    async waitForLoginForm() {
        await this.page.waitForSelector(this.usernameField);
        console.log('Login form is visible');
    }
    
    async login(username, password) {
        await this.page.fill(this.usernameField, username);
        await this.page.fill(this.passwordField, password);
        await this.page.click(this.loginButton);
    }
}

class SecureLoginPage extends LoginPage {
    constructor(page) {
        super(page);
        this.captchaField = '#captcha';
        this.twoFactorField = '#two-factor-code';
    }
    
    // Override waitForPageLoad with additional security checks
    async waitForPageLoad() {
        await super.waitForPageLoad(); // Call LoginPage's waitForPageLoad
        await this.page.waitForSelector(this.captchaField);
        console.log('Secure login page loaded with captcha');
    }
    
    // Override login to handle captcha and 2FA
    async login(username, password, captcha, twoFactorCode) {
        await this.page.fill(this.usernameField, username);
        await this.page.fill(this.passwordField, password);
        await this.page.fill(this.captchaField, captcha);
        await this.page.click(this.loginButton);
        
        // Handle 2FA if required
        if (twoFactorCode) {
            await this.page.waitForSelector(this.twoFactorField);
            await this.page.fill(this.twoFactorField, twoFactorCode);
            await this.page.click('#verify-2fa');
        }
    }
}

// Usage
const loginPage = new LoginPage(page);
await loginPage.navigate();
await loginPage.waitForPageLoad();
await loginPage.login('testuser', 'password123');

const secureLoginPage = new SecureLoginPage(page);
await secureLoginPage.navigate();
await secureLoginPage.waitForPageLoad();
await secureLoginPage.login('testuser', 'password123', 'ABC123', '654321');
```

#### Example 2: API Client with Method Overriding

```javascript
class APIClient {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.headers = {
            'Content-Type': 'application/json'
        };
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        console.log(`Making request to: ${url}`);
        
        const config = {
            ...options,
            headers: { ...this.headers, ...options.headers }
        };
        
        const response = await fetch(url, config);
        
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}: ${response.statusText}`);
        }
        
        return await response.json();
    }
    
    async get(endpoint) {
        return this.request(endpoint, { method: 'GET' });
    }
}

class AuthenticatedAPIClient extends APIClient {
    constructor(baseURL, token) {
        super(baseURL);
        this.token = token;
        this.headers['Authorization'] = `Bearer ${token}`;
    }
    
    // Override request to add token refresh logic
    async request(endpoint, options = {}) {
        try {
            return await super.request(endpoint, options);
        } catch (error) {
            // If unauthorized, try to refresh token
            if (error.message.includes('401')) {
                console.log('Token expired, refreshing...');
                await this.refreshToken();
                return await super.request(endpoint, options); // Retry
            }
            throw error;
        }
    }
    
    async refreshToken() {
        // Token refresh logic
        console.log('Token refreshed');
        this.headers['Authorization'] = `Bearer ${this.token}`;
    }
}

class RetryableAPIClient extends AuthenticatedAPIClient {
    constructor(baseURL, token, maxRetries = 3) {
        super(baseURL, token);
        this.maxRetries = maxRetries;
    }
    
    // Override request to add retry logic
    async request(endpoint, options = {}) {
        let lastError;
        
        for (let attempt = 1; attempt <= this.maxRetries; attempt++) {
            try {
                return await super.request(endpoint, options);
            } catch (error) {
                lastError = error;
                console.log(`Attempt ${attempt} failed: ${error.message}`);
                
                if (attempt < this.maxRetries) {
                    await this.wait(1000 * attempt); // Exponential backoff
                }
            }
        }
        
        throw lastError;
    }
    
    wait(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
}

// Usage
const basicClient = new APIClient('https://api.example.com');
const authClient = new AuthenticatedAPIClient('https://api.example.com', 'token123');
const retryClient = new RetryableAPIClient('https://api.example.com', 'token123', 5);

await basicClient.get('/users/1');     // Basic request
await authClient.get('/users/1');      // With auth + token refresh
await retryClient.get('/users/1');     // With auth + token refresh + retry
```

#### Example 3: Test Reporter with Method Overriding

```javascript
class BaseReporter {
    constructor(testName) {
        this.testName = testName;
        this.startTime = null;
        this.endTime = null;
    }
    
    start() {
        this.startTime = Date.now();
        console.log(`\n[TEST STARTED] ${this.testName}`);
    }
    
    end(status) {
        this.endTime = Date.now();
        const duration = this.endTime - this.startTime;
        console.log(`[TEST ${status}] ${this.testName} (${duration}ms)`);
    }
    
    log(message) {
        console.log(`  ${message}`);
    }
}

class DetailedReporter extends BaseReporter {
    constructor(testName) {
        super(testName);
        this.steps = [];
    }
    
    // Override start to add more details
    start() {
        super.start(); // Call parent start
        console.log(`  Test Suite: Automation Tests`);
        console.log(`  Environment: ${process.env.NODE_ENV || 'development'}`);
        console.log(`  Start Time: ${new Date(this.startTime).toISOString()}`);
    }
    
    // Override end to include step summary
    end(status) {
        super.end(status); // Call parent end
        console.log(`  Total Steps: ${this.steps.length}`);
        console.log(`  Steps Executed:`);
        this.steps.forEach((step, index) => {
            console.log(`    ${index + 1}. ${step}`);
        });
    }
    
    // Override log to track steps
    log(message) {
        super.log(message); // Call parent log
        this.steps.push(message);
    }
}

class HTMLReporter extends DetailedReporter {
    constructor(testName, outputFile) {
        super(testName);
        this.outputFile = outputFile;
        this.htmlContent = '';
    }
    
    // Override start to initialize HTML
    start() {
        super.start(); // Call parent start
        this.htmlContent += `<div class="test-report">`;
        this.htmlContent += `<h2>${this.testName}</h2>`;
        this.htmlContent += `<p>Started: ${new Date(this.startTime).toISOString()}</p>`;
    }
    
    // Override end to save HTML file
    end(status) {
        super.end(status); // Call parent end
        
        const duration = this.endTime - this.startTime;
        this.htmlContent += `<p>Status: <strong>${status}</strong></p>`;
        this.htmlContent += `<p>Duration: ${duration}ms</p>`;
        this.htmlContent += `<ul>`;
        this.steps.forEach(step => {
            this.htmlContent += `<li>${step}</li>`;
        });
        this.htmlContent += `</ul></div>`;
        
        console.log(`  HTML Report saved: ${this.outputFile}`);
        // In real scenario: fs.writeFileSync(this.outputFile, this.htmlContent);
    }
    
    // Override log to add HTML formatting
    log(message) {
        super.log(message); // Call parent log
        // HTML-specific logging handled in end()
    }
}

// Usage
const reporter1 = new BaseReporter('Login Test');
reporter1.start();
reporter1.log('Navigate to login page');
reporter1.log('Fill credentials');
reporter1.log('Click login button');
reporter1.end('PASSED');

const reporter2 = new DetailedReporter('Checkout Test');
reporter2.start();
reporter2.log('Add item to cart');
reporter2.log('Proceed to checkout');
reporter2.log('Fill payment details');
reporter2.end('PASSED');

const reporter3 = new HTMLReporter('Registration Test', 'report.html');
reporter3.start();
reporter3.log('Navigate to registration page');
reporter3.log('Fill user details');
reporter3.log('Submit form');
reporter3.end('FAILED');
```

---

### Method Overriding vs Method Overloading

**Important**: JavaScript does **NOT** support traditional method overloading (multiple methods with the same name but different parameters).

```javascript
class Calculator {
    // âŒ This is NOT method overloading
    add(a, b) {
        return a + b;
    }
    
    // This will REPLACE the previous add() method
    add(a, b, c) {
        return a + b + c;
    }
}

const calc = new Calculator();
console.log(calc.add(2, 3));     // NaN (c is undefined)
console.log(calc.add(2, 3, 4));  // 9

// âœ… Workaround: Use default parameters or rest parameters
class BetterCalculator {
    add(...numbers) {
        return numbers.reduce((sum, num) => sum + num, 0);
    }
}

const betterCalc = new BetterCalculator();
console.log(betterCalc.add(2, 3));        // 5
console.log(betterCalc.add(2, 3, 4));     // 9
console.log(betterCalc.add(1, 2, 3, 4));  // 10
```

---

### Common Pitfalls

#### Pitfall 1: Forgetting to call `super()`

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
}

class Child extends Parent {
    constructor(name, age) {
        // âŒ ERROR: Must call super() before accessing 'this'
        this.age = age; // ReferenceError!
        super(name);
    }
}

// âœ… Correct version
class CorrectChild extends Parent {
    constructor(name, age) {
        super(name);    // Call super first
        this.age = age; // Then use 'this'
    }
}
```

#### Pitfall 2: Infinite recursion

```javascript
class Parent {
    greet() {
        return 'Hello from Parent';
    }
}

class Child extends Parent {
    greet() {
        // âŒ Infinite loop - calls itself, not parent
        return this.greet() + ' and Child'; // Stack overflow!
    }
}

// âœ… Correct version
class CorrectChild extends Parent {
    greet() {
        return super.greet() + ' and Child'; // Calls parent method
    }
}
```

#### Pitfall 3: Overriding without understanding parent behavior

```javascript
class DataValidator {
    validate(data) {
        if (!data) {
            throw new Error('Data is required');
        }
        // More validation logic
        return true;
    }
}

class StrictValidator extends DataValidator {
    // âŒ Bad: Completely replaces parent validation
    validate(data) {
        if (data.length < 5) {
            throw new Error('Data too short');
        }
        return true;
    }
    // Lost the parent's null check!
}

// âœ… Good: Extends parent validation
class BetterStrictValidator extends DataValidator {
    validate(data) {
        super.validate(data); // Keep parent validation
        if (data.length < 5) {
            throw new Error('Data too short');
        }
        return true;
    }
}
```

---

### Interview Questions on Method Overriding

#### Q1: What is method overriding in JavaScript?

**Answer**: Method overriding is when a child class provides its own implementation of a method that already exists in the parent class. When you call the method on a child instance, the child's version executes instead of the parent's version. You can still access the parent's method using `super.methodName()`.

```javascript
class Parent {
    greet() { return 'Hello from Parent'; }
}

class Child extends Parent {
    greet() { return 'Hello from Child'; } // Overrides parent method
}

const child = new Child();
console.log(child.greet()); // "Hello from Child"
```

---

#### Q2: How do you call the parent's overridden method from a child class?

**Answer**: Use the `super` keyword followed by the method name: `super.methodName()`.

```javascript
class Parent {
    introduce() {
        return 'I am the parent';
    }
}

class Child extends Parent {
    introduce() {
        const parentIntro = super.introduce(); // Call parent method
        return `${parentIntro} and I am the child`;
    }
}

const child = new Child();
console.log(child.introduce());
// "I am the parent and I am the child"
```

---

#### Q3: Can you override the constructor in JavaScript?

**Answer**: Yes, you can override the constructor in a child class, but you **must** call `super()` before accessing `this`. The `super()` call invokes the parent class constructor.

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
}

class Child extends Parent {
    constructor(name, age) {
        super(name);    // Must call super() first
        this.age = age; // Then can use 'this'
    }
}
```

---

#### Q4: What's the difference between method overriding and method overloading?

**Answer**: 
- **Method Overriding**: Child class redefines a parent method (same name, same signature). JavaScript **fully supports** this.
- **Method Overloading**: Multiple methods with the same name but different parameters. JavaScript does **NOT support** traditional method overloading.

```javascript
// Method Overriding (Supported)
class Parent {
    greet() { return 'Hello'; }
}
class Child extends Parent {
    greet() { return 'Hi'; } // Overrides
}

// Method Overloading (NOT Supported)
class Calculator {
    add(a, b) { return a + b; }
    add(a, b, c) { return a + b + c; } // This REPLACES the previous add()
}

// Workaround using rest parameters
class BetterCalculator {
    add(...nums) { return nums.reduce((sum, n) => sum + n, 0); }
}
```

---

#### Q5: Can you override static methods in JavaScript?

**Answer**: Yes, static methods can be overridden in child classes. They work similarly to instance method overriding, but are called on the class itself, not on instances.

```javascript
class Parent {
    static getType() {
        return 'Parent Type';
    }
}

class Child extends Parent {
    static getType() {
        return 'Child Type'; // Overrides parent static method
    }
}

console.log(Parent.getType()); // "Parent Type"
console.log(Child.getType());  // "Child Type"

// Can also call parent static method
class AnotherChild extends Parent {
    static getType() {
        return super.getType() + ' Extended';
    }
}
console.log(AnotherChild.getType()); // "Parent Type Extended"
```

---

#### Q6: What happens if you don't override a method in a child class?

**Answer**: If a child class doesn't override a method, it inherits the parent's method and uses that implementation when called.

```javascript
class Animal {
    eat() {
        return 'Eating...';
    }
    
    sleep() {
        return 'Sleeping...';
    }
}

class Dog extends Animal {
    // Only override eat(), inherit sleep()
    eat() {
        return 'Dog is eating';
    }
}

const dog = new Dog();
console.log(dog.eat());   // "Dog is eating" (overridden)
console.log(dog.sleep()); // "Sleeping..." (inherited from Animal)
```

---

#### Q7: How does method overriding enable polymorphism?

**Answer**: Method overriding enables polymorphism by allowing different classes to respond to the same method call in different ways. This allows you to write code that works with the parent type but behaves differently based on the actual child type at runtime.

```javascript
class Shape {
    area() {
        return 0;
    }
}

class Circle extends Shape {
    constructor(radius) {
        super();
        this.radius = radius;
    }
    
    area() {
        return Math.PI * this.radius * this.radius;
    }
}

class Rectangle extends Shape {
    constructor(width, height) {
        super();
        this.width = width;
        this.height = height;
    }
    
    area() {
        return this.width * this.height;
    }
}

// Polymorphism in action
function printArea(shape) {
    console.log(`Area: ${shape.area()}`); // Works with any Shape
}

printArea(new Circle(5));         // Area: 78.54
printArea(new Rectangle(4, 6));   // Area: 24
```

---

#### Q8: In a test automation framework, when would you use method overriding?

**Answer**: Method overriding is commonly used in test automation for:

1. **Page Object Model**: Different pages override base page methods with page-specific behavior
2. **Custom Waits**: Override waitForPageLoad() with specific element checks
3. **Authentication**: Override login() for different authentication mechanisms (SSO, 2FA, etc.)
4. **Reporting**: Override report methods for different output formats (HTML, JSON, XML)
5. **API Clients**: Override request() to add retry logic, caching, or custom headers

```javascript
// Example: Different login mechanisms
class BasePage {
    async login(username, password) {
        await this.fillField('#username', username);
        await this.fillField('#password', password);
        await this.click('#login');
    }
}

class SSOLoginPage extends BasePage {
    async login(username, password) {
        await this.click('#sso-login'); // Different flow
        // SSO-specific logic
    }
}

class TwoFactorLoginPage extends BasePage {
    async login(username, password, code) {
        await super.login(username, password); // Use base login
        await this.fillField('#2fa-code', code);
        await this.click('#verify');
    }
}
```

---

#### Q9: What's the difference between overriding an instance method vs a static method?

**Answer**:
- **Instance methods** are overridden and called on object instances
- **Static methods** are overridden and called on the class itself
- Both use the same syntax with `super` to call parent methods

```javascript
class Parent {
    // Instance method
    instanceMethod() {
        return 'Parent instance';
    }
    
    // Static method
    static staticMethod() {
        return 'Parent static';
    }
}

class Child extends Parent {
    // Override instance method
    instanceMethod() {
        return 'Child instance';
    }
    
    // Override static method
    static staticMethod() {
        return 'Child static';
    }
}

const child = new Child();
console.log(child.instanceMethod());  // "Child instance"
console.log(Child.staticMethod());    // "Child static"

// Can't call static method on instance
// child.staticMethod(); // TypeError
```

---

#### Q10: Can you prevent a method from being overridden in JavaScript?

**Answer**: JavaScript doesn't have a built-in `final` keyword like Java, but you can achieve similar behavior using several techniques:

**Option 1**: Use `Object.freeze()` on the prototype
```javascript
class Parent {
    finalMethod() {
        return 'Cannot override this';
    }
}

Object.freeze(Parent.prototype);

class Child extends Parent {
    finalMethod() {  // This won't work in strict mode
        return 'Trying to override';
    }
}
```

**Option 2**: Check method source in constructor
```javascript
class Parent {
    constructor() {
        if (this.criticalMethod !== Parent.prototype.criticalMethod) {
            throw new Error('criticalMethod cannot be overridden');
        }
    }
    
    criticalMethod() {
        return 'Critical logic';
    }
}
```

**Option 3**: Use private methods (recommended with modern JavaScript)
```javascript
class Parent {
    #privateMethod() {
        return 'Cannot be overridden';
    }
    
    publicMethod() {
        return this.#privateMethod();
    }
}
```

---

### Best Practices for Method Overriding

1. **âœ… Always call `super.methodName()` when extending functionality**
   ```javascript
   // Good
   class Child extends Parent {
       method() {
           super.method(); // Keep parent behavior
           // Add child-specific logic
       }
   }
   ```

2. **âœ… Maintain the same method signature**
   ```javascript
   class Parent {
       process(data, options) { }
   }
   
   // Good - same signature
   class Child extends Parent {
       process(data, options) {
           super.process(data, options);
       }
   }
   ```

3. **âœ… Document why you're overriding**
   ```javascript
   class SecureLoginPage extends LoginPage {
       /**
        * Override login to add 2FA verification
        * Extends parent login with two-factor authentication
        */
       async login(username, password, twoFactorCode) {
           await super.login(username, password);
           await this.verify2FA(twoFactorCode);
       }
   }
   ```

4. **âœ… Keep overridden methods focused**
   ```javascript
   // Good - clear purpose
   class Child extends Parent {
       validate() {
           super.validate();
           this.validateSpecificRules();
       }
   }
   
   // Bad - doing too much
   class Child extends Parent {
       validate() {
           super.validate();
           this.validateSpecificRules();
           this.sendEmail();
           this.logToDatabase();
           this.generateReport();
       }
   }
   ```

5. **âŒ Don't override methods you don't need to change**
   ```javascript
   // Bad - unnecessary override
   class Child extends Parent {
       method() {
           return super.method(); // Just calling parent, no point
       }
   }
   
   // Good - only override when needed
   class Child extends Parent {
       // Inherit parent's method as-is
   }
   ```

---

### Summary

**Method Overriding** is a powerful OOP feature that allows child classes to customize parent behavior while maintaining the inheritance relationship. Key points:

- âœ… Child method replaces parent method with same name
- âœ… Use `super.methodName()` to call the parent version
- âœ… Enables polymorphism - different behavior for different types
- âœ… Constructor overriding requires calling `super()` first
- âœ… Common in test frameworks for page-specific behavior
- âœ… JavaScript supports method overriding but NOT method overloading
- âœ… Keep overridden methods focused and well-documented

---

## Inheritance Best Practices

### 1. Prefer Composition Over Inheritance

```javascript
// ❌ Bad: Deep inheritance hierarchy
class Animal {}
class Mammal extends Animal {}
class Carnivore extends Mammal {}
class Dog extends Carnivore {}
class Labrador extends Dog {}

// ✅ Good: Composition
class Dog {
    constructor() {
        this.movementBehavior = new WalkBehavior();
        this.soundBehavior = new BarkBehavior();
        this.dietBehavior = new CarnivoreDiet();
    }
}
```

### 2. Use `super` Correctly

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
    
    greet() {
        return `Hello from ${this.name}`;
    }
}

class Child extends Parent {
    constructor(name, age) {
        // ✅ Call super() before accessing 'this'
        super(name);
        this.age = age;
    }
    
    greet() {
        // ✅ Call parent method with super
        const parentGreeting = super.greet();
        return `${parentGreeting}, I am ${this.age} years old`;
    }
}
```

### 3. Override Methods Carefully

```javascript
class BasePage {
    async navigate(url) {
        console.log(`Navigating to: ${url}`);
        // Base implementation
    }
}

class LoginPage extends BasePage {
    async navigate(url) {
        // ✅ Good: Call parent method and extend
        await super.navigate(url);
        console.log('Login page specific logic');
        await this.waitForPageLoad();
    }
    
    async waitForPageLoad() {
        // Additional logic
    }
}
```

### 4. Keep Inheritance Shallow

```javascript
// ✅ Good: 2-3 levels deep
class BasePage {}
class FormPage extends BasePage {}
class LoginPage extends FormPage {}

// ❌ Bad: Too deep
class Level1 {}
class Level2 extends Level1 {}
class Level3 extends Level2 {}
class Level4 extends Level3 {}
class Level5 extends Level4 {}
class Level6 extends Level5 {}  // Too deep!
```

### 5. Document Inheritance Relationships

```javascript
/**
 * BasePage - Base class for all page objects
 * Provides common navigation and interaction methods
 */
class BasePage {
    // ...
}

/**
 * FormPage - Extends BasePage
 * Adds form-specific functionality like fillField, submitForm
 * Parent: BasePage
 */
class FormPage extends BasePage {
    // ...
}

/**
 * LoginPage - Extends FormPage
 * Implements login-specific logic
 * Parent: FormPage -> BasePage
 */
class LoginPage extends FormPage {
    // ...
}
```

---

## Real-World Test Automation Examples

### Example 1: Page Object Model Hierarchy

```javascript
// Level 1: Base Page
class BasePage {
    constructor(page) {
        this.page = page;
        this.timeout = 30000;
    }
    
    async navigate(url) {
        await this.page.goto(url, { timeout: this.timeout });
    }
    
    async waitForElement(selector) {
        await this.page.waitForSelector(selector, { timeout: this.timeout });
    }
    
    async getText(selector) {
        return await this.page.textContent(selector);
    }
    
    async click(selector) {
        await this.page.click(selector);
    }
}

// Level 2: Authenticated Page
class AuthenticatedPage extends BasePage {
    constructor(page) {
        super(page);
        this.logoutButton = '#logout';
        this.userMenu = '#user-menu';
    }
    
    async logout() {
        await this.click(this.userMenu);
        await this.click(this.logoutButton);
    }
    
    async getUserName() {
        return await this.getText('#username-display');
    }
    
    async isAuthenticated() {
        try {
            await this.waitForElement(this.userMenu);
            return true;
        } catch {
            return false;
        }
    }
}

// Level 3: Specific Pages
class DashboardPage extends AuthenticatedPage {
    constructor(page) {
        super(page);
        this.url = 'https://example.com/dashboard';
        this.welcomeMessage = '.welcome-message';
        this.statsCards = '.stats-card';
    }
    
    async open() {
        await this.navigate(this.url);
    }
    
    async getWelcomeMessage() {
        return await this.getText(this.welcomeMessage);
    }
    
    async getStatsCount() {
        return await this.page.locator(this.statsCards).count();
    }
}

class ProfilePage extends AuthenticatedPage {
    constructor(page) {
        super(page);
        this.url = 'https://example.com/profile';
        this.nameField = '#name';
        this.emailField = '#email';
        this.saveButton = '#save-profile';
    }
    
    async open() {
        await this.navigate(this.url);
    }
    
    async updateProfile(name, email) {
        await this.page.fill(this.nameField, name);
        await this.page.fill(this.emailField, email);
        await this.click(this.saveButton);
    }
}

// Usage
const dashboard = new DashboardPage(page);
await dashboard.open();
console.log(await dashboard.getUserName());        // From AuthenticatedPage
console.log(await dashboard.getWelcomeMessage());  // From DashboardPage
await dashboard.logout();                          // From AuthenticatedPage
```

### Example 2: Test Framework with Mixins

```javascript
// Mixin: Screenshot capability
const ScreenshotMixin = (Base) => class extends Base {
    async takeScreenshot(filename) {
        await this.page.screenshot({ path: `screenshots/${filename}` });
        this.log(`Screenshot saved: ${filename}`);
    }
    
    async takeFullPageScreenshot(filename) {
        await this.page.screenshot({ 
            path: `screenshots/${filename}`,
            fullPage: true 
        });
        this.log(`Full page screenshot saved: ${filename}`);
    }
};

// Mixin: Logging capability
const LoggingMixin = (Base) => class extends Base {
    log(message) {
        const timestamp = new Date().toISOString();
        console.log(`[${timestamp}] ${message}`);
    }
    
    logStep(step) {
        this.log(`STEP: ${step}`);
    }
    
    logError(error) {
        console.error(`[ERROR] ${error}`);
    }
};

// Mixin: Retry capability
const RetryMixin = (Base) => class extends Base {
    async retryAction(action, maxAttempts = 3) {
        for (let attempt = 1; attempt <= maxAttempts; attempt++) {
            try {
                return await action();
            } catch (error) {
                if (attempt === maxAttempts) throw error;
                this.log(`Attempt ${attempt} failed, retrying...`);
                await this.wait(1000);
            }
        }
    }
    
    async wait(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
};

// Base test class
class BaseTest {
    constructor(page) {
        this.page = page;
    }
}

// Enhanced test class with multiple capabilities
class EnhancedTest extends RetryMixin(ScreenshotMixin(LoggingMixin(BaseTest))) {
    async runTest(testName, testFunction) {
        this.logStep(`Starting test: ${testName}`);
        
        try {
            await this.retryAction(async () => {
                await testFunction();
            });
            this.logStep(`Test passed: ${testName}`);
            await this.takeScreenshot(`${testName}-pass.png`);
        } catch (error) {
            this.logError(`Test failed: ${testName} - ${error.message}`);
            await this.takeScreenshot(`${testName}-fail.png`);
            throw error;
        }
    }
}

// Usage
const test = new EnhancedTest(page);
await test.runTest('Login Test', async () => {
    await page.goto('https://example.com/login');
    await page.fill('#username', 'testuser');
    await page.fill('#password', 'password');
    await page.click('#login-button');
});
```

### Example 3: API Client Inheritance

```javascript
// Base HTTP Client
class HTTPClient {
    constructor(baseURL) {
        this.baseURL = baseURL;
        this.headers = {
            'Content-Type': 'application/json'
        };
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        const config = {
            ...options,
            headers: { ...this.headers, ...options.headers }
        };
        
        const response = await fetch(url, config);
        return await response.json();
    }
    
    async get(endpoint, options = {}) {
        return this.request(endpoint, { ...options, method: 'GET' });
    }
    
    async post(endpoint, body, options = {}) {
        return this.request(endpoint, {
            ...options,
            method: 'POST',
            body: JSON.stringify(body)
        });
    }
}

// Authenticated API Client (extends HTTPClient)
class AuthenticatedAPIClient extends HTTPClient {
    constructor(baseURL, token) {
        super(baseURL);
        this.token = token;
        this.headers['Authorization'] = `Bearer ${token}`;
    }
    
    async refreshToken(refreshToken) {
        const response = await this.post('/auth/refresh', { refreshToken });
        this.token = response.token;
        this.headers['Authorization'] = `Bearer ${this.token}`;
        return this.token;
    }
    
    setToken(token) {
        this.token = token;
        this.headers['Authorization'] = `Bearer ${token}`;
    }
}

// User API Client (extends AuthenticatedAPIClient)
class UserAPIClient extends AuthenticatedAPIClient {
    async getUser(userId) {
        return await this.get(`/users/${userId}`);
    }
    
    async createUser(userData) {
        return await this.post('/users', userData);
    }
    
    async updateUser(userId, userData) {
        return await this.request(`/users/${userId}`, {
            method: 'PUT',
            body: JSON.stringify(userData)
        });
    }
    
    async deleteUser(userId) {
        return await this.request(`/users/${userId}`, {
            method: 'DELETE'
        });
    }
    
    async getUserProfile() {
        return await this.get('/users/me');
    }
}

// Usage
const userAPI = new UserAPIClient('https://api.example.com', 'auth-token-123');
const user = await userAPI.getUser(1);
const profile = await userAPI.getUserProfile();
await userAPI.updateUser(1, { name: 'Updated Name' });
```

### Example 4: Test Data Builder with Inheritance

```javascript
// Base Builder
class BaseDataBuilder {
    constructor() {
        this.data = {};
    }
    
    with(key, value) {
        this.data[key] = value;
        return this;
    }
    
    build() {
        return { ...this.data };
    }
    
    reset() {
        this.data = {};
        return this;
    }
}

// User Data Builder (extends BaseDataBuilder)
class UserDataBuilder extends BaseDataBuilder {
    constructor() {
        super();
        this.data = {
            username: 'testuser',
            email: 'test@example.com',
            role: 'user',
            isActive: true
        };
    }
    
    withUsername(username) {
        return this.with('username', username);
    }
    
    withEmail(email) {
        return this.with('email', email);
    }
    
    withRole(role) {
        return this.with('role', role);
    }
    
    asAdmin() {
        return this.withRole('admin');
    }
    
    asGuest() {
        return this.withRole('guest');
    }
    
    inactive() {
        return this.with('isActive', false);
    }
}

// Employee Data Builder (extends UserDataBuilder)
class EmployeeDataBuilder extends UserDataBuilder {
    constructor() {
        super();
        this.data = {
            ...this.data,
            employeeId: null,
            department: 'Engineering',
            salary: 50000,
            hireDate: new Date().toISOString()
        };
    }
    
    withEmployeeId(id) {
        return this.with('employeeId', id);
    }
    
    withDepartment(department) {
        return this.with('department', department);
    }
    
    withSalary(salary) {
        return this.with('salary', salary);
    }
    
    inDepartment(department) {
        return this.withDepartment(department);
    }
}

// Usage
const user = new UserDataBuilder()
    .withUsername('john_doe')
    .withEmail('john@example.com')
    .asAdmin()
    .build();

const employee = new EmployeeDataBuilder()
    .withUsername('jane_smith')
    .withEmployeeId('EMP001')
    .inDepartment('QA')
    .withSalary(60000)
    .build();

console.log(user);
console.log(employee);
```

---

## Interview Tips

### Common Interview Questions

**Q1: What is inheritance in JavaScript?**

**Answer**: Inheritance is an OOP mechanism where a class (child) can inherit properties and methods from another class (parent). JavaScript uses the `extends` keyword for class inheritance and the `super` keyword to call parent class constructors and methods. Under the hood, JavaScript uses prototypal inheritance through the prototype chain.

**Q2: Is multi-level inheritance supported in JavaScript?**

**Answer**: Yes, JavaScript fully supports multi-level inheritance. A class can inherit from another class, which itself inherits from another class, creating an inheritance chain. For example:
```javascript
class Grandparent { }
class Parent extends Grandparent { }
class Child extends Parent { }
```
The child class has access to all methods from both Parent and Grandparent classes through the prototype chain.

**Q3: Does JavaScript support multiple inheritance?**

**Answer**: No, JavaScript does not support multiple inheritance directly. A class can only extend one parent class. However, we can achieve similar functionality using:
- **Mixins** - Functions that add methods to a class
- **Composition** - Combining multiple objects
- **Object.assign()** - Copying methods from multiple sources

**Q4: What is the prototype chain?**

**Answer**: The prototype chain is JavaScript's inheritance mechanism. When you access a property or method on an object, JavaScript first looks on the object itself. If not found, it looks at the object's prototype, then that prototype's prototype, and so on, until it reaches `null`. This forms a chain of prototypes.

**Q5: What is the difference between `__proto__` and `prototype`?**

**Answer**:
- `__proto__` is the actual object used in the lookup chain (instance's prototype)
- `prototype` is a property on constructor functions that becomes the `__proto__` of instances created with that constructor
```javascript
class Animal { }
const dog = new Animal();
console.log(dog.__proto__ === Animal.prototype);  // true
```

**Q6: When should you use inheritance vs composition?**

**Answer**: 
- Use **inheritance** when there's a clear "is-a" relationship (Dog is an Animal)
- Use **composition** when there's a "has-a" relationship (Car has an Engine)
- Favor composition over deep inheritance hierarchies
- Use composition when you need functionality from multiple sources

**Q7: What does `super` do and when must you call it?**

**Answer**: `super` is used to:
1. Call the parent constructor: `super(args)` - **must** be called before using `this` in child constructor
2. Call parent methods: `super.methodName()`

```javascript
class Parent {
    constructor(name) {
        this.name = name;
    }
}

class Child extends Parent {
    constructor(name, age) {
        super(name);  // Must call before using 'this'
        this.age = age;
    }
}
```

**Q8: How can you check if an object inherits from a class?**

**Answer**: Use `instanceof`:
```javascript
class Animal { }
class Dog extends Animal { }
const dog = new Dog();

console.log(dog instanceof Dog);     // true
console.log(dog instanceof Animal);  // true
console.log(dog instanceof Object);  // true
```

**Q9: What is method overriding?**

**Answer**: Method overriding is when a child class provides its own implementation of a method that exists in the parent class:
```javascript
class Animal {
    speak() {
        return 'Some sound';
    }
}

class Dog extends Animal {
    speak() {
        return 'Woof!';  // Overrides parent method
    }
}
```

**Q10: Explain the diamond problem in multiple inheritance**

**Answer**: The diamond problem occurs when a class inherits from two classes that share a common ancestor, creating ambiguity about which parent's method to use. JavaScript avoids this by not supporting multiple inheritance. Instead, it uses a single prototype chain and provides mixins/composition as alternatives.

---

## Comparison Table

| Feature | Single Inheritance | Multi-Level Inheritance | Multiple Inheritance |
|---------|-------------------|------------------------|---------------------|
| **Supported in JS** | ✅ Yes | ✅ Yes | ❌ No (use mixins) |
| **Syntax** | `extends ParentClass` | `extends ParentClass` (chain) | N/A |
| **Complexity** | Low | Medium | High |
| **Use Case** | Basic hierarchy | Layered functionality | Mixed behaviors |
| **Prototype Chain** | Simple | Linear chain | Would be complex |
| **Best Practice** | ✅ Recommended | ✅ Acceptable (3-4 levels) | ✅ Use composition |

---

## Key Takeaways

1. ✅ JavaScript supports **single inheritance** via the `extends` keyword
2. ✅ **Multi-level inheritance** is fully supported (but keep it shallow)
3. ❌ **Multiple inheritance** is NOT supported directly
4. ✅ Use **mixins** or **composition** for multiple inheritance behavior
5. ✅ The `super` keyword calls parent constructors and methods
6. ✅ Inheritance creates a **prototype chain** for property lookup
7. ✅ Prefer **composition over inheritance** for complex scenarios
8. ✅ Keep inheritance hierarchies **shallow** (2-4 levels maximum)
9. ✅ Use `instanceof` to check inheritance relationships
10. ✅ Method overriding allows child classes to customize parent behavior

---

## Quick Reference

### Basic Inheritance Syntax

```javascript
// Parent class
class Parent {
    constructor(prop) {
        this.prop = prop;
    }
    
    parentMethod() {
        return 'from parent';
    }
}

// Child class
class Child extends Parent {
    constructor(prop, childProp) {
        super(prop);  // Call parent constructor
        this.childProp = childProp;
    }
    
    childMethod() {
        return 'from child';
    }
    
    // Override parent method
    parentMethod() {
        const parentResult = super.parentMethod();
        return `${parentResult} and child`;
    }
}

// Create instance
const instance = new Child('value1', 'value2');
```

### Mixin Template

```javascript
const MyMixin = (Base) => class extends Base {
    mixinMethod() {
        // Mixin functionality
    }
};

// Apply mixin
class MyClass extends MyMixin(BaseClass) {
    // Class implementation
}
```

### Checking Inheritance

```javascript
// Check if instance
instance instanceof ClassName;

// Check prototype
ClassName.prototype.isPrototypeOf(instance);

// Check constructor
instance.constructor === ClassName;

// Check property existence
'propertyName' in instance;
instance.hasOwnProperty('propertyName');
```

---

## Additional Resources

- [MDN - Inheritance and the prototype chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)
- [MDN - Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN - extends](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/extends)
- [MDN - super](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/super)
- [JavaScript.info - Class inheritance](https://javascript.info/class-inheritance)

---

**Note**: This document is prepared for automation test architect interview preparation. Understanding inheritance is crucial for building maintainable test frameworks with Page Object Models and reusable test components. Focus on practical patterns like mixins and composition for real-world test automation!