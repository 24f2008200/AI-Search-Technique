I'll unpack the Week 2 zip and read through the transcript.

I've read all seven Week 2 transcripts. I'm saving a note of this to your course file, then I'll give you the summary.

# Week 2 revision notes: Search Methods for Problem Solving (Prof. Khemani)

## 1. State space search: the setup
- **General search** means domain-independent algorithms. You write the domain functions, `MoveGen` and `GoalTest`, and plug them into a generic algorithm.
- **State** is a representation of a situation. **S** is the start state and **G** the goal.
- **`MoveGen(n)`** returns the set of neighbours (successor states) of n. It defines the state space as an **implicit graph**: you are given only S, and you generate the rest on the fly.
- **`GoalTest(n)`** returns true or false. It can describe a set of goals, not just one state. For the water jug problem, "some jug has 4 L" is a **disjunction**, so several states pass.
- **State space vs search space.** The state space is the graph of the problem. The search space (search tree) is what the algorithm actually generates. The same state space gives different search trees under different algorithms.
- **Generate and test** is the core idea. You generate a node, test it for the goal, and the key question is **which node to pick next**.
- **OPEN** holds nodes that are generated but not yet visited (the candidates). **CLOSED** holds nodes that have been visited and tested.
- **Constraint processing** is the other general approach. It uses variables, domains and constraints, and it subsumes state space search. It comes later in the course.

**Sample problems and their representations**
| Problem | Key point |
|---|---|
| Water jug (8, 5, 3 L; start 8 0 0) | State is a triple. Some moves are reversible and some are not. No water may be thrown away. (A slide has a typo: the state should be 0 5 3 or 3 2 3.) |
| 8-puzzle | The blank tile moves. The state graph splits into **two disjoint subgraphs**, so not every start state can reach the goal. Rubik's cube has 12. |
| Man–Goat–Lion–Cabbage | Three possible representations: both banks, left bank only, or "objects on the boat's side". The constraints are lion–goat and goat–cabbage. |
| N-Queens, map colouring, Sudoku | Configuration problems. |
| Travelling salesman (TSP) | **O(n!)**. It is an optimisation problem, not just satisficing. |
| Maze | The algorithm sees only the current node and its neighbours, with no bird's-eye view. |

## 2. Simple search algorithms: the evolution
1. **Simple Search 1** picks some node from OPEN, tests it, and adds all its neighbours. It has no CLOSED list, so it **loops forever** (S→A→S→B→S…) and OPEN keeps growing.
2. **Simple Search 2** adds CLOSED and does not add neighbours that are on CLOSED. It is smaller and avoids cycles, but duplicates can still sit on OPEN.
3. **Simple Search 3** adds only **new** nodes, meaning not on OPEN and not on CLOSED. Each node appears exactly once in the search tree.
4. **Problem:** returning just the goal node says "reachable" but not **how**. For planning problems you need the **path**.

**Path reconstruction**
- Store **node pairs** `(node, parent)`, with `(S, null)` as the start.
- When the goal is found, trace parents back through CLOSED: G→E→D→A→S.
- Alternative: store the **entire path** in each node and test the last element.

**Algorithm refinements:** OPEN and CLOSED become **lists**, not sets. "Pick some node" becomes "always pick the **head** of OPEN". Only where new nodes are inserted varies.

## 3. DFS vs BFS (Blind Search)

**Algorithm (same for both):**
```
OPEN ← [(S, null)];  CLOSED ← []
while OPEN not empty:
    nodePair ← head(OPEN); N ← first(nodePair)
    if GoalTest(N): return ReconstructPath(nodePair, CLOSED)
    CLOSED ← nodePair : CLOSED
    children ← RemoveSeen(MoveGen(N), OPEN, CLOSED)
    newPairs ← MakePairs(children, N)
    DFS: OPEN ← newPairs ++ tail(OPEN)     // front, a stack (LIFO)
    BFS: OPEN ← tail(OPEN) ++ newPairs     // back, a queue (FIFO)
```
- **DFS** is "impetuous": it expands the newest and deepest nodes first, dives down, then backtracks.
- **BFS** is "conservative": it goes level by level, staying as close to the start as possible.
- **The only difference** between them is where new nodes are inserted in OPEN.
- Both are **blind (uninformed)**: they behave the same regardless of where the goal is.

**Effect of pruning (the "tiny example", S, A, B, C, D, G):**
- Pruning nodes on OPEN and CLOSED: DFS and BFS generate the **same search tree** but explore it in a **different order**.
- Pruning only CLOSED: slightly bigger trees.
- **No pruning at all:** DFS goes into an **infinite loop** (S→A→C→B→S…). BFS still reaches the goal eventually (on the 11th node in the lecture example).

## 4. Analysis of DFS and BFS

**Assumptions:** constant branching factor **b**, goal at depth **d**. Time is measured as nodes inspected. Space is measured as the size of OPEN.

**Time (nodes inspected)**
- Full tree to depth d: **N = (b^(d+1) − 1) / (b − 1)**.
- DFS best case (goal at the leftmost edge): **d + 1** nodes. BFS best case: **1 + b + … + b^(d−1)**.
- Worst case for both: the whole tree, **(b^(d+1) − 1) / (b − 1)**.
- Average DFS: **≈ b^d / 2**.
- Average BFS: **≈ b^d · (b + 1) / (2(b − 1))**.
- **Ratio N_BFS / N_DFS ≈ (b + 1) / (b − 1)**. For b = 10 this is 11/9. The larger b is, the closer the two get.
- Both are **exponential**.

