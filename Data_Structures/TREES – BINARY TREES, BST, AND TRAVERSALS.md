
## 1. Introduction – Why Trees?

So far, we've only dealt with **linear** data structures – Arrays, Linked Lists, Stacks, and Queues. Everything has been in a straight line. But the real world isn't linear.

Think about:

- A **company's organizational chart** (CEO → Managers → Employees)
    
- A **computer's file system** (Root → Folders → Subfolders → Files)
    
- An **HTML document** (html → head/body → divs → paragraphs)
    
- A **chess game's decision tree** (each move branches into possibilities)
    

All of these are **hierarchical** – one thing leads to many others. You can't represent this with a simple list. This is exactly why we need **Trees**.

A **Tree** is a **non-linear**, **hierarchical** data structure consisting of **nodes** connected by **edges**. It has a **root** node and every other node has exactly one **parent**, except the root. Nodes can have zero or more **children**.

---

## 2. Tree Terminology – The Lingo You Must Know

Before we dive deeper, let's lock down the vocabulary. This is **non-negotiable** – you'll use these words constantly.

|Term|Definition|Example (from diagram below)|
|---|---|---|
|**Root**|The topmost node of the tree. Only one root exists.|A|
|**Parent**|A node that has branches to other nodes below it.|A is parent of B, C|
|**Child**|A node that is directly connected to a node above it.|B, C are children of A|
|**Siblings**|Nodes that share the same parent.|B and C are siblings|
|**Leaf**|A node with **no children**. (Also called external nodes)|D, E, F, G are leaves|
|**Internal Node**|A node with at least one child.|A, B, C|
|**Ancestor**|Any node on the path from root to a given node.|Ancestors of E: A, B|
|**Descendant**|Any node below a given node in the tree.|Descendants of B: D, E|
|**Depth**|The number of edges from the root to a given node.|Depth of E = 2 (A→B→E)|
|**Height**|The number of edges on the longest path from a node to a leaf.|Height of B = 1; Height of A = 2|
|**Subtree**|Any node and all its descendants.|B, D, E form a subtree|
|**Level**|The depth + 1 (root is at level 1).|Root = Level 1, B/C = Level 2|

**Visual Reference:**

                    ┌───────────┐
                    │     A      │ ← Root
                    │  (Root)    │
                    └─────┬─────┘
                          │
              ┌───────────┼───────────┐
              │           │           │
          ┌───▼───┐   ┌───▼───┐   ┌───▼───┐
          │   B   │   │   C   │   │   D   │
          └───┬───┘   └───────┘   └───────┘
              │
          ┌───┼───┐
          │   │   │
       ┌──▼──┐ ┌─▼───┐
       │  E  │ │  F  │
       └─────┘ └─────┘

**Check your understanding:**

- Root? → A
    
- Leaves? → D, E, F (C is a leaf too in this diagram)
    
- Parent of E? → B
    
- Siblings of E? → F
    
- Depth of F? → 2
    
- Height of the tree? → 2
    

---

## 3. Binary Trees – The Foundation

### 3.1 What is a Binary Tree?

A **Binary Tree** is a tree where **each node has at most TWO children**. These children are typically called the **left child** and the **right child**.

**Definition:**

Each node in a Binary Tree contains:
1. Data
2. A pointer/reference to the left child
3. A pointer/reference to the right child

**Node Structure (Pseudocode):**

CLASS BTNode
    DATA value AS INTEGER
    LEFT AS BTNode    // Pointer to the left child
    RIGHT AS BTNode   // Pointer to the right child
END CLASS

**Visual Representation:**

        ┌───────┐
        │   1   │
        └───┬───┘
        ┌───┴───┐
        │       │
    ┌───▼───┐ ┌─▼─────┐
    │   2   │ │   3   │
    └───┬───┘ └───────┘
    ┌───┴───┐
    │       │
┌───▼──┐ ┌─▼────┐
│   4        │ │   5       │
└───────┘  └──────┘

Notice: Node 2 has two children (4 and 5). Node 3 has none (leaf). Node 1 has two children. Maximum children per node = 2.

### 3.2 Types of Binary Trees

Not all binary trees are the same. Here are the important variations:

