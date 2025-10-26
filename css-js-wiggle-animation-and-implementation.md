Here's a complete wiggle animation for a centered dialog:

## **CSS Solution (Recommended)**

### **1. CSS Keyframes Animation:**
```css
/* Center the dialog */
.dialog {
    position: fixed;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.15);
    z-index: 1000;
}

/* Wiggle Animation */
@keyframes wiggle {
    0%, 7% {
        transform: translate(-50%, -50%) rotateZ(0);
    }
    15% {
        transform: translate(-50%, -50%) rotateZ(-15deg);
    }
    20% {
        transform: translate(-50%, -50%) rotateZ(10deg);
    }
    25% {
        transform: translate(-50%, -50%) rotateZ(-10deg);
    }
    30% {
        transform: translate(-50%, -50%) rotateZ(6deg);
    }
    35% {
        transform: translate(-50%, -50%) rotateZ(-4deg);
    }
    40%, 100% {
        transform: translate(-50%, -50%) rotateZ(0);
    }
}

.wiggle {
    animation: wiggle 2s linear;
}
```

### **2. JavaScript to Trigger Animation:**
```javascript
function wiggleDialog(dialogId = 'dialog') {
    const dialog = document.getElementById(dialogId);
    
    // Remove existing animation
    dialog.classList.remove('wiggle');
    
    // Trigger reflow
    void dialog.offsetWidth;
    
    // Add animation class
    dialog.classList.add('wiggle');
    
    // Remove class after animation completes
    setTimeout(() => {
        dialog.classList.remove('wiggle');
    }, 2000);
}

// Usage
wiggleDialog(); // Wiggle the default 'dialog' element
wiggleDialog('myDialog'); // Wiggle specific dialog
```

## **JavaScript-Only Solution**

### **3. Pure JavaScript Animation:**
```javascript
function wiggleDialogJS(dialogId = 'dialog') {
    const dialog = document.getElementById(dialogId);
    const startTime = performance.now();
    const duration = 2000; // 2 seconds
    
    function animate(currentTime) {
        const elapsed = currentTime - startTime;
        const progress = Math.min(elapsed / duration, 1);
        
        // Wiggle easing function
        const wiggle = (time) => {
            return Math.sin(time * 20) * Math.pow(2, -time * 4);
        };
        
        const rotation = wiggle(progress) * 15; // Max 15 degrees
        
        // Apply transform while maintaining center position
        dialog.style.transform = `translate(-50%, -50%) rotate(${rotation}deg)`;
        
        if (progress < 1) {
            requestAnimationFrame(animate);
        } else {
            // Reset to original position
            dialog.style.transform = 'translate(-50%, -50%) rotate(0deg)';
        }
    }
    
    requestAnimationFrame(animate);
}
```

## **Advanced Wiggle Variations**

### **4. Different Wiggle Styles:**
```css
/* Gentle wiggle */
@keyframes gentleWiggle {
    0%, 100% { transform: translate(-50%, -50%) rotate(0deg); }
    25% { transform: translate(-50%, -50%) rotate(1deg); }
    75% { transform: translate(-50%, -50%) rotate(-1deg); }
}

.gentle-wiggle {
    animation: gentleWiggle 0.5s ease-in-out 3;
}

/* Bounce wiggle */
@keyframes bounceWiggle {
    0%, 20%, 50%, 80%, 100% {
        transform: translate(-50%, -50%) scale(1);
    }
    40% {
        transform: translate(-50%, -50%) scale(1.1) rotate(5deg);
    }
    60% {
        transform: translate(-50%, -50%) scale(1.05) rotate(-5deg);
    }
}

.bounce-wiggle {
    animation: bounceWiggle 0.6s ease-in-out;
}

/* Horizontal shake */
@keyframes shake {
    0%, 100% { transform: translate(-50%, -50%); }
    10%, 30%, 50%, 70%, 90% { transform: translate(calc(-50% - 5px), -50%); }
    20%, 40%, 60%, 80% { transform: translate(calc(-50% + 5px), -50%); }
}

.shake {
    animation: shake 0.5s ease-in-out;
}
```

