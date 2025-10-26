# Complete Guide to Regular Expressions: `/^[1-4]$/.test(e.key)`

## 🔍 **Breaking Down the Pattern:**

```javascript
/^[1-4]$/.test(e.key)
```

Let's dissect this piece by piece:

## 📐 **Regex Components:**

### **1. Delimiters: `/ /`**
```javascript
/pattern/    // Forward slashes define regex boundaries
```

### **2. Start Anchor: `^`**
```javascript
^           // Matches the START of the string
"hello" -> ^ matches position before "h"
```

### **3. Character Class: `[1-4]`**
```javascript
[1-4]       // Matches any SINGLE character from 1 to 4
// Equivalent to: 1 OR 2 OR 3 OR 4
```

### **4. End Anchor: `$`**
```javascript
$           // Matches the END of the string  
"hello" -> $ matches position after "o"
```

### **5. Combined Meaning:**
```javascript
/^[1-4]$/   // Matches strings that:
            // 1. Start immediately with [1-4]
            // 2. Contain exactly one character
            // 3. End immediately after that character
```

## ✅ **What It Matches:**

```javascript
/^[1-4]$/.test('1')    // true
/^[1-4]$/.test('2')    // true  
/^[1-4]$/.test('3')    // true
/^[1-4]$/.test('4')    // true
```

## ❌ **What It Rejects:**

```javascript
/^[1-4]$/.test('0')    // false - outside range
/^[1-4]$/.test('5')    // false - outside range
/^[1-4]$/.test('12')   // false - too long
/^[1-4]$/.test('')     // false - empty string
/^[1-4]$/.test('a')    // false - not a number
/^[1-4]$/.test(' 1')   // false - has space
/^[1-4]$/.test('1 ')   // false - has space
```

---

## 📚 **Complete Regex Syntax Guide**

### **Anchors:**
```javascript
/^hello/    // Starts with "hello"
/world$/    // Ends with "world"  
/^hello$/   // Exactly "hello" (start to end)
```

### **Character Classes:**
```javascript
[abc]       // a, b, or c
[^abc]      // NOT a, b, or c
[a-z]       // Any lowercase letter
[A-Z]       // Any uppercase letter  
[0-9]       // Any digit
[a-zA-Z]    // Any letter
[0-9a-fA-F] // Hex digits
```

### **Quantifiers:**
```javascript
a?          // Zero or one 'a'
a*          // Zero or more 'a's  
a+          // One or more 'a's
a{3}        // Exactly 3 'a's
a{3,}       // 3 or more 'a's
a{3,5}      // 3 to 5 'a's
```

### **Special Characters:**
```javascript
.           // Any single character (except newline)
\d          // Any digit [0-9]
\D          // Any non-digit [^0-9]
\w          // Word character [a-zA-Z0-9_]
\W          // Non-word character [^a-zA-Z0-9_]
\s          // Whitespace [ \t\r\n\f]
\S          // Non-whitespace [^ \t\r\n\f]
```

### **Groups and Alternation:**
```javascript
(abc)       // Capture group
(?:abc)     // Non-capturing group
a|b         // a OR b
```

---

## 🎯 **Practical Examples for Your Quiz App**

### **Number Validation:**
```javascript
// Single digit 1-4 (your original)
/^[1-4]$/.test(e.key)

// Multiple choice (1-10)
/^[1-9]$|^10$/.test(e.key)

// Any single digit
/^\d$/.test(e.key)

// Numbers with optional decimal
/^\d*\.?\d+$/.test(input)
```

### **Navigation Keys:**
```javascript
// Arrow keys + WASD
/^(ArrowUp|ArrowDown|ArrowLeft|ArrowRight|w|a|s|d)$/i.test(e.key)

// Common game controls
/^[ wasdqrfe]$/i.test(e.key)  // Common game keys
```

### **Form Validation:**
```javascript
// Username: 3-20 alphanumeric chars
/^[a-zA-Z0-9]{3,20}$/.test(username)

// Email validation (basic)
/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email)

// Password: 8+ chars, 1 uppercase, 1 lowercase, 1 number
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d).{8,}$/.test(password)
```

### **Input Filtering:**
```javascript
// Only allow numbers in input
input.addEventListener('keydown', (e) => {
    if (!/^\d$/.test(e.key) && 
        !['Backspace', 'Delete', 'ArrowLeft', 'ArrowRight', 'Tab'].includes(e.key)) {
        e.preventDefault();
    }
});

// Allow numbers and decimal point
input.addEventListener('keydown', (e) => {
    if (!/^[\d.]$/.test(e.key) && 
        !['Backspace', 'Delete', 'ArrowLeft', 'ArrowRight', 'Tab'].includes(e.key)) {
        e.preventDefault();
    }
    
    // Prevent multiple decimal points
    if (e.key === '.' && input.value.includes('.')) {
        e.preventDefault();
    }
});
```

