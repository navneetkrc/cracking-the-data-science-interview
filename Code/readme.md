**The Core 7 Steps for approaching an interview coding problem:**

1.  **Understand the Problem & Listen for Clues:**
    *   **Note:** "Listen for clues (what is given -> hints | idea)"
    *   **Principle:** Actively listen and thoroughly understand the problem statement. Identify inputs, outputs, constraints, and any hints or specific requirements mentioned. Don't jump to coding.
    *   "Ask clarifying questions, Restating the problem" - This is crucial for ensuring you and the interviewer are on the same page.

2.  **Work Through Examples (Manually):**
    *   **Note:** "Draw an example: *large and generic / *so brain automatically tries to find sol'n / *don't make any assumption (both the arrays are same sized array/ arrays are already sorted/ unsorted)"
    *   **Principle:** Create and manually solve a few examples, including simple ones, edge cases (empty input, single element, very large input), and generic cases. This helps solidify your understanding and often reveals patterns or potential issues. Avoid assumptions about the input (e.g., sorted, positive numbers) unless explicitly stated.

3.  **Devise a Brute-Force Solution First:**
    *   **Note:** "Come up with brute-force for space-time complexity. *Initial approach to fix shortcomings."
    *   **Principle:** Start by thinking of the most straightforward, even if inefficient, way to solve the problem. This gets a working solution on the table and serves as a baseline.
    *   ** "Kick it off -> Start with baseline -> Need not be good." - This echoes the idea of starting with a basic, functional solution.

4.  **Optimize the Solution (BUD):**
    *   **Note:** "Optimize (generally before writing code)." and "Optimizing with BUD: Bottlenecks, Unnecessary Work, Duplicated Work. Recomputing the same values multiple times. Computations that don't affect the result. Part of code that dominates runtime."
    *   **Principle:** Once you have a brute-force idea, analyze its time and space complexity. Look for:
        *   **B**ottlenecks: The slowest parts of your algorithm.
        *   **U**nnecessary Work: Calculations or steps that aren't needed.
        *   **D**uplicate Work: Re-calculating the same thing multiple times.
    *   Think about data structures and algorithms that could improve performance.

5.  **Walk Through Your Algorithm (Dry Run):**
    *   **Note:** "Walk Through algo before actually coding. {Pause and ensure -> should know where you are heading}."
    *   **Principle:** Before writing a single line of code, mentally (or on paper) execute your optimized algorithm with an example. This helps catch logical flaws early and ensures you have a clear plan. "Pause and ensure" you know the direction.

6.  **Write Clean Code & Articulate Your Thought Process:**
    *   **Note:** "Write code. No Run Required. *they want to hear our thought process."
    *   **Principle:** Implement your planned algorithm. Write clean, readable, and well-structured code. As you code (especially in an interview), explain what you're doing and why. They want to understand your thinking.
    *   **From Page 1:** "Coding skills, well-written. Good style. Correct(ish) Not necessarily flawless." "Real Code X Pseudo code." "Good code (may not be perfect)."

7.  **Verify and Test Your Code:**
    *   **Note:** "Verification -> Runtime -> Value check. -> As much bug-free as possible. Code walk through -> Code Review and explain the approach to end results."
    *   **Principle:** After coding, test your solution with various inputs (the examples you created earlier, plus new edge cases). Walk through your code again, checking for bugs. Explain your testing strategy and how you'd ensure correctness.
    *   **From Page 1:** "Verify that code works."

**Overarching Principles & Interview Context :**

*   **Iterative Improvement:** "You can improve while coding as well." Problem-solving isn't always linear. You might revisit earlier steps.
*   **Communication & Collaboration:** "Ask questions to interviewer as well." "Interviewer as collaborative exercises." "Interviewer also suggests improvements." "Communication & collaboration." This is key in an interview setting.
*   **Focus on the Process:** "Process of solving a question." Interviewers care about *how* you approach problems, not just the final answer.
*   **Time & Space Complexity (Big O):** "Master Big O Notations." "Space and Time Complexity." This should be a consideration throughout, especially during optimization. 
*   **Mindset:** "Push Yourself Even when its hard." "Energy to work through tough problems." "It's okay to struggle through a problem." Resilience and a positive attitude are valued.
*   **Practice:** "Implement DSA from scratch." "Practice on real interview questions." "Practice coding in 'real' environments." 
