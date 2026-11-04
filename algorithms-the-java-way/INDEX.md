# Algorithms: The Java Way — Complete Curriculum Index

**18 Parts · 74 Chapters · Full Computer Science Curriculum**

> ✅ = Chapter file exists and is complete · 🔲 = Planned (index entry only, no chapter file yet)

---

## Navigation

- [Part I — Foundations of Algorithmic Thinking](#part-i--foundations-of-algorithmic-thinking)
- [Part II — Arrays and Array Patterns](#part-ii--arrays-and-array-patterns)
- [Part III — Strings and String Algorithms](#part-iii--strings-and-string-algorithms)
- [Part IV — Searching and Sorting](#part-iv--searching-and-sorting)
- [Part V — Hashing and Lookup Structures](#part-v--hashing-and-lookup-structures)
- [Part VI — Linked Lists](#part-vi--linked-lists)
- [Part VII — Stacks, Queues and Deques](#part-vii--stacks-queues-and-deques)
- [Part VIII — Trees and Hierarchical Structures](#part-viii--trees-and-hierarchical-structures)
- [Part IX — Heaps and Priority Queues](#part-ix--heaps-and-priority-queues)
- [Part X — Graph Theory and Algorithms](#part-x--graph-theory-and-algorithms)
- [Part XI — Greedy Algorithms and Divide and Conquer](#part-xi--greedy-algorithms-and-divide-and-conquer)
- [Part XII — Dynamic Programming](#part-xii--dynamic-programming)
- [Part XIII — Backtracking and Bit Manipulation](#part-xiii--backtracking-and-bit-manipulation)
- [Part XIV — Advanced Data Structures and Range Queries](#part-xiv--advanced-data-structures-and-range-queries)
- [Part XV — Advanced Algorithmic Techniques](#part-xv--advanced-algorithmic-techniques)
- [Part XVI — Data Structure Design and Systems-Oriented Problems](#part-xvi--data-structure-design-and-systems-oriented-problems)
- [Part XVII — Algorithmic Paradigms and Complexity Theory](#part-xvii--algorithmic-paradigms-and-complexity-theory)
- [Part XVIII — Interview Mastery](#part-xviii--interview-mastery)

---

## Part I — Foundations of Algorithmic Thinking

### ✅ Chapter 1 — [Algorithmic Thinking: The Art of Problem Solving](chapters/chapter-01-algorithmic-thinking.md)

- What is an algorithm?
- Properties of a well-defined algorithm (Knuth's five properties)
- Inputs, outputs, constraints and assumptions
- Abstracting real-world problems
- The six-phase problem-solving framework
- Brute force as a starting point
- Your first algorithm: Linear Search
- Correctness, loop invariants and edge cases
- Why efficiency matters (scaling intuition)
- Why Java?
- Java essentials: integer overflow, strings, collections

---

### ✅ Chapter 2 — [Mathematical Foundations for Programmers](chapters/chapter-02-mathematical-foundations.md)

- Number systems: binary, decimal, octal and hexadecimal
- Place values and positional representation
- Signed integers and two's complement
- Integer overflow and underflow
- Powers, logarithms and exponents
- Summations and mathematical series
- Combinatorics and counting
- Factorials, permutations and combinations
- GCD, LCM and the Euclidean algorithm
- Modular arithmetic
- Prime numbers and divisibility
- Mathematical induction

---

### ✅ Chapter 3 — [Bit Manipulation: The Language of Hardware](chapters/chapter-03-bit-manipulation.md)

- Why bit manipulation matters
- Bitwise operators: AND, OR, XOR and NOT with truth tables
- Shift operators: left shift, arithmetic right shift, logical right shift
- Setting, clearing, toggling and reading individual bits
- Bit masks and masking patterns
- XOR properties and interview patterns
- Detecting and working with powers of two
- Counting set bits: naive, Brian Kernighan's algorithm, Integer.bitCount()
- Hamming weight and Hamming distance
- Isolating the lowest set bit and turning it off
- Building a reusable bit operations toolkit in Java
- Java-specific pitfalls: operator precedence, shift semantics, sign extension

---

### ✅ Chapter 4 — [Java Toolkit for Data Structures and Algorithms](chapters/chapter-04-java-toolkit.md)

- Java execution model and JVM basics
- Primitive types, wrappers and type conversion
- Integer limits, overflow and safe arithmetic
- Arrays and multidimensional arrays
- Strings and StringBuilder
- Object equality, hashCode and comparison
- Collections Framework overview
- ArrayList, LinkedList and arrays
- HashMap, HashSet, TreeMap and TreeSet
- ArrayDeque and PriorityQueue
- Comparator and Comparable
- Generics and type erasure
- Sorting and binary search APIs
- Fast input/output
- Java-specific performance considerations

---

### ✅ Chapter 5 — [Recursion: Learning to Think Recursively](chapters/chapter-05-recursion.md)

- What is recursion?
- Recursive vs iterative thinking
- Base cases and recursive cases
- Identifying recursive structures
- Call stack and stack frames
- Tracing recursive calls
- Return values and recursive execution
- Direct and indirect recursion
- Linear and tree recursion
- Multiple recursive calls
- Recursion depth and StackOverflowError
- Recursion vs iteration
- Converting recursive solutions into iterative solutions
- Recursion trees and introductory recurrence relations
- Java recursion patterns

---

### ✅ Chapter 6 — [Asymptotic Analysis: Measuring Algorithm Efficiency](chapters/chapter-06-asymptotic-analysis.md)

- Why efficiency matters
- Input size and growth rates
- Big-O, Big-Omega and Big-Theta notation
- Best, average and worst cases
- Complexity classes: O(1), O(log n), O(n), O(n log n), O(n²), O(2ⁿ)
- Analysing loops and nested loops
- Analysing recursive algorithms
- Recurrence relations
- Recursion trees
- Substitution method
- Master Theorem
- Amortised analysis
- Time vs space complexity
- Auxiliary space and recursion stack

---

### ✅ [Part I Review — The Foundations, Revisited](chapters/part-01-review.md)

- The Algorithm Detective: a cross-chapter narrative
- Concept map: how the six foundations interconnect
- The Six Principles: core idea from each chapter distilled to one line
- Five cross-chapter challenges (Chapters 2+3, 5+6, 1+4, 3+5, 2+6)
- Part I lightning quiz (10 questions)
- Grand Integration Problem: Single Number III — all six chapters
- Looking ahead: how Part I lives inside Part II

---

## Part II — Arrays and Array Patterns

### ✅ Chapter 7 — [Arrays: Foundations and Memory Model](chapters/chapter-07-arrays.md)

- Array representation in Java
- Indexing and random access
- Traversal, insertion and deletion
- Static and dynamic arrays
- Array copying and resizing
- Multidimensional arrays
- In-place algorithms
- Array manipulation

---

### ✅ Chapter 8 — [Two Pointers](chapters/chapter-08-two-pointers.md)

- Opposite-direction pointers
- Same-direction pointers
- Fast and slow pointers
- Read/write pointer technique
- Partitioning arrays
- Removing duplicates
- Pair and triplet problems
- When two pointers work and when they fail

---

### ✅ Chapter 9 — [Sliding Window](chapters/chapter-09-sliding-window.md)

- Fixed-size sliding window
- Variable-size sliding window
- Expanding and shrinking windows
- Window invariants
- Frequency maps within windows
- Longest and shortest subarray problems
- Multiple constraints
- Common pitfalls

---

### ✅ Chapter 10 — [Prefix Sums and Difference Arrays](chapters/chapter-10-prefix-sums.md)

- Running sums
- Prefix sum construction
- Range sum queries
- Subarray sum problems
- Prefix sums with HashMap
- Two-dimensional prefix sums
- Difference arrays
- Range updates

---

### ✅ Chapter 11 — [Advanced Array Patterns](chapters/chapter-11-advanced-array-patterns.md)

- Kadane's algorithm
- Maximum subarray
- Cyclic sort
- Index placement patterns
- Dutch National Flag
- Matrix traversal
- Matrix rotation and transformation
- Spiral traversal
- In-place matrix manipulation
- Array partitioning and rearrangement

---

### ✅ [Part II Review — The Array Toolkit, Consolidated](chapters/part-02-review.md)

- Story: The Data Pipeline Incident (5-act narrative)
- Pattern map: how Chapters 7–11 interconnect
- The five patterns summary table
- Four cross-chapter challenges
- Part II lightning quiz (10 questions)
- Grand Integration Problem: Maximum Sum of Two Non-Overlapping Subarrays
- Preview of Part III

---

## Part III — Strings and String Algorithms

### ✅ Chapter 12 — [String Fundamentals](chapters/chapter-12-string-fundamentals.md)

- Character encoding and Unicode
- Java String internals
- String immutability
- StringBuilder and StringBuffer
- Character arithmetic
- String comparison and equality
- Frequency counting
- Palindromes
- Anagrams
- Substrings and subsequences

---

### ✅ Chapter 13 — [Advanced String Algorithms](chapters/chapter-13-advanced-string-algorithms.md)

- Naive pattern matching
- Prefix and suffix concepts
- KMP algorithm
- Z-algorithm
- Rabin-Karp
- Rolling hash
- Boyer-Moore overview
- Aho-Corasick overview
- Trie-based matching
- String matching applications

---

### ✅ [Part III Review — Strings, Consolidated](chapters/part-03-review.md)

- Two-chapter concept map
- The four string principles
- Four cross-chapter challenges (anagram+sliding window, rotation via KMP, repeated unit via failure fn, palindrome prefix)
- Part III lightning quiz (8 questions)
- Grand Integration Problem: Longest Happy Prefix
- Preview of Part IV

---

## Part IV — Searching and Sorting

### ✅ Chapter 14 — [Searching Fundamentals](chapters/chapter-14-searching-fundamentals.md)

- Linear search
- Binary search intuition and derivation
- Binary search invariants
- Iterative and recursive binary search
- Boundary conditions
- First and last occurrence
- Lower bound and upper bound
- Search insertion position
- Java binary search APIs

---

### ✅ Chapter 15 — [Advanced Binary Search Patterns](chapters/chapter-15-advanced-binary-search.md)

- Binary search on answer
- Search in rotated arrays
- Search in nearly sorted arrays
- Peak element problems
- Search in matrices
- Monotonic predicates
- Minimum feasible value problems
- Binary search over continuous ranges
- Off-by-one errors

---

### ✅ Chapter 16 — [Elementary Sorting Algorithms](chapters/chapter-16-elementary-sorting.md)

- Sorting fundamentals
- Stability
- Selection sort
- Bubble sort
- Insertion sort
- Adaptive sorting
- In-place vs out-of-place sorting
- When simple sorting is useful

---

### ✅ Chapter 17 — [Efficient and Non-Comparison Sorting](chapters/chapter-17-efficient-sorting.md)

- Merge sort
- Quick sort
- Partitioning and pivot selection
- Three-way partitioning
- Heap sort
- Counting sort
- Radix sort
- Bucket sort
- Comparison sorting lower bound O(n log n)
- Java Arrays.sort internals
- Sorting objects and custom comparators

---

### ✅ [Part IV Review — Searching and Sorting, Consolidated](chapters/part-04-review.md)

- Story analogy and four-chapter concept map
- Six principles of Part IV
- Four cross-chapter challenges (sort+binary search, binary search on answer+sort, inversion counting, non-comparison sort)
- Part IV lightning quiz (8 questions)
- Grand Integration Problem: H-Index (3 approaches — sorting, counting sort, binary search)
- Preview of Part V

---

## Part V — Hashing and Lookup Structures

### ✅ Chapter 18 — [Hash Tables: Design and Theory](chapters/chapter-18-hash-tables.md)

- Direct addressing
- Hash functions
- Collisions
- Separate chaining
- Open addressing
- Linear and quadratic probing
- Double hashing
- Load factor
- Rehashing
- Complexity

---

### ✅ Chapter 19 — [HashMap, HashSet and Problem Patterns](chapters/chapter-19-hashmap-patterns.md)

- Java HashMap internals
- Java HashSet internals
- Frequency counting
- Duplicate detection
- Two Sum and variations
- Grouping and categorisation
- Prefix sum with hashing
- Sliding window with hash maps
- Hash-based optimisation

---

### ✅ Chapter 20 — [Ordered Maps and Advanced Hashing](chapters/chapter-20-ordered-maps.md)

- TreeMap and TreeSet
- Sorted lookup and range queries
- Floor and ceiling operations
- Ordered frequency problems
- Rolling hash applications
- Hash collisions in adversarial inputs
- Consistent hashing
- Hashing in distributed systems

---

### ✅ [Part V Review — Hashing and Lookup Structures, Consolidated](chapters/part-05-review.md)

- Story: The Recommendation Engine (3-act narrative using all chapters)
- Three-chapter concept map
- Four principles of Part V
- Three cross-chapter challenges
- Part V lightning quiz (6 questions)
- Grand Integration Problem: LFU Cache (3-part breakdown)
- Preview of Part VI

---

## Part VI — Linked Lists

### ✅ Chapter 21 — [Linked List Foundations](chapters/chapter-21-linked-list-foundations.md)

- Linked list concepts
- Nodes and references
- Singly linked lists
- Doubly linked lists
- Circular linked lists
- Insertion and deletion
- Traversal
- Java implementation
- Linked list vs array

---

### ✅ Chapter 22 — [Linked List Problem Patterns](chapters/chapter-22-linked-list-patterns.md)

- Reversal
- Recursive reversal
- Fast and slow pointers
- Cycle detection
- Middle node
- Intersection
- Merging sorted lists
- Reordering
- Palindrome linked list
- Java LinkedList considerations

---

### ✅ [Part VI Review — Linked Lists, Consolidated](chapters/part-06-review.md)

- Story: The Packet Router
- Two-chapter concept map
- Four principles
- Three cross-chapter challenges
- Part VI lightning quiz
- Grand Integration Problem: LRU Cache (3-part breakdown)
- Preview of Part VII

---

## Part VII — Stacks, Queues and Deques

### ✅ Chapter 23 — [Stacks and Their Applications](chapters/chapter-23-stacks.md)

- Stack ADT
- Array and linked-list implementations
- Java ArrayDeque as stack
- Balanced parentheses
- Expression evaluation
- Infix, postfix and prefix notation
- Monotonic stack
- Next greater and smaller elements
- Histogram and contribution problems

---

### ✅ Chapter 24 — [Queues, Deques and Circular Buffers](chapters/chapter-24-queues-deques.md)

- Queue ADT
- Queue implementations
- Circular queues
- Deques
- Java ArrayDeque as queue
- Monotonic queues
- Sliding window maximum
- BFS applications
- Queue design problems

---

### ✅ [Part VII Review — Stacks, Queues and Deques, Consolidated](chapters/part-07-review.md)

- Story: The Expression Engine
- Two-chapter concept map
- Four principles
- Three cross-chapter challenges (calculator, sliding-window min, BFS level-order)
- Part VII lightning quiz (5 questions)
- Grand Integration Problem: Maximal Rectangle (3-part breakdown)
- Preview of Part VIII

---

## Part VIII — Trees and Hierarchical Structures

### ✅ Chapter 25 — [Tree Fundamentals and Binary Trees](chapters/chapter-25-tree-fundamentals.md)

- Tree terminology
- Root, leaf, parent, child and height
- Binary tree representation
- Recursive tree structure
- Java implementation
- Tree properties
- Full, complete and perfect binary trees

---

### ✅ Chapter 26 — [Tree Traversal and Recursive Patterns](chapters/chapter-26-tree-traversal.md)

- Preorder traversal
- Inorder traversal
- Postorder traversal
- Level-order traversal
- Recursive and iterative traversal
- Height and depth
- Diameter
- Root-to-leaf paths
- Path sum
- Lowest common ancestor

---

### ✅ Chapter 27 — [Binary Search Trees](chapters/chapter-27-binary-search-trees.md)

- BST property and invariant
- Search, insertion and deletion
- Inorder traversal and sorted order
- Minimum and maximum
- Predecessor and successor
- Validation
- BST pathologies
- Balanced vs unbalanced trees

---

### ✅ Chapter 28 — [Balanced Search Trees](chapters/chapter-28-balanced-trees.md)

- Why balance matters
- AVL trees
- Rotations
- Red-Black tree properties
- B-Trees and B+ Trees overview
- Java TreeMap internals
- Ordered structures in practice

---

### ✅ Chapter 29 — [Advanced Tree Structures](chapters/chapter-29-advanced-trees.md)

- N-ary trees
- Tries and prefix trees
- Trie implementation
- Autocomplete
- Word search
- Tree serialization and deserialization
- Segment tree introduction
- Tree problem-solving patterns

---

### ✅ [Part VIII Review — Trees and Hierarchical Structures, Consolidated](chapters/part-08-review.md)

- Story: The Version Control System (3-act narrative using all chapters)
- Five-chapter concept map
- Five principles of Part VIII
- Three cross-chapter challenges
- Part VIII lightning quiz (6 questions)
- Grand Integration Problem: Git History Viewer (3-part breakdown)
- Preview of Part IX

---

## Part IX — Heaps and Priority Queues

### ✅ Chapter 30 — [Heaps and Priority Queues](chapters/chapter-30-heaps.md)

- Complete binary tree property
- Min heap and max heap
- Array representation
- Insertion and extraction
- Heapify
- Heap sort
- Java PriorityQueue
- Custom comparators
- Top-K problems
- K-way merge
- Running median

---

### ✅ [Part IX Review — Heaps and Priority Queues, Consolidated](chapters/part-09-review.md)

- Story: The Hospital Dispatch System (3-act narrative)
- Chapter 30 concept map
- Four principles of Part IX
- Three cross-chapter challenges (merge K BSTs, sliding window median, task scheduler)
- Part IX lightning quiz (5 questions)
- Grand Integration Problem: Real-Time Leaderboard (3-part breakdown)
- Preview of Part X

---

## Part X — Graph Theory and Algorithms

### ✅ [Chapter 31 — Graph Fundamentals and Representation](chapters/chapter-31-graph-fundamentals.md)

- Graph terminology
- Directed and undirected graphs
- Weighted and unweighted graphs
- Cyclic and acyclic graphs
- Adjacency matrix
- Adjacency list
- Edge list
- Java implementation
- Graph modelling

---

### ✅ [Chapter 32 — Graph Traversal: BFS and DFS](chapters/chapter-32-graph-traversal.md)

- Breadth-First Search (BFS)
- Depth-First Search (DFS)
- Recursive DFS
- Iterative DFS
- Visited arrays and sets
- Connected components
- Cycle detection
- Bipartite graphs
- Grid traversal
- Multi-source BFS

---

### ✅ [Chapter 33 — Shortest Path Algorithms](chapters/chapter-33-shortest-paths.md)

- Shortest path fundamentals
- BFS for unweighted shortest paths
- Dijkstra's algorithm
- Bellman-Ford algorithm
- Floyd-Warshall (all pairs)
- Negative edge weights
- Path reconstruction
- Algorithm selection

---

### ✅ [Chapter 34 — Minimum Spanning Trees and Disjoint Sets](chapters/chapter-34-mst-and-dsu.md)

- Spanning trees
- MST properties
- Prim's algorithm
- Kruskal's algorithm
- Disjoint Set Union (DSU)
- Union by rank and size
- Path compression
- Applications

---

### ✅ [Chapter 35 — Topological Sorting and DAGs](chapters/chapter-35-topological-sort.md)

- DAG properties
- Dependency resolution
- Kahn's algorithm (BFS-based)
- DFS-based topological sort
- Directed cycle detection
- Longest path in DAGs
- Scheduling problems

---

### ✅ [Chapter 36 — Advanced Graph Algorithms](chapters/chapter-36-advanced-graphs.md)

- Strongly connected components
- Kosaraju's algorithm
- Tarjan's algorithm
- Bridges and articulation points
- Euler paths and circuits
- Network flow
- Maximum flow and minimum cut
- Advanced graph applications

---

### ✅ [Part 10 Review](chapters/part-10-review.md)

---

## Part XI — Greedy Algorithms and Divide and Conquer

### ✅ [Chapter 37 — Greedy Algorithms](chapters/chapter-37-greedy-algorithms.md)

- Greedy choice property
- Optimal substructure
- Exchange arguments
- Activity selection
- Interval scheduling
- Fractional knapsack
- Huffman coding
- Job sequencing
- When greedy fails
- Correctness proofs

---

### ✅ [Chapter 38 — Divide and Conquer](chapters/chapter-38-divide-and-conquer.md)

- Divide, conquer and combine
- Recursive decomposition
- Merge sort (divide and conquer view)
- Quick sort (divide and conquer view)
- Binary search (divide and conquer view)
- Closest pair of points
- Matrix multiplication overview
- Master Theorem applications

---

### ✅ [Chapter 39 — Advanced Problem Decomposition](chapters/chapter-39-advanced-decomposition.md)

- Choosing decomposition boundaries
- Divide and conquer on arrays
- Divide and conquer on trees
- Binary search combinations
- Meet-in-the-middle
- Small-to-large merging
- Combining subproblem results

### ✅ [Part 11 Review](chapters/part-11-review.md)

---

## Part XII — Dynamic Programming

### 🔲 Chapter 40 — Dynamic Programming Foundations

- What is DP?
- Overlapping subproblems
- Optimal substructure
- Recursion to memoisation
- Memoisation vs tabulation
- State definition
- Transition formulation
- Base cases
- State-space design
- Fibonacci introduction

---

### 🔲 Chapter 41 — One-Dimensional and Sequence DP

- Climbing stairs
- House robber
- Maximum subarray (DP view)
- Coin change
- Longest increasing subsequence
- Subsequence problems
- State optimisation
- Space optimisation

---

### 🔲 Chapter 42 — Two-Dimensional and Grid DP

- Grid path counting
- Obstacle paths
- Minimum path sum
- 0/1 knapsack
- Subset sum
- Partition problems
- Two-dimensional transitions
- Rolling array optimisation

---

### 🔲 Chapter 43 — Advanced Dynamic Programming

- Longest common subsequence
- Edit distance
- Interval DP
- Matrix chain multiplication
- DP on trees
- DP on graphs
- Bitmask DP
- Digit DP
- State compression
- Optimisation techniques

---

## Part XIII — Backtracking and Bit Manipulation

### 🔲 Chapter 44 — Backtracking and Combinatorial Search

- State-space search
- Decision trees
- Choose, explore and unchoose
- Subsets
- Permutations
- Combinations
- N-Queens
- Sudoku solver
- Word search
- Pruning
- Backtracking vs brute force
- Relationship to recursion

---

### 🔲 Chapter 45 — Bitmask Patterns and Competitive Programming

- Revisiting bitmask fundamentals (see Chapter 3 for foundations)
- Representing sets with integer bitmasks
- Iterating over all subsets of a bitmask
- Iterating over all submasks of a given mask
- Bitmask DP: state design and transitions
- Classic bitmask DP problems (TSP, assignment, covering)
- State compression techniques
- Bitwise tricks for competitive programming
- Java int and long capacity limits for bitmask DP
- Common pitfalls and debugging bitmask code

---

### 🔲 Chapter 46 — Advanced Bitmasking and State Representation

- Representing sets with bitmasks
- Subset enumeration
- Submask enumeration
- Bitmask DP
- State compression
- Bitwise tricks
- Java int and long limitations
- Common pitfalls

---

## Part XIV — Advanced Data Structures and Range Queries

### 🔲 Chapter 47 — Segment Trees

- Range queries
- Segment tree structure
- Build
- Query
- Point updates
- Range updates
- Lazy propagation
- Iterative segment trees
- Applications

---

### 🔲 Chapter 48 — Fenwick Trees and Range Query Techniques

- Prefix sum revisit
- Fenwick tree (Binary Indexed Tree)
- Point updates
- Prefix queries
- Range queries
- Coordinate compression
- Offline queries
- Segment tree vs Fenwick tree

---

### 🔲 Chapter 49 — Advanced Tree-Based Data Structures

- Sparse tables
- Range minimum queries (RMQ)
- Binary lifting
- Lowest common ancestor (LCA)
- Heavy-light decomposition overview
- Persistent data structures overview

---

### 🔲 Chapter 50 — Advanced Union-Find and Connectivity

- DSU optimisations
- Offline connectivity
- Dynamic connectivity overview
- DSU on trees overview
- Graph applications

---

### 🔲 Chapter 51 — Advanced String and Prefix Structures

- Trie optimisation
- Compressed tries (Patricia/radix trees)
- Suffix arrays
- Suffix trees overview
- Longest common prefix (LCP)
- String indexing
- Advanced pattern matching

---

## Part XV — Advanced Algorithmic Techniques

### 🔲 Chapter 52 — Monotonic Structures and Contribution Techniques

- Monotonic stacks
- Monotonic queues
- Next greater and smaller elements
- Previous greater and smaller elements
- Subarray contribution counting
- Histogram patterns
- Range dominance

---

### 🔲 Chapter 53 — Two Heaps, K-Way Merge and Top-K Patterns

- Two heaps pattern
- Running median
- Kth largest and smallest element
- Top-K frequent elements
- K-way merge
- Multiple sorted streams
- Priority queue applications

---

### 🔲 Chapter 54 — Randomized Algorithms

- Randomisation
- Randomised quicksort
- Randomised selection (quickselect)
- Reservoir sampling
- Probability foundations
- Expected complexity

---

### 🔲 Chapter 55 — Computational Geometry

- Points, lines and vectors
- Distance
- Cross products
- Orientation tests
- Line intersection
- Convex hull
- Sweep line
- Floating-point precision issues

---

### 🔲 Chapter 56 — Advanced Mathematical Algorithms

- Sieve of Eratosthenes
- Prime factorisation
- GCD and LCM revisit
- Fast exponentiation
- Modular exponentiation
- Modular inverses
- Combinatorics
- Number theory patterns

---

### 🔲 Chapter 57 — Advanced Optimisation Techniques

- Meet-in-the-middle
- Coordinate compression
- Offline queries
- Sweep line
- Divide and conquer optimisation
- Monotonic queue optimisation
- Convex hull trick overview
- Recognising optimisation opportunities

---

## Part XVI — Data Structure Design and Systems-Oriented Problems

### 🔲 Chapter 58 — Designing Data Structures

- Abstract data types
- Internal representations
- Implementing a HashMap from scratch
- Stack and queue design
- MinStack
- Circular queue
- Data structure invariants

---

### 🔲 Chapter 59 — Cache and Eviction Design

- LRU cache
- LFU cache
- HashMap + doubly linked list
- Eviction policies
- Complexity requirements
- Java implementation

---

### 🔲 Chapter 60 — Advanced Design Problems

- Design Twitter
- Time-based key-value store
- Randomised set with O(1) operations
- Median finder
- Trie autocomplete
- Task scheduler
- Combining data structures

---

## Part XVII — Algorithmic Paradigms and Complexity Theory

### 🔲 Chapter 61 — Algorithmic Paradigms

- Brute force
- Decrease and conquer
- Divide and conquer
- Greedy
- Dynamic programming
- Backtracking
- Branch and bound
- Randomised algorithms
- Paradigm selection

---

### 🔲 Chapter 62 — Advanced Complexity Theory

- P, NP and NP-completeness
- Polynomial vs exponential time
- Reductions
- NP-hardness
- Decision problems
- Approximation algorithms
- Complexity trade-offs

---

### 🔲 Chapter 63 — Memory, Performance and Java Optimisation

- JVM memory basics
- Stack and heap
- Object allocation
- Garbage collection basics
- Boxing and unboxing
- Primitive vs wrapper performance
- Array vs collection trade-offs
- Recursion stack depth
- Avoiding unnecessary allocations
- Performance measurement pitfalls

---

### 🔲 Chapter 64 — Competitive Programming and Java Problem-Solving Toolkit

- LeetCode environment
- Java versions in competitive programming
- Fast input/output
- Efficient string construction
- Custom comparators
- PriorityQueue patterns
- Array and collection performance
- Overflow prevention
- Recursion depth limits
- Runtime and compilation errors
- Constraint-driven optimisation

---

## Part XVIII — Interview Mastery

### 🔲 Chapter 65 — Problem-Solving Patterns Catalogue

- Two pointers
- Sliding window
- Fast and slow pointers
- Prefix sums
- Binary search
- Merge intervals
- Cyclic sort
- Monotonic stack and queue
- Top-K
- K-way merge
- BFS and DFS
- Backtracking
- Dynamic programming
- Bitmasking
- Union-Find
- Pattern selection heuristics

---

### 🔲 Chapter 66 — Recognising Patterns in Unfamiliar Problems

- Reading constraints
- Hidden structure
- Repeated work
- Monotonicity
- State transitions
- Candidate data structures
- Distinguishing similar patterns
- Avoiding pattern overfitting

---

### 🔲 Chapter 67 — Hard Problem Solving

- Multi-pattern problems
- Graph + DP
- Advanced binary search
- Greedy + sorting
- Tree + recursion
- State-space search
- Constraint handling
- Deriving solutions independently

---

### 🔲 Chapter 68 — Google-Style Interview Framework

- Understanding the problem
- Clarifying questions
- Examples and test cases
- Brute force to optimal progression
- Trade-offs
- Edge cases
- Clean Java
- Complexity analysis
- Communication strategies

---

### 🔲 Chapter 69 — Coding Interview Practice

- Easy foundations
- Medium mastery
- Hard reasoning
- Timed practice
- Mock interviews
- Reviewing failed attempts
- Knowledge-gap analysis

---

### 🔲 Chapter 70 — Advanced Interview Topics

- Algorithm design under constraints
- Follow-up questions
- Optimising existing solutions
- Alternative data structures
- Large input handling
- Test-case design
- Correctness reasoning

---

### 🔲 Chapter 71 — Interview-Level Data Structure Design

- LRU and LFU
- Complex API design
- Combining structures
- Invariants and guarantees
- Concurrency concepts (overview)
- Design trade-offs

---

### 🔲 Chapter 72 — Problem-Solving Communication

- Thinking aloud
- Structuring explanations
- Brute-force presentation
- Justifying optimisation
- Correctness communication
- Complexity communication
- Responding to hints

---

### 🔲 Chapter 73 — Building an Independent Problem-Solving Practice

- Deliberate practice
- Spaced repetition
- Revisiting unsolved problems
- Pattern notebook
- Mistake analysis
- Mastery tracking

---

### 🔲 Chapter 74 — Final Mastery and Mock Interview Collection

- Mixed-topic problems
- Unseen problem sets
- Google-level practice problems
- Full mock interviews
- Revision framework
- Long-term roadmap

---

*This index reflects the planned curriculum. Chapters are written and published sequentially. Each chapter builds on the foundations established by its predecessors.*
