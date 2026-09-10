
## 1. Introduction – The Problem with Everything Else

Let's think about what we've learned so far:

- **Array (unsorted)**: Search is O(n). You have to check every element.
    
- **Array (sorted)**: Search is O(log n) with binary search. Better, but still not instant.
    
- **BST (balanced)**: Search is O(log n). Good, but requires ordering and balancing.
    
- **Linked List**: Search is O(n). Slow.
    

All of these require **comparisons**. You're always asking: _"Is this the one? No? Okay, what about this one?"_

Now imagine this: You have a **magic function** that takes any key (a name, an ID, a string) and tells you **exactly** where its value is stored. No searching. No comparisons. No loops. Just **one step**.

That's a **Hash Table**. It gives you **O(1) average-case** insertion, deletion, and lookup. It's the fastest general-purpose data structure for key-value storage.

**The Trade-Off:** That "magic function" isn't perfect. Sometimes it sends two different keys to the same spot. We call that a **collision**, and handling collisions is where all the complexity lives.

---

## 2. The Core Idea – Hash Functions

### 2.1 What is a Hash Function?

A **Hash Function** is a mathematical function that takes an input (called a **key**) and converts it into an integer (called a **hash code** or **hash value**). This integer is then used as an **index** into an array (called the **hash table**).

**The Formula:**


index = hashFunction(key) % tableSize

- `hashFunction(key)` produces a large integer.
    
- `% tableSize` (modulo) ensures the index fits within the array bounds.
    

**Visual Concept:**


Key: "Alice"  →  hashFunction("Alice")  →  48291  →  48291 % 10  →  1
Key: "Bob"    →  hashFunction("Bob")    →  71934  →  71934 % 10  →  4
Key: "Charlie"→  hashFunction("Charlie")→  30527  →  30527 % 10  →  7
Hash Table (size 10):
Index:  [0]   [1]     [2]   [3]   [4]    [5]   [6]   [7]      [8]   [9]
Value:  null  Alice   null  null  Bob    null  null  Charlie  null  null

### 2.2 Properties of a Good Hash Function

A good hash function should be:

1. **Deterministic**: The same key always produces the same hash value. (No randomness!)
    
2. **Fast to Compute**: It should be O(1) – no loops or heavy computation.
    
3. **Uniformly Distributed**: Keys should be spread evenly across the table. Clustering is bad.
    
4. **Minimize Collisions**: Different keys should ideally map to different indices.
    
5. **Avalanche Effect**: A small change in the key should produce a completely different hash.
    

### 2.3 Common Hash Functions

#### (A) Division Method


hash(key) = key % tableSize

Simple but effective if `tableSize` is a **prime number**. Primes reduce clustering.

**Pseudocode:**


FUNCTION hashDivision(key, tableSize)
    RETURN key % tableSize
END FUNCTION

#### (B) Multiplication Method


hash(key) = floor(tableSize * ((key * A) % 1))

Where `A` is a constant between 0 and 1 (e.g., 0.6180339887 – the golden ratio).

**Pseudocode:**


FUNCTION hashMultiplication(key, tableSize)
    A ← 0.6180339887   // Golden ratio
    fractionalPart ← (key * A) - FLOOR(key * A)
    RETURN FLOOR(tableSize * fractionalPart)
END FUNCTION

#### (C) String Hashing (Polynomial Rolling Hash)

For strings, we treat each character as a number and combine them.

hash("abc") = (a * p² + b * p¹ + c * p⁰) % tableSize

Where `p` is a prime number (e.g., 31 or 37).

**Pseudocode:**

FUNCTION hashString(key, tableSize)
    p ← 31
    hashValue ← 0
    power ← 1
    
    FOR i ← 0 TO length(key) - 1
        hashValue ← (hashValue + (ASCII(key[i]) * power)) % tableSize
        power ← (power * p) % tableSize
    END FOR
    
    RETURN hashValue
END FUNCTION

#### (D) Java's hashCode() Style

hash = 0
FOR each character c in string:
    hash = hash * 31 + ASCII(c)
RETURN hash

**Pseudocode:**

FUNCTION hashStringJava(key, tableSize)
    hashValue ← 0
    
    FOR EACH char IN key
        hashValue ← (hashValue * 31 + ASCII(char)) % tableSize
    END FOR
    
    RETURN hashValue
