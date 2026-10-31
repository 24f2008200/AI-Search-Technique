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

\#chatgpt-mermaid-\_r_dh\_{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;fill:rgb(237, 237, 237);}@keyframes edge-animation-frame{from{stroke-dashoffset:0;}}@keyframes dash{to{stroke-dashoffset:0;}}#chatgpt-mermaid-\_r_dh\_ .edge-animation-slow{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 50s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_dh\_ .edge-animation-fast{stroke-dasharray:9,5!important;stroke-dashoffset:900;animation:dash 20s linear infinite;stroke-linecap:round;}#chatgpt-mermaid-\_r_dh\_ .error-icon{fill:rgb(48, 48, 48);}#chatgpt-mermaid-\_r_dh\_ .error-text{fill:rgb(237, 237, 237);stroke:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .edge-thickness-normal{stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .edge-thickness-thick{stroke-width:3.5px;}#chatgpt-mermaid-\_r_dh\_ .edge-pattern-solid{stroke-dasharray:0;}#chatgpt-mermaid-\_r_dh\_ .edge-thickness-invisible{stroke-width:0;fill:none;}#chatgpt-mermaid-\_r_dh\_ .edge-pattern-dashed{stroke-dasharray:3;}#chatgpt-mermaid-\_r_dh\_ .edge-pattern-dotted{stroke-dasharray:2;}#chatgpt-mermaid-\_r_dh\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_dh\_ .marker.cross{stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_dh\_ svg{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:16px;}#chatgpt-mermaid-\_r_dh\_ p{margin:0;}#chatgpt-mermaid-\_r_dh\_ .label{font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .cluster-label text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .cluster-label span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .cluster-label span p{background-color:transparent;}#chatgpt-mermaid-\_r_dh\_ .label text,#chatgpt-mermaid-\_r_dh\_ span{fill:rgb(237, 237, 237);color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .node rect,#chatgpt-mermaid-\_r_dh\_ .node circle,#chatgpt-mermaid-\_r_dh\_ .node ellipse,#chatgpt-mermaid-\_r_dh\_ .node polygon,#chatgpt-mermaid-\_r_dh\_ .node path{fill:rgb(9, 23, 44);stroke:rgb(31, 78, 148);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .rough-node .label text,#chatgpt-mermaid-\_r_dh\_ .node .label text,#chatgpt-mermaid-\_r_dh\_ .image-shape .label,#chatgpt-mermaid-\_r_dh\_ .icon-shape .label{text-anchor:middle;}#chatgpt-mermaid-\_r_dh\_ .node .katex path{fill:#000;stroke:#000;stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .rough-node .label,#chatgpt-mermaid-\_r_dh\_ .node .label,#chatgpt-mermaid-\_r_dh\_ .image-shape .label,#chatgpt-mermaid-\_r_dh\_ .icon-shape .label{text-align:center;}#chatgpt-mermaid-\_r_dh\_ .node.clickable{cursor:pointer;}#chatgpt-mermaid-\_r_dh\_ .root .anchor path{fill:rgb(175, 175, 175)!important;stroke-width:0;stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_dh\_ .arrowheadPath{fill:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_dh\_ .edgePath .path{stroke:rgb(175, 175, 175);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .flowchart-link{stroke:rgb(175, 175, 175);fill:none;}#chatgpt-mermaid-\_r_dh\_ .edgeLabel{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_dh\_ .edgeLabel p{background-color:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_dh\_ .edgeLabel rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_dh\_ .labelBkg{background-color:rgba(0, 0, 0, 0.5);}#chatgpt-mermaid-\_r_dh\_ .cluster rect{fill:rgb(48, 48, 48);stroke:rgba(255, 255, 255, 0.15);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .cluster text{fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ .cluster span{color:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ div.mermaidTooltip{position:absolute;text-align:center;max-width:200px;padding:2px;font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";font-size:12px;background:rgb(48, 48, 48);border:1px solid rgba(255, 255, 255, 0.15);border-radius:2px;pointer-events:none;z-index:100;}#chatgpt-mermaid-\_r_dh\_ .flowchartTitleText{text-anchor:middle;font-size:18px;fill:rgb(237, 237, 237);}#chatgpt-mermaid-\_r_dh\_ rect.text{fill:none;stroke-width:0;}#chatgpt-mermaid-\_r_dh\_ .icon-shape,#chatgpt-mermaid-\_r_dh\_ .image-shape{background-color:rgb(0, 0, 0);text-align:center;}#chatgpt-mermaid-\_r_dh\_ .icon-shape p,#chatgpt-mermaid-\_r_dh\_ .image-shape p{background-color:rgb(0, 0, 0);padding:2px;}#chatgpt-mermaid-\_r_dh\_ .icon-shape .label rect,#chatgpt-mermaid-\_r_dh\_ .image-shape .label rect{opacity:0.5;background-color:rgb(0, 0, 0);fill:rgb(0, 0, 0);}#chatgpt-mermaid-\_r_dh\_ .label-icon{display:inline-block;height:1em;overflow:visible;vertical-align:-0.125em;}#chatgpt-mermaid-\_r_dh\_ .node .label-icon path{fill:currentColor;stroke:revert;stroke-width:revert;}#chatgpt-mermaid-\_r_dh\_ .node .neo-node{stroke:rgb(31, 78, 148);}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node rect,#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].cluster rect,#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node polygon{stroke:url(#chatgpt-mermaid-\_r_dh\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].swimlane.cluster rect{filter:none;}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node path{stroke:url(#chatgpt-mermaid-\_r_dh\_-gradient);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node .outer-path{filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node .neo-line path{stroke:rgb(31, 78, 148);filter:none;}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node circle{stroke:url(#chatgpt-mermaid-\_r_dh\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].node circle .state-start{fill:#000000;}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].icon-shape .icon{fill:url(#chatgpt-mermaid-\_r_dh\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_dh\_ [data-look="neo"].icon-shape .icon-neo path{stroke:url(#chatgpt-mermaid-\_r_dh\_-gradient);filter:drop-shadow( 1px 2px 2px rgba(185,185,185,1));}#chatgpt-mermaid-\_r_dh\_ .node text{font-size:14px;font-weight:600;letter-spacing:normal;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_dh\_ .edgeLabels text{font-size:13px;font-weight:600;letter-spacing:-0.08px;fill:rgb(153, 206, 255);}#chatgpt-mermaid-\_r_dh\_ .node tspan[font-weight="normal"],#chatgpt-mermaid-\_r_dh\_ .edgeLabels tspan[font-weight="normal"]{font-weight:600;}#chatgpt-mermaid-\_r_dh\_ .edgeLabel .label rect{opacity:1;rx:13px;ry:13px;fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .node rect,#chatgpt-mermaid-\_r_dh\_ .node circle,#chatgpt-mermaid-\_r_dh\_ .node ellipse,#chatgpt-mermaid-\_r_dh\_ .node polygon,#chatgpt-mermaid-\_r_dh\_ .node path{fill:rgb(0, 40, 77);stroke:rgba(255, 255, 255, 0.1);stroke-width:1px;}#chatgpt-mermaid-\_r_dh\_ .node rect{rx:16px;ry:16px;}#chatgpt-mermaid-\_r_dh\_ .mermaid-decision .label-container{fill:rgb(0, 14, 26);stroke:rgb(26, 62, 95);stroke-dasharray:2,2;}#chatgpt-mermaid-\_r_dh\_ .edgePaths .flowchart-link{stroke:rgb(175, 175, 175);stroke-width:1px;stroke-linecap:round;stroke-linejoin:round;}#chatgpt-mermaid-\_r_dh\_ .marker{fill:rgb(175, 175, 175);stroke:rgb(175, 175, 175);}#chatgpt-mermaid-\_r_dh\_ :root{--mermaid-font-family:-apple-system-body,ui-sans-serif,-apple-system,system-ui,"Segoe UI",Helvetica,"Apple Color Emoji",Arial,sans-serif,"Segoe UI Emoji","Segoe UI Symbol";}Root: LB = 35A → B: LB = 35A → C: LB = 39A → D: LB = 40A → B → C: LB = 39A → B → D: LB = 35A → B → D → C → ACost = 35PrunedPrunedPruned

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
