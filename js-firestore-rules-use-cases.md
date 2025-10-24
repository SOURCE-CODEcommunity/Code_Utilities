# Complete Firestore Security Rules Guide

## 🔧 **Understanding Rules Structure**

### **Basic Pattern:**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Your rules go here
  }
}
```

## 🏗️ **Common Application Patterns**

### **1. Social Media App (Twitter-like)**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can only manage their own profile
    match /users/{userId} {
      allow read: if request.auth != null; // Anyone can view profiles
      allow write: if request.auth != null && request.auth.uid == userId;
    }
    
    // Posts: anyone can read, only owner can modify
    match /posts/{postId} {
      allow read: if true; // Public read
      allow create: if request.auth != null 
        && request.resource.data.userId == request.auth.uid;
      allow update, delete: if request.auth != null 
        && resource.data.userId == request.auth.uid;
    }
    
    // Comments: anyone can read, authenticated users can write
    match /posts/{postId}/comments/{commentId} {
      allow read: if true;
      allow create: if request.auth != null 
        && request.resource.data.userId == request.auth.uid;
      allow update, delete: if request.auth != null 
        && resource.data.userId == request.auth.uid;
    }
    
    // Likes: simple tracking
    match /posts/{postId}/likes/{userId} {
      allow read: if true;
      allow write: if request.auth != null 
        && request.auth.uid == userId;
    }
  }
}
```

### **2. E-commerce Store**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Public product catalog
    match /products/{productId} {
      allow read: if true; // Anyone can browse products
      allow write: if request.auth != null 
        && get(/databases/$(database)/documents/admins/$(request.auth.uid)).exists;
    }
    
    // User orders - private to each user
    match /orders/{orderId} {
      allow read, write: if request.auth != null 
        && resource.data.userId == request.auth.uid;
      allow create: if request.auth != null 
        && request.resource.data.userId == request.auth.uid;
    }
    
    // Shopping carts - user specific
    match /carts/{userId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
    
    // Admin-only collections
    match /analytics/{document} {
      allow read, write: if request.auth != null 
        && get(/databases/$(database)/documents/admins/$(request.auth.uid)).exists;
    }
  }
}
```

### **3. Blog/CMS Platform**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Published articles - public read
    match /articles/{articleId} {
      allow read: if resource.data.published == true || 
        (request.auth != null && request.auth.uid == resource.data.authorId);
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null 
        && resource.data.authorId == request.auth.uid;
    }
    
    // Drafts - only visible to author
    match /drafts/{draftId} {
      allow read, write: if request.auth != null 
        && resource.data.authorId == request.auth.uid;
    }
    
    // Categories - public read, admin write
    match /categories/{categoryId} {
      allow read: if true;
      allow write: if request.auth != null 
        && get(/databases/$(database)/documents/admins/$(request.auth.uid)).exists;
    }
  }
}
```

### **4. Chat Application**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Users can read/write their own data
    match /users/{userId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
    
    // Chat rooms - members can read/write
    match /chatRooms/{roomId} {
      allow read, write: if request.auth != null 
        && resource.data.members.hasAny([request.auth.uid]);
      allow create: if request.auth != null 
        && request.resource.data.members.hasAny([request.auth.uid]);
    }
    
    // Messages within chat rooms
    match /chatRooms/{roomId}/messages/{messageId} {
      allow read: if request.auth != null 
        && get(/databases/$(database)/documents/chatRooms/$(roomId)).data.members.hasAny([request.auth.uid]);
      allow create: if request.auth != null 
        && request.auth.uid == request.resource.data.senderId
        && get(/databases/$(database)/documents/chatRooms/$(roomId)).data.members.hasAny([request.auth.uid]);
    }
  }
}
```

### **5. Task/Project Management**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Projects - team members can access
    match /projects/{projectId} {
      allow read, write: if request.auth != null 
        && resource.data.teamMembers.hasAny([request.auth.uid]);
      allow create: if request.auth != null 
        && request.resource.data.teamMembers.hasAny([request.auth.uid]);
    }
    
    // Tasks within projects
    match /projects/{projectId}/tasks/{taskId} {
      allow read, write: if request.auth != null 
        && get(/databases/$(database)/documents/projects/$(projectId)).data.teamMembers.hasAny([request.auth.uid]);
    }
    
    // User-specific preferences
    match /userPreferences/{userId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
  }
}
```

## 🔐 **Advanced Security Patterns**

