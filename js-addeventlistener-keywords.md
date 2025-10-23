Here's a comprehensive list of all JavaScript event listener keywords, categorized by type:

## 📝 **Mouse Events**
```javascript
// Basic mouse events
'click'          // Element is clicked
'dblclick'       // Double click
'mousedown'      // Mouse button is pressed
'mouseup'        // Mouse button is released
'mousemove'      // Mouse is moved over element
'mouseover'      // Mouse enters element
'mouseout'       // Mouse leaves element
'mouseenter'     // Mouse enters element (no bubbling)
'mouseleave'     // Mouse leaves element (no bubbling)
'mousewheel'     // Mouse wheel is rotated
'wheel'          // Modern wheel event

// Context menu
'contextmenu'    // Right-click context menu

// Drag and drop
'drag'           // Element is being dragged
'dragstart'      // Dragging starts
'dragend'        // Dragging ends
'dragenter'      // Dragged element enters drop target
'dragleave'      // Dragged element leaves drop target
'dragover'       // Dragged element is over drop target
'drop'           // Element is dropped on target
```

## ⌨️ **Keyboard Events**
```javascript
'keydown'        // Key is pressed down
'keyup'          // Key is released
'keypress'       // Key is pressed (deprecated, use keydown)
```

## 📄 **Form Events**
```javascript
'submit'         // Form is submitted
'reset'          // Form is reset
'change'         // Input value changes (after blur)
'input'          // Input value changes (immediately)
'focus'          // Element gains focus
'focusin'        // Element gains focus (bubbles)
'blur'           // Element loses focus
'focusout'       // Element loses focus (bubbles)
'select'         // Text is selected in input
'invalid'        // Form validation fails
```

## 🖼️ **Window/Document Events**
```javascript
// Loading events
'load'           // Page/resources finished loading
'DOMContentLoaded' // DOM is ready (no images)
'beforeunload'   // Before page is unloaded
'unload'         // Page is being unloaded

// Resize and scroll
'resize'         // Window is resized
'scroll'         // Element is scrolled

// Visibility
'visibilitychange' // Tab becomes visible/hidden
```

## 📱 **Touch Events**
```javascript
'touchstart'     // Touch starts
'touchend'       // Touch ends
'touchmove'      // Touch moves
'touchcancel'    // Touch is interrupted
```

## 🎮 **Gamepad Events**
```javascript
'gamepadconnected'    // Gamepad is connected
'gamepaddisconnected' // Gamepad is disconnected
```

## 📍 **Pointer Events** (Universal pointer input)
```javascript
'pointerdown'    // Pointer is pressed
'pointerup'      // Pointer is released
'pointermove'    // Pointer moves
'pointerover'    // Pointer enters element
'pointerout'     // Pointer leaves element
'pointerenter'   // Pointer enters element
'pointerleave'   // Pointer leaves element
'pointercancel'  // Pointer interaction cancelled
```

## 🎵 **Media Events**
```javascript
// Audio/Video events
'play'           // Media starts playing
'pause'          // Media is paused
'ended'          // Media playback ends
'timeupdate'     // Playback position changes
'volumechange'   // Volume changes
'loadeddata'     // Media data is loaded
'canplay'        // Media can start playing
'waiting'        // Media pauses for buffering
'seeking'        // Seeking operation begins
'seeked'         // Seeking operation ends

// Fullscreen
'fullscreenchange'    // Fullscreen mode changes
'fullscreenerror'     // Fullscreen error occurs
```

## 🔗 **Clipboard Events**
```javascript
'copy'           // Content is copied
'cut'            // Content is cut
'paste'          // Content is pasted
```

## 🖨️ **Print Events**
```javascript
'beforeprint'    // Before printing
'afterprint'     // After printing
```

## 📡 **Network Events**
```javascript
'online'         // Browser goes online
'offline'        // Browser goes offline
```

## 🎯 **Selection Events**
```javascript
'selectstart'    // Text selection starts
'selectionchange' // Text selection changes
```

## 🏗️ **CSS/Animation Events**
```javascript
'transitionstart'    // CSS transition starts
'transitionend'      // CSS transition ends
'transitioncancel'   // CSS transition cancelled
'transitionrun'      // CSS transition is created

'animationstart'     // CSS animation starts
'animationend'       // CSS animation ends
'animationcancel'    // CSS animation cancelled
'animationiteration' // CSS animation repeats
```

## 🔄 **Mutation Events** (Observing DOM changes)
```javascript
'DOMNodeInserted'           // Node is inserted
'DOMNodeRemoved'            // Node is removed
'DOMSubtreeModified'        // Subtree is modified
'DOMAttrModified'           // Attribute is modified
'DOMCharacterDataModified'  // Text data is modified
```

## 📦 **Storage Events**
```javascript
'storage'        // localStorage/sessionStorage changes
```

## 🌐 **Page Visibility Events**
```javascript
'pageshow'       // Page is shown
'pagehide'       // Page is hidden
```

## 🔄 **History Events**
```javascript
'popstate'       // Browser navigation (back/forward)
'hashchange'     // URL hash changes
```

## 📋 **Clipboard API Events**
```javascript
'clipboard-write' // Writing to clipboard
'clipboard-read'  // Reading from clipboard
```

## 🎮 **VR/AR Events**
```javascript
'vrdisplaypresentchange' // VR display presentation changes
'vrdisplayconnect'       // VR display connected
'vrdisplaydisconnect'    // VR display disconnected
```

## 🔔 **Notification Events**
```javascript
'show'           // Notification is shown
'click'          // Notification is clicked
'close'          // Notification is closed
'error'          // Notification error
```

## 📹 **Picture-in-Picture Events**
```javascript
'enterpictureinpicture'  // Enters PiP mode
'leavepictureinpicture'  // Leaves PiP mode
```

## 🔒 **Security Events**
```javascript
'securitypolicyviolation' // Content Security Policy violation
```

## 🛰️ **Geolocation Events**
```javascript
'geolocationchange' // Geolocation data changes
```

## 💾 **File Events**
```javascript
'change'         // File input changes (for files)
```

## 🎪 **Special Events**
```javascript
'error'          // Error occurs
'abort'          // Loading is aborted
'cancel'         // Operation is cancelled
'show'           // Element is shown (for details/menu)
'toggle'         // Element is toggled (for details)
```

## 📚 **Custom Events**
```javascript
// You can create your own custom events
const event = new CustomEvent('myCustomEvent', {
    detail: { message: 'Hello World' }
});
element.dispatchEvent(event);
```

## 🔧 **Usage Examples:**
```javascript
// Mouse events
element.addEventListener('click', handler);
element.addEventListener('mouseover', handler);

// Keyboard events
document.addEventListener('keydown', handler);

// Form events
form.addEventListener('submit', handler);
input.addEventListener('input', handler);

// Touch events
element.addEventListener('touchstart', handler);

// Media events
video.addEventListener('play', handler);

// Custom events
element.addEventListener('myCustomEvent', handler);
```

## ⚠️ **Important Notes:**
- Some events are **deprecated** (like `keypress`)
- Some events don't **bubble** (like `focus`, `blur`)
- Use `focusin`/`focusout` for bubbling focus events
- Mobile devices may have different behavior
- Always check **browser compatibility**

This is the most comprehensive list of JavaScript event listeners! Each serves specific purposes for creating interactive web applications.