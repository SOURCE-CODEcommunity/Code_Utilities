I'll teach you how to create all these data visualization components with CSS and JavaScript control. Let's start with each one:

## **1. Edge Circle (Radial Progress/Donut Chart)**

### **HTML Structure:**
```html
<div class="chart-container">
    <div class="edge-circle" data-female="13" data-male="14">
        <div class="circle-progress"></div>
        <div class="circle-info">
            <span class="circle-value">27</span>
            <span class="circle-label">Total</span>
        </div>
    </div>
    <div class="legend">
        <div class="legend-item">
            <span class="color female"></span>
            <span>Female: 13</span>
        </div>
        <div class="legend-item">
            <span class="color male"></span>
            <span>Male: 14</span>
        </div>
    </div>
</div>
```

### **CSS Styles:**
```css
.edge-circle {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    position: relative;
    background: conic-gradient(
        #ff6b6b 0deg 173deg,    /* Female: (13/27)*360 = 173° */
        #4ecdc4 173deg 360deg   /* Male: (14/27)*360 = 187° */
    );
    display: flex;
    align-items: center;
    justify-content: center;
}

.edge-circle::before {
    content: '';
    position: absolute;
    width: 160px;
    height: 160px;
    background: white;
    border-radius: 50%;
    z-index: 1;
}

.circle-info {
    position: relative;
    z-index: 2;
    text-align: center;
}

.circle-value {
    display: block;
    font-size: 24px;
    font-weight: bold;
}

.circle-label {
    font-size: 14px;
    color: #666;
}

.legend {
    margin-top: 20px;
}

.legend-item {
    display: flex;
    align-items: center;
    margin: 5px 0;
}

.color {
    width: 20px;
    height: 20px;
    border-radius: 4px;
    margin-right: 10px;
}

.color.female { background: #ff6b6b; }
.color.male { background: #4ecdc4; }
```

### **JavaScript Control:**
```javascript
class EdgeCircle {
    constructor(container, data) {
        this.container = container;
        this.data = data;
        this.total = Object.values(data).reduce((sum, val) => sum + val, 0);
        this.init();
    }

    init() {
        this.createChart();
        this.update();
    }

    createChart() {
        let gradientStops = '';
        let currentAngle = 0;
        const colors = ['#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', '#feca57'];

        Object.entries(this.data).forEach(([key, value], index) => {
            const percentage = (value / this.total) * 100;
            const angle = (percentage / 100) * 360;
            const startAngle = currentAngle;
            const endAngle = currentAngle + angle;

            gradientStops += `${colors[index]} ${startAngle}deg ${endAngle}deg, `;
            currentAngle = endAngle;
        });

        gradientStops = gradientStops.slice(0, -2); // Remove last comma

        this.container.querySelector('.edge-circle').style.background = 
            `conic-gradient(${gradientStops})`;

        this.container.querySelector('.circle-value').textContent = this.total;
    }

    update(newData) {
        if (newData) {
            this.data = newData;
            this.total = Object.values(newData).reduce((sum, val) => sum + val, 0);
        }
        this.createChart();
        this.updateLegend();
    }

    updateLegend() {
        const legend = this.container.querySelector('.legend');
        legend.innerHTML = '';

        Object.entries(this.data).forEach(([key, value], index) => {
            const legendItem = document.createElement('div');
            legendItem.className = 'legend-item';
            legendItem.innerHTML = `
                <span class="color" style="background: ${this.getColor(index)}"></span>
                <span>${key}: ${value} (${Math.round((value/this.total)*100)}%)</span>
            `;
            legend.appendChild(legendItem);
        });
    }

    getColor(index) {
        const colors = ['#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', '#feca57'];
        return colors[index % colors.length];
    }
}

// Usage
const circleData = { Female: 13, Male: 14 };
const circle = new EdgeCircle(document.querySelector('.chart-container'), circleData);

// Update dynamically
circle.update({ Female: 20, Male: 15, Other: 5 });
```

## **2. Pie Chart**

### **HTML & CSS:**
```html
<div class="pie-chart-container">
    <div class="pie-chart" data-values="13,14">
        <div class="pie-segment" data-value="13" data-color="#ff6b6b"></div>
        <div class="pie-segment" data-value="14" data-color="#4ecdc4"></div>
    </div>
</div>
```

```css
.pie-chart {
    width: 200px;
    height: 200px;
    border-radius: 50%;
    position: relative;
    background: #f0f0f0;
    overflow: hidden;
}

.pie-segment {
    position: absolute;
    width: 100%;
    height: 100%;
    border-radius: 50%;
    clip-path: polygon(50% 50%, 50% 0%, 100% 0%, 100% 100%, 50% 50%);
    transform-origin: center;
    transition: transform 0.3s ease;
}

.pie-info {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    text-align: center;
    z-index: 2;
}
```

