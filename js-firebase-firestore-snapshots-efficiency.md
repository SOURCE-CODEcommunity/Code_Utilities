This is a **common issue** with `serverTimestamp()`! The timestamp appears as `null` when you read the document **immediately after writing**.

## **Why This Happens:**

### **Server Timestamp Behavior:**
```javascript
// When you write:
await updateDoc(doc(db, "users", "user123"), {
  lastActive: serverTimestamp(), // ← This is a placeholder
  name: "John",
  email: "john@example.com"
});

// Immediately after:
const docSnap = await getDoc(doc(db, "users", "user123"));
console.log(docSnap.data());
// Output: { name: "John", email: "john@example.com", lastActive: null }
```

The `serverTimestamp()` is a **placeholder** that gets replaced with the actual server time **on the server side**, but there's a brief period where it shows as `null`.

## **Solutions:**

### **1. Wait for the Write to Complete (Recommended)**
```javascript
// Write and then wait a bit
await updateDoc(doc(db, "users", "user123"), {
  lastActive: serverTimestamp(),
  name: "John"
});

// Small delay to ensure server processing
await new Promise(resolve => setTimeout(resolve, 100));

const docSnap = await getDoc(doc(db, "users", "user123"));
console.log(docSnap.data().lastActive); // Now shows actual timestamp
```

### **2. Use Real-time Listener (`onSnapshot`)**
```javascript
// onSnapshot gives you real-time updates including the resolved timestamp
const unsubscribe = onSnapshot(doc(db, "users", "user123"), (docSnap) => {
  if (docSnap.exists()) {
    const data = docSnap.data();
    console.log("Real-time data:", data.lastActive); // Will show actual timestamp
  }
});

// Update the document
await updateDoc(doc(db, "users", "user123"), {
  lastActive: serverTimestamp()
});

// Don't forget to unsubscribe later
// unsubscribe();
```

### **3. Use Local Timestamp + Server Timestamp**
```javascript
// Store both client and server timestamps
const localTime = new Date();

await updateDoc(doc(db, "users", "user123"), {
  lastActive: serverTimestamp(),
  lastActiveLocal: localTime, // Fallback
  name: "John"
});

const docSnap = await getDoc(doc(db, "users", "user123"));
const data = docSnap.data();

// Use server timestamp if available, otherwise fallback to local
const effectiveTime = data.lastActive || data.lastActiveLocal;
```

### **4. Check Multiple Times**
```javascript
async function getDocWithRetry(docRef, maxRetries = 3) {
  for (let i = 0; i < maxRetries; i++) {
    const docSnap = await getDoc(docRef);
    const data = docSnap.data();
    
    if (data.lastActive !== null) {
      return docSnap; // Timestamp is resolved
    }
    
    // Wait before retrying
    await new Promise(resolve => setTimeout(resolve, 200));
  }
  
  return await getDoc(docRef); // Final attempt
}

// Usage
const docSnap = await getDocWithRetry(doc(db, "users", "user123"));
```

## **Working with the Timestamp:**

When it **does** resolve, you'll get a `Timestamp` object:

```javascript
const docSnap = await getDoc(doc(db, "users", "user123"));
const data = docSnap.data();

if (data.lastActive) {
  console.log(data.lastActive); // Firestore Timestamp object
  console.log(data.lastActive.toDate()); // JavaScript Date object
  console.log(data.lastActive.toMillis()); // Milliseconds since epoch
  console.log(data.lastActive.seconds); // Seconds since epoch
  console.log(data.lastActive.nanoseconds); // Nanoseconds
}
```

## **Best Practice for Activity Tracking:**

```javascript
async function updateLastActive(userId) {
  // Update with server timestamp
  await updateDoc(doc(db, "users", userId), {
    lastActive: serverTimestamp(),
    status: 'online'
  });
  
  // Use real-time listener to get the resolved timestamp
  return new Promise((resolve) => {
    const unsubscribe = onSnapshot(doc(db, "users", userId), (docSnap) => {
      if (docSnap.exists() && docSnap.data().lastActive) {
        unsubscribe(); // Stop listening after we get the timestamp
        resolve(docSnap.data().lastActive);
      }
    });
    
    // Timeout fallback
    setTimeout(() => {
      unsubscribe();
      resolve(null);
    }, 2000);
  });
}

// Usage
const timestamp = await updateLastActive("user123");
```

## **Key Takeaways:**
- `serverTimestamp()` starts as `null` and gets resolved on the server
- Use **real-time listeners** (`onSnapshot`) to get the actual timestamp
- Add a **small delay** if using `getDoc()` after writing
- Consider **fallback strategies** for critical timing needs

The timestamp will eventually resolve to the actual server time, but there's a brief window where it shows as `null`!