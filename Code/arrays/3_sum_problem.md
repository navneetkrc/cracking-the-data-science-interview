
---

# LeetCode 15: 3Sum

## Problem Statement

> Given an integer array `nums`, return all the triplets `[nums[i], nums[j], nums[k]]` such that:
>
> 1.  `i != j`, `i != k`, and `j != k` (the indices must be distinct).
> 2.  `nums[i] + nums[j] + nums[k] == 0` (the sum of the three elements is zero).
>
> Notice that the **solution set must not contain duplicate triplets**.

---

## Examples

**Example 1:**

*   **Input:** `nums = [-1,0,1,2,-1,-4]`
*   **Output:** `[[-1,-1,2],[-1,0,1]]`
*   **Explanation:**
    *   `nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`
    *   `nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.`
    *   `nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`
    *   The distinct triplets are `[-1,0,1]` and `[-1,-1,2]`.
    *   The order of the output and the order of the triplets does not matter.

**Example 2:**

*   **Input:** `nums = [0,1,1]`
*   **Output:** `[]`
*   **Explanation:** The only possible triplet does not sum up to 0.

**Example 3:**

*   **Input:** `nums = [0,0,0]`
*   **Output:** `[[0,0,0]]`
*   **Explanation:** The only possible triplet sums up to 0.

---

## Constraints

*   `3 <= nums.length <= 3000`
*   `-10^5 <= nums[i] <= 10^5`

---

## Python Solution (Sort + Two-Pointer Approach)

```python
from typing import List

class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        # Step 1: Sort the input array.
        # Sorting allows us to efficiently use the two-pointer technique
        # and easily handle duplicate triplets.
        nums.sort()
        
        result = [] # List to store the found triplets
        n = len(nums)
        
        # Step 2: Iterate through the array, fixing the first element of the potential triplet.
        # We only need to iterate up to n-2 because we need at least two more elements
        # for the triplet (nums[left] and nums[right]).
        for i in range(n - 2):
            # Optimization: If the current fixed element (nums[i]) is positive,
            # and since the array is sorted, any subsequent elements (nums[left], nums[right])
            # will also be non-negative. Thus, their sum cannot be 0. We can break early.
            if nums[i] > 0:
                break

            # Skip duplicate values for the first element (nums[i]).
            # If the current element is the same as the previous one,
            # it would lead to duplicate triplets, so we continue to the next iteration.
            # This check is valid only if i > 0 (not for the very first element).
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            # Step 3: Use the Two-Pointer technique for the remaining part of the array.
            # 'left' pointer starts right after the fixed element 'i'.
            # 'right' pointer starts at the end of the array.
            left, right = i + 1, n - 1
            
            while left < right:
                current_sum = nums[i] + nums[left] + nums[right]
                
                if current_sum == 0:
                    # Found a triplet that sums to zero.
                    result.append([nums[i], nums[left], nums[right]])
                    
                    # Move pointers and skip duplicates for the second and third elements.
                    left += 1
                    right -= 1
                    
                    # Skip duplicate values for the second element (nums[left]).
                    # Ensure left is still less than right.
                    while left < right and nums[left] == nums[left - 1]:
                        left += 1
                    
                    # Skip duplicate values for the third element (nums[right]).
                    # Ensure left is still less than right.
                    while left < right and nums[right] == nums[right + 1]:
                        right -= 1
                        
                elif current_sum < 0:
                    # The sum is too small. We need to increase it.
                    # Move the 'left' pointer to the right to consider a larger number.
                    left += 1
                else: # current_sum > 0
                    # The sum is too large. We need to decrease it.
                    # Move the 'right' pointer to the left to consider a smaller number.
                    right -= 1
                    
        return result
```

---

## Key Points & Algorithm Explanation

This problem is a variation of the "Two Sum" problem, extended to three numbers. The common and efficient approach involves sorting the array first, then using a two-pointer technique.