### **JavaScript:**
```javascript
class PieChart {
    constructor(container, data) {
        this.container = container;
        this.data = data;
        this.total = Object.values(data).reduce((sum, val) => sum + val, 0);
        this.init();
    }

    init() {
        this.createSegments();
    }

    createSegments() {
        const pieChart = this.container.querySelector('.pie-chart');
        pieChart.innerHTML = '<div class="pie-info"></div>';
        
        let currentAngle = 0;
        const colors = ['#ff6b6b', '#4ecdc4', '#45b7d1', '#96ceb4', '#feca57'];

        Object.entries(this.data).forEach(([key, value], index) => {
            const percentage = (value / this.total);
            const angle = percentage * 360;
            
            const segment = document.createElement('div');
            segment.className = 'pie-segment';
            segment.style.background = colors[index % colors.length];
            segment.style.transform = `rotate(${currentAngle}deg)`;
            segment.style.clipPath = this.createClipPath(angle);
            
            segment.addEventListener('mouseenter', () => this.showTooltip(key, value, percentage));
            pieChart.appendChild(segment);
            
            currentAngle += angle;
        });
    }

    createClipPath(angle) {
        if (angle <= 180) {
            return `polygon(50% 50%, 50% 0%, ${50 + 50 * Math.cos((angle * Math.PI) / 180)}% ${50 - 50 * Math.sin((angle * Math.PI) / 180)}%)`;
        } else {
            return `polygon(50% 50%, 50% 0%, 100% 0%, 100% 100%, 0% 100%, 0% 0%, ${50 + 50 * Math.cos((angle * Math.PI) / 180)}% ${50 - 50 * Math.sin((angle * Math.PI) / 180)}%)`;
        }
    }

    showTooltip(label, value, percentage) {
        // Implement tooltip logic
        console.log(`${label}: ${value} (${(percentage * 100).toFixed(1)}%)`);
    }
}
```

## **3. Histogram**

### **HTML & CSS:**
```html
<div class="histogram-container">
    <div class="histogram" data-values="10,25,45,30,15,5">
        <div class="histogram-bars"></div>
        <div class="histogram-labels"></div>
        <div class="histogram-axis"></div>
    </div>
</div>
```

```css
.histogram {
    display: flex;
    align-items: flex-end;
    height: 200px;
    gap: 10px;
    padding: 20px;
    border-bottom: 2px solid #333;
    border-left: 2px solid #333;
    position: relative;
}

.histogram-bar {
    background: linear-gradient(to top, #4ecdc4, #44a08d);
    border-radius: 4px 4px 0 0;
    transition: all 0.3s ease;
    flex: 1;
    position: relative;
    cursor: pointer;
}

.histogram-bar:hover {
    opacity: 0.8;
    transform: scaleY(1.05);
}

.histogram-label {
    text-align: center;
    margin-top: 5px;
    font-size: 12px;
}

.histogram-value {
    position: absolute;
    top: -25px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0,0,0,0.8);
    color: white;
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 12px;
    opacity: 0;
    transition: opacity 0.3s ease;
}

.histogram-bar:hover .histogram-value {
    opacity: 1;
}
```

### **JavaScript:**
```javascript
class Histogram {
    constructor(container, data, labels) {
        this.container = container;
        this.data = data;
        this.labels = labels;
        this.maxValue = Math.max(...data);
        this.init();
    }

    init() {
        this.createBars();
    }

    createBars() {
        const barsContainer = this.container.querySelector('.histogram-bars');
        const labelsContainer = this.container.querySelector('.histogram-labels');
        
        barsContainer.innerHTML = '';
        labelsContainer.innerHTML = '';

        this.data.forEach((value, index) => {
            const percentage = (value / this.maxValue) * 100;
            
            // Create bar
            const bar = document.createElement('div');
            bar.className = 'histogram-bar';
            bar.style.height = `${percentage}%`;
            
            const valueLabel = document.createElement('div');
            valueLabel.className = 'histogram-value';
            valueLabel.textContent = value;
            bar.appendChild(valueLabel);
            
            bar.addEventListener('mouseenter', () => this.highlightBar(bar));
            barsContainer.appendChild(bar);

            // Create label
            const label = document.createElement('div');
            label.className = 'histogram-label';
            label.textContent = this.labels[index] || `Item ${index + 1}`;
            labelsContainer.appendChild(label);
        });
    }

    highlightBar(bar) {
        document.querySelectorAll('.histogram-bar').forEach(b => {
            b.style.opacity = b === bar ? '1' : '0.6';
        });
    }

    update(newData, newLabels) {
        if (newData) {
            this.data = newData;
            this.maxValue = Math.max(...newData);
        }
        if (newLabels) {
            this.labels = newLabels;
        }
        this.createBars();
    }
}

// Usage
const histogramData = [10, 25, 45, 30, 15, 5];
const histogramLabels = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun'];
const histogram = new Histogram(
    document.querySelector('.histogram-container'),
    histogramData,
    histogramLabels
);
```