**Space**
- DFS adds **b − 1** nodes per level (it generates b, picks one), so OPEN grows **linearly with depth**: about (b − 1)·d, so **O(bd)**.
- BFS multiplies by b at each level, so OPEN grows **exponentially**: about **b^d**.

**Comparison table**
| Property | DFS | BFS |
|---|---|---|
| Time | Exponential | Exponential |
| Space | **Linear** ✅ | Exponential ❌ |
| Shortest path | ❌ (e.g. found S-A-C-D-G, not the shortest) | ✅ |
| Complete | ❌ in infinite spaces | ✅ if a **finite-length** path exists |

**Completeness details**
- Complete means: if a path to the goal exists, the algorithm finds it. "Systematic" means it explores the whole reachable space and can then report failure.
- **Finite space:** both explore everything and give the right yes/no answer.
- **Infinite space with a goal at finite depth:** BFS finds it. DFS can go down an infinite branch forever.
- **Infinite space with no path:** both run forever.
- **Professor's advice:** if the space is infinite, use BFS. If it is finite, trade off space against solution quality.
- **DFS advantage:** you can keep **one copy of the state** and do/undo moves (useful for N-Queens with huge N). It also mimics a physical agent.

## 5. Depth-Bounded DFS and DFID

**Depth-bounded DFS (DB-DFS):** DFS with a depth limit d. Nodes become triples `(node, parent, depth)`, and children are generated only if depth < bound.
- Linear space ✅, but **not complete** (the goal may lie beyond the bound) and **no shortest-path guarantee** ❌.

**DFID (Depth First Iterative Deepening)**
```
bound ← 0
repeat:
    (path, count) ← DB-DFS2(bound)    // also counts nodes
    bound ← bound + 1
until path is found OR count == previous count
```
- It runs DFS repeatedly with bound 0, 1, 2, …
- **Termination condition:** if the node **count** doesn't grow between iterations, there is nothing new to explore, so stop and report failure. Without it, DFID loops forever when no goal exists.
- It combines **linear space** (from DFS) with **shortest path / BFS-like order** (from BFS).

**Subtle point (Siddharth Sagar's question, IIT Dharwad 2019):** DFID does **not** always give the shortest path **if you prune using CLOSED**.
- Example: D is first reached via S-A-C-D (length 3). It goes on CLOSED, so when B is expanded, D is not generated as B's child. That kills the shorter route S-B-D-G, and DFID returns the longer S-A-C-E-D-G.
- **Fix:** do **not** use CLOSED for pruning in DFID. This also means the path reconstruction must pick the **right parent**. The simplest way is to **store the full path in each node**, such as (S,B,D,G).

**Cost of DFID (the extra work)**
- Leaves L, internal nodes I, with a full tree of branching factor b: **L = (b − 1)·I + 1**.
- DFID inspects **L + I** nodes where BFS would inspect L.
- **Ratio ≈ (L + I) / L ≈ b / (b − 1)**. For b = 10 that is 10/9, only about 10% extra time.
- The trade-off is a small amount of extra time for exponential savings in space.

**Other DFID points**
- Invented by **chess programmers**: it gives "anytime" move generation. After each iteration you have the best move so far and can stop when time runs out.
- For **configuration problems** (N-Queens, SAT, map colouring) the path doesn't matter, so DFID's main benefit is speed. It mostly matters for **planning** problems.

## 6. Planning vs configuration problems
| | Configuration | Planning |
|---|---|---|
| Goal | A **state** satisfying a description | Goal known or described, and the **path** is wanted |
| Examples | N-Queens, Sudoku, crosswords, map colouring, SAT | Water jugs, river crossing, route finding, Rubik's cube, cooking |
| Path matters? | No | **Yes** |

**List and tuple notation used in the algorithms**
- `[]` is the empty list. `x : L` adds x to the **head** of L. `head(L)` is an element and `tail(L)` is a list.
- `L1 ++ L2` **appends** two lists. The colon takes an element plus a list, while `++` takes two lists. Example: `[a,b,c] : [d,e]` gives `[[a,b,c], d, e]`.
- Tuples use parentheses, with `first`, `second`, `third`, or pattern matching with `_` for "don't care". `head(second(t))` is a valid composition.

## 7. Combinatorial explosion (CombEx)
- The search tree grows exponentially with branching factor b. The lecture compares it to the **Hydra**: cut off one head and several more grow back.
- DFS, BFS and DFID are all **blind or uninformed**. They don't know where the goal is. The next topic is **heuristic search**, which gives the search a sense of direction.

## Quick formula sheet
| Quantity | Formula |
|---|---|
| Nodes in full tree to depth d | (b^(d+1) − 1)/(b − 1) |
| DFS best case | d + 1 |
| DFS average | ≈ b^d / 2 |
| BFS average | ≈ b^d (b + 1) / (2(b − 1)) |
| BFS / DFS time ratio | (b + 1)/(b − 1) |
| DFS space | O(b·d), linear |
| BFS space | O(b^d), exponential |
| DFID / BFS node ratio | ≈ b/(b − 1) |
| Leaves in a full tree | L = (b − 1)·I + 1 |
| TSP search space | O(n!) |

## Likely exam traps
1. **DFS vs BFS differ only in where children are inserted** (front for a stack, back for a queue).
2. **DFS is not complete** in infinite spaces, but is complete in finite ones.
3. **DFID with CLOSED pruning can miss the shortest path**, so drop CLOSED pruning and keep full paths.
4. **BFS and DFS have the same time complexity order**, but DFS wins on space and BFS wins on solution quality.
5. **DFID's termination check** is a count comparison.
6. **Ancillary functions to know:** `RemoveSeen`, `OccursIn`, `MakePairs`, `ReconstructPath`.

