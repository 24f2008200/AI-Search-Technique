The key distinction is **what the solver is searching over and what makes a solution acceptable**.

### 1. State Space Search

In **state-space search**, we think of a problem as:

> **initial state → possible actions → new states → goal state**

The solver explores different possible states until it finds a state satisfying the goal.

**Example: 8-puzzle**

![Image](https://images.openai.com/static-rsc-4/hUkZqYP0iT68sxGzrIJZXNXLKe2RnwS_DuwAvNfwc3oA6bEVZ8b5wZ9MMc-s5_SNuyBwTBzR9IaU9npQmQuYl2Io77Gkk2a2KqEVWfc_Cj9YyE52zGEGC52Nm6WnnMq_wZVW6tVNSvxnK9rN1-c6dgWwyZJh3iRaBrAwl4jPJEMHMIdGo6oRYYDmVD1M3Ett?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/a88vlU71vQiwDUlSLgj9gWL9NvjmNObZlyVhOra-NKBURahvVlkK8ZpCPRKBXc-QQ22aVX1zmMinX8gOZ5Oa9qT02Jqu4c4kd7hwoHg2TKSzaj-jcTJ2hXYWE8F1-7NQC5Pg4SKxpuxtM7tilpgXp1mUpJG4mM3NM9QT0jcRgjE2nYCFYkmtjDIvf8cuEHQX?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/ZQ6ZeYhootGXFYVXENOHcCC7I6qnjqYza7CReGFtWzxrQOa6b-ddsnQQT1I33XRahjSrfTvQRs_SZMTxD2cm07vBU5Ko_5LZiRreDda9ZmfTZm0LLndR9-qNXY6JxUDCGbrHI2BERuAp3Ev6qiEMQyjRkOvUxgqMc2g09wP0sGjG2DHXM7wWfoqCpwD6lgYq?purpose=fullsize)

* **State:** a particular arrangement of the 8 tiles.
* **Action:** move a tile into the empty position.
* **Initial state:** the given arrangement.
* **Goal state:** the required arrangement.

The algorithm might explore:

$$
S_0 \rightarrow S_1 \rightarrow S_2 \rightarrow \cdots \rightarrow S_g
$$

The emphasis is on **finding a path through a space of states**.

---

### 2. Constraint Optimisation

Here, instead of thinking primarily in terms of a sequence of states, we describe the problem using:

> **Variables + possible values + constraints + possibly an objective**

**Example: Map colouring**

Suppose we have four regions:

$$
A,B,C,D
$$

Each variable represents a region:

$$
A,B,C,D \in \{Red,Green,Blue\}
$$

Constraints might be:

$$
A\neq B,\quad A\neq C,\quad B\neq C,\quad C\neq D
$$

The solver searches for an **assignment of values** satisfying all the constraints.

If there is also an objective, for example:

> Use as few colours as possible,

then it becomes a **constraint optimisation problem**:

$$
\min \{\text{number of colours used}\}
$$

The emphasis is on **finding a feasible/best assignment under constraints**.

---

### The difference in one picture

|                   | State-space search                 | Constraint optimisation                       |
| ----------------- | ---------------------------------- | --------------------------------------------- |
| Think in terms of | **States**                         | **Variables and values**                      |
| Main question     | "Which state should I go to next?" | "Which assignment satisfies the constraints?" |
| Solution          | Usually a **path** to a goal       | Usually an **assignment**                     |
| Typical structure | Initial state → actions → goal     | Variables + domains + constraints + objective |
| Example           | 8-puzzle                           | Map colouring                                 |
| Optimisation      | Often shortest/cheapest path       | Best assignment according to objective        |

### An important subtlety

The two approaches are **not mutually exclusive**.

A constraint problem can itself be solved using **search**.

For map colouring, for example, we could search:

$$
A=R
\rightarrow B=G
\rightarrow C=R
\rightarrow \cdots
$$

But the **model of the problem** is still a constraint model.

So I would remember the distinction as:

> **State-space search:** *"Where can I go from here?"*
> **Constraint optimisation:** *"What values can I assign so that all my requirements are satisfied — and perhaps optimised?"*

That distinction is more fundamental than the particular algorithm used to solve either problem.
