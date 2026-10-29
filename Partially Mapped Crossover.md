https://chatgpt.com/s/t_6ae34aa1c8208191a68b4185f5c99c7a
# Partially Mapped Crossover (PMX) in Genetic Algorithms

PMX is a crossover operator used in Genetic Algorithms for permutation problems, especially the Travelling Salesperson Problem (TSP).

Its purpose is to combine two parent tours to produce offspring while ensuring that every city appears exactly once.

Think of PMX as swapping a segment between two parents, then using a mapping to repair any duplicate cities.

## 1. The basic idea

Suppose we have two parent tours:

Parent 1

### A

### B

### C

### D

### E

### F

### G

### H

Parent 2

### H

### G

### E

### D

### C

### B

### A

### F

Illustration: the selected segment is at positions 3 to 5 (1-based indexing).

PMX follows three main steps:

1. Select a segment from both parents, using the same positions.
2. Exchange the segments to create the initial offspring.
3. Repair duplicates using a mapping, so each city appears once.

## 2. Worked example, step by step

Let's use these parents, with the crossover segment at positions 4 to 6.

Parent 1

### 1

### 2

### 3

### 4

### 5

### 6

### 7

### 8

### 9

Parent 2

### 4

### 5

### 2

### 1

### 8

### 7

### 6

### 9

### 3

Blue = Parent 1 segment; orange = Parent 2 segment.

### Step A: Create the mapping

Compare the genes at the selected positions.

| Parent 1 | Parent 2 |
| -------- | -------- |
| 4        | 1        |
| 5        | 8        |
| 6        | 7        |

Therefore, the mapping is:

\\[ 4\leftrightarrow1,\quad 5\leftrightarrow8,\quad 6\leftrightarrow7 \\]

These pairs are the key to repairing duplicates.

### Step B: Construct Child 1

Take the segment from Parent 2 and put it into the same positions in Parent 1.

Child 1, before repair

### 1

### 2

### 3

### 1

### 8

### 7

### 7

### 8

### 9

The segment 1, 8, 7 is copied from Parent 2. Duplicate genes now appear outside the segment.

Repair the outside genes using the mapping:

- The outside `1` conflicts with the segment's `1`. Mapping \\(1\leftrightarrow4\\), so replace it with `4`.
- The outside `8` conflicts with `8`. Mapping \\(8\leftrightarrow5\\), so replace it with `5`.
- The outside `7` conflicts with `7`. Mapping \\(7\leftrightarrow6\\), so replace it with `6`.
- The other outside `8` also conflicts with the segment. Replace it with `5` initially, but `5` is already used. Follow the mapping chain: \\(8\to5\\), and since `5` is now used, the correct repair must be checked against the full offspring.

The safe way to handle this last situation is to apply the mapping consistently, not simply substitute genes independently. For these parents, the outside positions in Child 1 are inherited from Parent 1, which has `7` and `8` in those positions. The mapping repairs produce the final offspring below.

Child 1, after repair

### 4

### 2

### 3

### 1

### 8

### 7

### 6

### 5

### 9

Every gene from `1` to `9` appears exactly once.

### Step C: Construct Child 2

Now reverse the roles:

- Copy the segment `4, 5, 6` from Parent 1.
- Keep the other positions from Parent 2.
- Repair duplicates using the same mapping.

The final Child 2 is:

### 1

### 8

### 2

### 4

### 5

### 6

### 7

### 9

### 3

Green indicates repaired outside positions; blue indicates the segment inherited from Parent 1.

## 3. The exam algorithm

Remember this sequence:

1\. Select the same positions in both parents

2\. Exchange the segments

3\. Build the mapping pairs

4\. Repair duplicates outside the segment using the mapping

5\. Verify: every city appears exactly once

Important exam tip: A PMX mapping is a chain of replacements. If a replacement creates another duplicate, follow the mapping until you find a valid city. Never leave a duplicate or lose a city.
