# JavaScript ES5 Objects - Quick Reference Guide

## Table of Contents
1. [Object Creation](#object-creation)
2. [Property Access](#property-access)
3. [Methods and 'this'](#methods-and-this)
4. [Constructor Functions](#constructor-functions)
5. [Prototype](#prototype)
6. [Inheritance](#inheritance)
7. [Built-in Methods](#built-in-methods)
8. [Advanced Features](#advanced-features)

## Object Creation

### Object Literal
```javascript
var obj = {
    property: "value",
    method: function() { return "result"; }
};
```

### Object Constructor
```javascript
var obj = new Object();
obj.property = "value";
```

### Object.create()
```javascript
var obj = Object.create(null); // No prototype
var obj2 = Object.create(Object.prototype); // With prototype
```

## Property Access

### Dot Notation
```javascript
obj.property        // Get
obj.property = val; // Set
```

### Bracket Notation
```javascript
obj["property"]        // Get
obj["property"] = val; // Set
obj[variable]          // Dynamic access
```

### Property Existence
```javascript
"property" in obj                    // true/false
obj.hasOwnProperty("property")       // true/false (own properties only)
obj.property !== undefined          // true/false
```

## Methods and 'this'

### Basic Method
```javascript
var obj = {
    value: 42,
    getValue: function() {
        return this.value; // 'this' refers to obj
    }
};
```

### Method Context Issues
```javascript
var method = obj.getValue;
method();              // 'this' context lost
method.call(obj);      // Explicitly set 'this'
method.apply(obj);     // Explicitly set 'this'
```

## Constructor Functions

### Basic Constructor
```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
    this.greet = function() {
        return "Hello, I'm " + this.name;
    };
}

var person = new Person("John", 30);
```

### What 'new' Does
1. Creates empty object
2. Sets prototype
3. Binds 'this' to new object
4. Executes constructor
5. Returns new object

## Prototype

### Adding Methods to Prototype
```javascript
function Car(brand) {
    this.brand = brand;
}

Car.prototype.start = function() {
    return this.brand + " started";
};

var car = new Car("Toyota");
car.start(); // Uses prototype method
```

### Prototype vs Instance Properties
```javascript
function Student(name) {
    this.name = name;           // Instance property
}

Student.prototype.school = "Default"; // Prototype property

var s1 = new Student("Alice");
var s2 = new Student("Bob");

// Both share prototype property
console.log(s1.school); // "Default"
console.log(s2.school); // "Default"
```

## Inheritance

### Basic Inheritance Pattern
```javascript
// Parent
function Animal(name) {
    this.name = name;
}

Animal.prototype.speak = function() {
    return this.name + " makes a sound";
};

// Child
function Dog(name, breed) {
    Animal.call(this, name); // Call parent constructor
    this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Override method
Dog.prototype.speak = function() {
    return this.name + " barks";
};
```

## Built-in Methods

### Object.keys()
```javascript
var obj = {a: 1, b: 2, c: 3};
Object.keys(obj); // ["a", "b", "c"]
```

### Object.getOwnPropertyNames()
```javascript
Object.getOwnPropertyNames(obj); // All own properties (enumerable + non-enumerable)
```

### hasOwnProperty()
```javascript
obj.hasOwnProperty("property"); // true/false (own properties only)
```

### propertyIsEnumerable()
```javascript
obj.propertyIsEnumerable("property"); // true/false
```

## Advanced Features

### Property Descriptors
```javascript
Object.defineProperty(obj, "property", {
    value: "value",
    writable: true,     // Can be changed
    enumerable: true,   // Shows in for...in
    configurable: true  // Can be deleted/reconfigured
});
```

### Getters and Setters
```javascript
var obj = {
    _value: 0,
    get value() {
        return this._value;
    },
    set value(val) {
        this._value = val;
    }
};
```

### Object Sealing and Freezing
```javascript
Object.seal(obj);    // No add/remove properties, can modify
Object.freeze(obj);  // No changes at all

Object.isSealed(obj);  // Check if sealed
Object.isFrozen(obj);  // Check if frozen
```

## Common Patterns

### Module Pattern (using closures)
```javascript
var module = (function() {
    var privateVar = "private";
    
    return {
        publicMethod: function() {
            return privateVar;
        }
    };
})();
```

### Factory Function
```javascript
function createPerson(name, age) {
    return {
        name: name,
        age: age,
        greet: function() {
            return "Hello, I'm " + this.name;
        }
    };
}

var person = createPerson("John", 30);
```

### Mixin Pattern
```javascript
var Mixin = {
    method1: function() { /* ... */ },
    method2: function() { /* ... */ }
};

function extend(target, source) {
    for (var key in source) {
        if (source.hasOwnProperty(key)) {
            target[key] = source[key];
        }
    }
    return target;
}

var obj = {};
extend(obj, Mixin); // obj now has method1 and method2
```

## Best Practices

1. **Use object literals for simple objects**
2. **Use constructor functions for multiple similar objects**
3. **Add methods to prototype for memory efficiency**
4. **Use meaningful property names**
5. **Check property existence before accessing**
6. **Be careful with 'this' context**
7. **Use strict mode to catch errors**
8. **Prefer composition over inheritance when possible**

## Common Pitfalls

1. **Forgetting 'new' with constructors**
   ```javascript
   // Wrong: this becomes global object
   var person = Person("John");
   
   // Correct
   var person = new Person("John");
   ```

2. **Losing 'this' context**
   ```javascript
   var method = obj.method;
   method(); // 'this' is not obj anymore
   ```

3. **Modifying prototype of built-in objects**
   ```javascript
   // Don't do this
   Array.prototype.myMethod = function() {};
   ```

4. **Creating methods inside constructor (memory waste)**
   ```javascript
   // Inefficient: each instance gets its own copy
   function Person(name) {
       this.greet = function() { return "Hello"; };
   }
   
   // Better: shared method
   Person.prototype.greet = function() { return "Hello"; };
   ```

## Summary

Objects in JavaScript ES5 are fundamental building blocks that allow you to:
- Group related data and functionality
- Create reusable code with constructor functions
- Implement inheritance through prototypes
- Control property behavior with descriptors
- Build complex applications with modular design