|Type|Definition|Example Use|
|---|---|---|
|**Full Binary Tree**|Every node has **either 0 or 2 children**. No node has exactly 1 child.|Perfect for representing binary expressions|
|**Complete Binary Tree**|All levels are completely filled **except possibly the last**, and the last level is filled from left to right.|Used in **Heap** data structure (coming soon!)|
|**Perfect Binary Tree**|All internal nodes have 2 children, and all leaves are at the **same depth**.|The most balanced form – rarely perfect in practice|
|**Balanced Binary Tree**|The heights of the left and right subtrees differ by at most 1 (for **every** node).|AVL Trees, Red-Black Trees – efficient search|
|**Skewed Binary Tree**|Every node has **exactly 1 child** – either all left or all right.|Degenerate case – essentially a linked list (bad for performance!)|

**Visual Examples:**

**Full Binary Tree:**

        ┌───┐
        │ 1 │
        └─┬─┘
       ┌──┴──┐
       │     │
    ┌──▼──┐ ┌▼────┐
    │  2  │ │  3  │
    └──┬──┘ └──────┘
   ┌───┴───┐
   │             │
┌──▼──┐ ┌─▼─┐
│  4      │ │  5    │
└─────┘ └─────┘

All nodes have 0 or 2 children. ✅

**Complete Binary Tree:**


        ┌───┐
        │ 1 │
        └─┬─┘
       ┌──┴──┐
       │     │
    ┌──▼──┐ ┌▼────┐
    │  2  │ │  3  │
    └──┬──┘ └─────┘
   ┌───┴───┐
   │             │
┌──▼──┐ ┌─▼──┐
│  4       │ │  5    │
└─────┘  └─────┘

All levels filled except the last, which is left-filled. ✅

**Skewed Binary Tree (Right-Skewed):**

┌───┐
│ 1  │
└─┬─┘
  │
┌─▼─┐
│ 2    │
└─┬──┘
  │
┌─▼─┐
│ 3    │
└────┘

This is just a linked list! Operations on this tree will be O(n), not O(log n).

---

## 4. Binary Search Tree (BST) – The Game Changer

### 4.1 What Makes a BST Special?

A **Binary Search Tree (BST)** is a binary tree with a **superpower** – it maintains an **order** that allows for **efficient searching**, **insertion**, and **deletion**.

**The BST Property (The Golden Rule):**

For EVERY node in a BST:
- All values in the LEFT subtree are SMALLER than the node's value.
- All values in the RIGHT subtree are GREATER than the node's value.
- Both left and right subtrees are also BSTs.

**Visual Example:**

        ┌───────┐
        │   50  │
        └───┬───┘
        ┌───┴───┐
        │       │
    ┌───▼───┐ ┌─▼─────┐
    │   30  │ │  70   │
    └───┬───┘ └───┬───┘
    ┌───┴───┐ ┌───┴───┐
    │       │ │       │
┌───▼───┐ ┌─▼─────┐ ┌─▼─────┐
│  20        │ │  40       │ │  60      │ │  80   │
└───────┘ └───────┘ └───────┘ └───────┘

**Check it:**

- Left of 50: 30, 20, 40 – all smaller than 50 ✅
    
- Right of 50: 70, 60, 80 – all greater than 50 ✅
    
- Left of 30: 20 – smaller than 30 ✅
    
- Right of 30: 40 – greater than 30 ✅
    
- Left of 70: 60 – smaller than 70 ✅
    
- Right of 70: 80 – greater than 70 ✅
    

This ordering is **the key** to everything.

### 4.2 Why is BST So Powerful? – The Search Magic

The BST property enables **Binary Search** on a tree!

**Search Algorithm (Pseudocode):**

PROCEDURE search(root, target)
    IF root IS NULL THEN
        RETURN NULL    // Target not found
    END IF
    
    IF target = root.value THEN
        RETURN root    // Found it!
    END IF
    
    IF target < root.value THEN
        RETURN search(root.left, target)   // Go left (smaller values)
    ELSE
        RETURN search(root.right, target)  // Go right (larger values)
    END IF
END PROCEDURE
// Time Complexity: O(log n) in a balanced BST, O(n) in a skewed BST

**Why is this fast?**  
At each step, you eliminate **half** of the remaining tree!

