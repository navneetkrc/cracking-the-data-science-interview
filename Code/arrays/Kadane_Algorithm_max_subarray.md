
---

# 53. Maximum Subarray

## Problem Description

> Given an integer array `nums`, find the **subarray** with the largest sum, and return *its sum*.
>
> A subarray is a contiguous part of an array.

---

## Examples

**Example 1:**

*   **Input:** `nums = [-2,1,-3,4,-1,2,1,-5,4]`
*   **Output:** `6`
*   **Explanation:** The subarray `[4,-1,2,1]` has the largest sum `6`.

**Example 2:**

*   **Input:** `nums = [1]`
*   **Output:** `1`
*   **Explanation:** The subarray `[1]` has the largest sum `1`.

**Example 3:**

*   **Input:** `nums = [5,4,-1,7,8]`
*   **Output:** `23`
*   **Explanation:** The subarray `[5,4,-1,7,8]` has the largest sum `23`.

---

## Constraints

*   `1 <= nums.length <= 10^5`
*   `-10^4 <= nums[i] <= 10^4`

---

## Follow Up

> If you have figured out the O(n) solution, try coding another solution using the **divide and conquer** approach, which is more subtle.

---

## Python Solution (Kadane's Algorithm)

```python
from typing import List

class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        # Edge case: If the list is empty, although constraints say nums.length >= 1.
        # If it could be empty, problem would need to specify return value (e.g., 0 or error).
        # Given constraints, nums[0] is safe.

        # --- Kadane's Algorithm Initialization ---
        
        # max_so_far: Stores the maximum sum found for any subarray encountered globally so far.
        # Initialize with the first element, as a single element is a valid subarray.
        max_so_far = nums[0]
        
        # current_max_ending_here: Stores the maximum sum of a subarray ending at the current element.
        # Initialize with the first element.
        current_max_ending_here = nums[0]

        # --- Iterate through the array starting from the second element ---
        # The first element is already accounted for in the initializations.
        for i in range(1, len(nums)):
            num = nums[i] # The current number being considered
            
            # --- Core Logic of Kadane's Algorithm ---
            # For the current number `num`, the maximum subarray sum ending at this position
            # can be one of two things:
            # 1. The number `num` itself (meaning we start a new subarray here).
            # 2. The number `num` added to `current_max_ending_here` from the previous step
            #    (meaning we extend the previous best subarray ending at i-1).
            # We choose the larger of these two.
            current_max_ending_here = max(num, current_max_ending_here + num)
            
            # After updating the maximum sum ending at the current position,
            # we check if this value is greater than our overall `max_so_far`.
            # If it is, we update `max_so_far`.
            max_so_far = max(max_so_far, current_max_ending_here)
            
        # After iterating through all numbers, max_so_far will hold the largest
        # sum of any contiguous subarray.
        return max_so_far

```

### Explanation of Kadane's Algorithm:

The algorithm iterates through the array, keeping track of two main variables:

1.  `current_max_ending_here`: The maximum sum of a subarray that *ends* at the current position `i`.
    *   To calculate this for the current element `nums[i]`, we have two choices:
        *   Start a new subarray with just `nums[i]`.
        *   Extend the best subarray ending at `nums[i-1]` by adding `nums[i]` to it.
    *   We take the maximum of these two: `current_max_ending_here = max(nums[i], current_max_ending_here + nums[i])`.

2.  `max_so_far`: The overall maximum subarray sum found anywhere in the array up to the current position.
    *   After calculating `current_max_ending_here` for the current position, we compare it with `max_so_far` and update `max_so_far` if `current_max_ending_here` is greater: `max_so_far = max(max_so_far, current_max_ending_here)`.

This algorithm efficiently solves the problem in O(n) time complexity and O(1) space complexity.


![image](https://github.com/user-attachments/assets/0695b62f-3885-440e-bfa6-fa47cea8738e)