### **Role-Based Access Control**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Helper functions
    function isSignedIn() {
      return request.auth != null;
    }
    
    function isAdmin() {
      return isSignedIn() && 
        exists(/databases/$(database)/documents/admins/$(request.auth.uid));
    }
    
    function isUser(userId) {
      return isSignedIn() && request.auth.uid == userId;
    }
    
    function hasRole(role) {
      return isSignedIn() && 
        get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == role;
    }
    
    // Application rules using helpers
    match /users/{userId} {
      allow read: if isSignedIn();
      allow write: if isUser(userId) || isAdmin();
    }
    
    match /content/{document} {
      allow read: if true;
      allow write: if hasRole('editor') || hasRole('admin');
    }
    
    match /analytics/{document} {
      allow read, write: if hasRole('admin');
    }
  }
}
```

### **Time-Based Rules**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Events - only editable before start time
    match /events/{eventId} {
      allow read: if true;
      allow write: if request.auth != null && 
        request.time < resource.data.startTime;
    }
    
    // Limited time offers
    match /offers/{offerId} {
      allow read: if request.time < resource.data.expiresAt;
      allow write: if isAdmin(); // Your admin check
    }
  }
}
```

## 🎯 **Quick Rule Templates**

### **Template 1: Public Read, Owner Write**
```javascript
match /{collection}/{document} {
  allow read: if true; // Public
  allow write: if request.auth != null && 
    request.auth.uid == resource.data.ownerId;
}
```

### **Template 2: Authenticated Users Only**
```javascript
match /{collection}/{document} {
  allow read, write: if request.auth != null;
}
```

### **Template 3: Owner-Only Access**
```javascript
match /{collection}/{document} {
  allow read, write: if request.auth != null && 
    request.auth.uid == resource.data.userId;
}
```

### **Template 4: Mixed Access**
```javascript
match /{collection}/{document} {
  allow read: if true; // Public read
  allow create: if request.auth != null; // Any auth user can create
  allow update, delete: if request.auth != null && 
    request.auth.uid == resource.data.ownerId; // Only owner can modify
}
```

## 🔧 **Rule Building Blocks**

### **Common Conditions:**
```javascript
// Authentication checks
request.auth != null                          // User is signed in
request.auth.uid == 'specific-user-id'        // Specific user
request.auth.token.email == 'user@email.com'  // User email

// Data validation
request.resource.data.size() < 10000          // Limit data size
request.resource.data.keys().hasAll(['name', 'email']) // Required fields
request.resource.data.score is int            // Type checking

// Time-based
request.time < timestamp.date(2024, 12, 31)   // Before date

// Document references
exists(/databases/$(database)/documents/users/$(request.auth.uid))
get(/databases/$(database)/documents/config/public).data.allowWrite
```

### **Data Validation Examples:**
```javascript
match /posts/{postId} {
  allow create: if 
    request.auth != null &&
    request.resource.data.title is string &&
    request.resource.data.title.size() > 0 &&
    request.resource.data.title.size() < 100 &&
    request.resource.data.content is string &&
    request.resource.data.content.size() < 10000 &&
    request.resource.data.authorId == request.auth.uid &&
    request.resource.data.createdAt == request.time;
}
```

## 🚨 **Common Mistakes & Fixes**

### **Problem: "Missing or insufficient permissions"**
```javascript
// WRONG - Trying to access user data without proper rules
match /users/{userId} {
  allow read, write: if true; // Too permissive!
}

// CORRECT - User-specific access
match /users/{userId} {
  allow read, write: if request.auth != null && 
    request.auth.uid == userId;
}
```

### **Problem: Can't query collections**
```javascript
// Need to allow list operations
match /posts/{postId} {
  allow list: if true; // Allow listing/querying
  allow get: if true;  // Allow reading single documents
  allow create: if request.auth != null;
}
```

## 🛠️ **Testing Your Rules**

### **Use Firebase Emulator:**
```bash
# Install emulator
firebase init emulators

# Start emulators
firebase emulators:start

# Test rules
firebase emulators:exec --only firestore "npm test"
```

### **Sample Test:**
```javascript
// test-rules.js
const firebase = require('@firebase/testing');

describe("Firestore Rules", () => {
  it("should allow users to read their own data", async () => {
    const db = firebase.initializeTestApp({
      projectId: "my-project",
      auth: { uid: "user1" }
    }).firestore();

    await firebase.assertSucceeds(
      db.collection('users').doc('user1').get()
    );
  });
});
```

## 📋 **Quick Reference Card**

| Application Type | Read Rule | Write Rule |
|------------------|-----------|------------|
| **Public Blog** | `if true` | `if isAdmin()` |
| **Social App** | `if true` | `if isOwner()` |
| **E-commerce** | `if true` | `if isOwner() || isAdmin()` |
| **Chat App** | `if isMember()` | `if isMember()` |
| **Internal Tool** | `if isAuthenticated()` | `if isAuthenticated()` |

## 🎯 **Your To-Do App Rules (Improved)**
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Todos - user can only access their own
    match /todos/{todoId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == resource.data.userId;
      allow create: if request.auth != null 
        && request.resource.data.userId == request.auth.uid;
    }
    
    // User status - users can only manage their own
    match /userStatus/{userId} {
      allow read, write: if request.auth != null 
        && request.auth.uid == userId;
    }
  }
}
```

Choose the pattern that matches your application type and customize as needed!