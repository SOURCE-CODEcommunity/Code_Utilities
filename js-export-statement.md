There are several ways to import variables from another file in JavaScript, depending on your environment and module system:

## 📦 **1. ES6 Modules (Modern Standard)**

### **Exporting from `config.js`:**
```javascript
// config.js

// Named exports
export const API_URL = 'https://api.example.com';
export const MAX_QUESTIONS = 50;
export const THEME_COLORS = {
    primary: '#4CAF50',
    secondary: '#2196F3'
};

// Exporting functions
export function formatTime(seconds) {
    return `${Math.floor(seconds / 60)}:${seconds % 60}`;
}

// Default export (one per file)
const APP_CONFIG = {
    version: '1.0.0',
    debug: true
};
export default APP_CONFIG;
```

### **Importing in `app.js`:**
```javascript
// app.js

// Import named exports
import { API_URL, MAX_QUESTIONS, formatTime } from './config.js';

// Import with aliases
import { API_URL as endpoint, THEME_COLORS as colors } from './config.js';

// Import all named exports as an object
import * as config from './config.js';
console.log(config.API_URL);

// Import default export
import APP_CONFIG from './config.js';

// Mixed imports
import APP_CONFIG, { API_URL, formatTime } from './config.js';

// Usage
console.log(API_URL); // "https://api.example.com"
console.log(MAX_QUESTIONS); // 50
formatTime(125); // "2:5"
```

## 🏗️ **2. CommonJS (Node.js)**

### **Exporting from `config.js`:**
```javascript
// config.js

// Multiple exports
exports.API_URL = 'https://api.example.com';
exports.MAX_QUESTIONS = 50;

// Or module.exports
module.exports = {
    API_URL: 'https://api.example.com',
    MAX_QUESTIONS: 50,
    formatTime: function(seconds) {
        return `${Math.floor(seconds / 60)}:${seconds % 60}`;
    }
};
```

### **Importing in `app.js`:**
```javascript
// app.js

// Destructuring import
const { API_URL, MAX_QUESTIONS } = require('./config.js');

// Or import entire object
const config = require('./config.js');
console.log(config.API_URL);

// Usage
console.log(API_URL); // "https://api.example.com"
```

## 🌐 **3. For Your Quiz App - HTML Setup**

### **Update your HTML:**
```html
<!-- Add type="module" to your script tag -->
<script type="module" src="app.js"></script>
```

### **Create separate files:**

**`config.js`**
```javascript
// config.js
export const questionsData = {
    "Mathematics": {
        questions: [
            {
                question: "What is 2 + 2?",
                options: ["3", "4", "5", "6"],
                answers: ["4"]
            }
            // ... more questions
        ]
    }
};

export const APP_CONFIG = {
    testDuration: 30,
    maxSubjects: 5,
    themes: ['light', 'dark']
};

export const KEYBOARD_SHORTCUTS = {
    PREVIOUS: ['ArrowLeft', 'p'],
    NEXT: ['ArrowRight', 'n'],
    SUBMIT: ['s']
};
```

**`utils.js`**
```javascript
// utils.js
export function shuffleArray(array) {
    return array.sort(() => Math.random() - 0.5);
}

export function formatTime(seconds) {
    const mins = Math.floor(seconds / 60);
    const secs = seconds % 60;
    return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
}

export function calculateScore(correct, total) {
    return Math.round((correct / total) * 100);
}
```

**`app.js`**
```javascript
// app.js - Your main application file
import { questionsData, APP_CONFIG, KEYBOARD_SHORTCUTS } from './config.js';
import { shuffleArray, formatTime, calculateScore } from './utils.js';

// Now use the imported variables
console.log(questionsData.Mathematics.questions.length);
console.log(APP_CONFIG.testDuration);

// Use imported functions
const shuffledOptions = shuffleArray([1, 2, 3, 4]);
const timeString = formatTime(125);
```

## 🔄 **4. Dynamic Imports (Lazy Loading)**

```javascript
// Load module only when needed
async function loadQuestions(subject) {
    try {
        const questionsModule = await import(`./questions/${subject}.js`);
        return questionsModule.questions;
    } catch (error) {
        console.error('Failed to load questions:', error);
        return [];
    }
}

// Usage
const mathQuestions = await loadQuestions('mathematics');
```

## 🎯 **5. Complete Example for Your App Structure:**

