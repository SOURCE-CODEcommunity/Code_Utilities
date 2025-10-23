# Complete Guide to the `key` Property in Event Listeners

## 🔑 **What is the `key` Property?**

The `key` property is part of the `KeyboardEvent` object and represents the **physical key** that was pressed, in a human-readable string format.

```javascript
document.addEventListener('keydown', (event) => {
    console.log(event.key); // Shows which key was pressed
});
```

## 📋 **Complete List of `key` Values**

### **Alphanumeric Keys**
```javascript
// Letters
'a' through 'z' (case-insensitive in keydown/keyup)

// Numbers  
'0' through '9'

// Special characters
' ' (Space), '!', '@', '#', '$', '%', '^', '&', '*', '(', ')',
'-', '_', '=', '+', '[', ']', '{', '}', '\\', '|', ';', ':', "'", '"',
',', '<', '.', '>', '/', '?', '`', '~'
```

### **Modifier Keys**
```javascript
'Alt'           // Alt key
'AltGraph'      // AltGr key (right Alt on some keyboards)
'CapsLock'      // Caps Lock
'Control'       // Ctrl key
'Fn'            // Function key
'FnLock'        // Function Lock
'Hyper'         // Hyper key
'Meta'          // Meta/Command key (⊞ on Windows, ⌘ on Mac)
'NumLock'       // Number Lock
'ScrollLock'    // Scroll Lock
'Shift'         // Shift key
'Super'         // Super key
'Symbol'        // Symbol key
'SymbolLock'    // Symbol Lock
```

### **Navigation Keys**
```javascript
'ArrowDown'     // Down arrow key
'ArrowLeft'     // Left arrow key  
'ArrowRight'    // Right arrow key
'ArrowUp'       // Up arrow key
'End'           // End key
'Home'          // Home key
'PageDown'      // Page Down key
'PageUp'        // Page Up key
```

### **Editing Keys**
```javascript
'Backspace'     // Backspace key
'Clear'         // Clear key
'Copy'          // Copy key
'CrSel'         // Cursor Select
'Cut'           // Cut key
'Delete'        // Delete key
'EraseEof'      // Erase to End of Field
'ExSel'         // Extend Selection
'Insert'        // Insert key
'Paste'         // Paste key
'Redo'          // Redo key
'Undo'          // Undo key
```

### **Function Keys**
```javascript
'F1' through 'F24'  // Function keys F1 through F24
```

### **Numpad Keys**
```javascript
// When NumLock is ON
'0' through '9'     // Numbers
'.' or 'Decimal'    // Decimal point
'+' or 'Add'        // Addition
'-' or 'Subtract'   // Subtraction  
'*' or 'Multiply'   // Multiplication
'/' or 'Divide'     // Division

// When NumLock is OFF or always
'ArrowDown' 'ArrowLeft' 'ArrowRight' 'ArrowUp'
'End' 'Home' 'PageDown' 'PageUp' 'Clear' 'Insert' 'Delete'
```

### **Whitespace Keys**
```javascript
'Enter'         // Enter/Return key
'Tab'           // Tab key
' '             // Spacebar
```

### **UI Keys**
```javascript
'ContextMenu'   // Context Menu key
'Escape'        // Esc key
'Pause'         // Pause/Break key
'Print'         // Print Screen key
```

### **Audio Control Keys**
```javascript
'AudioVolumeDown'   // Volume Down
'AudioVolumeMute'   // Volume Mute  
'AudioVolumeUp'     // Volume Up
'MediaPlayPause'    // Play/Pause
'MediaStop'         // Stop
'MediaTrackNext'    // Next Track
'MediaTrackPrevious' // Previous Track
```

### **Browser Keys**
```javascript
'BrowserBack'       // Browser Back
'BrowserFavorites'  // Browser Favorites
'BrowserForward'    // Browser Forward
'BrowserHome'       // Browser Home
'BrowserRefresh'    // Browser Refresh
'BrowserSearch'     // Browser Search
'BrowserStop'       // Browser Stop
```

## 🆚 **`key` vs `code` vs `keyCode`**

### **`key` Property**
```javascript
// Represents the CHARACTER produced by the key
// Affected by modifier keys (Shift, CapsLock)
event.key === 'A'  // Shift + a
event.key === 'a'  // a without Shift
event.key === '!'  // Shift + 1
```

### **`code` Property** 
```javascript
// Represents the PHYSICAL KEY on keyboard
// NOT affected by modifier keys
event.code === 'KeyA'      // A key (always)
event.code === 'Digit1'    // 1 key (always)
event.code === 'ShiftLeft' // Left Shift key
```

### **`keyCode` Property** (DEPRECATED)
```javascript
// Legacy numeric code (AVOID USING)
event.keyCode === 65  // 'A' key (deprecated)
```

## 💻 **Practical Examples**

### **Game Controls**
```javascript
document.addEventListener('keydown', (e) => {
    switch(e.key) {
        case 'ArrowUp':
            player.moveUp();
            break;
        case 'ArrowDown':
            player.moveDown();
            break;
        case 'ArrowLeft':
            player.moveLeft();
            break;
        case 'ArrowRight':
            player.moveRight();
            break;
        case ' ':
            player.jump();
            break;
    }
});
```

### **Form Navigation**
```javascript
document.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' && !e.shiftKey) {
        e.preventDefault();
        submitForm();
    }
    
    if (e.key === 'Escape') {
        closeModal();
    }
    
    if (e.key === 'Tab') {
        // Handle tab navigation
        handleTabNavigation(e);
    }
});
```

### **Keyboard Shortcuts**
```javascript
document.addEventListener('keydown', (e) => {
    // Ctrl/Cmd + S
    if ((e.ctrlKey || e.metaKey) && e.key === 's') {
        e.preventDefault();
        saveDocument();
    }
    
    // Ctrl/Cmd + C
    if ((e.ctrlKey || e.metaKey) && e.key === 'c') {
        // Copy logic
    }
    
    // Escape key
    if (e.key === 'Escape') {
        closeAllModals();
    }
});
```

### **Input Validation**
```javascript
const numberInput = document.getElementById('number-input');

