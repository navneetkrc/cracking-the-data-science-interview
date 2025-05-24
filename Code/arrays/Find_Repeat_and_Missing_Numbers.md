
---

# Problem: Find the Repeating and Missing Numbers

## Problem Statement

> You are given a read-only array of **N** integers. The values in the array are also in the range **[1, N]** (both inclusive).
> Each integer appears exactly once *except* for one integer **A** which appears twice, and one integer **B** which is missing from the sequence `1` to `N`.
>
> The task is to find the repeating number **A** and the missing number **B**.

---

## Examples

**Example 1:**

*   **Input Format:** `array[] = {3,1,2,5,3}` (N=5, range of numbers should be [1,5])
*   **Result:** `{3, 4}`
*   **Explanation:** `A = 3` (repeating), `B = 4` (missing)

**Example 2:**

*   **Input Format:** `array[] = {3,1,2,5,4,6,7,5}` (N=8, range of numbers should be [1,8])
*   **Result:** `{5, 8}`
*   **Explanation:** `A = 5` (repeating), `B = 8` (missing)

---

## Constraints (Implied by "read-only array of N integers with values in range [1,N]")

*   `N` is the size of the array.
*   The numbers in the array are from `1` to `N`.
*   Exactly one number is repeated.
*   Exactly one number is missing.

---

## Solution (Mathematical Approach)

```python
class Solution:
    # @param A : tuple of integers (or list of integers)
    # @return a list of integers [repeating_number, missing_number]
    def repeatedNumber(self, arr_nums): # Renamed A to arr_nums for clarity
        n = len(arr_nums)
        
        # Let 'R' be the repeating number and 'M' be the missing number.
        
        # Calculate Sum(actual_numbers) - Sum(expected_numbers_1_to_N)
        # Sum(actual_numbers) = Sum_N + R - M
        # Sum(expected_numbers_1_to_N) = Sum_N
        # So, Sum(actual_numbers) - Sum(expected_numbers_1_to_N) = (Sum_N + R - M) - Sum_N = R - M
        sum_actual = 0
        sum_expected = 0
        
        # Calculate Sum(actual_numbers_squared) - Sum(expected_numbers_1_to_N_squared)
        # Sum(actual_numbers_squared) = Sum_N_sq + R^2 - M^2
        # Sum(expected_numbers_1_to_N_squared) = Sum_N_sq
        # So, Sum(actual_numbers_squared) - Sum(expected_numbers_1_to_N_squared) = (Sum_N_sq + R^2 - M^2) - Sum_N_sq = R^2 - M^2
        sum_sq_actual = 0
        sum_sq_expected = 0

        for i in range(n):
            sum_actual += arr_nums[i]
            sum_expected += (i + 1)
            
            sum_sq_actual += arr_nums[i] * arr_nums[i]
            sum_sq_expected += (i + 1) * (i + 1)
            
        # Equation 1: R - M = sum_actual - sum_expected
        diff_sum = sum_actual - sum_expected  # This is (R - M)
        
        # Equation 2: R^2 - M^2 = sum_sq_actual - sum_sq_expected
        # R^2 - M^2 = (R - M)(R + M)
        diff_sum_sq = sum_sq_actual - sum_sq_expected # This is (R^2 - M^2)
        
        # From Equation 2 and Equation 1:
        # R + M = (R^2 - M^2) / (R - M)
        # R + M = diff_sum_sq / diff_sum
        sum_val = diff_sum_sq / diff_sum # This is (R + M)
        
        # Now we have two simple linear equations:
        # 1) R - M = diff_sum
        # 2) R + M = sum_val
        
        # Solving for R:
        # Add (1) and (2): 2R = diff_sum + sum_val  => R = (diff_sum + sum_val) / 2
        repeating_num_A = (diff_sum + sum_val) / 2
        
        # Solving for M:
        # M = sum_val - R  (from equation 2)
        # OR M = R - diff_sum (from equation 1)
        missing_num_B = sum_val - repeating_num_A
                
        return [int(repeating_num_A), int(missing_num_B)]

```
*Note: The provided solution in the prompt used a slightly condensed calculation within the loop. The version above breaks it down for clarity to match standard sum formulas, but the mathematical principle is the same.*

**Original Condensed Logic Equivalence:**
The original code's `sum_diff` directly calculates `(R - M)`.
`sum_diff = sum(A[i] - (i+1))`

