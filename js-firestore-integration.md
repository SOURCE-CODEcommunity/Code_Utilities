# Complete Firestore & Firebase Authentication Guide

## 🔥 **Firebase Setup & Configuration**

### **1. Create Firebase Project**
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click "Add project"
3. Enter project name and follow setup steps

### **2. Get Firebase Config**
In your Firebase Console:
1. Project Settings → General → Your apps
2. Add web app if not exists
3. Copy config object

**`config.js`**
```javascript
// config.js
const firebaseConfig = {
    apiKey: "your-api-key",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your-app-id"
};
```

## 📊 **Firestore Database Basics**

### **Firestore Structure:**
```
Collection → Document → Field
    ↓
todos (collection)
├── document1
│   ├── text: "Buy groceries"
│   ├── completed: false
│   └── userId: "user123"
└── document2
    ├── text: "Walk dog"
    ├── completed: true
    └── userId: "user456"
```

### **Basic Operations:**

#### **Add Data:**
```javascript
// Add to collection (auto-generate ID)
await db.collection('todos').add({
    text: "Learn Firestore",
    completed: false,
    userId: "user123",
    createdAt: firebase.firestore.FieldValue.serverTimestamp()
});

// Add with custom ID
await db.collection('todos').doc('custom-id').set({
    text: "Custom todo",
    completed: false
});
```

#### **Read Data:**
```javascript
// Get single document
const doc = await db.collection('todos').doc('document-id').get();
if (doc.exists) {
    console.log(doc.data());
}

// Get all documents in collection
const snapshot = await db.collection('todos').get();
snapshot.forEach(doc => {
    console.log(doc.id, doc.data());
});
```

#### **Update Data:**
```javascript
// Update specific fields
await db.collection('todos').doc('doc-id').update({
    completed: true,
    updatedAt: firebase.firestore.FieldValue.serverTimestamp()
});

// Set entire document (overwrites)
await db.collection('todos').doc('doc-id').set({
    text: "Updated text",
    completed: true
}, { merge: true }); // Merge with existing data
```

#### **Delete Data:**
```javascript
await db.collection('todos').doc('doc-id').delete();
```

## 🔐 **Firebase Authentication**

### **Setup Authentication:**
1. Firebase Console → Authentication → Get started
2. Enable sign-in methods:
   - Email/Password
   - Google
   - Anonymous

### **Authentication Methods:**

#### **1. Anonymous Auth:**
```javascript
// Your current method
auth.signInAnonymously()
    .then((userCredential) => {
        const user = userCredential.user;
        console.log("Anonymous user:", user.uid);
    })
    .catch(error => {
        console.error("Anonymous auth error:", error);
    });
```

#### **2. Email/Password Auth:**
```javascript
// Sign up
async function signUp(email, password) {
    try {
        const userCredential = await auth.createUserWithEmailAndPassword(email, password);
        console.log("User created:", userCredential.user);
    } catch (error) {
        console.error("Sign up error:", error.message);
    }
}

// Sign in
async function signIn(email, password) {
    try {
        const userCredential = await auth.signInWithEmailAndPassword(email, password);
        console.log("User signed in:", userCredential.user);
    } catch (error) {
        console.error("Sign in error:", error.message);
    }
}
```

#### **3. Google Auth:**
```html
<!-- Add Google button -->
<button id="google-signin">Sign in with Google</button>
```

```javascript
// Configure Google provider
const provider = new firebase.auth.GoogleAuthProvider();

// Sign in with Google
document.getElementById('google-signin').addEventListener('click', () => {
    auth.signInWithPopup(provider)
        .then((result) => {
            const user = result.user;
            console.log("Google user:", user);
        })
        .catch(error => {
            console.error("Google auth error:", error);
        });
});
```

#### **4. Auth State Listener:**
```javascript
// Listen for auth state changes
auth.onAuthStateChanged((user) => {
    if (user) {
        console.log("User is signed in:", user.uid);
        // User is signed in
    } else {
        console.log("User is signed out");
        // User is signed out
    }
});
```

## 🛡️ **Security Rules (Crucial!)**

### **Firestore Rules:**
In Firebase Console → Firestore Database → Rules

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Allow read/write only if authenticated
    match /todos/{document} {
      allow read, write: if request.auth != null 
        && request.auth.uid == resource.data.userId;
    }
    
    // Allow users to read/write their own status
    match /userStatus/{userId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
    
    // Public read, authenticated write
    match /publicData/{document} {
      allow read: if true;
      allow write: if request.auth != null;
    }
  }
}
```

## 🔄 **Real-time Listeners**

### **Types of Listeners:**

#### **1. Collection Listener:**
```javascript
// Real-time updates for entire collection
const unsubscribe = db.collection('todos')
    .where('userId', '==', userId)
    .orderBy('createdAt', 'desc')
    .onSnapshot(snapshot => {
        snapshot.docChanges().forEach(change => {
            if (change.type === 'added') {
                console.log('New todo:', change.doc.data());
            }
            if (change.type === 'modified') {
                console.log('Modified todo:', change.doc.data());
            }
            if (change.type === 'removed') {
                console.log('Removed todo:', change.doc.id);
            }
        });
    });

// Stop listening later
unsubscribe();
```

#### **2. Document Listener:**
```javascript
// Real-time updates for single document
db.collection('todos').doc('specific-doc')
    .onSnapshot(doc => {
        if (doc.exists) {
            console.log('Current data:', doc.data());
        }
    });
```

## 📝 **Queries & Filtering**

### **Common Query Patterns:**
```javascript
// Basic where clause
db.collection('todos')
    .where('completed', '==', false)
    .get();

// Multiple conditions
db.collection('todos')
    .where('userId', '==', userId)
    .where('completed', '==', false)
    .get();

