## 1. Introduction – The Brute Force Approach

**Linear Search** (also called **Sequential Search**) is the simplest searching algorithm in computer science. It checks every element in the array **one by one, in order**, from the beginning, until it either finds the target value or reaches the end of the array.

There's no cleverness involved — no assumptions about the data, no splitting, no skipping. Just a straightforward scan.

**Real-World Analogy:** Imagine looking for a specific book on a messy shelf. You don't have a catalog or any ordering system — you just start from the left and check each book one by one until you find the one you want. That's Linear Search.

**Why start with Linear Search?**

- It's the **baseline** against which all other search algorithms are compared.
    
- It's the only search algorithm that works on **any** data — sorted or unsorted.
    
- It teaches the fundamental O(n) complexity that appears everywhere in computer science.
    

---

## 2. How Linear Search Works

**The Algorithm:**

1. Start at index `0`.
    
2. Compare `arr[i]` with the target `x`.
    
3. If they match, return `i` (the index where it was found).
    
4. If not, move to the next index and repeat.
    
5. If the loop finishes without a match, return `-1` (element not found).
    

**Visual Trace:**

Array: [2, 3, 4, 10, 40]
Target: 10
Step 1: Check index 0 → 2 ≠ 10 → continue
Step 2: Check index 1 → 3 ≠ 10 → continue
Step 3: Check index 2 → 4 ≠ 10 → continue
Step 4: Check index 3 → 10 == 10 → FOUND at index 3!

**What if the target isn't there?**

Array: [2, 3, 4, 10, 40]
Target: 7
Step 1: Check index 0 → 2 ≠ 7 → continue
Step 2: Check index 1 → 3 ≠ 7 → continue
Step 3: Check index 2 → 4 ≠ 7 → continue
Step 4: Check index 3 → 10 ≠ 7 → continue
Step 5: Check index 4 → 40 ≠ 7 → continue
Step 6: End of array reached → NOT FOUND → return -1

---

## 3. Pseudocode


FUNCTION linearSearch(arr, target)
    n ← length(arr)
    
    // Iterate over the array in order
    FOR i ← 0 TO n - 1
        IF arr[i] = target THEN
            RETURN i    // Element found at index i
        END IF
    END FOR
    
    // If we reach here, the element was not present
    RETURN -1
END FUNCTION

**That's it.** No sorting required, no special conditions, no edge cases beyond an empty array. It's as simple as it gets.

---

## 4. Complexity Analysis

|Case|Time|Why|
|---|---|---|
|**Best** (target is first element)|O(1)|Found on the first comparison|
|**Average**|O(n)|On average, checks half the elements|
|**Worst** (target is last or not present)|O(n)|Must check every element|
|**Space**|O(1)|Only uses a loop counter — no extra memory|

**Why O(n)?**  
Because in the worst case, you have to look at **every single element** once. If the array doubles in size, the worst-case time doubles too — that's linear growth.

**Comparison with Binary Search:**

|Feature|Linear Search|Binary Search|
|---|---|---|
|**Time Complexity**|O(n)|O(log n)|
|**Requires sorted data?**|No|Yes|
|**Works on linked lists?**|Yes|No (needs random access)|
|**Best for**|Small/unsorted data|Large sorted data|
|**1,000,000 items (worst case)**|1,000,000 checks|~20 checks|

That last row is the kicker — Binary Search is **50,000× faster** on a million items. But Binary Search has a catch: the data **must be sorted**. If your data isn't sorted and you'd only search it once, Linear Search is often the smarter choice — because sorting first (O(n log n)) costs more than a single linear scan (O(n)).

---

## 5. Key Properties

- **No assumptions about the data.** Linear Search works on any array — sorted, unsorted, random, whatever. This is its biggest advantage over Binary Search.
    
- **Works on Linked Lists.** Since linked lists don't support random access (you can't jump to index `i` directly), you have to traverse them one node at a time anyway — which is exactly what Linear Search does. This makes it a natural fit, whereas Binary Search can't be used efficiently on a plain linked list.
    
- **Best for small datasets.** When `n` is small, the simplicity of Linear Search often makes it just as fast in practice as more complex algorithms — and there's no overhead (like sorting the data first) to worry about.
    
- **Stable and predictable.** No edge cases, no pivot selection, no recursion. It just works.
    
- **In-place and memory-efficient.** Uses O(1) extra space — just a loop counter.
    

---

## 6. When to Use Linear Search

**Use it when:**

- The data is **unsorted** and you're only searching **once** (sorting first would cost more than a linear scan).
    
- The dataset is **small** (n < ~100). The overhead of more complex algorithms isn't worth it.
    
- You're searching a **linked list** (no random access → Binary Search is impossible).
    
- You need to find **all occurrences** of a value (just continue scanning after finding one).
    
- You're doing a **one-time search** on data that changes frequently (re-sorting every time would be wasteful).
    

**Avoid it when:**

- The data is **sorted** and you're searching **many times** → use Binary Search (O(log n)).
    
- The dataset is **large** and performance matters → sort once, then Binary Search.
    
- You need **fast lookups** by key → use a Hash Table (O(1) average).
    

---

## 7. Variations and Extensions

### 7.1 Finding All Occurrences

Instead of returning the first match, collect all indices where the target appears.

FUNCTION linearSearchAll(arr, target)
    result ← NEW LIST
    
    FOR i ← 0 TO length(arr) - 1
        IF arr[i] = target THEN
            result.add(i)
        END IF
    END FOR
    
    RETURN result
END FUNCTION

### 7.2 Sentinel Linear Search

A minor optimization: place the target at the end of the array as a **sentinel**, so you don't need to check bounds on every iteration.

FUNCTION sentinelLinearSearch(arr, target)
    n ← length(arr)
    last ← arr[n - 1]
    
    // Place the target at the end as a sentinel
    arr[n - 1] ← target
    
    i ← 0
    WHILE arr[i] ≠ target
        i ← i + 1
    END WHILE
    
    // Restore the original last element
    arr[n - 1] ← last
    
    IF i < n - 1 OR arr[n - 1] = target THEN
        RETURN i
    END IF
    
    RETURN -1
END FUNCTION

**Note:** This saves one comparison per iteration but doesn't change the O(n) complexity.

---

## 8. Real-World Applications

|Application|How Linear Search Is Used|
|---|---|
|**Small configuration files**|Scanning key-value pairs for a specific setting.|
|**Linked list traversal**|Finding a node by value (no random access possible).|
|**Unsorted logs**|Searching for a specific event in a log file.|
|**Finding all duplicates**|Scanning an array to collect all occurrences of a value.|
|**Simple game logic**|Checking if a player's move is valid by scanning a small list.|
|**Interview warm-ups**|The classic "find an element in an array" question.|

---

## 9. Summary – Your Linear Search Takeaway

- **Linear Search** checks every element one by one until the target is found or the array ends.
    
- **Time Complexity:** O(n) — worst case checks all n elements.
    
- **Space Complexity:** O(1) — no extra memory needed.
    
- **No sorting required** — works on any array, sorted or unsorted.
    
- **Works on linked lists** — no random access needed.
    
- **Best for small datasets, unsorted data, and one-time searches.**
    
- **Key insight:** Linear Search is the **baseline** — it's what you compare every other search algorithm against. Its simplicity is both its weakness (slow on large data) and its strength (works anywhere, no preconditions).