END FUNCTION

---

## 3. Collisions – When Two Keys Want the Same Spot

A **collision** occurs when two different keys hash to the **same index**.

**Example:**

hash("Alice") % 10 = 1
hash("Bob")   % 10 = 1   ← COLLISION! Both want index 1.

Collisions are **unavoidable** (by the Pigeonhole Principle: if you have more keys than slots, some must share). The question is: **how do we handle them?**

There are two main strategies:

1. **Separate Chaining** (Open Hashing)
    
2. **Open Addressing** (Closed Hashing)
    

---

### 3.1 Collision Resolution #1: Separate Chaining

**Idea:** Each slot in the hash table points to a **linked list** (or dynamic array). When a collision occurs, we simply **add the new key-value pair to the list** at that index.

**Visual Representation:**

Hash Table (size 5) with Chaining:
Index  [0]  →  NULL
Index  [1]  →  ("Alice", 25)  →  ("Bob", 30)  →  NULL
Index  [2]  →  ("Charlie", 35)  →  NULL
Index  [3]  →  NULL
Index  [4]  →  ("Dave", 40)  →  ("Eve", 45)  →  NULL

Both "Alice" and "Bob" hash to index 1, so they're stored in a linked list at that index.

**Node Structure (Pseudocode):**

CLASS HashNode
    DATA key
    DATA value
    NEXT AS HashNode
END CLASS

**Hash Table with Chaining (Pseudocode):**

CLASS HashTableChaining
    DECLARE tableSize AS INTEGER
    DECLARE table AS ARRAY OF HashNode
    DECLARE count AS INTEGER    // Number of elements stored
    
    PROCEDURE HashTableChaining(size)
        tableSize ← size
        table ← NEW ARRAY[tableSize]
        count ← 0
        
        FOR i ← 0 TO tableSize - 1
            table[i] ← NULL
        END FOR
    END PROCEDURE
    
    // Insert or update a key-value pair
    PROCEDURE insert(key, value)
        index ← hashFunction(key) % tableSize
        
        // Check if key already exists – update it
        current ← table[index]
        WHILE current IS NOT NULL
            IF current.key = key THEN
                current.value ← value   // Update existing
                RETURN
            END IF
            current ← current.next
        END WHILE
        
        // Key doesn't exist – insert at the head of the list
        newNode ← NEW HashNode()
        newNode.key ← key
        newNode.value ← value
        newNode.next ← table[index]
        table[index] ← newNode
        count ← count + 1
    END PROCEDURE
    // Time Complexity: O(1) average, O(n) worst case (all keys collide)
    
    // Search for a key
    FUNCTION search(key)
        index ← hashFunction(key) % tableSize
        current ← table[index]
        
        WHILE current IS NOT NULL
            IF current.key = key THEN
                RETURN current.value
            END IF
            current ← current.next
        END WHILE
        
        RETURN NULL   // Key not found
    END FUNCTION
    // Time Complexity: O(1) average, O(n) worst case
    
    // Delete a key
    PROCEDURE delete(key)
        index ← hashFunction(key) % tableSize
        current ← table[index]
        prev ← NULL
        
        WHILE current IS NOT NULL
            IF current.key = key THEN
                // Remove from linked list
                IF prev IS NULL THEN
                    table[index] ← current.next
                ELSE
                    prev.next ← current.next
                END IF
                DELETE current
                count ← count - 1
                RETURN
            END IF
            prev ← current
            current ← current.next
        END WHILE
    END PROCEDURE
    // Time Complexity: O(1) average, O(n) worst case
    
    // Get load factor
    FUNCTION loadFactor()
        RETURN count / tableSize
    END FUNCTION
END CLASS

**Advantages of Separate Chaining:**

- Simple to implement.
    
- Table never "fills up" – you can always add more to a chain.
    
- Deletion is easy.
    
- Performance degrades gracefully as load factor increases.
    

**Disadvantages:**

- Extra memory for pointers.
    
- Cache performance is poor (nodes scattered in memory).
    
- If one chain gets too long, performance drops to O(n).
    

---

### 3.2 Collision Resolution #2: Open Addressing

