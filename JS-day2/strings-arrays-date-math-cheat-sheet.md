# JavaScript ES5 - Strings, Arrays, Date & Math Cheat Sheet

## Table of Contents
1. [String Methods](#string-methods)
2. [Arrays](#arrays)
3. [Array Methods](#array-methods)
4. [Date Object](#date-object)
5. [Math Object](#math-object)

## String Methods

### String Extraction
```javascript
var text = "JavaScript ES5";

// substring(start, end) - negative values treated as 0
text.substring(0, 4);        // "Java"
text.substring(4, 10);       // "Script"

// substr(start, length) - DEPRECATED
text.substr(0, 4);           // "Java"
text.substr(4, 6);           // "Script"

// slice(start, end) - handles negative indices
text.slice(0, 4);            // "Java"
text.slice(-3);              // "ES5"
text.slice(0, -4);           // "JavaScript"
```

### Search Methods
```javascript
var text = "Hello World Hello";

text.indexOf("Hello");       // 0 (first occurrence)
text.lastIndexOf("Hello");   // 12 (last occurrence)
text.charAt(0);              // "H"
text.charCodeAt(0);          // 72 (ASCII code)
```

### Case and Formatting
```javascript
var text = "  JavaScript ES5  ";

text.toLowerCase();          // "  javascript es5  "
text.toUpperCase();          // "  JAVASCRIPT ES5  "
text.trim();                 // "JavaScript ES5" (ES5)

// Replace and split
text.replace("ES5", "ES6");  // Replace first occurrence
text.split(" ");             // ["", "", "JavaScript", "ES5", "", ""]
```

### Concatenation
```javascript
var a = "Hello", b = "World";

a.concat(" ", b);            // "Hello World"
a + " " + b;                 // "Hello World" (more common)
```

## Arrays

### Creation and Access
```javascript
// Array creation
var fruits = ["apple", "banana", "orange"];
var numbers = new Array(1, 2, 3, 4, 5);
var empty = new Array(5);            // 5 empty slots

// Access and modify
fruits[0];                           // "apple"
fruits.length;                       // 3
fruits[1] = "grape";                 // Modify element
fruits[fruits.length] = "kiwi";      // Add to end
```

### Iteration
```javascript
var fruits = ["apple", "banana", "orange"];

// for loop
for (var i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// for...in loop
for (var index in fruits) {
    console.log(fruits[index]);
}
```

## Array Methods

### Mutating Methods (Modify Original Array)

#### Add/Remove Elements
```javascript
var arr = ["a", "b"];

// Add/remove at end
arr.push("c");               // ["a", "b", "c"] - returns new length
arr.pop();                   // ["a", "b"] - returns removed element

// Add/remove at beginning
arr.unshift("z");            // ["z", "a", "b"] - returns new length
arr.shift();                 // ["a", "b"] - returns removed element
```

#### splice() - Swiss Army Knife
```javascript
var arr = ["a", "b", "c", "d"];

// Remove elements
arr.splice(1, 2);            // ["a", "d"] - removes 2 from index 1

// Add elements
arr.splice(1, 0, "x", "y");  // ["a", "x", "y", "d"] - insert at index 1

// Replace elements
arr.splice(1, 2, "z");       // ["a", "z", "d"] - replace 2 with "z"
```

#### Sorting and Reversing
```javascript
var numbers = [3, 1, 4, 1, 5];
var letters = ["c", "a", "b"];

letters.sort();              // ["a", "b", "c"]
numbers.sort();              // [1, 1, 3, 4, 5] (string sort!)

// Numeric sort
numbers.sort(function(a, b) { return a - b; }); // [1, 1, 3, 4, 5]
numbers.sort(function(a, b) { return b - a; }); // [5, 4, 3, 1, 1]

arr.reverse();               // Reverse in place
```

### Non-Mutating Methods (Return New Array/Value)

#### Basic Operations
```javascript
var arr = ["a", "b", "c"];

// Join and slice
arr.join();                  // "a,b,c"
arr.join(" - ");             // "a - b - c"
arr.slice(1);                // ["b", "c"]
arr.slice(0, 2);             // ["a", "b"]

// Concatenation
arr.concat(["d", "e"]);      // ["a", "b", "c", "d", "e"]
```

#### Search Methods
```javascript
var arr = [1, 2, 3, 2, 4];

arr.indexOf(2);              // 1 (first occurrence)
arr.lastIndexOf(2);          // 3 (last occurrence)
arr.indexOf(5);              // -1 (not found)
```

#### ES5 Iteration Methods
```javascript
var numbers = [1, 2, 3, 4, 5];

// forEach - execute function for each element
numbers.forEach(function(num, index) {
    console.log(index + ": " + num);
});

// map - transform each element
var doubled = numbers.map(function(num) {
    return num * 2;
});                          // [2, 4, 6, 8, 10]

// filter - elements that pass test
var evens = numbers.filter(function(num) {
    return num % 2 === 0;
});                          // [2, 4]

// reduce - reduce to single value
var sum = numbers.reduce(function(acc, num) {
    return acc + num;
}, 0);                       // 15

// some - test if any element passes
var hasEven = numbers.some(function(num) {
    return num % 2 === 0;
});                          // true

// every - test if all elements pass
var allPositive = numbers.every(function(num) {
    return num > 0;
});                          // true
```

## Date Object

### Creation
```javascript
var now = new Date();                    // Current date/time
var specific = new Date(2023, 0, 15);    // Jan 15, 2023 (month is 0-based!)
var withTime = new Date(2023, 0, 15, 14, 30, 0); // With time
var fromString = new Date("2023-01-15");
var fromTimestamp = new Date(1673740800000);
```

### Getting Components
```javascript
var date = new Date(2023, 0, 15, 14, 30, 45);

date.getFullYear();          // 2023
date.getMonth();             // 0 (January = 0!)
date.getDate();              // 15 (day of month)
date.getDay();               // 0 (Sunday = 0, Monday = 1, etc.)
date.getHours();             // 14 (24-hour format)
date.getMinutes();           // 30
date.getSeconds();           // 45
date.getTime();              // Milliseconds since Jan 1, 1970
```

### Setting Components
```javascript
var date = new Date();

date.setFullYear(2024);
date.setMonth(5);            // June (0-based)
date.setDate(20);
date.setHours(16);
date.setMinutes(45);
date.setSeconds(30);
date.setTime(1640995200000); // Set from timestamp
```

### Formatting
```javascript
var date = new Date(2023, 0, 15, 14, 30, 45);

date.toString();             // Full date string
date.toDateString();         // "Sun Jan 15 2023"
date.toTimeString();         // "14:30:45 GMT..."
date.toLocaleDateString();   // "1/15/2023" (US format)
date.toLocaleTimeString();   // "2:30:45 PM"
date.toISOString();          // "2023-01-15T14:30:45.000Z"

// Custom formatting
function formatDate(date) {
    var day = ("0" + date.getDate()).slice(-2);
    var month = ("0" + (date.getMonth() + 1)).slice(-2);
    var year = date.getFullYear();
    return day + "/" + month + "/" + year;
}
```

### Date Calculations
```javascript
// Days between dates
function daysBetween(date1, date2) {
    var oneDay = 24 * 60 * 60 * 1000;
    return Math.round(Math.abs((date2 - date1) / oneDay));
}

// Add days
function addDays(date, days) {
    var result = new Date(date);
    result.setDate(result.getDate() + days);
    return result;
}

// Age calculation
function calculateAge(birthDate) {
    var today = new Date();
    var age = today.getFullYear() - birthDate.getFullYear();
    var monthDiff = today.getMonth() - birthDate.getMonth();
    
    if (monthDiff < 0 || (monthDiff === 0 && today.getDate() < birthDate.getDate())) {
        age--;
    }
    return age;
}
```

## Math Object

### Constants
```javascript
Math.PI;                     // 3.141592653589793
Math.E;                      // 2.718281828459045
Math.LN2;                    // 0.6931471805599453
Math.LN10;                   // 2.302585092994046
Math.SQRT2;                  // 1.4142135623730951
```

### Rounding
```javascript
var num = 4.7;

Math.round(4.3);             // 4
Math.round(4.7);             // 5
Math.floor(4.7);             // 4 (round down)
Math.ceil(4.3);              // 5 (round up)

// Round to decimal places
function roundTo(num, decimals) {
    var factor = Math.pow(10, decimals);
    return Math.round(num * factor) / factor;
}
```

### Min, Max, Absolute
```javascript
Math.min(5, 3, 8, 1);        // 1
Math.max(5, 3, 8, 1);        // 8
Math.abs(-5);                // 5

// With arrays
var numbers = [5, 3, 8, 1];
Math.min.apply(null, numbers); // 1
Math.max.apply(null, numbers); // 8
```

### Power and Roots
```javascript
Math.pow(2, 3);              // 8 (2³)
Math.sqrt(16);               // 4
Math.pow(27, 1/3);           // 3 (cube root)
```


### Random Numbers
```javascript
Math.random();               // 0-1 (exclusive)

// Random integer between min and max (inclusive)
function randomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
}

// Random float between min and max
function randomFloat(min, max) {
    return Math.random() * (max - min) + min;
}

// Random array element
function randomChoice(array) {
    return array[Math.floor(Math.random() * array.length)];
}
```

## Quick Reference

### Common String Operations
```javascript
// Get file extension
filename.substring(filename.lastIndexOf('.') + 1);

// Capitalize first letter
str.charAt(0).toUpperCase() + str.slice(1);

// Remove whitespace
str.trim();                  // Both ends (ES5)

// Count occurrences
function countChar(str, char) {
    return str.split(char).length - 1;
}
```

### Common Array Operations
```javascript
// Remove duplicates
function unique(arr) {
    return arr.filter(function(item, index) {
        return arr.indexOf(item) === index;
    });
}



// Find maximum
function findMax(arr) {
    return Math.max.apply(null, arr);
}
```

### Common Date Operations
```javascript
// Format as YYYY-MM-DD
function formatISO(date) {
    return date.getFullYear() + '-' + 
           ('0' + (date.getMonth() + 1)).slice(-2) + '-' + 
           ('0' + date.getDate()).slice(-2);
}

// Check if leap year
function isLeapYear(year) {
    return (year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0);
}

// Get days in month
function getDaysInMonth(year, month) {
    return new Date(year, month + 1, 0).getDate();
}
```

### Common Math Operations
```javascript

// Generate random ID
function randomId(length) {
    var chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
    var result = '';
    for (var i = 0; i < length; i++) {
        result += chars.charAt(Math.floor(Math.random() * chars.length));
    }
    return result;
}
```

## Array Method Categories

### Mutating (Change Original)
- `push()`, `pop()`, `shift()`, `unshift()`
- `splice()`, `sort()`, `reverse()`

### Non-Mutating (Return New/Value)
- `slice()`, `concat()`, `join()`
- `indexOf()`, `lastIndexOf()`
- `forEach()`, `map()`, `filter()`, `reduce()`, `some()`, `every()`

---

*Remember: Always consider whether you need to modify the original array or create a new one!*