// Range queries
db.collection('todos')
    .where('priority', '>=', 3)
    .where('priority', '<=', 5)
    .get();

// Ordering and limiting
db.collection('todos')
    .orderBy('createdAt', 'desc')
    .limit(10)
    .get();

// Compound queries (need composite indexes)
db.collection('todos')
    .where('userId', '==', userId)
    .where('completed', '==', false)
    .orderBy('createdAt', 'desc')
    .get();
```

## 🏗️ **Enhanced To-Do App with Authentication**

### **Improved HTML:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Enhanced Firestore To-Do</title>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/10.7.1/firebase-auth-compat.js"></script>
    <style>
        .auth-section { margin-bottom: 20px; padding: 15px; background: #f0f0f0; border-radius: 8px; }
        .user-info { margin: 10px 0; }
        .hidden { display: none; }
    </style>
</head>
<body>
    <div id="app">
        <h1>Enhanced Firestore To-Do</h1>
        
        <!-- Authentication Section -->
        <div class="auth-section">
            <div id="signed-out">
                <button id="google-signin">Sign in with Google</button>
                <button id="anonymous-signin">Continue Anonymously</button>
                <div>
                    <input type="email" id="email" placeholder="Email">
                    <input type="password" id="password" placeholder="Password">
                    <button id="email-signup">Sign Up</button>
                    <button id="email-signin">Sign In</button>
                </div>
            </div>
            <div id="signed-in" class="hidden">
                <div class="user-info">
                    Welcome, <span id="user-name"></span>!
                    <button id="signout">Sign Out</button>
                </div>
            </div>
        </div>

        <!-- Rest of your to-do app -->
        <div id="todo-app" class="hidden">
            <!-- Your existing to-do interface -->
        </div>
    </div>

    <script src="config.js"></script>
    <script>
        // Enhanced app with multiple auth methods
        const app = firebase.initializeApp(firebaseConfig);
        const db = firebase.firestore();
        const auth = firebase.auth();
        
        // DOM Elements
        const signedOutUI = document.getElementById('signed-out');
        const signedInUI = document.getElementById('signed-in');
        const todoApp = document.getElementById('todo-app');
        const userNameSpan = document.getElementById('user-name');
        // ... other DOM elements

        // Auth state listener
        auth.onAuthStateChanged(user => {
            if (user) {
                // User is signed in
                signedOutUI.classList.add('hidden');
                signedInUI.classList.remove('hidden');
                todoApp.classList.remove('hidden');
                
                // Display user info
                userNameSpan.textContent = user.displayName || user.email || 'Anonymous User';
                
                // Setup Firestore listeners
                setupFirestoreListeners(user.uid);
            } else {
                // User is signed out
                signedOutUI.classList.remove('hidden');
                signedInUI.classList.add('hidden');
                todoApp.classList.add('hidden');
            }
        });

        // Auth methods
        document.getElementById('google-signin').addEventListener('click', () => {
            const provider = new firebase.auth.GoogleAuthProvider();
            auth.signInWithPopup(provider);
        });

        document.getElementById('anonymous-signin').addEventListener('click', () => {
            auth.signInAnonymously();
        });

        document.getElementById('email-signup').addEventListener('click', () => {
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            auth.createUserWithEmailAndPassword(email, password);
        });

        document.getElementById('email-signin').addEventListener('click', () => {
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            auth.signInWithEmailAndPassword(email, password);
        });

        document.getElementById('signout').addEventListener('click', () => {
            auth.signOut();
        });

        // Firestore functions (now using auth UID)
        function setupFirestoreListeners(userId) {
            // Todos listener with user-based security
            db.collection('todos')
                .where('userId', '==', userId)
                .orderBy('createdAt', 'desc')
                .onSnapshot(snapshot => {
                    // Update UI
                });
        }

        // Add todo with authenticated user
        async function addTodo(text) {
            const user = auth.currentUser;
            if (!user) return;
            
            await db.collection('todos').add({
                text: text,
                completed: false,
                userId: user.uid,
                userEmail: user.email,
                createdAt: firebase.firestore.FieldValue.serverTimestamp()
            });
        }
    </script>
</body>
</html>
```

## ⚡ **Performance & Best Practices**

### **1. Use Composite Indexes:**
Firestore will prompt you to create indexes for complex queries.

### **2. Paginate Large Datasets:**
```javascript
// Pagination example
const firstPage = await db.collection('todos')
    .orderBy('createdAt')
    .limit(10)
    .get();

const lastDoc = firstPage.docs[firstPage.docs.length - 1];
const nextPage = await db.collection('todos')
    .orderBy('createdAt')
    .startAfter(lastDoc)
    .limit(10)
    .get();
```

### **3. Batch Operations:**
```javascript
// Batch write (atomic operation)
const batch = db.batch();

batch.set(db.collection('todos').doc(), {
    text: "Todo 1",
    userId: user.uid
});

batch.update(db.collection('todos').doc('doc2'), {
    completed: true
});

await batch.commit();
```

### **4. Error Handling:**
```javascript
try {
    await db.collection('todos').add({
        text: "New todo",
        userId: user.uid
    });
} catch (error) {
    if (error.code === 'permission-denied') {
        console.log("User doesn't have permission");
    } else if (error.code === 'unavailable') {
        console.log("Network error - retrying...");
    } else {
        console.error("Unexpected error:", error);
    }
}
```

## 🚀 **Getting Started Steps:**

1. **Create Firebase project**
2. **Enable Firestore Database**
3. **Enable Authentication methods**
4. **Set up Security Rules**
5. **Add your config to `config.js`**
6. **Start with basic operations**
7. **Add authentication**
8. **Implement real-time listeners**

This gives you a solid foundation for building any Firestore-powered app!