Here's a comprehensive list of JavaScript features across different categories:

## **String Methods & Properties**

### **String Methods:**
```javascript
// Creation
const str = "Hello World";
const strObj = new String("Hello");

// Properties
str.length

// Case manipulation
str.toUpperCase()
str.toLowerCase()
str.toLocaleUpperCase()
str.toLocaleLowerCase()

// Search & Query
str.charAt(0)
str.charCodeAt(0)
str.codePointAt(0)
str.indexOf('o')
str.lastIndexOf('o')
str.includes('World')
str.startsWith('Hello')
str.endsWith('World')
str.search(/World/)

// Extraction
str.slice(0, 5)
str.substring(0, 5)
str.substr(0, 5)
str.split(' ')

// Modification
str.concat('!')
str.repeat(2)
str.replace('World', 'JavaScript')
str.replaceAll('l', 'L')
str.padStart(15, '-')
str.padEnd(15, '-')
str.trim()
str.trimStart()
str.trimEnd()

// Unicode
str.normalize()
str.localeCompare('Hello')

// Template Literals
`Hello ${name}`
String.raw`Hello\nWorld`
```

## **Number Methods & Properties**

### **Number Methods:**
```javascript
// Conversion
Number.parseInt('123')
Number.parseFloat('123.45')
Number.isInteger(123)
Number.isFinite(123)
Number.isNaN(NaN)
Number.isSafeInteger(123)

// Instance methods
num.toString()
num.toFixed(2)
num.toExponential(2)
num.toPrecision(4)
num.toLocaleString()

// Properties
Number.MAX_VALUE
Number.MIN_VALUE
Number.MAX_SAFE_INTEGER
Number.MIN_SAFE_INTEGER
Number.POSITIVE_INFINITY
Number.NEGATIVE_INFINITY
Number.EPSILON
Number.NaN

// Global number functions
isNaN()
isFinite()
parseInt()
parseFloat()
```

## **Boolean Methods & Values**

```javascript
// Creation
const bool = true;
const boolObj = new Boolean(true);

// Methods
bool.toString()
bool.valueOf()

// Boolean context
Boolean(0) // false
Boolean(1) // true
!!'hello'  // true

// Falsy values: false, 0, "", null, undefined, NaN
// Truthy values: everything else
```

## **Math Object (Mathematical Operations)**

```javascript
// Constants
Math.PI
Math.E
Math.LN2
Math.LN10
Math.LOG2E
Math.LOG10E
Math.SQRT2
Math.SQRT1_2

// Basic operations
Math.abs(-5)
Math.ceil(4.2)
Math.floor(4.8)
Math.round(4.5)
Math.max(1, 2, 3)
Math.min(1, 2, 3)
Math.sqrt(16)
Math.cbrt(27)
Math.hypot(3, 4)

// Powers & Logarithms
Math.pow(2, 3)
Math.exp(1)
Math.log(10)
Math.log2(8)
Math.log10(100)

// Trigonometry
Math.sin(0)
Math.cos(0)
Math.tan(0)
Math.asin(0.5)
Math.acos(0.5)
Math.atan(1)
Math.atan2(1, 1)

// Random
Math.random()
```

## **Console Functions**

```javascript
// Basic logging
console.log()
console.info()
console.warn()
console.error()
console.debug()

// Grouping
console.group()
console.groupCollapsed()
console.groupEnd()

// Counting
console.count()
console.countReset()

// Timing
console.time()
console.timeLog()
console.timeEnd()

// Tables
console.table()

// Tracing
console.trace()

// Assertions
console.assert()

// Clearing
console.clear()

// Styling
console.log('%cStyled text', 'color: red; font-size: 20px;')

// Memory
console.memory
```

## **Window Functions & Properties**

### **Window Methods:**
```javascript
// Dialog boxes
window.alert()
window.confirm()
window.prompt()

// Timing
window.setTimeout()
window.setInterval()
window.clearTimeout()
window.clearInterval()
window.requestAnimationFrame()
window.cancelAnimationFrame()

// Navigation
window.open()
window.close()
window.print()
window.stop()
window.find()

// Scrolling
window.scroll()
window.scrollTo()
window.scrollBy()
window.scrollX
window.scrollY

// Focus
window.focus()
window.blur()

// Storage
window.localStorage
window.sessionStorage

// Location
window.location
window.history
window.navigator

// Screen
window.screen
window.innerWidth
window.innerHeight
window.outerWidth
window.outerHeight

// Events
window.addEventListener()
window.removeEventListener()
window.dispatchEvent()

// Frames
window.frames
window.parent
window.top
window.self
```

