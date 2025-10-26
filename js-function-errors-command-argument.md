The error is happening because you're trying to use **default parameter syntax** incorrectly in a function call. You're mixing **function call syntax** with **function definition syntax**.

## **The Problem:**

```javascript
// ❌ Wrong - this is function CALL syntax, not definition
showMessage(quotes[getRandomInteger(quotes.length)], command = () => {
    document.getElementById('messageDialog').style.backgroundColor = 'rgba(255, 255, 255, 0.28)'
})
```

## **Solutions:**

### **Solution 1: Pass the function directly**
```javascript
showMessage(quotes[getRandomInteger(quotes.length)], () => {
    document.getElementById('messageDialog').style.backgroundColor = 'rgba(255, 255, 255, 0.28)';
});
```

### **Solution 2: Define the function separately**
```javascript
const command = () => {
    document.getElementById('messageDialog').style.backgroundColor = 'rgba(255, 255, 255, 0.28)';
};

showMessage(quotes[getRandomInteger(quotes.length)], command);
```

### **Solution 3: Use undefined check in showMessage**
```javascript
// In your showMessage function
function showMessage(message, command) {
    try {
        if (command) {
            command();
        }
        // Rest of your function...
    } catch (error) {
        console.log(error);
    }
}

// Then call without command (it will be undefined but handled)
showMessage(quotes[getRandomInteger(quotes.length)]);
```

## **Complete Fixed Code:**

### **Option A: With command**
```javascript
showMessage(
    quotes[getRandomInteger(quotes.length)], 
    () => {
        document.getElementById('messageDialog').style.backgroundColor = 'rgba(255, 255, 255, 0.28)';
    }
);
```

### **Option B: Without command**
```javascript
showMessage(quotes[getRandomInteger(quotes.length)]);
```

## **Understanding the Issue:**

### **Default parameters are for FUNCTION DEFINITIONS:**
```javascript
// ✅ Correct - function definition
function showMessage(message, command = () => {}) {
    // command has a default value
}
```

### **Not for FUNCTION CALLS:**
```javascript
// ❌ Wrong - function call
showMessage("hello", command = () => {});

// ✅ Correct - function call
showMessage("hello", () => {});
// OR
showMessage("hello", someFunction);
```

## **Also fix the rgba syntax:**
```javascript
// ❌ Missing quotes
rgba(255, 255, 255, 0.28)

// ✅ Correct - string value
'rgba(255, 255, 255, 0.28)'
```

## **Final corrected version:**
```javascript
showMessage(quotes[getRandomInteger(quotes.length)], () => {
    document.getElementById('messageDialog').style.backgroundColor = 'rgba(255, 255, 255, 0.28)';
});
```

The key is that **default parameters** (`param = defaultValue`) only work in function **definitions**, not function **calls**!