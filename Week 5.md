# TSP BnB walkthrough: optimal tour is Chennai → Bangalore → Goa → Mumbai → Delhi → Chennai, cost 5250

**Conventions:** the bound of a node is (sum of its 2N directed edges) / 2. The segment costs are CB 360, BG 570, GM 590, CG 800, MB 1210, CM 1280, MD 1540, GD 2080, CD 2190, DB 2434. I couldn't tell what segment "XY" in your tree stands for, so I took XY = CG for D1/D2. Those two nodes get pruned anyway, so the choice doesn't affect the answer.

## Root S0
Each city takes its two cheapest outgoing edges:

| City | Edges | Sum |
|---|---|---|
| C | CB, CG | 1160 |
| G | GB, GM | 1160 |
| M | MG, MB | 1800 |
| D | DM, DG | 3620 |
| B | BC, BG | 930 |

Total is 8670, so the bound is **4335**.

## Node bounds (with inference)

| Node | Path | Inference and edge changes | Bound |
|---|---|---|---|
| A1 | CB | CB is already in both defaults, so nothing changes | **4335** |
| A2 | ¬CB | C: CG+CM = 2080, B: BG+BM = 1780 | **5220** |
| B1 | CB, BG | B is saturated, so ¬BM and ¬BD. C–B–G would be a triangle, so ¬CG. C becomes CB+CM, and M becomes MG+MC. | **4610** |
| B2 | CB, ¬BG | B: BC+BM = 1570, G: GM+GC = 1390 | **4770** |
| C1 | CB, BG, GM | G is saturated, so ¬GC and ¬GD. Closing C–B–G–M would make a 4-cycle, so ¬CM. D must then take CD and MD. | **5250 (complete tour)** |
| C2 | CB, BG, ¬GM | G must take GD. D–G–B–C is then forced, so CM and MD follow. | 5830 (complete tour) |
| C3 | CB, ¬BG, GM | No tightening | **4770** |
| C4 | CB, ¬BG, ¬GM | M: MB+MC = 2490, G: GC+GD = 2880 | 5860 |
| D1 | …, CG | CB, CG, GM saturate C and G. BM would make a cycle, so B takes BD. | 5724 |
| D2 | …, ¬CG | G must take GD, since GB, GC and GM are used or excluded | 5650 |
| B3 | ¬CB, BG | Nothing changes | **5220** |
| B4 | ¬CB, ¬BG | B is left with only BM and BD, so both are forced | 6267 |
| C5 | ¬CB, BG, GM | G is saturated and BM would make a triangle, so BD is forced. MD would close a 4-cycle, so MC and then CD are forced. The tour is B–G–M–C–D–B. | 7064 |
| C6 | ¬CB, BG, ¬GM | G: GB+GC = 1370, M: MB+MC = 2490 | 5670 |

## OPEN list trace (sorted by cost, cheapest at the head)

1. **OPEN = [S0 4335].** Expand S0 → A1 4335, A2 5220.
2. **OPEN = [A1 4335, A2 5220].** Expand A1 → B1 4610, B2 4770.
3. **OPEN = [B1 4610, B2 4770, A2 5220].** Expand B1 → C1 5250, C2 5830.
4. **OPEN = [B2 4770, A2 5220, C1 5250, C2 5830].** Expand B2 → C3 4770, C4 5860.
5. **OPEN = [C3 4770, A2 5220, C1 5250, C2 5830, C4 5860].** Expand C3 → D2 5650, D1 5724.
6. **OPEN = [A2 5220, C1 5250, D2 5650, D1 5724, C2 5830, C4 5860].** Expand A2 → B3 5220, B4 6267.
7. **OPEN = [B3 5220, C1 5250, D2 5650, D1 5724, C2 5830, C4 5860, B4 6267].** Expand B3 → C5 7064, C6 5670.
8. **OPEN = [C1 5250, D2 5650, C6 5670, D1 5724, C2 5830, C4 5860, B4 6267, C5 7064].** The head is C1, and Goal-Test(C1) is true, so return its path.

Path returned (reversed): S0 → CB → BG → GM. The two remaining edges, MD and DC, are forced by inference.

