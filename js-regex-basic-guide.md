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