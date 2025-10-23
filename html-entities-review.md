No, `&;copy` is **NOT correct**. You're missing the `#` symbol and have an extra semicolon.

## ✅ **Correct HTML Entities:**

```html
&copy;    <!-- Copyright symbol: © -->
&reg;     <!-- Registered trademark: ® -->
&trade;   <!-- Trademark symbol: ™ -->
```

## 📚 **Complete Guide to HTML Entities**

### **What are HTML Entities?**
Special codes that represent characters that are:
- Hard to type on keyboard
- Reserved in HTML (`<`, `>`, `&`)
- Special symbols

### **Syntax:**
```html
&entity_name;    <!-- Named entity -->
&#entity_number; <!-- Numeric entity -->
```

## 📋 **Complete List of Common HTML Entities**

### **Copyright & Legal Symbols:**
```html
&copy;   <!-- © Copyright -->
&reg;    <!-- ® Registered trademark -->
&trade;  <!-- ™ Trademark -->
&sect;   <!-- § Section -->
&para;   <!-- ¶ Paragraph -->
```

### **Currency Symbols:**
```html
&dollar; <!-- $ Dollar -->
&euro;   <!-- € Euro -->
&pound;  <!-- £ Pound -->
&yen;    <!-- ¥ Yen -->
&cent;   <!-- ¢ Cent -->
```

### **Mathematical Symbols:**
```html
&plus;   <!-- + Plus -->
&minus;  <!-- − Minus -->
×   <!-- × Multiplication -->
&divide; <!-- ÷ Division -->
&equals; <!-- = Equals -->
&ne;     <!-- ≠ Not equal -->
&lt;     <!-- < Less than -->
&gt;     <!-- > Greater than -->
&le;     <!-- ≤ Less than or equal -->
&ge;     <!-- ≥ Greater than or equal -->
&infin;  <!-- ∞ Infinity -->
&pi;     <!-- π Pi -->
&sum;    <!-- ∑ Summation -->
&int;    <!-- ∫ Integral -->
```

### **Greek Letters:**
```html
&alpha;   <!-- α Alpha -->
&beta;    <!-- β Beta -->
&gamma;   <!-- γ Gamma -->
&delta;   <!-- δ Delta -->
&epsilon; <!-- ε Epsilon -->
&theta;   <!-- θ Theta -->
&pi;      <!-- π Pi -->
&sigma;   <!-- σ Sigma -->
&omega;   <!-- ω Omega -->
```

### **Arrows:**
```html
&larr;  <!-- ← Left arrow -->
&rarr;  <!-- → Right arrow -->
&uarr;  <!-- ↑ Up arrow -->
&darr;  <!-- ↓ Down arrow -->
&harr;  <!-- ↔ Left-right arrow -->
&crarr; <!-- ↵ Carriage return -->
```

### **Punctuation & Special Characters:**
```html
&nbsp;   <!--   Non-breaking space -->
&ensp;   <!--   En space -->
&emsp;   <!--   Em space -->
&quot;   <!-- " Double quote -->
&apos;   <!-- ' Apostrophe -->
&amp;    <!-- & Ampersand -->
&lt;     <!-- < Less than -->
&gt;     <!-- > Greater than -->
&hellip; <!-- … Horizontal ellipsis -->
&bull;   <!-- • Bullet -->
&ndash;  <!-- – En dash -->
&mdash;  <!-- — Em dash -->
```

### **Accented Characters:**
```html
&aacute; <!-- á a with acute -->
&agrave; <!-- à a with grave -->
&acirc;  <!-- â a with circumflex -->
&auml;   <!-- ä a with diaeresis -->
&eacute; <!-- é e with acute -->
&egrave; <!-- è e with grave -->
&ecirc;  <!-- ê e with circumflex -->
&euml;   <!-- ë e with diaeresis -->
&iuml;   <!-- ï i with diaeresis -->
&ouml;   <!-- ö o with diaeresis -->
&uuml;   <!-- ü u with diaeresis -->
&ntilde; <!-- ñ n with tilde -->
&ccedil; <!-- ç c with cedilla -->
```

## 💻 **Usage in Your Quiz App:**

