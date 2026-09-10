
## 1. Introduction – What Makes Graphs Special?

Think about everything you've learned so far:

- **Arrays/Linked Lists**: Linear relationships (one after another).
    
- **Trees**: Hierarchical relationships (one parent, many children).
    

Now imagine this:

- Facebook: You have friends, they have friends, everyone is connected.
    
- Google Maps: Cities are connected by roads, some are one-way, some have different distances.
    
- The Web: Websites link to each other in a giant web of connections.
    
- Dependency Graphs: Package A depends on B, which depends on C.
    

**None of these fit into a tree** because:

- In a tree, each node has exactly ONE parent.
    
- In these real-world examples, nodes can have MANY connections going in ANY direction.
    
- There can be **cycles** (A → B → C → A) – which trees cannot handle!
    

This is where **Graphs** come in. A **Graph** is a **non-linear** data structure consisting of:

- **Vertices (Nodes)**: The entities (users, cities, web pages).
    
- **Edges**: The connections between them (friendships, roads, hyperlinks).
    

Graphs are the **most flexible and expressive** data structure in computer science. They can model almost any relationship you can imagine.

---

## 2. Graph Terminology – The Vocabulary You MUST Know

|Term|Definition|Example|
|---|---|---|
|**Vertex (Node)**|A single entity in the graph.|A user on Facebook, a city on a map.|
|**Edge**|A connection between two vertices.|A friendship between two users, a road between two cities.|
|**Adjacent (Neighbor)**|Two vertices connected by an edge.|If A and B are friends, they are adjacent.|
|**Degree**|The number of edges incident to a vertex.|If A has 5 friends, its degree is 5.|
|**Path**|A sequence of vertices where each consecutive pair is connected by an edge.|A → B → C is a path.|
|**Cycle**|A path that starts and ends at the same vertex.|A → B → C → A is a cycle.|
|**Connected**|A graph where there's a path between every pair of vertices.|All users in a friend group.|
|**Disconnected**|A graph where some vertices are unreachable from others.|Two separate friend circles with no connection.|
|**Weight**|A numerical value assigned to an edge.|Distance between cities (km), cost of a flight ($).|
|**Directed (Digraph)**|Edges have a direction (one-way).|Twitter follows (A follows B but B may not follow A).|
|**Undirected**|Edges have no direction (two-way).|Facebook friendships (if A is friends with B, B is friends with A).|
|**Complete Graph**|Every vertex is connected to every other vertex.|A group where everyone is friends with everyone.|
|**Sparse Graph**|Few edges compared to the maximum possible.|Most real-world networks (the web, social networks).|
|**Dense Graph**|Many edges compared to the maximum possible.|A fully connected team where everyone talks to everyone.|

**Visual Reference: Undirected Graph**


        ┌─────────────────────┐
        │                     │
        ▼                     │
     ┌─────┐              ┌─────┐
     │  A  │──────────────│  B  │
     └──┬──┘              └──┬──┘
        │                    │
        │              ┌─────┴─────┐
        │              │           │
        ▼              ▼           ▼
     ┌─────┐        ┌─────┐    ┌─────┐
     │  C  │────────│  D  │────│  E  │
     └─────┘        └─────┘    └─────┘

Vertices: A, B, C, D, E  
Edges: (A,B), (A,C), (B,D), (B,E), (C,D), (D,E)  
Undirected – you can go both ways on any edge.

**Visual Reference: Directed Graph**


     ┌─────┐         ┌─────┐
     │  A  │─────────│  B  │
     └──┬──┘         └──┬──┘
        │               │
        ▼               ▼
     ┌─────┐         ┌─────┐
     │  C  │◀────────│  D  │
     └─────┘         └─────┘

Edges: A→B, A→C, B→D, D→C (C has no outgoing edges).  
You cannot go from B to A unless there's an edge (there isn't).

---

## 3. Graph Representations – How Do We Store a Graph?

There are TWO main ways to represent a graph in memory. Choosing the right one is crucial for performance.

### 3.1 Adjacency Matrix

An **Adjacency Matrix** is a 2D array of size `V × V` (V = number of vertices).

