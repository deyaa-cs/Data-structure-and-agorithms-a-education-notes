## 1. Introduction – The Big Idea

Some of the most powerful algorithms in computer science share a single idea: **solve a big problem by solving smaller versions of the same problem.** This idea is called **recursion**.

Think of it like a set of Russian nesting dolls. To open the biggest doll, you open the one inside it, and the one inside that, and so on — until you reach the smallest doll that doesn't open any further. That smallest doll is your **stopping point**.

Recursion is everywhere in computer science:

- **Trees and graphs** – traversing a tree means recursively visiting its subtrees.
    
- **Divide and conquer** – Merge Sort, Quick Sort, and Binary Search all use recursion.
    
- **Dynamic programming** – many optimization problems are solved recursively with memoization.
    
- **Backtracking** – solving mazes, puzzles, and constraint satisfaction problems.
    
- **File systems** – exploring a folder means recursively exploring its subfolders.
    

Once you understand recursion, you unlock a whole new way of thinking about problems.

---

## 2. What Exactly Is Recursion?

**Recursion** is when a function **calls itself** to solve a smaller instance of the same problem, until it reaches a problem so small it can be solved directly.

Every recursive function has two essential parts:

1. **Base Case**: The simplest case where the answer is known without any further recursion. This is the **stopping condition** — without it, the function would call itself forever (infinite recursion → stack overflow).
    
2. **Recursive Case**: The problem is broken down into a smaller version of itself, and the function calls itself to solve that smaller version.
    

**The Golden Rule:**


Every recursive call MUST move toward the base case.

If it doesn't, you get infinite recursion, which crashes your program with a **stack overflow**.

---

## 3. Steps to Implement Recursion

Here's a systematic four-step framework for writing any recursive function:

**Step 1 — Define a base case:**  
Identify the simplest case for which the solution is known or trivial. This is the stopping condition for the recursion.

**Step 2 — Define a recursive case:**  
Define the problem in terms of smaller subproblems. Break the problem down into smaller versions of itself, and call the function recursively to solve each subproblem.

**Step 3 — Ensure the recursion terminates:**  
Make sure that the recursive function eventually reaches the base case and does not enter an infinite loop.

**Step 4 — Combine the solutions:**  
Combine the solutions of the subproblems to solve the original problem.

---

## 4. Example: Sum of the First N Natural Numbers

Let's apply these four steps to a simple, classic example — calculating the sum of numbers from 1 to `n`.

**The Mathematical Definition:**



Sum(n) = 1 + 2 + 3 + ... + n

But we can also define it recursively:



Sum(n) = n + Sum(n - 1)     ← Recursive case
Sum(1) = 1                  ← Base case

**Pseudocode:**



FUNCTION Sum(n)
    // Base case: sum of 1 is just 1
    IF n = 1 THEN
        RETURN 1
    END IF
    
    // Recursive case: n + sum of everything below n
    RETURN n + Sum(n - 1)
END FUNCTION
// Main
PRINT Sum(5)    // Output: 15

**Mapping the code back to the four steps:**

- **Base case (Step 1):** `IF n = 1 THEN RETURN 1` — we know the sum of just the number 1 is 1. No further calculation is needed.
    
- **Recursive case (Step 2):** `RETURN n + Sum(n - 1)` — we express "sum of 1 to n" as "n plus the sum of 1 to (n-1)," which is a smaller version of the same problem.
    
- **Termination (Step 3):** Every call reduces `n` by 1, so no matter what value we start with, `n` will eventually hit the base case of 1.
    
- **Combining (Step 4):** As each call returns, its result is added (`n +`) to the result of the smaller subproblem, building the final answer on the way back up.
    

---

## 5. Tracing the Call Stack

It helps to actually watch what happens when `Sum(5)` is called. Each call **pauses and waits** for the next one to finish, building a **stack** of paused calls:

**Going Down (Building the Stack):**



Sum(5) → 5 + Sum(4)
            Sum(4) → 4 + Sum(3)
                        Sum(3) → 3 + Sum(2)
                                    Sum(2) → 2 + Sum(1)
                                                Sum(1) → returns 1   (base case reached!)

**Coming Back Up (Unwinding the Stack):**



Sum(1) = 1
Sum(2) = 2 + 1 = 3
Sum(3) = 3 + 3 = 6
Sum(4) = 4 + 6 = 10
Sum(5) = 5 + 10 = 15

**Final output: 15.**

This "go down to the base case, then combine on the way back up" pattern is the **heartbeat** of almost every recursive function you'll write.

---

## 6. The Call Stack – What's Really Happening

When a function is called, the computer creates a **stack frame** containing:

- The function's parameters.
    
- Local variables.
    
- The return address (where to go back to when the function finishes).
    

For `Sum(5)`, the call stack grows like this:



Step 1:  [Sum(5)]                          ← Bottom of stack
Step 2:  [Sum(5), Sum(4)]
Step 3:  [Sum(5), Sum(4), Sum(3)]
Step 4:  [Sum(5), Sum(4), Sum(3), Sum(2)]
Step 5:  [Sum(5), Sum(4), Sum(3), Sum(2), Sum(1)]  ← Top of stack (base case)

Then it unwinds:



Step 6:  [Sum(5), Sum(4), Sum(3), Sum(2)]  ← Sum(1) returned 1
Step 7:  [Sum(5), Sum(4), Sum(3)]          ← Sum(2) returned 3
Step 8:  [Sum(5), Sum(4)]                  ← Sum(3) returned 6
Step 9:  [Sum(5)]                          ← Sum(4) returned 10
Step 10: []                                ← Sum(5) returned 15

