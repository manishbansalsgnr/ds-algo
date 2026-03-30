# Java Collections, Streams & Utility Classes — Interview & Problem Solving Cheatsheet

> Quick-reference for LeetCode / competitive programming / DSA interview rounds.
> Time complexities are **amortized** unless noted otherwise.

---

## Table of Contents

1. [Collection Interfaces at a Glance](#1-collection-interfaces-at-a-glance)
2. [List Implementations](#2-list-implementations)
3. [Set Implementations](#3-set-implementations)
4. [Map Implementations](#4-map-implementations)
5. [Queue / Deque Implementations](#5-queue--deque-implementations)
6. [Stack Operations](#6-stack-operations)
7. [PriorityQueue (Heap)](#7-priorityqueue-heap)
8. [Common Collection Operations Comparison](#8-common-collection-operations-comparison)
9. [java.util.Collections Utility Methods](#9-javautilcollections-utility-methods)
10. [java.util.Arrays Utility Methods](#10-javautilarrays-utility-methods)
11. [Java Streams API](#11-java-streams-api)
12. [String / StringBuilder Operations](#12-string--stringbuilder-operations)
13. [Bit Manipulation Helpers](#13-bit-manipulation-helpers)
14. [Math Utility Methods](#14-math-utility-methods)
15. [Common Patterns & Idioms](#15-common-patterns--idioms)
16. [When to Use What — Decision Guide](#16-when-to-use-what--decision-guide)

---

## 1. Collection Interfaces at a Glance

| Interface | Ordered | Duplicates | Null Values | Key Implementations |
|-----------|---------|------------|-------------|---------------------|
| `List` | Yes (index) | Yes | Yes | `ArrayList`, `LinkedList` |
| `Set` | No* | No | One null (`HashSet`) | `HashSet`, `LinkedHashSet`, `TreeSet` |
| `Queue` | FIFO | Yes | No (`PriorityQueue`) | `LinkedList`, `PriorityQueue`, `ArrayDeque` |
| `Deque` | Both ends | Yes | No (`ArrayDeque`) | `ArrayDeque`, `LinkedList` |
| `Map` | No* | Keys: No, Values: Yes | One null key (`HashMap`) | `HashMap`, `LinkedHashMap`, `TreeMap` |

> \* `LinkedHash*` preserves insertion order; `Tree*` maintains sorted order.

---

## 2. List Implementations

### ArrayList vs LinkedList

| Operation | `ArrayList` | `LinkedList` |
|-----------|:-----------:|:------------:|
| `get(index)` | **O(1)** | O(n) |
| `set(index, val)` | **O(1)** | O(n) |
| `add(val)` (end) | **O(1)** amortized | **O(1)** |
| `add(index, val)` | O(n) | O(n)* |
| `remove(index)` | O(n) | O(n)* |
| `contains(val)` | O(n) | O(n) |
| `indexOf(val)` | O(n) | O(n) |
| `size()` | O(1) | O(1) |
| `isEmpty()` | O(1) | O(1) |
| Iterator `remove()` | O(n) | **O(1)** |

> \* LinkedList: O(1) removal/insert once you have the node reference, but finding the node is O(n).

### Key List Operations

| Method | Description | Example |
|--------|-------------|---------|
| `list.add(e)` | Append to end | `list.add(5);` |
| `list.add(i, e)` | Insert at index | `list.add(0, 5);` |
| `list.get(i)` | Access by index | `list.get(2);` |
| `list.set(i, e)` | Replace at index | `list.set(0, 10);` |
| `list.remove(i)` | Remove by index (returns removed element) | `list.remove(0);` |
| `list.remove(obj)` | Remove first occurrence | `list.remove(Integer.valueOf(5));` |
| `list.contains(e)` | Check existence | `list.contains(5);` |
| `list.indexOf(e)` | First index of element, -1 if absent | `list.indexOf(5);` |
| `list.lastIndexOf(e)` | Last index of element | `list.lastIndexOf(5);` |
| `list.subList(from, to)` | View (inclusive `from`, exclusive `to`) | `list.subList(1, 4);` |
| `list.sort(cmp)` | In-place sort | `list.sort(Comparator.naturalOrder());` |
| `list.toArray()` | Convert to `Object[]` | `list.toArray(new int[0]);` |
| `list.addAll(coll)` | Append all from another collection | `list.addAll(other);` |
| `list.clear()` | Remove all elements | `list.clear();` |
| `list.replaceAll(op)` | Apply unary operator to each | `list.replaceAll(x -> x * 2);` |

### List Creation Shortcuts

```java
// Immutable lists
List<Integer> fixed = List.of(1, 2, 3);

// Mutable from array
List<Integer> mutable = new ArrayList<>(List.of(1, 2, 3));
List<Integer> fromArr = new ArrayList<>(Arrays.asList(arr));

// Initialize with size
List<Integer> sized = new ArrayList<>(1000);

// N copies of a value
List<Integer> zeros = new ArrayList<>(Collections.nCopies(10, 0));
```

---

## 3. Set Implementations

### HashSet vs LinkedHashSet vs TreeSet

| Operation | `HashSet` | `LinkedHashSet` | `TreeSet` |
|-----------|:---------:|:---------------:|:---------:|
| `add(e)` | **O(1)** | **O(1)** | O(log n) |
| `remove(e)` | **O(1)** | **O(1)** | O(log n) |
| `contains(e)` | **O(1)** | **O(1)** | O(log n) |
| `size()` | O(1) | O(1) | O(1) |
| Iteration order | Unpredictable | **Insertion order** | **Sorted order** |

### Key Set Operations

| Method | Description | Example |
|--------|-------------|---------|
| `set.add(e)` | Add (returns `false` if duplicate) | `set.add(5);` |
| `set.remove(e)` | Remove element | `set.remove(5);` |
| `set.contains(e)` | Check existence | `set.contains(5);` |
| `set.size()` | Number of elements | `set.size();` |
| `set.isEmpty()` | Check if empty | `set.isEmpty();` |
| `set.clear()` | Remove all | `set.clear();` |
| `set.addAll(coll)` | Union | `set.addAll(other);` |
| `set.retainAll(coll)` | Intersection (modifies `set`) | `set.retainAll(other);` |
| `set.removeAll(coll)` | Difference (modifies `set`) | `set.removeAll(other);` |

### TreeSet-Specific (NavigableSet) Methods

| Method | Description | Time |
|--------|-------------|------|
| `first()` / `last()` | Smallest / largest element | O(log n) |
| `lower(e)` | Greatest element strictly less than `e` | O(log n) |
| `floor(e)` | Greatest element ≤ `e` | O(log n) |
| `ceiling(e)` | Smallest element ≥ `e` | O(log n) |
| `higher(e)` | Smallest element strictly greater than `e` | O(log n) |
| `pollFirst()` / `pollLast()` | Remove and return min / max | O(log n) |
| `headSet(e)` | View of elements < `e` | O(log n) |
| `tailSet(e)` | View of elements ≥ `e` | O(log n) |
| `subSet(from, to)` | View of elements in `[from, to)` | O(log n) |

---

## 4. Map Implementations

### HashMap vs LinkedHashMap vs TreeMap

| Operation | `HashMap` | `LinkedHashMap` | `TreeMap` |
|-----------|:---------:|:---------------:|:---------:|
| `put(k, v)` | **O(1)** | **O(1)** | O(log n) |
| `get(k)` | **O(1)** | **O(1)** | O(log n) |
| `remove(k)` | **O(1)** | **O(1)** | O(log n) |
| `containsKey(k)` | **O(1)** | **O(1)** | O(log n) |
| `containsValue(v)` | O(n) | O(n) | O(n) |
| Iteration order | Unpredictable | **Insertion order** | **Sorted by key** |

### Key Map Operations

| Method | Description | Example |
|--------|-------------|---------|
| `map.put(k, v)` | Insert / update | `map.put("a", 1);` |
| `map.get(k)` | Get value (null if absent) | `map.get("a");` |
| `map.getOrDefault(k, def)` | Get with fallback | `map.getOrDefault("a", 0);` |
| `map.containsKey(k)` | Check key | `map.containsKey("a");` |
| `map.containsValue(v)` | Check value (O(n)) | `map.containsValue(1);` |
| `map.remove(k)` | Remove entry | `map.remove("a");` |
| `map.putIfAbsent(k, v)` | Insert only if key missing | `map.putIfAbsent("a", 1);` |
| `map.merge(k, v, fn)` | Merge with remapping function | `map.merge("a", 1, Integer::sum);` |
| `map.compute(k, fn)` | Compute new value from key + old val | `map.compute("a", (k,v) -> v+1);` |
| `map.computeIfAbsent(k, fn)` | Compute only if absent | `map.computeIfAbsent("a", k -> new ArrayList<>());` |
| `map.computeIfPresent(k, fn)` | Compute only if present | `map.computeIfPresent("a", (k,v) -> v+1);` |
| `map.replace(k, v)` | Replace if key exists | `map.replace("a", 2);` |
| `map.replaceAll(fn)` | Replace all values | `map.replaceAll((k,v) -> v*2);` |
| `map.keySet()` | Set of keys | `for (String k : map.keySet())` |
| `map.values()` | Collection of values | `for (int v : map.values())` |
| `map.entrySet()` | Set of entries | `for (var e : map.entrySet())` |
| `map.size()` | Number of entries | `map.size();` |
| `map.isEmpty()` | Check if empty | `map.isEmpty();` |
| `map.forEach(action)` | Iterate | `map.forEach((k,v) -> ...);` |

### Frequency Count Idiom

```java
// Count frequencies — most common LeetCode pattern
Map<Integer, Integer> freq = new HashMap<>();
for (int num : nums) {
    freq.merge(num, 1, Integer::sum);
}

// Or with getOrDefault
freq.put(num, freq.getOrDefault(num, 0) + 1);

// Adjacency list
Map<Integer, List<Integer>> graph = new HashMap<>();
graph.computeIfAbsent(u, k -> new ArrayList<>()).add(v);
```

### TreeMap-Specific (NavigableMap) Methods

| Method | Description | Time |
|--------|-------------|------|
| `firstKey()` / `lastKey()` | Smallest / largest key | O(log n) |
| `firstEntry()` / `lastEntry()` | Entry with min / max key | O(log n) |
| `lowerKey(k)` / `lowerEntry(k)` | Greatest key strictly < `k` | O(log n) |
| `floorKey(k)` / `floorEntry(k)` | Greatest key ≤ `k` | O(log n) |
| `ceilingKey(k)` / `ceilingEntry(k)` | Smallest key ≥ `k` | O(log n) |
| `higherKey(k)` / `higherEntry(k)` | Smallest key strictly > `k` | O(log n) |
| `pollFirstEntry()` / `pollLastEntry()` | Remove & return min/max entry | O(log n) |
| `headMap(k)` | View of keys < `k` | O(log n) |
| `tailMap(k)` | View of keys ≥ `k` | O(log n) |
| `subMap(from, to)` | View of keys in `[from, to)` | O(log n) |
| `descendingMap()` | Reverse-order view | O(1) |

---

## 5. Queue / Deque Implementations

### Queue (FIFO) — Use `LinkedList` or `ArrayDeque`

| Method | Throws Exception | Returns Special Value | Description |
|--------|:----------------:|:---------------------:|-------------|
| Insert | `add(e)` | `offer(e)` | Add to tail |
| Remove | `remove()` | `poll()` | Remove from head |
| Examine | `element()` | `peek()` | Look at head |

> Prefer `offer` / `poll` / `peek` — they return `null` or `false` instead of throwing.

### Deque (Double-Ended Queue) — Use `ArrayDeque`

| Operation | Head | Tail |
|-----------|------|------|
| Insert | `offerFirst(e)` / `addFirst(e)` | `offerLast(e)` / `addLast(e)` |
| Remove | `pollFirst()` / `removeFirst()` | `pollLast()` / `removeLast()` |
| Examine | `peekFirst()` / `getFirst()` | `peekLast()` / `getLast()` |

**All operations are O(1).**

### ArrayDeque vs LinkedList for Queue/Deque

| Aspect | `ArrayDeque` | `LinkedList` |
|--------|:------------:|:------------:|
| Speed | **Faster** (cache-friendly) | Slower (pointer chasing) |
| Memory | **Less** (no node overhead) | More (node objects) |
| Null elements | **Not allowed** | Allowed |
| As List | Not a `List` | Implements `List` |
| Recommendation | **Prefer this** | Use only if you need `List` API |

---

## 6. Stack Operations

> `java.util.Stack` is **legacy** — use `ArrayDeque` as a stack instead.

| Stack Operation | `ArrayDeque` Method | Time |
|-----------------|---------------------|:----:|
| Push | `push(e)` / `addFirst(e)` | O(1) |
| Pop | `pop()` / `removeFirst()` | O(1) |
| Peek | `peek()` / `peekFirst()` | O(1) |
| Is Empty | `isEmpty()` | O(1) |
| Size | `size()` | O(1) |

```java
Deque<Integer> stack = new ArrayDeque<>();
stack.push(1);
stack.push(2);
stack.peek();  // 2
stack.pop();   // 2
```

---

## 7. PriorityQueue (Heap)

| Operation | Time |
|-----------|:----:|
| `offer(e)` / `add(e)` | O(log n) |
| `poll()` | O(log n) |
| `peek()` | **O(1)** |
| `remove(e)` | O(n) |
| `contains(e)` | O(n) |
| `size()` | O(1) |
| Construction from collection | O(n) |

### Common Heap Patterns

```java
// Min-heap (default)
PriorityQueue<Integer> minHeap = new PriorityQueue<>();

// Max-heap
PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());
// or
PriorityQueue<Integer> maxHeap = new PriorityQueue<>((a, b) -> b - a);

// Heap of pairs — sort by value ascending, then key ascending
PriorityQueue<int[]> pq = new PriorityQueue<>((a, b) -> 
    a[1] != b[1] ? a[1] - b[1] : a[0] - b[0]
);

// Top-K frequent elements (keep k smallest → min-heap of size k)
PriorityQueue<Integer> topK = new PriorityQueue<>();

// Median finder (two heaps)
PriorityQueue<Integer> lo = new PriorityQueue<>(Collections.reverseOrder()); // max-heap
PriorityQueue<Integer> hi = new PriorityQueue<>(); // min-heap
```

---

## 8. Common Collection Operations Comparison

| Operation | `ArrayList` | `HashSet` | `TreeSet` | `HashMap` | `TreeMap` | `PriorityQueue` | `ArrayDeque` |
|-----------|:-----------:|:---------:|:---------:|:---------:|:---------:|:----------------:|:------------:|
| Add/Insert | O(1)* | O(1) | O(log n) | O(1) | O(log n) | O(log n) | O(1) |
| Remove | O(n) | O(1) | O(log n) | O(1) | O(log n) | O(n) | O(1)† |
| Search / Contains | O(n) | O(1) | O(log n) | O(1) | O(log n) | O(n) | O(n) |
| Access by index | O(1) | — | — | — | — | — | — |
| Min / Max | O(n) | O(n) | O(log n) | O(n) | O(log n) | O(1)‡ | — |
| Sorted iteration | O(n log n) | O(n log n) | O(n) | O(n log n) | O(n) | O(n log n) | — |

> \* Amortized append; insertion at index is O(n). † Remove from head/tail only. ‡ Peek returns min (or max if reversed).

---

## 9. java.util.Collections Utility Methods

| Method | Description | Time | Example |
|--------|-------------|:----:|---------|
| `sort(list)` | Natural order sort | O(n log n) | `Collections.sort(list);` |
| `sort(list, cmp)` | Custom comparator sort | O(n log n) | `Collections.sort(list, (a,b) -> b-a);` |
| `reverse(list)` | Reverse in-place | O(n) | `Collections.reverse(list);` |
| `reverseOrder()` | Returns reverse comparator | O(1) | `Collections.reverseOrder()` |
| `shuffle(list)` | Random shuffle in-place | O(n) | `Collections.shuffle(list);` |
| `swap(list, i, j)` | Swap two elements | O(1) | `Collections.swap(list, 0, 1);` |
| `rotate(list, dist)` | Rotate elements by distance | O(n) | `Collections.rotate(list, 2);` |
| `binarySearch(list, key)` | Search in sorted list | O(log n) | `Collections.binarySearch(list, 5);` |
| `min(coll)` | Minimum element | O(n) | `Collections.min(list);` |
| `max(coll)` | Maximum element | O(n) | `Collections.max(list);` |
| `min(coll, cmp)` | Min with comparator | O(n) | `Collections.min(list, cmp);` |
| `max(coll, cmp)` | Max with comparator | O(n) | `Collections.max(list, cmp);` |
| `frequency(coll, obj)` | Count occurrences | O(n) | `Collections.frequency(list, 5);` |
| `disjoint(c1, c2)` | True if no common elements | O(n) | `Collections.disjoint(a, b);` |
| `fill(list, val)` | Fill all positions with val | O(n) | `Collections.fill(list, 0);` |
| `nCopies(n, val)` | Immutable list of n copies | O(1) | `Collections.nCopies(10, 0);` |
| `singletonList(e)` | Immutable single-element list | O(1) | `Collections.singletonList(5);` |
| `emptyList()` | Immutable empty list | O(1) | `Collections.emptyList();` |
| `unmodifiableList(list)` | Immutable view | O(1) | `Collections.unmodifiableList(list);` |
| `synchronizedList(list)` | Thread-safe wrapper | O(1) | `Collections.synchronizedList(list);` |
| `addAll(coll, elems...)` | Add multiple elements | O(k) | `Collections.addAll(list, 1, 2, 3);` |

### Binary Search Details

```java
// Returns index if found, otherwise -(insertion point) - 1
List<Integer> sorted = List.of(1, 3, 5, 7, 9);
Collections.binarySearch(sorted, 5);  //  2
Collections.binarySearch(sorted, 4);  // -2 (would be inserted at index 2)

// To get insertion point from negative result:
int idx = Collections.binarySearch(sorted, 4);
if (idx < 0) idx = -(idx + 1); // insertion point = 2
```

---

## 10. java.util.Arrays Utility Methods

| Method | Description | Time | Example |
|--------|-------------|:----:|---------|
| `sort(arr)` | Sort entire array (DualPivotQuicksort for primitives, TimSort for objects) | O(n log n) | `Arrays.sort(nums);` |
| `sort(arr, from, to)` | Sort range `[from, to)` | O(k log k) | `Arrays.sort(nums, 0, 5);` |
| `sort(arr, cmp)` | Sort with comparator (object arrays only) | O(n log n) | `Arrays.sort(arr, (a,b) -> a[0]-b[0]);` |
| `binarySearch(arr, key)` | Search in sorted array | O(log n) | `Arrays.binarySearch(nums, 5);` |
| `binarySearch(arr, from, to, key)` | Search in sorted range | O(log n) | `Arrays.binarySearch(nums, 0, 5, 3);` |
| `fill(arr, val)` | Fill entire array | O(n) | `Arrays.fill(nums, 0);` |
| `fill(arr, from, to, val)` | Fill range `[from, to)` | O(k) | `Arrays.fill(nums, 0, 5, -1);` |
| `copyOf(arr, len)` | Copy with new length (truncates or pads) | O(n) | `Arrays.copyOf(nums, 10);` |
| `copyOfRange(arr, from, to)` | Copy range `[from, to)` | O(k) | `Arrays.copyOfRange(nums, 1, 4);` |
| `equals(a, b)` | Element-wise equality | O(n) | `Arrays.equals(a, b);` |
| `deepEquals(a, b)` | Deep equality for nested arrays | O(n) | `Arrays.deepEquals(a, b);` |
| `hashCode(arr)` | Hash based on contents | O(n) | `Arrays.hashCode(nums);` |
| `toString(arr)` | Pretty-print `[1, 2, 3]` | O(n) | `Arrays.toString(nums);` |
| `deepToString(arr)` | Pretty-print multi-dim arrays | O(n) | `Arrays.deepToString(matrix);` |
| `asList(arr)` | Fixed-size `List` view (Object arrays only) | O(1) | `Arrays.asList("a", "b", "c");` |
| `stream(arr)` | Create a stream | O(1) | `Arrays.stream(nums);` |
| `setAll(arr, gen)` | Set each element by index-based generator | O(n) | `Arrays.setAll(arr, i -> i * 2);` |
| `parallelSort(arr)` | Parallel merge sort | O(n log n) | `Arrays.parallelSort(nums);` |

### 2D Array Sort

```java
// Sort intervals by start time
int[][] intervals = {{1,3},{2,6},{8,10}};
Arrays.sort(intervals, (a, b) -> a[0] - b[0]);

// Sort by end time (greedy problems)
Arrays.sort(intervals, (a, b) -> a[1] - b[1]);

// Sort by start, then by end descending
Arrays.sort(intervals, (a, b) -> a[0] != b[0] ? a[0] - b[0] : b[1] - a[1]);
```

---

## 11. Java Streams API

### Stream Creation

| Source | Method | Example |
|--------|--------|---------|
| Collection | `.stream()` | `list.stream()` |
| Array | `Arrays.stream(arr)` | `Arrays.stream(nums)` |
| Values | `Stream.of(...)` | `Stream.of(1, 2, 3)` |
| Range | `IntStream.range(a, b)` | `IntStream.range(0, 10)` — `[0, 10)` |
| Range inclusive | `IntStream.rangeClosed(a, b)` | `IntStream.rangeClosed(1, 10)` — `[1, 10]` |
| Generate | `Stream.generate(supplier)` | `Stream.generate(Math::random).limit(5)` |
| Iterate | `Stream.iterate(seed, fn)` | `Stream.iterate(0, n -> n + 2).limit(5)` |
| String chars | `str.chars()` | `"abc".chars()` — returns `IntStream` |
| Map entries | `map.entrySet().stream()` | `map.entrySet().stream()` |

### Intermediate Operations (Lazy — return a Stream)

| Operation | Description | Example |
|-----------|-------------|---------|
| `filter(pred)` | Keep matching elements | `.filter(x -> x > 0)` |
| `map(fn)` | Transform elements | `.map(x -> x * 2)` |
| `mapToInt(fn)` | Map to `IntStream` | `.mapToInt(String::length)` |
| `mapToLong(fn)` | Map to `LongStream` | `.mapToLong(x -> (long)x)` |
| `mapToDouble(fn)` | Map to `DoubleStream` | `.mapToDouble(x -> x * 1.0)` |
| `flatMap(fn)` | One-to-many transform, flatten | `.flatMap(list -> list.stream())` |
| `distinct()` | Remove duplicates | `.distinct()` |
| `sorted()` | Natural order sort | `.sorted()` |
| `sorted(cmp)` | Custom comparator sort | `.sorted(Comparator.reverseOrder())` |
| `peek(action)` | Debug — perform action without consuming | `.peek(System.out::println)` |
| `limit(n)` | Take first n elements | `.limit(5)` |
| `skip(n)` | Skip first n elements | `.skip(2)` |
| `takeWhile(pred)` | Take while predicate is true (Java 9+) | `.takeWhile(x -> x < 5)` |
| `dropWhile(pred)` | Drop while predicate is true (Java 9+) | `.dropWhile(x -> x < 5)` |

### Terminal Operations (Eager — produce result)

| Operation | Description | Return Type | Example |
|-----------|-------------|-------------|---------|
| `forEach(action)` | Perform action on each | `void` | `.forEach(System.out::println)` |
| `toArray()` | Collect to array | `Object[]` or `int[]` | `.toArray()` / `.mapToInt(x->x).toArray()` |
| `collect(collector)` | Collect to collection | varies | `.collect(Collectors.toList())` |
| `reduce(identity, fn)` | Fold / accumulate | `T` | `.reduce(0, Integer::sum)` |
| `reduce(fn)` | Fold without identity | `Optional<T>` | `.reduce(Integer::max)` |
| `count()` | Number of elements | `long` | `.count()` |
| `min(cmp)` | Minimum | `Optional<T>` | `.min(Comparator.naturalOrder())` |
| `max(cmp)` | Maximum | `Optional<T>` | `.max(Comparator.naturalOrder())` |
| `sum()` | Sum (`IntStream` only) | `int` / `long` | `.mapToInt(x->x).sum()` |
| `average()` | Average (`IntStream`) | `OptionalDouble` | `.mapToInt(x->x).average()` |
| `anyMatch(pred)` | Any element matches? | `boolean` | `.anyMatch(x -> x > 0)` |
| `allMatch(pred)` | All elements match? | `boolean` | `.allMatch(x -> x > 0)` |
| `noneMatch(pred)` | No element matches? | `boolean` | `.noneMatch(x -> x < 0)` |
| `findFirst()` | First element | `Optional<T>` | `.findFirst()` |
| `findAny()` | Any element (nondeterministic) | `Optional<T>` | `.findAny()` |

### Common Collectors

| Collector | Description | Example |
|-----------|-------------|---------|
| `toList()` | Collect to `List` | `.collect(Collectors.toList())` |
| `toSet()` | Collect to `Set` | `.collect(Collectors.toSet())` |
| `toMap(keyFn, valFn)` | Collect to `Map` | `.collect(Collectors.toMap(x -> x, x -> x*x))` |
| `toMap(keyFn, valFn, merge)` | Map with merge for duplicates | `.collect(Collectors.toMap(k, v, Integer::sum))` |
| `toUnmodifiableList()` | Immutable list (Java 10+) | `.collect(Collectors.toUnmodifiableList())` |
| `joining()` | Concatenate strings | `.collect(Collectors.joining())` |
| `joining(delim)` | Join with delimiter | `.collect(Collectors.joining(", "))` |
| `groupingBy(fn)` | Group into `Map<K, List<V>>` | `.collect(Collectors.groupingBy(x -> x % 2))` |
| `groupingBy(fn, downstream)` | Group + aggregate | `.collect(Collectors.groupingBy(fn, Collectors.counting()))` |
| `partitioningBy(pred)` | Split into `Map<Boolean, List>` | `.collect(Collectors.partitioningBy(x -> x > 0))` |
| `counting()` | Count elements (downstream) | Used as downstream in `groupingBy` |
| `summingInt(fn)` | Sum (downstream) | `.collect(Collectors.summingInt(x -> x))` |
| `maxBy(cmp)` / `minBy(cmp)` | Max / min (downstream) | `.collect(Collectors.maxBy(Comparator.naturalOrder()))` |
| `mapping(fn, downstream)` | Map + collect downstream | `.collect(Collectors.mapping(fn, toList()))` |
| `reducing(identity, fn)` | General reduction | `.collect(Collectors.reducing(0, Integer::sum))` |

### Common Stream Patterns for Problem Solving

```java
// Array to List
List<Integer> list = Arrays.stream(nums).boxed().collect(Collectors.toList());

// List to int[]
int[] arr = list.stream().mapToInt(Integer::intValue).toArray();

// Sum of array
int sum = Arrays.stream(nums).sum();

// Max of array
int max = Arrays.stream(nums).max().getAsInt();

// Frequency map
Map<Integer, Long> freq = Arrays.stream(nums).boxed()
    .collect(Collectors.groupingBy(x -> x, Collectors.counting()));

// Filter and collect
List<Integer> positive = list.stream().filter(x -> x > 0).collect(Collectors.toList());

// Flatten 2D list
List<Integer> flat = nestedList.stream()
    .flatMap(Collection::stream)
    .collect(Collectors.toList());

// String to char frequency map
Map<Character, Long> charFreq = str.chars()
    .mapToObj(c -> (char) c)
    .collect(Collectors.groupingBy(c -> c, Collectors.counting()));

// Check if sorted
boolean isSorted = IntStream.range(0, arr.length - 1)
    .allMatch(i -> arr[i] <= arr[i + 1]);

// Generate prefix sum array
int[] prefix = new int[nums.length + 1];
Arrays.parallelPrefix(prefix = Arrays.copyOf(nums, nums.length), Integer::sum);
// Or manually:
for (int i = 1; i < prefix.length; i++) prefix[i] = prefix[i-1] + nums[i-1];

// Top K elements
List<Integer> topK = list.stream()
    .sorted(Comparator.reverseOrder())
    .limit(k)
    .collect(Collectors.toList());

// Group anagrams
Map<String, List<String>> groups = Arrays.stream(strs)
    .collect(Collectors.groupingBy(s -> {
        char[] c = s.toCharArray();
        Arrays.sort(c);
        return new String(c);
    }));
```

---

## 12. String / StringBuilder Operations

### String Methods (Immutable)

| Method | Description | Time | Example |
|--------|-------------|:----:|---------|
| `charAt(i)` | Character at index | O(1) | `s.charAt(0)` |
| `length()` | Length | O(1) | `s.length()` |
| `substring(from)` | Substring from index | O(n) | `s.substring(2)` |
| `substring(from, to)` | Substring `[from, to)` | O(n) | `s.substring(0, 3)` |
| `indexOf(str)` | First occurrence | O(nm) | `s.indexOf("ab")` |
| `lastIndexOf(str)` | Last occurrence | O(nm) | `s.lastIndexOf("ab")` |
| `contains(seq)` | Check containment | O(nm) | `s.contains("ab")` |
| `startsWith(prefix)` | Prefix check | O(k) | `s.startsWith("ab")` |
| `endsWith(suffix)` | Suffix check | O(k) | `s.endsWith("cd")` |
| `equals(str)` | Value equality | O(n) | `s.equals("abc")` |
| `equalsIgnoreCase(str)` | Case-insensitive equality | O(n) | `s.equalsIgnoreCase("ABC")` |
| `compareTo(str)` | Lexicographic comparison | O(n) | `s.compareTo("xyz")` |
| `toCharArray()` | Convert to `char[]` | O(n) | `s.toCharArray()` |
| `toUpperCase()` / `toLowerCase()` | Case conversion | O(n) | `s.toLowerCase()` |
| `trim()` / `strip()` | Remove whitespace | O(n) | `s.strip()` |
| `replace(old, new)` | Replace all occurrences | O(n) | `s.replace('a', 'b')` |
| `replaceAll(regex, repl)` | Regex replace | O(n) | `s.replaceAll("[^a-z]", "")` |
| `split(regex)` | Split by regex | O(n) | `s.split(",")` |
| `join(delim, elems)` | Join elements | O(n) | `String.join(",", list)` |
| `valueOf(x)` | Convert to string | O(k) | `String.valueOf(123)` |
| `isEmpty()` | True if length == 0 | O(1) | `s.isEmpty()` |
| `isBlank()` | True if only whitespace (Java 11+) | O(n) | `s.isBlank()` |
| `repeat(n)` | Repeat n times (Java 11+) | O(n*k) | `"ab".repeat(3)` → `"ababab"` |
| `chars()` | IntStream of characters | O(1) | `s.chars()` |

### StringBuilder (Mutable)

| Method | Description | Time | Example |
|--------|-------------|:----:|---------|
| `append(x)` | Append any type | O(1)* | `sb.append('a');` |
| `insert(i, x)` | Insert at index | O(n) | `sb.insert(0, "x");` |
| `delete(from, to)` | Delete range `[from, to)` | O(n) | `sb.delete(0, 2);` |
| `deleteCharAt(i)` | Delete single char | O(n) | `sb.deleteCharAt(0);` |
| `replace(from, to, str)` | Replace range | O(n) | `sb.replace(0, 2, "xy");` |
| `reverse()` | Reverse in-place | O(n) | `sb.reverse();` |
| `charAt(i)` | Char at index | O(1) | `sb.charAt(0);` |
| `setCharAt(i, c)` | Set char at index | O(1) | `sb.setCharAt(0, 'x');` |
| `length()` | Current length | O(1) | `sb.length();` |
| `toString()` | Convert to String | O(n) | `sb.toString();` |

> \* Amortized. StringBuilder avoids creating new String objects on each concatenation — always prefer it over `+` in loops.

### Common String Patterns

```java
// Reverse a string
String rev = new StringBuilder(s).reverse().toString();

// Check palindrome
boolean isPalin = s.equals(new StringBuilder(s).reverse().toString());

// Character frequency with array (faster than HashMap for a-z)
int[] freq = new int[26];
for (char c : s.toCharArray()) freq[c - 'a']++;

// Convert char to digit / digit to char
int digit = ch - '0';
char ch = (char) ('0' + digit);

// Check if character is letter/digit
Character.isLetterOrDigit(c);
Character.isLetter(c);
Character.isDigit(c);
```

---

## 13. Bit Manipulation Helpers

| Method / Operator | Description | Example |
|-------------------|-------------|---------|
| `Integer.bitCount(n)` | Number of 1-bits | `Integer.bitCount(7)` → `3` |
| `Integer.highestOneBit(n)` | Highest set bit value | `Integer.highestOneBit(10)` → `8` |
| `Integer.lowestOneBit(n)` | Lowest set bit value | `Integer.lowestOneBit(10)` → `2` |
| `Integer.numberOfLeadingZeros(n)` | Leading zeros in 32-bit | `Integer.numberOfLeadingZeros(1)` → `31` |
| `Integer.numberOfTrailingZeros(n)` | Trailing zeros | `Integer.numberOfTrailingZeros(8)` → `3` |
| `Integer.reverse(n)` | Reverse bit order | `Integer.reverse(n)` |
| `Integer.toBinaryString(n)` | Binary string representation | `Integer.toBinaryString(10)` → `"1010"` |
| `n & (n - 1)` | Clear lowest set bit | Used in Brian Kernighan's bit counting |
| `n & (-n)` | Isolate lowest set bit | Useful in BIT / Fenwick tree |
| `n \| (1 << i)` | Set bit at position i | |
| `n & ~(1 << i)` | Clear bit at position i | |
| `(n >> i) & 1` | Check bit at position i | |
| `n ^ (1 << i)` | Toggle bit at position i | |

---

## 14. Math Utility Methods

| Method | Description | Example |
|--------|-------------|---------|
| `Math.abs(x)` | Absolute value | `Math.abs(-5)` → `5` |
| `Math.max(a, b)` | Maximum of two | `Math.max(3, 7)` → `7` |
| `Math.min(a, b)` | Minimum of two | `Math.min(3, 7)` → `3` |
| `Math.pow(a, b)` | Power (returns `double`) | `Math.pow(2, 10)` → `1024.0` |
| `Math.sqrt(x)` | Square root | `Math.sqrt(16)` → `4.0` |
| `Math.ceil(x)` | Round up | `Math.ceil(2.1)` → `3.0` |
| `Math.floor(x)` | Round down | `Math.floor(2.9)` → `2.0` |
| `Math.round(x)` | Round to nearest | `Math.round(2.5)` → `3` |
| `Math.log(x)` | Natural log | `Math.log(Math.E)` → `1.0` |
| `Math.log10(x)` | Base-10 log | `Math.log10(100)` → `2.0` |
| `Math.addExact(a, b)` | Add with overflow check | Throws `ArithmeticException` |
| `Math.multiplyExact(a, b)` | Multiply with overflow check | Throws `ArithmeticException` |
| `Integer.MAX_VALUE` | 2³¹ - 1 = 2,147,483,647 | |
| `Integer.MIN_VALUE` | -2³¹ = -2,147,483,648 | |
| `Long.MAX_VALUE` | 2⁶³ - 1 | |
| `Long.MIN_VALUE` | -2⁶³ | |

### GCD / LCM

```java
// GCD (built-in is not available; use this)
int gcd(int a, int b) { return b == 0 ? a : gcd(b, a % b); }

// LCM
long lcm(int a, int b) { return (long) a / gcd(a, b) * b; }
```

---

## 15. Common Patterns & Idioms

### Comparator Patterns

```java
// Natural order
Comparator.naturalOrder()
Comparator.reverseOrder()

// Compare by field
Comparator.comparingInt(a -> a[0])
Comparator.comparingInt(String::length)

// Chain comparators
Comparator.comparingInt((int[] a) -> a[0]).thenComparingInt(a -> a[1])

// Reverse a comparator
comparator.reversed()

// Lambda comparator — careful with overflow!
(a, b) -> Integer.compare(a, b)  // safe
(a, b) -> a - b                   // can overflow for extreme values
```

### Conversion Cheat Sheet

| From | To | Code |
|------|----|------|
| `int[]` | `List<Integer>` | `Arrays.stream(arr).boxed().collect(Collectors.toList())` |
| `List<Integer>` | `int[]` | `list.stream().mapToInt(Integer::intValue).toArray()` |
| `int[]` | `Integer[]` | `Arrays.stream(arr).boxed().toArray(Integer[]::new)` |
| `Integer[]` | `int[]` | `Arrays.stream(arr).mapToInt(Integer::intValue).toArray()` |
| `char[]` | `String` | `new String(charArr)` or `String.valueOf(charArr)` |
| `String` | `char[]` | `str.toCharArray()` |
| `String[]` | `List<String>` | `Arrays.asList(arr)` or `List.of(arr)` |
| `List<String>` | `String[]` | `list.toArray(new String[0])` |
| `Set` | `List` | `new ArrayList<>(set)` |
| `List` | `Set` | `new HashSet<>(list)` |
| `Map keys` | `List` | `new ArrayList<>(map.keySet())` |
| `Map values` | `List` | `new ArrayList<>(map.values())` |
| `int` | `String` | `String.valueOf(n)` or `Integer.toString(n)` |
| `String` | `int` | `Integer.parseInt(s)` |
| `char` | `int` | `ch - '0'` (digit) or `(int) ch` (ASCII) |

### Initializing 2D Arrays

```java
// Fill 2D array
int[][] dp = new int[m][n];
for (int[] row : dp) Arrays.fill(row, -1);

// Boolean visited
boolean[][] visited = new boolean[m][n]; // defaults to false

// Directions array for grid traversal
int[][] dirs = {{0,1},{0,-1},{1,0},{-1,0}};          // 4-directional
int[][] dirs8 = {{0,1},{0,-1},{1,0},{-1,0},
                 {1,1},{1,-1},{-1,1},{-1,-1}};        // 8-directional
```

---

## 16. When to Use What — Decision Guide

| Problem Pattern | Best Data Structure | Why |
|----------------|---------------------|-----|
| Fast lookup by key | `HashMap` | O(1) get/put |
| Frequency counting | `HashMap` or `int[26]` for chars | O(1) increment |
| Unique elements | `HashSet` | O(1) add/contains |
| Maintain sorted order | `TreeMap` / `TreeSet` | O(log n) ops, sorted iteration |
| Find min/max dynamically | `PriorityQueue` | O(1) peek, O(log n) insert/remove |
| Sliding window with min/max | `ArrayDeque` (monotonic) | O(1) amortized per element |
| FIFO processing (BFS) | `ArrayDeque` as Queue | O(1) offer/poll |
| LIFO processing (DFS, parsing) | `ArrayDeque` as Stack | O(1) push/pop |
| Range queries / sorted neighbors | `TreeMap` (ceiling/floor) | O(log n) range lookups |
| Index-based access | `ArrayList` | O(1) random access |
| Ordered unique elements (LRU) | `LinkedHashSet` / `LinkedHashMap` | Insertion order + O(1) access |
| Interval merging | Sort + `List` or `PriorityQueue` | Sort by start, merge greedily |
| Graph adjacency | `Map<Node, List<Node>>` | Sparse graph; flexible |
| Union-Find | `int[] parent, rank` | Near O(1) with path compression |
| Prefix sums | `int[]` | O(1) range sum queries after O(n) build |
| Trie (prefix search) | `TrieNode[]` children or `Map` | O(L) per word, L = word length |
| Bit manipulation sets | `int` or `long` bitmask | O(1) set operations on small universes |

---

> **Tip:** In interviews, always state the time and space complexity of your solution. Knowing these tables helps you pick the right data structure instantly and justify your choice.