- `matrix[i][j] = 1` (or the weight) if there's an edge from vertex `i` to vertex `j`.
    
- `matrix[i][j] = 0` if there's NO edge.
    

**Visual Example (Undirected Graph):**


Graph:         Adjacency Matrix (V = 5):
     A───B        │ A │ B │ C │ D │ E │
     │   │        ├───┼───┼───┼───┼───┤
     C───D───E    │ A │ 0 │ 1 │ 1 │ 0 │ 0 │
                  │ B │ 1 │ 0 │ 0 │ 1 │ 1 │
                  │ C │ 1 │ 0 │ 0 │ 1 │ 0 │
                  │ D │ 0 │ 1 │ 1 │ 0 │ 1 │
                  │ E │ 0 │ 1 │ 0 │ 1 │ 0 │

**Notice the symmetry:** For an undirected graph, the matrix is **symmetric** across the diagonal. `matrix[i][j] = matrix[j][i]` because if A is connected to B, B is connected to A.

**For Directed Graphs:**

Graph:         Adjacency Matrix (V = 4):
     A→B        │ A │ B │ C │ D │
     ↓  ↓       ├───┼───┼───┼───┤
     C←D        │ A │ 0 │ 1 │ 1 │ 0 │
                │ B │ 0 │ 0 │ 0 │ 1 │
                │ C │ 0 │ 0 │ 0 │ 0 │
                │ D │ 0 │ 0 │ 1 │ 0 │

`matrix[i][j] = 1` if there's an edge from `i` to `j`. Not symmetric!

**For Weighted Graphs:**

Graph (Weights):      Adjacency Matrix (V = 3):
     A──5──B           │ A │ B │ C │
     │     /           ├───┼───┼───┤
     2   4             │ A │ 0 │ 5 │ 2 │
     │  /              │ B │ 5 │ 0 │ 4 │
     C                 │ C │ 2 │ 4 │ 0 │

`matrix[i][j] = weight` of the edge.

---

**Pseudocode – Adjacency Matrix:**

CLASS GraphMatrix
    DECLARE V AS INTEGER               // Number of vertices
    DECLARE matrix AS 2D ARRAY OF INTEGER
    
    // Constructor
    PROCEDURE GraphMatrix(vertices)
        V ← vertices
        matrix ← NEW 2D ARRAY[V][V]
        
        // Initialize all cells to 0
        FOR i ← 0 TO V-1
            FOR j ← 0 TO V-1
                matrix[i][j] ← 0
            END FOR
        END FOR
    END PROCEDURE
    
    // Add edge (undirected)
    PROCEDURE addEdge(u, v)
        matrix[u][v] ← 1
        matrix[v][u] ← 1   // For undirected graphs
    END PROCEDURE
    
    // Add directed edge
    PROCEDURE addDirectedEdge(u, v)
        matrix[u][v] ← 1
    END PROCEDURE
    
    // Add weighted edge
    PROCEDURE addWeightedEdge(u, v, weight)
        matrix[u][v] ← weight
        matrix[v][u] ← weight   // For undirected
    END PROCEDURE
    
    // Check if edge exists
    FUNCTION hasEdge(u, v)
        RETURN matrix[u][v] ≠ 0
    END FUNCTION
    
    // Get neighbors of a vertex
    FUNCTION getNeighbors(u)
        DECLARE neighbors AS LIST
        FOR v ← 0 TO V-1
            IF matrix[u][v] ≠ 0 THEN
                neighbors.add(v)
            END IF
        END FOR
        RETURN neighbors
    END FUNCTION
END CLASS

**Complexity:**

- Space: O(V²) – this is **huge** for large graphs!
    
- Add Edge: O(1)
    
- Remove Edge: O(1)
    
- Check Edge: O(1)
    
- Get Neighbors: O(V)
    

**When to use an Adjacency Matrix:**

- When the graph is **dense** (many edges).
    
- When V is small (under a few thousand).
    
- When you need constant-time edge lookup (O(1)).
    
- **NOT** suitable for sparse graphs (wastes memory).
    

---

### 3.2 Adjacency List

An **Adjacency List** is an array of lists. For each vertex `i`, we store a **list of its neighbors** (and optionally the edge weights).

