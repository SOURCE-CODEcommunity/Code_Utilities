`firebase.firestore.FieldValue.serverTimestamp()` is used to **set a timestamp using Firestore's server time** rather than the client's local time.

## **Basic Usage:**

### **Setting a timestamp:**
```javascript
import { doc, setDoc, updateDoc, serverTimestamp } from "firebase/firestore";

// When creating a document
await setDoc(doc(db, "todos", "doc-id"), {
  text: "My todo",
  completed: false,
  createdAt: serverTimestamp(),  // ← Server time
  updatedAt: serverTimestamp()   // ← Server time
});

// When updating a document
await updateDoc(doc(db, "todos", "doc-id"), {
  completed: true,
  updatedAt: serverTimestamp()  // ← Updated to current server time
});
```

## **Why use server timestamp:**

### **Client time (problematic):**
```javascript
// This uses client's local time (can be wrong)
createdAt: new Date()
```

### **Server time (recommended):**
```javascript
// This uses Firestore server time (always accurate)
createdAt: serverTimestamp()
```

## **Common use cases:**

### **1. Creation and update timestamps:**
```javascript
const todoData = {
  text: "Learn Firestore",
  completed: false,
  createdAt: serverTimestamp(),
  updatedAt: serverTimestamp(),
  author: user.uid
};
await setDoc(doc(db, "todos", "doc-id"), todoData);
```

### **2. Update only the timestamp:**
```javascript
// Update just the "updatedAt" field
await updateDoc(doc(db, "todos", "doc-id"), {
  updatedAt: serverTimestamp()
});
```

### **3. With security rules:**
```javascript
// Prevent users from setting fake timestamps
allow create: if request.resource.data.createdAt == request.time;
allow update: if request.resource.data.updatedAt == request.time;
```

## **What it looks like in Firestore:**
When you read the document, the timestamp field will be a `Timestamp` object that you can convert:

```javascript
const docSnap = await getDoc(doc(db, "todos", "doc-id"));
if (docSnap.exists()) {
  const data = docSnap.data();
  console.log(data.createdAt); // Firestore Timestamp object
  console.log(data.createdAt.toDate()); // JavaScript Date object
  console.log(data.createdAt.toMillis()); // Milliseconds since epoch
}
```

## **Important notes:**
- **Initially shows as `null`** - The timestamp is populated on the server, so it might be `null` briefly until the write completes
- **Consistent ordering** - Perfect for sorting documents by time
- **Trustworthy** - Can't be manipulated by clients

Use `serverTimestamp()` whenever you need accurate, trustworthy timestamps in your app!