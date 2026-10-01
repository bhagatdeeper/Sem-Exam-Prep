# 🎯 DAA Semester Exam Fast-Track Preparation Kit
**Subject:** Design and Analysis of Algorithms (DAA)  
**Target:** 1-Month Exam Preparation | Beginner-Friendly & High-Scoring  

---

## 📑 Quick Navigation
1. [Cheat Sheet: Master Complexity Table](#-master-complexity-table)
2. [Module 1: Algorithm Development & Complexity Analysis](#-module-1-algorithm-development--complexity-analysis)
3. [Module 2: Algorithm Design Techniques](#-module-2-algorithm-design-techniques)
4. [Module 3: String Matching Algorithms](#-module-3-string-matching-algorithms)
5. [Module 4: Graph Algorithms & Network Flows](#-module-4-graph-algorithms--network-flows)
6. [Module 5: Complexity Classes, Approximation & Randomized Algorithms](#-module-5-complexity-classes-p-np-and-beyond)
7. [Important Semester Questions Bank (2, 5 & 10 Marks with Solutions)](#-important-semester-questions-bank)

---

# ⚡ Master Complexity Table

| Algorithm / Problem | Paradigm | Time Complexity (Best / Avg / Worst) | Space Complexity | Key Exam Catch |
|---|---|---|---|---|
| **Strassen’s Matrix Mult.** | Divide & Conquer | $\Theta(n^{\log_2 7}) \approx O(n^{2.81})$ | $O(\log n)$ recursion | 7 multiplications instead of 8 |
| **Fractional Knapsack** | Greedy | $O(n \log n)$ | $O(1)$ | Items can be broken; sort by $v_i / w_i$ |
| **0/1 Knapsack** | Dynamic Programming | $O(n \cdot W)$ (Pseudo-polynomial) | $O(n \cdot W)$ or $O(W)$ | Items cannot be broken; greedy fails |
| **Job Sequencing with Deadlines** | Greedy | $O(n^2)$ or $O(n \log n)$ with DSU | $O(n)$ | Sort by profit descending, put in latest free slot |
| **Prim’s Algorithm** | Greedy | $O(E \log V)$ (Binary Heap) / $O(V^2)$ (Matrix) | $O(V)$ | Grows a single tree from a start vertex |
| **Kruskal’s Algorithm** | Greedy | $O(E \log E)$ or $O(E \log V)$ | $O(V)$ | Sorts all edges globally, uses Union-Find |
| **Huffman Coding** | Greedy | $O(n \log n)$ | $O(n)$ | Prefix-free code; greedy min-heap |
| **Matrix Chain Mult. (MCM)** | Dynamic Programming | $O(n^3)$ time, $O(n^2)$ space | $O(n^2)$ | Minimizes scalar multiplications: $m[i,j]$ |
| **TSP (Brute Force)** | Exhaustive Search | $O(n!)$ | $O(n)$ | Check all permutations |
| **TSP (Dynamic Prog.)** | DP (Held-Karp) | $O(n^2 \cdot 2^n)$ | $O(n \cdot 2^n)$ | Subsets of visited vertices |
| **N-Queens** | Backtracking | $O(N!)$ | $O(N)$ | Prunes invalid columns & diagonals |
| **Subset Sum** | Backtracking / DP | $O(2^n)$ (Backtracking) / $O(n \cdot \text{Sum})$ (DP) | $O(n)$ / $O(n \cdot \text{Sum})$ | State-space tree pruning |
| **Naive String Matching** | Brute Force | Best: $O(n)$, Worst: $O((n-m+1)m)$ | $O(1)$ | Shifts pattern by 1 each mismatch |
| **KMP Algorithm** | Preprocessing + Shift | Preprocess $\pi$: $O(m)$, Search: $O(n)$ $\rightarrow$ Total: $O(n+m)$ | $O(m)$ | Never backtracks text pointer! |
| **Dijkstra’s Algorithm** | Greedy | $O((V + E) \log V)$ | $O(V)$ | **Fails for negative edge weights** |
| **Bellman-Ford Algorithm** | Dynamic Programming | $O(V \cdot E)$ | $O(V)$ | Handles negative weights & detects negative cycles |
| **Floyd-Warshall** | Dynamic Programming | $O(V^3)$ | $O(V^2)$ | All-pairs shortest path; triple nested loop |
| **Ford-Fulkerson (Edmonds-Karp)**| Augmenting Path | $O(E \cdot |f^*|)$ (DFS) / $O(V \cdot E^2)$ (BFS) | $O(V + E)$ | Min-Cut = Max-Flow theorem |
| **Push-Relabel (Preflow-Push)** | Network Flow | $O(V^2 E)$ (Generic) / $O(V^3)$ (FIFO) | $O(V)$ | Maintains preflow & height function (excess flow) |

---

# 📦 Module 1: Algorithm Development & Complexity Analysis

### 1. Stages of Algorithm Development
When solving any computational problem in computer science, an algorithm goes through five clear stages:
1. **Problem Formulation / Description:** Clearly define inputs, outputs, constraints, and edge cases.
2. **Technique Identification:** Choose an algorithmic paradigm (e.g., Divide and Conquer, Greedy, DP, Backtracking, Branch & Bound).
3. **Algorithm Design:** Formulate pseudo-code or step-by-step logic.
4. **Correctness Verification:** Mathematically prove it works for all valid inputs (e.g., Loop Invariants, Mathematical Induction).
5. **Complexity Analysis:** Evaluate running time (Time Complexity) and memory requirement (Space Complexity) as input size $n \to \infty$.

---

### 2. Time & Space Complexity
- **Time Complexity:** The number of fundamental operations executed by the algorithm as a function of input size $n$.
- **Space Complexity:** Total memory used = **Fixed Part** (instruction space, constants) + **Variable Part** (dynamic variables, recursion stack).

---

### 3. Asymptotic Notations (The "Big 5")
Asymptotic notations describe behavior for very large $n$ ($n \ge n_0$), ignoring machine hardware and constant multipliers.

```
       f(n)
        ^
        |           c2 * g(n)  [Upper bound - Big-O]
        |          /
        |         /   f(n)     [Exact order - Theta]
        |        /   /
        |       /   /
        |      /   /   c1 * g(n) [Lower bound - Omega]
        |     /   /   /
        |    /   /   /
        +---/---/---/---------> n
               n0
```

1. **Big-O Notation ($\mathcal{O}$): Asymptotic Upper Bound (Worst-case ceiling)**
   - **Math:** $0 \le f(n) \le c \cdot g(n)$ for all $n \ge n_0$, where $c > 0$ and $n_0 \ge 1$.
   - **Layman:** "Algorithm takes **at most** this much time."

2. **Big-Omega Notation ($\Omega$): Asymptotic Lower Bound (Best-case floor)**
   - **Math:** $0 \le c \cdot g(n) \le f(n)$ for all $n \ge n_0$, where $c > 0$ and $n_0 \ge 1$.
   - **Layman:** "Algorithm takes **at least** this much time."

3. **Big-Theta Notation ($\Theta$): Asymptotic Tight Bound**
   - **Math:** $0 \le c_1 \cdot g(n) \le f(n) \le c_2 \cdot g(n)$ for all $n \ge n_0$, where $c_1, c_2 > 0$ and $n_0 \ge 1$.
   - **Layman:** $f(n)$ grows at the exact same rate as $g(n)$. **$f(n) = \Theta(g(n)) \iff f(n) = O(g(n)) \text{ and } f(n) = \Omega(g(n))$.**

4. **Little-o ($o$): Strict Upper Bound**
   - **Math:** $f(n) < c \cdot g(n)$ for **all** $c > 0$ as $n \ge n_0$. Equivalently: $\lim_{n \to \infty} \frac{f(n)}{g(n)} = 0$.
   - *Example:* $2n = o(n^2)$, but $2n^2 \neq o(n^2)$.

5. **Little-omega ($\omega$): Strict Lower Bound**
   - **Math:** $\lim_{n \to \infty} \frac{f(n)}{g(n)} = \infty$.

---

### 4. Solving Recurrence Relations (Exam Favorite!)

#### Technique 1: Master Theorem (Statement Only - Must Memorize)
For recurrences of the form:
$$T(n) = aT\left(\frac{n}{b}\right) + f(n)$$
where $a \ge 1$, $b > 1$, and $f(n) = \Theta(n^k \log^p n)$.

Let $c_{crit} = \log_b a$. Compare $f(n)$ with $n^{\log_b a}$:
- **Case 1:** If $f(n) = O(n^{\log_b a - \epsilon})$ for some $\epsilon > 0$ (i.e., $n^{\log_b a}$ dominates):
  $$\mathbf{T(n) = \Theta(n^{\log_b a})}$$
- **Case 2:** If $f(n) = \Theta(n^{\log_b a} \log^k n)$ where $k \ge 0$:
  $$\mathbf{T(n) = \Theta(n^{\log_b a} \log^{k+1} n)}$$
  *(For standard $f(n) = \Theta(n^{\log_b a})$, $T(n) = \Theta(n^{\log_b a} \log n)$).*
- **Case 3:** If $f(n) = \Omega(n^{\log_b a + \epsilon})$ for some $\epsilon > 0$ (i.e., $f(n)$ dominates) AND regularity condition holds ($a f(n/b) \le c f(n)$ for $c < 1$):
  $$\mathbf{T(n) = \Theta(f(n))}$$

> **Exam Shortcut Tip:**
> - $T(n) = 2T(n/2) + n \implies a=2, b=2 \implies n^{\log_2 2} = n^1$. Since $f(n) = n$, Case 2 applies $\implies \Theta(n \log n)$ (Merge Sort).
> - $T(n) = 8T(n/2) + n^2 \implies n^{\log_2 8} = n^3$. Since $n^3 > n^2$, Case 1 applies $\implies \Theta(n^3)$.
> - $T(n) = 2T(n/2) + n^2 \implies n^{\log_2 2} = n^1$. Since $n^2 > n^1$, Case 3 applies $\implies \Theta(n^2)$.

#### Technique 2: Substitution Method
1. **Guess** the form of the solution (e.g., $T(n) \le c \cdot n \log n$).
2. Use **Mathematical Induction** to find the constant $c$ and verify that the base case and inductive step hold.

#### Technique 3: Recurrence Tree Method
1. Draw the tree where each node represents the cost of $f(n)$ at that recursive call.
2. Sum the cost across each level.
3. Determine total depth (tree height = $\log_b n$).
4. Sum costs over all levels $\sum_{i=0}^{\text{height}} (\text{level cost})$.

---

# 🛠️ Module 2: Algorithm Design Techniques

### 1. Brute Force: Traveling Salesman Problem (TSP)
- **Problem:** Given $n$ cities with pairwise distances, find the tour visiting every city exactly once and returning to the start with minimum total cost.
- **Brute Force Approach:** Generate all $(n-1)!$ possible permutations of tours, evaluate costs, and pick the minimum.
- **Complexity:** Time $O(n!)$, Space $O(n)$. Impractical for $n > 20$.

---

### 2. Divide and Conquer: Strassen’s Matrix Multiplication
- **Standard Matrix Mult:** For two $n \times n$ matrices:
  $$C_{ij} = \sum_{k=1}^n A_{ik} B_{kj} \implies 8 \text{ recursive multiplications for } n/2 \text{ blocks} \implies T(n) = 8T(n/2) + \Theta(n^2) = \Theta(n^3)$$
- **Strassen's Breakthrough:** Computes 7 matrix products ($P_1$ to $P_7$) using clever additions/subtractions instead of 8:
  $$T(n) = 7T(n/2) + \Theta(n^2)$$
  By Master Theorem: $\log_2 7 \approx 2.807 \implies \mathbf{T(n) = \Theta(n^{2.81})}$.
- **Exam Steps to remember:** Divide into 4 sub-blocks of size $n/2 \times n/2$, create 7 intermediate products, combine to form $C_{11}, C_{12}, C_{21}, C_{22}$.

---

### 3. Greedy Algorithms
> **Greedy Choice Property:** A globally optimal solution can be arrived at by making locally optimal (greedy) choices at each step.  
> **Optimal Substructure:** Optimal solution to problem contains optimal solutions to subproblems.

#### A. Fractional Knapsack Problem
- **Rules:** Items have weights $w_i$, values $v_i$. Capacity $W$. Items **CAN be broken into fractions**.
- **Algorithm:**
  1. Compute ratio $r_i = \frac{v_i}{w_i}$ for each item.
  2. Sort items descending by $r_i$.
  3. Greedily take as much of the highest ratio item as fits. If capacity left $< w_i$, take fraction $\frac{\text{capacity left}}{w_i}$.
- **Time Complexity:** $O(n \log n)$ (due to sorting).

#### B. Job Sequencing with Deadlines
- **Problem:** $n$ jobs, each with deadline $d_i \ge 1$ and profit $p_i$. Each job takes 1 unit of time. Maximize total profit.
- **Algorithm:**
  1. Sort all jobs descending by profit $p_i$.
  2. Find the latest available free time slot $\le d_i$.
  3. Assign the job to that slot; if no slot is free, discard it.
- **Time:** $O(n^2)$ naive, or $O(n \log n)$ using Disjoint Set Union (DSU).

#### C. Minimum Spanning Tree (MST): Prim’s vs. Kruskal’s

| Feature | Prim’s Algorithm | Kruskal’s Algorithm |
|---|---|---|
| **Strategy** | Grows a single connected tree vertex by vertex | Adds smallest edge globally across whole graph |
| **Data Structure** | Priority Queue / Min-Heap | Disjoint Set Union (DSU / Find-Union) |
| **Graph Preference** | Preferred for **Dense Graphs** ($E \approx V^2$) | Preferred for **Sparse Graphs** ($E \ll V^2$) |
| **Cycle Check** | Inherently avoids cycles by picking cut-edges | Needs Union-Find cycle detection |
| **Complexity** | $O(E \log V)$ with Heap, $O(V^2)$ with Matrix | $O(E \log E) = O(E \log V)$ |

#### D. Huffman Coding (Data Compression)
- **Concept:** Variable-length prefix codes based on character frequencies (frequent characters get shorter codes; rare ones get longer).
- **Prefix Rule:** No code is a prefix of another code $\implies$ unambiguous decoding.
- **Algorithm:**
  1. Put all characters into a Min-Priority Queue by frequency.
  2. Extract two minimum nodes $L$ and $R$.
  3. Create parent with frequency $f_L + f_R$, make $L$ and $R$ its children, push back to queue.
  4. Repeat until 1 node remains (root). Assign '0' to left branches, '1' to right branches.
- **Time:** $O(n \log n)$.

#### E. Graph Coloring (Greedy Heuristic)
- **Problem:** Color vertices such that no two adjacent vertices share the same color using minimum colors ($\chi(G)$ chromatic number).
- **Greedy heuristic:** Order vertices $v_1, \dots, v_n$. For vertex $v_i$, assign the smallest available color not used by any of its colored neighbors.
- *Note:* Greedy is not guaranteed to find the true chromatic number $\chi(G)$ (NP-Hard problem), but gives a valid coloring in $O(V^2 + E)$.

---

### 4. Dynamic Programming (DP)
> **Key Properties of DP:**  
> 1. **Overlapping Subproblems:** Same subproblems solved repeatedly (cached in table).  
> 2. **Optimal Substructure:** Optimal solution constructed from optimal sub-solutions.

#### A. 0/1 Knapsack Problem
- **Rule:** Items **CANNOT** be broken (either take 0 or 1). Greedy method fails here!
- **Recurrence:**
  Let $dp[i][w]$ be max value using a subset of first $i$ items with capacity $w$:
  $$dp[i][w] = \begin{cases} dp[i-1][w] & \text{if } w_i > w \\ \max\Big(dp[i-1][w], \; v_i + dp[i-1][w - w_i]\Big) & \text{if } w_i \le w \end{cases}$$
- **Base Case:** $dp[0][w] = 0$ and $dp[i][0] = 0$.
- **Complexity:** Time $O(n \cdot W)$, Space $O(n \cdot W)$ or $O(W)$. (Pseudo-polynomial time).

#### B. Matrix Chain Multiplication (MCM)
- **Problem:** Given matrices $A_1, A_2, \dots, A_n$ where $A_i$ has dimension $p_{i-1} \times p_i$. Find optimal parenthesization to minimize scalar multiplications.
- **Recurrence:**
  Let $m[i,j]$ be min multiplications to compute $A_i \dots A_j$:
  $$m[i,j] = \begin{cases} 0 & \text{if } i = j \\ \min_{i \le k < j} \Big( m[i,k] + m[k+1,j] + p_{i-1} \cdot p_k \cdot p_j \Big) & \text{if } i < j \end{cases}$$
- **Time Complexity:** $O(n^3)$, Space $O(n^2)$. Table filled diagonal by diagonal (chain lengths $L = 2$ to $n$).

#### C. Traveling Salesman Problem (Held-Karp DP)
- **Recurrence:** Let $C(S, j)$ be minimum cost path starting at vertex 1, visiting every vertex in set $S$ exactly once, ending at vertex $j$:
  $$C(S, j) = \min_{i \in S, i \neq j} \Big( C(S \setminus \{j\}, i) + \text{cost}(i, j) \Big)$$
- **Base Case:** $C(\{1\}, 1) = 0$.
- **Complexity:** Time $O(n^2 \cdot 2^n)$, Space $O(n \cdot 2^n)$. (Massive improvement over $O(n!)$ brute force).

---

### 5. Backtracking
> **Philosophy:** Systematic search of the state space tree with **pruning** (bounding function abandons non-promising paths early).

#### A. N-Queens Problem
- **Problem:** Place $N$ queens on an $N \times N$ chessboard such that no two queens attack each other (same row, column, or diagonal).
- **Condition for 2 queens at $(i, x[i])$ and $(k, x[k])$:**
  - Same column: $x[i] == x[k]$
  - Same diagonal: $|x[i] - x[k]| == |i - k|$
- **Algorithm:** Place queen row by row. At row $k$, try columns $1 \dots N$. If safe, recurse to $k+1$. If no column is safe, backtrack to $k-1$.

#### B. Subset Sum Problem
- **Problem:** Given set of positive integers $S = \{w_1, w_2, \dots, w_n\}$ and target sum $M$, find all subsets summing exactly to $M$.
- **Bounding Functions (Pruning criteria):**
  1. If current sum $+ w_{k+1} > M \implies$ prune right branch.
  2. If current sum $+ \sum_{i=k+1}^n w_i < M \implies$ even taking all remaining items cannot reach $M$, prune!

---

# 🔍 Module 3: String Matching Algorithms

### 1. Naive String Matching Algorithm
- **Idea:** Slide pattern $P[1 \dots m]$ over text $T[1 \dots n]$ for each possible shift $s \in [0, n-m]$.
- Check character-by-character from left to right.
- **Worst-case Time:** $O((n - m + 1) \cdot m) \approx O(n \cdot m)$ (e.g., $T = \text{"AAAAAAAAAB"}$, $P = \text{"AAB"}$).
- **Best-case Time:** $O(n)$ when first character mismatches immediately.
- **Space:** $O(1)$.

---

### 2. Knuth-Morris-Pratt (KMP) Algorithm
- **Key Insight:** When a mismatch occurs, the pattern itself holds enough information to know how far forward we can safely slide without re-examining text characters. Text pointer $i$ **NEVER decrements / backtracks**.

#### The $\pi$ Array (LPS - Longest Proper Prefix which is also Suffix):
- $\pi[q] = \text{length of the longest proper prefix of } P[1 \dots q] \text{ that is also a suffix of } P[1 \dots q]$.

#### Example Calculation for Exam:
Pattern: `A B A B A C A`
- $\pi[1]$ (`A`): 0
- $\pi[2]$ (`AB`): 0
- $\pi[3]$ (`ABA`): 1 (`A`)
- $\pi[4]$ (`ABAB`): 2 (`AB`)
- $\pi[5]$ (`ABABA`): 3 (`ABA`)
- $\pi[6]$ (`ABABAC`): 0
- $\pi[7]$ (`ABABACA`): 1 (`A`)
$\implies \pi = [0, 0, 1, 2, 3, 0, 1]$.

#### Complexities:
- Preprocessing ($\pi$ table): $\Theta(m)$ time and space.
- Matching Phase: $\Theta(n)$ time.
- **Total Time Complexity:** $\Theta(n + m)$.
- **Space Complexity:** $\Theta(m)$.

---

# 🌐 Module 4: Graph Algorithms & Network Flows

### 1. Shortest Path Algorithms Comparison

| Algorithm | Type | Edge Weight Restriction | Time Complexity | Core Logic / Recurrence |
|---|---|---|---|---|
| **Dijkstra** | Single-Source Shortest Path (SSSP) | **Non-negative weights only** ($\ge 0$) | $O((V+E)\log V)$ | Greedy; picks vertex $u$ with min $d[u]$, relaxes edges $(u,v)$ |
| **Bellman-Ford** | SSSP | Can handle **negative weights** | $O(V \cdot E)$ | Relaxes all $E$ edges $(V-1)$ times; $V$-th iteration detects negative cycle |
| **Floyd-Warshall** | All-Pairs Shortest Path (APSP) | Can handle negative weights, **no negative cycles** | $O(V^3)$ | DP: $d^{(k)}[i,j] = \min(d^{(k-1)}[i,j], d^{(k-1)}[i,k] + d^{(k-1)}[k,j])$ |

> **Exam Warning on Dijkstra vs Bellman-Ford:**
> Why does Dijkstra fail on negative edge weights?
> Because Dijkstra assumes that once a vertex is marked "visited" (its distance is finalized), no shorter path to it can ever be found. A negative edge later in the graph violates this greedy assumption! Bellman-Ford relaxes all edges $(V-1)$ times, allowing distances to decrease.

---

### 2. Network Flows & Maximum Flow

#### Key Concepts & Definitions:
- **Flow Network:** Directed graph $G=(V, E)$ with source $s$, sink $t$, and edge capacities $c(u,v) \ge 0$.
- **Capacity Constraint:** For every edge: $0 \le f(u,v) \le c(u,v)$.
- **Flow Conservation:** For every vertex $u \notin \{s, t\}$: $\sum f(v, u) = \sum f(u, w)$ (Total Flow In = Total Flow Out).
- **Residual Capacity:** $c_f(u,v) = c(u,v) - f(u,v)$. Residual backward edge allows "canceling" flow: $c_f(v,u) = f(u,v)$.
- **Augmenting Path:** A simple directed path from $s$ to $t$ in the residual network $G_f$ with capacity $> 0$.

#### A. Ford-Fulkerson Method
1. Initialize flow $f(u,v) = 0$ everywhere.
2. While there exists an augmenting path $p$ from $s$ to $t$ in $G_f$:
   - Find bottleneck capacity: $c_f(p) = \min_{(u,v) \in p} c_f(u,v)$.
   - Augment flow along $p$ by $c_f(p)$ (increase forward edges, decrease backward edges).
3. Return total flow.
- **Max-Flow Min-Cut Theorem:** The value of maximum flow equals the capacity of the minimum $s-t$ cut.
- **Complexity:** $O(E \cdot |f^*|)$ where $|f^*|$ is max flow value. If BFS is used to choose shortest augmenting path (**Edmonds-Karp algorithm**), time drops to $O(V \cdot E^2)$.

#### B. Push-Relabel (Preflow-Push) Algorithm
- Unlike Ford-Fulkerson (which maintains flow conservation at all intermediate steps), Push-Relabel works locally:
  - Permits **Preflow**: vertices can have excess flow $e(u) = \text{inflow} - \text{outflow} \ge 0$.
  - Assigns each vertex a **height (label)** $h(u)$. Initialized with $h(s) = |V|$, $h(t) = 0$, others 0.
- **Two Basic Operations:**
  1. **Push$(u, v)$:** If $u$ has excess flow ($e(u) > 0$), $c_f(u,v) > 0$, and $h(u) = h(v) + 1$, push $\min(e(u), c_f(u,v))$ units downhill.
  2. **Relabel$(u)$:** If $e(u) > 0$ and cannot push to any neighbor ($h(u) \le h(v)$ for all residual neighbors), increment height: $h(u) = 1 + \min \{h(v) : (u,v) \in E_f\}$.
- **Complexity:** Generic: $O(V^2 E)$; with FIFO queue: $O(V^3)$. Much faster than Ford-Fulkerson on dense graphs!

---

# 🧠 Module 5: Complexity Classes, P, NP, and Beyond

### 1. Fundamental Complexity Classes

```
+-------------------------------------------------------------+
|                      NP-Hard                                |
|        +----------------------------------------+           |
|        |               NP                       |           |
|        |   +--------------------+               |           |
|        |   |         P          |  NP-Complete  |           |
|        |   |  (Poly-time        |  (Hardest in  |           |
|        |   |   solvable)        |   NP)         |           |
|        |   +--------------------+               |           |
|        +----------------------------------------+           |
+-------------------------------------------------------------+
               (Assuming P != NP)
```

- **Class P (Polynomial Time):** Decision problems that can be **solved** by a Deterministic Turing Machine (standard computer) in polynomial time ($O(n^k)$).
  - *Examples:* Shortest path, MST, Sorting, Matrix multiplication.
- **Class NP (Nondeterministic Polynomial Time):** Decision problems whose proposed solution (certificate) can be **verified** in polynomial time.
  - Every problem in P is in NP ($P \subseteq NP$). Whether $P = NP$ remains an open Millennium Prize Problem.
- **NP-Hard:** A problem $X$ is NP-Hard if every problem $Y \in NP$ can be polynomial-time reduced to $X$ ($Y \le_P X$). $X$ does *not* need to be in NP (it may not even be a decision problem).
- **NP-Complete (NPC):** A problem $X$ is NP-Complete if:
  1. $X \in NP$ (verifiable in polynomial time).
  2. $X$ is NP-Hard (every problem in NP reduces to $X$).
  - **Significance:** If *any single* NP-Complete problem is solved in polynomial time, then **all** problems in NP can be solved in polynomial time ($P = NP$).

---

### 2. Polynomial Reduction & Proving NP-Completeness
To prove a new problem $B$ is NP-Complete:
1. Prove $B \in NP$ (show a certificate can be checked in $O(n^k)$).
2. Choose a known NP-Complete problem $A$ (e.g., 3-SAT).
3. Construct a function $f$ mapping any instance of $A$ to an instance of $B$ in polynomial time ($A \le_P B$).
4. Prove: $A$ is true $\iff B$ is true.

#### The Core Reductions Chain:
- **Cook-Levin Theorem:** SAT (Boolean Satisfiability) is NP-Complete (First proven NP-Complete problem).
- **SAT $\le_P$ 3-SAT:** Convert arbitrary Boolean CNF formulas into clauses with exactly 3 literals.
- **3-SAT $\le_P$ Independent Set / Clique / Vertex Cover**.

---

### 3. Key Classic NP-Complete Problems
1. **SAT (Satisfiability):** Given a Boolean formula in CNF, is there a truth assignment to variables making the formula TRUE?
2. **3-SAT:** Each clause has exactly 3 literals (e.g., $(x_1 \lor \neg x_2 \lor x_3) \land (\dots)$).
3. **Vertex Cover:** Given graph $G=(V, E)$ and integer $k$, does there exist a subset $V' \subseteq V$ of size $|V'| \le k$ such that every edge in $E$ has at least one endpoint in $V'$?
4. **Independent Set:** A set of vertices $S \subseteq V$ such that no two vertices in $S$ are adjacent. Does there exist an independent set of size $\ge k$?
   - *Relationship:* $S$ is an Independent Set $\iff (V \setminus S)$ is a Vertex Cover.
5. **Clique Problem:** A subset of vertices where every pair is connected by an edge (complete subgraph). Does a clique of size $\ge k$ exist?
   - *Relationship:* $G$ has a Clique of size $k \iff$ Complement graph $\overline{G}$ has an Independent Set of size $k$.

---

### 4. Approximation & Randomized Algorithms
- **Approximation Algorithms:**
  - Used when finding exact optimal solution is NP-Hard. Runs in polynomial time and returns a solution guaranteed to be within a factor $\alpha$ of the optimal solution.
  - **Approximation Ratio:** $\max\left(\frac{C}{C^*}, \frac{C^*}{C}\right) \le \alpha$.
  - *Example: 2-Approximation for Vertex Cover:* Repeatedly pick an arbitrary edge $(u,v)$, add both $u$ and $v$ to the cover, and remove all incident edges. Produces a cover of size at most $2 \times$ optimal!
- **Randomized Algorithms:**
  - Uses random bits in its logic.
  - **Las Vegas Algorithms:** Always produces the correct answer; running time is a random variable (e.g., Randomized QuickSort - always sorts correctly, expected $O(n \log n)$).
  - **Monte Carlo Algorithms:** Runs in fixed time, but output has a small probability of error (e.g., Miller-Rabin Primality Test, Karger’s Min-Cut).

---

# 📝 Important Semester Questions Bank
*(Curated high-probability questions with model answers and answering frameworks)*

### Part A: High-Yield Short Questions (2 Marks Each)

#### Q1. Define Asymptotic notations $\Theta$, $\mathcal{O}$, and $\Omega$.
**Ans:**
- $\mathcal{O}(g(n)) = \{f(n) : \exists c > 0, n_0 \ge 1 \text{ s.t. } 0 \le f(n) \le c \cdot g(n), \forall n \ge n_0\}$ (Upper bound).
- $\Omega(g(n)) = \{f(n) : \exists c > 0, n_0 \ge 1 \text{ s.t. } 0 \le c \cdot g(n) \le f(n), \forall n \ge n_0\}$ (Lower bound).
- $\Theta(g(n)) = \{f(n) : \exists c_1, c_2 > 0, n_0 \ge 1 \text{ s.t. } 0 \le c_1 g(n) \le f(n) \le c_2 g(n), \forall n \ge n_0\}$ (Tight bound).

#### Q2. Why does Dijkstra's algorithm fail for negative edge weights?
**Ans:** Dijkstra is greedy: once a vertex is marked visited (extracted from the priority queue), its shortest distance is finalized and never re-evaluated. If an unseen negative edge exists later in the path, it could reduce the distance to an already finalized vertex, which Dijkstra misses.

#### Q3. What is the difference between Greedy and Dynamic Programming?
**Ans:**
- **Greedy:** Makes the best choice locally at each step without looking back or recalculating past decisions. Cannot guarantee optimality for problems like 0/1 Knapsack.
- **DP:** Solves all subproblems, caches intermediate results in a table, and makes decisions by exploring all overlapping subproblem choices.

#### Q4. What is a prefix-free code in Huffman coding?
**Ans:** A code where no assigned bitstring is a prefix of any other character's bitstring. This allows unambiguous decoding from left to right without any delimiters.

#### Q5. State the Max-Flow Min-Cut Theorem.
**Ans:** In any network flow graph, the maximum amount of flow passing from source $s$ to sink $t$ is strictly equal to the minimum capacity among all cuts separating $s$ and $t$.

#### Q6. What is the relation between P, NP, and NP-Complete?
**Ans:**
- $P \subseteq NP$.
- NP-Complete problems are the hardest problems in $NP$. A problem $X$ is NP-Complete if $X \in NP$ and for every $Y \in NP$, $Y \le_P X$. If any NPC problem is solved in polynomial time, then $P = NP$.

---

### Part B: Medium Numerical & Algorithm Questions (5 Marks Each)

#### Q7. Solve the recurrence relation using Master Theorem:
1. $T(n) = 4T(n/2) + n$
2. $T(n) = 2T(n/2) + n \log n$
3. $T(n) = 7T(n/2) + \Theta(n^2)$

**Model Solution:**
1. $a = 4, b = 2 \implies n^{\log_b a} = n^{\log_2 4} = n^2$.
   $f(n) = n = O(n^{2 - 1}) \implies$ **Case 1 of Master Theorem** applies:
   $$T(n) = \Theta(n^2)$$
2. $a = 2, b = 2 \implies n^{\log_2 2} = n^1$.
   $f(n) = n \log n = \Theta(n^1 \log^1 n) \implies$ **Case 2 of Master Theorem** with $k = 1$:
   $$T(n) = \Theta(n \log^{1+1} n) = \Theta(n \log^2 n)$$
3. $a = 7, b = 2 \implies n^{\log_2 7} \approx n^{2.807}$.
   $f(n) = n^2 = O(n^{2.807 - \epsilon}) \implies$ **Case 1** applies:
   $$T(n) = \Theta(n^{\log_2 7}) \approx \Theta(n^{2.81})$$

#### Q8. Compute the $\pi$ (LPS) table for pattern `P = "AABAACAABAA"` in KMP algorithm.
**Model Solution:**
Let indices be 1 to 11:
- `i = 1`: `A` $\implies \pi[1] = 0$
- `i = 2`: `AA` $\implies$ Prefix `A` = Suffix `A` $\implies \pi[2] = 1$
- `i = 3`: `AAB` $\implies \pi[3] = 0$
- `i = 4`: `AABA` $\implies$ Prefix `A` = Suffix `A` $\implies \pi[4] = 1$
- `i = 5`: `AABAA` $\implies$ Prefix `AA` = Suffix `AA` $\implies \pi[5] = 2$
- `i = 6`: `AABAAC` $\implies \pi[6] = 0$
- `i = 7`: `AABAACA` $\implies \pi[7] = 1$
- `i = 8`: `AABAACAA` $\implies \pi[8] = 2$
- `i = 9`: `AABAACAAB` $\implies \pi[9] = 3$ (`AAB`)
- `i = 10`: `AABAACAABA` $\implies \pi[10] = 4$ (`AABA`)
- `i = 11`: `AABAACAABAA` $\implies \pi[11] = 5$ (`AABAA`)

**Final Table:**
| Index | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 |
|---|---|---|---|---|---|---|---|---|---|---|---|
| **Char** | A | A | B | A | A | C | A | A | B | A | A |
| **$\pi$** | 0 | 1 | 0 | 1 | 2 | 0 | 1 | 2 | 3 | 4 | 5 |

#### Q9. Solve 0/1 Knapsack for $W = 5$ with items:
- Item 1: $w_1 = 2, v_1 = 12$
- Item 2: $w_2 = 1, v_2 = 10$
- Item 3: $w_3 = 3, v_3 = 20$
- Item 4: $w_4 = 2, v_4 = 15$

**Model Solution Table ($dp[i][w]$):**
Using $dp[i][w] = \max(dp[i-1][w], v_i + dp[i-1][w - w_i])$:

| Item $i$ | $(w_i, v_i)$ | $w=0$ | $w=1$ | $w=2$ | $w=3$ | $w=4$ | $w=5$ |
|---|---|---|---|---|---|---|---|
| **0** | - | 0 | 0 | 0 | 0 | 0 | 0 |
| **1** | $(2, 12)$ | 0 | 0 | 12 | 12 | 12 | 12 |
| **2** | $(1, 10)$ | 0 | 10 | 12 | 22 | 22 | 22 |
| **3** | $(3, 20)$ | 0 | 10 | 12 | 22 | 30 | 32 |
| **4** | $(2, 15)$ | 0 | 10 | 15 | 25 | 30 | **37** |

- Traceback for $w=5$:
  - $dp[4][5] = 37 \neq dp[3][5] (32) \implies$ **Item 4 included** ($w = 5 - 2 = 3$).
  - $dp[3][3] = 22 = dp[2][3] (22) \implies$ Item 3 not included.
  - $dp[2][3] = 22 \neq dp[1][3] (12) \implies$ **Item 2 included** ($w = 3 - 1 = 2$).
  - $dp[1][2] = 12 \neq dp[0][2] (0) \implies$ **Item 1 included** ($w = 2 - 2 = 0$).
- **Max Profit = 37** (Items chosen: {1, 2, 4}, Total Weight = $2+1+2 = 5$).

---

### Part C: High-Mark Detailed Questions (10 Marks Each)

#### Q10. Explain Matrix Chain Multiplication (MCM) with dynamic programming. Given matrices with dimensions $\langle 10, 100, 5, 50 \rangle$, find the minimum scalar multiplications and optimal parenthesization.

**Answer Blueprint:**
1. **Define the problem:** Given chain of matrices $A_1, A_2, \dots, A_n$ where $A_i$ is $p_{i-1} \times p_i$.
2. **Derive DP recurrence:**
   $$m[i,j] = \min_{i \le k < j} \{m[i,k] + m[k+1,j] + p_{i-1} p_k p_j\}, \quad m[i,i] = 0$$
3. **Step-by-step calculation for dimensions $\langle 10, 100, 5, 50 \rangle$:**
   $n = 3 \implies A_1 (10 \times 100), A_2 (100 \times 5), A_3 (5 \times 50)$.
   - Length $L=1$: $m[1,1] = m[2,2] = m[3,3] = 0$.
   - Length $L=2$:
     - $m[1,2] = m[1,1] + m[2,2] + p_0 p_1 p_2 = 0 + 0 + 10 \times 100 \times 5 = \mathbf{5,000}$. ($k=1$)
     - $m[2,3] = m[2,2] + m[3,3] + p_1 p_2 p_3 = 0 + 0 + 100 \times 5 \times 50 = \mathbf{25,000}$. ($k=2$)
   - Length $L=3$: $m[1,3]$:
     - For $k=1$: $m[1,1] + m[2,3] + p_0 p_1 p_3 = 0 + 25,000 + (10 \times 100 \times 50) = 25,000 + 50,000 = 75,000$.
     - For $k=2$: $m[1,2] + m[3,3] + p_0 p_2 p_3 = 5,000 + 0 + (10 \times 5 \times 50) = 5,000 + 2,500 = \mathbf{7,500}$.
   - Minimum is $\mathbf{7,500}$ at split point $k=2$.
4. **Optimal Parenthesization:**
   $k=2$ splits as $(A_1 A_2) A_3$.
5. **Conclusion:** Minimum scalar multiplications = **7,500**. Time complexity = $O(n^3)$, Space = $O(n^2)$.

---

#### Q11. Explain Bellman-Ford Algorithm with pseudo-code and trace an example. Why is it able to detect negative weight cycles?

**Answer Blueprint:**
1. **Purpose:** Single-source shortest path that works with negative edge weights and flags negative weight cycles.
2. **Pseudo-code:**
   ```
   BellmanFord(G, w, s):
       Initialize dist[v] = infinity for all v; dist[s] = 0
       // Relax edges |V| - 1 times
       for i = 1 to |V| - 1:
           for each edge (u, v) in E:
               if dist[u] + w(u, v) < dist[v]:
                   dist[v] = dist[u] + w(u, v)

       // Check for negative-weight cycles
       for each edge (u, v) in E:
           if dist[u] + w(u, v) < dist[v]:
               return "Graph contains negative weight cycle"
       return dist
   ```
3. **Why $|V|-1$ passes?** A simple shortest path in a graph with $|V|$ vertices can contain at most $|V|-1$ edges. In each iteration, at least one edge along the true shortest path tree is correctly relaxed.
4. **Why the $V$-th check detects negative cycles?** If an edge can still be relaxed after $|V|-1$ iterations, it means the path length can decrease indefinitely by looping through that cycle $\implies$ a negative weight cycle exists!
5. **Time Complexity:** $O(V \cdot E)$. Space: $O(V)$.

---

#### Q12. Explain Ford-Fulkerson Algorithm and prove the Max-Flow Min-Cut Theorem.

**Answer Blueprint:**
1. **Definitions:**
   - Cut $(S, T)$: A partition of vertices into $S$ and $T = V \setminus S$ such that $s \in S$ and $t \in T$.
   - Capacity of Cut: $c(S, T) = \sum_{u \in S, v \in T} c(u, v)$.
   - Net flow across Cut: $f(S, T) = \sum_{u \in S, v \in T} f(u, v) - \sum_{u \in T, v \in S} f(u, v) = |f|$.
2. **Weak Duality:** For any flow $f$ and any cut $(S, T)$, $|f| \le c(S, T)$. (Flow can never exceed cut capacity).
3. **Equivalence Theorem (3 statements are equivalent):**
   1. $f$ is a maximum flow in $G$.
   2. The residual network $G_f$ contains no augmenting path from $s$ to $t$.
   3. $|f| = c(S, T)$ for some cut $(S, T)$.
4. **Proof of $(2) \implies (3)$:**
   - Define $S = \{v \in V : \text{there is a path from } s \text{ to } v \text{ in } G_f\}$.
   - Define $T = V \setminus S$. Since no path reaches $t$, $t \in T$.
   - For any edge $(u,v)$ with $u \in S, v \in T$: $f(u,v) = c(u,v)$ (must be saturated, otherwise $v$ would be reachable in $G_f$).
   - For any edge $(v,u)$ with $v \in T, u \in S$: $f(v,u) = 0$ (otherwise residual edge $(u,v)$ with capacity $f(v,u) > 0$ would exist, making $v \in S$).
   - Hence, $|f| = f(S, T) = \sum c(u, v) = c(S, T)$.
5. Since $|f| \le c(S', T')$ for all cuts, and $|f| = c(S, T)$, $|f|$ is maximum and $c(S,T)$ is minimum. $\square$

---

#### Q13. Prove that the Vertex Cover Problem is NP-Complete by reducing from 3-SAT (or Clique).

**Answer Blueprint:**
1. **Show Vertex Cover $\in NP$:**
   - A certificate is a subset of vertices $V' \subseteq V$ of size $k$.
   - Verifier checks:
     1. $|V'| \le k$ (takes $O(|V|)$).
     2. For every edge $(u, v) \in E$, at least one of $u \in V'$ or $v \in V'$ (takes $O(|E|)$).
   - Verification runs in polynomial time $O(V + E) \implies \text{Vertex Cover} \in NP$.
2. **Reduction from 3-SAT ($\text{3-SAT} \le_P \text{Vertex Cover}$):**
   - Given a 3-SAT formula with $n$ variables $x_1, \dots, x_n$ and $m$ clauses $C_1, \dots, C_m$.
   - **Construct Graph $G$:**
     - **Variable gadgets:** For each variable $x_i$, create 2 connected vertices $(x_i, \neg x_i)$. An edge connects them. To cover this edge, any vertex cover must pick at least 1 vertex ($n$ vertices total).
     - **Clause gadgets:** For each clause $C_j = (l_1 \lor l_2 \lor l_3)$, create a triangle of 3 vertices. To cover the 3 edges of the triangle, any vertex cover must pick at least 2 vertices ($2m$ vertices total).
     - **Connecting edges:** Connect each literal in the clause triangle to its corresponding literal in the variable gadget.
   - Set target size $k = n + 2m$.
3. **Proof of Equivalence:**
   - *If 3-SAT is satisfiable:* Pick the true literal in each variable gadget ($n$ vertices). In each clause gadget, since at least one literal is satisfied, pick the other 2 vertices ($2m$ vertices). Total size $= n + 2m = k$, and all edges are covered.
   - *If graph has vertex cover of size $\le n + 2m$:* It must contain exactly 1 vertex from each variable gadget and 2 from each clause triangle. Setting variable values according to chosen variable vertices satisfies all clauses.
4. **Conclusion:** Construction takes polynomial time $\implies$ Vertex Cover is NP-Complete. $\square$

---

# 📅 30-Day Semester Exam Study Schedule

| Week | Target Focus | Key Deliverables & Study Goals |
|---|---|---|
| **Week 1** | **Module 1 & 3** | Master Big-O/$\Omega$/$\Theta$ definitions, solve 15 Master Theorem recurrences, calculate KMP $\pi$-table 5 times by hand. |
| **Week 2** | **Module 2 (Greedy & D&C)** | Fractional Knapsack vs Job Sequencing, Prim's vs Kruskal's, Huffman coding tree construction, Strassen's matrix mult intuition. |
| **Week 3** | **Module 2 (DP & Backtracking)** | Matrix Chain Multiplication ($m[i,j]$ table), 0/1 Knapsack table filling, N-Queens state space tree, Backtracking bounding functions. |
| **Week 4** | **Module 4 & 5** | Dijkstra vs Bellman-Ford, Floyd-Warshall trace, Ford-Fulkerson Max-Flow/Min-Cut proof, P vs NP vs NP-Complete definitions, Vertex Cover reduction. |
| **Last 2 Days** | **Final Mock & Cheat Sheet** | Revise Master Complexity Table, memorize definitions and standard recurrence cases, practice 1 full 3-hour previous year paper. |