**Visual Example (Undirected Graph):**


Graph:          Adjacency List:
     A───B        A: [B, C]
     │   │        B: [A, D, E]
     C───D───E    C: [A, D]
                  D: [B, C, E]
                  E: [B, D]

**For Directed Graphs:**


Graph:          Adjacency List:
     A→B        A: [B, C]
     ↓  ↓       B: [D]
     C←D        C: []
                D: [C]

**For Weighted Graphs (Storing pairs):**

Graph:          Adjacency List (Storing (neighbor, weight)):
     A──5──B    A: [(B,5), (C,2)]
     │     /    B: [(A,5), (C,4)]
     2   4      C: [(A,2), (B,4)]
     │  /
     C

---

**Pseudocode – Adjacency List:**

CLASS GraphList
    DECLARE V AS INTEGER
    DECLARE adj AS ARRAY OF LIST
    
    // Constructor
    PROCEDURE GraphList(vertices)
        V ← vertices
        adj ← NEW ARRAY[V]
        
        // Initialize each list
        FOR i ← 0 TO V-1
            adj[i] ← NEW LIST()
        END FOR
    END PROCEDURE
    
    // Add edge (undirected)
    PROCEDURE addEdge(u, v)
        adj[u].add(v)
        adj[v].add(u)   // For undirected graphs
    END PROCEDURE
    
    // Add directed edge
    PROCEDURE addDirectedEdge(u, v)
        adj[u].add(v)
    END PROCEDURE
    
    // Add weighted edge
    PROCEDURE addWeightedEdge(u, v, weight)
        adj[u].add((v, weight))
        adj[v].add((u, weight))   // For undirected
    END PROCEDURE
    
    // Check if edge exists
    FUNCTION hasEdge(u, v)
        FOR EACH neighbor IN adj[u]
            IF neighbor = v THEN
                RETURN TRUE
            END IF
        END FOR
        RETURN FALSE
    END FUNCTION
    
    // Get neighbors of a vertex
    FUNCTION getNeighbors(u)
        RETURN adj[u]
    END FUNCTION
END CLASS

**Complexity:**

- Space: O(V + E) – **much** more efficient for sparse graphs!
    
- Add Edge: O(1) (unless you need to check for duplicates)
    
- Remove Edge: O(degree(u)) – need to search the list
    
- Check Edge: O(degree(u)) – need to search the list
    
- Get Neighbors: O(degree(u))
    

**When to use an Adjacency List:**

- When the graph is **sparse** (few edges) – which is MOST real-world graphs.
    
- When V is large (millions of vertices).
    
- When you frequently iterate over neighbors (BFS, DFS).
    
- **Preferred in most practical applications.**
    

---

### 3.3 Comparison – Matrix vs List

|Feature|Adjacency Matrix|Adjacency List|
|---|---|---|
|**Space**|O(V²)|O(V + E)|
|**Edge Lookup**|O(1)|O(degree(u))|
|**Add Edge**|O(1)|O(1)|
|**Remove Edge**|O(1)|O(degree(u))|
|**Get Neighbors**|O(V)|O(degree(u))|
|**Memory Efficiency**|Poor for sparse graphs|Excellent for sparse graphs|
|**Best For**|Dense graphs, small V|Sparse graphs, large V|

**Pro Tip:** In 95% of real-world applications, **Adjacency List** is the better choice.

---

## 4. Graph Traversals – How Do We Explore a Graph?

Just like trees, we need ways to **visit every vertex** in a graph. However, graphs can have **cycles** – so we MUST track which vertices we've already visited to avoid infinite loops!

### 4.1 Depth-First Search (DFS)

DFS explores as **deep as possible** along each branch before backtracking. It's exactly like the tree DFS we saw earlier, but with a **visited array** to handle cycles.

**DFS Algorithm (Pseudocode) – Recursive:**


PROCEDURE DFS_recursive(graph, startVertex)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    
    // Initialize all to FALSE
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
    END FOR
    
    // Start the traversal
    DFS_util(startVertex, visited)