---

## 🔧 **Regex Methods in JavaScript**

### **1. `test()` Method:**
```javascript
const regex = /^[1-4]$/;
regex.test('3');  // returns boolean
```

### **2. `exec()` Method:**
```javascript
const regex = /(\d+)/;
const result = regex.exec('Answer 42');
console.log(result[0]); // "42"
console.log(result[1]); // "42" (captured group)
```

### **3. `match()` Method:**
```javascript
const str = 'Hello 123 World';
const matches = str.match(/\d+/);
console.log(matches[0]); // "123"
```

### **4. `replace()` Method:**
```javascript
const str = 'Price: $100';
const newStr = str.replace(/\$(\d+)/, '€$1');
console.log(newStr); // "Price: €100"
```

### **5. `search()` Method:**
```javascript
const str = 'Find the number 42';
const position = str.search(/\d+/);
console.log(position); // 14
```

---

## 🆚 **Regex vs Alternative Approaches**

### **For Your Specific Case:**
```javascript
// Method 1: Regex (clean and explicit)
/^[1-4]$/.test(e.key)

// Method 2: Array includes
['1', '2', '3', '4'].includes(e.key)

// Method 3: String comparison  
e.key >= '1' && e.key <= '4'

// Method 4: Set (fast for large sets)
new Set(['1', '2', '3', '4']).has(e.key)

// Method 5: Switch statement
switch(e.key) {
    case '1': case '2': case '3': case '4': 
        return true;
    default:
        return false;
}
```

### **Performance Comparison:**
```javascript
// For small sets (like 1-4), all methods are fast
// Regex is very readable and explicit about the pattern
```

---

## 🎓 **Advanced Regex Features**

### **Lookaheads:**
```javascript
// Positive lookahead: must contain digit
/(?=.*\d)/.test('abc1')  // true

// Negative lookahead: must NOT contain digit  
/(?!.*\d)/.test('abc')   // true

// Password validation with multiple requirements
/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$/
```

### **Flags:**
```javascript
/pattern/g    // Global - find all matches
/pattern/i    // Case insensitive  
/pattern/m    // Multiline - ^ and $ match line boundaries
/pattern/u    // Unicode support
/pattern/y    // Sticky - matches only from lastIndex
```

### **Real-World Quiz App Examples:**

```javascript
// Validate subject name (letters, spaces, hyphens)
function isValidSubjectName(name) {
    return /^[a-zA-Z\s\-]{2,50}$/.test(name);
}

// Validate test duration (1-180 minutes)
function isValidDuration(duration) {
    return /^[1-9]\d{0,2}$/.test(duration) && parseInt(duration) <= 180;
}

// Sanitize user input for questions
function sanitizeQuestionText(text) {
    return text.replace(/[<>]/g, ''); // Remove < and >
}

// Extract LaTeX expressions from text
function extractLatex(text) {
    return text.match(/\$[^$]+\$|\\\([^)]+\\\)/g) || [];
}

// Validate email for user profile
function isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
}

// Check if string contains LaTeX
function containsLatex(text) {
    return /\$[^$]+\$|\\\([^)]+\\\)|\\\[[^\]]+\\\]/.test(text);
}
```

---

## 🚀 **Complete Keyboard Handler with Regex:**

```javascript
document.addEventListener('keydown', (e) => {
    // Ignore if user is typing in an input
    if (e.target.matches('input, textarea, [contenteditable]')) {
        return;
    }
    
    // Number keys 1-4 for answer selection
    if (/^[1-4]$/.test(e.key)) {
        const answerIndex = parseInt(e.key) - 1;
        selectAnswer(answerIndex);
        e.preventDefault();
        return;
    }
    
    // Navigation between questions
    if (/^(ArrowLeft|ArrowUp|a|w)$/i.test(e.key)) {
        goToPreviousQuestion();
        e.preventDefault();
        return;
    }
    
    if (/^(ArrowRight|ArrowDown|d|s)$/i.test(e.key)) {
        goToNextQuestion();
        e.preventDefault();
        return;
    }
    
    // Special actions
    switch(e.key) {
        case 'Enter':
            if (elements.submitTestBtn) elements.submitTestBtn.click();
            break;
        case 'Escape':
            closeAllModals();
            break;
        case ' ':
            if (elements.startTestBtn) elements.startTestBtn.click();
            break;
    }
});
```

---

## 📖 **Key Takeaways:**

1. **`/^[1-4]$/`** means "exactly one character between 1 and 4"
2. **`^`** anchors to start, **`$`** anchors to end
3. **`test()`** returns boolean - perfect for validations
4. **Regex is very readable** for pattern matching
5. **Great for keyboard shortcuts** and input validation

