
## 1. Introduction – What Makes These Special?

So far, we've looked at **Arrays** and **Linked Lists** – data structures that give you **full freedom**. You can access any element, insert anywhere, delete from anywhere. They're like a messy desk – everything is there, but you have to organize it yourself.

**Stacks** and **Queues** are different. They are **restricted data structures**. They say: _"You can only interact with us in one specific way, and we enforce that rule strictly."_

Why would anyone want restrictions? Because **restrictions bring structure**, and structure brings **predictability**, which leads to **cleaner, safer code**. When you use a stack or a queue, you're telling anyone reading your code: _"This is exactly how this data is meant to be used – no funny business."_

These two structures are everywhere in computer science – from function calls in your code to CPU scheduling, from undo/redo in text editors to managing print jobs. Let's break them down.

---

## 2. The Stack – LIFO (Last In, First Out)

### 2.1 What Exactly is a Stack?

A **stack** is a linear data structure that follows the **LIFO** principle: **Last In, First Out**. Think of it as a **stack of plates** in a cafeteria:

- You can only add (push) a new plate on **top**.
    
- You can only remove (pop) a plate from the **top**.
    
- You cannot grab a plate from the middle or bottom.
    

**Visual Representation:**

        Push (Add) →            Pop (Remove) ←
                │                    │
                ▼                    ▼
          ┌─────────┐
          │  Item 3 │ ← Top (most recently added)
          ├─────────┤
          │  Item 2 │
          ├─────────┤
          │  Item 1 │ ← Bottom (first added)
          └─────────┘

### 2.2 Key Characteristics of a Stack

1. **LIFO Behavior**: The last element inserted is the first one to be removed.
    
2. **Limited Access**: You can only interact with the **top** element.
    
3. **Dynamic or Static**: Can be implemented using either arrays (fixed size) or linked lists (dynamic).
    
4. **Simple Operations**: Only a handful of core operations – `push`, `pop`, `peek/top`, `isEmpty`.
    

### 2.3 Core Operations

|Operation|Description|Time Complexity|
|---|---|---|
|**push(value)**|Adds an element to the **top** of the stack|O(1)|
|**pop()**|Removes and returns the element from the **top**|O(1)|
|**peek() / top()**|Returns the top element **without** removing it|O(1)|
|**isEmpty()**|Checks if the stack has no elements|O(1)|
|**size()**|Returns the number of elements in the stack|O(1)|

### 2.4 Stack Pseudocode (Using Arrays – Static Implementation)

Here's the simplest way to understand a stack – using an array with a `top` pointer that tracks the index of the last element.

**Initialization:**

DECLARE stack[MAX_SIZE] AS INTEGER
DECLARE top ← -1    // -1 means the stack is empty

**Push Operation – Pseudocode:**

PROCEDURE push(value)
    // Check for overflow (stack is full)
    IF top = MAX_SIZE - 1 THEN
        PRINT "Stack Overflow! Cannot push."
        RETURN
    END IF
    
    // Increment top and add the value
    top ← top + 1
    stack[top] ← value
    
    PRINT value + " pushed to stack"
END PROCEDURE
// Time Complexity: O(1)

**Pop Operation – Pseudocode:**

PROCEDURE pop()
    // Check for underflow (stack is empty)
    IF top = -1 THEN
        PRINT "Stack Underflow! Cannot pop."
        RETURN NULL
    END IF
    
    // Retrieve the top value and decrement top
    value ← stack[top]
    top ← top - 1
    
    RETURN value
END PROCEDURE
// Time Complexity: O(1)

**Peek Operation – Pseudocode:**

PROCEDURE peek()
    IF top = -1 THEN
        PRINT "Stack is empty."
        RETURN NULL
    END IF
    
    RETURN stack[top]
END PROCEDURE
// Time Complexity: O(1)

**Check if Empty – Pseudocode:**

FUNCTION isEmpty()
    RETURN top = -1
END FUNCTION
// Time Complexity: O(1)

### 2.5 Stack Pseudocode (Using Linked Lists – Dynamic Implementation)

Using a linked list gives us **unlimited size** (no overflow unless we run out of memory entirely). The **head** of the linked list becomes the **top** of the stack.

**Node Definition (Same as before):**


CLASS Node
    DATA value AS INTEGER
    NEXT AS Node
END CLASS

**Push Operation – Pseudocode (Linked List):**


PROCEDURE push(head, value)
    newNode ← NEW Node()
    newNode.value ← value
    
    // New node points to the old top
    newNode.next ← head
    
    // Update head to be the new node
    head ← newNode
    
    RETURN head
END PROCEDURE
// Time Complexity: O(1) – no traversal needed!

**Pop Operation – Pseudocode (Linked List):**


