
---

# LeetCode 31: Next Permutation

## Problem Statement

> A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.
>
> For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3]`, `[1,3,2]`, `[2,1,3]`, `[2,3,1]`, `[3,1,2]`, `[3,2,1]`.
>
> The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the next permutation of that array is the permutation that follows it in the sorted container.
>
> If such arrangement is not possible (i.e., the array is already in its largest permutation, like `[3,2,1]`), the array must be rearranged as the **lowest possible order** (i.e., sorted in ascending order).
>
> Given an array of integers `nums`, find the next permutation of `nums`.
>
> The replacement must be **in-place** and use only **constant extra memory**.

---

## Examples

1.  **Input:** `nums = [1,2,3]`
    **Output:** `[1,3,2]`

2.  **Input:** `nums = [3,2,1]`
    **Output:** `[1,2,3]` (Since `[3,2,1]` is the largest, next is the smallest)

3.  **Input:** `nums = [1,1,5]`
    **Output:** `[1,5,1]`

---

## Constraints

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

---

## Solution (Python)

```python
from typing import List

class Solution:
    def nextPermutation(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        n = len(nums)

        # Step 1: Find the largest index 'k' such that nums[k] < nums[k + 1].
        # Iterate from right to left (second to last element to the first).
        # This 'k' is the "pivot" or "break point" from the non-increasing suffix.
        k = -1
        for i in range(n - 2, -1, -1):
            if nums[i] < nums[i + 1]:
                k = i
                break
        
        # Case 1: If no such index 'k' exists (k == -1).
        # This means the entire array is in descending order (e.g., [3,2,1]).
        # It's the last possible permutation. The next permutation is the smallest one (sorted ascending).
        # So, we reverse the entire array.
        if k == -1:
            nums.reverse()
            return
        
        # Case 2: Such an index 'k' is found.
        # Step 2: Find the largest index 'l' > k such that nums[l] > nums[k].
        # Iterate from right to left starting from the end of the array.
        # We are looking for the smallest number in the suffix nums[k+1:] that is still greater than nums[k].
        l = n - 1
        while l > k and nums[l] <= nums[k]: # Note: <= to find the rightmost element that's strictly greater
            l -= 1
        
        # Step 3: Swap the elements at indices 'k' and 'l'.
        # This ensures nums[k] is now larger, and the prefix up to k is part of the next permutation.
        nums[k], nums[l] = nums[l], nums[k]
        
        # Step 4: Reverse the subarray from index 'k + 1' to the end of the array.
        # The suffix nums[k+1:] was in non-increasing order before the swap (or mostly so).
        # To get the lexicographically smallest permutation for this suffix (now that nums[k] is fixed),
        # we sort it in ascending order, which is achieved by reversing it.
        left, right = k + 1, n - 1
        while left < right:
            nums[left], nums[right] = nums[right], nums[left]
            left += 1
            right -= 1
```

---

## Key Points & Algorithm Explanation

The algorithm to find the next lexicographically greater permutation involves these steps:

1.  **Find the Pivot (`k`):**
    *   Iterate from the right end of the array towards the left (`n-2` down to `0`).
    *   Find the first element `nums[k]` such that `nums[k] < nums[k+1]`.
    *   This `nums[k]` is the element we need to increase to get the next permutation. The suffix `nums[k+1:]` is currently in non-increasing (descending) order.
    *   **Edge Case:** If no such `k` is found (the entire array is in descending order), it means we are at the largest permutation. The "next" permutation is the smallest one (sorted ascending). In this case, reverse the entire array and return.

2.  **Find the Successor to the Pivot (`l`):**
    *   If a pivot `k` is found, iterate from the right end of the array towards `k+1`.
    *   Find the first element `nums[l]` (where `l > k`) such that `nums[l] > nums[k]`.
    *   This `nums[l]` is the smallest element in the suffix `nums[k+1:]` that is still greater than `nums[k]`. It will replace `nums[k]`.

3.  **Swap:**
    *   Swap `nums[k]` and `nums[l]`.
    *   After this swap, the prefix `nums[0...k]` is now correctly set for the next permutation. `nums[k]` has been increased to the smallest possible next value.

4.  **Sort the Suffix:**
    *   The suffix of the array starting from `k+1` (i.e., `nums[k+1:]`) now needs to be arranged in its smallest possible order (ascending) to ensure we have the lexicographically *next* permutation.
    *   Since the elements in the suffix `nums[k+1:]` (before the swap, and largely after the swap of one element from it) were in non-increasing order, reversing this suffix will sort it into non-decreasing (ascending) order.

---

## Complexity Analysis

*   **Time Complexity:** O(N)
    *   Step 1 (finding `k`): At most N-1 comparisons.
    *   Step 2 (finding `l`): At most N-k comparisons.
    *   Step 3 (swap): O(1).
    *   Step 4 (reversing suffix): At most N/2 swaps.
    *   In the worst case, each step involves a linear scan of a portion of the array.

*   **Space Complexity:** O(1)
    *   The algorithm modifies the array in-place using only a few variables for indices, thus meeting the constant extra memory requirement.

---

## Important Considerations

*   **In-place Modification:** The solution directly modifies the input `nums` list and does not return a new list, as per the problem requirement.
*   **Lexicographical Order:** Understanding this is key. It's like dictionary order: `[1,2,3]` comes before `[1,3,2]`.
*   **Handling the "Last Permutation":** The check for `k == -1` correctly identifies when the array is already the largest permutation (e.g., `[3,2,1]`) and transforms it into the smallest (`[1,2,3]`).

This structured approach helps in understanding and implementing the solution for finding the next permutation.
