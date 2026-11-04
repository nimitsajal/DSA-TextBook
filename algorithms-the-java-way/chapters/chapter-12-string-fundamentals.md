# Chapter 12 — String Fundamentals

> **Part III — Strings and String Algorithms**  
> [← Part II Review](part-02-review.md) · [← Index](../INDEX.md) · [→ Chapter 13](../INDEX.md#-chapter-13--advanced-string-algorithms)

---

## Learning Objectives

By the end of this chapter you will be able to:

- Explain Java's String immutability and its performance implications.
- Use `StringBuilder` for efficient string construction and mutation.
- Apply character arithmetic for case conversion, digit extraction, and alphabet indexing.
- Implement frequency counting using `int[26]`, `int[128]`, and `HashMap`.
- Check for palindromes and anagrams using the right technique for the constraints.
- Distinguish substrings from subsequences and iterate over both.

---

## Contents

- [12.1 Character Encoding and Unicode](#121-character-encoding-and-unicode)
- [12.2 Java String Internals](#122-java-string-internals)
- [12.3 String Immutability and the Builder Pattern](#123-string-immutability-and-the-builder-pattern)
- [12.4 Core String Methods](#124-core-string-methods)
- [12.5 Character Arithmetic](#125-character-arithmetic)
- [12.6 String Comparison and Equality](#126-string-comparison-and-equality)
- [12.7 Frequency Counting](#127-frequency-counting)
- [12.8 Palindromes](#128-palindromes)
- [12.9 Anagrams](#129-anagrams)
- [12.10 Substrings vs Subsequences](#1210-substrings-vs-subsequences)
- [12.11 Common Pitfalls](#1211-common-pitfalls)
- [Interview Insight](#interview-insight)
- [Quick Quiz](#quick-quiz)
- [12.12 Practice Problems](#1212-practice-problems)
- [12.13 Hard Problem Corner](#1213-hard-problem-corner)
- [Chapter Summary](#chapter-summary)

---

## 12.1 Character Encoding and Unicode

A **character** is an abstract symbol. An **encoding** maps characters to numbers so they can be stored in memory.

| Encoding | Range | Bytes per char | Java relevance |
|----------|-------|---------------|----------------|
| ASCII | 0–127 | 1 | 7-bit subset; lowercase letters, digits, punctuation |
| Latin-1 | 0–255 | 1 | First 256 Unicode code points |
| UTF-16 | 0–65535 (BMP) | 2 (or 4 for surrogate pairs) | Java's internal `char` type |
| UTF-8 | 0–1,114,111 | 1–4 | Dominant web encoding; not what Java uses internally |

*Table 12.1: Character encodings.*

**Java's `char` is a 16-bit unsigned integer** encoding a UTF-16 code unit. For the vast majority of programming problems (ASCII printable characters: `'!'`=33 to `'~'`=126, lowercase letters `'a'`–`'z'`, uppercase `'A'`–`'Z'`, digits `'0'`–`'9'`), all characters fit comfortably in a single `char`.

**Key ASCII values to memorise:**

| Character | ASCII value |
|-----------|------------|
| `'0'` | 48 |
| `'9'` | 57 |
| `'A'` | 65 |
| `'Z'` | 90 |
| `'a'` | 97 |
| `'z'` | 122 |
| `'a' - 'A'` | 32 (the case gap) |

*Table 12.2: Key ASCII values. You do not need to memorise exact numbers — just the relationships.*

---

## 12.2 Java String Internals

A `String` in Java is an immutable object wrapping a `char[]` (prior to Java 9) or a `byte[]` with a charset flag (Java 9+ compact strings). Key properties:

```java
String s = "hello";
int len = s.length();           // 5 — O(1) (cached)
char c = s.charAt(2);           // 'l' — O(1)
int idx = s.indexOf('l');       // 2 — O(n)
String sub = s.substring(1, 4); // "ell" — O(k) where k = length of substring
```

### The String Pool

String literals are **interned** in a pool:

```java
String a = "hello";
String b = "hello";
System.out.println(a == b);      // true  — same pool reference
String c = new String("hello");
System.out.println(a == c);      // false — c is a new heap object
System.out.println(a.equals(c)); // true  — same content
```

> [!WARNING]
> **Never compare strings with `==`.** Always use `.equals()` or `.equalsIgnoreCase()`. `==` compares references; `.equals()` compares content. This is the single most common Java string bug.

### `substring()` Complexity

`s.substring(i, j)` creates a **new** `String` object by copying `j-i` characters. It is O(j − i), not O(1). Be careful when calling `substring` inside loops.

---

## 12.3 String Immutability and the Builder Pattern

Every "modification" to a `String` creates a new object:

```java
String s = "";
for (int i = 0; i < n; i++) s += "x";  // O(n²)! Each += creates a new String of increasing length
```

This is O(1 + 2 + 3 + ... + n) = O(n²) — the classic string concatenation trap.

**Always use `StringBuilder` for repeated string building:**

```java
StringBuilder sb = new StringBuilder();
for (int i = 0; i < n; i++) sb.append('x');  // O(n) — amortised O(1) per append
String result = sb.toString();                 // O(n) — one final copy
```

### Essential StringBuilder Methods

| Method | Description | Complexity |
|--------|-------------|-----------|
| `sb.append(x)` | Append string, char, int, etc. | O(1) amortised |
| `sb.insert(i, x)` | Insert at index i | O(n) |
| `sb.deleteCharAt(i)` | Delete char at index i | O(n) |
| `sb.delete(i, j)` | Delete range [i, j) | O(n) |
| `sb.reverse()` | Reverse in-place | O(n) |
| `sb.charAt(i)` | Get char at index i | O(1) |
| `sb.setCharAt(i, c)` | Set char at index i | O(1) |
| `sb.length()` | Current length | O(1) |
| `sb.toString()` | Build final String | O(n) |

*Table 12.3: StringBuilder method reference.*

> [!TIP]
> `StringBuilder` is not thread-safe. `StringBuffer` is (all methods are `synchronized`). In single-threaded contexts (every algorithm problem), use `StringBuilder`.

---

## 12.4 Core String Methods

```java
String s = "Hello, World!";

// Case
s.toLowerCase()                 // "hello, world!"
s.toUpperCase()                 // "HELLO, WORLD!"

// Trimming and stripping
"  hi  ".trim()                 // "hi" — removes leading/trailing ASCII whitespace
"  hi  ".strip()                // "hi" — Unicode-aware (Java 11+)

// Searching
s.contains("World")             // true
s.startsWith("Hello")           // true
s.endsWith("!")                 // true
s.indexOf("o")                  // 4 (first occurrence)
s.lastIndexOf("o")              // 8 (last occurrence)

// Extraction
s.substring(7, 12)              // "World"
s.charAt(0)                     // 'H'
s.toCharArray()                 // ['H','e','l','l','o',',',...]

// Splitting and joining
"a,b,c".split(",")              // ["a", "b", "c"]
String.join("-", "a","b","c")   // "a-b-c"

// Comparison
"abc".compareTo("abd")          // negative (c < d)
"abc".compareToIgnoreCase("ABC") // 0

// Conversion
String.valueOf(42)              // "42"
Integer.parseInt("42")          // 42
```

---

## 12.5 Character Arithmetic

Characters in Java are numeric. Arithmetic on them is natural and efficient.

### Alphabet Index

```java
char c = 'g';
int index = c - 'a';          // 6  (0-indexed position in lowercase alphabet)
char restored = (char)(index + 'a');  // 'g'
```

### Digit Value

```java
char d = '7';
int digit = d - '0';          // 7
char c = (char)(digit + '0'); // '7'
```

### Case Conversion (without String methods)

```java
char upper = 'G';
char lower = (char)(upper + 32);     // 'g' — add 32 to convert upper to lower
char backUp = (char)(lower - 32);    // 'G' — subtract 32

// Safer version using the bit-flip trick (bit 5 controls case in ASCII)
char lower2 = (char)(upper | 32);    // set bit 5 → lowercase
char upper2 = (char)(lower & ~32);   // clear bit 5 → uppercase
```

### Is Letter / Is Digit

```java
Character.isLetter('A')      // true
Character.isDigit('3')       // true
Character.isLetterOrDigit(c) // true for letters and digits
Character.isWhitespace(' ')  // true
Character.isLowerCase('a')   // true
Character.isUpperCase('A')   // true
```

---

## 12.6 String Comparison and Equality

| Method | What it compares | Returns |
|--------|-----------------|---------|
| `a == b` | References | `true` only if same object |
| `a.equals(b)` | Content (case-sensitive) | `true` if same chars |
| `a.equalsIgnoreCase(b)` | Content (case-insensitive) | `true` if same chars ignoring case |
| `a.compareTo(b)` | Lexicographic order | negative/0/positive |
| `Arrays.equals(a.toCharArray(), b.toCharArray())` | Char arrays | `true` if identical |

*Table 12.4: String comparison methods.*

**Lexicographic comparison:** `"abc".compareTo("abd")` compares character by character. At index 2: `'c'` vs `'d'` → `'c' - 'd' = -1`. Returns a negative value → "abc" comes before "abd".

---

## 12.7 Frequency Counting

Counting character occurrences is the foundation of anagram, palindrome, and sliding window string problems.

### Method 1: `int[26]` for Lowercase Letters (Fastest)

```java
int[] freq = new int[26];
for (char c : s.toCharArray()) freq[c - 'a']++;

// Check if char c is present
if (freq['e' - 'a'] > 0) { ... }
```

**When to use:** Only lowercase (or only uppercase) English letters. O(1) lookup, minimal memory.

### Method 2: `int[128]` for All ASCII

```java
int[] freq = new int[128];
for (char c : s.toCharArray()) freq[c]++;

// Direct char-as-index access
if (freq['A'] > 0) { ... }
```

**When to use:** Mix of upper/lowercase, digits, punctuation — any ASCII input.

### Method 3: `HashMap<Character, Integer>` for Large Alphabets

```java
Map<Character, Integer> freq = new HashMap<>();
for (char c : s.toCharArray()) freq.merge(c, 1, Integer::sum);
```

**When to use:** Unicode characters, non-ASCII input, or when you need to remove entries cleanly.

---

## 12.8 Palindromes

A string is a palindrome if it reads the same forwards and backwards.

### Two-Pointer Check — O(n)

```java
boolean isPalindrome(String s) {
    int lo = 0, hi = s.length() - 1;
    while (lo < hi) {
        if (s.charAt(lo) != s.charAt(hi)) return false;
        lo++; hi--;
    }
    return true;
}
```

### Valid Palindrome (Skip Non-Alphanumeric) — O(n)

```java
boolean isPalindromeFiltered(String s) {
    int lo = 0, hi = s.length() - 1;
    while (lo < hi) {
        while (lo < hi && !Character.isLetterOrDigit(s.charAt(lo))) lo++;
        while (lo < hi && !Character.isLetterOrDigit(s.charAt(hi))) hi--;
        if (Character.toLowerCase(s.charAt(lo)) != Character.toLowerCase(s.charAt(hi)))
            return false;
        lo++; hi--;
    }
    return true;
}
```

### Longest Palindromic Substring — Expand Around Centre — O(n²)

Every palindrome expands from its centre. For odd-length palindromes the centre is a single character; for even-length, the centre is between two characters.

```java
String longestPalindrome(String s) {
    int start = 0, maxLen = 1;
    for (int centre = 0; centre < s.length(); centre++) {
        // Odd length
        int len1 = expandAroundCentre(s, centre, centre);
        // Even length
        int len2 = expandAroundCentre(s, centre, centre + 1);
        int len = Math.max(len1, len2);
        if (len > maxLen) {
            maxLen = len;
            start = centre - (len - 1) / 2;
        }
    }
    return s.substring(start, start + maxLen);
}

int expandAroundCentre(String s, int lo, int hi) {
    while (lo >= 0 && hi < s.length() && s.charAt(lo) == s.charAt(hi)) { lo--; hi++; }
    return hi - lo - 1;  // length of palindrome found
}
```

---

## 12.9 Anagrams

Two strings are anagrams if they contain the same characters with the same frequencies.

### Method 1: Sort Both — O(n log n)

```java
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    char[] a = s.toCharArray(), b = t.toCharArray();
    Arrays.sort(a);
    Arrays.sort(b);
    return Arrays.equals(a, b);
}
```

Simple but O(n log n). Acceptable when n is small.

### Method 2: Frequency Array — O(n)

```java
boolean isAnagram(String s, String t) {
    if (s.length() != t.length()) return false;
    int[] count = new int[26];
    for (char c : s.toCharArray()) count[c - 'a']++;
    for (char c : t.toCharArray()) count[c - 'a']--;
    for (int v : count) if (v != 0) return false;
    return true;
}
```

**Optimisation:** Instead of two separate loops, increment for `s` and decrement for `t`. Any non-zero entry at the end means the strings are not anagrams.

### Find All Anagram Indices in a String — Sliding Window + Frequency Array — O(n)

```java
List<Integer> findAnagrams(String s, String p) {
    List<Integer> result = new ArrayList<>();
    if (s.length() < p.length()) return result;

    int[] need = new int[26], have = new int[26];
    for (char c : p.toCharArray()) need[c - 'a']++;

    int match = 0, required = 0;
    for (int i = 0; i < 26; i++) if (need[i] > 0) required++;

    int k = p.length();
    for (int right = 0; right < s.length(); right++) {
        int rc = s.charAt(right) - 'a';
        have[rc]++;
        if (need[rc] > 0 && have[rc] == need[rc]) match++;

        if (right >= k) {
            int lc = s.charAt(right - k) - 'a';
            if (need[lc] > 0 && have[lc] == need[lc]) match--;
            have[lc]--;
        }
        if (match == required) result.add(right - k + 1);
    }
    return result;
}
```

---

## 12.10 Substrings vs Subsequences

| Property | Substring | Subsequence |
|----------|-----------|------------|
| Definition | Contiguous portion of the string | Characters in order, not necessarily contiguous |
| Count | O(n²) distinct substrings | Up to 2ⁿ distinct subsequences |
| Example (`"abc"`) | `""`, `"a"`, `"ab"`, `"abc"`, `"b"`, `"bc"`, `"c"` | `""`, `"a"`, `"b"`, `"c"`, `"ab"`, `"ac"`, `"bc"`, `"abc"` |
| Enumeration | Nested loops on start/end indices | Recursive or bitmask |

*Table 12.5: Substrings vs subsequences.*

```java
// All substrings of s
for (int i = 0; i < s.length(); i++) {
    for (int j = i + 1; j <= s.length(); j++) {
        String sub = s.substring(i, j);
        process(sub);
    }
}
// Total: O(n²) substrings, O(n³) with the O(n) substring copy

// Check if t is a subsequence of s — two pointers — O(n)
boolean isSubsequence(String t, String s) {
    int i = 0;
    for (int j = 0; j < s.length() && i < t.length(); j++) {
        if (s.charAt(j) == t.charAt(i)) i++;
    }
    return i == t.length();
}
```

---

## 12.11 Common Pitfalls

### Pitfall 1: `==` for String Comparison

```java
if (s == "hello") { ... }         // WRONG — compares references
if (s.equals("hello")) { ... }    // CORRECT
```

### Pitfall 2: `+` Concatenation in Loops

```java
// WRONG — O(n²)
String result = "";
for (String word : words) result += word + " ";

// CORRECT — O(n)
StringBuilder sb = new StringBuilder();
for (String word : words) sb.append(word).append(' ');
String result = sb.toString();
```

### Pitfall 3: `substring()` Inside a Loop

```java
// WRONG — O(n²) total
for (int i = 0; i < n; i++) process(s.substring(0, i));

// CORRECT — Use index arithmetic or StringBuilder
```

### Pitfall 4: `charAt()` vs `toCharArray()` for Repeated Access

```java
// Calling toCharArray() once and working with the array
// is marginally faster for heavy character-level access
char[] arr = s.toCharArray();
for (int i = 0; i < arr.length; i++) process(arr[i]);
```

### Pitfall 5: Java's `%` on Chars

```java
int idx = ('A' - 'a') % 26;   // -32 % 26 = -6 in Java — WRONG for negative dividends
int idx2 = (('A' - 'a') % 26 + 26) % 26;  // 20 — CORRECT
```

---

## Interview Insight

> ### 🎯 Interview Insight
> **The String Instinct Checklist**
>
> When you see a string problem, run this checklist mentally:
> 1. **Alphabet?** Lowercase only → `int[26]`. ASCII → `int[128]`. Unicode → `HashMap`.
> 2. **Is it about frequencies?** → Frequency array first.
> 3. **Palindrome?** → Two pointers (check) or expand-around-centre (find longest).
> 4. **Anagram / permutation in string?** → Sliding window + frequency array.
> 5. **Building a string?** → `StringBuilder`, never `+` in a loop.
> 6. **Subsequence?** → Two pointers (check if `t` is subsequence of `s`).
> 7. **Pattern match?** → Chapter 13.
>
> Most easy–medium string problems reduce to one of: frequency counting, two pointers, or sliding window. The technique is exactly the same as in Part II — the input is just characters instead of integers.

---

## Quick Quiz

**Q1.** What does `'f' - 'a'` evaluate to, and what does it represent?

<details><summary><strong>Answer</strong></summary>

> `5`. It is the 0-indexed position of `'f'` in the lowercase alphabet (a=0, b=1, ..., f=5). Used to index a `int[26]` frequency array.

</details>

---

**Q2.** Why is `s += "x"` inside a loop O(n²) while `sb.append("x")` is O(n)?

<details><summary><strong>Answer</strong></summary>

> `String` is immutable. `s += "x"` creates a new `String` of length `len+1` and copies all existing characters. After $n$ appends, total work = `1 + 2 + 3 + ... + n = n(n+1)/2 = O(n²)`. `StringBuilder` uses an internal resizable array — append is O(1) amortised (same analysis as `ArrayList.add`). Final `toString()` does one O(n) copy.

</details>

---

**Q3.** Check if `"anagram"` and `"nagaram"` are anagrams using a frequency array. What is the array state after processing both?

<details><summary><strong>Answer</strong></summary>

> Increment for `"anagram"`: a→3, n→1, g→1, r→1, m→1.  
> Decrement for `"nagaram"`: n→0, a→2, g→0, a→1, r→0, a→0, m→0.  
> All zeros → they are anagrams ✓.

</details>

---

**Q4.** What is the difference between `"abc"` being a **substring** of `"xabcy"` vs a **subsequence** of `"xaybcz"`?

<details><summary><strong>Answer</strong></summary>

> **Substring:** the characters must appear contiguously. `"abc"` is a substring of `"xabcy"` (at index 1–3). It is NOT a substring of `"xaybcz"` because `a`, `b`, `c` are separated by `y`.  
> **Subsequence:** characters must appear in order, but gaps are allowed. `"abc"` IS a subsequence of `"xaybcz"` (a at index 1, b at index 3, c at index 4).

</details>

---

## 12.12 Practice Problems

| Problem | Name | Difficulty | Technique |
|---------|------|------------|-----------|
| [LC 125](https://leetcode.com/problems/valid-palindrome/) | Valid Palindrome | Easy | Two pointers |
| [LC 242](https://leetcode.com/problems/valid-anagram/) | Valid Anagram | Easy | Frequency array |
| [LC 392](https://leetcode.com/problems/is-subsequence/) | Is Subsequence | Easy | Two pointers |
| [LC 387](https://leetcode.com/problems/first-unique-character-in-a-string/) | First Unique Character | Easy | Frequency array |
| [LC 3](https://leetcode.com/problems/longest-substring-without-repeating-characters/) | Longest Substring Without Repeating Chars | Medium | Sliding window + freq array |
| [LC 567](https://leetcode.com/problems/permutation-in-string/) | Permutation in String | Medium | Fixed window + freq array |
| [LC 438](https://leetcode.com/problems/find-all-anagrams-in-a-string/) | Find All Anagrams in a String | Medium | Sliding window + freq array |
| [LC 5](https://leetcode.com/problems/longest-palindromic-substring/) | Longest Palindromic Substring | Medium | Expand around centre |
| [LC 647](https://leetcode.com/problems/palindromic-substrings/) | Palindromic Substrings | Medium | Expand around centre |
| [LC 49](https://leetcode.com/problems/group-anagrams/) | Group Anagrams | Medium | HashMap + sorted key |

---

## 12.13 Hard Problem Corner

> ### 🔥 Hard Problem Corner
>
> **Problem: Minimum Window Substring** ([LC 76](https://leetcode.com/problems/minimum-window-substring/))
>
> Given strings `s` and `t`, return the minimum-length substring of `s` that contains all characters of `t`. If none exists, return `""`.
>
> This problem combines: sliding window (Chapter 9) + frequency maps. It was introduced in Chapter 9 Section 9.8. Revisit it here and also implement the case where `t` may contain duplicate characters (e.g., `t = "aabc"` requires at least two `'a'`s in the window).

<details><summary><strong>Solution note</strong></summary>

> The solution from Chapter 9 already handles duplicates correctly via the `formed` counter — it only counts a character as "satisfied" when `have[c] == need[c]`. If `need['a'] = 2`, then `formed` only increments for `'a'` when `have['a']` reaches exactly 2. See [Chapter 9, Section 9.8](chapter-09-sliding-window.md#98-multiple-constraints) for the full implementation.

</details>

---

## Chapter Summary

| Concept | Key Takeaway |
|---------|-------------|
| **String immutability** | Every modification creates a new object. `+` in a loop is O(n²). |
| **StringBuilder** | O(1) amortised append. Use for all string construction. `toString()` is O(n). |
| **Character arithmetic** | `c - 'a'` → 0-indexed alphabet position. `c - '0'` → digit value. |
| **Frequency counting** | `int[26]` for lowercase, `int[128]` for ASCII, `HashMap` for Unicode. |
| **Palindrome** | Two-pointer check O(n). Expand-around-centre for longest O(n²). |
| **Anagram** | Frequency array O(n). Sort-based O(n log n). Sliding window for anagram search O(n). |
| **Substring vs subsequence** | Substring: contiguous. Subsequence: ordered, non-contiguous. |
| **Never `==` for strings** | Always `.equals()`. `==` compares references, not content. |
| **`substring()` cost** | O(k) where k = substring length. Avoid inside tight loops. |

*Table 12.6: Chapter 12 key concepts.*

---

**Coming Up Next:** [Chapter 13 (Advanced String Algorithms)](../INDEX.md#-chapter-13--advanced-string-algorithms) covers pattern matching — finding a string `p` inside a string `s` efficiently — with KMP, Z-algorithm, and Rabin-Karp.

---

> **Part III — Strings and String Algorithms**  
> [← Part II Review](part-02-review.md) · [← Index](../INDEX.md) · [→ Chapter 13 (planned)](../INDEX.md#-chapter-13--advanced-string-algorithms)
