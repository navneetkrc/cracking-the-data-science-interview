# 3Sum Algorithm Interactive Visualization

Find all unique triplets that sum to zero using the two-pointer technique.

## HTML Code

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>3Sum Algorithm Visualization</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            margin: 0;
            padding: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            color: #333;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 20px;
            padding: 30px;
            box-shadow: 0 20px 40px rgba(0,0,0,0.1);
        }
        
        h1 {
            text-align: center;
            color: #2c3e50;
            margin-bottom: 10px;
            font-size: 2.5em;
        }
        
        .subtitle {
            text-align: center;
            color: #7f8c8d;
            margin-bottom: 30px;
            font-size: 1.2em;
        }
        
        .step-section {
            margin: 30px 0;
            padding: 25px;
            border-radius: 15px;
            background: #f8f9fa;
            border-left: 5px solid #3498db;
        }
        
        .step-title {
            font-size: 1.4em;
            font-weight: bold;
            color: #2c3e50;
            margin-bottom: 15px;
        }
        
        .array-container {
            display: flex;
            justify-content: center;
            margin: 20px 0;
            flex-wrap: wrap;
            gap: 5px;
        }
        
        .array-element {
            width: 50px;
            height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            border: 2px solid #bdc3c7;
            border-radius: 8px;
            font-weight: bold;
            font-size: 16px;
            background: white;
            transition: all 0.3s ease;
            position: relative;
        }
        
        .array-element.fixed {
            background: #e74c3c;
            color: white;
            border-color: #c0392b;
            transform: scale(1.1);
        }
        
        .array-element.left-pointer {
            background: #3498db;
            color: white;
            border-color: #2980b9;
            transform: scale(1.1);
        }
        
        .array-element.right-pointer {
            background: #2ecc71;
            color: white;
            border-color: #27ae60;
            transform: scale(1.1);
        }
        
        .array-element.duplicate {
            background: #95a5a6;
            color: white;
            opacity: 0.6;
        }
        
        .pointer-label {
            position: absolute;
            top: -25px;
            font-size: 12px;
            font-weight: bold;
            color: #2c3e50;
        }
        
        .controls {
            text-align: center;
            margin: 30px 0;
        }
        
        button {
            background: #3498db;
            color: white;
            border: none;
            padding: 12px 24px;
            margin: 0 10px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
        }
        
        button:hover {
            background: #2980b9;
            transform: translateY(-2px);
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
        }
        
        button:disabled {
            background: #bdc3c7;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .input-section {
            text-align: center;
            margin: 20px 0;
        }
        
        input {
            padding: 10px;
            border: 2px solid #bdc3c7;
            border-radius: 8px;
            margin: 0 10px;
            font-size: 16px;
            width: 200px;
        }
        
        .results {
            background: #ecf0f1;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }
        
        .triplet {
            display: inline-block;
            background: #27ae60;
            color: white;
            padding: 8px 15px;
            margin: 5px;
            border-radius: 20px;
            font-weight: bold;
        }
        
        .status {
            text-align: center;
            padding: 15px;
            margin: 10px 0;
            border-radius: 10px;
            font-weight: bold;
        }
        
        .status.found {
            background: #d5f4e6;
            color: #27ae60;
            border: 2px solid #27ae60;
        }
        
        .status.searching {
            background: #fef9e7;
            color: #f39c12;
            border: 2px solid #f39c12;
        }
        
        .status.skip {
            background: #fadbd8;
            color: #e74c3c;
            border: 2px solid #e74c3c;
        }
        
        .explanation {
            background: #fff3cd;
            border: 1px solid #ffeaa7;
            border-radius: 10px;
            padding: 15px;
            margin: 15px 0;
        }
        
        .sum-display {
            text-align: center;
            font-size: 1.2em;
            margin: 15px 0;
            padding: 10px;
            background: #e8f4f8;
            border-radius: 8px;
            border-left: 4px solid #3498db;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>3Sum Algorithm Visualization</h1>
        <p class="subtitle">Find all unique triplets that sum to zero</p>
        
        <div class="input-section">
            <input type="text" id="arrayInput" placeholder="Enter numbers (e.g., -1,0,1,2,-1,-4)" value="-1,0,1,2,-1,-4">
            <button onclick="initializeArray()">Set Array</button>
        </div>
        
        <div class="step-section">
            <div class="step-title">Step 1: Sort the Array</div>
            <p>First, we sort the array to enable the two-pointer technique and handle duplicates efficiently.</p>
            <div id="originalArray" class="array-container"></div>
            <div style="text-align: center; margin: 10px 0;">↓</div>
            <div id="sortedArray" class="array-container"></div>
        </div>
        
        <div class="step-section">
            <div class="step-title">Step 2: Fix First Element & Use Two Pointers</div>
            <p>We iterate through the array, fixing the first element (red), then use two pointers (blue=left, green=right) to find pairs that complete the triplet.</p>
            <div id="workingArray" class="array-container"></div>
            <div id="sumDisplay" class="sum-display"></div>
            <div id="status" class="status"></div>
        </div>
        
        <div class="controls">
            <button onclick="resetDemo()">Reset</button>
            <button onclick="stepForward()" id="stepBtn">Next Step</button>
            <button onclick="autoPlay()" id="autoBtn">Auto Play</button>
        </div>
        
        <div class="results">
            <h3>Found Triplets:</h3>
            <div id="triplets"></div>
        </div>
        
        <div class="explanation">
            <h3>Algorithm Intuition:</h3>
            <p><strong>Why sorting helps:</strong> After sorting, we can use two pointers moving towards each other. If the sum is too small, move left pointer right (increase sum). If too large, move right pointer left (decrease sum).</p>
            <p><strong>Duplicate handling:</strong> Skip duplicate values to avoid duplicate triplets in the result.</p>
            <p><strong>Early termination:</strong> If the fixed element is positive, all remaining sums will be positive (can't sum to 0).</p>
            <p><strong>Time Complexity:</strong> O(n²) - O(n log n) for sorting + O(n²) for the nested loops.</p>
        </div>
    </div>

    <script>
        let nums = [];
        let originalNums = [];
        let currentI = 0;
        let currentLeft = 1;
        let currentRight = 0;
        let triplets = [];
        let isRunning = false;
        let stepCount = 0;
        
        function initializeArray() {
            const input = document.getElementById('arrayInput').value;
            originalNums = input.split(',').map(n => parseInt(n.trim())).filter(n => !isNaN(n));
            nums = [...originalNums];
            resetDemo();
            displayArrays();
        }
        
        function displayArrays() {
            // Original array
            const originalContainer = document.getElementById('originalArray');
            originalContainer.innerHTML = '';
            originalNums.forEach(num => {
                const div = document.createElement('div');
                div.className = 'array-element';
                div.textContent = num;
                originalContainer.appendChild(div);
            });
            
            // Sorted array
            nums.sort((a, b) => a - b);
            const sortedContainer = document.getElementById('sortedArray');
            sortedContainer.innerHTML = '';
            nums.forEach(num => {
                const div = document.createElement('div');
                div.className = 'array-element';
                div.textContent = num;
                sortedContainer.appendChild(div);
            });
            
            updateWorkingArray();
        }
        
        function updateWorkingArray() {
            const container = document.getElementById('workingArray');
            container.innerHTML = '';
            
            nums.forEach((num, index) => {
                const div = document.createElement('div');
                div.className = 'array-element';
                div.textContent = num;
                
                if (index === currentI) {
                    div.classList.add('fixed');
                    const label = document.createElement('div');
                    label.className = 'pointer-label';
                    label.textContent = 'Fixed (i)';
                    div.appendChild(label);
                } else if (index === currentLeft) {
                    div.classList.add('left-pointer');
                    const label = document.createElement('div');
                    label.className = 'pointer-label';
                    label.textContent = 'Left';
                    div.appendChild(label);
                } else if (index === currentRight) {
                    div.classList.add('right-pointer');
                    const label = document.createElement('div');
                    label.className = 'pointer-label';
                    label.textContent = 'Right';
                    div.appendChild(label);
                }
                
                container.appendChild(div);
            });
            
            updateSumDisplay();
        }
        
        function updateSumDisplay() {
            if (currentI < nums.length && currentLeft < nums.length && currentRight < nums.length) {
                const sum = nums[currentI] + nums[currentLeft] + nums[currentRight];
                document.getElementById('sumDisplay').innerHTML = 
                    `Current sum: ${nums[currentI]} + ${nums[currentLeft]} + ${nums[currentRight]} = ${sum}`;
            }
        }
        
        function updateStatus(message, type = 'searching') {
            const status = document.getElementById('status');
            status.textContent = message;
            status.className = `status ${type}`;
        }
        
        function displayTriplets() {
            const container = document.getElementById('triplets');
            container.innerHTML = '';
            triplets.forEach(triplet => {
                const div = document.createElement('div');
                div.className = 'triplet';
                div.textContent = `[${triplet.join(', ')}]`;
                container.appendChild(div);
            });
        }
        
        function resetDemo() {
            currentI = 0;
            currentLeft = 1;
            currentRight = nums.length - 1;
            triplets = [];
            stepCount = 0;
            isRunning = false;
            updateWorkingArray();
            displayTriplets();
            updateStatus('Ready to start');
            document.getElementById('stepBtn').disabled = false;
            document.getElementById('autoBtn').textContent = 'Auto Play';
        }
        
        function stepForward() {
            if (currentI >= nums.length - 2) {
                updateStatus('Algorithm completed!', 'found');
                document.getElementById('stepBtn').disabled = true;
                return;
            }
            
            // Early termination optimization
            if (nums[currentI] > 0) {
                updateStatus('Early termination: fixed element is positive, remaining sums cannot be 0', 'skip');
                document.getElementById('stepBtn').disabled = true;
                return;
            }
            
            // Skip duplicates for fixed element
            if (currentI > 0 && nums[currentI] === nums[currentI - 1]) {
                updateStatus(`Skipping duplicate fixed element: ${nums[currentI]}`, 'skip');
                currentI++;
                currentLeft = currentI + 1;
                currentRight = nums.length - 1;
                updateWorkingArray();
                return;
            }
            
            const currentSum = nums[currentI] + nums[currentLeft] + nums[currentRight];
            
            if (currentSum === 0) {
                triplets.push([nums[currentI], nums[currentLeft], nums[currentRight]]);
                updateStatus(`Found triplet: [${nums[currentI]}, ${nums[currentLeft]}, ${nums[currentRight]}]`, 'found');
                displayTriplets();
                
                // Move pointers and skip duplicates
                currentLeft++;
                currentRight--;
                
                while (currentLeft < currentRight && nums[currentLeft] === nums[currentLeft - 1]) {
                    currentLeft++;
                }
                while (currentLeft < currentRight && nums[currentRight] === nums[currentRight + 1]) {
                    currentRight--;
                }
            } else if (currentSum < 0) {
                updateStatus(`Sum ${currentSum} is too small, moving left pointer right`, 'searching');
                currentLeft++;
            } else {
                updateStatus(`Sum ${currentSum} is too large, moving right pointer left`, 'searching');
                currentRight--;
            }
            
            // Check if we need to move to next fixed element
            if (currentLeft >= currentRight) {
                currentI++;
                currentLeft = currentI + 1;
                currentRight = nums.length - 1;
            }
            
            updateWorkingArray();
        }
        
        function autoPlay() {
            if (isRunning) {
                isRunning = false;
                document.getElementById('autoBtn').textContent = 'Auto Play';
                return;
            }
            
            isRunning = true;
            document.getElementById('autoBtn').textContent = 'Pause';
            
            const interval = setInterval(() => {
                if (!isRunning || currentI >= nums.length - 2) {
                    clearInterval(interval);
                    isRunning = false;
                    document.getElementById('autoBtn').textContent = 'Auto Play';
                    return;
                }
                stepForward();
            }, 1500);
        }
        
        // Initialize with default array
        initializeArray();
    </script>
</body>
</html>
```

## Algorithm Explanation

### Problem Statement
Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that:
- `i != j`, `i != k`, and `j != k`
- `nums[i] + nums[j] + nums[k] == 0`

The solution set must not contain duplicate triplets.

### Algorithm Steps

1. **Sort the Array**
   - Enables two-pointer technique
   - Makes duplicate detection easier
   - Allows for early termination optimizations

2. **Fix First Element**
   - Iterate through array with index `i`
   - Skip duplicates to avoid duplicate triplets
   - Early exit if `nums[i] > 0` (remaining elements are positive)

3. **Two-Pointer Search**
   - Set `left = i + 1` and `right = n - 1`
   - Calculate `sum = nums[i] + nums[left] + nums[right]`
   - If `sum == 0`: Found triplet, add to result
   - If `sum < 0`: Move left pointer right (increase sum)
   - If `sum > 0`: Move right pointer left (decrease sum)

4. **Handle Duplicates**
   - Skip duplicate values for all three positions
   - Ensures unique triplets in result

### Key Insights

- **Why Sorting Works**: After sorting, we can make intelligent decisions about pointer movement based on the current sum
- **Two-Pointer Intuition**: If sum is too small, we need a larger number (move left right). If too large, we need a smaller number (move right left)
- **Duplicate Prevention**: Skip same values to avoid duplicate triplets
- **Time Complexity**: O(n²) overall - O(n log n) for sorting + O(n²) for the nested iteration

### Usage Instructions

1. **Input Array**: Enter comma-separated integers in the input field
2. **Step Through**: Use "Next Step" to manually walk through each iteration
3. **Auto Play**: Watch the algorithm run automatically with explanations
4. **Reset**: Start over with the same or different input

### Visual Elements

- 🔴 **Red**: Fixed element (i)
- 🔵 **Blue**: Left pointer
- 🟢 **Green**: Right pointer
- **Sum Display**: Shows current calculation
- **Status Messages**: Explains what's happening at each step
- **Triplets**: Shows all found solutions

This visualization helps understand the mechanical process of the algorithm and builds intuition for why the two-pointer technique works effectively for this problem.
