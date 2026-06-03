# BFS vs DFS — When to Use Which

---

## Use **BFS** when...

| Scenario | Why BFS |
|---|---|
| **Shortest path** in unweighted graph/grid | Explores level by level, guarantees minimum steps |
| **Minimum steps/time/cost** to reach target | Same reason — closest nodes first |
| **Multi-source spreading** (like rotting oranges) | All sources expand simultaneously |
| Target is **close to the source** | BFS finds it faster, doesn't overshoot |
| **Level-order traversal** of a tree | BFS is literally designed for this |

> 🧠 **BFS keyword triggers**: *minimum, shortest, nearest, fewest steps, level by level, simultaneous*

---

## Use **DFS** when...

| Scenario | Why DFS |
|---|---|
| **Detect a cycle** in a graph | DFS tracks back-edges naturally |
| **Topological sort** | DFS post-order gives topo order |
| **Connected components / flood fill** | Just need to explore fully, not optimally |
| **Backtracking** (permutations, subsets, N-Queens) | DFS explores one path, backtracks, tries next |
| **Pathfinding where ALL paths matter** | DFS explores every possibility |
| Target is **deep** in the tree/graph | DFS reaches deep nodes faster |
| **Tree problems** (height, diameter, LCA) | Recursive DFS maps naturally to tree structure |

> 🧠 **DFS keyword triggers**: *all paths, exists a path, count ways, detect cycle, topological order, combinations/permutations*

---

## The Core Intuition

```
BFS = "Explore outward in rings"       → care about DISTANCE
DFS = "Go as deep as possible first"   → care about EXISTENCE or STRUCTURE
```

Think of it like searching a city for someone:
- **BFS** = check your street first, then neighboring streets — you find the *nearest* person
- **DFS** = pick one road and follow it to the end before backtracking — you find *a* person, not necessarily the nearest

---

## Quick Decision Flowchart

```
Is shortest path / minimum steps needed?
        ├─ YES → BFS
        └─ NO
              ├─ Backtracking / all combinations? → DFS
              ├─ Tree structure problem?           → DFS
              ├─ Cycle detection / topo sort?      → DFS
              └─ Just need full exploration?       → Either (DFS simpler)
```

---

## Classic Problem Mapping

| Problem | Algorithm |
|---|---|
| Rotting Oranges | BFS (multi-source) |
| Word Ladder | BFS (shortest transformation) |
| Number of Islands | DFS or BFS (just flood fill) |
| Course Schedule (cycle) | DFS |
| Binary Tree Height | DFS |
| Binary Tree Level Order | BFS |
| Sudoku Solver | DFS + Backtracking |
| 01 Matrix | BFS (shortest distance) |
| All Paths Source to Target | DFS |
| Alien Dictionary | DFS (topo sort) |

---

## The Single Most Important Rule

> If the problem asks for **minimum/shortest** → almost always **BFS**.
> If it asks **if something exists** or **all ways** → almost always **DFS**.
