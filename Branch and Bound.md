Let us work through a complete TSP example, step by step, using Branch and Bound in Refined State.

We will use a small 4-city problem so that we can calculate every lower bound by hand and understand exactly why certain branches are pruned.

## Problem: Travelling Salesman Problem

A salesman starts at city A, visits every other city exactly once, and returns to A.

The travel costs are:

| From / To | A | B  | C  | D  |
| --------- | - | -- | -- | -- |
| A         | ∞ | 10 | 15 | 20 |
| B         | 5 | ∞  | 9  | 10 |
| C         | 6 | 13 | ∞  | 12 |
| D         | 8 | 8  | 9  | ∞  |

We need to find the minimum-cost tour.

## Step 1: Construct the root refined state

First, reduce each row by its minimum finite value.

| From / To | A | B | C | D  | Row minimum |
| --------- | - | - | - | -- | ----------- |
| A         | ∞ | 0 | 5 | 10 | 10          |
| B         | 0 | ∞ | 4 | 5  | 5           |
| C         | 0 | 7 | ∞ | 6  | 6           |
| D         | 0 | 0 | 1 | ∞  | 8           |

The row reduction cost is:

$$ 10+5+6+8=29 $$

Now reduce the columns.

| Column | Minimum |
| ------ | ------- |
| A      | 0       |
| B      | 0       |
| C      | 1       |
| D      | 5       |

Column reduction cost:

$$ 0+0+1+5=6 $$

Therefore, the root lower bound is:

$$ LB(S_0)=29+6=\boxed{35} $$

Notice that the correct column minima are $0,0,1,5$, so the total is 6.

Our root refined state is:

$$ S_0=(\text{no edges selected},\\; LB=35) $$

This means every possible complete tour costs at least 35.

## Step 2: Branch from city A

From A, we have three possible decisions:

$$ A\rightarrow B,\quad A\rightarrow C,\quad A\rightarrow D $$

For each branch, we calculate:

$$ LB(\text{child})=LB(\text{parent})+ \text{selected reduced cost}+ \text{additional reduction} $$

The selected reduced cost comes from the reduced matrix, not directly from the original cost matrix.

### Branch 1: Include A → B

The reduced cost of A → B is 0.

We remove:

- Row A
- Column B
- The reverse edge B → A, to prevent an immediate cycle

The remaining matrix is:

| From / To | A | C | D |
| --------- | - | - | - |
| B         | ∞ | 3 | 0 |
| C         | 0 | ∞ | 1 |
| D         | 0 | 0 | ∞ |

There is no further reduction needed.

$$ LB(S_1)=35+0=\boxed{35} $$

### Branch 2: Include A → C

The reduced cost of A → C is 4.

After removing row A and column C, and preventing C → A:

| From / To | A | B | D |
| --------- | - | - | - |
| B         | 0 | ∞ | 0 |
| C         | ∞ | 7 | 1 |
| D         | 0 | 0 | ∞ |

No further reduction is needed.

$$ LB(S_2)=35+4=\boxed{39} $$

### Branch 3: Include A → D

The reduced cost of A → D is 5.

After removing row A and column D, and preventing D → A:

| From / To | A | B | C |
| --------- | - | - | - |
| B         | 0 | ∞ | 3 |
| C         | 0 | 7 | ∞ |
| D         | ∞ | 0 | 0 |

No further reduction is needed.

$$ LB(S_3)=35+5=\boxed{40} $$

### Our search frontier

| State     | Partial tour | Lower bound |
| --------- | ------------ | ----------- |
| $S_1$ | A → B        | 35          |
| $S_2$ | A → C        | 39          |
| $S_3$ | A → D        | 40          |

We choose $S_1$, because it has the smallest lower bound.

This is the Best-First Branch and Bound strategy.

## Step 3: Branch from A → B

We are currently at:

$$ S_1=(A\rightarrow B,\\; LB=35) $$

From B, we can go to C or D.

### Branch 1A: Include B → C

The reduced cost of B → C is 3.

After removing row B and column C, and preventing C → A:

| From / To | A | D |
| --------- | - | - |
| C         | ∞ | 1 |
| D         | 0 | ∞ |

Row C has minimum 1, so we need an additional reduction of 1.

$$ LB(S\_{1A})=35+3+1=\boxed{39} $$

### Branch 1B: Include B → D

The reduced cost of B → D is 0.

After removing row B and column D, and preventing D → A:

| From / To | A | C |
| --------- | - | - |
| C         | 0 | ∞ |
| D         | ∞ | 0 |

No additional reduction is needed.

$$ LB(S\_{1B})=35+0=\boxed{35} $$

Our frontier is now:

| State         | Partial tour | Lower bound |
| ------------- | ------------ | ----------- |
| $S\_{1A}$ | A → B → C    | 39          |
| $S\_{1B}$ | A → B → D    | 35          |
| $S_2$     | A → C        | 39          |
| $S_3$     | A → D        | 40          |

We continue with $S\_{1B}$.

## Step 4: Complete the tour

We have selected:

$$ A\rightarrow B\rightarrow D $$

Only city C remains to be visited.

The remaining choices are forced:

$$ D\rightarrow C\rightarrow A $$

Our complete tour is:

COMPLETE TOUR

| A → B      | 10 |
| ---------- | -- |
| B → D      | 10 |
| D → C      | 9  |
| C → A      | 6  |
| Total cost | 35 |

We now have a complete tour costing 35.

This becomes our upper bound:

$$ UB=\boxed{35} $$

## Step 5: Prune the remaining states

Recall our remaining states:

| State     | Lower bound | Decision |
| --------- | ----------- | -------- |
| A → B → C | 39          | Prune    |
| A → C     | 39          | Prune    |
| A → D     | 40          | Prune    |

Why?

Because each of these states has a lower bound greater than or equal to our best complete tour cost:

$$ LB(S)\geq UB $$

Therefore, none of them can produce a better tour.

All remaining branches have been pruned, so we have proved that:

$$ \boxed{\text{Optimal TSP cost}=35} $$

## The complete search tree
<img width="1128" height="577" alt="image" src="https://github.com/user-attachments/assets/ce3db1e0-5c89-4e98-b0b1-4ba163a1a9b3" />



## What makes this "Branch and Bound in Refined State"?

The important insight is that we did not simply explore all possible tours.

At every stage:

1. We refined the state by updating its cost matrix.
2. We calculated a lower bound for that state.
3. We selected the most promising state to explore.
4. Once we found a complete tour, we used its cost as the upper bound.
5. We pruned states that could not improve that upper bound.

The lower bound is the optimistic estimate; the upper bound is the best actual solution found.

One exam point to remember: A lower bound does not tell us the actual cost of the best tour in that branch. It tells us the minimum cost that branch could possibly achieve. That is why it is safe to prune a branch when its lower bound is at least the best complete tour cost.
