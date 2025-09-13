# JavaScript ES5 Introduction - Cheat Sheet

## Table of Contents
1. [JavaScript ES5 Introduction](#javascript-es5-introduction)
2. [JavaScript Features](#javascript-features)
3. [Where to Write JavaScript](#where-to-write-javascript)
4. [Numbers - Literal vs Constructor](#numbers---literal-vs-constructor)
5. [Data Types](#data-types)
6. [toString() Method](#tostring-method)
7. [Dialogs and Return Values](#dialogs-and-return-values)
8. [NaN (Not a Number)](#nan-not-a-number)
9. [Type Casting](#type-casting)
10. [Variable Declaration with var](#variable-declaration-with-var)

## JavaScript ES5 Introduction

### What is JavaScript?
- High-level, interpreted programming language
- Makes web pages interactive and dynamic
- Released in 1995 by Brendan Eich
- ES5 (ECMAScript 5) released in 2009

```javascript
// Basic JavaScript
console.log("Hello, JavaScript ES5!");
document.write("Welcome to JavaScript!");
```

## JavaScript Features

### 1. Loosely Typed (Dynamically Typed)
- No need to declare variable types explicitly
- Variables can change types during execution

```javascript
var name = "John";          // String
var age = 25;               // Number
var isStudent = true;       // Boolean

// Variables can change types
var dynamic = "Hello";      // String
dynamic = 42;               // Now Number
dynamic = false;            // Now Boolean
```

### 2. Object-Based Language
- Everything is treated as an object or can be converted to one
- Even primitive types have object-like behavior

```javascript
var str = "Hello";
console.log(str.length);        // 5
console.log(str.toUpperCase()); // "HELLO"

var num = 3.14159;
console.log(num.toFixed(2));    // "3.14"
```

### 3. Interpreted Language
- Code is executed line by line at runtime
- No compilation step needed

```javascript
console.log("This runs first");
console.log("This runs second");
var result = 5 + 3;
console.log("Result: " + result);
```

## Where to Write JavaScript

### 1. Inline JavaScript
```html
<button onclick="alert('Clicked!')">Click Me</button>
<a href="javascript:void(0)" onclick="console.log('Link')">Link</a>
```

### 2. Internal JavaScript
```html
<script>
    function greet() {
        alert("Hello World!");
    }
    window.onload = function() {
        console.log("Page loaded!");
    };
</script>
```

### 3. External JavaScript (Recommended)
```html
<script src="script.js"></script>
```

## Numbers - Literal vs Constructor

### Number Literals (Recommended)
```javascript
var integer = 42;
var decimal = 3.14;
var scientific = 2.5e3;        // 2500
var binary = 0b1010;           // 10
var octal = 0o777;             // 511
var hex = 0xFF;                // 255
```

### Number Constructor
```javascript
var numObj = new Number(42);    // Creates object
var numFunc = Number("123");    // Creates primitive

console.log(typeof 42);             // "number"
console.log(typeof new Number(42)); // "object"
console.log(typeof Number("123"));  // "number"
```

## Data Types

### Primitive Types (5 types in ES5)
```javascript
// 1. Number
var age = 25;
var price = 99.99;

// 2. String
var name = "John";
var message = 'Hello';

// 3. Boolean
var isActive = true;
var isComplete = false;

// 4. Undefined
var notDefined;
var explicit = undefined;

// 5. Null
var empty = null;
```

### Non-Primitive Types
```javascript
// Object
var person = { name: "Alice", age: 30 };

// Array (special object)
var numbers = [1, 2, 3, 4, 5];

// Function (special object)
var greet = function(name) {
    return "Hello, " + name;
};

// Date
var now = new Date();
```

### Type Checking
```javascript
console.log(typeof 42);         // "number"
console.log(typeof "hello");    // "string"
console.log(typeof true);       // "boolean"
console.log(typeof undefined);  // "undefined"
console.log(typeof null);       // "object" (quirk!)
console.log(typeof {});         // "object"
console.log(typeof []);         // "object"
console.log(typeof function(){}); // "function"
```

## toString() Method

### Number toString()
```javascript
var num = 255;
console.log(num.toString());    // "255"
console.log(num.toString(2));   // "11111111" (binary)
console.log(num.toString(8));   // "377" (octal)
console.log(num.toString(16));  // "ff" (hexadecimal)
```

### Boolean toString()
```javascript
console.log(true.toString());   // "true"
console.log(false.toString());  // "false"
```

### Array toString()
```javascript
var arr = [1, 2, 3];
console.log(arr.toString());    // "1,2,3"
```

### Custom toString()
```javascript
var person = {
    name: "John",
    age: 30,
    toString: function() {
        return this.name + " (" + this.age + ")";
    }
};
console.log(person.toString()); // "John (30)"
```

## Dialogs and Return Values

### alert() - Returns undefined
```javascript
var result = alert("Hello!");
console.log(result);            // undefined

alert("Welcome!");
alert("Form submitted!");
```

### confirm() - Returns boolean
```javascript
var choice = confirm("Continue?");
console.log(choice);            // true or false

if (confirm("Delete item?")) {
    console.log("Deleted");
} else {
    console.log("Cancelled");
}
```

### prompt() - Returns string or null
```javascript
var input = prompt("Your name?");
console.log(input);             // String or null

var age = prompt("Your age?", "25");
console.log(typeof age);        // "string" (always!)

// Handle responses
var name = prompt("Name:");
if (name === null) {
    console.log("Cancelled");
} else if (name === "") {
    console.log("Empty");
} else {
    console.log("Hello, " + name);
}
```

## NaN (Not a Number)

### What Produces NaN?
```javascript
console.log(0 / 0);             // NaN
console.log("hello" * 2);       // NaN
console.log(Math.sqrt(-1));     // NaN
console.log(parseInt("abc"));   // NaN
```

### NaN Properties
```javascript
console.log(typeof NaN);        // "number"
console.log(NaN === NaN);       // false!
console.log(NaN == NaN);        // false!
```

### Detecting NaN
```javascript
// Use isNaN() (with caution)
console.log(isNaN(NaN));        // true
console.log(isNaN("hello"));    // true (coercion!)
console.log(isNaN("123"));      // false (coercion!)

// Safe NaN check
function isSafeNaN(value) {
    return value !== value;     // Only NaN fails this
}

console.log(isSafeNaN(NaN));    // true
console.log(isSafeNaN("hello"));// false
```

## Type Casting

### Implicit Casting (Type Coercion)
```javascript
// String concatenation
console.log("5" + 3);           // "53"
console.log(5 + "3");           // "53"

// Arithmetic operations
console.log("10" - 5);          // 5
console.log("10" * "2");        // 20
console.log("20" / "4");        // 5

// Boolean context
console.log("5" == 5);          // true
console.log(true == 1);         // true
console.log(false == 0);        // true
```

### Explicit Casting

#### To String
```javascript
String(123);                    // "123"
(123).toString();               // "123"
123 + "";                       // "123"
```

#### To Number
```javascript
Number("123");                  // 123
parseInt("123");                // 123
parseFloat("123.45");           // 123.45
+"123";                         // 123 (unary +)
```

#### To Boolean
```javascript
Boolean(1);                     // true
Boolean(0);                     // false
Boolean("");                    // false
Boolean("hello");               // true
!!"hello";                      // true (double negation)
```

### Falsy Values
```javascript
// These are falsy in JavaScript:
false
0
""
null
undefined
NaN
```

## Variable Declaration with var

### Basic Declaration
```javascript
var name;                       // undefined
var age = 25;                   // declared and initialized
var city = "NYC", country = "USA"; // multiple declarations
```

### Function Scope
```javascript
function demo() {
    var functionVar = "function scope";
    
    if (true) {
        var blockVar = "still function scope";
    }
    
    console.log(blockVar);      // accessible!
    
    for (var i = 0; i < 3; i++) {
        var loopVar = "loop variable";
    }
    
    console.log(i);             // 3 (still accessible!)
}
```

### Global Scope
```javascript
var globalVar = "global";

function test() {
    console.log(globalVar);     // accessible
    implicitGlobal = "oops";    // creates global (avoid!)
}

// In browser: window.globalVar
```

### Variable Hoisting
```javascript
// What you write:
function example() {
    console.log(hoisted);       // undefined (not error!)
    var hoisted = "value";
    console.log(hoisted);       // "value"
}

// How JavaScript interprets it:
function example() {
    var hoisted;                // declaration hoisted
    console.log(hoisted);       // undefined
    hoisted = "value";          // assignment stays
    console.log(hoisted);       // "value"
}
```

### Common Issues and Solutions



### Best Practices
```javascript
// 1. Declare at top of function
function good() {
    var name, age, isActive;    // declarations at top
    
    name = "John";
    age = 30;
    isActive = true;
}

// 2. Initialize when declaring
function better() {
    var name = "";
    var age = 0;
    var isActive = false;
}
```

## Quick Reference

### Type Checking Utilities
```javascript
function getType(value) {
    if (value === null) return 'null';
    if (Array.isArray(value)) return 'array';
    return typeof value;
}
```

### Safe Parsing
```javascript
function safeParseInt(str) {
    var num = parseInt(str, 10);
    return isNaN(num) ? 0 : num;
}

function safeParseFloat(str) {
    var num = parseFloat(str);
    return isNaN(num) ? 0 : num;
}
```

### Common Patterns
```javascript
// Check if variable exists and is not null/undefined
if (typeof myVar !== 'undefined' && myVar !== null) {
    // use myVar
}

// Provide default value
var value = someValue || "default";

// Convert to boolean
var bool = !!someValue;

// Check if number is valid
if (!isNaN(num) && isFinite(num)) {
    // valid number
}
```

---