# Chapter 19 — HashMap, HashSet and Problem Patterns

> **Part V — Hashing and Lookup Structures**  
> [← Chapter 18](chapter-18-hash-tables.md) · [← Index](../INDEX.md) · [→ Chapter 20](../INDEX.md#-chapter-20--ordered-maps-and-advanced-hashing)

---

## Learning Objectives

By the end of this chapter you will be able to:

- Use all core `HashMap` and `HashSet` API methods correctly, including `merge`, `computeIfAbsent`, and `getOrDefault`.
- Explain Java's `hashCode`/`equals` contract and why violating it corrupts a `HashMap`.
- Apply seven canonical hash-based problem patterns from memory.
- Recognise which pattern applies to a given problem and implement it cleanly.

---

## Contents

- [19.1 Java HashMap Internals](#191-java-hashmap-internals)
- [19.2 Java HashSet Internals](#192-java-hashset-internals)
- [19.3 The hashCode/equals Contract](#193-the-hashcodeequals-contract)
- [19.4 Core API Reference](#194-core-api-reference)
- [19.5 Pattern 1 — Frequency Counting](#195-pattern-1--frequency-counting)
- [19.6 Pattern 2 — Duplicate Detection and First Seen](#196-pattern-2--duplicate-detection-and-first-seen)
- [19.7 Pattern 3 — Two Sum and Complement Lookup](#197-pattern-3--two-sum-and-complement-lookup)
- [19.8 Pattern 4 — Grouping and Categorisation](#198-pattern-4--grouping-and-categorisation)
- [19.9 Pattern 5 — Prefix Sum with HashMap](#199-pattern-5--prefix-sum-with-hashmap)
- [19.10 Pattern 6 — Sliding Window with Frequency Map](#1910-pattern-6--sliding-window-with-frequency-map)
- [19.11 Pattern 7 — Index and Position Tracking](#1911-pattern-7--index-and-position-tracking)
- [Interview Insight](#interview-insight)
- [Quick Quiz](#quick-quiz)
- [19.12 Practice Problems](#1912-practice-problems)
- [19.13 Hard Problem Corner](#1913-hard-problem-corner)
- [Chapter Summary](#chapter-summary)

---

> [!NOTE]
> **Historical Context**  
> Java's `HashMap` was introduced in **Java 1.2 (1998)** as part of the Collections Framework, designed by Joshua Bloch. Before this, Java developers used `Hashtable` (synchronised, slow). The Collections Framework's key design decision was to separate the interface (`Map<K,V>`) from the implementation (`HashMap`, `TreeMap`, `LinkedHashMap`), making it easy to swap implementations. The seven problem patterns in this chapter were not invented in a single paper; they emerged from decades of practical use and were codified by the competitive programming and interview prep communities in the 2010s.

> [!TIP]
> **Did You Know?**  
> Python's `dict` has preserved **insertion order** since Python **3.7 (2016)** — making it closer to Java's `LinkedHashMap` than to `HashMap`. Before 3.7, Python dicts were unordered (similar to `HashMap`). Java's `HashMap` deliberately does *not* preserve insertion order because doing so would require extra bookkeeping (the `prev`/`next` pointers in `LinkedHashMap`'s entry nodes). If you need ordered iteration in Java, use `LinkedHashMap`. If you need sorted order, use `TreeMap`.

---

## 19.1 Java HashMap Internals

`HashMap<K, V>` is backed by an array of **buckets** (linked lists, converted to red-black trees when too long).

**Key parameters:**
- **Initial capacity:** number of buckets at creation (default: 16, must be a power of 2).
- **Load factor:** threshold before resize (default: 0.75).
- **Resize trigger:** when `size > capacity × loadFactor`, capacity doubles and all entries rehash.

```java
// Default: initial capacity 16, load factor 0.75
HashMap<String, Integer> map = new HashMap<>();

// Pre-sized to avoid rehashing when you know approximate size
HashMap<String, Integer> map = new HashMap<>(expectedSize * 2);  // double to stay below 0.75

// Custom load factor (rarely needed)
HashMap<String, Integer> map = new HashMap<>(16, 0.5f);
```

**Bucket structure (Java 8+):**
1. Array of `Node<K,V>` (linked list nodes).
2. When a bucket's chain exceeds **8 entries** AND total capacity ≥ 64: converts to `TreeNode` (red-black tree) → O(log n) per bucket.
3. When chain shrinks to **6 entries**: converts back to linked list.

**Iteration order:** `HashMap` does not maintain insertion or sorted order. Iteration order is arbitrary and can change between JVM runs (randomised hash seed).

---

## 19.2 Java HashSet Internals

`HashSet<E>` is literally a `HashMap<E, PRESENT>` where `PRESENT` is a static dummy object. Every `HashSet` operation delegates to `HashMap`.

```java
HashSet<Integer> set = new HashSet<>();
// Equivalent to:
HashMap<Integer, Object> map = new HashMap<>();
Object PRESENT = new Object();
map.put(e, PRESENT);     // set.add(e)
map.containsKey(e);       // set.contains(e)
map.remove(e);            // set.remove(e)
```

**Consequence:** `HashSet` has the same O(1) average complexity and the same `hashCode/equals` requirements as `HashMap`.

---

## 19.3 The hashCode/equals Contract

For `HashMap` and `HashSet` to work correctly, stored keys must satisfy:

1. **Consistency:** `k.hashCode()` returns the same value every time it is called on the same object during a JVM run.
2. **Equality implies equal hash:** if `a.equals(b)`, then `a.hashCode() == b.hashCode()`.
3. **Non-equality doesn't require different hashes:** `a.hashCode() == b.hashCode()` does NOT imply `a.equals(b)` (this is a collision, handled by chaining).

> [!WARNING]
> **Violating the contract causes silent data corruption.** If you override `equals` without overriding `hashCode` (or vice versa), `HashMap.get(key)` will silently fail to find entries that were put with an equal key — the entry is in a different bucket because `hashCode` changed. Java's `Objects.hash(fields...)` and `Objects.equals` are the standard building blocks for custom `hashCode`/`equals`.

```java
// Correct custom key
record Point(int x, int y) {
    // Java records auto-generate equals() and hashCode() based on all fields
}

// If writing manually:
@Override public boolean equals(Object o) {
    if (this == o) return true;
    if (!(o instanceof Point)) return false;
    Point p = (Point) o;
    return x == p.x && y == p.y;
}
@Override public int hashCode() { return Objects.hash(x, y); }
```

---

## 19.4 Core API Reference

```java
Map<K, V> map = new HashMap<>();

// Insert / update
map.put(key, value);                             // returns old value or null
map.putIfAbsent(key, value);                     // only if key not present

// Read
map.get(key);                                    // null if absent
map.getOrDefault(key, defaultVal);               // safe — no null check needed
map.containsKey(key);                            // O(1)
map.containsValue(value);                        // O(n) — scans all values

// Update atomically
map.merge(key, 1, Integer::sum);                 // freq++ in one line
map.compute(key, (k, v) -> v == null ? 1 : v+1); // same, more flexible
map.computeIfAbsent(key, k -> new ArrayList<>());// initialise if missing

// Remove
map.remove(key);                                 // returns removed value or null
map.remove(key, value);                          // only if key maps to this exact value

// Iteration
for (Map.Entry<K,V> e : map.entrySet())  { e.getKey(); e.getValue(); }
map.forEach((k, v) -> process(k, v));
map.keySet();     // Set<K> view
map.values();     // Collection<V> view
map.entrySet();   // Set<Map.Entry<K,V>> view

// Size
map.size();       // number of entries
map.isEmpty();
```

**`merge` is the most useful one-liner for counting:**

```java
map.merge(key, 1, Integer::sum);
// Equivalent to:
map.put(key, map.getOrDefault(key, 0) + 1);
```

---

## 19.5 Pattern 1 — Frequency Counting

**Use:** Count occurrences of elements. The foundation of anagram detection, most-frequent-k, character histograms.

```java
// Count character frequencies
Map<Character, Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);

// Or with int[26] for lowercase-only (faster):
int[] freq = new int[26];
for (char c : s.toCharArray()) freq[c - 'a']++;

// Find the k most frequent elements
Map<Integer, Integer> count = new HashMap<>();
for (int x : nums) count.merge(x, 1, Integer::sum);

// Bucket sort by frequency for O(n) top-k
List<Integer>[] buckets = new List[nums.length + 1];
count.forEach((val, f) -> {
    if (buckets[f] == null) buckets[f] = new ArrayList<>();
    buckets[f].add(val);
});

List<Integer> result = new ArrayList<>();
for (int f = buckets.length - 1; f >= 1 && result.size() < k; f--) {
    if (buckets[f] != null) result.addAll(buckets[f]);
}
```

---

## 19.6 Pattern 2 — Duplicate Detection and First Seen

**Use:** Has this element appeared before? Where did it first appear? Find the first duplicate, the first non-repeating character.

```java
// Detect any duplicate — O(n)
boolean hasDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int x : nums) if (!seen.add(x)) return true;  // add() returns false on duplicate
    return false;
}

// Find first duplicate
int firstDuplicate(int[] nums) {
    Set<Integer> seen = new HashSet<>();
    for (int x : nums) {
        if (!seen.add(x)) return x;
    }
    return -1;
}

// First non-repeating character
char firstUnique(String s) {
    int[] freq = new int[26];
    for (char c : s.toCharArray()) freq[c - 'a']++;
    for (char c : s.toCharArray()) if (freq[c - 'a'] == 1) return c;
    return ' ';
}
```

---

## 19.7 Pattern 3 — Two Sum and Complement Lookup

**Use:** Find pairs/triplets with a target sum. Extends to difference, product, and ratio queries.

```java
// Two Sum — unsorted array, O(n) time and space
int[] twoSum(int[] nums, int target) {
    Map<Integer, Integer> complement = new HashMap<>();
    for (int i = 0; i < nums.length; i++) {
        if (complement.containsKey(nums[i])) {
            return new int[]{complement.get(nums[i]), i};
        }
        complement.put(target - nums[i], i);  // store what we need
    }
    return new int[]{-1, -1};
}

// Two Sum — count all pairs (not just indices)
int countPairs(int[] nums, int target) {
    Map<Integer, Integer> freq = new HashMap<>();
    int count = 0;
    for (int x : nums) {
        count += freq.getOrDefault(target - x, 0);
        freq.merge(x, 1, Integer::sum);
    }
    return count;
}

// Four Sum Count — count quadruples (a+b+c+d == target)
int fourSumCount(int[] A, int[] B, int[] C, int[] D) {
    Map<Integer, Integer> ab = new HashMap<>();
    for (int a : A) for (int b : B) ab.merge(a + b, 1, Integer::sum);
    int count = 0;
    for (int c : C) for (int d : D) count += ab.getOrDefault(-(c + d), 0);
    return count;
}
```

---

## 19.8 Pattern 4 — Grouping and Categorisation

**Use:** Partition elements into groups that share a computed key (anagrams, equal remainders, same digit sets).

```java
// Group Anagrams
List<List<String>> groupAnagrams(String[] strs) {
    Map<String, List<String>> groups = new HashMap<>();
    for (String s : strs) {
        char[] chars = s.toCharArray();
        Arrays.sort(chars);
        String key = new String(chars);           // sorted string as canonical key
        groups.computeIfAbsent(key, k -> new ArrayList<>()).add(s);
    }
    return new ArrayList<>(groups.values());
}

// Group by remainder
Map<Integer, List<Integer>> groupByRemainder(int[] nums, int k) {
    Map<Integer, List<Integer>> groups = new HashMap<>();
    for (int x : nums) {
        int rem = ((x % k) + k) % k;              // non-negative remainder
        groups.computeIfAbsent(rem, r -> new ArrayList<>()).add(x);
    }
    return groups;
}
```

**`computeIfAbsent`** is the idiomatic Java way to initialise a list in a map on first access:

```java
map.computeIfAbsent(key, k -> new ArrayList<>()).add(element);
// replaces the verbose:
map.putIfAbsent(key, new ArrayList<>());
map.get(key).add(element);
```

---

## 19.9 Pattern 5 — Prefix Sum with HashMap

Covered in depth in [Chapter 10](chapter-10-prefix-sums.md#103-subarray-sum-problems-with-hashmap). The canonical pattern for counting subarrays with sum = k (including negative values).

```java
int subarraySum(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);            // empty prefix
    int prefix = 0, count = 0;
    for (int x : nums) {
        prefix += x;
        count += freq.getOrDefault(prefix - k, 0);
        freq.merge(prefix, 1, Integer::sum);
    }
    return count;
}
```

**Extension — Subarray with XOR = k** (from [Chapter 10](chapter-10-prefix-sums.md#107-prefix-xor)):

```java
int subarrayXOR(int[] nums, int k) {
    Map<Integer, Integer> freq = new HashMap<>();
    freq.put(0, 1);
    int prefix = 0, count = 0;
    for (int x : nums) {
        prefix ^= x;
        count += freq.getOrDefault(prefix ^ k, 0);
        freq.merge(prefix, 1, Integer::sum);
    }
    return count;
}
```

---

## 19.10 Pattern 6 — Sliding Window with Frequency Map

Covered in [Chapter 9](chapter-09-sliding-window.md#95-frequency-maps-within-windows). The window invariant is tracked via a frequency map; the window shrinks when the invariant is violated.

```java
// Longest substring with at most k distinct characters
int longestSubstringKDistinct(String s, int k) {
    Map<Character, Integer> freq = new HashMap<>();
    int left = 0, best = 0;
    for (int right = 0; right < s.length(); right++) {
        freq.merge(s.charAt(right), 1, Integer::sum);
        while (freq.size() > k) {
            char lc = s.charAt(left++);
            freq.merge(lc, -1, Integer::sum);
            if (freq.get(lc) == 0) freq.remove(lc);
        }
        best = Math.max(best, right - left + 1);
    }
    return best;
}
```

---

## 19.11 Pattern 7 — Index and Position Tracking

**Use:** Store the index of the first (or last) occurrence of each element. Find the longest subarray with unique elements, the minimum-length subarray containing a target, or the interval between two occurrences.

```java
// Longest subarray with all unique elements
int longestUniqueSubarray(int[] nums) {
    Map<Integer, Integer> lastSeen = new HashMap<>();
    int left = 0, best = 0;
    for (int right = 0; right < nums.length; right++) {
        if (lastSeen.containsKey(nums[right])) {
            left = Math.max(left, lastSeen.get(nums[right]) + 1);  // jump left past duplicate
        }
        lastSeen.put(nums[right], right);
        best = Math.max(best, right - left + 1);
    }
    return best;
}

// Find the maximum distance between equal elements
int maxEqualDistance(int[] nums) {
    Map<Integer, Integer> firstIndex = new HashMap<>();
    int maxDist = 0;
    for (int i = 0; i < nums.length; i++) {
        if (firstIndex.containsKey(nums[i])) {
            maxDist = Math.max(maxDist, i - firstIndex.get(nums[i]));
        } else {
            firstIndex.put(nums[i], i);  // only store first occurrence
        }
    }
    return maxDist;
}
```

---

## Interview Insight

> ### 🎯 Interview Insight
> **The HashMap Instinct Check**
>
> When you feel the urge to write a nested `for` loop, ask yourself: "Am I doing a lookup of one collection against another?" If yes, the first loop should build a `HashMap`; the second loop does O(1) lookups against it. This converts O(n²) to O(n) in one structural change.
>
> **Pattern recognition in 5 seconds:**
> - *"Count how many times…"* → Frequency map (Pattern 1)
> - *"Have we seen X before?"* → HashSet `contains` / `add` (Pattern 2)
> - *"Find two elements that…"* → Complement map (Pattern 3)
> - *"Group elements by…"* → `computeIfAbsent` with key = canonical form (Pattern 4)
> - *"Subarrays with sum/XOR = k"* → Prefix + HashMap (Pattern 5)
> - *"Longest/shortest window with property"* → Sliding window + freq map (Pattern 6)
> - *"Longest subarray with no repeats / distance between repeats"* → Index tracking map (Pattern 7)

---

## Quick Quiz

**Q1.** What does `map.merge(key, 1, Integer::sum)` do when the key already has value 3?

<details><summary><strong>Answer</strong></summary>

> It applies `Integer::sum(3, 1) = 4` and updates the value to 4. If the key were absent, it would put the value 1. `merge` is equivalent to `map.put(key, map.getOrDefault(key, 0) + 1)` but more concise.

</details>

---

**Q2.** Why does `HashSet.add(e)` return `false` for a duplicate, and why is this useful?

<details><summary><strong>Answer</strong></summary>

> `add` returns `true` if the element was inserted (not already present), `false` if the element was already in the set. This allows duplicate detection in one line: `if (!seen.add(x)) return true; // duplicate found` — avoiding a separate `contains` call.

</details>

---

**Q3.** What goes wrong if you use a mutable list as a `HashMap` key and then mutate it?

<details><summary><strong>Answer</strong></summary>

> Mutating the list changes its `hashCode()`. The entry is now stored in the wrong bucket (the original one, determined when it was inserted). `get(list)` will hash to the new bucket, find nothing, and return `null` — even though the entry exists. Never use mutable objects as `HashMap` keys.

</details>

---

## 19.12 Practice Problems

| Problem | Name | Difficulty | Pattern |
|---------|------|------------|---------|
| [LC 1](https://leetcode.com/problems/two-sum/) | Two Sum | Easy | Pattern 3 |
| [LC 217](https://leetcode.com/problems/contains-duplicate/) | Contains Duplicate | Easy | Pattern 2 |
| [LC 383](https://leetcode.com/problems/ransom-note/) | Ransom Note | Easy | Pattern 1 |
| [LC 49](https://leetcode.com/problems/group-anagrams/) | Group Anagrams | Medium | Pattern 4 |
| [LC 347](https://leetcode.com/problems/top-k-frequent-elements/) | Top K Frequent Elements | Medium | Pattern 1 + bucket sort |
| [LC 560](https://leetcode.com/problems/subarray-sum-equals-k/) | Subarray Sum Equals K | Medium | Pattern 5 |
| [LC 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Longest Substring Without Repeating Chars | Medium | Pattern 7 |
| [LC 325](https://leetcode.com/problems/maximum-size-subarray-sum-equals-k/) | Maximum Size Subarray Sum Equals K | Medium | Pattern 5 (longest, not count) |
| [LC 525](https://leetcode.com/problems/contiguous-array/) | Contiguous Array | Medium | Pattern 5 |
| [LC 454](https://leetcode.com/problems/4sum-ii/) | 4Sum II | Medium | Pattern 3 |

---

## 19.13 Hard Problem Corner

> ### 🔥 Hard Problem Corner
>
> **Problem: LRU Cache** ([LC 146](https://leetcode.com/problems/lru-cache/))
>
> Design a data structure that supports `get(key)` and `put(key, value)` in O(1), evicting the **least recently used** entry when capacity is exceeded.
>
> **Key insight:** Combine a `HashMap` for O(1) lookup with a **doubly linked list** for O(1) ordered eviction. The list maintains recency order; the map stores key → node references. `get` moves a node to the head; `put` adds to the head and removes from the tail when over capacity.

<details><summary><strong>Solution sketch (click to reveal)</strong></summary>

> [!TIP]
> ```java
> class LRUCache {
>     private final int capacity;
>     private final Map<Integer, Node> map;
>     private final Node head, tail;   // sentinel nodes
>
>     LRUCache(int capacity) {
>         this.capacity = capacity;
>         this.map = new HashMap<>();
>         head = new Node(0, 0); tail = new Node(0, 0);
>         head.next = tail; tail.prev = head;
>     }
>
>     int get(int key) {
>         if (!map.containsKey(key)) return -1;
>         Node node = map.get(key);
>         moveToHead(node);
>         return node.val;
>     }
>
>     void put(int key, int value) {
>         if (map.containsKey(key)) {
>             Node node = map.get(key);
>             node.val = value;
>             moveToHead(node);
>         } else {
>             Node node = new Node(key, value);
>             map.put(key, node);
>             addToHead(node);
>             if (map.size() > capacity) {
>                 Node lru = removeTail();
>                 map.remove(lru.key);
>             }
>         }
>     }
>     // ... doubly linked list helpers: addToHead, removeNode, moveToHead, removeTail
> }
> ```
>
> Java provides `LinkedHashMap` with access-order mode that implements LRU directly — but implementing from scratch demonstrates HashMap + DLL composition.

</details>

---

## Chapter Summary

| Pattern | Signal phrase | Data structure |
|---------|-------------|----------------|
| Frequency counting | "count how many times" | `HashMap<K, Integer>` or `int[26/128]` |
| Duplicate detection | "have we seen before" | `HashSet` |
| Complement lookup | "find pair/triple with sum" | `HashMap<complement, index>` |
| Grouping | "group by / categorise" | `HashMap<key, List<>>` + `computeIfAbsent` |
| Prefix + map | "subarray sum/XOR = k" | `HashMap<prefixSum, count>` |
| Sliding window + map | "longest/shortest window" | `HashMap<char, freq>` + shrink loop |
| Index tracking | "first seen / max distance" | `HashMap<val, firstIndex>` |

*Table 19.1: The seven HashMap problem patterns.*

---

> **Part V — Hashing and Lookup Structures**  
> [← Chapter 18](chapter-18-hash-tables.md) · [← Index](../INDEX.md) · [→ Chapter 20 (planned)](../INDEX.md#-chapter-20--ordered-maps-and-advanced-hashing)