- If you're looking for 40 in the tree above:
    
    - 40 < 50 → Go left (ignore everything in the right subtree – 70, 60, 80)
        
    - 40 > 30 → Go right (ignore the left subtree of 30 – 20)
        
    - 40 = 40 → Found it!
        
- You only visited 3 nodes (50, 30, 40) out of 7. That's the power of O(log n)!
    

**Compare with an Array:**

- Searching in an unsorted array = O(n) (linear search)
    
- Searching in a sorted array = O(log n) (binary search)
    
- Searching in a BST = O(log n) on average – but in a **skewed BST**, it's O(n) (worst case).
    

This is why **balancing** a BST is so important (we'll cover AVL and Red-Black trees later).

### 4.3 Insertion into a BST

Insertion follows the **same logic** as search – we just find the correct spot and place the new node.

**Insertion Algorithm (Pseudocode):**

PROCEDURE insert(root, value)
    // Base case: empty spot found
    IF root IS NULL THEN
        newNode ← NEW BTNode()
        newNode.value ← value
        newNode.left ← NULL
        newNode.right ← NULL
        RETURN newNode
    END IF
    
    // Recursively find the correct position
    IF value < root.value THEN
        root.left ← insert(root.left, value)   // Go left
    ELSE IF value > root.value THEN
        root.right ← insert(root.right, value) // Go right
    ELSE
        // Value already exists – handle duplicate (ignore or count)
        PRINT "Duplicate value! Not inserting."
        RETURN root
    END IF
    
    RETURN root
END PROCEDURE
// Time Complexity: O(log n) in a balanced BST, O(n) in a skewed BST

**Trace Insertion Example:**  
Let's build a BST by inserting: [50, 30, 70, 20, 40, 60, 80]

1. Insert 50 → Root
    


    [50]

2. Insert 30 → 30 < 50, go left
    


    [50]
    /
  [30]

3. Insert 70 → 70 > 50, go right
    


    [50]
    /  \
  [30] [70]

4. Insert 20 → 20 < 50 → 20 < 30, go left
    


    [50]
    /  \
  [30] [70]
  /
[20]

5. Insert 40 → 40 < 50 → 40 > 30, go right
    


    [50]
    /  \
  [30] [70]
  /  \
[20] [40]

6. Insert 60 → 60 > 50 → 60 < 70, go left
    


    [50]
    /  \
  [30] [70]
  /  \  /
[20] [40][60]

7. Insert 80 → 80 > 50 → 80 > 70, go right
    


    [50]
    /  \
  [30] [70]
  /  \  /  \
[20] [40][60][80]

### 4.4 Deletion from a BST (The Tricky One)

Deletion is the **most complex** operation in a BST because of three different cases.

**Case 1: Deleting a LEAF Node**  
Simply remove it.


    [50]                    [50]
    /  \      Delete 20     /  \
  [30] [70]    →           [30] [70]
  /  \                    /  \
[20] [40]               [40]

**Case 2: Deleting a Node with ONE Child**  
Replace the node with its child.


    [50]                    [50]
    /  \      Delete 30    /  \
  [30] [70]    →          [40] [70]
    \                      (40 moves up)
   [40]

**Case 3: Deleting a Node with TWO Children**  
Find the **in-order successor** (the smallest value in the right subtree) OR the **in-order predecessor** (the largest value in the left subtree). Replace the node with that value, then delete that successor/predecessor.


        [50]                          [60]
        /  \      Delete 50           /  \
      [30] [70]    →                [30] [70]
      /  \  /                       /  \
    [20] [40][60]                 [20] [40]

We found the in-order successor (60 – the smallest in the right subtree), replaced 50 with it, and then deleted the leaf node 60.

**Deletion Algorithm (Pseudocode) – The Full Version:**


PROCEDURE deleteNode(root, value)
    // Base case: tree is empty
    IF root IS NULL THEN
        RETURN NULL
    END IF
    
    // Search for the node to delete
    IF value < root.value THEN
        root.left ← deleteNode(root.left, value)
    ELSE IF value > root.value THEN
        root.right ← deleteNode(root.right, value)
    ELSE
        // FOUND THE NODE TO DELETE!
        
        // Case 1: Leaf node
        IF root.left IS NULL AND root.right IS NULL THEN
            DELETE root
            RETURN NULL
        END IF
        
        // Case 2: Node with one child
        IF root.left IS NULL THEN
            temp ← root.right
            DELETE root
            RETURN temp
        END IF
        
        IF root.right IS NULL THEN
            temp ← root.left
            DELETE root
            RETURN temp
        END IF
        
        // Case 3: Node with two children
        // Find the in-order successor (minimum in right subtree)
        successor ← findMin(root.right)
        
        // Replace root's value with successor's value
        root.value ← successor.value
        
        // Delete the successor from the right subtree
        root.right ← deleteNode(root.right, successor.value)
    END IF
    
    RETURN root
END PROCEDURE
// Helper: Find minimum value in a subtree
PROCEDURE findMin(node)
    current ← node
    WHILE current.left IS NOT NULL
        current ← current.left
    END WHILE
    RETURN current
END PROCEDURE
// Time Complexity: O(log n) in a balanced BST, O(n) in a skewed BST

### 4.5 Time Complexity Summary for BST Operations

|Operation|Balanced BST|Skewed BST (Worst Case)|
|---|---|---|
|**Search**|O(log n)|O(n)|
|**Insert**|O(log n)|O(n)|
|**Delete**|O(log n)|O(n)|
|**Traversal**|O(n) for all|O(n) for all|

The **height** of the tree determines the performance. A balanced tree has height O(log n), while a skewed tree has height O(n).

---

## 5. Tree Traversals – How to Visit Every Node

Traversal means **visiting every node** in the tree exactly once. Unlike arrays where you just go from 0 to n-1, trees give you **different orders** depending on when you visit the root relative to the children.

### 5.1 Depth-First Search (DFS) Traversals

DFS explores as **deep as possible** along each branch before backtracking.

**Three Types of DFS Traversal:**

#### (A) In-Order Traversal (Left → Root → Right)

**The Rule:** Visit left subtree, then root, then right subtree.

**Algorithm (Pseudocode):**

PROCEDURE inOrder(node)
    IF node IS NOT NULL THEN
        inOrder(node.left)       // Step 1: Left
        PRINT node.value         // Step 2: Root
        inOrder(node.right)      // Step 3: Right
    END IF
END PROCEDURE
// Time Complexity: O(n)
// Space Complexity: O(h) where h is the height (recursion stack)

**Visual Trace:**

        [50]
        /  \
      [30] [70]
      /  \  /  \
    [20] [40][60][80]
In-Order: 20, 30, 40, 50, 60, 70, 80

**Key Insight:** For a BST, **In-Order traversal gives you a SORTED list!** This is incredibly useful.

#### (B) Pre-Order Traversal (Root → Left → Right)

**The Rule:** Visit root, then left subtree, then right subtree.

**Algorithm (Pseudocode):**

PROCEDURE preOrder(node)
    IF node IS NOT NULL THEN
        PRINT node.value         // Step 1: Root
        preOrder(node.left)      // Step 2: Left
        preOrder(node.right)     // Step 3: Right
    END IF
END PROCEDURE
// Time Complexity: O(n)
// Space Complexity: O(h)

**Visual Trace:**

        [50]
        /  \
      [30] [70]
      /  \  /  \
    [20] [40][60][80]
Pre-Order: 50, 30, 20, 40, 70, 60, 80

**Key Insight:** Pre-Order is useful for **serializing** a tree (saving to file) or **creating a copy** of the tree.

#### (C) Post-Order Traversal (Left → Right → Root)

**The Rule:** Visit left subtree, then right subtree, then root.

**Algorithm (Pseudocode):**

PROCEDURE postOrder(node)
    IF node IS NOT NULL THEN
        postOrder(node.left)     // Step 1: Left
        postOrder(node.right)    // Step 2: Right
        PRINT node.value         // Step 3: Root
    END IF
END PROCEDURE
// Time Complexity: O(n)
// Space Complexity: O(h)

**Visual Trace:**

        [50]
        /  \
      [30] [70]
      /  \  /  \
    [20] [40][60][80]
Post-Order: 20, 40, 30, 60, 80, 70, 50

**Key Insight:** Post-Order is useful for **deleting** a tree (delete children before parent) or calculating the **size of the tree**.

### 5.2 Summary of DFS Traversals

|Traversal|Order|Use Case|
|---|---|---|
|**In-Order**|Left → Root → Right|Sorted output (BST), expression evaluation (infix)|
|**Pre-Order**|Root → Left → Right|Tree serialization, copying, prefix expression|
|**Post-Order**|Left → Right → Root|Tree deletion, calculating sizes/height, postfix expression|

**Expression Tree Example:**  
Consider the expression: `(5 + 3) * 2`

        [*]
        /  \
      [+]  [2]
      /  \
    [5]  [3]

- **In-Order:** 5 + 3 * 2 → Infix notation (with parentheses issues!)
    
- **Pre-Order:** * + 5 3 2 → Prefix notation
    
- **Post-Order:** 5 3 + 2 * → Postfix notation (easy for stack evaluation!)
    

### 5.3 Breadth-First Search (BFS) – Level-Order Traversal

BFS explores the tree **level by level**. It visits all nodes at depth 0, then depth 1, then depth 2, and so on.

**Visual Trace:**

        [50]          Level 0
        /  \
      [30] [70]       Level 1
      /  \  /  \
    [20] [40][60][80] Level 2
Level-Order: 50, 30, 70, 20, 40, 60, 80

**Algorithm (Pseudocode) – Using a Queue:**

PROCEDURE levelOrder(root)
    IF root IS NULL THEN
        RETURN
    END IF
    
    DECLARE queue AS NEW Queue()
    queue.enqueue(root)
    
    WHILE NOT queue.isEmpty()
        current ← queue.dequeue()
        PRINT current.value
        
        // Enqueue children from left to right
        IF current.left IS NOT NULL THEN
            queue.enqueue(current.left)
        END IF
        
        IF current.right IS NOT NULL THEN
            queue.enqueue(current.right)
        END IF
    END WHILE
END PROCEDURE
// Time Complexity: O(n)
// Space Complexity: O(w) where w is the maximum width of the tree

**Key Insight:** BFS is perfect for finding the **shortest path** in unweighted trees/graphs and for problems where you need the **closest** node.

---

## 6. Important Tree Problems (With Pseudocode)

### 6.1 Height of a Binary Tree

The height is the number of edges on the longest path from root to a leaf.

**Algorithm (Pseudocode):**

PROCEDURE height(node)
    IF node IS NULL THEN
        RETURN -1    // Empty tree has height -1 (or 0 depending on definition)
    END IF
    
    leftHeight ← height(node.left)
    rightHeight ← height(node.right)
    
    RETURN 1 + MAX(leftHeight, rightHeight)
END PROCEDURE
// Time Complexity: O(n) – we visit every node
// Space Complexity: O(h) – recursion stack

### 6.2 Count of Nodes (Size of Tree)

**Algorithm (Pseudocode):**

PROCEDURE countNodes(node)
    IF node IS NULL THEN
        RETURN 0
    END IF
    
    RETURN 1 + countNodes(node.left) + countNodes(node.right)
END PROCEDURE
// Time Complexity: O(n)

### 6.3 Count of Leaves

**Algorithm (Pseudocode):**

PROCEDURE countLeaves(node)
    IF node IS NULL THEN
        RETURN 0
    END IF
    
    // If both children are NULL, it's a leaf
    IF node.left IS NULL AND node.right IS NULL THEN
        RETURN 1
    END IF
    
    RETURN countLeaves(node.left) + countLeaves(node.right)
END PROCEDURE
// Time Complexity: O(n)

### 6.4 Check if Two Trees are Identical

**Algorithm (Pseudocode):**

PROCEDURE isIdentical(node1, node2)
    // Both NULL → identical
    IF node1 IS NULL AND node2 IS NULL THEN
        RETURN TRUE
    END IF
    
    // One NULL, other not → not identical
    IF node1 IS NULL OR node2 IS NULL THEN
        RETURN FALSE
    END IF
    
    // Check value and recursively check left and right subtrees
    RETURN (node1.value = node2.value) AND
           isIdentical(node1.left, node2.left) AND
           isIdentical(node1.right, node2.right)
END PROCEDURE
// Time Complexity: O(min(n1, n2))

### 6.5 Check if a Binary Tree is a BST

This is a classic interview question! We need to check that EVERY node follows the BST property.

**Algorithm (Pseudocode) – Using Bounds:**

PROCEDURE isBST(node, minValue, maxValue)
    // Empty tree is a BST
    IF node IS NULL THEN
        RETURN TRUE
    END IF
    
    // Check current node's value is within bounds
    IF node.value <= minValue OR node.value >= maxValue THEN
        RETURN FALSE
    END IF
    
    // Recursively check left and right subtrees with updated bounds
    RETURN isBST(node.left, minValue, node.value) AND
           isBST(node.right, node.value, maxValue)
END PROCEDURE
// Initial call: isBST(root, -∞, +∞)
// Time Complexity: O(n)

### 6.6 Find the Lowest Common Ancestor (LCA) in a BST

The LCA of two nodes is the deepest node that is an ancestor of both.

**Algorithm (Pseudocode) – Optimized for BST:**

PROCEDURE findLCA_BST(root, node1, node2)
    IF root IS NULL THEN
        RETURN NULL
    END IF
    
    // If both nodes are smaller, LCA is in the left subtree
    IF node1.value < root.value AND node2.value < root.value THEN
        RETURN findLCA_BST(root.left, node1, node2)
    END IF
    
    // If both nodes are greater, LCA is in the right subtree
    IF node1.value > root.value AND node2.value > root.value THEN
        RETURN findLCA_BST(root.right, node1, node2)
    END IF
    
    // If one is smaller and the other is greater, current root is the LCA
    RETURN root
END PROCEDURE
// Time Complexity: O(log n) in a balanced BST

---

## 7. Array Representation of Binary Trees

You can represent a complete binary tree using an array – this is how **Heaps** work!

**The Rules:**

- Root is at index 0.
    
- For a node at index `i`:
    
    - Left child: `2*i + 1`
        
    - Right child: `2*i + 2`
        
    - Parent: `(i - 1) / 2` (integer division)
        

**Visual Example:**

Array: [50, 30, 70, 20, 40, 60, 80]
Index:  0    1    2    3    4    5    6
        [50]        ← Root (index 0)
        /  \
      [30] [70]     ← Children of 0: 1, 2
      /  \  /  \
    [20] [40][60][80] ← Children of 1: 3, 4; Children of 2: 5, 6

**Why is this useful?**

- **No pointers** needed – saves memory.
    
- **Cache-friendly** – contiguous memory.
    
- Perfect for **Heaps** and **Priority Queues**.
    

---

## 8. Real-World Applications of Trees

|Application|How Trees Are Used|
|---|---|
|**File Systems**|Hierarchical directory structure.|
|**HTML/XML Parsing**|DOM trees represent the structure of a webpage.|
|**Databases**|B-Trees and B+ Trees are used for indexing (efficient search, insert, delete on disk).|
|**Compilers**|Abstract Syntax Trees (AST) represent the structure of code.|
|**Routing Algorithms**|Network routing uses trees to find the best path.|
|**Artificial Intelligence**|Game trees (chess, tic-tac-toe) for decision-making.|
|**Priority Queues**|Implemented using Heaps (a special type of complete binary tree).|
|**Autocomplete / Spell Check**|Tries (prefix trees) for efficient word lookup.|

---

## 9. Summary – Your Tree Takeaway

- **Trees** are **non-linear**, **hierarchical** data structures used to represent relationships.
    
- **Binary Trees** restrict each node to **at most 2 children**.
    
- **BSTs** maintain an **ordering property** (left < root < right) enabling O(log n) search, insert, and delete in balanced trees.
    
- The **height** of a tree determines its performance. **Skewed** trees become O(n) – this is why **balancing** matters.
    
- **Traversals** allow you to visit all nodes:
    
    - **DFS**: In-Order (sorted BST), Pre-Order (serialization), Post-Order (deletion).
        
    - **BFS**: Level-Order (shortest path, closest node).
        
- **Many problems** – height, count, LCA, BST validation – are solved with simple recursion.
    
- Trees are **everywhere** – from file systems to databases to AI. You cannot escape them!
    

**The Big Secret:** Once you deeply understand trees, you unlock the ability to understand **graphs**, **heaps**, **tries**, and **advanced tree structures** (AVL, Red-Black). This is the turning point in your DSA journey!
