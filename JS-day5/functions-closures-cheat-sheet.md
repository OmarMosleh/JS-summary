# JavaScript ES5/ES6 - Functions, Closures & Modern Features Cheat Sheet

## Table of Contents
1. [Function Types](#function-types)
2. [Function Hoisting](#function-hoisting)
3. [First-Class Functions](#first-class-functions)
4. [Arguments Object](#arguments-object)
5. [Closures](#closures)
6. [IIFE Pattern](#iife-pattern)
7. [this Context](#this-context)
8. [Call, Apply, Bind](#call-apply-bind)
9. [let, const vs var](#let-const-vs-var)
10. [Template Literals](#template-literals)
11. [Arrow Functions](#arrow-functions)

---

## Function Types

### Function Declaration (Statement)
```javascript
// Hoisted - can be called before declaration
sum(5, 6); // Works!

function sum(a, b) {
    return a + b;
}

// Characteristics:
// ✅ Hoisted
// ✅ Named function
// ✅ Function name is mandatory
```

### Function Expression
```javascript
// Not hoisted - cannot be called before declaration
// multiply(3, 4); // Error!

// Anonymous function expression
var multiply = function(a, b) {
    return a * b;
};

// Named function expression
var factorial = function fact(n) {
    if (n <= 1) return 1;
    return n * fact(n - 1); // Can use 'fact' internally
};

// Characteristics:
// ❌ Not hoisted
// ⚠️ Function name optional
// ✅ Assigned to variable
```

---

## Function Hoisting

### How Hoisting Works
```javascript
// What you write:
console.log(myFunc()); // "I'm hoisted!"
var myVar = 5;

function myFunc() {
    return "I'm hoisted!";
}

// How JavaScript sees it:
function myFunc() {
    return "I'm hoisted!";
}
var myVar; // undefined

console.log(myFunc()); // "I'm hoisted!"
myVar = 5;
```

### Function vs Variable Hoisting
```javascript
// Function declarations - fully hoisted
console.log(hoistedFn()); // Works

function hoistedFn() {
    return "I work!";
}

// Function expressions - only variable hoisted
console.log(notHoisted); // undefined
console.log(notHoisted()); // Error!

var notHoisted = function() {
    return "I don't work before assignment";
};
```

---

## First-Class Functions

### 1. Assign to Variables
```javascript
var greet = function(name) {
    return "Hello " + name;
};

var sayHello = greet; // Copy reference
console.log(sayHello("Omar")); // "Hello Omar"
```

### 2. Pass as Parameters
```javascript
function processUser(name, callback) {
    console.log("Processing " + name);
    callback(name);
}

function welcomeUser(name) {
    console.log("Welcome " + name);
}

processUser("Omar", welcomeUser);
```

### 3. Return from Functions
```javascript
function createMultiplier(factor) {
    return function(number) {
        return number * factor;
    };
}

var double = createMultiplier(2);
var triple = createMultiplier(3);

console.log(double(5)); // 10
console.log(triple(4)); // 12
```

### 4. Store in Arrays/Objects
```javascript
var mathOps = [
    function(a, b) { return a + b; },
    function(a, b) { return a - b; },
    function(a, b) { return a * b; }
];

console.log(mathOps[0](5, 3)); // 8
console.log(mathOps[1](5, 3)); // 2
console.log(mathOps[2](5, 3)); // 15
```

---

## Arguments Object

### Basic Usage
```javascript
function flexibleSum() {
    var total = 0;
    
    // Arguments is array-like but not a real array
    console.log(arguments.length);
    console.log(typeof arguments); // "object"
    console.log(Array.isArray(arguments)); // false
    
    for (var i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    
    return total;
}

console.log(flexibleSum(1, 2, 3)); // 6
console.log(flexibleSum(1, 2, 3, 4, 5)); // 15
```

### Convert to Real Array
```javascript
function processArgs() {
    // ES5 way
    var argsArray = Array.prototype.slice.call(arguments);
    
    // Now you can use array methods
    argsArray.push("new item");
    var doubled = argsArray.map(function(item) {
        return item * 2;
    });
    
    return doubled;
}

// Modern way (ES6+)
function processArgsES6(...args) {
    // args is already a real array
    return args.map(item => item * 2);
}
```

### Arguments vs Named Parameters
```javascript
// Using arguments - flexible but unclear
function oldWay() {
    var name = arguments[0];
    var age = arguments[1];
    var city = arguments[2];
    // What parameters does this function expect?
}

// Named parameters - clear and self-documenting
function newWay(name, age, city) {
    // Clear what this function expects
}

// Default parameters (ES6+)
function withDefaults(name = "Guest", age = 0) {
    return `${name} is ${age} years old`;
}
```

---

## Closures

### Basic Closure
```javascript
function outer() {
    var count = 0; // Private variable
    
    return function inner() {
        count++; // Access outer variable
        console.log(count);
    };
}

var counter1 = outer();
var counter2 = outer();

counter1(); // 1
counter1(); // 2
counter2(); // 1 (separate instance)
```

### Practical Closure Patterns

#### Data Privacy
```javascript
function createBankAccount(initialBalance) {
    var balance = initialBalance; // Private
    
    return {
        deposit: function(amount) {
            balance += amount;
            return balance;
        },
        withdraw: function(amount) {
            if (amount <= balance) {
                balance -= amount;
                return balance;
            }
            return "Insufficient funds";
        },
        getBalance: function() {
            return balance;
        }
        // balance is not directly accessible
    };
}

var account = createBankAccount(100);
console.log(account.getBalance()); // 100
account.deposit(50); // 150
console.log(account.balance); // undefined (private)
```

#### Function Factory
```javascript
function createValidator(type) {
    return function(value) {
        switch(type) {
            case 'email':
                return value.includes('@');
            case 'phone':
                return /^\d{10}$/.test(value);
            case 'required':
                return value && value.trim().length > 0;
            default:
                return true;
        }
    };
}

var emailValidator = createValidator('email');
var phoneValidator = createValidator('phone');

console.log(emailValidator('test@example.com')); // true
console.log(phoneValidator('1234567890')); // true
```

#### Once Function
```javascript
function once(fn) {
    var called = false;
    var result;
    
    return function() {
        if (!called) {
            called = true;
            result = fn.apply(this, arguments);
        }
        return result;
    };
}

var expensiveOperation = once(function(data) {
    console.log("Processing expensive operation...");
    return data * 2;
});

console.log(expensiveOperation(5)); // Processes and returns 10
console.log(expensiveOperation(5)); // Returns cached 10
```

---

## IIFE Pattern

### Basic IIFE
```javascript
// Syntax variations
(function() {
    console.log("IIFE executed!");
})();

(function() {
    console.log("Alternative syntax!");
}());

// With parameters
(function(name, version) {
    console.log(name + " v" + version);
})("MyApp", "1.0");
```

### Module Pattern
```javascript
var MyModule = (function() {
    // Private variables
    var privateVar = "secret";
    var counter = 0;
    
    // Private functions
    function privateFunction() {
        return "This is private";
    }
    
    // Public API
    return {
        publicMethod: function() {
            return "Accessing: " + privateVar;
        },
        
        increment: function() {
            counter++;
            return counter;
        },
        
        reset: function() {
            counter = 0;
        },
        
        getCount: function() {
            return counter;
        }
    };
})();

// Usage
console.log(MyModule.publicMethod()); // "Accessing: secret"
MyModule.increment(); // 1
console.log(MyModule.privateVar); // undefined
```

### Avoiding Global Pollution
```javascript
// Without IIFE - pollutes global scope
var userCount = 0;
var maxUsers = 100;
function addUser() { /* ... */ }
function removeUser() { /* ... */ }

// With IIFE - clean global scope
var UserManager = (function() {
    var userCount = 0;
    var maxUsers = 100;
    
    return {
        addUser: function() { /* ... */ },
        removeUser: function() { /* ... */ },
        getUserCount: function() { return userCount; }
    };
})();
```

---

## this Context

### this in Different Contexts
```javascript
// Global context
console.log(this); // window (browser) or global (Node.js)

// Object method
var user = {
    name: "Omar",
    greet: function() {
        console.log("Hello " + this.name); // this = user
    }
};

// Function call
function standalone() {
    console.log(this); // window (non-strict) or undefined (strict)
}

// Constructor
function Person(name) {
    this.name = name; // this = new instance
}
var person = new Person("Ali");
```

### Common this Problems
```javascript
var user = {
    name: "Omar",
    greet: function() {
        console.log("Hello " + this.name);
    }
};

user.greet(); // "Hello Omar" (this = user)

var greetFn = user.greet;
greetFn(); // "Hello undefined" (this = window/global)

// Callback problem
setTimeout(user.greet, 1000); // "Hello undefined"

// Solutions:
// 1. Bind
setTimeout(user.greet.bind(user), 1000); // "Hello Omar"

// 2. Arrow function
setTimeout(() => user.greet(), 1000); // "Hello Omar"

// 3. Store reference
var self = user;
setTimeout(function() { self.greet(); }, 1000); // "Hello Omar"
```

---

## Call, Apply, Bind

### call() Method
```javascript
// Syntax: fn.call(thisArg, arg1, arg2, ...)

var user1 = { name: "Omar" };
var user2 = { name: "Ali" };

function greet(greeting, punctuation) {
    console.log(greeting + " " + this.name + punctuation);
}

greet.call(user1, "Hello", "!"); // "Hello Omar!"
greet.call(user2, "Hi", "?"); // "Hi Ali?"

// Borrowing methods
var numbers = [1, 2, 3, 4, 5];
var max = Math.max.call(null, 1, 2, 3, 4, 5); // 5
```

### apply() Method
```javascript
// Syntax: fn.apply(thisArg, [arg1, arg2, ...])

var user = { name: "Omar" };
function greet(greeting, punctuation) {
    console.log(greeting + " " + this.name + punctuation);
}

greet.apply(user, ["Hello", "!"]); // "Hello Omar!"

// Useful with arrays
var numbers = [1, 2, 3, 4, 5];
var max = Math.max.apply(null, numbers); // 5

// Converting arguments to array
function toArray() {
    return Array.prototype.slice.apply(arguments);
}
```

### bind() Method
```javascript
// Syntax: fn.bind(thisArg, arg1, arg2, ...)

var user = { name: "Omar" };
function greet(greeting, punctuation) {
    console.log(greeting + " " + this.name + punctuation);
}

// Create bound function
var greetOmar = greet.bind(user);
greetOmar("Hello", "!"); // "Hello Omar!"

// Partial application
var sayHelloToOmar = greet.bind(user, "Hello");
sayHelloToOmar("!"); // "Hello Omar!"
sayHelloToOmar("?"); // "Hello Omar?"

// Event handlers
button.addEventListener('click', this.handleClick.bind(this));
```

### call vs apply vs bind
```javascript
var obj = { name: "Test" };
function fn(a, b, c) {
    console.log(this.name, a, b, c);
}

// call - individual arguments, executes immediately
fn.call(obj, 1, 2, 3); // "Test 1 2 3"

// apply - array of arguments, executes immediately
fn.apply(obj, [1, 2, 3]); // "Test 1 2 3"

// bind - returns new function, doesn't execute
var boundFn = fn.bind(obj, 1, 2);
boundFn(3); // "Test 1 2 3"
```

---

## let, const vs var

### Scope Differences
```javascript
// var - function scoped
function varTest() {
    if (true) {
        var x = 1;
    }
    console.log(x); // 1 (accessible outside block)
}

// let - block scoped
function letTest() {
    if (true) {
        let y = 1;
    }
    console.log(y); // ReferenceError: y is not defined
}

// const - block scoped, immutable binding
function constTest() {
    if (true) {
        const z = 1;
        // z = 2; // TypeError: Assignment to constant variable
    }
    console.log(z); // ReferenceError: z is not defined
}
```

### Hoisting Differences
```javascript
// var - hoisted and initialized with undefined
console.log(varVariable); // undefined
var varVariable = 5;

// let/const - hoisted but not initialized (temporal dead zone)
console.log(letVariable); // ReferenceError
let letVariable = 5;

console.log(constVariable); // ReferenceError
const constVariable = 5;
```

### Loop Variable Capture
```javascript
// Problem with var
for (var i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 3, 3, 3
    }, 100);
}

// Solution with let
for (let i = 0; i < 3; i++) {
    setTimeout(function() {
        console.log(i); // 0, 1, 2
    }, 100);
}

// ES5 solution with IIFE
for (var i = 0; i < 3; i++) {
    (function(j) {
        setTimeout(function() {
            console.log(j); // 0, 1, 2
        }, 100);
    })(i);
}
```

### const with Objects/Arrays
```javascript
// const prevents reassignment, not mutation
const obj = { name: "Omar" };
obj.name = "Ali"; // ✅ Allowed
obj.age = 28; // ✅ Allowed
// obj = {}; // ❌ TypeError

const arr = [1, 2, 3];
arr.push(4); // ✅ Allowed
arr[0] = 0; // ✅ Allowed
// arr = []; // ❌ TypeError

// To make truly immutable:
const frozenObj = Object.freeze({ name: "Omar" });
// frozenObj.name = "Ali"; // Silently fails in non-strict mode
```

---

## Template Literals

### Basic Syntax
```javascript
const name = "Omar";
const age = 28;

// Old way
const oldString = "Hello " + name + ", you are " + age + " years old";

// Template literal
const newString = `Hello ${name}, you are ${age} years old`;

// Expressions
const mathString = `5 + 3 = ${5 + 3}`;
const conditionalString = `You are ${age >= 18 ? 'adult' : 'minor'}`;
```

### Multiline Strings
```javascript
// Old way
const oldMultiline = "Line 1\n" +
                     "Line 2\n" +
                     "Line 3";

// Template literal
const newMultiline = `Line 1
Line 2
Line 3`;

// HTML generation
const htmlTemplate = `
<div class="card">
    <h3>${title}</h3>
    <p>${description}</p>
    <button onclick="handleClick(${id})">Click Me</button>
</div>`;
```

### Advanced Features
```javascript
// Function calls
function formatDate(date) {
    return date.toLocaleDateString();
}

const message = `Today is ${formatDate(new Date())}`;

// Nested templates
const items = ['apple', 'banana', 'orange'];
const list = `
<ul>
${items.map(item => `    <li>${item}</li>`).join('\n')}
</ul>`;

// Tagged template literals (advanced)
function highlight(strings, ...values) {
    return strings.reduce((result, string, i) => {
        const value = values[i] ? `<mark>${values[i]}</mark>` : '';
        return result + string + value;
    }, '');
}

const highlighted = highlight`Hello ${'world'} and ${'everyone'}!`;
// "Hello <mark>world</mark> and <mark>everyone</mark>!"
```

---

## Arrow Functions

### Syntax Variations
```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function - full form
const addArrow = (a, b) => {
    return a + b;
};

// Arrow function - concise form
const addConcise = (a, b) => a + b;

// Single parameter (parentheses optional)
const square = x => x * x;
const squareWithParens = (x) => x * x;

// No parameters (parentheses required)
const sayHello = () => "Hello";

// Multiple statements (braces required)
const complexOperation = (a, b) => {
    const sum = a + b;
    const product = a * b;
    return { sum, product };
};

// Returning objects (wrap in parentheses)
const createUser = (name, age) => ({ name, age });
```

### When to Use Arrow Functions
```javascript
// ✅ Good for: Array methods
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

// ✅ Good for: Short callbacks
setTimeout(() => console.log("Timer fired"), 1000);

// ✅ Good for: Preserving 'this' context
class Counter {
    constructor() {
        this.count = 0;
    }
    
    start() {
        setInterval(() => {
            this.count++; // 'this' refers to Counter instance
            console.log(this.count);
        }, 1000);
    }
}

// ❌ Avoid for: Object methods
const user = {
    name: "Omar",
    greet: () => {
        console.log(`Hello ${this.name}`); // 'this' is not user!
    }
};

// ✅ Use regular function for object methods
const user = {
    name: "Omar",
    greet() {
        console.log(`Hello ${this.name}`); // 'this' is user
    }
};
```

### Arrow Functions and 'this'
```javascript
// Arrow functions don't have their own 'this'
function Timer() {
    this.seconds = 0;
    
    // Problem with regular function
    setInterval(function() {
        this.seconds++; // 'this' is window/global!
        console.log(this.seconds); // NaN
    }, 1000);
    
    // Solution with arrow function
    setInterval(() => {
        this.seconds++; // 'this' is Timer instance
        console.log(this.seconds); // 1, 2, 3...
    }, 1000);
}

// Event handling
class Button {
    constructor(element) {
        this.element = element;
        this.clickCount = 0;
        
        // Arrow function preserves 'this'
        this.element.addEventListener('click', () => {
            this.clickCount++;
            console.log(`Clicked ${this.clickCount} times`);
        });
    }
}
```

### Arrow Function Limitations
```javascript
// ❌ No 'arguments' object
const arrowFn = () => {
    console.log(arguments); // ReferenceError
};

// ✅ Use rest parameters instead
const restFn = (...args) => {
    console.log(args); // [1, 2, 3]
};

// ❌ Cannot be constructors
const ArrowConstructor = () => {};
// new ArrowConstructor(); // TypeError

// ❌ No 'super' in arrow functions
class Parent {
    method() {
        return "parent";
    }
}

class Child extends Parent {
    method() {
        // return super.method(); // ✅ Works in regular method
    }
    
    arrowMethod = () => {
        // return super.method(); // ❌ SyntaxError
    }
}
```

---

## Quick Reference

### Function Comparison
| Feature | Function Declaration | Function Expression | Arrow Function |
|---------|---------------------|-------------------|----------------|
| Hoisting | ✅ Yes | ❌ No | ❌ No |
| 'this' binding | ✅ Dynamic | ✅ Dynamic | ❌ Lexical |
| 'arguments' object | ✅ Yes | ✅ Yes | ❌ No |
| Can be constructor | ✅ Yes | ✅ Yes | ❌ No |
| Method shorthand | ❌ No | ❌ No | ✅ Yes |

### ES6 Features Summary
```javascript
// Variables
const API_URL = "https://api.example.com"; // Immutable binding
let counter = 0; // Block scoped, mutable
// Avoid var in modern code

// Template literals
const message = `Hello ${name}, today is ${new Date().toDateString()}`;

// Arrow functions
const processData = data => data.map(item => item.value);

// Destructuring (bonus)
const { name, age } = user;
const [first, second] = array;

// Default parameters
const greet = (name = "Guest") => `Hello ${name}`;

// Rest parameters
const sum = (...numbers) => numbers.reduce((a, b) => a + b, 0);
```

### Best Practices
- Use `const` by default, `let` when reassignment needed
- Prefer arrow functions for callbacks and array methods
- Use regular functions for object methods and constructors
- Use template literals for string interpolation
- Leverage closures for data privacy and module patterns
- Use IIFE to avoid global namespace pollution
- Understand 'this' context in different scenarios