PROCEDURE pop(head)
    IF head IS NULL THEN
        PRINT "Stack Underflow! Cannot pop."
        RETURN NULL, head   // Return NULL value and unchanged head
    END IF
    
    // Store the value to return
    value ← head.value
    
    // Move head to the next node (removing the top)
    temp ← head
    head ← head.next
    
    // Delete the old top (optional)
    DELETE temp
    
    RETURN value, head
END PROCEDURE
// Time Complexity: O(1) – just one pointer change!

### 2.6 Why Are All Stack Operations O(1)?

This is **crucial** to understand. Every operation in a stack – whether implemented with an array or a linked list – is **O(1) constant time**. Why?

- **Array version**: You always work at the end (top). `top` is an index variable. Incrementing/decrementing an integer and accessing `stack[top]` is instant.
    
- **Linked List version**: You always work at the head. Inserting at the head or removing from the head is just changing one or two pointers. No traversal needed.
    

This constant-time performance is why stacks are used in performance-critical areas like function call management.

### 2.7 Real-World Applications of Stacks

|Application|How It Works|
|---|---|
|**Function Call Stack**|When you call a function, its local variables and return address are pushed onto the call stack. When the function returns, everything is popped off. This is why recursion works!|
|**Undo/Redo in Editors**|Each action (typing, deleting, formatting) is pushed onto an "undo" stack. Pressing Ctrl+Z pops the last action and reverses it.|
|**Expression Evaluation**|Compilers use stacks to evaluate mathematical expressions (infix to postfix conversion, evaluating postfix expressions).|
|**Backtracking Algorithms**|Maze solving, pathfinding, and puzzle solving use stacks to remember which paths have been tried.|
|**Browser History**|When you visit a new page, it's pushed onto the history stack. The "Back" button pops the current page and goes to the previous one.|
|**Syntax Parsing**|Checking balanced parentheses `( [ { } ] )` – when you see an opening bracket, push it; when you see a closing bracket, pop and check if they match.|

**Balanced Parentheses – Pseudocode (Classic Stack Problem):**


FUNCTION isBalanced(expression)
    DECLARE stack AS NEW Stack()    // Using our stack from above
    
    FOR EACH char IN expression
        // If opening bracket, push it
        IF char = '(' OR char = '[' OR char = '{' THEN
            stack.push(char)
        END IF
        
        // If closing bracket, check if it matches the top
        IF char = ')' OR char = ']' OR char = '}' THEN
            IF stack.isEmpty() THEN
                RETURN FALSE    // No opening bracket to match
            END IF
            
            top ← stack.pop()
            
            IF (char = ')' AND top ≠ '(') OR
               (char = ']' AND top ≠ '[') OR
               (char = '}' AND top ≠ '{') THEN
                RETURN FALSE    // Mismatched brackets
            END IF
        END IF
    END FOR
    
    // If stack is empty, all brackets matched correctly
    RETURN stack.isEmpty()
END FUNCTION
// Time Complexity: O(n) – we scan the expression once

---

## 3. The Queue – FIFO (First In, First Out)

### 3.1 What Exactly is a Queue?

A **queue** is a linear data structure that follows the **FIFO** principle: **First In, First Out**. Think of a **queue (line) of people** at a ticket counter:

- New people join (enqueue) at the **back** of the line.
    
- People are served (dequeue) from the **front** of the line.
    
- No cutting in line – the person who has been waiting the longest gets served first.
    

**Visual Representation:**

text

  Dequeue (Remove from front) ←
                │
                ▼
          ┌─────────┐
    Front │  Item 1     │ ← First added, first to leave
          ├─────────┤
          │  Item 2    │
          ├─────────┤
          │  Item 3    │
          ├─────────┤
     Back │  Item 4     │ ← Most recently added
          └─────────┘
                ▲
                │
  Enqueue (Add to back) →

### 3.2 Key Characteristics of a Queue

1. **FIFO Behavior**: The first element inserted is the first one to be removed.
    
2. **Two Ends**: We add at the **back (rear)** and remove from the **front (head)**.
    
3. **Limited Access**: You can only interact with the front (for removal) and the back (for insertion).
    
4. **Widely Used**: Essential in scheduling, buffering, and breadth-first algorithms.
    

### 3.3 Core Operations

|Operation|Description|Time Complexity|
|---|---|---|
|**enqueue(value)**|Adds an element to the **back** of the queue|O(1)|
|**dequeue()**|Removes and returns the element from the **front**|O(1)|
|**front() / peek()**|Returns the front element **without** removing it|O(1)|
|**isEmpty()**|Checks if the queue has no elements|O(1)|
|**size()**|Returns the number of elements in the queue|O(1)|

### 3.4 Queue Pseudocode (Using Linked Lists – The Simpler Way)

Implementing a queue with a linked list is cleaner because we just maintain two pointers: `front` and `rear`.

**Node Definition:**

CLASS Node
    DATA value AS INTEGER
    NEXT AS Node