## **4. Bar Chart**

### **HTML & CSS:**
```html
<div class="barchart-container">
    <div class="barchart">
        <div class="bars"></div>
        <div class="labels"></div>
    </div>
</div>
```

```css
.barchart {
    display: flex;
    flex-direction: column;
    gap: 15px;
    padding: 20px;
}

.bar-item {
    display: flex;
    align-items: center;
    gap: 10px;
}

.bar-label {
    min-width: 80px;
    font-weight: bold;
}

.bar-track {
    flex: 1;
    height: 30px;
    background: #f0f0f0;
    border-radius: 15px;
    overflow: hidden;
    position: relative;
}

.bar-fill {
    height: 100%;
    background: linear-gradient(90deg, #ff6b6b, #ff8e8e);
    border-radius: 15px;
    transition: width 1s ease-in-out;
    position: relative;
}

.bar-value {
    position: absolute;
    right: 10px;
    top: 50%;
    transform: translateY(-50%);
    font-weight: bold;
    color: #333;
}
```

### **JavaScript:**
```javascript
class BarChart {
    constructor(container, data) {
        this.container = container;
        this.data = data;
        this.maxValue = Math.max(...Object.values(data));
        this.init();
    }

    init() {
        this.createBars();
    }

    createBars() {
        const barsContainer = this.container.querySelector('.bars');
        barsContainer.innerHTML = '';

        Object.entries(this.data).forEach(([label, value]) => {
            const percentage = (value / this.maxValue) * 100;
            
            const barItem = document.createElement('div');
            barItem.className = 'bar-item';
            barItem.innerHTML = `
                <div class="bar-label">${label}</div>
                <div class="bar-track">
                    <div class="bar-fill" style="width: 0%" data-value="${value}">
                        <span class="bar-value">${value}</span>
                    </div>
                </div>
            `;
            barsContainer.appendChild(barItem);
        });

        // Animate bars
        setTimeout(() => {
            document.querySelectorAll('.bar-fill').forEach(bar => {
                const value = bar.getAttribute('data-value');
                const percentage = (value / this.maxValue) * 100;
                bar.style.width = `${percentage}%`;
            });
        }, 100);
    }

    update(newData) {
        if (newData) {
            this.data = newData;
            this.maxValue = Math.max(...Object.values(newData));
        }
        this.createBars();
    }
}

// Usage
const barData = { 'Category A': 75, 'Category B': 45, 'Category C': 90 };
const barChart = new BarChart(document.querySelector('.barchart-container'), barData);
```

## **5. Progress Bar**

### **HTML & CSS:**
```html
<div class="progress-container">
    <div class="progress-bar">
        <div class="progress-fill" style="width: 65%">
            <span class="progress-text">65%</span>
        </div>
    </div>
    <div class="progress-controls">
        <button onclick="progressBar.setProgress(25)">25%</button>
        <button onclick="progressBar.setProgress(50)">50%</button>
        <button onclick="progressBar.setProgress(75)">75%</button>
        <button onclick="progressBar.setProgress(100)">100%</button>
    </div>
</div>
```

```css
.progress-bar {
    width: 100%;
    height: 30px;
    background: #f0f0f0;
    border-radius: 15px;
    overflow: hidden;
    position: relative;
    box-shadow: inset 0 2px 4px rgba(0,0,0,0.1);
}

.progress-fill {
    height: 100%;
    background: linear-gradient(90deg, #4ecdc4, #44a08d);
    border-radius: 15px;
    transition: width 0.5s ease-in-out;
    position: relative;
    display: flex;
    align-items: center;
    justify-content: center;
}

.progress-text {
    color: white;
    font-weight: bold;
    text-shadow: 1px 1px 2px rgba(0,0,0,0.5);
}

.progress-controls {
    margin-top: 15px;
    display: flex;
    gap: 10px;
}

.progress-controls button {
    padding: 5px 15px;
    border: none;
    background: #4ecdc4;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    transition: background 0.3s ease;
}

.progress-controls button:hover {
    background: #44a08d;
}
```

