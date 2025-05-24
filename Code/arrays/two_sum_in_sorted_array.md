# Two Sum II - Sorted Array Algorithm Guide

## 🎯 Problem Statement

**LeetCode 167: Two Sum II - Input Array Is Sorted**

Given a **1-indexed** array of integers that is already sorted in **non-decreasing order**, find two numbers such that they add up to a specific target number.

- Return the indices as `[index1, index2]` where `1 <= index1 < index2`
- Indices should be **1-indexed** (add 1 to 0-based indices)
- Exactly one solution exists
- Cannot use the same element twice
- Must use only **constant extra space**

---

## 🧠 Algorithm Intuition

### The Two-Pointer Strategy

The key insight is leveraging the **sorted property** of the array. We use two pointers starting from opposite ends:

```
Array: [2, 7, 11, 15]  Target: 9
        ↑           ↑
      left        right
```

### Decision Logic

```python
current_sum = numbers[left] + numbers[right]

if current_sum == target:
    return [left + 1, right + 1]  # Found it! (1-indexed)
elif current_sum < target:
    left += 1                     # Need bigger sum, move left right
else:
    right -= 1                    # Need smaller sum, move right left
```

---

## 📋 Step-by-Step Walkthrough

Let's trace through the example: `numbers = [2, 7, 11, 15]`, `target = 9`

### Initial State
```
Index (0-based): [0, 1,  2,  3]
Values:          [2, 7, 11, 15]
1-indexed:       [1, 2,  3,  4]
                  ↑           ↑
                left        right
```

### Step 1: Check Sum
```
left = 0, right = 3
numbers[0] + numbers[3] = 2 + 15 = 17
17 > 9 (target) → Sum too large, move right pointer left
```

### Step 2: Move Right Pointer
```
Index (0-based): [0, 1,  2,  3]
Values:          [2, 7, 11, 15]
                  ↑      ↑
                left   right
```

### Step 3: Check Sum Again
```
left = 0, right = 2
numbers[0] + numbers[2] = 2 + 11 = 13
13 > 9 (target) → Sum still too large, move right pointer left
```

### Step 4: Move Right Pointer Again
```
Index (0-based): [0, 1,  2,  3]
Values:          [2, 7, 11, 15]
                  ↑  ↑
                left right
```

### Step 5: Found Solution!
```
left = 0, right = 1
numbers[0] + numbers[1] = 2 + 7 = 9
9 == 9 (target) → Found it!
Return [1, 2] (1-indexed)
```

---

## 💻 Complete Solution

```python
from typing import List

class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            
            if current_sum == target:
                return [left + 1, right + 1]  # Convert to 1-indexed
            elif current_sum < target:
                left += 1                     # Need bigger sum
            else:
                right -= 1                    # Need smaller sum
        
        return []  # No solution found (shouldn't happen per problem constraints)
```

---

## 🎪 Visual Animation Concept

*Since markdown can't show true animations, here's how the algorithm would look animated:*

**Frame 1:** Initial Setup
```
[2,  7, 11, 15]    Target: 9
 ↑            ↑     Sum: 2 + 15 = 17 > 9
LEFT        RIGHT   → Move RIGHT left
```

**Frame 2:** First Move
```
[2,  7, 11, 15]    Target: 9
 ↑       ↑          Sum: 2 + 11 = 13 > 9
LEFT   RIGHT        → Move RIGHT left
```

**Frame 3:** Second Move
```
[2,  7, 11, 15]    Target: 9
 ↑   ↑              Sum: 2 + 7 = 9 = 9
LEFT RIGHT          → FOUND! Return [1, 2]
```

---

## 📊 Complexity Analysis

| Metric | Complexity | Explanation |
|--------|------------|-------------|
| **Time** | `O(n)` | Each element visited at most once |
| **Space** | `O(1)` | Only using two pointer variables |

### Comparison with Other Approaches

| Approach | Time | Space | Notes |
|----------|------|-------|-------|
| **Brute Force** | `O(n²)` | `O(1)` | Check all pairs |
| **Hash Map** | `O(n)` | `O(n)` | Trade space for time |
| **Two Pointers** | `O(n)` | `O(1)` | ✅ Optimal for sorted arrays |

---

## 💡 Key Insights

### Why Two Pointers Work Here

1. **Sorted Property**: We know the relationship between adjacent elements
2. **Extremes Strategy**: Starting from ends gives maximum control over sum
3. **Elimination**: Each step eliminates multiple possibilities at once
4. **Guaranteed Convergence**: Pointers will meet if solution exists

### The Magic of Elimination

When we move a pointer, we're not just trying one new combination - we're eliminating entire groups of combinations:

```
If left=0, right=5 and sum is too large:
- We eliminate ALL pairs involving right=5
- (0,5), (1,5), (2,5), (3,5), (4,5) are all too large
- Move right to 4 and continue
```

### Why Not Hash Map Here?

While a hash map approach works (`O(n)` time, `O(n)` space`), the two-pointer technique:
- Uses the sorted property that hash maps can't leverage
- Achieves the same time complexity with better space complexity
- Is more intuitive once understood
- Demonstrates algorithmic elegance

---

## 🔧 Practice Variations

Try these examples to solidify your understanding:

### Example 1
```
Input: numbers = [2,3,4], target = 6
Output: [1,3]
Explanation: 2 + 4 = 6, indices are 1 and 3 (1-indexed)
```

### Example 2
```
Input: numbers = [-1,0], target = -1
Output: [1,2]
Explanation: -1 + 0 = -1, indices are 1 and 2 (1-indexed)
```

### Example 3
```
Input: numbers = [5,25,75], target = 100
Output: [2,3]
Explanation: 25 + 75 = 100, indices are 2 and 3 (1-indexed)
```

---

## 🚀 Mental Model

Think of the two pointers as:
- **Left pointer**: "Give me something bigger!"
- **Right pointer**: "Give me something smaller!"
- **The dance**: They move toward each other, adjusting the sum until they meet at the perfect combination

The beauty is in the **intelligent movement** - each step is purposeful and eliminates possibilities, leading us efficiently to the solution.

---

## 📝 Common Pitfalls

1. **Index Confusion**: Remember to return 1-indexed results (`left + 1, right + 1`)
2. **Boundary Conditions**: Ensure `left < right` in the while loop
3. **Sorted Assumption**: This technique only works because the array is sorted
4. **Single Solution**: The problem guarantees exactly one solution exists

---

## 🎯 Takeaway

The Two Sum II algorithm beautifully demonstrates how **leveraging data structure properties** (sorted array) can lead to elegant and efficient solutions. The two-pointer technique transforms a potentially quadratic problem into a linear one while using minimal space.

**Remember**: When you see a sorted array and need to find pairs/combinations, think two pointers!