Your regex `/^[1-4]$/.test(e.key)` is **perfect** for detecting number keys 1-4! It's clean, readable, and does exactly what you need.


The regex `/^[1]$/.test('101')` is **incorrect** for checking if a string starts with `1`. Let me show you the right ways:

## **Correct Regex Patterns:**

### **1. Check if string STARTS with 1:**
```javascript
/^1/.test('101')     // true - starts with "1"
/^1/.test('101')     // true
/^1/.test('123')     // true
/^1/.test('010')     // false
/^1/.test('201')     // false
```

### **2. Check if string STARTS with specific digit:**
```javascript
/^[1]/.test('101')   // true - starts with "1"
/^[1-5]/.test('301') // true - starts with 3 (1-5 range)
/^[a-z]/.test('abc') // true - starts with lowercase letter
```

## **What Your Regex Does:**
```javascript
/^[1]$/.test('101')  // FALSE - why?
```
- `^` = start of string
- `[1]` = exactly the character "1"  
- `$` = end of string
- So it checks if the **entire string is exactly "1"**

## **Complete Guide to String Start Checking:**

### **1. Basic Start Check:**
```javascript
// Using regex
/^1/.test('101')           // true
/^hello/.test('hello world') // true

// Using string methods
'101'.startsWith('1')      // true (modern approach)
'101'.charAt(0) === '1'    // true
'101'[0] === '1'           // true
```

### **2. Multiple Possible Starting Characters:**
```javascript
// Starts with 1, 2, or 3
/^[123]/.test('201')       // true (starts with 2)
/^[1-3]/.test('301')       // true (starts with 3)

// Using OR
/^(1|2|3)/.test('101')     // true

// Using string method
['1','2','3'].includes('101'[0]) // true
```

### **3. Case-Insensitive Start:**
```javascript
/^hello/i.test('Hello World')  // true
/^[a-z]/i.test('Apple')        // true
```

### **4. Complex Start Patterns:**
```javascript
// Starts with digit
/^\d/.test('123abc')        // true

// Starts with letter
/^[a-zA-Z]/.test('Hello')   // true

// Starts with specific word
/^Mr\./.test('Mr. Smith')   // true

// Starts with http or https
/^https?:\/\//.test('https://example.com') // true
```

## **Practical Examples:**

### **Form Validation:**
```javascript
function validatePhone(phone) {
  return /^1/.test(phone); // Must start with 1
}

console.log(validatePhone('123-456-7890')); // true
console.log(validatePhone('555-1234'));     // false
```

### **URL Routing:**
```javascript
function routePath(path) {
  if (/^\/admin/.test(path)) {
    return 'admin panel';
  } else if (/^\/user/.test(path)) {
    return 'user dashboard';
  } else if (/^\//.test(path)) {
    return 'home page';
  }
  return 'invalid route';
}
```

### **File Type Checking:**
```javascript
function getFileType(filename) {
  if (/^\./.test(filename)) {
    return 'hidden file';
  } else if (/^\d/.test(filename)) {
    return 'numbered file';
  }
  return 'regular file';
}
```

## **Performance Comparison:**

```javascript
// Benchmark different approaches
const testString = '101';

// Regex
console.time('regex');
/^1/.test(testString);
console.timeEnd('regex');

// startsWith (usually faster)
console.time('startsWith');
testString.startsWith('1');
console.timeEnd('startsWith');

// charAt
console.time('charAt');
testString.charAt(0) === '1';
console.timeEnd('charAt');

// Bracket notation (fastest)
console.time('bracket');
testString[0] === '1');
console.timeEnd('bracket');
```

## **Best Practices:**

### **Use `startsWith()` for simple checks:**
```javascript
// ✅ Recommended
'101'.startsWith('1')

// ❌ Overkill for simple checks
/^1/.test('101')
```

### **Use regex for complex patterns:**
```javascript
// ✅ Use regex for complex patterns
/^https?:\/\//.test(url)
/^[A-Z][a-z]+/.test(name)
/^\d{3}-/.test(phone)

// ❌ Don't use string methods for complex patterns
// (would require multiple checks)
```

## **Common Regex Start Patterns:**
```javascript
/^1/        // Starts with 1
/^[1-9]/    // Starts with digit 1-9
/^[a-z]/i   // Starts with letter (case-insensitive)
/^\./       // Starts with dot (hidden files)
/^\//       // Starts with slash (URL paths)
/^https?/   // Starts with http or https
/^#/        // Starts with hash (CSS selectors)
```

**So for checking if a string starts with "1", use either:**
- `'101'.startsWith('1')` (modern, readable)
- `/^1/.test('101')` (regex approach)