### **Footer with Copyright:**
```html
<footer>
    <p>&copy; 2024 QuizMaster Pro. All rights reserved.</p>
    <p>QuizMaster&trade; is a registered trademark.</p>
</footer>
```

### **Mathematical Questions:**
```html
<div class="question">
    <p>Solve for x: x &lt; 5 &amp; x &gt; 2</p>
    <p>Calculate the area: A = &pi;r&sup2;</p>
    <p>Limit: lim<sub>x&rarr;&infin;</sub> f(x)</p>
</div>
```

### **Multiple Choice Options:**
```html
<div class="options">
    <div class="option">&alpha; &rarr; &beta; decay</div>
    <div class="option">&gamma; radiation</div>
    <div class="option">&delta; rays</div>
    <div class="option">&epsilon; particles</div>
</div>
```

### **Currency Questions:**
```html
<div class="question">
    <p>Convert &euro;100 to &dollar;USD</p>
    <p>Price: &pound;50 &plus; &pound;10 tax</p>
</div>
```

## 🔧 **JavaScript Usage:**

### **Creating Entities in JavaScript:**
```javascript
// Method 1: Using innerHTML
element.innerHTML = 'Copyright &copy; 2024';

// Method 2: Using textContent (shows raw entity)
element.textContent = 'Copyright &copy; 2024'; // Shows "&copy;" literally

// Method 3: Using Unicode
element.textContent = 'Copyright \u00A9 2024'; // © symbol

// Method 4: Creating element with entity
const footer = document.createElement('footer');
footer.innerHTML = `&copy; ${new Date().getFullYear()} Quiz App`;
```

### **Utility Function for Entities:**
```javascript
const HTML Entities = {
    COPYRIGHT: '&copy;',
    TRADEMARK: '&trade;',
    REGISTERED: '&reg;',
    EURO: '&euro;',
    POUND: '&pound;',
    ARROW_LEFT: '&larr;',
    ARROW_RIGHT: '&rarr;',
    INFINITY: '&infin;',
    PI: '&pi;'
};

function createCopyrightText() {
    return `${HTML Entities.COPYRIGHT} ${new Date().getFullYear()} QuizMaster`;
}

// Usage
document.getElementById('footer').innerHTML = createCopyrightText();
```

## 🎯 **Common Mistakes & Corrections:**

### **Incorrect:**
```html
&;copy          <!-- Wrong: extra semicolon, missing # -->
&copy           <!-- Wrong: missing semicolon -->
&Copy;          <!-- Wrong: wrong case -->
copy;           <!-- Wrong: missing & -->
```

### **Correct:**
```html
&copy;          <!-- Correct copyright symbol -->
&#169;          <!-- Correct numeric entity -->
&#xA9;          <!-- Correct hexadecimal -->
```

## 📖 **Numeric Entities Reference:**

```html
<!-- Copyright symbols -->
&copy;    <!-- Named entity -->
&#169;    <!-- Decimal -->
&#x00A9;  <!-- Hexadecimal -->

<!-- Common numeric entities -->
&#32;     <!-- Space -->
&#33;     <!-- ! -->
&#34;     <!-- " -->
&#35;     <!-- # -->
&#36;     <!-- $ -->
&#37;     <!-- % -->
&#38;     <!-- & -->
&#39;     <!-- ' -->
&#40;     <!-- ( -->
&#41;     <!-- ) -->
&#42;     <!-- * -->
&#43;     <!-- + -->
&#44;     <!-- , -->
&#45;     <!-- - -->
&#46;     <!-- . -->
&#47;     <!-- / -->
```

## 🌐 **Browser Support:**
- ✅ All modern browsers support HTML entities
- ✅ Mobile browsers
- ✅ Screen readers understand them
- ✅ Search engines properly interpret them

## 🚀 **Best Practices:**

1. **Use named entities** when available (more readable)
2. **Always include the semicolon** - `&copy;` not `&copy`
3. **Use for special characters**, not for styling
4. **Test across browsers** for consistency
5. **Consider accessibility** - screen readers handle entities well

So the correct copyright symbol is **`&copy;`** which produces: ©