## **Utility Functions & Global Objects**

### **Global Functions:**
```javascript
// Evaluation
eval()

// Encoding
encodeURI()
encodeURIComponent()
decodeURI()
decodeURIComponent()

// Type checking
typeof
instanceof

// Object conversion
String()
Number()
Boolean()
Object()
Array()
Symbol()
BigInt()
```

### **JSON Methods:**
```javascript
JSON.parse()
JSON.stringify()
```

### **Object Methods:**
```javascript
Object.keys()
Object.values()
Object.entries()
Object.assign()
Object.create()
Object.defineProperty()
Object.defineProperties()
Object.getOwnPropertyDescriptor()
Object.getPrototypeOf()
Object.setPrototypeOf()
Object.freeze()
Object.seal()
Object.preventExtensions()
Object.isFrozen()
Object.isSealed()
Object.isExtensible()
Object.hasOwn()
```

### **Array Methods:**
```javascript
// Mutation
array.push()
array.pop()
array.shift()
array.unshift()
array.splice()
array.sort()
array.reverse()
array.fill()

// Access
array.concat()
array.slice()
array.indexOf()
array.lastIndexOf()
array.includes()
array.join()

// Iteration
array.forEach()
array.map()
array.filter()
array.reduce()
array.reduceRight()
array.some()
array.every()
array.find()
array.findIndex()
array.findLast()
array.findLastIndex()

// Conversion
array.flat()
array.flatMap()
array.toString()
array.toLocaleString()

// Static methods
Array.from()
Array.isArray()
Array.of()
```

## **Statements & Control Flow**

### **Declaration Statements:**
```javascript
var
let
const
function
class
import
export
```

### **Control Flow Statements:**
```javascript
// Conditional
if
else
else if
switch
case
default
ternary operator (?:)

// Looping
for
for...in
for...of
for await...of
while
do...while

// Jump
break
continue
return
yield
throw
try
catch
finally

// Other
debugger
with (deprecated)
```

### **Error Handling:**
```javascript
try {
  // code
} catch (error) {
  // handle error
} finally {
  // cleanup
}

throw new Error('Message')
```

## **Date & Time APIs**

```javascript
// Date constructor
new Date()
new Date(timestamp)
new Date(year, month, day, hours, minutes, seconds, milliseconds)
new Date(dateString)

// Date methods
date.getFullYear()
date.getMonth()
date.getDate()
date.getDay()
date.getHours()
date.getMinutes()
date.getSeconds()
date.getMilliseconds()
date.getTime()
date.getTimezoneOffset()

// Setter methods
date.setFullYear()
date.setMonth()
// ... and other setters

// Static methods
Date.now()
Date.parse()
Date.UTC()

// Formatting
date.toISOString()
date.toJSON()
date.toLocaleString()
date.toLocaleDateString()
date.toLocaleTimeString()
date.toString()
date.toDateString()
date.toTimeString()
```

## **Modern APIs & Features**

### **Promise APIs:**
```javascript
Promise.resolve()
Promise.reject()
Promise.all()
Promise.allSettled()
Promise.any()
Promise.race()
async/await
```

### **Web APIs:**
```javascript
// Storage
localStorage
sessionStorage
IndexedDB

// Network
fetch()
XMLHttpRequest
WebSocket

// Graphics
Canvas API
WebGL

// Media
Audio API
Video API

// Device
Geolocation API
Device Orientation
Battery API

// File
File API
FileReader
Blob

// Workers
Worker
SharedWorker
```

### **ES6+ Features:**
```javascript
// Destructuring
const {a, b} = obj
const [x, y] = array

// Spread/Rest
...array
...obj

// Modules
import
export

// Enhanced Object Literals
{ method() {} }
{ computedProp: value }

// Symbols & Iterators
Symbol()
[Symbol.iterator]()
generator functions
```

This covers the vast majority of JavaScript's built-in functionality across different categories!