The original code's `sum_diff_square` directly calculates `(R^2 - M^2)`.
`sum_diff_square = sum(A[i]^2 - (i+1)^2)`

Then it solves:
`R + M = sum_diff_square / sum_diff`
`R = ((sum_diff) + (R+M)) / 2 = (sum_diff + sum_diff_square/sum_diff) / 2 = ((sum_diff * sum_diff) + sum_diff_square) / (2*sum_diff)`
`M = (R+M) - R` or `M = R - sum_diff`

---

## Key Points & Algorithm Explanation (Mathematical Approach)

This solution cleverly uses mathematical properties of sums and sums of squares to find the repeating (A) and missing (B) numbers.

1.  **Understanding the Problem:**
    *   We have numbers from `1` to `N`.
    *   One number `A` is repeated.
    *   One number `B` is missing.
    *   All other numbers appear exactly once.

2.  **Formulating Equations:**
    Let:
    *   `S_N` = Sum of first N natural numbers = `1 + 2 + ... + N` = `N*(N+1)/2`
    *   `S_actual` = Sum of elements in the given array.
    *   `S_sq_N` = Sum of squares of first N natural numbers = `1^2 + 2^2 + ... + N^2` = `N*(N+1)*(2N+1)/6`
    *   `S_sq_actual` = Sum of squares of elements in the given array.

    We can observe:
    *   `S_actual = S_N - B + A`  (The sum of array elements is the sum of 1 to N, minus the missing `B`, plus the extra `A`)
        => `S_actual - S_N = A - B`  --- (Equation 1)

    *   `S_sq_actual = S_sq_N - B^2 + A^2` (Similarly for sum of squares)
        => `S_sq_actual - S_sq_N = A^2 - B^2` --- (Equation 2)

3.  **Solving the Equations:**
    *   From Equation 1, let `diff_sum = A - B`.
    *   From Equation 2, let `diff_sum_sq = A^2 - B^2`.
    *   We know `A^2 - B^2 = (A - B)(A + B)`.
    *   So, `diff_sum_sq = (diff_sum) * (A + B)`.
    *   Therefore, `A + B = diff_sum_sq / diff_sum`. Let this be `sum_val`.

    Now we have a system of two linear equations:
    *   `A - B = diff_sum`
    *   `A + B = sum_val`

    Solving this system:
    *   Adding the two equations: `2A = diff_sum + sum_val` => `A = (diff_sum + sum_val) / 2`
    *   Substituting A back: `B = sum_val - A` (or `B = A - diff_sum`)

4.  **Implementation Details:**
    *   The code iterates once through the array to calculate `S_actual` and `S_sq_actual`.
    *   Simultaneously, it can calculate `S_N` (as `sum(i+1)`) and `S_sq_N` (as `sum((i+1)^2)`), or use the direct formulas if preferred (though direct iteration for `S_N` and `S_sq_N` is less prone to overflow for very large N if not using arbitrary-precision integers). The provided solution directly computes `A-B` and `A^2-B^2` in the loop.
    *   It then applies the formulas derived above to find `A` and `B`.

---

## Complexity Analysis

*   **Time Complexity:** O(N)
    *   The algorithm iterates through the array once to calculate the sums. All other operations are constant time.

*   **Space Complexity:** O(1)
    *   The algorithm uses a few variables to store sums and intermediate results, which is constant extra space.

---

## Important Considerations

*   **Read-Only Array:** This method respects the "read-only" constraint as it doesn't modify the input array.
*   **Range of Numbers:** The solution relies on the numbers being in the range `[1, N]`.
*   **Integer Overflow:** For very large `N`, the sum of squares (`A^2`, `B^2`, `S_sq_N`, `S_sq_actual`) could potentially overflow standard integer types in some languages if not handled carefully (e.g., by using 64-bit integers or Python's arbitrary-precision integers). Python handles large integers automatically, so it's less of a concern there.
*   **Division by Zero:** If `diff_sum` (which is `A - B`) is zero, this would imply `A = B`. This scenario is theoretically impossible given the problem statement (one repeating, one missing, implying `A != B`). However, in a flawed implementation or with unexpected input, this could be an edge case. The problem constraints usually ensure `A != B`.

This mathematical approach is elegant and efficient for this specific problem.