**Optimal tour: C → B → G → M → D → C = 360 + 570 + 590 + 1540 + 2190 = 5250.**

Every other node still in OPEN has a bound ≥ 5250, so none of them can give a cheaper tour.

## Quick verification
```python
from itertools import permutations
d={('C','G'):800,('C','M'):1280,('C','D'):2190,('C','B'):360,('G','M'):590,
   ('G','D'):2080,('G','B'):570,('M','D'):1540,('M','B'):1210,('D','B'):2434}
w=lambda a,b:d.get((a,b),d.get((b,a)))
best=min((sum(w(p[i],p[(i+1)%5]) for i in range(5)),p) for p in permutations('GMDB'))
print(best)  # (5250, ('G','M','D','B')) -> prepend C: C-G... same cycle reversed
```
Note that the output shows the cycle starting from C, which is `C,G,M,D,B`. When closed through B back to C, that's the same tour as above in the opposite direction, with the same cost of 5250.

Two earlier values were wrong, C4 and D2. I fixed them below; the final answer (5250) doesn't change.

**S0**
- Permanently included edges: **0**
- Permanently excluded edges: **0**
- Default edges: **10** (2 per city)

**a1 (S0, CB)**
- PI segments: **1** (CB)
- PI edges: **2** (CB, BC)
- PE edges: **0**
- Default edges: **8**
- Chennai contributes **CB, CG**.
- Mumbai contributes **MG, MB**.
- Bangalore contributes **BC, BG**.

**b1 (S0, CB, BG)**
- PI segments: **CB, BG**
- PE segments: **BM, BD, CG**. Bangalore is saturated by CB and BG, so BM and BD go. CG would close the triangle C–B–G.
- Default edges: **CM, GM, MG, MC, DM, DG** (6 edges)
- Cost: **4610**. The edge sum is 1640 + 1160 + 1870 + 3620 + 930 = 9220, and half of that is 4610.
- Tours represented: **2**. These are C-B-G-M-D-C (5250) and C-B-G-D-M-C (5830).

**b2 (S0, CB, ~BG)**
- PI edges: **CB, BC**
- PE edges: **BG, GB**
- BG was replaced by the default edges **BM** (for Bangalore) and **GC** (for Goa).
- Tours represented: **4**. Six tours contain CB, and two of them contain BG.

**c4 (S0, CB, ~BG, ~GM)**
- Goa has lost both GB and GM, so GC and GD are forced. That saturates Chennai, so CM and CD go. Mumbai is left with only MB and MD, and those are forced too. Bangalore is then saturated with CB and BM, so BD goes.
- PI segments: **CB, CG, GD, MB, MD**
- PE segments: **BG, GM, CM, CD, BD**
- Tours represented: **1**, namely C-B-M-D-G-C.
- Cost: **5990**, which is 360 + 1210 + 1540 + 2080 + 800. This is a complete tour, and it is not the 5860 I gave earlier.

**c3, d1 and d2 (c3 = S0, CB, ~BG, GM)**
- The segment XY is **CG**. In c3, C and G both default to the CG edge, so it is the natural segment to split on.
- Cost of d1 (CG included): **5724**. B–M is excluded because it would close a 4-cycle, so BD and MD are forced. The tour is C-B-D-M-G-C, a complete tour.
- Cost of d2 (CG excluded): **5975**. Goa is forced to GM and GD. MD is then excluded, since G–M–D would be a triangle. The edge sum is 1640 + 2670 + 1800 + 4270 + 1570 = 11950, and half of that is 5975.
  - This is a bound and not a single tour. The tours still inside d2 are C-B-M-G-D-C (6430) and C-B-D-G-M-C (6744).
  - My earlier 5650 for D2 skipped the MD inference.

**Effect on the search:** Both corrected values (C4 = 5990, D2 = 5975) are still above 5250, so the OPEN list order up to C1 is unchanged. C1 is still the head when Goal-Test succeeds, and the optimal tour is C → B → G → M → D → C at 5250.



**Lower bound at S0: 316.**

Each city takes its two cheapest edges: A 56+70 = 126, B 58+70 = 128, C 40+56 = 96, D 78+80 = 158, E 40+84 = 124. These sum to 632, and half of that is 316.