**No, you can't directly use a variable `b` inside the regex literal `/^b/`** because regex literals are compiled at creation time and `b` would be treated as the literal character "b", not the variable's value.

## **The Problem:**
```javascript
let b = "1";
/^b/.test("101")  // ❌ This checks for string starting with "b", not "1"
// false - because "101" doesn't start with "b"
```

## **Solutions to Use Variables in Regex:**

### **1. `RegExp` Constructor (Recommended)**
```javascript
let b = "1";
let regex = new RegExp(`^${b}`);
regex.test("101");  // ✅ true - checks if starts with "1"

// With special characters escaped
let b = "1.0";
let regex = new RegExp(`^${b}`); // ❌ Problem if b has regex chars
```

### **2. String Concatenation (Older Approach)**
```javascript
let b = "1";
let regex = new RegExp("^" + b);
regex.test("101");  // ✅ true
```

### **3. For Dynamic Patterns:**
```javascript
function startsWith(char, str) {
  let regex = new RegExp(`^${char}`);
  return regex.test(str);
}

let b = "1";
startsWith(b, "101");  // ✅ true
```

## **Complete Examples:**

### **Dynamic Starting Character:**
```javascript
let startingChar = "A";
let testString = "Apple";

let regex = new RegExp(`^${startingChar}`);
console.log(regex.test(testString)); // true

// Multiple test cases
let tests = [
  { char: "1", str: "101", expected: true },
  { char: "A", str: "Apple", expected: true },
  { char: "x", str: "xyz", expected: true },
  { char: "1", str: "201", expected: false }
];

tests.forEach(({char, str, expected}) => {
  let regex = new RegExp(`^${char}`);
  let result = regex.test(str);
  console.log(`Starts with "${char}": ${result} (expected: ${expected})`);
});
```

### **Escaping Special Characters:**
```javascript
function createStartsWithRegex(char) {
  // Escape regex special characters
  const escapedChar = char.replace(/[.*+?^${}()|[\]\\]/g, '\\$&');
  return new RegExp(`^${escapedChar}`);
}

// Test with special characters
let b = "1.";
let regex = createStartsWithRegex(b);
console.log(regex.test("1.01")); // true - correctly matches "1."
console.log(regex.test("101"));  // false - doesn't match "1"
```

### **Multiple Dynamic Characters:**
```javascript
let allowedStarts = ["1", "2", "3"];
let testString = "201";

// Check if starts with any of the allowed characters
let regex = new RegExp(`^[${allowedStarts.join('')}]`);
console.log(regex.test(testString)); // true (starts with 2)

// Alternative approach
let result = allowedStarts.some(char => 
  new RegExp(`^${char}`).test(testString)
);
console.log(result); // true
```

## **Performance Considerations:**

```javascript
// ❌ Don't create regex inside loops (if possible)
function badExample(strings, startingChar) {
  return strings.filter(str => {
    return new RegExp(`^${startingChar}`).test(str); // Creates new regex each time
  });
}

// ✅ Better: create regex once
function goodExample(strings, startingChar) {
  const regex = new RegExp(`^${startingChar}`);
  return strings.filter(str => regex.test(str));
}
```

## **Real-World Use Cases:**

### **Form Validation with Dynamic Rules:**
```javascript
class InputValidator {
  constructor(rules) {
    this.rules = rules;
  }

  validate(input, field) {
    const rule = this.rules[field];
    if (rule.startsWith) {
      const regex = new RegExp(`^${rule.startsWith}`);
      return regex.test(input);
    }
    return true;
  }
}

const validator = new InputValidator({
  phone: { startsWith: "1" },
  username: { startsWith: "@" }
});

console.log(validator.validate("123-4567", "phone")); // true
console.log(validator.validate("@john", "username")); // true
```

### **Dynamic Filtering:**
```javascript
function filterItems(items, startingPattern) {
  const regex = new RegExp(`^${startingPattern}`);
  return items.filter(item => regex.test(item.name));
}

const products = [
  { name: "1kg sugar", price: 10 },
  { name: "2kg flour", price: 15 },
  { name: "1L milk", price: 5 }
];

const ones = filterItems(products, "1");
console.log(ones); // [{ name: "1kg sugar", ... }, { name: "1L milk", ... }]
```

## **Key Points:**
- **Use `new RegExp()`** when you need variables in regex
- **Escape special characters** if the variable might contain `. * + ?` etc.
- **Regex literals** (`/pattern/`) are static and can't use variables directly
- **Cache regex objects** when used repeatedly for better performance

So for your case:
```javascript
let b = "1";
let regex = new RegExp(`^${b}`);
regex.test("101"); // ✅ true
```