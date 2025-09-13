# JavaScript ES5 - BOM & DOM Traversal Cheat Sheet

## BOM (Browser Object Model)

### Window Object
```javascript
// Window dimensions
window.innerWidth          // Viewport width
window.innerHeight         // Viewport height
window.outerWidth          // Browser window width
window.outerHeight         // Browser window height

// Window position
window.screenX             // Horizontal position
window.screenY             // Vertical position (screenTop in IE)

// Window methods
window.open(url, name, features)  // Open new window
window.close()                    // Close current window
window.moveTo(x, y)              // Move window to position
window.resizeTo(width, height)   // Resize window
```

### Navigator Object
```javascript
// Browser information
navigator.userAgent        // User agent string
navigator.platform         // Operating system
navigator.language         // Browser language
navigator.languages        // Array of languages (if supported)

// Browser capabilities
navigator.onLine           // Online status
navigator.cookieEnabled    // Cookies enabled
navigator.javaEnabled()    // Java enabled

// Browser detection (use with caution)
var isChrome = navigator.userAgent.indexOf('Chrome') > -1;
var isFirefox = navigator.userAgent.indexOf('Firefox') > -1;
var isSafari = navigator.userAgent.indexOf('Safari') > -1 && 
               navigator.userAgent.indexOf('Chrome') === -1;
```

### Location Object
```javascript
// URL components
location.href              // Complete URL
location.protocol          // Protocol (http:, https:)
location.hostname          // Domain name
location.host              // Hostname + port
location.port              // Port number
location.pathname          // Path after domain
location.search            // Query string (?param=value)
location.hash              // Fragment identifier (#section)

// Navigation methods
location.assign(url)       // Load new page
location.replace(url)      // Replace current page (no history)
location.reload()          // Reload current page
location.reload(true)      // Force reload from server
```

### History Object
```javascript
// History navigation
history.length             // Number of entries
history.back()             // Go back one page
history.forward()          // Go forward one page
history.go(-2)             // Go back 2 pages
history.go(1)              // Go forward 1 page

// Modern history API (if supported)
if (window.history && window.history.pushState) {
    history.pushState(state, title, url);   // Add history entry
    history.replaceState(state, title, url); // Replace current entry
}
```

### Screen Object
```javascript
// Screen dimensions
screen.width               // Total screen width
screen.height              // Total screen height
screen.availWidth          // Available screen width
screen.availHeight         // Available screen height

// Screen properties
screen.colorDepth          // Color depth
screen.pixelDepth          // Pixel depth (same as colorDepth)

// Orientation detection
var orientation = screen.width > screen.height ? 'landscape' : 'portrait';
```

### Timer Functions
```javascript
// Single execution
var timeoutId = setTimeout(function() {
    console.log("Executed after delay");
}, 1000);
clearTimeout(timeoutId);   // Cancel timeout

// Repeated execution
var intervalId = setInterval(function() {
    console.log("Executed repeatedly");
}, 1000);
clearInterval(intervalId); // Cancel interval

// Timer management pattern
var timers = [];
function addTimer(callback, delay, repeat) {
    var id = repeat ? 
        setInterval(callback, delay) : 
        setTimeout(callback, delay);
    timers.push(id);
    return id;
}
function clearAllTimers() {
    for (var i = 0; i < timers.length; i++) {
        clearTimeout(timers[i]);
        clearInterval(timers[i]);
    }
    timers = [];
}
```

## DOM Traversal

### Node vs Element
```javascript
// Node types
Node.ELEMENT_NODE          // 1 - Element
Node.TEXT_NODE             // 3 - Text
Node.COMMENT_NODE          // 8 - Comment

// Node properties
node.nodeName              // Node name (uppercase for elements)
node.nodeType              // Node type number
node.nodeValue             // Node value (null for elements)

// Element properties (only for element nodes)
element.tagName            // Tag name (uppercase)
element.id                 // Element ID
element.className          // CSS classes
```

### Child Access Methods
```javascript
// All child nodes (includes text nodes)
parent.childNodes          // NodeList of all children
parent.firstChild          // First child node
parent.lastChild           // Last child node

// Element children only
parent.children            // HTMLCollection of element children
parent.firstElementChild   // First element child
parent.lastElementChild    // Last element child
parent.childElementCount   // Number of element children
```