**Idea:** Instead of storing a list at each index, we store **one key-value pair per slot**. When a collision occurs, we **probe** (search) for the next available slot using a specific pattern.

**The key difference:** Everything is stored **inside the table itself** – no linked lists.

**Visual Representation:**

Hash Table (size 10) with Open Addressing:
Insert "Alice" (hash=1):   [_, Alice, _, _, _, _, _, _, _, _]
Insert "Bob"   (hash=1):   [_, Alice, Bob, _, _, _, _, _, _, _]  ← Probing found slot 2
Insert "Eve"   (hash=1):   [_, Alice, Bob, Eve, _, _, _, _, _, _]  ← Probing found slot 3

When "Bob" collides with "Alice" at index 1, we probe forward to index 2. When "Eve" also hashes to 1, we probe to index 2 (occupied), then index 3 (free).

**There are three main probing strategies:**

#### (A) Linear Probing

nextIndex = (currentIndex + 1) % tableSize

Probe sequentially: `i, i+1, i+2, ...` (wrapping around).

**Problem:** **Primary Clustering** – consecutive occupied slots form long runs, making future collisions more likely and slowing down operations.

#### (B) Quadratic Probing

nextIndex = (hash(key) + i²) % tableSize    // i = 0, 1, 2, 3, ...

Probe: `h, h+1, h+4, h+9, h+16, ...`

**Problem:** **Secondary Clustering** – keys with the same initial hash follow the same probe sequence.

#### (C) Double Hashing

nextIndex = (hash1(key) + i * hash2(key)) % tableSize    // i = 0, 1, 2, ...

Use a **second hash function** to determine the step size.

**Advantage:** Eliminates both primary and secondary clustering. Best distribution.

**Pseudocode – Hash Table with Linear Probing:**

CLASS HashTableOpenAddressing
    DECLARE tableSize AS INTEGER
    DECLARE keys AS ARRAY
    DECLARE values AS ARRAY
    DECLARE occupied AS ARRAY OF BOOLEAN
    DECLARE count AS INTEGER
    
    PROCEDURE HashTableOpenAddressing(size)
        tableSize ← size
        keys ← NEW ARRAY[tableSize]
        values ← NEW ARRAY[tableSize]
        occupied ← NEW ARRAY OF BOOLEAN[tableSize]
        count ← 0
        
        FOR i ← 0 TO tableSize - 1
            occupied[i] ← FALSE
        END FOR
    END PROCEDURE
    
    // Insert using linear probing
    PROCEDURE insert(key, value)
        IF count >= tableSize THEN
            PRINT "Table is full!"
            RETURN
        END IF
        
        index ← hashFunction(key) % tableSize
        i ← 0
        
        // Probe until we find an empty slot or the key itself
        WHILE occupied[(index + i) % tableSize]
            IF keys[(index + i) % tableSize] = key THEN
                // Key exists – update value
                values[(index + i) % tableSize] ← value
                RETURN
            END IF
            i ← i + 1
        END WHILE
        
        // Found an empty slot
        slot ← (index + i) % tableSize
        keys[slot] ← key
        values[slot] ← value
        occupied[slot] ← TRUE
        count ← count + 1
    END PROCEDURE
    // Time Complexity: O(1) average, O(n) worst case
    
    // Search using linear probing
    FUNCTION search(key)
        index ← hashFunction(key) % tableSize
        i ← 0
        
        WHILE occupied[(index + i) % tableSize]
            IF keys[(index + i) % tableSize] = key THEN
                RETURN values[(index + i) % tableSize]
            END IF
            i ← i + 1
            
            IF i >= tableSize THEN
                BREAK   // We've probed the entire table
            END IF
        END WHILE
        
        RETURN NULL   // Key not found
    END FUNCTION
    // Time Complexity: O(1) average, O(n) worst case
    
    // Delete using linear probing (tricky!)
    PROCEDURE delete(key)
        index ← hashFunction(key) % tableSize
        i ← 0
        
        WHILE occupied[(index + i) % tableSize]
            IF keys[(index + i) % tableSize] = key THEN
                // Mark as deleted (use a "tombstone" flag)
                occupied[(index + i) % tableSize] ← FALSE
                count ← count - 1
                
                // Rehash all elements in the same cluster
                // (This is necessary to maintain probe chains)
                rehashCluster((index + i) % tableSize)
                RETURN
            END IF
            i ← i + 1
            
            IF i >= tableSize THEN
                BREAK
            END IF
        END WHILE
    END PROCEDURE
    // Deletion in open addressing is expensive – O(n) worst case
