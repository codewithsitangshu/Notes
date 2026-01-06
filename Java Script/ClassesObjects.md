# JavaScript Classes & Objects - Interview Preparation Notes

## Table of Contents
1. [How to Create Class in JavaScript](#how-to-create-class-in-javascript)
2. [How to Create Objects from Class](#how-to-create-objects-from-class)
3. [Can We Overload a Constructor in JavaScript?](#can-we-overload-a-constructor-in-javascript)
4. [Alternatives to Constructor Overloading](#alternatives-to-constructor-overloading)
5. [Best Practices for Test Automation](#best-practices-for-test-automation)
6. [Advanced Class Concepts](#advanced-class-concepts)

---

## How to Create Class in JavaScript

JavaScript supports **two ways** to create classes:

### Method 1: ES6 Class Syntax (Modern - Recommended)

Introduced in ES6 (2015), this is the **preferred** way to create classes.

```javascript
// Basic class structure
class User {
    // Constructor method - called when object is created
    constructor(username, email) {
        this.username = username;
        this.email = email;
        this.isActive = true;  // default property
    }
    
    // Instance method
    login() {
        console.log(`${this.username} logged in`);
        return true;
    }
    
    // Instance method
    logout() {
        console.log(`${this.username} logged out`);
        return true;
    }
    
    // Instance method
    getInfo() {
        return {
            username: this.username,
            email: this.email,
            isActive: this.isActive
        };
    }
}
```

### Method 2: Constructor Function (Old Way - ES5)

Before ES6, classes were created using constructor functions.

```javascript
// Constructor function (old way)
function User(username, email) {
    this.username = username;
    this.email = email;
    this.isActive = true;
}

// Adding methods to prototype
User.prototype.login = function() {
    console.log(`${this.username} logged in`);
    return true;
};

User.prototype.logout = function() {
    console.log(`${this.username} logged out`);
    return true;
};

User.prototype.getInfo = function() {
    return {
        username: this.username,
        email: this.email,
        isActive: this.isActive
    };
};
```

### Complete Class Example with All Features

```javascript
class TestUser {
    // Static property (shared across all instances)
    static userCount = 0;
    
    // Private field (ES2022 - starts with #)
    #password;
    
    // Constructor
    constructor(username, email, password) {
        this.username = username;        // Public property
        this.email = email;              // Public property
        this.#password = password;       // Private property
        this.createdAt = new Date();     // Public property
        TestUser.userCount++;            // Increment static property
    }
    
    // Public method
    login() {
        console.log(`${this.username} logged in`);
        return true;
    }
    
    // Public method
    validatePassword(password) {
        return this.#password === password;
    }
    
    // Getter method
    get info() {
        return `User: ${this.username}, Email: ${this.email}`;
    }
    
    // Setter method
    set username(value) {
        if (value.length < 3) {
            throw new Error('Username must be at least 3 characters');
        }
        this._username = value;
    }
    
    get username() {
        return this._username;
    }
    
    // Static method (called on class, not instance)
    static getTotalUsers() {
        return TestUser.userCount;
    }
    
    // Private method (ES2022)
    #encryptPassword(password) {
        return `encrypted_${password}`;
    }
}
```

### Class with Inheritance

```javascript
// Parent class
class BasePage {
    constructor(page, url) {
        this.page = page;
        this.url = url;
    }
    
    async navigate() {
        await this.page.goto(this.url);
        console.log(`Navigated to ${this.url}`);
    }
    
    async getTitle() {
        return await this.page.title();
    }
}

// Child class (inherits from BasePage)
class LoginPage extends BasePage {
    constructor(page) {
        super(page, 'https://example.com/login');  // Call parent constructor
        this.usernameField = '#username';
        this.passwordField = '#password';
        this.loginButton = '#login-btn';
    }
    
    async login(username, password) {
        await this.page.fill(this.usernameField, username);
        await this.page.fill(this.passwordField, password);
        await this.page.click(this.loginButton);
        console.log(`Login attempt for user: ${username}`);
    }
    
    // Override parent method
    async navigate() {
        await super.navigate();  // Call parent method
        console.log('Login page loaded');
    }
}
```

---

## How to Create Objects from Class

### Creating Objects (Instantiation)

```javascript
class User {
    constructor(username, email) {
        this.username = username;
        this.email = email;
    }
    
    greet() {
        return `Hello, I'm ${this.username}`;
    }
}

// Creating objects using 'new' keyword
const user1 = new User('john_doe', 'john@example.com');
const user2 = new User('jane_smith', 'jane@example.com');
const user3 = new User('test_user', 'test@example.com');

// Accessing properties
console.log(user1.username);      // Output: john_doe
console.log(user2.email);         // Output: jane@example.com

// Calling methods
console.log(user1.greet());       // Output: Hello, I'm john_doe
console.log(user2.greet());       // Output: Hello, I'm jane_smith
```

### Multiple Ways to Create Objects

```javascript
class TestConfig {
    constructor(browser, headless) {
        this.browser = browser;
        this.headless = headless;
    }
}

// Method 1: Direct instantiation
const config1 = new TestConfig('chrome', true);

// Method 2: Store in array
const configs = [
    new TestConfig('chrome', true),
    new TestConfig('firefox', false),
    new TestConfig('safari', true)
];

// Method 3: Factory function
function createTestConfig(browser, headless) {
    return new TestConfig(browser, headless);
}
const config2 = createTestConfig('edge', false);

// Method 4: Create from data
const testData = [
    { browser: 'chrome', headless: true },
    { browser: 'firefox', headless: false }
];
const testConfigs = testData.map(data => 
    new TestConfig(data.browser, data.headless)
);
```

### Object Properties and Methods

```javascript
class TestCase {
    constructor(name, priority) {
        this.name = name;
        this.priority = priority;
        this.status = 'pending';
        this.startTime = null;
        this.endTime = null;
    }
    
    start() {
        this.startTime = new Date();
        this.status = 'running';
    }
    
    pass() {
        this.endTime = new Date();
        this.status = 'passed';
    }
    
    fail(error) {
        this.endTime = new Date();
        this.status = 'failed';
        this.error = error;
    }
    
    getDuration() {
        if (this.startTime && this.endTime) {
            return this.endTime - this.startTime;
        }
        return 0;
    }
}

// Creating and using test case objects
const test1 = new TestCase('Login Test', 'high');
test1.start();
// ... test execution
test1.pass();
console.log(`Test ${test1.name} took ${test1.getDuration()}ms`);

const test2 = new TestCase('Logout Test', 'medium');
test2.start();
// ... test execution
test2.fail('Element not found');
console.log(`Test ${test2.name} status: ${test2.status}`);
```

### Checking Object Type

```javascript
class User {
    constructor(name) {
        this.name = name;
    }
}

const user = new User('John');

// Check if object is instance of class
console.log(user instanceof User);           // true
console.log(user instanceof Object);         // true

// Check constructor
console.log(user.constructor === User);      // true
console.log(user.constructor.name);          // "User"

// Check type
console.log(typeof user);                    // "object"

// Check if property exists
console.log('name' in user);                 // true
console.log(user.hasOwnProperty('name'));    // true
```

---

## Can We Overload a Constructor in JavaScript?

### Short Answer: ❌ **NO, JavaScript Does NOT Support Constructor Overloading**

Just like function overloading, **constructor overloading is not supported** in JavaScript. You can only have **ONE constructor** per class.

### What Happens When You Try?

```javascript
class User {
    constructor(username) {
        this.username = username;
        console.log('Constructor 1: username only');
    }
    
    // ❌ This will cause a SyntaxError!
    constructor(username, email) {
        this.username = username;
        this.email = email;
        console.log('Constructor 2: username and email');
    }
    
    // ❌ This will also cause a SyntaxError!
    constructor(username, email, age) {
        this.username = username;
        this.email = email;
        this.age = age;
        console.log('Constructor 3: username, email, and age');
    }
}

// Error: A class may only have one constructor
```

**Result**: You'll get a **SyntaxError**: *A class may only have one constructor*

### Why Constructor Overloading Doesn't Work

1. **JavaScript allows only one constructor per class** - This is a language design decision
2. **No compile-time type checking** - JavaScript can't distinguish between `constructor(string)` and `constructor(number)`
3. **Dynamic typing** - Parameters don't have types, so there's no way to differentiate signatures
4. **Functions are objects** - Multiple constructors would overwrite each other like regular functions

---

## Alternatives to Constructor Overloading

### ✅ Method 1: Optional Parameters with Default Values (RECOMMENDED)

```javascript
class User {
    constructor(username, email = null, age = null, role = 'user') {
        this.username = username;
        this.email = email;
        this.age = age;
        this.role = role;
        this.createdAt = new Date();
    }
    
    getInfo() {
        return {
            username: this.username,
            email: this.email || 'Not provided',
            age: this.age || 'Not provided',
            role: this.role
        };
    }
}

// Usage - Very flexible!
const user1 = new User('john_doe');
// { username: 'john_doe', email: null, age: null, role: 'user' }

const user2 = new User('jane_smith', 'jane@example.com');
// { username: 'jane_smith', email: 'jane@example.com', age: null, role: 'user' }

const user3 = new User('admin_user', 'admin@example.com', 30, 'admin');
// All parameters provided

console.log(user1.getInfo());
console.log(user2.getInfo());
console.log(user3.getInfo());
```

### ✅ Method 2: Options Object Pattern (BEST PRACTICE)

```javascript
class TestConfig {
    constructor(options = {}) {
        // Destructure with default values
        const {
            browser = 'chrome',
            headless = true,
            viewport = { width: 1280, height: 720 },
            timeout = 30000,
            retries = 0,
            screenshots = false,
            video = false,
            baseURL = 'http://localhost:3000',
            slowMo = 0
        } = options;
        
        // Assign properties
        this.browser = browser;
        this.headless = headless;
        this.viewport = viewport;
        this.timeout = timeout;
        this.retries = retries;
        this.screenshots = screenshots;
        this.video = video;
        this.baseURL = baseURL;
        this.slowMo = slowMo;
    }
    
    display() {
        console.log('Test Configuration:');
        console.log(`Browser: ${this.browser}`);
        console.log(`Headless: ${this.headless}`);
        console.log(`Timeout: ${this.timeout}ms`);
        console.log(`Base URL: ${this.baseURL}`);
    }
}

// Usage - Very clean and self-documenting!

// Minimal config
const config1 = new TestConfig();
// Uses all defaults

// Partial config
const config2 = new TestConfig({
    browser: 'firefox',
    headless: false
});

// Full config
const config3 = new TestConfig({
    browser: 'chrome',
    headless: false,
    viewport: { width: 1920, height: 1080 },
    timeout: 60000,
    retries: 3,
    screenshots: true,
    video: true,
    baseURL: 'https://staging.example.com',
    slowMo: 100
});

config1.display();
config2.display();
config3.display();
```

### ✅ Method 3: Check Argument Types and Count

```javascript
class User {
    constructor(...args) {
        if (args.length === 0) {
            // No arguments - create default user
            this.username = 'guest';
            this.email = null;
            this.role = 'guest';
        } 
        else if (args.length === 1) {
            if (typeof args[0] === 'string') {
                // Single string - username only
                this.username = args[0];
                this.email = null;
                this.role = 'user';
            } 
            else if (typeof args[0] === 'object') {
                // Single object - use object pattern
                const { username, email, role = 'user' } = args[0];
                this.username = username;
                this.email = email;
                this.role = role;
            }
        } 
        else if (args.length === 2) {
            // Two arguments - username and email
            this.username = args[0];
            this.email = args[1];
            this.role = 'user';
        } 
        else if (args.length >= 3) {
            // Three or more arguments - username, email, and role
            this.username = args[0];
            this.email = args[1];
            this.role = args[2];
        }
        
        this.createdAt = new Date();
    }
    
    display() {
        console.log(`User: ${this.username}, Email: ${this.email || 'N/A'}, Role: ${this.role}`);
    }
}

// Usage - Supports multiple patterns!
const user1 = new User();
user1.display();  // User: guest, Email: N/A, Role: guest

const user2 = new User('john_doe');
user2.display();  // User: john_doe, Email: N/A, Role: user

const user3 = new User('jane_smith', 'jane@example.com');
user3.display();  // User: jane_smith, Email: jane@example.com, Role: user

const user4 = new User('admin', 'admin@example.com', 'admin');
user4.display();  // User: admin, Email: admin@example.com, Role: admin

const user5 = new User({ username: 'object_user', email: 'obj@example.com', role: 'moderator' });
user5.display();  // User: object_user, Email: obj@example.com, Role: moderator
```

### ✅ Method 4: Static Factory Methods (HIGHLY RECOMMENDED)

```javascript
class User {
    constructor(username, email, role) {
        this.username = username;
        this.email = email;
        this.role = role;
        this.createdAt = new Date();
    }
    
    // Static factory method - create guest user
    static createGuest() {
        return new User('guest', null, 'guest');
    }
    
    // Static factory method - create regular user
    static createUser(username, email) {
        return new User(username, email, 'user');
    }
    
    // Static factory method - create admin user
    static createAdmin(username, email) {
        return new User(username, email, 'admin');
    }
    
    // Static factory method - create from API response
    static fromAPIResponse(apiData) {
        return new User(
            apiData.username,
            apiData.email,
            apiData.role || 'user'
        );
    }
    
    // Static factory method - create from database record
    static fromDatabase(dbRecord) {
        const user = new User(
            dbRecord.username,
            dbRecord.email,
            dbRecord.role
        );
        user.createdAt = new Date(dbRecord.created_at);
        return user;
    }
    
    display() {
        console.log(`User: ${this.username}, Role: ${this.role}`);
    }
}

// Usage - Very clear and explicit!
const guest = User.createGuest();
guest.display();  // User: guest, Role: guest

const regularUser = User.createUser('john_doe', 'john@example.com');
regularUser.display();  // User: john_doe, Role: user

const admin = User.createAdmin('admin', 'admin@example.com');
admin.display();  // User: admin, Role: admin

// From API
const apiUser = User.fromAPIResponse({
    username: 'api_user',
    email: 'api@example.com',
    role: 'moderator'
});
apiUser.display();

// From Database
const dbUser = User.fromDatabase({
    username: 'db_user',
    email: 'db@example.com',
    role: 'user',
    created_at: '2024-01-01'
});
dbUser.display();
```

### ✅ Method 5: Builder Pattern

```javascript
class UserBuilder {
    constructor() {
        this.user = {
            username: 'guest',
            email: null,
            role: 'user',
            age: null,
            country: null,
            preferences: {}
        };
    }
    
    setUsername(username) {
        this.user.username = username;
        return this;  // Return this for chaining
    }
    
    setEmail(email) {
        this.user.email = email;
        return this;
    }
    
    setRole(role) {
        this.user.role = role;
        return this;
    }
    
    setAge(age) {
        this.user.age = age;
        return this;
    }
    
    setCountry(country) {
        this.user.country = country;
        return this;
    }
    
    setPreferences(preferences) {
        this.user.preferences = preferences;
        return this;
    }
    
    build() {
        return new User(this.user);
    }
}

class User {
    constructor(data) {
        Object.assign(this, data);
        this.createdAt = new Date();
    }
    
    display() {
        console.log('User Details:', this);
    }
}

// Usage - Very flexible and readable!
const user1 = new UserBuilder()
    .setUsername('john_doe')
    .setEmail('john@example.com')
    .build();

const user2 = new UserBuilder()
    .setUsername('jane_smith')
    .setEmail('jane@example.com')
    .setRole('admin')
    .setAge(28)
    .setCountry('USA')
    .setPreferences({ theme: 'dark', notifications: true })
    .build();

user1.display();
user2.display();
```

---

## Best Practices for Test Automation

### 1. Page Object Model with Options Object

```javascript
class BasePage {
    constructor(page, options = {}) {
        this.page = page;
        this.timeout = options.timeout || 30000;
        this.waitForNavigation = options.waitForNavigation !== false;
        this.screenshotOnError = options.screenshotOnError || false;
    }
    
    async navigate(url) {
        try {
            await this.page.goto(url, { timeout: this.timeout });
        } catch (error) {
            if (this.screenshotOnError) {
                await this.page.screenshot({ path: 'error.png' });
            }
            throw error;
        }
    }
}

class LoginPage extends BasePage {
    constructor(page, options = {}) {
        super(page, options);
        this.url = options.url || 'https://example.com/login';
        this.selectors = {
            username: options.usernameSelector || '#username',
            password: options.passwordSelector || '#password',
            loginButton: options.loginButtonSelector || '#login-btn',
            errorMessage: options.errorSelector || '.error-message'
        };
    }
    
    async login(username, password) {
        await this.navigate(this.url);
        await this.page.fill(this.selectors.username, username);
        await this.page.fill(this.selectors.password, password);
        await this.page.click(this.selectors.loginButton);
        
        if (this.waitForNavigation) {
            await this.page.waitForNavigation({ timeout: this.timeout });
        }
    }
    
    async getErrorMessage() {
        return await this.page.textContent(this.selectors.errorMessage);
    }
}

// Usage - Highly configurable
const loginPage1 = new LoginPage(page);  // Use defaults

const loginPage2 = new LoginPage(page, {
    timeout: 60000,
    screenshotOnError: true,
    url: 'https://staging.example.com/login'
});

const loginPage3 = new LoginPage(page, {
    timeout: 45000,
    waitForNavigation: false,
    usernameSelector: '[data-testid="username"]',
    passwordSelector: '[data-testid="password"]',
    loginButtonSelector: '[data-testid="login"]'
});
```

### 2. Test Data Model with Factory Methods

```javascript
class TestData {
    constructor(data) {
        this.username = data.username;
        this.password = data.password;
        this.email = data.email;
        this.firstName = data.firstName;
        this.lastName = data.lastName;
        this.role = data.role || 'user';
    }
    
    // Factory method: Create valid user
    static validUser() {
        return new TestData({
            username: 'test_user',
            password: 'Test@123',
            email: 'test@example.com',
            firstName: 'Test',
            lastName: 'User',
            role: 'user'
        });
    }
    
    // Factory method: Create admin user
    static adminUser() {
        return new TestData({
            username: 'admin',
            password: 'Admin@123',
            email: 'admin@example.com',
            firstName: 'Admin',
            lastName: 'User',
            role: 'admin'
        });
    }
    
    // Factory method: Create invalid user (for negative testing)
    static invalidUser() {
        return new TestData({
            username: 'a',  // Too short
            password: '123',  // Weak password
            email: 'invalid-email',  // Invalid format
            firstName: '',
            lastName: '',
            role: 'user'
        });
    }
    
    // Factory method: Create user with missing fields
    static incompleteUser() {
        return new TestData({
            username: 'incomplete_user',
            password: null,
            email: null,
            firstName: 'Incomplete',
            lastName: null,
            role: 'user'
        });
    }
    
    // Factory method: Create random user
    static randomUser() {
        const timestamp = Date.now();
        return new TestData({
            username: `user_${timestamp}`,
            password: `Pass@${timestamp}`,
            email: `user${timestamp}@example.com`,
            firstName: `First${timestamp}`,
            lastName: `Last${timestamp}`,
            role: 'user'
        });
    }
}

// Usage in tests
describe('User Registration Tests', () => {
    test('Should register valid user', async () => {
        const user = TestData.validUser();
        await registrationPage.register(user);
        expect(await homePage.isLoggedIn()).toBe(true);
    });
    
    test('Should not register invalid user', async () => {
        const user = TestData.invalidUser();
        await registrationPage.register(user);
        expect(await registrationPage.hasError()).toBe(true);
    });
    
    test('Should create unique users', async () => {
        const user1 = TestData.randomUser();
        const user2 = TestData.randomUser();
        expect(user1.username).not.toBe(user2.username);
    });
});
```

### 3. API Client with Configuration Object

```javascript
class APIClient {
    constructor(config = {}) {
        this.baseURL = config.baseURL || 'https://api.example.com';
        this.timeout = config.timeout || 30000;
        this.retries = config.retries || 0;
        this.headers = {
            'Content-Type': 'application/json',
            ...config.headers
        };
        this.validateStatus = config.validateStatus !== false;
        this.logging = config.logging || false;
    }
    
    async request(endpoint, options = {}) {
        const url = `${this.baseURL}${endpoint}`;
        const method = options.method || 'GET';
        
        const requestConfig = {
            method,
            headers: { ...this.headers, ...options.headers }
        };
        
        if (options.body && method !== 'GET') {
            requestConfig.body = JSON.stringify(options.body);
        }
        
        if (this.logging) {
            console.log(`${method} ${url}`);
        }
        
        // Retry logic
        let lastError;
        for (let attempt = 0; attempt <= this.retries; attempt++) {
            try {
                const response = await fetch(url, requestConfig);
                
                if (this.validateStatus && !response.ok) {
                    throw new Error(`HTTP ${response.status}: ${response.statusText}`);
                }
                
                return await response.json();
            } catch (error) {
                lastError = error;
                if (this.logging) {
                    console.log(`Attempt ${attempt + 1} failed: ${error.message}`);
                }
            }
        }
        
        throw lastError;
    }
    
    // Convenience methods
    async get(endpoint, options = {}) {
        return this.request(endpoint, { ...options, method: 'GET' });
    }
    
    async post(endpoint, body, options = {}) {
        return this.request(endpoint, { ...options, method: 'POST', body });
    }
    
    async put(endpoint, body, options = {}) {
        return this.request(endpoint, { ...options, method: 'PUT', body });
    }
    
    async delete(endpoint, options = {}) {
        return this.request(endpoint, { ...options, method: 'DELETE' });
    }
}

// Usage
const api = new APIClient({
    baseURL: 'https://api.staging.example.com',
    timeout: 60000,
    retries: 3,
    headers: {
        'X-API-Key': 'secret-key-123'
    },
    logging: true
});

// Make API calls
const users = await api.get('/users');
const newUser = await api.post('/users', {
    username: 'new_user',
    email: 'new@example.com'
});
```

### 4. Test Report Generator

```javascript
class TestReport {
    constructor(options = {}) {
        this.suiteName = options.suiteName || 'Test Suite';
        this.tests = [];
        this.startTime = new Date();
        this.endTime = null;
        this.totalTests = 0;
        this.passedTests = 0;
        this.failedTests = 0;
        this.skippedTests = 0;
    }
    
    // Static factory methods for different report types
    static createUnitTestReport(suiteName) {
        return new TestReport({
            suiteName: `Unit Tests - ${suiteName}`
        });
    }
    
    static createIntegrationTestReport(suiteName) {
        return new TestReport({
            suiteName: `Integration Tests - ${suiteName}`
        });
    }
    
    static createE2ETestReport(suiteName) {
        return new TestReport({
            suiteName: `E2E Tests - ${suiteName}`
        });
    }
    
    addTest(test) {
        this.tests.push(test);
        this.totalTests++;
        
        if (test.status === 'passed') this.passedTests++;
        if (test.status === 'failed') this.failedTests++;
        if (test.status === 'skipped') this.skippedTests++;
    }
    
    complete() {
        this.endTime = new Date();
    }
    
    getDuration() {
        if (this.endTime) {
            return this.endTime - this.startTime;
        }
        return Date.now() - this.startTime;
    }
    
    getSummary() {
        return {
            suiteName: this.suiteName,
            totalTests: this.totalTests,
            passed: this.passedTests,
            failed: this.failedTests,
            skipped: this.skippedTests,
            duration: this.getDuration(),
            passRate: this.totalTests > 0 
                ? ((this.passedTests / this.totalTests) * 100).toFixed(2) + '%'
                : '0%'
        };
    }
    
    display() {
        console.log('='.repeat(50));
        console.log(`Test Suite: ${this.suiteName}`);
        console.log('='.repeat(50));
        console.log(`Total Tests: ${this.totalTests}`);
        console.log(`Passed: ${this.passedTests}`);
        console.log(`Failed: ${this.failedTests}`);
        console.log(`Skipped: ${this.skippedTests}`);
        console.log(`Duration: ${this.getDuration()}ms`);
        console.log(`Pass Rate: ${this.getSummary().passRate}`);
        console.log('='.repeat(50));
    }
}

// Usage
const report = TestReport.createE2ETestReport('Login Flow');

report.addTest({ name: 'Valid login', status: 'passed', duration: 1500 });
report.addTest({ name: 'Invalid login', status: 'failed', duration: 2000 });
report.addTest({ name: 'Logout', status: 'passed', duration: 500 });

report.complete();
report.display();
```

---

## Advanced Class Concepts

### 1. Static Members

```javascript
class DatabaseConnection {
    static activeConnections = 0;
    static maxConnections = 10;
    
    constructor(connectionString) {
        if (DatabaseConnection.activeConnections >= DatabaseConnection.maxConnections) {
            throw new Error('Maximum connections reached');
        }
        
        this.connectionString = connectionString;
        this.isConnected = false;
        DatabaseConnection.activeConnections++;
    }
    
    connect() {
        this.isConnected = true;
        console.log('Connected to database');
    }
    
    disconnect() {
        this.isConnected = false;
        DatabaseConnection.activeConnections--;
        console.log('Disconnected from database');
    }
    
    // Static method
    static getActiveConnections() {
        return DatabaseConnection.activeConnections;
    }
    
    // Static method
    static getAvailableConnections() {
        return DatabaseConnection.maxConnections - DatabaseConnection.activeConnections;
    }
}

// Usage
const db1 = new DatabaseConnection('localhost:5432/db1');
const db2 = new DatabaseConnection('localhost:5432/db2');

console.log(DatabaseConnection.getActiveConnections());  // 2
console.log(DatabaseConnection.getAvailableConnections());  // 8
```

### 2. Private Fields and Methods

```javascript
class SecureUser {
    // Private fields (ES2022)
    #password;
    #apiToken;
    
    constructor(username, password) {
        this.username = username;  // Public
        this.#password = password;  // Private
        this.#apiToken = this.#generateToken();  // Private
    }
    
    // Private method
    #generateToken() {
        return `token_${Math.random().toString(36).substr(2, 9)}`;
    }
    
    // Private method
    #hashPassword(password) {
        return `hashed_${password}`;
    }
    
    // Public method
    validatePassword(password) {
        return this.#hashPassword(password) === this.#password;
    }
    
    // Public method - safe access to private field
    getTokenHash() {
        return this.#apiToken.substring(0, 10) + '...';
    }
}

const user = new SecureUser('john', 'secret123');
console.log(user.username);           // Works: john
console.log(user.getTokenHash());     // Works: token_abc1...
// console.log(user.#password);       // SyntaxError: Private field
// console.log(user.#generateToken());// SyntaxError: Private method
```

### 3. Getters and Setters

```javascript
class TestConfig {
    constructor() {
        this._browser = 'chrome';
        this._timeout = 30000;
        this._retries = 0;
    }
    
    // Getter
    get browser() {
        return this._browser;
    }
    
    // Setter with validation
    set browser(value) {
        const validBrowsers = ['chrome', 'firefox', 'safari', 'edge'];
        if (!validBrowsers.includes(value)) {
            throw new Error(`Invalid browser: ${value}`);
        }
        this._browser = value;
    }
    
    // Getter
    get timeout() {
        return this._timeout;
    }
    
    // Setter with validation
    set timeout(value) {
        if (value < 1000 || value > 120000) {
            throw new Error('Timeout must be between 1000 and 120000');
        }
        this._timeout = value;
    }
    
    // Computed property
    get timeoutInSeconds() {
        return this._timeout / 1000;
    }
    
    // Getter
    get retries() {
        return this._retries;
    }
    
    // Setter with validation
    set retries(value) {
        if (value < 0 || value > 5) {
            throw new Error('Retries must be between 0 and 5');
        }
        this._retries = value;
    }
}

// Usage
const config = new TestConfig();
console.log(config.browser);      // chrome
config.browser = 'firefox';       // Valid
// config.browser = 'opera';      // Error: Invalid browser

config.timeout = 45000;
console.log(config.timeoutInSeconds);  // 45

config.retries = 3;
// config.retries = 10;           // Error: Retries must be between 0 and 5
```

### 4. Abstract Class Pattern (Simulation)

JavaScript doesn't have built-in abstract classes, but we can simulate them:

```javascript
class AbstractPage {
    constructor(page) {
        if (new.target === AbstractPage) {
            throw new Error('Cannot instantiate abstract class AbstractPage');
        }
        
        this.page = page;
        
        // Check if child class implemented required methods
        if (!this.getPageTitle) {
            throw new Error('Child class must implement getPageTitle()');
        }
        if (!this.navigate) {
            throw new Error('Child class must implement navigate()');
        }
    }
    
    // Concrete method (available to all child classes)
    async waitForElement(selector, timeout = 30000) {
        await this.page.waitForSelector(selector, { timeout });
    }
    
    async screenshot(filename) {
        await this.page.screenshot({ path: filename });
    }
}

class LoginPage extends AbstractPage {
    constructor(page) {
        super(page);
        this.url = 'https://example.com/login';
    }
    
    // Must implement
    async navigate() {
        await this.page.goto(this.url);
    }
    
    // Must implement
    async getPageTitle() {
        return await this.page.title();
    }
    
    async login(username, password) {
        await this.navigate();
        await this.page.fill('#username', username);
        await this.page.fill('#password', password);
        await this.page.click('#login-btn');
    }
}

// Usage
// const abstract = new AbstractPage(page);  // Error: Cannot instantiate abstract class
const loginPage = new LoginPage(page);      // Works fine
await loginPage.login('user', 'pass');
```

### 5. Mixins (Multiple Inheritance Simulation)

```javascript
// Mixin: Logging functionality
const LoggingMixin = (Base) => class extends Base {
    log(message) {
        console.log(`[${new Date().toISOString()}] ${message}`);
    }
    
    logError(error) {
        console.error(`[${new Date().toISOString()}] ERROR: ${error}`);
    }
};

// Mixin: Retry functionality
const RetryMixin = (Base) => class extends Base {
    async retryOperation(operation, maxRetries = 3) {
        for (let i = 0; i <= maxRetries; i++) {
            try {
                return await operation();
            } catch (error) {
                if (i === maxRetries) throw error;
                this.log(`Retry attempt ${i + 1} after error: ${error.message}`);
                await this.delay(1000 * (i + 1));
            }
        }
    }
    
    delay(ms) {
        return new Promise(resolve => setTimeout(resolve, ms));
    }
};

// Base class
class APIClient {
    constructor(baseURL) {
        this.baseURL = baseURL;
    }
    
    async get(endpoint) {
        const response = await fetch(`${this.baseURL}${endpoint}`);
        return await response.json();
    }
}

// Apply mixins
class EnhancedAPIClient extends RetryMixin(LoggingMixin(APIClient)) {
    async fetchWithRetry(endpoint) {
        this.log(`Fetching ${endpoint}`);
        return await this.retryOperation(async () => {
            return await this.get(endpoint);
        });
    }
}

// Usage
const api = new EnhancedAPIClient('https://api.example.com');
const data = await api.fetchWithRetry('/users');
```

---

## Interview Tips

### Common Interview Questions

**Q1: How many constructors can a JavaScript class have?**

**Answer**: A JavaScript class can have **only ONE constructor**. Unlike languages like Java or C++, JavaScript does not support constructor overloading. Attempting to define multiple constructors will result in a SyntaxError.

**Q2: What happens if you don't define a constructor?**

**Answer**: If you don't define a constructor, JavaScript provides a default constructor automatically:
```javascript
class User {
    // No constructor defined
}

// Equivalent to:
class User {
    constructor() {
        // Empty default constructor
    }
}

// For inherited classes:
class Admin extends User {
    // Default constructor calls super()
    constructor(...args) {
        super(...args);
    }
}
```

**Q3: What are the best alternatives to constructor overloading?**

**Answer**: The three best approaches are:
1. **Options Object Pattern** (most recommended) - Single object parameter with default values
2. **Static Factory Methods** - Different named methods for different creation patterns
3. **Builder Pattern** - Fluent interface for step-by-step object construction

**Q4: Can you explain the difference between class and constructor function?**

**Answer**:
```javascript
// Constructor function (ES5)
function User(name) {
    this.name = name;
}
User.prototype.greet = function() {
    return `Hello, ${this.name}`;
};

// Class syntax (ES6) - syntactic sugar over constructor functions
class User {
    constructor(name) {
        this.name = name;
    }
    
    greet() {
        return `Hello, ${this.name}`;
    }
}

// Both create the same result, but class syntax is:
// - More readable and intuitive
// - Stricter (must use 'new' keyword)
// - Supports inheritance better
// - Industry standard for modern JavaScript
```

**Q5: How do you create objects without using the 'new' keyword?**

**Answer**:
```javascript
class User {
    constructor(name) {
        this.name = name;
    }
    
    // Static factory method
    static create(name) {
        return new User(name);
    }
}

// Create object without directly using 'new'
const user = User.create('John');

// Or using Object.create()
const userProto = {
    greet() {
        return `Hello, ${this.name}`;
    }
};
const user2 = Object.create(userProto);
user2.name = 'Jane';
```

**Q6: What is the purpose of 'super()' in a constructor?**

**Answer**: `super()` calls the parent class constructor and **must be called** before accessing `this` in a child class constructor:
```javascript
class BasePage {
    constructor(page) {
        this.page = page;
    }
}

class LoginPage extends BasePage {
    constructor(page, url) {
        super(page);  // Must call before using 'this'
        this.url = url;
    }
}
```

**Q7: Explain private fields in classes**

**Answer**: ES2022 introduced private fields using `#` prefix:
```javascript
class BankAccount {
    #balance = 0;  // Private field
    
    constructor(initialBalance) {
        this.#balance = initialBalance;
    }
    
    deposit(amount) {
        this.#balance += amount;
    }
    
    getBalance() {
        return this.#balance;  // Can access inside class
    }
}

const account = new BankAccount(1000);
// console.log(account.#balance);  // SyntaxError
console.log(account.getBalance());  // Works: 1000
```

---

## Comparison Table

| Feature | ES6 Classes | Constructor Functions | Factory Functions |
|---------|-------------|----------------------|-------------------|
| **Syntax** | Clean, modern | Verbose | Flexible |
| **'new' required** | Yes (enforced) | Yes (not enforced) | No |
| **Inheritance** | `extends` keyword | Prototype chain | Custom implementation |
| **Private fields** | Yes (`#field`) | No | Via closures |
| **Static methods** | Yes | Manual addition | Manual addition |
| **Hoisting** | No | Yes | Yes |
| **Recommended** | ✅ Yes | ❌ Legacy | ✅ For specific patterns |

---

## Key Takeaways

1. ✅ JavaScript classes are created using `class` keyword (ES6+)
2. ✅ Objects are created from classes using the `new` keyword
3. ✅ A class can have **ONLY ONE constructor**
4. ✅ Constructor overloading is **NOT supported**
5. ✅ Use **options object pattern** for flexible constructors
6. ✅ Use **static factory methods** for named constructors
7. ✅ Use **builder pattern** for complex object creation
8. ✅ Classes support inheritance via `extends` keyword
9. ✅ Private fields use `#` prefix (ES2022+)
10. ✅ Static members belong to the class, not instances

---

## Quick Reference

### Class Syntax Template

```javascript
class ClassName {
    // Static property
    static staticProperty = value;
    
    // Private field
    #privateField;
    
    // Constructor (only one allowed)
    constructor(param1, param2) {
        this.publicProperty = param1;
        this.#privateField = param2;
    }
    
    // Instance method
    instanceMethod() {
        // method body
    }
    
    // Static method
    static staticMethod() {
        // method body
    }
    
    // Getter
    get propertyName() {
        return this._property;
    }
    
    // Setter
    set propertyName(value) {
        this._property = value;
    }
    
    // Private method
    #privateMethod() {
        // method body
    }
}

// Create object
const instance = new ClassName(arg1, arg2);

// Inheritance
class ChildClass extends ClassName {
    constructor(param1, param2, param3) {
        super(param1, param2);  // Call parent constructor
        this.childProperty = param3;
    }
}
```

---

## Additional Resources

- [MDN - Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
- [MDN - Constructor](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/constructor)
- [MDN - Private Class Features](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/Private_class_fields)
- [JavaScript.info - Classes](https://javascript.info/classes)

---

**Note**: This document is prepared for automation test architect interview preparation. Focus on understanding options object pattern and static factory methods as these are most commonly used in test automation frameworks like Playwright, Cypress, and Selenium!