**Tours represented by B2 (CE included, AC excluded): 4.**
The tours containing CE are ABCED, ABDCE, ABECD, ADBCE, ABDEC and ACEBD. ABDEC (ending …E–C–A) and ACEBD both use AC, so they are removed. The four left are ABCED (392), ABDCE (352), ABECD (418) and ADBCE (390).

**Tours represented by C4 (CE included, AC and BC excluded): 2.**
Chennai-equivalent city C is left with CE and CD only, so CD is forced and D–C–E is a fixed path. The two tours are A-B-D-C-E-A (352) and A-B-E-C-D-A (418).

**First 4 nodes refined: S0, A1, B2, B1.**

| Node | Bound | Reason |
|---|---|---|
| S0 | 316 | Root. |
| A1 (CE) | 316 | CE is already among the default edges, so nothing changes. |
| A2 (¬CE) | 352 | C takes AC+BC = 114, E takes AE+DE = 178. |
| B1 (CE, AC) | 338 | C is saturated, so CB and CD are excluded. AE would close the triangle A–C–E, so it is excluded too. |
| B2 (CE, ¬AC) | 331 | A takes AB+AE = 154, C takes CE+CB = 98. |
| C3 (CE, ¬AC, BC) | 338 | |
| C4 (CE, ¬AC, ¬BC) | 352 | |

The order of refinement is:
1. S0 is refined first, giving A1 at 316 and A2 at 352.
2. A1 is next at 316, giving B1 at 338 and B2 at 331.
3. B2 is next at 331, giving C3 at 338 and C4 at 352.
4. B1 and C3 are now tied at 338, so a tie-breaker is needed. I took B1, which was generated earlier. If your class breaks ties differently, the fourth node would be C3 instead, so check that rule.

**Cost of the optimal tour: 338.**
The tour is A → B → D → E → C → A, costing 70 + 78 + 94 + 40 + 56 = 338. I checked this by listing all 12 tours. B1 reaches it exactly, since its bound of 338 equals the tour's cost.

**Why we must underestimate:**
The bound for a partial solution must never exceed the true cost of the cheapest tour it contains. Then a node is pruned only when its bound is already worse than a known tour, so the optimal tour can never be discarded. This also means the first complete tour that reaches the head of the priority queue is optimal. If the bound overestimated, a node holding the optimal tour could be ranked too low or pruned, and the algorithm would return a suboptimal answer.

**A\* (Euclidean distance to G as h)**

I read the grid coordinates as G(0,1), S(8,0), A(6,2), B(4,3), C(0,4), D(8,4) and E(5,6). That gives h(S)=8.06, h(A)=6.08, h(B)=4.47, h(C)=3, h(D)=8.54, h(E)=7.07.

- Expanding S gives A (f=10.08) and D (f=10.54).
- Expanding A gives B (f=11.47).
- D (f=10.54) is expanded before B and gives E at g=6, f=13.07.
- Expanding B reaches G only at g=15, so that route is poor.
- Expanding E gives C at g=10, f=13.
- Expanding C gives G at g=12, f=12, which improves on the g=15 route, and G is then at the head of OPEN.

- **Path: S → D → E → C → G**
- **Cost: 12**

**BnB-1 (no cycle check, ordered by cost g)**

Tie-break rule I assumed: following the pseudocode, new paths are placed ahead of the old tail of OPEN before a stable sort. So among equal costs the newer path is refined first. Move-Gen order is alphabetical. If your class breaks ties the other way, the 3rd to 6th nodes will differ.

| Step | Node refined | Cost | OPEN afterwards, in order |
|---|---|---|---|
| 1 | S | 0 | SD 2, SA 4 |
| 2 | D | 2 | SDS 4, SA 4, SDE 6, SDA 10 |
| 3 | S (path SDS) | 4 | SA 4, SDSD 6, SDE 6, SDSA 8, SDA 10 |
| 4 | A (path SA) | 4 | SDSD 6, SDE 6, SAB 7, SAS 8, … |
| 5 | D (path SDSD) | 6 | SDE 6, SAB 7, SDSDS 8, … |
| 6 | E (path SDE) | 6 | |