END CLASS

**Why is deletion tricky in open addressing?**  
If you simply mark a slot as empty, you break the probe chain for other keys. For example:

Insert A (hash=1) → slot 1
Insert B (hash=1) → slot 2 (probed)
Insert C (hash=1) → slot 3 (probed)
Now delete B: If we mark slot 2 as empty:
Search for C: hash=1 → slot 1 (A, not C) → slot 2 (empty, STOP!) → C not found! ❌

Solution: Use a **tombstone** (a special marker indicating "deleted but was occupied") or **rehash** the cluster.

---

### 3.3 Separate Chaining vs Open Addressing

|Feature|Separate Chaining|Open Addressing|
|---|---|---|
|**Storage**|Each slot points to a linked list|Each slot stores one key-value pair|
|**Memory Overhead**|Extra pointers for linked lists|No pointers, but wastes empty slots|
|**Load Factor**|Can exceed 1.0 (chains grow)|Must stay below 1.0 (table can fill up)|
|**Cache Performance**|Poor (nodes scattered)|Excellent (all in one array)|
|**Deletion**|Easy – just remove from list|Tricky – needs tombstones or rehashing|
|**Clustering**|No clustering|Clustering is a major issue|
|**Worst Case**|O(n) – all keys in one chain|O(n) – all keys in one probe sequence|
|**Best For**|Unknown number of elements, frequent deletions|Known size, cache-sensitive applications|

---

## 4. Load Factor – The Key Metric

The **Load Factor (α)** measures how full the hash table is:

Load Factor (α) = Number of Elements (n) / Table Size (m)

**Why it matters:**

- **Low load factor (α ≈ 0.1)**: Fast operations, but wastes memory.
    
- **High load factor (α ≈ 0.9)**: Memory efficient, but many collisions → slow operations.
    
- **Sweet spot**: Most implementations resize when α > 0.75.
    

**For Separate Chaining:**

- α can exceed 1.0 (chains grow longer).
    
- Average chain length = α.
    
- Search time = O(1 + α).
    

**For Open Addressing:**

- α must be < 1.0 (table can fill up).
    
- As α approaches 1.0, probe sequences become extremely long.
    
- Performance degrades **dramatically** when α > 0.7.
    

**Rule of Thumb:**

- Separate Chaining: Resize when α > 1.0 (or 0.75 for better performance).
    
- Open Addressing: Resize when α > 0.7.
    

---

## 5. Resizing (Rehashing) – Growing the Table

When the load factor gets too high, we need to **resize** the table. This involves:

1. Create a new array (usually **double** the size).
    
2. Recompute the hash for **every existing element** (because `hash % newSize` is different).
    
3. Insert each element into the new table.
    
4. Delete the old array.
    

**Pseudocode – Rehashing:**

PROCEDURE resize()
    oldTable ← table
    oldSize ← tableSize
    
    // Double the size (and ensure it's prime for better distribution)
    tableSize ← nextPrime(oldSize * 2)
    table ← NEW ARRAY[tableSize]
    count ← 0
    
    // Rehash all existing elements
    FOR i ← 0 TO oldSize - 1
        IF oldTable[i] IS NOT NULL THEN
            // For chaining: traverse the linked list
            current ← oldTable[i]
            WHILE current IS NOT NULL
                insert(current.key, current.value)
                current ← current.next
            END WHILE
        END IF
    END FOR
END PROCEDURE
// Time Complexity: O(n) – we rehash every element
// Amortized Time Complexity: O(1) per insertion

**Why double?**  
Doubling ensures that the amortized cost of resizing is O(1) per insertion. If you resized by adding only 1 slot each time, resizing would happen constantly, making insertions O(n).

**Why prime numbers?**  
Prime table sizes reduce clustering and improve hash distribution. Common choices: 7, 13, 31, 61, 127, 251, 509, 1021, 2039, 4093, 8191, ...

---

## 6. Time Complexity Summary

