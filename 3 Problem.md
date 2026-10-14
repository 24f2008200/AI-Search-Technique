These three problems are all instances of the same abstraction: **single-agent, deterministic, fully observable, discrete state-space search** with a goal test and a successor (MoveGen) function. They differ in how the state is encoded, how the graph is shaped, and what is being asked of the search.

## What is common

**1. Formal structure.** Each is a tuple ⟨S, s₀, MoveGen, GoalTest⟩.
- A state is a compact snapshot (a tuple, a string, or a board position).
- MoveGen maps a state to a finite list of neighbours.
- The state space is an implicit graph, built on demand and never stored in full.

**2. Algorithms.** The same uninformed toolkit applies to all three:
- BFS gives the shortest path (fewest moves) because every move costs 1.
- DFS, depth-limited search and iterative deepening are also usable.
- A visited/CLOSED list is needed to avoid cycles (essential in 1 and 3).
- Uniform edge cost means Dijkstra reduces to BFS, and a heuristic (A*) is optional rather than required.

**3. Complexity.** With branching factor b and solution depth d:
- BFS: O(bᵈ) time and O(bᵈ) space in the tree case.
- On a graph with a closed list, both are bounded by O(|V| + |E|), which is the real bound here because |V| is small.
- DFS: O(b·m) space (m = max depth), but not optimal.
- Iterative deepening: O(bᵈ) time, O(b·d) space, and optimal for unit costs.

## What is different

| Aspect | Problem 1: Water jugs | Problem 2: Rabbits | Problem 3: Knight |
|---|---|---|---|
| **State** | (Wa, Wb, Wc) with a conservation constraint (sum = 12) | A 5-character string over {R, L, -} | A single square, one of 21 |
| **State-space size** | Bounded by integer triples satisfying the constraint and capacities (a few dozen reachable) | Bounded by arrangements of 2 R, 2 L and 1 blank: 5!/(2!·2!·1!) = 30 | 21 nodes |
| **Branching factor** | Up to 6 (ordered pairs of jugs), often fewer | At most 4 (only the rabbits next to or one over from the blank can move) | Up to 8, in practice 2 to 4 because of the missing corners and the board edge |
| **Edge type** | Mixed: reversible (undirected) and irreversible (directed) | Mostly directed, since rabbits only move forward | Fully undirected, because a knight move is symmetric |
| **Cycles** | Yes, via reversible moves | Essentially none: every move goes forward, so the graph is a DAG | Yes, plentiful |
| **Question asked** | Shortest path between two interior states (not from the start) | Count of distinct paths to the goal, and whether the goal is reachable at all | Shortest path, with the path itself reported |
| **Goal** | A specific state | The mirror-image state | A specific square |

## Key abstract insights

- **Problem 2 is a DAG.** Forward-only moves mean no state repeats along a path, so no visited list is needed. Path counting is a DP over a topological order in O(|V| + |E|). Reachability asks whether the goal falls in the reachable set, and a dead end (a non-goal state with no moves) shows the problem can be unsolvable.
- **Problem 1 has asymmetric reversibility.** Some moves cannot be undone, so the graph is a mix of strongly connected clusters joined by one-way edges. Shortest-path queries need directed BFS, and reachability depends on direction. The conservation law reduces the apparent 3-dimensional space to 2 degrees of freedom.
- **Problem 3 is a plain undirected graph.** Shortest path is simple BFS, and symmetry means BFS could run from either end (bidirectional search cuts the work to roughly O(bᵈᐟ²)).
- **Shortest path vs. counting vs. reachability.** The same graph supports three different queries (min distance, number of paths, existence of a path), each with a different algorithm: BFS, DP or DFS with counting, and plain traversal.
- **All three are tiny.** Space is bounded by the number of distinct states, which is why the closed list makes the cost trivial. The exponential bᵈ only matters in the worst-case tree view, not on these graphs.

## One-line summary

All three are unit-cost state-space graph searches solvable by BFS in O(|V|+|E|) time and O(|V|) space; they differ in state encoding (tuple with an invariant, string with a blank, single square), edge directionality (mixed, directed/acyclic, undirected), and the query type (shortest path from an interior state, path counting, shortest path with the route).


----

# AI Search Techniques: Comparing the Three State-Space Problems

These three problems are excellent examples of a central idea in AI: a problem that looks like a puzzle can be represented as a graph, and solving it becomes a search through that graph.