numberInput.addEventListener('keydown', (e) => {
    // Allow: backspace, delete, tab, escape, enter
    if ([ 'Backspace', 'Delete', 'Tab', 'Escape', 'Enter' ].includes(e.key)) {
        return;
    }
    
    // Allow: Ctrl/Cmd + A, C, V, X
    if ((e.ctrlKey || e.metaKey) && ['a', 'c', 'v', 'x'].includes(e.key)) {
        return;
    }
    
    // Prevent if not a number
    if (!/^\d$/.test(e.key)) {
        e.preventDefault();
    }
});
```

## 🌍 **International Keyboard Support**

### **Handling Different Keyboard Layouts**
```javascript
document.addEventListener('keydown', (e) => {
    // Use 'key' for character input (layout-aware)
    console.log(`You pressed: ${e.key}`);
    
    // Use 'code' for physical key position (layout-independent)
    console.log(`Physical key: ${e.code}`);
});

// Example: QWERTY vs AZERTY
// On QWERTY: 'KeyA' produces 'a'
// On AZERTY: 'KeyA' produces 'q' (but same physical position)
```

### **Special Characters**
```javascript
document.addEventListener('keydown', (e) => {
    // Handle special characters that might vary by layout
    if (e.key === 'Dead') {
        // Key that modifies next key (like ^ for accents)
        handleDeadKey(e);
    }
});
```

## ⚡ **Advanced Usage**

### **Key Combination Detection**
```javascript
class KeyboardManager {
    constructor() {
        this.keys = new Set();
        
        document.addEventListener('keydown', (e) => {
            this.keys.add(e.key);
            this.checkCombinations();
        });
        
        document.addEventListener('keyup', (e) => {
            this.keys.delete(e.key);
        });
    }
    
    checkCombinations() {
        // Check for multiple key combinations
        if (this.keys.has('Control') && this.keys.has('Shift') && this.keys.has('KeyP')) {
            this.printDocument();
        }
    }
}
```

### **Game Input System**
```javascript
class InputSystem {
    constructor() {
        this.keyState = {};
        
        document.addEventListener('keydown', (e) => {
            this.keyState[e.key] = true;
        });
        
        document.addEventListener('keyup', (e) => {
            this.keyState[e.key] = false;
        });
    }
    
    isKeyPressed(key) {
        return !!this.keyState[key];
    }
    
    getMovementVector() {
        return {
            x: (this.isKeyPressed('ArrowRight') - this.isKeyPressed('ArrowLeft')),
            y: (this.isKeyPressed('ArrowDown') - this.isKeyPressed('ArrowUp'))
        };
    }
}
```

## 🚫 **Common Pitfalls & Best Practices**

### **1. Case Sensitivity**
```javascript
// WRONG - 'Enter' vs 'enter'
if (e.key === 'enter') { }  // Won't work

// CORRECT - Always use exact case
if (e.key === 'Enter') { }

// SAFE - Normalize comparison
if (e.key.toLowerCase() === 'enter') { }
```

### **2. Modifier Key Detection**
```javascript
// Check for Shift + A
if (e.key === 'A' && e.shiftKey) {
    // Uppercase A with Shift
}

// Check for Ctrl + C
if (e.key === 'c' && (e.ctrlKey || e.metaKey)) {
    // Copy command
}
```

### **3. Prevent Default Carefully**
```javascript
document.addEventListener('keydown', (e) => {
    // Only prevent default when necessary
    if (e.key === ' ' && e.target === document.body) {
        e.preventDefault(); // Prevent page scroll
    }
});
```

### **4. Accessibility Considerations**
```javascript
// Provide keyboard alternatives for mouse actions
document.addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') {
        // Activate button or link
        if (e.target.matches('button, [role="button"]')) {
            e.target.click();
        }
    }
});
```

## 🔧 **Browser Compatibility**

### **Modern Browser Support**
- ✅ Chrome 51+
- ✅ Firefox 23+
- ✅ Safari 10.1+
- ✅ Edge 79+
- ✅ Opera 38+

### **Fallback for Older Browsers**
```javascript
function getKey(event) {
    if (event.key) {
        return event.key;
    }
    
    // Fallback for very old browsers (not recommended)
    switch(event.keyCode) {
        case 13: return 'Enter';
        case 27: return 'Escape';
        case 32: return ' ';
        case 37: return 'ArrowLeft';
        case 38: return 'ArrowUp';
        case 39: return 'ArrowRight';
        case 40: return 'ArrowDown';
        case 65: return 'a';
        case 66: return 'b';
        // ... more keyCodes
        default: return 'Unidentified';
    }
}
```

## 📚 **Complete Reference Table**

| Key Type | `key` Value | `code` Value | Common Use |
|----------|-------------|--------------|------------|
| Letters | 'a'-'z' | 'KeyA'-'KeyZ' | Text input |
| Numbers | '0'-'9' | 'Digit0'-'Digit9' | Numbers |
| Arrows | 'ArrowUp' etc. | 'ArrowUp' etc. | Navigation |
| Modifiers | 'Shift' etc. | 'ShiftLeft' etc. | Combinations |
| Function | 'F1'-'F24' | 'F1'-'F24' | Shortcuts |
| Special | 'Enter', 'Escape' | 'Enter', 'Escape' | UI controls |

The `key` property is your go-to for most keyboard interaction needs - it's intuitive, human-readable, and handles international keyboards gracefully!