### **File Structure:**
```
quiz-app/
├── index.html
├── app.js          (main application)
├── config.js       (configuration)
├── questions.js    (questions data)
├── utils.js        (utility functions)
└── ui.js           (UI functions)
```

**`questions.js`**
```javascript
// questions.js
export const questionsData = {
    "Mathematics": {
        questions: [
            {
                question: "Solve for x: $x^2 - 4 = 0$",
                options: ["x = ±2", "x = 2", "x = -2", "x = 4"],
                answers: ["x = ±2"],
                explanation: "The equation factors as (x-2)(x+2)=0"
            }
            // ... more questions
        ]
    },
    "Physics": {
        questions: [
            {
                question: "What is Newton's first law?",
                options: [
                    "F = ma",
                    "An object at rest stays at rest",
                    "For every action, there is an equal reaction",
                    "E = mc²"
                ],
                answers: ["An object at rest stays at rest"]
            }
        ]
    }
};

export const SUBJECTS = Object.keys(questionsData);
```

**`config.js`**
```javascript
// config.js
export const TEST_CONFIG = {
    DEFAULT_DURATION: 30,
    MIN_DURATION: 1,
    MAX_DURATION: 180,
    WARNING_TIME: 300, // 5 minutes
    DANGER_TIME: 60    // 1 minute
};

export const STORAGE_KEYS = {
    USER_PROFILE: 'userProfile',
    TEST_RESULTS: 'testResults',
    SETTINGS: 'appSettings'
};

export const KEYMAP = {
    PREVIOUS: ['ArrowLeft', 'p', 'a'],
    NEXT: ['ArrowRight', 'n', 'd'],
    SUBMIT: ['s', 'Enter'],
    CLOSE: ['Escape', 'q']
};
```

**`app.js`**
```javascript
// app.js - Main application
import { questionsData, SUBJECTS } from './questions.js';
import { TEST_CONFIG, STORAGE_KEYS, KEYMAP } from './config.js';
import { shuffleArray, formatTime, calculateScore } from './utils.js';

// Application State
const state = {
    selectedSubjects: [],
    testDuration: TEST_CONFIG.DEFAULT_DURATION,
    currentSubject: null,
    currentQuestionIndex: 0,
    userAnswers: {},
    timerInterval: null,
    timeRemaining: 0
};

// Use imported variables
function init() {
    console.log('Available subjects:', SUBJECTS);
    console.log('Test config:', TEST_CONFIG);
    populateSubjectGrid();
    setupEventListeners();
}

function populateSubjectGrid() {
    const subjectGrid = document.getElementById('subjectGrid');
    SUBJECTS.forEach(subject => {
        // Create subject cards using imported data
        const hasQuestions = questionsData[subject].questions.length > 0;
        if (hasQuestions) {
            createSubjectCard(subject);
        }
    });
}

// Use imported utility functions
function startTest() {
    state.timeRemaining = state.testDuration * 60;
    updateTimerDisplay();
    
    // Shuffle questions if needed
    state.selectedSubjects.forEach(subject => {
        const questions = questionsData[subject].questions;
        // shuffledQuestions = shuffleArray(questions); // Uncomment if you want shuffling
    });
}

function updateTimerDisplay() {
    const timerElement = document.getElementById('testTimer');
    timerElement.textContent = formatTime(state.timeRemaining);
}

// Initialize the app
document.addEventListener('DOMContentLoaded', init);
```

## ⚠️ **Important Notes:**

### **1. File Paths:**
```javascript
// Correct paths
import { something } from './config.js';      // Same directory
import { something } from '../utils.js';     // Parent directory  
import { something } from './lib/helpers.js'; // Subdirectory

// WRONG - missing ./
import { something } from 'config.js';       // Won't work!
```

### **2. Server Requirement:**
ES6 modules require a server (can't use `file://` protocol). Use:
- Live Server extension in VS Code
- `python -m http.server` 
- `npx http-server`

### **3. File Extensions:**
```javascript
// Include file extensions
import './config.js';    // ✅ Good
import './config';       // ❌ Bad (in browsers)
```

## 🚀 **Quick Start for Your App:**

1. **Add to HTML:**
```html
<script type="module" src="app.js"></script>
```

2. **Create `config.js`:**
```javascript
export const questionsData = { /* your questions */ };
export const APP_CONFIG = { /* your settings */ };
```

3. **Update `app.js`:**
```javascript
import { questionsData, APP_CONFIG } from './config.js';

// Remove your local questionsData and use imported one
```

This will make your code much more organized and maintainable!