END PROCEDURE
PROCEDURE DFS_util(vertex, visited)
    // Mark current vertex as visited and print it
    visited[vertex] ← TRUE
    PRINT vertex
    
    // Recursively visit all unvisited neighbors
    FOR EACH neighbor IN graph.getNeighbors(vertex)
        IF NOT visited[neighbor] THEN
            DFS_util(neighbor, visited)
        END IF
    END FOR
END PROCEDURE
// Time Complexity: O(V + E)
// Space Complexity: O(V) – for the visited array + recursion stack

**DFS Algorithm (Pseudocode) – Iterative (Using a Stack):**


PROCEDURE DFS_iterative(graph, startVertex)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE stack AS NEW Stack()
    
    // Initialize all to FALSE
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
    END FOR
    
    // Push the start vertex onto the stack
    stack.push(startVertex)
    
    WHILE NOT stack.isEmpty()
        vertex ← stack.pop()
        
        // Skip if already visited
        IF visited[vertex] THEN
            CONTINUE
        END IF
        
        // Mark as visited and print
        visited[vertex] ← TRUE
        PRINT vertex
        
        // Push all unvisited neighbors onto the stack
        FOR EACH neighbor IN graph.getNeighbors(vertex)
            IF NOT visited[neighbor] THEN
                stack.push(neighbor)
            END IF
        END FOR
    END WHILE
END PROCEDURE
// Time Complexity: O(V + E)
// Space Complexity: O(V)

**Visual Trace (DFS):**


Graph:          Start at A:
     A───B      A → B → D → E → C → F → G
     │   │      (Visits deeply first)
     C───D───E
         │
         F───G
DFS Order: A, B, D, E, C, F, G

**DFS Applications:**

- Detecting cycles in a graph.
    
- Topological sorting (dependency resolution).
    
- Solving puzzles and mazes (backtracking).
    
- Finding connected components.
    
- Generating mazes.
    

---

### 4.2 Breadth-First Search (BFS)

BFS explores the graph **level by level** (like tree level-order traversal). It visits all neighbors first, then their neighbors, and so on.

**BFS Algorithm (Pseudocode) – Using a Queue:**


PROCEDURE BFS(graph, startVertex)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE queue AS NEW Queue()
    
    // Initialize all to FALSE
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
    END FOR
    
    // Mark start vertex as visited and enqueue it
    visited[startVertex] ← TRUE
    queue.enqueue(startVertex)
    
    WHILE NOT queue.isEmpty()
        vertex ← queue.dequeue()
        PRINT vertex
        
        // Enqueue all unvisited neighbors
        FOR EACH neighbor IN graph.getNeighbors(vertex)
            IF NOT visited[neighbor] THEN
                visited[neighbor] ← TRUE
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
END PROCEDURE
// Time Complexity: O(V + E)
// Space Complexity: O(V) – for the visited array + queue

**Visual Trace (BFS):**


Graph:          Start at A:
     A───B      A → B, C → D, E, F → G
     │   │      (Visits level by level)
     C───D───E
         │
         F───G
BFS Order: A, B, C, D, E, F, G

**BFS Applications:**

- Finding the **shortest path** in an unweighted graph.
    
- Web crawling (crawlers use BFS to discover web pages).
    
- Social network friend suggestions (find people at distance 2).
    
- Finding connected components.
    
- GPS navigation (shortest route with equal edge weights).
    

---

### 4.3 DFS vs BFS – The Head-to-Head

|Feature|DFS|BFS|
|---|---|---|
|**Data Structure**|Stack (recursive or explicit)|Queue|
|**Traversal Order**|Deep → Backtrack → Next branch|Level by level|
|**Shortest Path**|Does NOT guarantee shortest path|Guarantees shortest path in unweighted graphs|
|**Memory Usage**|O(h) where h is the graph height (can be O(V) in worst case)|O(w) where w is the maximum width (can be O(V) in worst case)|
|**Use Case**|Cycle detection, topological sort, backtracking|Shortest path, web crawling, level-order problems|

---

## 5. Important Graph Algorithms

### 5.1 Cycle Detection in Undirected Graphs (Using DFS)

A cycle exists if we encounter a visited node that is **not the parent** of the current node.