END CLASS

**Initialization:**

front ← NULL    // Points to the first node (where we dequeue)
rear ← NULL     // Points to the last node (where we enqueue)

**Enqueue Operation – Pseudocode:**

PROCEDURE enqueue(value)
    newNode ← NEW Node()
    newNode.value ← value
    newNode.next ← NULL
    
    // If the queue is empty, both front and rear point to the new node
    IF rear IS NULL THEN
        front ← newNode
        rear ← newNode
    ELSE
        // Add new node at the end
        rear.next ← newNode
        rear ← newNode   // Update rear to the new last node
    END IF
    
    PRINT value + " enqueued to queue"
END PROCEDURE
// Time Complexity: O(1) – we always work at the rear

**Dequeue Operation – Pseudocode:**

PROCEDURE dequeue()
    // Check if the queue is empty
    IF front IS NULL THEN
        PRINT "Queue Underflow! Cannot dequeue."
        RETURN NULL
    END IF
    
    // Store the value to return
    value ← front.value
    
    // Move front to the next node
    temp ← front
    front ← front.next
    
    // If the queue becomes empty, also set rear to NULL
    IF front IS NULL THEN
        rear ← NULL
    END IF
    
    // Delete the old front node (optional)
    DELETE temp
    
    RETURN value
END PROCEDURE
// Time Complexity: O(1) – we always work at the front

**Peek (Front) – Pseudocode:**

PROCEDURE peek()
    IF front IS NULL THEN
        PRINT "Queue is empty."
        RETURN NULL
    END IF
    
    RETURN front.value
END PROCEDURE
// Time Complexity: O(1)

**Check if Empty – Pseudocode:**

FUNCTION isEmpty()
    RETURN front IS NULL
END FUNCTION
// Time Complexity: O(1)

### 3.5 Queue Pseudocode (Using Arrays – The Circular Trick)

Implementing a queue with arrays is **tricky** because if you keep incrementing the rear pointer, you'll quickly run out of space even if there's room at the front. The solution is a **Circular Queue**.

**The Problem:**

[0] [1] [2] [3] [4] [5] [6] [7]
 ↑               ↑
front           rear

After several enqueues and dequeues, the front moves forward, and the rear reaches the end. The front side is now empty, but we can't use it.

**The Solution: Circular Array**  
We wrap around using modulo arithmetic: `rear = (rear + 1) % MAX_SIZE`

**Initialization:**

DECLARE queue[MAX_SIZE] AS INTEGER
DECLARE front ← 0
DECLARE rear ← -1
DECLARE count ← 0    // Tracks the number of elements

**Enqueue – Pseudocode (Circular Array):**

PROCEDURE enqueue(value)
    IF count = MAX_SIZE THEN
        PRINT "Queue Overflow! Cannot enqueue."
        RETURN
    END IF
    
    // Move rear forward (with wrap-around)
    rear ← (rear + 1) % MAX_SIZE
    queue[rear] ← value
    count ← count + 1
END PROCEDURE
// Time Complexity: O(1)

**Dequeue – Pseudocode (Circular Array):**

PROCEDURE dequeue()
    IF count = 0 THEN
        PRINT "Queue Underflow! Cannot dequeue."
        RETURN NULL
    END IF
    
    value ← queue[front]
    
    // Move front forward (with wrap-around)
    front ← (front + 1) % MAX_SIZE
    count ← count - 1
    
    RETURN value
END PROCEDURE
// Time Complexity: O(1)

**Peek (Front) – Pseudocode (Circular Array):**

PROCEDURE peek()
    IF count = 0 THEN
        PRINT "Queue is empty."
        RETURN NULL
    END IF
    
    RETURN queue[front]
END PROCEDURE
// Time Complexity: O(1)

### 3.6 Why Are All Queue Operations O(1)?

Just like stacks, queues give us **O(1)** performance on all core operations:

- **Linked List version**: We maintain `front` and `rear` pointers. Enqueue adds at the rear (just updating `rear.next`). Dequeue removes from the front (just moving `front` forward). No traversal needed.
    
- **Array (Circular) version**: We use `front` and `rear` indices with modulo arithmetic. Moving an index forward and accessing an array position is instant. The count variable just increments/decrements.
    

### 3.7 Real-World Applications of Queues

|Application|How It Works|
|---|---|
|**CPU Scheduling**|Processes in a system are placed in a ready queue. The CPU picks the process at the front (FIFO scheduling).|
|**Print Spooling**|Multiple print jobs are queued; the printer processes them in the order they were submitted.|
|**Message Queues**|In messaging systems (like RabbitMQ, Kafka), producers enqueue messages, and consumers dequeue them.|
|**Breadth-First Search (BFS)**|In graph algorithms, BFS uses a queue to explore nodes level by level.|
|**Buffering (I/O, Networking)**|Data arriving from a network is buffered in a queue. The application reads from the front as data comes in.|
|**Call Center Systems**|Customer calls are placed in a queue; agents handle them in FIFO order.|

