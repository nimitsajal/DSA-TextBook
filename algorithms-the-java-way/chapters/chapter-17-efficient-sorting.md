# Chapter 17 — Efficient and Non-Comparison Sorting

> **Part IV — Searching and Sorting**  
> [← Chapter 16](chapter-16-elementary-sorting.md) · [← Index](../INDEX.md) · [→ Part IV Review](part-04-review.md)

---

## Learning Objectives

By the end of this chapter you will be able to:

- Implement merge sort and explain why it is O(n log n) and stable.
- Implement quicksort with Lomuto and Hoare partitions; explain the O(n²) worst case and how randomisation avoids it.
- Build a max-heap with `heapify` and implement heap sort in O(n log n) O(1) space.
- Apply counting, radix, and bucket sort to integer and string inputs.
- State the Ω(n log n) comparison-sort lower bound and why non-comparison sorts can break it.
- Choose the right Java sort API and understand what it uses internally.

---

## Contents

- [17.1 The Ω(n log n) Lower Bound](#171-the-n-log-n-lower-bound)
- [17.2 Merge Sort](#172-merge-sort)
- [17.3 Quick Sort](#173-quick-sort)
- [17.4 Heap Sort](#174-heap-sort)
- [17.5 Counting Sort](#175-counting-sort)
- [17.6 Radix Sort](#176-radix-sort)
- [17.7 Bucket Sort](#177-bucket-sort)
- [17.8 Java Arrays.sort Internals](#178-java-arrayssort-internals)
- [17.9 Sorting Objects and Custom Comparators](#179-sorting-objects-and-custom-comparators)
- [17.10 Algorithm Selection Guide](#1710-algorithm-selection-guide)
- [Interview Insight](#interview-insight)
- [Quick Quiz](#quick-quiz)
- [17.11 Practice Problems](#1711-practice-problems)
- [17.12 Hard Problem Corner](#1712-hard-problem-corner)
- [Chapter Summary](#chapter-summary)

---

## 17.1 The Ω(n log n) Lower Bound

**Claim:** Any comparison-based sorting algorithm requires at least $\Omega(n \log n)$ comparisons in the worst case.

**Proof sketch (decision tree):** Every comparison sort corresponds to a binary decision tree. Each internal node is a comparison `arr[i] vs arr[j]`; each leaf is a possible final ordering. For $n$ elements there are $n!$ possible orderings, so the tree has at least $n!$ leaves. The depth of a binary tree with $L$ leaves is at least $\log_2 L$. Therefore:

$$\text{depth} \geq \log_2(n!) \approx n \log_2 n - n \log_2 e = \Omega(n \log n)$$

(using Stirling's approximation: $\log_2(n!) = \sum_{k=1}^{n} \log_2 k \approx n \log_2 n$).

**Consequence:** Merge sort and heap sort are **optimal** comparison sorts. No comparison-based algorithm can do better in the worst case. Breaking O(n log n) requires non-comparison techniques (Sections 17.5–17.7).

---

## 17.2 Merge Sort

**Idea:** Divide the array in half, recursively sort each half, then merge the two sorted halves.

```java
void mergeSort(int[] arr, int lo, int hi) {
    if (lo >= hi) return;                        // base case: 0 or 1 element
    int mid = lo + (hi - lo) / 2;
    mergeSort(arr, lo, mid);
    mergeSort(arr, mid + 1, hi);
    merge(arr, lo, mid, hi);
}

void merge(int[] arr, int lo, int mid, int hi) {
    int[] tmp = Arrays.copyOfRange(arr, lo, hi + 1);
    int left = 0, right = mid - lo + 1;         // pointers into tmp
    for (int k = lo; k <= hi; k++) {
        if      (left  > mid - lo)         arr[k] = tmp[right++];
        else if (right > hi - lo)          arr[k] = tmp[left++];
        else if (tmp[left] <= tmp[right])  arr[k] = tmp[left++];   // <= preserves stability
        else                               arr[k] = tmp[right++];
    }
}
```

**Complexity:**
- Time: $T(n) = 2T(n/2) + O(n)$ → $O(n \log n)$ (Master Theorem Case 2).
- Space: $O(n)$ for the auxiliary merge buffer.
- Stable: **Yes** — the `<=` in the merge preserves relative order of equal elements.
- Not in-place (requires O(n) extra space — the trade-off for stability).

**Recurrence visualised:** At every level of the recursion tree, the total merge work is $O(n)$. There are $\log n$ levels → $O(n \log n)$ total.

> [!TIP]
> **Bottom-up merge sort** avoids recursion entirely. Start with runs of size 1, merge into runs of size 2, then 4, 8, ... This is the basis of **Timsort**: detect and exploit already-sorted runs (from insertion sort) instead of using fixed-size runs of 1.

---

## 17.3 Quick Sort

**Idea:** Choose a pivot, partition the array so elements ≤ pivot are left and elements > pivot are right, then recursively sort each partition.

### Lomuto Partition

```java
int partitionLomuto(int[] arr, int lo, int hi) {
    int pivot = arr[hi];       // last element as pivot
    int i = lo - 1;            // i: boundary of left partition
    for (int j = lo; j < hi; j++) {
        if (arr[j] <= pivot) swap(arr, ++i, j);
    }
    swap(arr, i + 1, hi);      // place pivot
    return i + 1;
}

void quickSortLomuto(int[] arr, int lo, int hi) {
    if (lo >= hi) return;
    int pivot = partitionLomuto(arr, lo, hi);
    quickSortLomuto(arr, lo, pivot - 1);
    quickSortLomuto(arr, pivot + 1, hi);
}
```

### Hoare Partition (Faster in Practice)

```java
int partitionHoare(int[] arr, int lo, int hi) {
    int pivot = arr[lo + (hi - lo) / 2];   // middle element as pivot value
    int i = lo - 1, j = hi + 1;
    while (true) {
        do { i++; } while (arr[i] < pivot);
        do { j--; } while (arr[j] > pivot);
        if (i >= j) return j;              // partition point
        swap(arr, i, j);
    }
}

void quickSortHoare(int[] arr, int lo, int hi) {
    if (lo >= hi) return;
    int pivot = partitionHoare(arr, lo, hi);
    quickSortHoare(arr, lo, pivot);
    quickSortHoare(arr, pivot + 1, hi);
}
```

**Hoare vs Lomuto:** Hoare makes about 3× fewer swaps in practice and handles duplicates better. Lomuto is simpler to understand.

### Complexity

| Case | Complexity | Trigger |
|------|-----------|---------|
| Best | O(n log n) | Pivot always splits evenly |
| Average | O(n log n) | Random input |
| Worst | **O(n²)** | Already sorted, pivot = min/max |

*Table 17.1: Quicksort complexity.*

**Avoiding O(n²):** Randomise the pivot by swapping `arr[lo]` with a random element in `[lo, hi]` before partitioning. This makes worst-case input impossible to construct deterministically.

```java
void randomQuickSort(int[] arr, int lo, int hi, Random rng) {
    if (lo >= hi) return;
    int r = lo + rng.nextInt(hi - lo + 1);
    swap(arr, r, hi);                          // random pivot to last position
    int p = partitionLomuto(arr, lo, hi);
    randomQuickSort(arr, lo, p - 1, rng);
    randomQuickSort(arr, p + 1, hi, rng);
}
```

### Three-Way Quicksort (for Many Duplicates)

Standard quicksort degrades to O(n²) on all-equal input. Three-way partitioning (Dutch National Flag from Chapter 8) groups elements into `< pivot`, `== pivot`, `> pivot`. Equal elements are placed permanently, reducing future work.

```java
void threeWayQuickSort(int[] arr, int lo, int hi) {
    if (lo >= hi) return;
    int pivot = arr[lo + (hi - lo) / 2];
    int lt = lo, gt = hi, i = lo;
    while (i <= gt) {
        int cmp = Integer.compare(arr[i], pivot);
        if      (cmp < 0) swap(arr, lt++, i++);
        else if (cmp > 0) swap(arr, i, gt--);
        else              i++;
    }
    // arr[lo..lt-1] < pivot, arr[lt..gt] == pivot, arr[gt+1..hi] > pivot
    threeWayQuickSort(arr, lo, lt - 1);
    threeWayQuickSort(arr, gt + 1, hi);
}
```

---

## 17.4 Heap Sort

**Idea:** Build a max-heap in O(n), then extract the maximum n times in O(log n) each — placing elements in reverse at the end of the array.

```java
void heapSort(int[] arr) {
    int n = arr.length;

    // Phase 1: Build max-heap (bottom-up heapify — O(n))
    for (int i = n / 2 - 1; i >= 0; i--) heapify(arr, n, i);

    // Phase 2: Extract max n-1 times (O(n log n))
    for (int i = n - 1; i > 0; i--) {
        swap(arr, 0, i);        // move current max to end
        heapify(arr, i, 0);     // restore heap property for size i
    }
}

void heapify(int[] arr, int heapSize, int root) {
    int largest = root;
    int left = 2 * root + 1, right = 2 * root + 2;
    if (left  < heapSize && arr[left]  > arr[largest]) largest = left;
    if (right < heapSize && arr[right] > arr[largest]) largest = right;
    if (largest != root) {
        swap(arr, root, largest);
        heapify(arr, heapSize, largest);  // cascade down
    }
}
```

**Why Phase 1 is O(n):** Heapifying a node at depth $d$ takes $O(h - d)$ where $h = \log n$. Summing over all nodes: $\sum_{d=0}^{h} 2^d (h-d) = O(2^h) = O(n)$.

**Complexity:**
- Time: O(n log n) — optimal worst case.
- Space: **O(1)** — in-place! This is heap sort's key advantage over merge sort.
- Stable: **No** — the heap operations can change relative order of equal elements.

---

## 17.5 Counting Sort

**Idea:** Count the frequency of each distinct value, then reconstruct the array in order. No comparisons needed.

```java
int[] countingSort(int[] arr, int maxVal) {
    int[] count = new int[maxVal + 1];
    for (int x : arr) count[x]++;

    // Accumulate counts (prefix sum) for stable placement
    for (int i = 1; i <= maxVal; i++) count[i] += count[i - 1];

    // Place elements in output (iterate backwards for stability)
    int[] output = new int[arr.length];
    for (int i = arr.length - 1; i >= 0; i--) {
        output[--count[arr[i]]] = arr[i];
    }
    return output;
}
```

**Complexity:**
- Time: O(n + k) where k = range of values.
- Space: O(n + k).
- Stable: **Yes** (with the backwards iteration trick).
- **Requires:** Integer keys in a known, bounded range [0, k]. Not applicable to arbitrary objects.

**When to use:** Sorting integers in a small range (e.g., sort 10⁶ values in [0, 100]). O(n + k) where k = 100 is essentially O(n).

---

## 17.6 Radix Sort

**Idea:** Sort digit by digit (or character by character), from least significant to most significant (LSD), using a stable sort for each digit. After processing all digits, the array is sorted.

```java
void radixSort(int[] arr) {
    int maxVal = Arrays.stream(arr).max().getAsInt();

    // Sort by each digit position (1s, 10s, 100s, ...)
    for (int exp = 1; maxVal / exp > 0; exp *= 10) {
        countingSortByDigit(arr, exp);
    }
}

void countingSortByDigit(int[] arr, int exp) {
    int n = arr.length;
    int[] output = new int[n];
    int[] count = new int[10];

    for (int x : arr)         count[(x / exp) % 10]++;
    for (int i = 1; i < 10; i++) count[i] += count[i - 1];
    for (int i = n - 1; i >= 0; i--) {
        int digit = (arr[i] / exp) % 10;
        output[--count[digit]] = arr[i];
    }
    System.arraycopy(output, 0, arr, 0, n);
}
```

**Complexity:**
- Time: O(d × (n + 10)) = **O(dn)** where d = number of digits.
- Space: O(n + 10) = O(n).
- Stable: **Yes** (because each pass uses stable counting sort).
- **Requires:** Integers (or strings of bounded length). Not comparison-based.

**Example:** Sorting 10⁶ 32-bit integers. d = 10 decimal digits (or 4 bytes/passes for base-256 radix sort). O(4n) ≈ 4 × 10⁶ — faster than O(n log n) ≈ 20 × 10⁶.

---

## 17.7 Bucket Sort

**Idea:** Distribute elements into k buckets based on value range, sort each bucket (usually with insertion sort), then concatenate.

```java
void bucketSort(double[] arr) {
    int n = arr.length;
    List<List<Double>> buckets = new ArrayList<>();
    for (int i = 0; i < n; i++) buckets.add(new ArrayList<>());

    for (double x : arr) {
        int idx = (int)(x * n);     // assumes values in [0, 1)
        buckets.get(Math.min(idx, n - 1)).add(x);
    }

    int pos = 0;
    for (List<Double> bucket : buckets) {
        Collections.sort(bucket);   // insertion sort within each bucket
        for (double x : bucket) arr[pos++] = x;
    }
}
```

**Complexity:**
- Average: **O(n + k)** when input is uniformly distributed.
- Worst: O(n²) if all elements land in one bucket.
- Best for: uniformly distributed floating-point values in [0, 1).

---

## 17.8 Java Arrays.sort Internals

| Method | Algorithm | Stable | Notes |
|--------|-----------|--------|-------|
| `Arrays.sort(int[])` | Dual-pivot Quicksort | ❌ No | Yaroslavskiy (2009). ~10% faster than 1-pivot. |
| `Arrays.sort(Object[])` | Timsort | ✅ Yes | Hybrid merge + insertion sort. O(n) on sorted/reverse-sorted. |
| `Arrays.sort(int[], lo, hi)` | Dual-pivot Quicksort | ❌ No | Range sort |
| `Collections.sort(List)` | Timsort (via `Arrays.sort`) | ✅ Yes | Calls `toArray()` internally |

*Table 17.2: Java sort internals.*

**Timsort** details:
1. Scan for natural runs (ascending or descending segments). Reverse descending runs.
2. Runs shorter than MIN_RUN (~32) are extended using binary insertion sort.
3. Merge runs using an optimised merge (with galloping mode that skips elements when one run is consistently smaller).
4. Result: O(n) for already-sorted input, O(n log n) worst case.

---

## 17.9 Sorting Objects and Custom Comparators

```java
// Sort by multiple fields: primary by name, secondary by age descending
List<Person> people = ...;
people.sort(Comparator.comparing(Person::getName)
                      .thenComparing(Comparator.comparingInt(Person::getAge).reversed()));

// Primitive array with custom order — must box to Integer[]
Integer[] arr = {3, 1, 4, 1, 5};
Arrays.sort(arr, Comparator.reverseOrder());

// Sort strings by length, then lexicographically
String[] words = {"banana", "apple", "fig", "cherry"};
Arrays.sort(words, Comparator.comparingInt(String::length)
                              .thenComparing(Comparator.naturalOrder()));
```

> [!WARNING]
> The subtraction comparator `(a, b) -> a - b` overflows for large values. Always use `Integer.compare(a, b)` or `Comparator.comparingInt(...)`.

---

## 17.10 Algorithm Selection Guide

| Use case | Algorithm | Why |
|----------|-----------|-----|
| General purpose (objects) | Timsort (Java default) | Stable, O(n log n), adaptive |
| General purpose (primitives) | Dual-pivot quicksort (Java default) | Fast constant factor, in-place |
| Need stable + O(1) space? | Impossible for comparison sort | Trade-off is fundamental |
| Need O(1) space? | Heap sort | Guaranteed O(n log n), in-place |
| Nearly sorted input | Insertion sort or Timsort | O(n + inversions) |
| Integer keys, bounded range | Counting sort | O(n + k) |
| Multi-digit integers | Radix sort (LSD) | O(dn) |
| Uniformly distributed floats | Bucket sort | O(n) average |
| Need minimum swaps? | Selection sort | Exactly n-1 swaps |

*Table 17.3: Sorting algorithm selection guide.*

---

## Interview Insight

> ### 🎯 Interview Insight
> **Sorting in Interviews: What's Actually Tested**
>
> You are almost never asked to implement merge sort or quicksort from scratch in product interviews. What is tested:
>
> 1. **Knowing what Java uses:** "`Arrays.sort` on primitives uses dual-pivot quicksort — not stable. On objects, it uses Timsort — stable."
> 2. **Comparator fluency:** Building multi-field, multi-direction comparators with `Comparator.comparing(...).thenComparing(...)`.
> 3. **Stability awareness:** "If I need to sort by department and preserve name order, I need a stable sort."
> 4. **Non-comparison sorts:** "If values are in [0, 10⁶], I could use counting sort for O(n) instead of O(n log n)."
> 5. **Lower bound awareness:** "No comparison sort can beat O(n log n) in the worst case — this is a proven lower bound."

---

## Quick Quiz

**Q1.** Why is merge sort's space complexity O(n) while heap sort's is O(1)?

<details><summary><strong>Answer</strong></summary>

> Merge sort needs an auxiliary array of size $n$ to merge two sorted halves without overwriting either. Heap sort works entirely in-place: it builds the heap within the original array and extracts elements by swapping the root with the last heap element, reducing heap size by 1.

</details>

---

**Q2.** What is the worst-case input for quicksort with a fixed first-element pivot, and how does randomisation fix it?

<details><summary><strong>Answer</strong></summary>

> An already-sorted (or reverse-sorted) array. Pivot is always the minimum (or maximum), producing partitions of size 0 and n−1. Recurrence: T(n) = T(n−1) + O(n) → O(n²). Randomisation swaps a random element to the pivot position before partitioning — no deterministic adversarial input can consistently produce worst-case partitions.

</details>

---

**Q3.** Why can't counting sort be used to sort a list of `Person` objects by age?

<details><summary><strong>Answer</strong></summary>

> Counting sort only sorts by integer keys and cannot directly handle objects with multiple fields. While you can extract the integer age key, counting sort would lose the other fields of `Person`. You would need to use it as a subroutine in a larger algorithm (like radix sort does) or use a comparison sort with a custom comparator.

</details>

---

**Q4.** Which Java sort is used for `Arrays.sort(int[])` and is it stable?

<details><summary><strong>Answer</strong></summary>

> **Dual-pivot quicksort** (Yaroslavskiy's algorithm). It is **not stable** — but for primitive arrays this is irrelevant since primitives have no identity beyond their value.

</details>

---

## 17.11 Practice Problems

| Problem | Name | Difficulty | Algorithm |
|---------|------|------------|-----------|
| [LC 912](https://leetcode.com/problems/sort-an-array/) | Sort an Array | Medium | Merge sort or heap sort |
| [LC 148](https://leetcode.com/problems/sort-list/) | Sort List | Medium | Merge sort on linked list |
| [LC 215](https://leetcode.com/problems/kth-largest-element-in-an-array/) | Kth Largest Element | Medium | Quickselect |
| [LC 347](https://leetcode.com/problems/top-k-frequent-elements/) | Top K Frequent Elements | Medium | Bucket sort by frequency |
| [LC 164](https://leetcode.com/problems/maximum-gap/) | Maximum Gap | Hard | Radix sort or bucket sort |
| [LC 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | Count of Smaller Numbers After Self | Hard | Merge sort (count inversions) |

---

## 17.12 Hard Problem Corner

> ### 🔥 Hard Problem Corner
>
> **Problem: Count of Smaller Numbers After Self** ([LC 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/))
>
> For each `nums[i]`, count how many numbers to its right are smaller. Return these counts.
>
> **Connection to inversions:** Each count is the number of inversions involving `nums[i]` as the larger element. Merge sort naturally counts these during the merge step.

<details><summary><strong>Solution sketch (click to reveal)</strong></summary>

> [!TIP]
> Augment merge sort: when merging two halves, whenever an element from the right half is placed before an element from the left half, it means ALL remaining elements in the left half are greater than the right element → add the count of remaining left elements to the left element's count.
>
> ```java
> int[] counts;
> int[] indices;  // sort indices, not values
>
> void mergeCount(int[] nums, int lo, int hi) {
>     if (lo >= hi) return;
>     int mid = lo + (hi - lo) / 2;
>     mergeCount(nums, lo, mid);
>     mergeCount(nums, mid + 1, hi);
>     mergeAndCount(nums, lo, mid, hi);
> }
>
> void mergeAndCount(int[] nums, int lo, int mid, int hi) {
>     int[] tmp = Arrays.copyOfRange(indices, lo, hi + 1);
>     int left = 0, right = mid - lo + 1;
>     for (int k = lo; k <= hi; k++) {
>         if (left > mid - lo) {
>             indices[k] = tmp[right++];
>         } else if (right > hi - lo || nums[tmp[left]] <= nums[tmp[right]]) {
>             counts[tmp[left]] += right - (mid - lo + 1);  // count right elements placed so far
>             indices[k] = tmp[left++];
>         } else {
>             indices[k] = tmp[right++];
>         }
>     }
> }
> ```
>
> **Complexity:** O(n log n) — merge sort complexity with O(1) extra work per comparison.

</details>

---

## Chapter Summary

| Algorithm | Time (worst) | Space | Stable | Key property |
|-----------|-------------|-------|--------|-------------|
| **Merge sort** | O(n log n) | O(n) | ✅ | Optimal stable sort; basis of Timsort |
| **Quicksort** | O(n²) | O(log n) | ❌ | O(n log n) average; randomise pivot |
| **3-way quicksort** | O(n log n) avg | O(log n) | ❌ | Handles duplicates efficiently |
| **Heap sort** | O(n log n) | **O(1)** | ❌ | Only O(n log n) in-place sort |
| **Counting sort** | O(n + k) | O(n + k) | ✅ | Non-comparison; bounded integer keys |
| **Radix sort** | O(dn) | O(n) | ✅ | Non-comparison; multi-digit integers |
| **Bucket sort** | O(n) avg | O(n) | ✅ | Uniform distribution |
| **Lower bound** | Ω(n log n) | — | — | No comparison sort can do better |

*Table 17.4: Efficient sorting algorithm summary.*

**Java's choices:** Primitives → dual-pivot quicksort (fast, not stable). Objects → Timsort (stable, adaptive, O(n) for sorted input).

---

> **Part IV — Searching and Sorting**  
> [← Chapter 16](chapter-16-elementary-sorting.md) · [← Index](../INDEX.md) · [→ Part IV Review](part-04-review.md)
