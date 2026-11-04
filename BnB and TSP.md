<img width="2200" height="822" alt="image" src="https://github.com/user-attachments/assets/e58e99a0-537e-47c7-9eb5-bca760ffdc6f" />

<img width="1674" height="1291" alt="image" src="https://github.com/user-attachments/assets/e29ac04e-9e91-422c-b8ac-9c5999913a39" />

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
