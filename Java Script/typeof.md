# JavaScript `typeof` Operator - Notes

## Overview

The `typeof` operator is a unary operator in JavaScript that returns a string indicating the type of the operand's value. It's commonly used for type checking in automation testing and general JavaScript development.

## Syntax

```javascript
typeof operand
// or
typeof(operand)
```

## Return Values

The `typeof` operator returns one of the following string values:

| Type | Result |
|------|--------|
| Undefined | `"undefined"` |
| Null | `"object"` (known quirk) |
| Boolean | `"boolean"` |
| Number | `"number"` |
| BigInt | `"bigint"` |
| String | `"string"` |
| Symbol | `"symbol"` |
| Function | `"function"` |
| Object | `"object"` |

## Examples

### Primitive Types

```javascript
// Undefined
typeof undefined                  // "undefined"
typeof declaredButUndefined       // "undefined"

// Boolean
typeof true                       // "boolean"
typeof false                      // "boolean"

// Number
typeof 42                         // "number"
typeof 3.14159                    // "number"
typeof Infinity                   // "number"
typeof NaN                        // "number" (Not a Number is still of type number!)

// BigInt
typeof 42n                        // "bigint"
typeof BigInt(42)                 // "bigint"

// String
typeof "Hello"                    // "string"
typeof 'Automation Testing'       // "string"
typeof `Template literal`         // "string"

// Symbol
typeof Symbol()                   // "symbol"
typeof Symbol('description')      // "symbol"
```

### Object Types

```javascript
// Objects
typeof { name: "John" }           // "object"
typeof [1, 2, 3]                  // "object" (arrays are objects)
typeof new Date()                 // "object"
typeof /regex/                    // "object"

// Null (Special case - known bug in JavaScript)
typeof null                       // "object" (this is a bug, but maintained for compatibility)

// Functions
typeof function() {}              // "function"
typeof class C {}                 // "function"
typeof Math.sin                   // "function"
```

## Common Use Cases in Automation Testing

### 1. Input Validation

```javascript
function validateTestData(data) {
    if (typeof data !== 'object') {
        throw new Error('Test data must be an object');
    }
    
    if (typeof data.username !== 'string') {
        throw new Error('Username must be a string');
    }
    
    if (typeof data.age !== 'number') {
        throw new Error('Age must be a number');
    }
}
```

### 2. API Response Validation

```javascript
async function verifyAPIResponse(response) {
    // Check if response exists
    if (typeof response === 'undefined') {
        console.error('Response is undefined');
        return false;
    }
    
    // Validate response structure
    if (typeof response.data === 'object' && 
        typeof response.status === 'number' &&
        typeof response.message === 'string') {
        return true;
    }
    
    return false;
}
```

### 3. Dynamic Element Validation

```javascript
function validateElement(element) {
    if (typeof element === 'undefined' || element === null) {
        console.log('Element not found');
        return false;
    }
    
    if (typeof element.click === 'function') {
        console.log('Element is clickable');
        return true;
    }
    
    return false;
}
```

### 4. Configuration Checker

```javascript
function checkConfig(config) {
    const checks = {
        baseURL: typeof config.baseURL === 'string',
        timeout: typeof config.timeout === 'number',
        retries: typeof config.retries === 'number',
        headless: typeof config.headless === 'boolean',
        beforeEach: typeof config.beforeEach === 'function'
    };
    
    return Object.entries(checks).every(([key, value]) => {
        if (!value) console.error(`Invalid type for ${key}`);
        return value;
    });
}
```

## Important Gotchas and Edge Cases

### 1. The `null` Problem

```javascript
typeof null === "object"  // true (historical bug)

// Better way to check for null
function isNull(value) {
    return value === null;
}

// Check for null or undefined
function isNullOrUndefined(value) {
    return value === null || typeof value === 'undefined';
}
```

### 2. Arrays

```javascript
typeof []                 // "object"
typeof [1, 2, 3]         // "object"

// Better way to check for arrays
Array.isArray([])         // true
Array.isArray({})         // false
```

### 3. NaN (Not a Number)

```javascript
typeof NaN                // "number" (counterintuitive!)

// Better way to check for NaN
isNaN(NaN)                // true
Number.isNaN(NaN)         // true (more reliable)
```

### 4. Undeclared vs Undefined

```javascript
let declaredVar;
typeof declaredVar        // "undefined"
typeof undeclaredVar      // "undefined" (doesn't throw error)

// This is useful for checking if variables exist
if (typeof someVariable !== 'undefined') {
    // Safe to use someVariable
}
```

## Best Practices for Test Automation

### 1. Type Guards in TypeScript/JavaScript

```javascript
function isString(value) {
    return typeof value === 'string';
}

function isNumber(value) {
    return typeof value === 'number' && !isNaN(value);
}

function isFunction(value) {
    return typeof value === 'function';
}
```

### 2. Safe Property Access

```javascript
function safelyAccessProperty(obj, property) {
    if (typeof obj === 'object' && obj !== null) {
        return obj[property];
    }
    return undefined;
}
```

### 3. Test Data Validation

```javascript
class TestDataValidator {
    static validate(data, schema) {
        for (let [key, expectedType] of Object.entries(schema)) {
            const actualType = typeof data[key];
            
            if (actualType !== expectedType) {
                throw new Error(
                    `Invalid type for ${key}: expected ${expectedType}, got ${actualType}`
                );
            }
        }
        return true;
    }
}

// Usage
const schema = {
    username: 'string',
    age: 'number',
    isActive: 'boolean'
};

TestDataValidator.validate(testData, schema);
```

## Interview Tips

1. **Know the quirks**: Be prepared to explain why `typeof null` returns `"object"` (it's a historical bug from JavaScript's early days).

2. **Difference from `instanceof`**: `typeof` checks primitive types, while `instanceof` checks if an object is an instance of a class.

3. **Type coercion awareness**: Remember that `typeof` doesn't trigger type coercion - it checks the actual type.

4. **Use in conditional checks**: Common pattern is `typeof variable !== 'undefined'` to safely check if a variable exists.

5. **Alternative methods**: Know when to use `Array.isArray()`, `Number.isNaN()`, or `instanceof` instead of `typeof`.

## Quick Reference Cheat Sheet

```javascript
// Primitives
typeof undefined          → "undefined"
typeof true              → "boolean"
typeof 42                → "number"
typeof "text"            → "string"
typeof Symbol()          → "symbol"
typeof 42n               → "bigint"

// Special cases
typeof null              → "object" ⚠️
typeof NaN               → "number" ⚠️
typeof []                → "object" ⚠️

// Objects and Functions
typeof {}                → "object"
typeof function(){}      → "function"
typeof new Date()        → "object"
```

## Additional Resources

- [MDN Web Docs - typeof](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof)
- [JavaScript.info - Data types](https://javascript.info/types)

---

**Note**: This document is prepared for automation test architect interview preparation. Keep practicing with real-world examples from your test automation framework!