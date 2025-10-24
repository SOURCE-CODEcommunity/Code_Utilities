There are several ways to track if a user is currently active on a page. Here are the most common approaches:

## **1. Real-time Activity Listeners (Recommended)**

```javascript
import { doc, updateDoc, serverTimestamp } from "firebase/firestore";

let activityTimeout;

function trackUserActivity() {
  const events = ['mousemove', 'keydown', 'scroll', 'click', 'touchstart'];
  
  events.forEach(event => {
    window.addEventListener(event, updateLastActive, { passive: true });
  });
}

function updateLastActive() {
  // Clear existing timeout
  clearTimeout(activityTimeout);
  
  // Update immediately
  updateUserStatus('online');
  
  // Set timeout for offline status (e.g., 30 seconds of inactivity)
  activityTimeout = setTimeout(() => {
    updateUserStatus('offline');
  }, 30000);
}

async function updateUserStatus(status) {
  if (!currentUser) return;
  
  try {
    await updateDoc(doc(db, "userStatus", currentUser.uid), {
      status: status,
      lastActive: serverTimestamp(),
      ...(status === 'online' && { isOnline: true })
    });
  } catch (error) {
    console.log("Error updating status:", error);
  }
}
```

## **2. Page Visibility API**

```javascript
// Track when user switches tabs/minimizes window
document.addEventListener('visibilitychange', () => {
  if (document.hidden) {
    updateUserStatus('away');
  } else {
    updateUserStatus('online');
  }
});
```

## **3. Combined Approach (Most Robust)**

```javascript
class UserActivityTracker {
  constructor(userId) {
    this.userId = userId;
    this.timeout = null;
    this.isActive = true;
    
    this.init();
  }
  
  init() {
    // Mouse/keyboard events
    const events = ['mousemove', 'keydown', 'scroll', 'click', 'touchstart'];
    events.forEach(event => {
      window.addEventListener(event, this.handleActivity.bind(this));
    });
    
    // Page visibility
    document.addEventListener('visibilitychange', this.handleVisibility.bind(this));
    
    // Before page unload
    window.addEventListener('beforeunload', this.handlePageUnload.bind(this));
    
    // Initial status
    this.updateStatus('online');
  }
  
  handleActivity() {
    if (!this.isActive) {
      this.updateStatus('online');
      this.isActive = true;
    }
    
    // Reset timeout
    clearTimeout(this.timeout);
    this.timeout = setTimeout(() => {
      this.isActive = false;
      this.updateStatus('away');
    }, 30000); // 30 seconds inactivity
  }
  
  handleVisibility() {
    if (document.hidden) {
      this.updateStatus('away');
    } else {
      this.updateStatus('online');
    }
  }
  
  handlePageUnload() {
    this.updateStatus('offline');
  }
  
  async updateStatus(status) {
    try {
      await updateDoc(doc(db, "userStatus", this.userId), {
        status: status,
        lastActive: serverTimestamp(),
        isOnline: status === 'online'
      });
    } catch (error) {
      console.log("Status update error:", error);
    }
  }
  
  destroy() {
    clearTimeout(this.timeout);
    // Remove event listeners
    // Update status to offline
    this.updateStatus('offline');
  }
}

// Usage
const activityTracker = new UserActivityTracker(currentUser.uid);
```

## **4. Simple Heartbeat System**

```javascript
// Send heartbeat every 15 seconds while active
let heartbeatInterval;

function startHeartbeat() {
  updateUserStatus('online');
  heartbeatInterval = setInterval(() => {
    updateDoc(doc(db, "userStatus", currentUser.uid), {
      lastActive: serverTimestamp(),
      isOnline: true
    });
  }, 15000);
}

function stopHeartbeat() {
  clearInterval(heartbeatInterval);
  updateUserStatus('offline');
}

// Start when page loads
startHeartbeat();

// Stop when page unloads
window.addEventListener('beforeunload', stopHeartbeat);
```

## **Firestore Security Rules:**

```javascript
match /userStatus/{userId} {
  allow read: if true; // Anyone can read status
  allow write: if request.auth != null 
    && request.auth.uid == userId
    && request.resource.data.lastActive == request.time; // Ensure timestamp is server-generated
}
```

## **Which to choose:**
- **Real-time listeners**: Most accurate, detects any user interaction
- **Heartbeat**: Simpler, but less precise
- **Combined**: Best for production apps

The real-time listener approach is usually the most reliable for detecting active users!