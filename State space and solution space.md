The easiest way to distinguish them is:

> **State space = all possible situations the problem can be in.**
> **Solution space = all possible answers that satisfy the problem's requirements.**

### Example: 8-puzzle

Suppose we have the familiar 8-puzzle.

![Image](https://images.openai.com/static-rsc-4/MrP_eVyKVtE558ZmMwxODxg-5Hhx0Lr195gAHdhOGqvdymERKJHLE-7c_7E9xBQThNE2VLo2TUzOp4YRRRQ02d5EQPwFTQdY_z0WHS2cpsfOJ5cID0ypA0_R1kxEpVi6AzDDfJIlUZZ1G9xrBn70goDqACAVIFscfGo5c3Af4TnCH4Qie7e6z9rxvHnGz0UT?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/sWp8FrO2_iebPrDp4DCRIDCiaXimxcaFgL2TTYLO6uCILqX0dqWV7rAeDtgf9bZ4U5IQTTuh_LOE7l7h6VxBB_ngh5dXqe0OGCZJ_aUHDBMT6z4MtmZ27rYUTfBARFIx5yWdZFVWA4Mb78V-945Tg7oaGCkyckZh2i6X-_O_8_A28odNpXq7G-cHuyDEWXNa?purpose=fullsize)

![Image](https://images.openai.com/static-rsc-4/CqNJ7zJl69PDk6UsYR7EopTwZ47pkcJrsYXkr7XP_FckFGkIt3_729EeVCE4wofXXezb4oDAeStuvKrr6hECEyCnI2rA9lle4jCQQ5smUkd8zBQ_hOwP1OQYGy2Ke1ZInQ_RjNxIKG8z7ZWOcFwtaCcVqsbuVWTqh1Sl1tCoMc0MW5slnHgIGIcKSP-dB0vv?purpose=fullsize)

**State space**

Every possible arrangement of the tiles is a **state**.

For example:

$$
\begin{matrix}
1&2&3\\
4&5&6\\
7&8&\square
\end{matrix}
$$

is one state.

Another arrangement is another state.

So the **state space** is:

$$
\boxed{\text{all configurations that can potentially occur}}
$$

The search algorithm moves from one state to another.

---

**Solution space**

Now suppose our goal is to transform the initial arrangement into a particular target arrangement.

A **solution** is a sequence of moves that accomplishes this:

$$
S_0 \rightarrow S_1 \rightarrow S_2 \rightarrow \cdots \rightarrow S_g
$$

There may be many such sequences.

The **solution space** is therefore concerned with the possible **solutions**—for example, different valid sequences of moves from initial state to goal.

---

### Map colouring makes the distinction even clearer

Suppose we have a map with regions \(A,B,C,D\), and colours:

$$
\{R,G,B\}
$$

**State space:**

A partial or complete assignment such as

$$
A=R,\quad B=G
$$

is a state.

The state space contains the possible assignments/configurations explored during the search.

**Solution space:**

The assignments that satisfy **all** the constraints are solutions:

$$
A\neq B,\quad B\neq C,\quad C\neq D,\ldots
$$

So:

$$
\boxed{\text{State space = possibilities we can explore}}
$$

$$
\boxed{\text{Solution space = possibilities that satisfy the problem}}
$$

### One subtle but important point

The **solution space is usually a subset of the state space only when states are themselves candidate solutions**.

More generally, the two concepts belong to slightly different viewpoints:

* **State space** describes the *world/configurations through which the search moves*.
* **Solution space** describes the *set of candidate answers being considered*.

For an optimisation problem, we might have:

$$
\text{Candidate solutions}
\supset
\text{Feasible solutions}
\supset
\text{Optimal solutions}
$$

So if Prof. Saya is discussing **problem-solving techniques**, I would keep this mental picture:

> **State-space search:** *"Explore the landscape of states until I reach a goal."*
> **Solution-space search:** *"Explore the possible answers until I find one—or the best one."*