1.  **Sort the Array:**
    *   Sorting `nums` is crucial. It allows:
        *   Efficiently finding pairs that sum to a target using two pointers.
        *   Easily skipping duplicate elements to ensure unique triplets in the result.
    *   Time complexity for sorting is O(N log N).

2.  **Iterate and Fix First Element:**
    *   Loop through the sorted array with an index `i`, considering `nums[i]` as the first element of a potential triplet.
    *   **Optimization:** If `nums[i]` is positive, then since the array is sorted, `nums[left]` and `nums[right]` (where `left > i`, `right > i`) will also be non-negative. Their sum `nums[i] + nums[left] + nums[right]` cannot be `0`. So, we can break the outer loop early.
    *   **Skip Duplicates for `nums[i]`:** If `i > 0` and `nums[i] == nums[i-1]`, it means we have already processed `nums[i-1]` as the first element. Using `nums[i]` again would lead to duplicate triplets. So, skip the current `nums[i]`.

3.  **Two-Pointer Approach for Remaining Elements:**
    *   For each `nums[i]`, we need to find two other numbers `nums[left]` and `nums[right]` in the rest of the array (`nums[i+1 ... n-1]`) such that `nums[left] + nums[right] == -nums[i]`.
    *   Initialize `left = i + 1` and `right = n - 1`.
    *   While `left < right`:
        *   Calculate `current_sum = nums[i] + nums[left] + nums[right]`.
        *   **If `current_sum == 0`:**
            *   A valid triplet is found: `[nums[i], nums[left], nums[right]]`. Add it to the `result`.
            *   To find other potential triplets with the same `nums[i]`, move both `left` and `right` pointers inward.
            *   **Skip Duplicates for `nums[left]` and `nums[right]`:** After finding a valid triplet and moving pointers, if `nums[left]` is the same as `nums[left-1]`, increment `left` until a new unique value is found (or `left` meets `right`). Similarly for `nums[right]` with `nums[right+1]`. This prevents adding duplicate triplets like `[-1, 0, 1]` multiple times if `0` or `1` appears more than once consecutively.
        *   **If `current_sum < 0`:**
            *   The sum is too small. Increment `left` to try a larger second number.
        *   **If `current_sum > 0`:**
            *   The sum is too large. Decrement `right` to try a smaller third number.

4.  **Result:**
    *   The `result` list will contain all unique triplets that sum to zero.

---

## Complexity Analysis

*   **Time Complexity:** O(N<sup>2</sup>)
    *   Sorting the array takes O(N log N).
    *   The main part involves a nested loop structure:
        *   The outer loop iterates `N` times (for `i`).
        *   The inner `while` loop (two-pointer approach) takes O(N) in the worst case for each `i`.
    *   So, the nested part is O(N<sup>2</sup>).
    *   Overall complexity is dominated by O(N<sup>2</sup>), i.e., O(N log N + N<sup>2</sup>) = O(N<sup>2</sup>).

*   **Space Complexity:** O(log N) or O(N) (depending on sorting algorithm)
    *   The space used for sorting depends on the implementation of the sort function. In Python, Timsort uses O(N) space in the worst case, and O(log N) on average.
    *   Excluding the space for sorting, the algorithm uses O(1) extra space for pointers and variables if we don't count the `result` list. If the `result` list is counted, its space depends on the number of triplets found. The problem usually asks for space complexity besides the output storage.

---

## Important Considerations

*   **Sorting is Key:** The efficiency and duplicate handling rely heavily on the initial sorting step.
*   **Duplicate Triplet Handling:** The checks `if i > 0 and nums[i] == nums[i-1]: continue` and the similar checks for `left` and `right` pointers after finding a triplet are crucial to ensure the uniqueness of triplets in the output.
*   **Distinct Indices Requirement:** The problem states `i != j, i != k, and j != k`. The two-pointer setup (`left = i + 1`) inherently ensures `i < left < right`, so the indices are always distinct.
