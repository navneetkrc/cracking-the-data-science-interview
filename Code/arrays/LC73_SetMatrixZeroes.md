
---

# LeetCode 73: Set Matrix Zeroes

## Problem Statement

> Given an `m x n` integer `matrix`, if an element is `0`, set its entire row and column to `0`'s.
>
> You must do it **in-place**.

---

## Examples

**Example 1:**

*   **Input:** `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
*   **Output:** `[[1,0,1],[0,0,0],[1,0,1]]`
    (The `0` at `matrix[1][1]` causes its row and column to become zeros)
![image](https://github.com/user-attachments/assets/468ecaa4-feaf-4688-9981-498c99e64c1a)

**Example 2:**

*   **Input:** `matrix = [[0,1,2,0],[3,4,5,2],[1,3,1,5]]`
*   **Output:** `[[0,0,0,0],[0,4,5,0],[0,3,1,0]]`
![image](https://github.com/user-attachments/assets/27422199-d9a8-44c2-9b51-9f7fea571f08)

---

## Constraints

*   `m == matrix.length`
*   `n == matrix[0].length`
*   `1 <= m, n <= 200`
*   `-2^31 <= matrix[i][j] <= 2^31 - 1`

---

## Follow Up (Space Complexity)

*   A straightforward solution using O(mn) space is probably a bad idea.
*   A simple improvement uses O(m + n) space, but still not the best solution.
*   Could you devise a **constant space** solution?

---

## Python Solution (O(1) Space Optimization)

```python
from typing import List # Standard import for type hints

class Solution:
    def setZeroes(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        This solution uses O(1) extra space by utilizing the first row and column
        of the matrix itself as markers.
        """
        rows = len(matrix)
        cols = len(matrix[0])

        # Step 1: Determine if the first row or first column need to be zeroed.
        # These flags are necessary because the first row/column will be used for marking
        # and their original zero-state might be overwritten.
        first_row_has_zero = False
        for j in range(cols):
            if matrix[0][j] == 0:
                first_row_has_zero = True
                break
        
        first_col_has_zero = False
        for i in range(rows):
            if matrix[i][0] == 0:
                first_col_has_zero = True
                break

        # Step 2: Use the first row and first column to mark rows/columns that need to be zeroed.
        # Iterate through the rest of the matrix (excluding the first row and column).
        # If matrix[i][j] is 0, mark its corresponding first_row_element (matrix[0][j])
        # and first_col_element (matrix[i][0]) as 0.
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][j] == 0:
                    matrix[i][0] = 0  # Mark this row to be zeroed
                    matrix[0][j] = 0  # Mark this column to be zeroed

        # Step 3: Zero out cells based on the marks in the first row and column.
        # Iterate through the rest of the matrix again.
        # If the marker in the first row (matrix[0][j]) or first column (matrix[i][0]) is 0,
        # then set matrix[i][j] to 0.
        for i in range(1, rows):
            for j in range(1, cols):
                if matrix[i][0] == 0 or matrix[0][j] == 0:
                    matrix[i][j] = 0

        # Step 4: Zero out the first row if it originally contained a zero.
        if first_row_has_zero:
            for j in range(cols):
                matrix[0][j] = 0

        # Step 5: Zero out the first column if it originally contained a zero.
        if first_col_has_zero:
            for i in range(rows):
                matrix[i][0] = 0
```

---

## Key Points & Algorithm Explanation (O(1) Space Approach)

The challenge is to perform this operation in-place using constant extra space. The key idea is to use the **first row and first column of the matrix itself** as storage to mark which rows and columns need to be zeroed out.

1.  **Initial State of First Row/Column:**
    *   Since `matrix[0][0]` is part of both the first row and first column, using it directly as a marker for both can be ambiguous.
    *   Therefore, we first check if the first row and first column *independently* contain any zeros. We store this information in two boolean flags: `first_row_has_zero` and `first_col_has_zero`. This is crucial because the marking process in the next step might alter `matrix[0][0]` or other elements in the first row/column.

2.  **Marking Rows and Columns:**
    *   Iterate through the *rest* of the matrix (i.e., from `matrix[1][1]` to `matrix[rows-1][cols-1]`).
    *   If an element `matrix[i][j]` is `0`, we mark its corresponding cell in the first row (`matrix[0][j] = 0`) and its corresponding cell in the first column (`matrix[i][0] = 0`). These cells now act as indicators that row `i` and column `j` should be zeroed.

3.  **Setting Zeros (excluding first row/column):**
    *   Iterate through the *rest* of the matrix again (from `matrix[1][1]`).
    *   For each element `matrix[i][j]`, if its corresponding marker in the first column (`matrix[i][0]`) is `0` OR its marker in the first row (`matrix[0][j]`) is `0`, then set `matrix[i][j] = 0`.

4.  **Processing First Row and Column:**
    *   Finally, use the `first_row_has_zero` flag. If it's true, set all elements in the first row to `0`.
    *   Similarly, use the `first_col_has_zero` flag. If it's true, set all elements in the first column to `0`.
    *   This order (processing the rest of the matrix first, then the first row/column) is important to avoid prematurely zeroing out markers.

---

## Complexity Analysis

*   **Time Complexity:** O(M × N)
    *   The algorithm involves several passes over the matrix:
        *   Checking first row/column: O(M + N)
        *   Marking using first row/column: O(M × N)
        *   Setting zeros based on marks: O(M × N)
        *   Processing first row/column finally: O(M + N)
    *   Overall, it's dominated by the O(M × N) steps.

*   **Space Complexity:** O(1)
    *   The solution modifies the matrix in-place.
    *   It uses only a few boolean variables (`first_row_has_zero`, `first_col_has_zero`) which count as constant extra space. The first row and column are re-purposed for marking, not allocated as *extra* space.

---

## Important Considerations

*   **In-Place Modification:** The solution adheres to the requirement of modifying the matrix in-place.
*   **O(1) Space Strategy:** The clever use of the first row and column as markers is the core of the constant space solution. This avoids the need for auxiliary arrays (which would take O(M+N) or O(M\*N) space).
*   **Order of Operations:**
    1.  Store the original zero status of the first row and column.
    2.  Use the first row/column to mark zeros for the *rest* of the matrix.
    3.  Set zeros for the *rest* of the matrix based on these marks.
    4.  Set zeros for the actual first row/column based on their initially stored status.
    This specific order prevents markers from being overwritten before they are used, and prevents the first row/column from being zeroed out prematurely, which would destroy the marker information.