|Operation|Average Case|Worst Case|Notes|
|---|---|---|---|
|**Insert**|O(1)|O(n)|Worst case: all keys collide|
|**Search**|O(1)|O(n)|Worst case: all keys collide|
|**Delete**|O(1)|O(n)|Worst case: all keys collide|
|**Resize**|O(n)|O(n)|Amortized O(1) per insertion|

**Why is average case O(1)?**  
With a good hash function and a reasonable load factor, the expected number of probes/chains is a small constant (like 1.5 or 2). So operations are effectively constant time.

**Why is worst case O(n)?**  
If every key hashes to the same index, the hash table degenerates into a linked list (chaining) or a linear search (open addressing).

---

## 7. Handling Different Key Types

### 7.1 Integer Keys

Easy – just use the integer directly or apply modulo.

hash(key) = key % tableSize

### 7.2 String Keys

Use polynomial rolling hash (as shown earlier).

hash("hello") = (h * 31⁴ + e * 31³ + l * 31² + l * 31¹ + o * 31⁰) % tableSize

### 7.3 Custom Objects

You need to define a `hashCode()` method for your object. In Java, this is done by combining the hash codes of all fields.

**Pseudocode – Custom Object Hashing:**

FUNCTION hashCode(person)
    result ← 17   // Start with a prime
    result ← 31 * result + hash(person.firstName)
    result ← 31 * result + hash(person.lastName)
    result ← 31 * result + person.age
    RETURN result
END FUNCTION