**Pseudocode – Cycle Detection (Undirected):**


PROCEDURE hasCycleUndirected(graph)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
    END FOR
    
    // Check every component
    FOR i ← 0 TO graph.V-1
        IF NOT visited[i] THEN
            IF hasCycleUtil(i, -1, visited) THEN
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    RETURN FALSE
END PROCEDURE
PROCEDURE hasCycleUtil(vertex, parent, visited)
    visited[vertex] ← TRUE
    
    FOR EACH neighbor IN graph.getNeighbors(vertex)
        IF NOT visited[neighbor] THEN
            IF hasCycleUtil(neighbor, vertex, visited) THEN
                RETURN TRUE
            END IF
        ELSE IF neighbor ≠ parent THEN
            // Found a visited node that's not the parent → CYCLE!
            RETURN TRUE
        END IF
    END FOR
    
    RETURN FALSE
END PROCEDURE
// Time Complexity: O(V + E)

### 5.2 Cycle Detection in Directed Graphs (Using DFS with 3 States)

For directed graphs, we need three states:

- **0**: Unvisited
    
- **1**: Visiting (currently in the recursion stack)
    
- **2**: Fully processed (no cycles found)
    

If we encounter a node that's **currently visiting**, we have a cycle.

**Pseudocode – Cycle Detection (Directed):**


PROCEDURE hasCycleDirected(graph)
    DECLARE state AS ARRAY OF INTEGER[graph.V]  // 0=Unvisited, 1=Visiting, 2=Processed
    
    FOR i ← 0 TO graph.V-1
        state[i] ← 0
    END FOR
    
    FOR i ← 0 TO graph.V-1
        IF state[i] = 0 THEN
            IF hasCycleDirectedUtil(i, state) THEN
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    RETURN FALSE
END PROCEDURE
PROCEDURE hasCycleDirectedUtil(vertex, state)
    state[vertex] ← 1   // Mark as visiting
    
    FOR EACH neighbor IN graph.getNeighbors(vertex)
        IF state[neighbor] = 1 THEN
            RETURN TRUE   // Found a back edge → CYCLE!
        END IF
        
        IF state[neighbor] = 0 THEN
            IF hasCycleDirectedUtil(neighbor, state) THEN
                RETURN TRUE
            END IF
        END IF
    END FOR
    
    state[vertex] ← 2   // Mark as processed
    RETURN FALSE
END PROCEDURE
// Time Complexity: O(V + E)

### 5.3 Topological Sorting (Only for Directed Acyclic Graphs – DAGs)

Topological sorting orders vertices such that for every directed edge `u → v`, `u` comes before `v`. This is used for **dependency resolution** (e.g., course prerequisites, task scheduling).

**Method 1: DFS-Based Topological Sort**


PROCEDURE topologicalSortDFS(graph)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE stack AS NEW Stack()
    
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
    END FOR
    
    FOR i ← 0 TO graph.V-1
        IF NOT visited[i] THEN
            topoUtil(i, visited, stack)
        END IF
    END FOR
    
    // Pop from stack to get the topological order
    WHILE NOT stack.isEmpty()
        PRINT stack.pop()
    END WHILE
END PROCEDURE
PROCEDURE topoUtil(vertex, visited, stack)
    visited[vertex] ← TRUE
    
    FOR EACH neighbor IN graph.getNeighbors(vertex)
        IF NOT visited[neighbor] THEN
            topoUtil(neighbor, visited, stack)
        END IF
    END FOR
    
    // Push after all descendants are processed
    stack.push(vertex)
END PROCEDURE
// Time Complexity: O(V + E)

**Method 2: Kahn's Algorithm (BFS-Based, Using In-Degree)**