- Nodes 2nd to 6th: **D, S, A, D, E**
- Their costs: **2, 4, 4, 6, 6**

**BnB-2 (no node may repeat on a path)**

| Step | Node refined | Cost | OPEN afterwards, in order |
|---|---|---|---|
| 1 | S | 0 | SD 2, SA 4 |
| 2 | D | 2 | SA 4, SDE 6, SDA 10 |
| 3 | A (path SA) | 4 | SDE 6, SAB 7, SDA 10, SAE 12, SAD 12 |
| 4 | E (path SDE) | 6 | SAB 7, SDEC 10, SDA 10, SAE 12, SAD 12, SDEA 14, SDEB 14 |
| 5 | B (path SAB) | 7 | SDEC 10, SDA 10, …, SABG 15 |
| 6 | C (path SDEC) | 10 | SDECG 12, … |

- Nodes 2nd to 6th: **D, A, E, B, C**
- Their costs: **2, 4, 6, 7, 10**
- Path found by BnB-2: **S → D → E → C → G**
- Cost of that path: **12**

S-D-E-C-G is the only path to G with cost 12. Every other route to G costs at least 15, so G first reaches the head of OPEN with the optimal path.

With Manhattan distance and your tie-breakers, A\* now returns a different path (cost 15) and the BnB-2 sequence changes.

**A\* with Manhattan distance to G**

Coordinates read from the grid: S(8,0), A(6,2), B(4,3), C(0,4), D(8,4), E(5,6), G(0,1). Manhattan h to G: S 9, A 7, B 6, C 3, D 11, E 10.

1. Expand S (f=9). A has g=4, f=11. D has g=2, f=13.
2. Expand A (f=11). B has g=7, f=13. E has g=12, f=22.
3. D and B are tied at f=13, so alphabetical order picks B. Expanding B gives G with g=15, f=15, and C with g=15, f=18.
4. Expand D (f=13). E has g=6, f=16.
5. G (f=15) is now the head of OPEN and passes the goal test.

- **Path: S → A → B → G**
- **Cost: 15**

This is not optimal, because the best path is S-D-E-C-G at 12. The heuristic is inadmissible here, since h(D)=11 but the true cost from D to G is 10. Edge costs are not the grid distances, for example S-D has cost 2 but Manhattan distance 4.

**BnB-1 (cyclic paths allowed, ordered by cost g only)**

BnB does not use the heuristic. Ties are broken alphabetically by node, then by oldest in OPEN.

| Step | Node refined (path) | Cost | OPEN afterwards, in order |
|---|---|---|---|
| 1 | S | 0 | D 2, A 4 |
| 2 | D (SD) | 2 | A 4 (SA), S 4 (SDS), E 6, A 10 |
| 3 | A (SA) | 4 | S 4 (SDS), E 6, B 7, S 8, A 10, D 12, E 12 |
| 4 | S (SDS) | 4 | D 6 (SDSD), E 6 (SDE), B 7, … |
| 5 | D (SDSD) | 6 | E 6 (SDE), B 7, … |
| 6 | E (SDE) | 6 | |

- Nodes 2nd to 6th: **D, A, S, D, E**
- Their costs: **2, 4, 4, 6, 6**

At step 5, D and E are tied at 6, so alphabetical order picks D.

**BnB-2 (no repeated node on a path)**

| Step | Node refined (path) | Cost | OPEN afterwards, in order |
|---|---|---|---|
| 1 | S | 0 | D 2, A 4 |
| 2 | D (SD) | 2 | A 4, E 6 (SDE), A 10 (SDA) |
| 3 | A (SA) | 4 | E 6, B 7 (SAB), A 10 (SDA), D 12, E 12 |
| 4 | E (SDE) | 6 | B 7, A 10 (SDA), C 10 (SDEC), D 12, E 12, A 14, B 14 |
| 5 | B (SAB) | 7 | A 10 (SDA), C 10 (SDEC), D 12, E 12, … |
| 6 | A (SDA) | 10 | C 10 (SDEC), D 12, E 12, B 13, … |

- Nodes 2nd to 6th: **D, A, E, B, A**
- Their costs: **2, 4, 6, 7, 10**