**Breadth-First Search (BFS) – Pseudocode (Conceptual):**

PROCEDURE BFS(graph, startNode)
    DECLARE visited AS ARRAY OF BOOLEAN
    DECLARE queue AS NEW Queue()
    
    visited[startNode] ← TRUE
    queue.enqueue(startNode)
    
    WHILE NOT queue.isEmpty()
        currentNode ← queue.dequeue()
        PRINT currentNode
        
        FOR EACH neighbor IN graph.getNeighbors(currentNode)
            IF NOT visited[neighbor] THEN
                visited[neighbor] ← TRUE
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
END PROCEDURE
// This explores the graph level by level – perfect for finding the shortest path in unweighted graphs

---

## 4. Stack vs Queue – The Head-to-Head

|Feature|Stack|Queue|
|---|---|---|
|**Principle**|LIFO (Last In, First Out)|FIFO (First In, First Out)|
|**Access Point**|Only the **top**|**Front** for removal, **Rear** for insertion|
|**Main Operations**|push, pop, peek|enqueue, dequeue, peek|
|**Time Complexity**|All operations O(1)|All operations O(1)|
|**Analogy**|Stack of plates, browser history|Line of people, print queue|
|**Use Case**|Function calls, undo/redo, expression evaluation|Scheduling, buffering, BFS|

---

## 5. The Deque – The Best of Both Worlds (Bonus)

A **Deque** (pronounced "deck") stands for **Double-Ended Queue**. It's a queue that allows insertion and deletion at **both ends**. You can push/pop from either the front or the back.

**Visual Representation:**

  ┌─────────────────────────────────────┐
  │  front ←→   Deque   ←→  back                  │
  │  (Add/Remove)       (Add/Remove)            │
  └─────────────────────────────────────┘

**Operations (All O(1)):**

- `addFront(value)`
    
- `addRear(value)`
    
- `removeFront()`
    
- `removeRear()`
    
- `peekFront()`
    
- `peekRear()`
    

**When to Use a Deque:**

- When you need flexibility to add/remove from both sides.
    
- Example: Implementing a sliding window maximum in an array (maintaining a deque of indices).
    
- Example: Palindrome checking (add characters to both ends and compare).
    

---

## 6. Stack vs Queue – When to Choose Which?

This is the most important practical question.

**Choose a Stack when:**

- You need to process data in reverse order of arrival (last thing added is the most important/urgent).
    
- You're dealing with nested structures (function calls, parentheses matching).
    
- You're implementing backtracking (undo, maze solving).
    
- The order of processing is "most recent first."
    

**Choose a Queue when:**

- You need to process data in the exact order it was received (fairness is important).
    
- You're dealing with scheduling or buffering.
    
- You're implementing BFS (level-by-level exploration).
    
- The order of processing is "first come, first served."
    

**Real-World Example:**  
Imagine you're at a tech support desk:

- **Stack**: You'd use a stack for handling support tickets if the most recent urgent issue always jumps to the top (LIFO).
    
- **Queue**: You'd use a queue if customers are served strictly based on who arrived first (FIFO).
    

---

## 7. Implementation Summary – Which One to Use?

|Implementation|Stack|Queue|
|---|---|---|
|**Array (Static)**|✅ Easy. `top` pointer works perfectly.|⚠️ Need circular array to avoid wasted space.|
|**Linked List**|✅ Insert/delete at head. Perfect fit.|✅ Maintain `front` and `rear` pointers. Perfect fit.|
|**When to Use Array**|When max size is known and you want cache efficiency.|When max size is known and you're okay with circular logic.|
|**When to Use Linked List**|When size is unpredictable and you want pure O(1) operations.|When size is unpredictable and you want pure O(1) operations.|

---

## 8. Summary

- **Stacks** operate on **LIFO** – you push to the top and pop from the top. All operations are O(1). They're perfect for managing nested/recursive structures and reversing order.
    
- **Queues** operate on **FIFO** – you enqueue at the rear and dequeue from the front. All operations are O(1). They're perfect for managing order-preserving workflows, scheduling, and buffering.
    
- Both are **restricted data structures** – you cannot access arbitrary elements. This restriction is a feature, not a bug! It enforces clean, predictable behavior.
    
- **Implementations** can be array-based (static, cache-friendly) or linked-list-based (dynamic, no size limit). Choose based on your memory and size constraints.
    
- The **Deque** gives you the best of both worlds – operations at both ends – but at the cost of slightly more complexity.
    

Stacks and queues are **everywhere**. Once you understand them, you'll start seeing them in everything – from the "Back" button in your browser to the way your operating system handles tasks. They are simple, elegant, and incredibly powerful.