### Sibling Navigation
```javascript
// All siblings (includes text nodes)
node.previousSibling       // Previous sibling node
node.nextSibling           // Next sibling node

// Element siblings only
element.previousElementSibling  // Previous element sibling
element.nextElementSibling      // Next element sibling
```

### Parent Access
```javascript
// Parent access
node.parentNode            // Parent node
element.parentElement      // Parent element (null for document)

// Root elements
document.documentElement   // <html> element
document.body              // <body> element
document.head              // <head> element
```

### DOM Collections
```javascript
// Document collections
document.forms             // All forms
document.images            // All images
document.links             // All links (with href)
document.anchors           // All anchors (with name)

// Collection access
collection.length          // Number of items
collection[index]          // Access by index
collection.namedItem(name) // Access by name/id
collection.item(index)     // Access by index (alternative)

// Form collections
form.elements              // All form elements
form.elements.length       // Number of form elements
form.elements[name]        // Access by name
form.elements.namedItem(name) // Access by name (alternative)
```

### Collection Iteration Patterns
```javascript
// Standard for loop
for (var i = 0; i < collection.length; i++) {
    var item = collection[i];
    // Process item
}

// Convert to array (for array methods)
var array = [];
for (var i = 0; i < collection.length; i++) {
    array.push(collection[i]);
}

// Array-like iteration function
function forEachInCollection(collection, callback) {
    for (var i = 0; i < collection.length; i++) {
        callback(collection[i], i, collection);
    }
}
```

### Element Selection Methods
```javascript
// By tag name
document.getElementsByTagName('div')     // All div elements
element.getElementsByTagName('p')        // Descendant paragraphs

// By class name (IE9+)
if (document.getElementsByClassName) {
    document.getElementsByClassName('myClass');
}

// By name attribute
document.getElementsByName('username');  // Elements with name="username"

// Single element selection
document.getElementById('myId');         // Element with id="myId"
```

### DOM Tree Walking
```javascript
// Recursive tree traversal
function walkTree(element, callback, depth) {
    depth = depth || 0;
    
    if (element && element.nodeType === 1) {
        callback(element, depth);
        
        for (var i = 0; i < element.children.length; i++) {
            walkTree(element.children[i], callback, depth + 1);
        }
    }
}

// Usage
walkTree(document.body, function(element, depth) {
    var indent = '  '.repeat(depth);
    console.log(indent + element.tagName);
});
```

### Practical Utilities
```javascript
// Safe element access
function safeGetElement(id) {
    var element = document.getElementById(id);
    if (!element) {
        console.warn('Element not found: ' + id);
    }
    return element;
}

// Get all siblings
function getAllSiblings(element) {
    var siblings = [];
    var sibling = element.parentNode.firstElementChild;
    
    while (sibling) {
        if (sibling !== element) {
            siblings.push(sibling);
        }
        sibling = sibling.nextElementSibling;
    }
    
    return siblings;
}

// Count elements recursively
function countElements(parent) {
    var count = 1; // Count this element
    
    for (var i = 0; i < parent.children.length; i++) {
        count += countElements(parent.children[i]);
    }
    
    return count;
}

// Find elements by predicate
function findElements(parent, predicate) {
    var results = [];
    
    function search(element) {
        if (predicate(element)) {
            results.push(element);
        }
        
        for (var i = 0; i < element.children.length; i++) {
            search(element.children[i]);
        }
    }
    
    search(parent);
    return results;
}
```


## Browser Support Notes

- **Collections**: All browsers support basic collections (forms, images, links)
- **Element traversal**: firstElementChild/lastElementChild supported in IE9+
- **getElementsByClassName**: IE9+ (use fallback for IE8-)
- **History API**: pushState/replaceState in modern browsers only
- **Timer accuracy**: setTimeout/setInterval may be throttled in background tabs
- **Popup blocking**: Most browsers block popups unless user-initiated

## Security Considerations

- **Popup blocking**: Always check if popup was successfully opened
- **Same-origin policy**: Cross-domain access restrictions apply
- **User agent spoofing**: Don't rely solely on navigator.userAgent for critical features
- **History manipulation**: Be careful with sensitive information in URLs
- **Timer precision**: Don't use for precise timing or security-critical operations