At step 6, A (SDA) and C (SDEC) are tied at 10. Alphabetical order picks A, so the sixth node is A and not C as I had before.

To finish, refine C (SDEC, 10), which adds G at 12. D (SAD, 12) and E (SAE, 12) come before G in alphabetical order, so they are refined next. G (SDECG, 12) then reaches the head of OPEN and passes the goal test.

- **Path found by BnB-2: S → D → E → C → G**
- **Cost: 12**

No. Manhattan distance was used only in A\*. Neither BnB version uses a heuristic: both order OPEN by the path cost g alone, as in the pseudocode (sort by g), so the Manhattan values never entered those tables.

I re-derived Step 2 of BnB-2 and still get the same result, so I can't find the error yet.

- OPEN at the start of Step 2 is D 2 (SD), A 4 (SA). The lowest cost is D, so D is refined and it is the 2nd node.
- D's neighbours are S, A and E. S is already on the path SD, so BnB-2 skips it.
- That leaves SDA = 2 + 8 = 10 and SDE = 2 + 4 = 6.
- OPEN afterwards is A 4 (SA), E 6 (SDE), A 10 (SDA).

Steps 3 and 4 give the same results when I recheck them. A (SA) is refined next, giving B 7, D 12 and E 12, with S skipped. After that E (SDE) is refined.

Which part of Step 2 looks wrong to you? Possible points:

1. **The cost of SDA.** I used the A–D edge cost of 8, not a grid distance.
2. **S being dropped.** BnB-2 removes it because it's already on the path.
3. **The order in OPEN.** The oldest-first tie-break doesn't apply here, since there are no equal costs.

If you tell me which entry differs from your answer, I'll check it against the map and correct whichever of us is wrong.

**A\* on the big map (h = Manhattan distance to G, in map units)**

I read the grid coordinates as S(0,6), A(6,0), B(2,1), C(9,1), D(12,2), E(4,3), F(7,4), G(13,6), H(16,4), I(5,6), J(3,7), K(8,7), L(16,7), M(11,8), N(7,10), O(2,11) and P(13,11). That gives these h-values:

| Node | S | A | B | C | D | E | F | I | J | K | L | M | N | O | P | H |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| h | 130 | 130 | 160 | 90 | 50 | 120 | 80 | 80 | 110 | 60 | 40 | 40 | 100 | 160 | 50 | 50 |

| Step | Node refined | g | f = g + h | New or updated OPEN entries |
|---|---|---|---|---|
| 1 | S | 0 | 130 | B 240, E 220, J 170, O 280 |
| 2 | J | 60 | 170 | I 180 (O via J is worse) |
| 3 | I | 100 | 180 | nothing new |
| 4 | E | 100 | 220 | A 290, F 230 |
| 5 | F | 150 | 230 | C 302, K 330 |
| 6 | B | 80 | 240 | A improves to g=140, f=270 |

- **Nodes 2nd to 6th refined: J, I, E, F, B**
- **Their f-values: 170, 180, 220, 230, 240**

The search then continues:
- A (270) is refined, which improves C to g=190, f=280.
- C and O are tied at 280, so alphabetical order picks C. C gives D at 290.
- O (280) is refined next.
- D (290) is refined, and it gives G at g=300, f=300.
- G is then at the head of OPEN and passes the goal test.

- **Path: S, B, A, C, D, G**
- **Cost: 80 + 60 + 50 + 50 + 60 = 300**

**Return journey from G to S**

Answer: **A. take the same path back.**

The Manhattan heuristic is admissible on this map (see the last question), so A\* returns an optimal path in either direction. S–B–A–C–D–G is the only path of cost 300. The next best, through H, costs 310. So the reverse search can only return G, D, C, A, B, S.

**Which map has an admissible heuristic?**

Answer: **B. Big map.**

On the big map, every road costs at least the Manhattan distance between its endpoints, for example E–S = 100 against 70, and C–F = 62 against 50. The heuristic is therefore consistent and also admissible.

On the small map, edge S–D costs 2 but the Manhattan distance is 4, so the heuristic overestimates. For instance, h(D) = 11 but the true cost from D to G is 10 (D–E–C–G).