I will not solve any of the three problems or reveal their requested answers. Instead, let's compare their underlying structures, how to model them, which search algorithms fit, and how to reason about time and space complexity.

## 1. The common abstraction: State-space search

All three problems can be described using the same framework.

State representation

What does the world look like right now?

MoveGen / successor function

Which legal states can I reach in one move?

Search algorithm

Which state should I explore next?

Goal test

Have I reached the desired state?

Every problem has these four ingredients:

- State space: All states that could be represented under the problem's rules.
- Initial state: Where the search begins.
- Transition function: The legal moves from one state to another.
- Goal condition: What qualifies as success.

The crucial insight is that the physical story is different in each problem, but the computational abstraction is remarkably similar.

## 2. Compare the three problems

| Property                   | Problem 1: Water jugs                             | Problem 2: Rabbits                                            | Problem 3: Knight                       |
| -------------------------- | ------------------------------------------------- | ------------------------------------------------------------- | --------------------------------------- |
| State representation       | Tuple of water quantities                         | String describing positions and rabbit directions             | Board coordinate                        |
| Example state              | `(Wa, Wb, Wc)`                                    | `RR-LL`                                                       | `a3`                                    |
| What defines a legal move? | Pouring or topping up according to jug capacities | Forward jumps into an empty position                          | Legal knight move to an existing square |
| State space                | Constrained quantities                            | Permutations of rabbits and empty spot                        | Existing board squares                  |
| Graph edges                | May be directed or reversible                     | Directed by movement rules                                    | Usually bidirectional                   |
| Typical objective          | Minimum moves between two specified states        | Reach the crossed configuration and count paths               | Shortest route between two squares      |
| Natural algorithm          | BFS for minimum moves                             | BFS/DFS or explicit graph analysis, depending on the question | BFS for shortest moves                  |

Notice three important differences:

1. How a state is encoded. Numbers, strings, and coordinates represent different kinds of configurations.
2. How transitions work. Water transfers obey capacity constraints; rabbits obey direction and jump restrictions; the knight obeys geometric movement rules.
3. What is being asked. A shortest path, a count of paths, and construction of a transition graph are related but distinct computational tasks.

## 3. The state-space size: How many states could exist?

This is where the problems begin to differ mathematically.

### Problem 1: Water jugs

A state is a triple:

$$ (W_a,W_b,W_c) $$

The constraints are:

$$ \begin{aligned} 0&\le W_a\le12\\\ 0&\le W_b\le8\\\ 0&\le W_c\le5\\\ W_a+W_b+W_c&=12 \end{aligned} $$

The total amount of water is conserved. Consequently, the three quantities are not independent.

If quantities are restricted to whole litres, there are 53 physically feasible triples before applying reachability restrictions. Only states reachable through legal transfers belong to the reachable state space.

This is a constrained configuration space, not a collection of arbitrary triples.

### Problem 2: Rabbits

A state is a five-character string containing two `R`s, two `L`s and one empty position.

For example, `RR-LL`.

If rabbits moving in the same direction are indistinguishable, the number of possible arrangements is

$$ \frac{5!}{2!\\,2!\\,1!}=30 $$

This is the upper bound on the number of configurations. Legal movement rules determine which states are reachable from the initial configuration.

Unlike the water-jug problem, the main constraint is not a numerical conservation equation. It is the arrangement of objects and the legal transitions between arrangements.

### Problem 3: Knight

A state is one valid square on the modified chessboard.

A standard $5\times5$ board has 25 squares, and four corners are removed. Therefore,

$$ |V|=25-4=21 $$

There are only 21 possible positions for the knight. The movement rules determine which pairs of positions are connected by an edge.

This is a particularly small state space: the vertices can be listed directly, and the edges can be generated from the knight's movement rules.

## 4. Which algorithms should you consider?

The choice depends on the question being asked, not merely on the type of puzzle.

Breadth-first search (BFS)

Explores states in increasing order of the number of moves from the start. For an unweighted graph, it finds a shortest path measured in edges.

Best fit for minimum-move questions in Problems 1 and 3.

Depth-first search (DFS)

Explores one branch deeply before backtracking. It is useful for exploring a finite state space, checking reachability and enumerating paths with appropriate safeguards.

Useful for constructing or studying Problem 2's state-space graph.

Explicit graph construction

Generate the states and transitions, then study the resulting graph directly. This is especially practical when the graph is small.

Useful for the manually constructed rabbit graph and the knight's MoveGen validation.

A few qualifications matter:

- Minimum moves: BFS is the natural choice when every move has equal cost. DFS does not generally guarantee the shortest path.
- Reachability: BFS or DFS can determine whether a goal is reachable.
- Counting paths: A traversal that merely marks visited states is not enough to count every path. The counting method must preserve the distinct routes being counted.
- Validating a graph: Compare each state's generated successors with the edges shown in the given graph. For a reversible move, verify the corresponding reverse transition as well.

## 5. Time and space complexity

There are two useful ways to analyze search complexity: in terms of the graph itself, and in terms of the search tree.

### A. Graph-based complexity

Let:

- $V$ = number of reachable states (vertices).
- $E$ = number of transitions (edges).
- $b$ = maximum branching factor, or number of successors per state.
- $d$ = depth of a shallowest goal.
- $m$ = maximum search depth.

If the graph is explicitly represented using an adjacency list:

| Algorithm / task        | Time complexity                                     | Space complexity  |
| ----------------------- | --------------------------------------------------- | ----------------- |
| BFS                     | $O(V+E)$                                        | $O(V)$        |
| DFS with visited states | $O(V+E)$                                        | $O(V)$        |
| Iterative deepening DFS | $O(b^d)$ time in the usual tree-search analysis | $O(bd)$       |
| Enumerate all paths     | Depends on the number and length of paths           | Can be very large |

BFS and DFS have the same asymptotic graph-traversal complexity, but they explore states in different orders and have different guarantees.

### B. Search-tree complexity

When analyzing a search without first constructing the entire graph, AI courses often use branching factor and depth:

| Search method       | Time         | Space        | Shortest path guaranteed? |
| ------------------- | ------------ | ------------ | ------------------------- |
| BFS                 | $O(b^d)$ | $O(b^d)$ | Yes, for equal-cost moves |
| DFS                 | $O(b^m)$ | $O(bm)$  | No                        |
| Iterative deepening | $O(b^d)$ | $O(bd)$  | Yes, for equal-cost moves |

These are standard worst-case tree-search bounds, not exact counts for your three puzzles. Repeated states and cycles can change how much work a graph-search implementation actually performs.

### C. Why space can matter more than time

Imagine that every state has, on average, four successors.

After $d$ levels, a search tree can contain roughly

$$ 1+b+b^2+\cdots+b^d =\frac{b^{d+1}-1}{b-1} $$

nodes, for $b\ne1$.

The number grows exponentially with depth. BFS stores many frontier states, whereas DFS can follow a single branch while retaining a much smaller stack.

This is why iterative deepening is useful when the solution depth is unknown and memory is limited.

## 6. Three subtle differences worth noticing

1\. Directed versus undirected transitions

The water-jug problem explicitly distinguishes reversible and irreversible moves. You must preserve the direction of each transition.

Knight moves are reversible: if a knight can move from square $u$ to square $v$, it can also move from $v$ to $u$, provided both squares exist.

Rabbit moves are governed by forward-only movement rules. Do not assume that a legal transition can simply be reversed.

2\. A state is not the same as a path

A state describes a configuration. A path describes a sequence of transitions.

Two different paths may reach the same state. BFS typically records the first discovery of a state to obtain a shortest path, but doing that alone does not count every possible path to the state.

3\. Counting paths requires care with cycles

In a graph with cycles, there may be infinitely many walks between two vertices if repeated vertices and edges are allowed. If the question asks for paths with no repeated vertices, the count is finite for a finite graph, but counting all such paths can be much more expensive than finding one shortest path.

This distinction is particularly important when interpreting the rabbit problem's request for the number of paths.

## 7. Final revision summary

| Question to ask                                            | Why it matters                                             |
| ---------------------------------------------------------- | ---------------------------------------------------------- |
| What exactly is a state?                                   | Determines how configurations are represented              |
| What constraints define valid states?                      | Determines the possible state space                        |
| What is the MoveGen function?                              | Determines the graph's transitions                         |
| Is the graph directed?                                     | Determines which transitions are legal                     |
| Is the goal reachability, shortest path, or path counting? | Determines the appropriate algorithm                       |
| Can states repeat during exploration?                      | Determines whether cycle detection is needed               |
| What are $V$, $E$, $b$, and $d$?           | Helps estimate time and memory                             |
| Must the entire graph be built?                            | Determines whether to use explicit or on-demand generation |

The central lesson for your AI Search Techniques course is this: first model the problem correctly, then choose the search algorithm. The story tells you how to represent states and generate moves. The question being asked tells you how to search the resulting graph.