**Important:** If two objects are equal (`equals()` returns true), they **must** have the same hash code. The reverse is not required (two different objects can have the same hash – that's a collision).

---

## 8. Hash Table vs Other Data Structures

|Feature|Hash Table|BST (Balanced)|Array (Sorted)|
|---|---|---|---|
|**Search**|O(1) average|O(log n)|O(log n)|
|**Insert**|O(1) average|O(log n)|O(n)|
|**Delete**|O(1) average|O(log n)|O(n)|
|**Ordered Traversal**|❌ Not supported|✅ In-order|✅ Sequential|
|**Min/Max**|❌ Not supported|✅ O(log n)|✅ O(1)|
|**Range Queries**|❌ Not supported|✅ O(log n + k)|✅ O(log n + k)|
|**Memory**|Moderate|Moderate|Low|

**When to use Hash Tables:**

- When you need **fast lookups** by key.
    
- When order doesn't matter.
    
- When you need to count frequencies (word counts, etc.).
    
- When implementing caches, sets, or dictionaries.
    

**When NOT to use Hash Tables:**

- When you need **sorted order** (use BST).
    
- When you need **range queries** (use BST).
    
- When you need **min/max** efficiently (use Heap).
    
- When memory is extremely tight (arrays are leaner).
    

---

## 9. Real-World Applications of Hash Tables

|Application|How Hash Tables Are Used|
|---|---|
|**Databases**|Indexing (hash indexes for equality lookups).|
|**Caches (Redis, Memcached)**|Key-value storage for fast retrieval.|
|**Compilers**|Symbol tables (variable names → types, scopes).|
|**Routers**|Routing tables (IP address → next hop).|
|**Spell Checkers**|Dictionary of valid words.|
|**Counting Frequencies**|Word counts, vote counts, inventory counts.|
|**Deduplication**|Detecting duplicate files/records (hash of content).|
|**Sets**|Implementing unordered sets (Python's `set`, Java's `HashSet`).|
|**Dictionaries**|Python's `dict`, JavaScript's `Map`, Java's `HashMap`.|
|**Caching**|LRU caches (combining hash table + doubly linked list).|
|**Cryptography**|Hash functions (SHA-256) for integrity verification.|
|**Blockchain**|Merkle trees use hashing to verify transactions.|

---

## 10. Classic Hash Table Problems (With Pseudocode)

### 10.1 Two Sum Problem

**Problem:** Given an array of integers and a target sum, find two numbers that add up to the target.

**Naive Solution:** O(n²) – check every pair.

**Hash Table Solution:** O(n) – for each number, check if `target - number` exists in the hash table.

**Pseudocode:**

FUNCTION twoSum(nums, target)
    DECLARE map AS NEW HashTable()
    
    FOR i ← 0 TO length(nums) - 1
        complement ← target - nums[i]
        
        IF map.search(complement) IS NOT NULL THEN
            RETURN [map.search(complement), i]
        END IF
        
        map.insert(nums[i], i)
    END FOR
    
    RETURN NULL   // No solution found
END FUNCTION
// Time Complexity: O(n)
// Space Complexity: O(n)

### 10.2 First Non-Repeating Character

**Problem:** Find the first character in a string that doesn't repeat.

**Pseudocode:**

FUNCTION firstNonRepeating(str)
    DECLARE freq AS NEW HashTable()
    
    // Count frequencies
    FOR EACH char IN str
        IF freq.search(char) IS NULL THEN
            freq.insert(char, 1)
        ELSE
            freq.insert(char, freq.search(char) + 1)
        END IF
    END FOR
    
    // Find first character with frequency 1
    FOR EACH char IN str
        IF freq.search(char) = 1 THEN
            RETURN char
        END IF
    END FOR
    
    RETURN NULL   // All characters repeat
END FUNCTION
// Time Complexity: O(n)

### 10.3 Group Anagrams

**Problem:** Given a list of strings, group anagrams together. (Anagrams are words with the same letters rearranged, like "listen" and "silent".)

**Key Insight:** Two anagrams have the same **sorted** string. Use the sorted string as the hash key!

**Pseudocode:**

FUNCTION groupAnagrams(words)
    DECLARE map AS NEW HashTable()
    
    FOR EACH word IN words
        sortedWord ← SORT(word)   // Sort characters alphabetically
        
        IF map.search(sortedWord) IS NULL THEN
            map.insert(sortedWord, NEW LIST())
        END IF
        
        map.search(sortedWord).add(word)
    END FOR
    
    RETURN map.values()
END FUNCTION
// Time Complexity: O(n * k log k) where k is the max word length

### 10.4 LRU Cache (Least Recently Used)

**Problem:** Design a cache that evicts the least recently used item when full.

**Key Insight:** Combine a **Hash Table** (for O(1) lookup) with a **Doubly Linked List** (for O(1) reordering).

**Pseudocode (Conceptual):**

CLASS LRUCache
    DECLARE capacity AS INTEGER
    DECLARE map AS NEW HashTable()          // key → node
    DECLARE head AS Node                     // Most recently used
    DECLARE tail AS Node                     // Least recently used
    
    FUNCTION get(key)
        IF map.search(key) IS NULL THEN
            RETURN NULL
        END IF
        
        node ← map.search(key)
        moveToFront(node)   // Mark as recently used
        RETURN node.value
    END FUNCTION
    
    PROCEDURE put(key, value)
        IF map.search(key) IS NOT NULL THEN
            node ← map.search(key)
            node.value ← value
            moveToFront(node)
        ELSE
            IF map.size() >= capacity THEN
                // Evict least recently used (tail)
                map.delete(tail.key)
                removeNode(tail)
            END IF
            
            newNode ← NEW Node(key, value)
            map.insert(key, newNode)
            addToFront(newNode)
        END IF
    END PROCEDURE
END CLASS
// All operations: O(1)

---

## 11. Summary – Your Hash Table Takeaway

- **Hash Tables** provide **O(1) average-case** insertion, deletion, and lookup – the fastest for key-value storage.
    
- **Hash Functions** convert keys into array indices. A good hash function is deterministic, fast, and uniformly distributed.
    
- **Collisions** are inevitable. Two main strategies:
    
    - **Separate Chaining**: Linked lists at each index. Easy deletion, but poor cache performance.
        
    - **Open Addressing**: Probing for the next free slot. Better cache performance, but tricky deletion and clustering issues.
        
- **Load Factor** (α = n/m) determines performance. Resize (rehash) when α exceeds ~0.75.
    
- **Resizing** doubles the table size and rehashes all elements. Amortized O(1) per insertion.
    
- **Worst Case** is O(n) when all keys collide – but a good hash function makes this extremely rare.
    
- **Hash Tables** don't maintain order. Use a BST if you need sorted data or range queries.
    
- **Real-world applications** are everywhere: databases, caches, compilers, routers, spell checkers, and more.
    

**The Big Secret:** Hash tables are the **engine of modern computing**. Every time you use a dictionary, a cache, or a database index, you're relying on hashing. Mastering hash tables means understanding the core of efficient data retrieval.