### **JavaScript:**
```javascript
class ProgressBar {
    constructor(container, initialProgress = 0) {
        this.container = container;
        this.progressFill = container.querySelector('.progress-fill');
        this.progressText = container.querySelector('.progress-text');
        this.setProgress(initialProgress);
    }

    setProgress(percentage) {
        const clampedPercentage = Math.max(0, Math.min(100, percentage));
        
        this.progressFill.style.width = `${clampedPercentage}%`;
        this.progressText.textContent = `${Math.round(clampedPercentage)}%`;
        
        // Change color based on progress
        if (clampedPercentage < 30) {
            this.progressFill.style.background = 'linear-gradient(90deg, #ff6b6b, #ff8e8e)';
        } else if (clampedPercentage < 70) {
            this.progressFill.style.background = 'linear-gradient(90deg, #feca57, #ff9ff3)';
        } else {
            this.progressFill.style.background = 'linear-gradient(90deg, #4ecdc4, #44a08d)';
        }
    }

    animateTo(targetPercentage, duration = 1000) {
        const startPercentage = parseInt(this.progressFill.style.width) || 0;
        const startTime = performance.now();

        const animate = (currentTime) => {
            const elapsed = currentTime - startTime;
            const progress = Math.min(elapsed / duration, 1);
            
            // Easing function
            const easeOutQuart = 1 - Math.pow(1 - progress, 4);
            const currentPercentage = startPercentage + (targetPercentage - startPercentage) * easeOutQuart;
            
            this.setProgress(currentPercentage);
            
            if (progress < 1) {
                requestAnimationFrame(animate);
            }
        };

        requestAnimationFrame(animate);
    }
}

// Usage
const progressBar = new ProgressBar(document.querySelector('.progress-container'), 65);

// Animate to 85% over 2 seconds
progressBar.animateTo(85, 2000);
```

## **Complete Integration Example:**

```html
<!DOCTYPE html>
<html>
<head>
    <style>
        /* All the CSS from above */
        .dashboard {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 30px;
            padding: 20px;
            max-width: 1200px;
            margin: 0 auto;
        }
        
        .chart-card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        
        .controls {
            margin-top: 20px;
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }
        
        .control-btn {
            padding: 8px 15px;
            border: none;
            background: #4ecdc4;
            color: white;
            border-radius: 5px;
            cursor: pointer;
            transition: background 0.3s ease;
        }
        
        .control-btn:hover {
            background: #44a08d;
        }
    </style>
</head>
<body>
    <div class="dashboard">
        <div class="chart-card">
            <h3>Gender Distribution</h3>
            <div class="edge-circle-container"></div>
            <div class="controls">
                <button class="control-btn" onclick="updateEdgeCircle()">Update Data</button>
            </div>
        </div>
        
        <div class="chart-card">
            <h3>Sales Progress</h3>
            <div class="progress-container">
                <div class="progress-bar">
                    <div class="progress-fill" style="width: 0%">
                        <span class="progress-text">0%</span>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="chart-card">
            <h3>Monthly Sales</h3>
            <div class="histogram-container">
                <div class="histogram">
                    <div class="histogram-bars"></div>
                    <div class="histogram-labels"></div>
                </div>
            </div>
        </div>
        
        <div class="chart-card">
            <h3>Performance</h3>
            <div class="barchart-container">
                <div class="barchart">
                    <div class="bars"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Initialize all charts
        const edgeCircle = new EdgeCircle(
            document.querySelector('.edge-circle-container'), 
            { Female: 13, Male: 14 }
        );

        const progressBar = new ProgressBar(
            document.querySelector('.progress-container'), 
            65
        );

        const histogram = new Histogram(
            document.querySelector('.histogram-container'),
            [10, 25, 45, 30, 15, 5],
            ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun']
        );

        const barChart = new BarChart(
            document.querySelector('.barchart-container'),
            { 'Q1': 75, 'Q2': 45, 'Q3': 90, 'Q4': 60 }
        );

        // Update functions
        function updateEdgeCircle() {
            const newData = {
                Female: Math.floor(Math.random() * 20) + 5,
                Male: Math.floor(Math.random() * 20) + 5,
                Other: Math.floor(Math.random() * 10) + 1
            };
            edgeCircle.update(newData);
        }

        // Animate progress bar
        setInterval(() => {
            const randomProgress = Math.floor(Math.random() * 100);
            progressBar.animateTo(randomProgress, 1000);
        }, 3000);
    </script>
</body>
</html>
```

These components are fully controllable via JavaScript and can be dynamically updated with new data!