PROCEDURE topologicalSortKahn(graph)
    DECLARE inDegree AS ARRAY OF INTEGER[graph.V]
    DECLARE queue AS NEW Queue()
    DECLARE result AS LIST
    
    // Calculate in-degree for each vertex
    FOR u ← 0 TO graph.V-1
        FOR EACH v IN graph.getNeighbors(u)
            inDegree[v] ← inDegree[v] + 1
        END FOR
    END FOR
    
    // Enqueue all vertices with in-degree 0
    FOR i ← 0 TO graph.V-1
        IF inDegree[i] = 0 THEN
            queue.enqueue(i)
        END IF
    END FOR
    
    WHILE NOT queue.isEmpty()
        vertex ← queue.dequeue()
        result.add(vertex)
        
        // Reduce in-degree of all neighbors
        FOR EACH neighbor IN graph.getNeighbors(vertex)
            inDegree[neighbor] ← inDegree[neighbor] - 1
            IF inDegree[neighbor] = 0 THEN
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    // If result size < V, there's a cycle!
    IF result.size() < graph.V THEN
        PRINT "Graph has a cycle! No topological order exists."
    ELSE
        PRINT result
    END IF
END PROCEDURE
// Time Complexity: O(V + E)

**Visual Example:**


Prerequisites for courses:
    CS101 → CS201 → CS301
           ↘ CS202 ↗
    
Topological Order: CS101, CS201, CS202, CS301 (or CS101, CS202, CS201, CS301)
Both are valid because dependencies are satisfied.

---

### 5.4 Shortest Path Algorithms

#### A) BFS for Shortest Path (Unweighted Graphs)

BFS naturally finds the shortest path in unweighted graphs because it explores level by level.

**Pseudocode – Shortest Path using BFS:**


PROCEDURE shortestPathBFS(graph, start, target)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE parent AS ARRAY OF INTEGER[graph.V]  // To reconstruct the path
    DECLARE queue AS NEW Queue()
    
    FOR i ← 0 TO graph.V-1
        visited[i] ← FALSE
        parent[i] ← -1
    END FOR
    
    visited[start] ← TRUE
    queue.enqueue(start)
    
    WHILE NOT queue.isEmpty()
        vertex ← queue.dequeue()
        
        IF vertex = target THEN
            BREAK   // Found the target
        END IF
        
        FOR EACH neighbor IN graph.getNeighbors(vertex)
            IF NOT visited[neighbor] THEN
                visited[neighbor] ← TRUE
                parent[neighbor] ← vertex
                queue.enqueue(neighbor)
            END IF
        END FOR
    END WHILE
    
    // Reconstruct path from target to start
    IF NOT visited[target] THEN
        PRINT "No path found"
        RETURN NULL
    END IF
    
    DECLARE path AS LIST
    current ← target
    
    WHILE current ≠ -1
        path.add(current)
        current ← parent[current]
    END WHILE
    
    REVERSE path   // Now it's from start to target
    RETURN path
END PROCEDURE
// Time Complexity: O(V + E)

#### B) Dijkstra's Algorithm (Weighted Graphs – No Negative Weights)

Dijkstra's algorithm finds the shortest path from a source to **all other vertices** in a weighted graph with **non-negative** edge weights.

**Key Idea:** Greedy approach – always pick the unvisited vertex with the smallest distance.

**Pseudocode – Dijkstra's Algorithm (Using a Priority Queue/Min-Heap):**


PROCEDURE dijkstra(graph, source)
    DECLARE dist AS ARRAY OF INTEGER[graph.V]
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE parent AS ARRAY OF INTEGER[graph.V]
    
    // Initialize distances to INFINITY
    FOR i ← 0 TO graph.V-1
        dist[i] ← INFINITY
        visited[i] ← FALSE
        parent[i] ← -1
    END FOR
    
    dist[source] ← 0
    DECLARE pq AS NEW PriorityQueue()   // Min-heap (distance, vertex)
    pq.enqueue((0, source))
    
    WHILE NOT pq.isEmpty()
        (currentDist, vertex) ← pq.dequeue()
        
        IF visited[vertex] THEN
            CONTINUE
        END IF
        
        visited[vertex] ← TRUE
        
        // If we found a better path, skip
        IF currentDist > dist[vertex] THEN
            CONTINUE
        END IF
        
        // Relax all neighbors
        FOR EACH (neighbor, weight) IN graph.getNeighbors(vertex)
            newDist ← currentDist + weight
            IF newDist < dist[neighbor] THEN
                dist[neighbor] ← newDist
                parent[neighbor] ← vertex
                pq.enqueue((newDist, neighbor))
            END IF
        END FOR
    END WHILE
    
    RETURN dist, parent
