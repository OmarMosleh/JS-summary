# JavaScript ES5 - DOM Manipulation & Events Cheat Sheet

## Table of Contents
1. [Element Selection](#element-selection)
2. [DOM Manipulation](#dom-manipulation)
3. [Styling Elements](#styling-elements)
4. [Creating Elements](#creating-elements)
5. [Inserting Elements](#inserting-elements)
6. [Removing Elements](#removing-elements)
7. [Event Handling](#event-handling)
8. [Event Types](#event-types)
9. [Event Propagation](#event-propagation)
10. [Cross-Browser Compatibility](#cross-browser-compatibility)

---

## Element Selection

### Basic Selection Methods
```javascript
// By ID
var element = document.getElementById('myId');

// By Tag Name
var elements = document.getElementsByTagName('div');

// By Class Name (IE9+)
var elements = document.getElementsByClassName('myClass');

// By Name attribute
var elements = document.getElementsByName('myName');
```

### Modern Selectors (IE8+)
```javascript
// Single element (first match)
var element = document.querySelector('#myId');
var element = document.querySelector('.myClass');
var element = document.querySelector('div.myClass');

// All matching elements
var elements = document.querySelectorAll('.myClass');
var elements = document.querySelectorAll('div p');
```

### CSS Selector Examples
```javascript
// Basic selectors
document.querySelector('#header')           // ID
document.querySelector('.menu')            // Class
document.querySelector('div')              // Tag
document.querySelector('*')                // Universal

// Attribute selectors
document.querySelector('[data-id="123"]')  // Attribute value
document.querySelector('[type="text"]')    // Input type
document.querySelector('a[href^="http"]')  // Starts with

// Combinators
document.querySelector('div p')            // Descendant
document.querySelector('div > p')          // Direct child
document.querySelector('h2 + p')           // Adjacent sibling
document.querySelector('h2 ~ p')           // General sibling

// Pseudo-classes
document.querySelector('li:first-child')   // First child
document.querySelector('li:last-child')    // Last child
document.querySelector('tr:nth-child(2n)') // Even rows
```

---

## DOM Manipulation

### Text Content
```javascript
// Get/Set text content
element.textContent = 'New text';
var text = element.textContent;

// Get/Set HTML content
element.innerHTML = '<strong>Bold text</strong>';
var html = element.innerHTML;

// Legacy text property (avoid in modern code)
element.innerText = 'Text only';
```

### Attributes
```javascript
// Set attribute
element.setAttribute('data-id', '123');
element.setAttribute('class', 'highlight');

// Get attribute
var value = element.getAttribute('data-id');

// Remove attribute
element.removeAttribute('data-id');

// Check if attribute exists
var hasAttr = element.hasAttribute('data-id');

// Direct property access (for standard attributes)
element.id = 'newId';
element.className = 'newClass';
element.src = 'image.jpg';
```

### Properties vs Attributes
```javascript
// Properties (JavaScript object properties)
element.value = 'new value';
element.checked = true;
element.disabled = false;

// Attributes (HTML attributes)
element.setAttribute('value', 'new value');
element.setAttribute('checked', 'checked');
element.setAttribute('disabled', 'disabled');
```

---

## Styling Elements

### Inline Styles
```javascript
// Single style
element.style.color = 'red';
element.style.backgroundColor = 'blue';
element.style.fontSize = '16px';

// Multiple styles
element.style.cssText = 'color: red; background-color: blue; font-size: 16px;';

// Remove style
element.style.color = '';
element.style.removeProperty('color');

// Get computed style (read-only)
var computedStyle = window.getComputedStyle(element);
var color = computedStyle.color;
```

### CSS Property Names
```javascript
// CSS property -> JavaScript property
'background-color'    → backgroundColor
'font-size'          → fontSize
'border-radius'      → borderRadius
'margin-top'         → marginTop
'z-index'            → zIndex
```

### Class Manipulation (ES5 Compatible)
```javascript
// Add class
function addClass(element, className) {
    if (element.className.indexOf(className) === -1) {
        element.className += (element.className ? ' ' : '') + className;
    }
}

// Remove class
function removeClass(element, className) {
    var regex = new RegExp('\\b' + className + '\\b', 'g');
    element.className = element.className.replace(regex, '').replace(/\s+/g, ' ').trim();
}

// Check class
function hasClass(element, className) {
    return element.className.indexOf(className) !== -1;
}

// Toggle class
function toggleClass(element, className) {
    if (hasClass(element, className)) {
        removeClass(element, className);
    } else {
        addClass(element, className);
    }
}

// Modern browsers (IE10+) - classList API
element.classList.add('className');
element.classList.remove('className');
element.classList.toggle('className');
element.classList.contains('className');
```

---

## Creating Elements

### Basic Element Creation
```javascript
// Create element
var div = document.createElement('div');
var p = document.createElement('p');
var img = document.createElement('img');

// Set content and attributes
div.textContent = 'Hello World';
div.id = 'myDiv';
div.className = 'container';

// Create text node
var textNode = document.createTextNode('Plain text');

// Create document fragment (for multiple elements)
var fragment = document.createDocumentFragment();
fragment.appendChild(div);
fragment.appendChild(p);
```

### Element Building Pattern
```javascript
function createCard(title, content, buttonText) {
    var card = document.createElement('div');
    card.className = 'card';
    
    var cardTitle = document.createElement('h3');
    cardTitle.textContent = title;
    
    var cardContent = document.createElement('p');
    cardContent.textContent = content;
    
    var cardButton = document.createElement('button');
    cardButton.textContent = buttonText;
    cardButton.onclick = function() {
        alert('Button clicked!');
    };
    
    card.appendChild(cardTitle);
    card.appendChild(cardContent);
    card.appendChild(cardButton);
    
    return card;
}
```

---

## Inserting Elements

### appendChild Method
```javascript
// Append to end of parent
parentElement.appendChild(newElement);

// Move existing element (removes from old location)
newParent.appendChild(existingElement);
```

### insertBefore Method
```javascript
// Insert before reference element
parentElement.insertBefore(newElement, referenceElement);

// Insert as first child
parentElement.insertBefore(newElement, parentElement.firstChild);
```

### insertAdjacentElement (IE5+)
```javascript
// Insert at different positions
element.insertAdjacentElement('beforebegin', newElement); // Before element
element.insertAdjacentElement('afterbegin', newElement);  // As first child
element.insertAdjacentElement('beforeend', newElement);   // As last child
element.insertAdjacentElement('afterend', newElement);    // After element
```

### insertAdjacentHTML
```javascript
// Insert HTML strings
element.insertAdjacentHTML('beforebegin', '<p>Before</p>');
element.insertAdjacentHTML('afterbegin', '<span>Start</span>');
element.insertAdjacentHTML('beforeend', '<span>End</span>');
element.insertAdjacentHTML('afterend', '<p>After</p>');
```

---

## Removing Elements

### removeChild Method
```javascript
// Remove child element
parentElement.removeChild(childElement);

// Remove element (if you have reference to parent)
element.parentNode.removeChild(element);

// Remove all children
while (element.firstChild) {
    element.removeChild(element.firstChild);
}
```

### Modern remove Method (IE Edge+)
```javascript
// Direct removal
element.remove();

// Fallback for older browsers
if (!Element.prototype.remove) {
    Element.prototype.remove = function() {
        if (this.parentNode) {
            this.parentNode.removeChild(this);
        }
    };
}
```

### Clearing Content
```javascript
// Clear all content
element.innerHTML = '';
element.textContent = '';

// Remove specific children
var children = element.children;
for (var i = children.length - 1; i >= 0; i--) {
    element.removeChild(children[i]);
}
```

---

## Event Handling

### Three Ways to Add Events

#### 1. HTML Attribute (Inline)
```html
<button onclick="myFunction()">Click me</button>
<input onchange="handleChange()" />
```

#### 2. DOM Property
```javascript
element.onclick = function() {
    console.log('Clicked!');
};

element.onchange = function(event) {
    console.log('Changed:', event.target.value);
};

// Only one handler per event type
element.onclick = null; // Remove handler
```

#### 3. addEventListener (Recommended)
```javascript
// Add event listener
element.addEventListener('click', function() {
    console.log('Clicked!');
});

// Multiple handlers allowed
element.addEventListener('click', handler1);
element.addEventListener('click', handler2);

// Remove event listener
element.removeEventListener('click', handler1);

// Event options (IE9+)
element.addEventListener('click', handler, false); // Bubbling phase
element.addEventListener('click', handler, true);  // Capturing phase
```

### Cross-Browser Event Handling
```javascript
function addEvent(element, event, handler) {
    if (element.addEventListener) {
        element.addEventListener(event, handler, false);
    } else if (element.attachEvent) {
        element.attachEvent('on' + event, handler);
    } else {
        element['on' + event] = handler;
    }
}

function removeEvent(element, event, handler) {
    if (element.removeEventListener) {
        element.removeEventListener(event, handler, false);
    } else if (element.detachEvent) {
        element.detachEvent('on' + event, handler);
    } else {
        element['on' + event] = null;
    }
}
```

---

## Event Types

### Mouse Events
```javascript
element.addEventListener('click', handler);       // Mouse click
element.addEventListener('dblclick', handler);    // Double click
element.addEventListener('mousedown', handler);   // Mouse button pressed
element.addEventListener('mouseup', handler);     // Mouse button released
element.addEventListener('mouseover', handler);   // Mouse enters element
element.addEventListener('mouseout', handler);    // Mouse leaves element
element.addEventListener('mouseenter', handler);  // Mouse enters (no bubbling)
element.addEventListener('mouseleave', handler);  // Mouse leaves (no bubbling)
element.addEventListener('mousemove', handler);   // Mouse moves over element
element.addEventListener('contextmenu', handler); // Right click context menu
```

### Keyboard Events
```javascript
element.addEventListener('keydown', handler);     // Key pressed down
element.addEventListener('keyup', handler);       // Key released
element.addEventListener('keypress', handler);    // Key pressed (deprecated)

// Key event properties
function keyHandler(event) {
    console.log('Key:', event.key);           // Modern property
    console.log('Code:', event.code);         // Physical key
    console.log('KeyCode:', event.keyCode);   // Legacy property
    console.log('Which:', event.which);       // Legacy property
}
```

### Form Events
```javascript
element.addEventListener('submit', handler);      // Form submission
element.addEventListener('change', handler);      // Input value changed
element.addEventListener('input', handler);       // Input value changing
element.addEventListener('focus', handler);       // Element gains focus
element.addEventListener('blur', handler);        // Element loses focus
element.addEventListener('select', handler);      // Text selected
element.addEventListener('reset', handler);       // Form reset
```

### Window/Document Events
```javascript
window.addEventListener('load', handler);         // Page fully loaded
window.addEventListener('DOMContentLoaded', handler); // DOM ready
window.addEventListener('resize', handler);       // Window resized
window.addEventListener('scroll', handler);       // Page scrolled
window.addEventListener('beforeunload', handler); // Before page unload
```

---

## Event Propagation

### Event Phases
```javascript
// 1. Capturing phase (top to bottom)
element.addEventListener('click', handler, true);

// 2. Target phase (at the target element)
// 3. Bubbling phase (bottom to top)
element.addEventListener('click', handler, false); // Default
```

### Event Object Properties
```javascript
function eventHandler(event) {
    event = event || window.event; // IE compatibility
    
    console.log('Target:', event.target);           // Element that triggered event
    console.log('CurrentTarget:', event.currentTarget); // Element with listener
    console.log('Type:', event.type);               // Event type
    console.log('Phase:', event.eventPhase);        // Event phase
    console.log('Bubbles:', event.bubbles);         // Can bubble
    console.log('Cancelable:', event.cancelable);   // Can be canceled
}
```

### Stopping Propagation
```javascript
function stopPropagation(event) {
    event = event || window.event;
    
    if (event.stopPropagation) {
        event.stopPropagation(); // Modern browsers
    } else {
        event.cancelBubble = true; // IE
    }
}

function stopImmediatePropagation(event) {
    if (event.stopImmediatePropagation) {
        event.stopImmediatePropagation(); // Stops all handlers
    }
}
```

### Preventing Default Behavior
```javascript
function preventDefault(event) {
    event = event || window.event;
    
    if (event.preventDefault) {
        event.preventDefault(); // Modern browsers
    } else {
        event.returnValue = false; // IE
    }
    
    return false; // Additional prevention
}

// Usage examples
function handleSubmit(event) {
    preventDefault(event);
    // Custom form handling
}

function handleLink(event) {
    preventDefault(event);
    // Custom navigation
}
```

---

## Event Delegation

### Basic Delegation Pattern
```javascript
// Instead of adding listeners to each button
var buttons = document.querySelectorAll('.btn');
for (var i = 0; i < buttons.length; i++) {
    buttons[i].addEventListener('click', handleClick);
}

// Use delegation on parent container
var container = document.getElementById('button-container');
container.addEventListener('click', function(event) {
    if (event.target.classList.contains('btn')) {
        handleClick(event);
    }
});
```

### Advanced Delegation
```javascript
function delegate(parent, selector, event, handler) {
    parent.addEventListener(event, function(e) {
        var target = e.target;
        
        // Check if target matches selector
        if (target.matches && target.matches(selector)) {
            handler.call(target, e);
        } else {
            // Check if any parent matches (for nested elements)
            while (target && target !== parent) {
                if (target.matches && target.matches(selector)) {
                    handler.call(target, e);
                    break;
                }
                target = target.parentNode;
            }
        }
    });
}

// Usage
delegate(document.body, '.btn', 'click', function(event) {
    console.log('Button clicked:', this.textContent);
});
```

---

## Cross-Browser Compatibility

### Feature Detection
```javascript
// Check for addEventListener support
if (element.addEventListener) {
    element.addEventListener('click', handler, false);
} else if (element.attachEvent) {
    element.attachEvent('onclick', handler);
} else {
    element.onclick = handler;
}

// Check for querySelector support
if (document.querySelector) {
    var element = document.querySelector('.myClass');
} else {
    var element = document.getElementsByClassName('myClass')[0];
}

// Check for classList support
if (element.classList) {
    element.classList.add('active');
} else {
    addClass(element, 'active'); // Use polyfill function
}
```

### Polyfills for ES5 Compatibility
```javascript
// matches polyfill
if (!Element.prototype.matches) {
    Element.prototype.matches = 
        Element.prototype.matchesSelector ||
        Element.prototype.mozMatchesSelector ||
        Element.prototype.msMatchesSelector ||
        Element.prototype.oMatchesSelector ||
        Element.prototype.webkitMatchesSelector ||
        function(s) {
            var matches = (this.document || this.ownerDocument).querySelectorAll(s);
            var i = matches.length;
            while (--i >= 0 && matches.item(i) !== this) {}
            return i > -1;
        };
}

// closest polyfill
if (!Element.prototype.closest) {
    Element.prototype.closest = function(s) {
        var el = this;
        do {
            if (el.matches(s)) return el;
            el = el.parentElement || el.parentNode;
        } while (el !== null && el.nodeType === 1);
        return null;
    };
}
```

### Common IE8+ Solutions
```javascript
// Get event object
function getEvent(event) {
    return event || window.event;
}

// Get event target
function getTarget(event) {
    return event.target || event.srcElement;
}

// Cross-browser ready state
function documentReady(callback) {
    if (document.readyState === 'complete' || 
        (document.readyState !== 'loading' && !document.documentElement.doScroll)) {
        callback();
    } else {
        document.addEventListener('DOMContentLoaded', callback);
    }
}
```

---

## Quick Reference

### Most Common Operations
```javascript
// Selection
var el = document.getElementById('myId');
var els = document.querySelectorAll('.myClass');

// Content
el.textContent = 'text';
el.innerHTML = '<strong>html</strong>';

// Attributes
el.setAttribute('data-id', '123');
var value = el.getAttribute('data-id');

// Styling
el.style.color = 'red';
el.className += ' newClass';

// Events
el.addEventListener('click', function() {
    console.log('clicked');
});

// Creating
var newEl = document.createElement('div');
newEl.textContent = 'content';
parent.appendChild(newEl);

// Removing
parent.removeChild(el);
```

### Performance Tips
- Use event delegation for dynamic content
- Cache DOM queries in variables
- Use DocumentFragment for multiple insertions
- Minimize DOM modifications in loops
- Use CSS classes instead of inline styles when possible