**Each recursive call uses memory.** If you recurse too deeply (e.g., `Sum(100000)`), you'll run out of stack space → **stack overflow**.

---

## 7. Recursion vs Iteration

Anything you can do with recursion, you can also do with loops (iteration). So why choose one over the other?

|Feature|Recursion|Iteration|
|---|---|---|
|**Readability**|Often cleaner and closer to the mathematical definition|Can be more verbose|
|**Memory**|Uses stack space (O(n) for n recursive calls)|Uses constant space (O(1))|
|**Speed**|Slower due to function call overhead|Faster — no function calls|
|**Risk**|Stack overflow for deep recursion|No stack overflow risk|
|**Best For**|Tree/graph traversal, divide-and-conquer, backtracking|Simple loops, linear processing|

**The Same Sum Problem — Iterative Version:**

FUNCTION SumIterative(n)
    total ← 0
    FOR i ← 1 TO n
        total ← total + i
    END FOR
    RETURN total
END FUNCTION
// Time Complexity: O(n)
// Space Complexity: O(1) — no stack growth!

**Key Insight:** The recursive version uses **O(n) space** (the call stack), while the iterative version uses **O(1) space**. For simple problems like this, iteration is usually better. But for problems with **inherent recursive structure** (trees, graphs, divide-and-conquer), recursion is the clear winner.

---

## 8. When to Use Recursion — and When to Be Careful

**Recursion shines when:**

- **The problem is naturally self-similar** — it can be broken into smaller versions of itself (tree traversal, divide-and-conquer algorithms like Merge Sort, exploring folder structures, generating permutations).
    
- **Readability matters** — a recursive solution often mirrors the mathematical or logical definition of a problem far more clearly than an equivalent loop would.
    
- **Backtracking is needed** — solving mazes, Sudoku, N-Queens, and other constraint satisfaction problems.
    
- **The data structure is recursive** — trees and graphs are defined recursively, so recursive algorithms are natural fits.
    

**Be careful when:**

- **The recursion depth could get very large.** Every recursive call adds a new frame to the call stack, and too many can cause a **stack overflow**. For example, `Sum(1000000)` would crash most systems.
    
- **Performance is critical and the recursive version recalculates the same subproblem repeatedly.** Naive recursive Fibonacci is O(2ⁿ) because it recomputes the same values over and over. **Memoization** (caching results) or an **iterative approach** is usually faster.
    

**Naive Recursive Fibonacci (Exponential — BAD):**


FUNCTION fib(n)
    IF n <= 1 THEN
        RETURN n
    END IF
    RETURN fib(n - 1) + fib(n - 2)   // Recalculates the same values!
END FUNCTION
// Time Complexity: O(2^n) — extremely slow for large n

**Memoized Fibonacci (Linear — GOOD):**


DECLARE memo AS MAP
FUNCTION fibMemo(n)
    IF n <= 1 THEN
        RETURN n
    END IF
    
    IF memo.contains(n) THEN
        RETURN memo.get(n)
    END IF
    
    result ← fibMemo(n - 1) + fibMemo(n - 2)
    memo.put(n, result)
    RETURN result
END FUNCTION
// Time Complexity: O(n) — each value computed only once

---

## 9. Classic Recursion Examples

### 9.1 Factorial


FUNCTION factorial(n)
    IF n = 0 OR n = 1 THEN
        RETURN 1
    END IF
    RETURN n * factorial(n - 1)
END FUNCTION
// factorial(5) = 5 * 4 * 3 * 2 * 1 = 120

### 9.2 Fibonacci (Naive)


FUNCTION fib(n)
    IF n <= 1 THEN
        RETURN n
    END IF
    RETURN fib(n - 1) + fib(n - 2)
END FUNCTION
// fib(6) = 8

### 9.3 Tower of Hanoi


PROCEDURE towerOfHanoi(n, source, auxiliary, destination)
    IF n = 1 THEN
        PRINT "Move disk 1 from " + source + " to " + destination
        RETURN
    END IF
    
    towerOfHanoi(n - 1, source, destination, auxiliary)
    PRINT "Move disk " + n + " from " + source + " to " + destination
    towerOfHanoi(n - 1, auxiliary, source, destination)
END PROCEDURE
// The classic puzzle — 2^n - 1 moves required

### 9.4 Binary Tree Traversal (In-Order)


PROCEDURE inOrder(node)
    IF node IS NOT NULL THEN
        inOrder(node.left)
        PRINT node.value
        inOrder(node.right)
    END IF
END PROCEDURE

---

## 10. Summary – Your Recursion Takeaway

- **Recursion** is a technique where a function calls itself to solve smaller instances of the same problem.
    
- Every recursive function needs a **base case** (stopping condition) and a **recursive case** (self-reference).
    
- The **call stack** tracks paused function calls. Deep recursion → stack overflow.
    
- **Recursion vs Iteration**: Recursion is cleaner for self-similar problems; iteration is more memory-efficient for simple loops.
    
- **When to use**: Trees, graphs, divide-and-conquer, backtracking, and problems with recursive definitions.
    
- **When to avoid**: Deep recursion (stack overflow risk) and cases where the same subproblem is recalculated (use memoization or iteration).
    
- **Key insight**: Recursion mirrors the mathematical structure of a problem, making complex algorithms surprisingly elegant.