END PROCEDURE
// Time Complexity: O((V + E) log V) with a min-heap
// O(V²) with a simple array (for dense graphs)

**Visual Example:**


Graph:          Dijkstra from A:
     A──5──B     A: 0
     │     /     B: 5 (A→B)
     2   4       C: 2 (A→C)
     │  /        D: 7 (A→C→D)  or 9 (A→B→D)
     C

**Key Insights:**

- Dijkstra works for **directed** and **undirected** graphs.
    
- Fails with **negative edge weights** (use Bellman-Ford instead).
    
- Guarantees the shortest path because it's greedy but optimal.
    

#### C) Bellman-Ford Algorithm (Handles Negative Weights)

Bellman-Ford is slower than Dijkstra but works with **negative edge weights** and detects negative cycles.

**Pseudocode – Bellman-Ford:**


PROCEDURE bellmanFord(graph, source)
    DECLARE dist AS ARRAY OF INTEGER[graph.V]
    
    FOR i ← 0 TO graph.V-1
        dist[i] ← INFINITY
    END FOR
    
    dist[source] ← 0
    
    // Relax all edges V-1 times
    FOR i ← 1 TO graph.V-1
        FOR EACH (u, v, weight) IN graph.getAllEdges()
            IF dist[u] ≠ INFINITY AND dist[u] + weight < dist[v] THEN
                dist[v] ← dist[u] + weight
            END IF
        END FOR
    END FOR
    
    // Check for negative cycles (one more relaxation)
    FOR EACH (u, v, weight) IN graph.getAllEdges()
        IF dist[u] ≠ INFINITY AND dist[u] + weight < dist[v] THEN
            PRINT "Negative cycle detected!"
            RETURN NULL
        END IF
    END FOR
    
    RETURN dist
END PROCEDURE
// Time Complexity: O(V * E)
// Space Complexity: O(V)

---

### 5.5 Minimum Spanning Tree (MST)

A **Minimum Spanning Tree** is a subset of edges that connects all vertices with the **minimum total edge weight** and **no cycles**.

#### A) Prim's Algorithm (Greedy – Grows a Tree)

**Pseudocode – Prim's Algorithm:**


PROCEDURE primMST(graph)
    DECLARE visited AS ARRAY OF BOOLEAN[graph.V]
    DECLARE key AS ARRAY OF INTEGER[graph.V]   // Minimum weight to connect
    DECLARE parent AS ARRAY OF INTEGER[graph.V]
    DECLARE pq AS NEW PriorityQueue()          // Min-heap
    
    FOR i ← 0 TO graph.V-1
        key[i] ← INFINITY
        visited[i] ← FALSE
        parent[i] ← -1
    END FOR
    
    key[0] ← 0
    pq.enqueue((0, 0))   // (key, vertex)
    
    WHILE NOT pq.isEmpty()
        (currentKey, vertex) ← pq.dequeue()
        
        IF visited[vertex] THEN
            CONTINUE
        END IF
        
        visited[vertex] ← TRUE
        
        FOR EACH (neighbor, weight) IN graph.getNeighbors(vertex)
            IF NOT visited[neighbor] AND weight < key[neighbor] THEN
                key[neighbor] ← weight
                parent[neighbor] ← vertex
                pq.enqueue((key[neighbor], neighbor))
            END IF
        END FOR
    END WHILE
    
    RETURN parent   // The MST edges are (parent[i], i)
END PROCEDURE
// Time Complexity: O((V + E) log V)

#### B) Kruskal's Algorithm (Greedy – Sort All Edges)

**Pseudocode – Kruskal's Algorithm (Using Union-Find):**


