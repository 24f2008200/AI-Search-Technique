Yes. There is a useful pencil-and-paper shortcut: calculate savings by scanning the smallest edge costs first, rather than calculating all 15 pairs in alphabetical order.

## 1. Memorize one formula

With $A$ as the fulcrum,

$$ \boxed{S(i,j)=d(A,i)+d(A,j)-d(i,j)} $$

Interpretation: remove the two edges connecting $i$ and $j$ to $A$, then join $i$ and $j$ directly.

## 2. Use the sorted edge list to your advantage

Look at the smallest edge costs in your table. For each edge, calculate the sum of the two distances from $A$, then subtract the edge cost.

| Edge | Sum of distances from A | Edge cost | Savings            |
| ---- | ----------------------- | --------- | ------------------ |
| DE   | 53 + 32 = 85            | 16        | 69                 |
| CG   | 82 + 47 = 129           | 17        | 112                |
| DF   | 53 + 27 = 80            | 17        | 63                 |
| AD   | —                       | —         | Ignore: contains A |

The edge $CG$ gives savings of $112$, the largest among these candidates.

In fact, you can confirm it is the maximum without checking all 15 pairs: the largest possible sum of two distances from $A$ is $82+57=139$. Any edge costing more than $139-112=27$ cannot produce savings greater than $112$. Among the edges costing at most 27, $CG$ gives the maximum.

## 3. Final answer

- First edges removed: $AC$ and $AG$
- First edge added: $\boxed{CG}$
- Savings: $\boxed{82+47-17=112}$

### Exam shortcut

1. Sort or scan the non-$A$ edges from lowest cost upward.
2. For each promising edge, add the two corresponding distances from $A$.
3. Subtract the edge cost.
4. Stop when the remaining edge costs cannot beat your best savings.

This reduces unnecessary arithmetic. Just remember that the smallest edge cost alone does not guarantee the greatest savings; the distances from the fulcrum matter too.

-----

Yes! There is a structural shortcut, and it is much more useful in an exam than drawing every branch. The trick is to use parity, the boat's position, and the rule that states must alternate banks after every move.

Let's derive it for your river-crossing problem.

## 1. First, exploit the boat's position

Initially, the state is:

`BMLGC/NONE`

The boat and everyone else are on the left bank. After every move, the boat crosses to the other bank.

Therefore:

- After 1 move, the boat is on the right.
- After 2 moves, the boat is on the left.
- After 3 moves, the boat is on the right.

So, after exactly 3 moves, every reachable state must have `B` on the right bank.

This immediately eliminates options where `B` is on the left.

| Option      | Boat's bank | Possible after 3 moves? |
| ----------- | ----------- | ----------------------- |
| `C/BMLG`    | Right       | Yes, potentially        |
| `L/BMGC`    | Right       | Yes, potentially        |
| `BMLG/C`    | Left        | No                      |
| `BMGC/L`    | Left        | No                      |
| `NONE/BML`  | Right       | Yes, potentially        |
| `NONE/BMLC` | Right       | Yes, potentially        |

We've reduced six options to four without exploring any branches.

## 2. Use a three-move pattern instead of drawing the whole tree

For three crossings, the boat follows this pattern:

$$ \text{Left}\rightarrow\text{Right} \rightarrow\text{Left}\rightarrow\text{Right} $$

A useful pattern to test is:

1. Move an item to the right.
2. Return the man alone to the left.
3. Move another item to the right.

This pattern is especially useful because the second move does not bring any item back, so you can reason about which items remain on each bank.

There is also a special case: the man can cross alone first, potentially causing the lion and goat situation to eliminate animals.

## 3. Test the four remaining options

Let's construct a three-move path for each.

### Option 1: `C/BMLG`

Move the goat across, return alone, then take the lion across.

Start → `LC/BMG` → `BMLC/G` → `C/BMLG`

Reachable in 3 moves

### Option 2: `L/BMGC`

Move the goat across, return alone, then take the cabbage across.

Start → `LC/BMG` → `BMLC/G` → `L/BMGC`

Reachable in 3 moves

### Option 3: `NONE/BML`

First cross alone, allowing the unattended lion, goat and cabbage to trigger the specified eating sequence. Return alone, then take the surviving lion across.

Start → `L/BM` → `BML/NONE` → `NONE/BML`

Reachable in 3 moves

### Option 4: `NONE/BMLC`

Take the cabbage across first, return alone, then take the surviving lion across.

Start → `LG/BMC` → `BML/G` with the cabbage on the right → `NONE/BMLC`

Reachable in 3 moves

In the last two examples, the eating rule changes which items survive, so track the surviving items carefully.

## 4. The structural exam method

Remember these three filters:

1. Parity filter: after an odd number of moves, the boat is on the opposite bank from where it started.
2. Move-pattern filter: for three moves, try out, back alone, out.
3. Safety/eating filter: after each crossing, apply the problem's rules to any unattended animals.

This reduces the amount of branching you need to examine. For this question, all four options with the boat on the right can be reached in exactly three moves under the stated eating rules.
-------

In TSP (Travelling Salesperson Problem), adjacency representation of a tour means representing the tour by recording the city that comes immediately after each city.

Let's use an example.

## 1. Tour representation

Suppose we have 5 cities: $1,2,3,4,5$.

A tour is:

$$ 1\rightarrow3\rightarrow5\rightarrow2\rightarrow4\rightarrow1 $$

### Path representation

Record the cities in visiting order:

`[1, 3, 5, 2, 4]`

### Adjacency representation

Record the next city for each city:

| City | Next city |
| ---- | --------- |
| 1    | 3         |
| 2    | 4         |
| 3    | 5         |
| 4    | 1         |
| 5    | 2         |

Therefore, the adjacency representation is:

`[3, 4, 5, 1, 2]`

The first entry means city 1 goes to city 3; the second means city 2 goes to city 4, and so on.

## 2. Compare the two representations

Path representation

## `[1, 3, 5, 2, 4]`

Read the cities from left to right in visiting order.

Adjacency representation

## `[3, 4, 5, 1, 2]`

The index is the current city; the value is its successor.

## 3. Why is this useful in TSP?

Adjacency representation is useful for genetic algorithms and tour exchange operators, because it lets you reason about which cities are connected by edges.

For example, if city 1's successor changes from city 3 to city 4, you can immediately see that the edge leaving city 1 has changed.

Exam memory trick:

- Path: Who comes next in the visiting list?
- Adjacency: Who is the next city for each city number?

One caution: adjacency representation is not the same as an adjacency matrix. An adjacency matrix records connections between every pair of cities, usually using a 0 or 1 (or edge costs).