### **5. Configurable Wiggle Function:**
```javascript
function wiggleDialogAdvanced(options = {}) {
    const {
        dialogId = 'dialog',
        intensity = 15, // degrees
        duration = 2000,
        shakes = 4,
        style = 'rotate' // 'rotate', 'shake', 'bounce'
    } = options;
    
    const dialog = document.getElementById(dialogId);
    const animationClass = `${style}-wiggle`;
    
    // Remove any existing animation
    dialog.classList.remove('wiggle', 'gentle-wiggle', 'bounce-wiggle', 'shake');
    
    // Create dynamic styles based on parameters
    const styleId = 'dynamic-wiggle-style';
    let styleElement = document.getElementById(styleId);
    
    if (!styleElement) {
        styleElement = document.createElement('style');
        styleElement.id = styleId;
        document.head.appendChild(styleElement);
    }
    
    // Generate keyframes based on parameters
    const keyframes = generateKeyframes(style, intensity, shakes);
    styleElement.textContent = `
        @keyframes dynamicWiggle { ${keyframes} }
        .dynamic-wiggle { animation: dynamicWiggle ${duration}ms ease-in-out; }
    `;
    
    // Trigger animation
    dialog.classList.add('dynamic-wiggle');
    
    setTimeout(() => {
        dialog.classList.remove('dynamic-wiggle');
    }, duration);
}

function generateKeyframes(style, intensity, shakes) {
    switch(style) {
        case 'shake':
            return generateShakeKeyframes(intensity, shakes);
        case 'bounce':
            return generateBounceKeyframes(intensity);
        default:
            return generateRotateKeyframes(intensity, shakes);
    }
}

function generateRotateKeyframes(intensity, shakes) {
    let keyframes = '0% { transform: translate(-50%, -50%) rotate(0deg); }';
    
    for (let i = 1; i <= shakes; i++) {
        const percentage = (i / (shakes * 2)) * 100;
        const nextPercentage = ((i + 0.5) / (shakes * 2)) * 100;
        
        keyframes += `${percentage}% { transform: translate(-50%, -50%) rotate(-${intensity}deg); }`;
        keyframes += `${nextPercentage}% { transform: translate(-50%, -50%) rotate(${intensity}deg); }`;
    }
    
    keyframes += '100% { transform: translate(-50%, -50%) rotate(0deg); }';
    return keyframes;
}
```

## **Complete HTML Example**

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        .dialog {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            text-align: center;
            min-width: 300px;
            font-family: Arial, sans-serif;
        }

        @keyframes wiggle {
            0%, 7% {
                transform: translate(-50%, -50%) rotateZ(0);
            }
            15% {
                transform: translate(-50%, -50%) rotateZ(-15deg);
            }
            20% {
                transform: translate(-50%, -50%) rotateZ(10deg);
            }
            25% {
                transform: translate(-50%, -50%) rotateZ(-10deg);
            }
            30% {
                transform: translate(-50%, -50%) rotateZ(6deg);
            }
            35% {
                transform: translate(-50%, -50%) rotateZ(-4deg);
            }
            40%, 100% {
                transform: translate(-50%, -50%) rotateZ(0);
            }
        }

        .wiggle {
            animation: wiggle 2s linear;
        }

        button {
            background: white;
            color: #667eea;
            border: none;
            padding: 10px 20px;
            border-radius: 5px;
            cursor: pointer;
            margin: 5px;
            font-weight: bold;
        }

        button:hover {
            background: #f0f0f0;
        }
    </style>
</head>
<body>
    <div id="dialog" class="dialog">
        <h2>Hello! I'm a Dialog</h2>
        <p>Click below to make me wiggle!</p>
        <button onclick="wiggleDialog()">Wiggle Me!</button>
        <button onclick="wiggleDialogAdvanced({style: 'shake'})">Shake Me!</button>
        <button onclick="wiggleDialogAdvanced({style: 'bounce'})">Bounce Me!</button>
    </div>

    <script>
        function wiggleDialog(dialogId = 'dialog') {
            const dialog = document.getElementById(dialogId);
            dialog.classList.remove('wiggle');
            void dialog.offsetWidth; // Trigger reflow
            dialog.classList.add('wiggle');
            
            setTimeout(() => dialog.classList.remove('wiggle'), 2000);
        }

        // Advanced version with multiple styles
        function wiggleDialogAdvanced(options = {}) {
            const { style = 'rotate' } = options;
            const styles = {
                rotate: 'wiggle',
                shake: 'shake',
                bounce: 'bounce-wiggle'
            };
            
            wiggleDialogWithStyle(styles[style] || 'wiggle');
        }

        function wiggleDialogWithStyle(animationClass) {
            const dialog = document.getElementById('dialog');
            dialog.classList.remove('wiggle', 'shake', 'bounce-wiggle');
            void dialog.offsetWidth;
            dialog.classList.add(animationClass);
            
            setTimeout(() => dialog.classList.remove(animationClass), 2000);
        }

        // Auto-wiggle on page load
        window.addEventListener('load', () => {
            setTimeout(() => wiggleDialog(), 1000);
        });
    </script>
</body>
</html>
```

## **Key Features:**

1. **Maintains center position** while wiggling
2. **Multiple animation styles**: rotate, shake, bounce
3. **Configurable parameters**: intensity, duration, shake count
4. **Smooth transitions** with proper CSS transforms
5. **Reusable functions** for any dialog
6. **Auto-cleanup** of animation classes

The dialog will stay perfectly centered while performing the wiggle animation!