PROCEDURE kruskalMST(graph)
    DECLARE edges ← graph.getAllEdges()   // List of (u, v, weight)
    SORT edges BY weight ASCENDING
    
    DECLARE parent AS ARRAY OF INTEGER[graph.V]
    DECLARE rank AS ARRAY OF INTEGER[graph.V]
    
    // Initialize Union-Find
    FOR i ← 0 TO graph.V-1
        parent[i] ← i
        rank[i] ← 0
    END FOR
    
    DECLARE mst AS LIST
    totalWeight ← 0
    
    FOR EACH (u, v, weight) IN edges
        IF find(u) ≠ find(v) THEN   // No cycle
            union(u, v)
            mst.add((u, v, weight))
            totalWeight ← totalWeight + weight
        END IF
    END FOR
    
    RETURN mst, totalWeight
END PROCEDURE
// Union-Find Helper Functions
FUNCTION find(x)
    IF parent[x] ≠ x THEN
        parent[x] ← find(parent[x])   // Path compression
    END IF
    RETURN parent[x]
END FUNCTION
PROCEDURE union(x, y)
    rootX ← find(x)
    rootY ← find(y)
    
    IF rootX = rootY THEN
        RETURN
    END IF
    
    // Union by rank
    IF rank[rootX] < rank[rootY] THEN
        parent[rootX] ← rootY
    ELSE IF rank[rootX] > rank[rootY] THEN
        parent[rootY] ← rootX
    ELSE
        parent[rootY] ← rootX
        rank[rootX] ← rank[rootX] + 1
    END IF
END PROCEDURE
// Time Complexity: O(E log E) due to sorting

---

## 6. Special Types of Graphs

|Type|Definition|Use Case|
|---|---|---|
|**Directed Acyclic Graph (DAG)**|Directed graph with **no cycles**.|Dependency resolution, topological sorting.|
|**Weighted Graph**|Edges have numerical values.|GPS navigation (distance), network routing (cost).|
|**Connected Graph**|Path exists between every pair of vertices.|Social networks (everyone connected in one component).|
|**Complete Graph**|Every vertex connected to every other.|Fully connected network.|
|**Bipartite Graph**|Vertices can be divided into two sets with no edges within a set.|Matching problems (job allocation).|
|**Tree**|A connected acyclic graph.|Any tree you've seen (simpler than general graphs).|

---

## 7. Real-World Applications of Graphs

|Application|How Graphs Are Used|
|---|---|
|**GPS Navigation**|Road networks as weighted graphs (Dijkstra's for shortest path).|
|**Social Networks**|Users as vertices, friendships as edges (BFS for friend suggestions, PageRank).|
|**Web Crawling**|Web pages as vertices, hyperlinks as directed edges (BFS/DFS to discover pages).|
|**Recommendation Systems**|User-item graph (collaborative filtering).|
|**Network Routing**|Routers as vertices, connections as edges (shortest path algorithms).|
|**Dependency Management**|Packages as vertices, dependencies as directed edges (topological sort).|
|**Machine Learning**|Graphs for representing data (GNNs, knowledge graphs).|
|**Compilers**|Dependency graphs for code optimization.|
|**Scheduling**|Task dependencies (topological sort for task ordering).|
|**Finance**|Fraud detection (graph analysis of transactions).|

---

## 8. Summary – Your Graph Takeaway

- **Graphs** are the **most general** data structure – they can represent any relationship.
    
- **Vertices** (nodes) and **Edges** (connections) are the building blocks.
    
- **Adjacency Matrix** (O(V²)) and **Adjacency List** (O(V+E)) are the two main representations.
    
    - Use **List** for sparse graphs (most real-world graphs).
        
    - Use **Matrix** for dense graphs or when O(1) edge lookup is critical.
        
- **DFS** (stack/recursion) and **BFS** (queue) are the fundamental traversals.
    
    - DFS → Path finding, cycle detection, topological sort.
        
    - BFS → Shortest path in unweighted graphs, level-order problems.
        
- **Dijkstra's Algorithm** finds shortest paths in weighted graphs with non-negative weights.
    
- **Bellman-Ford** handles negative weights and detects negative cycles.
    
- **Topological Sorting** orders tasks in a DAG.
    
- **Minimum Spanning Tree** (Prim's, Kruskal's) finds the cheapest way to connect all vertices.
    

**The Big Secret:** Graphs are **the language of connections**. Once you master graphs, you can model almost any real-world problem. They are the foundation of modern computing – from the internet to AI.
