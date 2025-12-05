AGA (Algorithms, Game Theory & Applications) — Major Exam: Detailed Concepts
══════════════════════════════════════════════════════════════════════════════

This document covers core topics from the course syllabus with definitions,
algorithms, complexity, important cases, and fully worked examples with diagrams.
Use this for revision and as a reference during problem solving.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
1) DETERMINISTIC MEDIAN FINDING (Selection / BFPRT / Median-of-Medians)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROBLEM STATEMENT
─────────────────
Given an unsorted array A of n distinct elements, find the k-th smallest element.
Special case: Median is k = ⌈n/2⌉.

WHY NOT SORT?
─────────────
Sorting takes O(n log n). Can we do better? Yes — O(n) worst-case!

ALGORITHM: SELECT(A, k) — Median-of-Medians (BFPRT)
───────────────────────────────────────────────────
```
SELECT(A, k):
    1. If |A| ≤ 5: sort A, return A[k]
    
    2. Divide A into ⌈n/5⌉ groups of 5 elements each
    
    3. Sort each group and find its median
       → Let M = {m₁, m₂, ..., m_{⌈n/5⌉}} be the set of medians
    
    4. Recursively find pivot p = SELECT(M, ⌈|M|/2⌉)
       (This is the "median of medians")
    
    5. Partition A around p:
       L = {x ∈ A : x < p}
       E = {x ∈ A : x = p}
       R = {x ∈ A : x > p}
    
    6. If k ≤ |L|:        return SELECT(L, k)
       If k ≤ |L| + |E|:  return p
       Else:              return SELECT(R, k - |L| - |E|)
```

VISUAL: WHY PIVOT GUARANTEES GOOD SPLIT
───────────────────────────────────────
Consider n=25 elements divided into 5 groups:

    Group 1    Group 2    Group 3    Group 4    Group 5
    ┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐    ┌─────┐
    │  1  │    │  6  │    │ 11  │    │ 16  │    │ 21  │  ← largest
    │  2  │    │  7  │    │ 12  │    │ 17  │    │ 22  │
    │ [3] │    │ [8] │    │[13] │    │[18] │    │[23] │  ← MEDIANS
    │  4  │    │  9  │    │ 14  │    │ 19  │    │ 24  │
    │  5  │    │ 10  │    │ 15  │    │ 20  │    │ 25  │  ← smallest
    └─────┘    └─────┘    └─────┘    └─────┘    └─────┘

Medians: {3, 8, 13, 18, 23}  →  Median-of-medians = 13

Elements GUARANTEED ≤ 13:
    - At least half of medians (3 medians: 3, 8, 13) are ≤ 13
    - Each such median has 2 elements smaller than it in its group
    - So at least 3 × 3 = 9 elements are ≤ 13

Elements GUARANTEED ≥ 13:
    - At least half of medians (3 medians: 13, 18, 23) are ≥ 13
    - Each such median has 2 elements larger than it in its group
    - So at least 3 × 3 = 9 elements are ≥ 13

    ┌───────────────────────────────────────────────────────┐
    │  General formula: At least 3⌈⌈n/5⌉/2⌉ ≈ 3n/10        │
    │  elements are on each side of the pivot!              │
    └───────────────────────────────────────────────────────┘

RECURRENCE AND COMPLEXITY
─────────────────────────
T(n) = T(n/5)     ← finding median of medians
     + T(7n/10)   ← worst-case recursion (at most 7n/10 elements remain)
     + O(n)       ← partitioning

Solving: T(n) = T(n/5) + T(7n/10) + cn
         n/5 + 7n/10 = 2n/10 + 7n/10 = 9n/10 < n  ✓

By substitution: T(n) ≤ 10cn = O(n)

COMPLETE WORKED EXAMPLE
───────────────────────
A = [47, 12, 93, 28, 61, 35, 84, 19, 56, 73, 42, 8, 67, 51, 39]
Find: 8th smallest (the median, since n=15)

Step 1: Divide into groups of 5
    Group 1: [47, 12, 93, 28, 61]
    Group 2: [35, 84, 19, 56, 73]
    Group 3: [42, 8, 67, 51, 39]

Step 2: Sort each group and find median
    Group 1 sorted: [12, 28, 47, 61, 93] → median = 47
    Group 2 sorted: [19, 35, 56, 73, 84] → median = 56
    Group 3 sorted: [8, 39, 42, 51, 67]  → median = 42

    Medians M = [47, 56, 42]

Step 3: Find median of medians (recursively, but small so sort)
    M sorted: [42, 47, 56] → median of medians p = 47

Step 4: Partition A around p = 47
    L = {12, 28, 35, 19, 42, 8, 39} = elements < 47  → |L| = 7
    E = {47}                        = elements = 47  → |E| = 1
    R = {93, 61, 84, 56, 73, 67, 51} = elements > 47 → |R| = 7

Step 5: Decide which partition to recurse into
    k = 8
    |L| = 7, so k > |L|
    |L| + |E| = 8, so k ≤ |L| + |E|
    
    ∴ Return p = 47  ✓

ANSWER: The 8th smallest element (median) is 47.

VERIFICATION: Sorted array = [8,12,19,28,35,39,42,47,51,56,61,67,73,84,93]
              Position 8 = 47 ✓

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
2) LINE SEGMENT INTERSECTION (Computational Geometry — Sweep Line)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROBLEM STATEMENT
─────────────────
Given n line segments in the plane, detect if any two intersect.
Extended: Report ALL intersection points.

NAIVE APPROACH
──────────────
Check all pairs: O(n²) — too slow for large n.

SWEEP LINE ALGORITHM (Bentley-Ottmann)
──────────────────────────────────────
Idea: Sweep a vertical line from left to right across the plane.
      Segments only need to be compared when they are "neighbors" on the sweep line.

Data Structures:
    1. Event Queue (priority queue by x-coordinate):
       - Left endpoints (segment starts)
       - Right endpoints (segment ends)
       - Intersection points (discovered during sweep)
    
    2. Status Structure (balanced BST ordered by y-coordinate):
       - Segments currently crossing the sweep line

```
SWEEP-LINE-INTERSECTION(S):
    Q ← empty priority queue (events)
    T ← empty balanced BST (status)
    
    For each segment s ∈ S:
        Insert left endpoint of s into Q
        Insert right endpoint of s into Q
    
    While Q is not empty:
        event ← ExtractMin(Q)
        
        CASE 1: Left endpoint of segment s
            Insert s into T
            Let a = predecessor of s in T
            Let b = successor of s in T
            Check(s, a) and Check(s, b) for intersection
            
        CASE 2: Right endpoint of segment s
            Let a = predecessor of s in T
            Let b = successor of s in T
            Delete s from T
            Check(a, b) for intersection
            
        CASE 3: Intersection of segments s₁ and s₂
            Report intersection
            Swap s₁ and s₂ in T
            Check new neighbors for intersections
```

VISUAL: SWEEP LINE IN ACTION
────────────────────────────

    y
    ↑
    │     s₂
    │    ╱  ╲
    │   ╱    ╲         s₄
    │  ╱      ╲       ╱
    │ ╱   ●    ╲     ╱       Legend:
    │╱  (int)   ╲   ╱        ─────────
    ├──────────────╳────→    │ = sweep line
    │╲           ╱  ╲        ● = intersection
    │ ╲    s₁   ╱    ╲       ╳ = intersection point
    │  ╲       ╱   s₃
    │   ╲     ╱
    │    ╲   ╱
    └─────────────────→ x
         ↑
       sweep line

Status at different x positions:
    x=1: T = {s₁}
    x=2: T = {s₂, s₁}      (s₂ above s₁)
    x=3: T = {s₁, s₂}      (after intersection, swap!)
    x=4: T = {s₁, s₂, s₃}

CHECKING FOR SEGMENT INTERSECTION
─────────────────────────────────
Two segments s₁: (p₁→q₁) and s₂: (p₂→q₂) intersect iff:
    orientation(p₁, q₁, p₂) ≠ orientation(p₁, q₁, q₂)  AND
    orientation(p₂, q₂, p₁) ≠ orientation(p₂, q₂, q₁)

Orientation test (using cross product):
```
orientation(A, B, C):
    val = (B.y - A.y)(C.x - B.x) - (B.x - A.x)(C.y - B.y)
    if val > 0: return "counterclockwise"
    if val < 0: return "clockwise"
    return "collinear"
```

COMPLETE WORKED EXAMPLE
───────────────────────
Segments:
    s₁: (1, 1) → (5, 5)
    s₂: (1, 5) → (5, 1)
    s₃: (3, 0) → (3, 6)

    y
    6 ┤         ╱ (s₃ top)
    5 ┤  s₂    ╱│
    4 ┤   ╲   ╱ │
    3 ┤    ╲ ●──│── intersections!
    2 ┤     ╳   │
    1 ┤  s₁╱ ╲  │
    0 ┼────────│───→ x
      0 1 2 3 4 5

Event Queue (sorted by x):
    (1, LEFT, s₁), (1, LEFT, s₂), (3, LEFT, s₃),
    (5, RIGHT, s₁), (5, RIGHT, s₂), (3, RIGHT, s₃)
    
    Wait — s₃ is vertical! Handle: left = (3,0), right = (3,6)

Processing:
    
    Event (1, LEFT, s₁):
        Insert s₁ into T
        T = {s₁}
        No neighbors to check
    
    Event (1, LEFT, s₂):
        Insert s₂ into T  (s₂ is above s₁ at x=1)
        T = {s₂, s₁}  (ordered by y)
        Check s₂ with predecessor (none) and successor s₁
        
        Do s₁ and s₂ intersect?
        s₁: (1,1)→(5,5), s₂: (1,5)→(5,1)
        orientation(1,1, 5,5, 1,5) = (5-1)(1-5)-(5-1)(5-5) = 4(-4)-4(0) = -16 < 0 → CW
        orientation(1,1, 5,5, 5,1) = (5-1)(5-5)-(5-1)(1-5) = 4(0)-4(-4) = 16 > 0 → CCW
        Different! And check other pair similarly → YES, INTERSECT!
        
        Compute intersection point:
        s₁: y = x (line from origin)
        s₂: y = -x + 6
        x = -x + 6 → x = 3, y = 3
        Intersection at (3, 3)!
        
        Add (3, INTERSECTION, s₁, s₂) to Q

    Event (3, INTERSECTION, s₁, s₂):
        Report intersection (3, 3) ← FIRST INTERSECTION FOUND!
        Swap s₁ and s₂ in T
        T = {s₁, s₂}  (now s₁ above s₂)
        Check new neighbor pairs...

    Event (3, LEFT, s₃):
        Insert s₃ (vertical segment)
        Check with neighbors → more intersections found at (3, 3)

COMPLEXITY
──────────
    - Detecting any intersection: O(n log n)
    - Reporting k intersections: O((n + k) log n)

EDGE CASES
──────────
    1. Overlapping collinear segments: Report as intersection
    2. Shared endpoints: Use consistent tie-breaking
    3. Vertical segments: Handle carefully in event ordering
    4. Numerical precision: Use exact arithmetic when possible

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
3) NETWORK FLOW (Max Flow, Min Cut, Ford-Fulkerson, Edmonds-Karp)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DEFINITIONS
───────────
Flow Network: G = (V, E) directed graph with:
    - Source s, Sink t
    - Capacity c(u,v) ≥ 0 for each edge (u,v)

Flow f: Assignment f(u,v) to each edge satisfying:
    1. Capacity constraint: 0 ≤ f(u,v) ≤ c(u,v)
    2. Flow conservation: For all v ∈ V - {s,t}:
       Σ f(u,v) = Σ f(v,w)   (flow in = flow out)

Flow Value: |f| = Σ f(s,v) - Σ f(v,s) = net flow out of source

RESIDUAL GRAPH
──────────────
Given flow f, residual graph Gf has edges:
    - Forward edge (u,v) with capacity cf(u,v) = c(u,v) - f(u,v)  if > 0
    - Backward edge (v,u) with capacity cf(v,u) = f(u,v)          if > 0

    Original Graph              Residual Graph (after flow f=2)
    ┌─────────────┐            ┌─────────────┐
    │    c=5      │            │   cf=3      │
    │  u ───→ v   │            │  u ───→ v   │
    │    f=2      │     →      │    cf=2     │
    │             │            │  u ←─── v   │
    └─────────────┘            └─────────────┘
                               (backward edge allows "undoing" flow)

FORD-FULKERSON METHOD
─────────────────────
```
FORD-FULKERSON(G, s, t):
    Initialize f(u,v) = 0 for all edges
    
    While there exists augmenting path P from s to t in Gf:
        cf(P) = min{cf(u,v) : (u,v) ∈ P}  // bottleneck capacity
        
        For each edge (u,v) in P:
            if (u,v) is forward edge:
                f(u,v) = f(u,v) + cf(P)
            else:  // backward edge
                f(v,u) = f(v,u) - cf(P)
    
    Return f
```

EDMONDS-KARP: Use BFS to find shortest augmenting path → O(VE²)

COMPLETE WORKED EXAMPLE
───────────────────────

Network:
                    a
                  ╱   ╲
               10╱     ╲8
                ╱       ╲
               s         t
                ╲       ╱
               5 ╲     ╱10
                  ╲   ╱
                    b
                    
    Also: a → b with capacity 2

    Detailed graph:
    
           ┌───── a ─────┐
           │      │      │
        10 │      │2     │ 8
           │      ↓      │
           s      │      t
           │      │      │
         5 │      │      │ 10
           │      ↓      │
           └───── b ─────┘

STEP-BY-STEP:

Initial flow: All edges have f = 0

Iteration 1:
    Find augmenting path (BFS): s → a → t
    
           ┌───── a ─────┐
           │  10  │  2   │  8
           s ────→│      ├────→ t
           
    Bottleneck: min(10, 8) = 8
    Augment by 8
    
    Current flow:
        f(s,a) = 8,  f(a,t) = 8
        |f| = 8

Iteration 2:
    Residual graph:
           ┌───── a ─────┐
           │  2   │  2   │  (saturated: no forward)
           │  ←8  │      │  ←8 (backward)
           s      │      t
           │  5   │      │  10
           └───── b ─────┘

    Find augmenting path (BFS): s → b → t
    Bottleneck: min(5, 10) = 5
    Augment by 5
    
    Current flow:
        f(s,a) = 8,  f(a,t) = 8
        f(s,b) = 5,  f(b,t) = 5
        |f| = 13

Iteration 3:
    Residual graph:
           ┌───── a ─────┐
           │  2   │  2   │  ←8
           │  ←8  │      │
           s      ↓      t
           │  ←5  │      │  5
           └───── b ─────┘
                  ↑
                  5

    Find augmenting path (BFS): s → a → b → t
    Bottleneck: min(2, 2, 5) = 2
    Augment by 2
    
    Current flow:
        f(s,a) = 10, f(a,t) = 8, f(a,b) = 2
        f(s,b) = 5,  f(b,t) = 7
        |f| = 15

Iteration 4:
    No more augmenting paths from s to t!
    
FINAL FLOW:
           ┌───── a ─────┐
           │ 10/10│ 2/2  │ 8/8
           s      ↓      t
           │  5/5 │      │ 7/10
           └───── b ─────┘

    Maximum Flow |f*| = 15

MAX-FLOW MIN-CUT THEOREM
────────────────────────
THEOREM: max flow value = min cut capacity

Cut (S, T): Partition of V where s ∈ S, t ∈ T
Cut capacity: c(S,T) = Σ c(u,v) for u∈S, v∈T

In our example:
    Cut {s}, {a,b,t}: capacity = c(s,a) + c(s,b) = 10 + 5 = 15
    
    This equals max flow! ✓

PROOF SKETCH (MAX-FLOW = MIN-CUT):
    1. Any flow ≤ any cut capacity (flow must cross cut)
    2. When Ford-Fulkerson terminates, no augmenting path exists
    3. Define S = vertices reachable from s in Gf
    4. Cut (S, V-S) has capacity = flow value
    5. Hence max flow = min cut ∎

BIPARTITE MATCHING APPLICATION
──────────────────────────────
Transform bipartite graph to flow network:

    Bipartite Graph         Flow Network
    
    L₁ ─── R₁              s ─1→ L₁ ─1→ R₁ ─1→ t
       ╲                        ╲    ╱
    L₂ ─╳─ R₂              s ─1→ L₂ ─1→ R₂ ─1→ t
       ╱                        ╱    ╲
    L₃ ─── R₃              s ─1→ L₃ ─1→ R₃ ─1→ t

Max flow = max matching size!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
4) FIRST-PRICE & SECOND-PRICE (VICKREY) AUCTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

AUCTION TYPES
─────────────

┌─────────────────────────────────────────────────────────────────────────┐
│  AUCTION TYPE      │  WINNER          │  PAYMENT                       │
├─────────────────────────────────────────────────────────────────────────┤
│  First-Price (FP)  │  Highest bidder  │  Own bid                       │
│  Second-Price (SP) │  Highest bidder  │  Second-highest bid            │
│  (Vickrey)         │                  │                                │
└─────────────────────────────────────────────────────────────────────────┘

SECOND-PRICE AUCTION (VICKREY)
──────────────────────────────
KEY PROPERTY: Truthful bidding is a DOMINANT STRATEGY!

Why? Consider bidder with true value v:

Case Analysis (bidding b instead of v):

    ┌─────────────────────────────────────────────────────────────────────┐
    │  Scenario           │  Bid v          │  Bid b > v                  │
    ├─────────────────────────────────────────────────────────────────────┤
    │  v > 2nd highest    │  WIN, pay 2nd   │  WIN, pay 2nd (same)        │
    │  v < 2nd highest    │  LOSE           │  May WIN but pay > v (LOSS!)│
    └─────────────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────────────┐
    │  Scenario           │  Bid v          │  Bid b < v                  │
    ├─────────────────────────────────────────────────────────────────────┤
    │  v > 2nd highest    │  WIN, pay 2nd   │  May LOSE (miss profit!)    │
    │  v < 2nd highest    │  LOSE           │  LOSE (same)                │
    └─────────────────────────────────────────────────────────────────────┘

    Conclusion: Bidding truthfully (b = v) is WEAKLY DOMINANT!

WORKED EXAMPLE (VICKREY)
────────────────────────
3 bidders with private values: v₁ = 100, v₂ = 75, v₃ = 60

In equilibrium, they bid truthfully: b₁ = 100, b₂ = 75, b₃ = 60

    Bids:     b₁ = 100    b₂ = 75    b₃ = 60
              ▓▓▓▓▓▓▓▓    ▒▒▒▒▒▒     ░░░░░
    
    Winner: Bidder 1 (highest bid)
    Payment: 75 (second-highest bid)
    
    Bidder 1's utility: v₁ - payment = 100 - 75 = 25 ✓
    Bidder 2's utility: 0 (didn't win)
    Bidder 3's utility: 0 (didn't win)

FIRST-PRICE AUCTION
───────────────────
In FP auctions, bidding truthfully is NOT optimal!
    - If you bid v and win, your utility = v - v = 0
    - Better to "shade" your bid below v

SYMMETRIC BAYESIAN NASH EQUILIBRIUM (FP):
For n bidders with values i.i.d. from Uniform[0,1]:

    Optimal bidding strategy: b(v) = (n-1)/n × v

    For n = 2: b(v) = v/2     (bid half your value)
    For n = 3: b(v) = 2v/3    (bid 2/3 of your value)
    For n → ∞: b(v) → v       (approaches truthful as competition increases)

WORKED EXAMPLE (FIRST-PRICE, n=2)
─────────────────────────────────
2 bidders with values: v₁ = 80, v₂ = 60 (from Uniform[0,100])

Equilibrium bids: b(v) = v/2
    b₁ = 80/2 = 40
    b₂ = 60/2 = 30

    Bids:     b₁ = 40      b₂ = 30
              ▓▓▓▓▓▓       ▒▒▒▒▒
    
    Winner: Bidder 1
    Payment: 40 (own bid)
    
    Bidder 1's utility: 80 - 40 = 40

REVENUE EQUIVALENCE THEOREM
───────────────────────────
Under certain conditions (IPV, risk-neutral, symmetric):
    Expected revenue is THE SAME in FP and SP auctions!

For Uniform[0,1], n bidders:
    E[Revenue] = (n-1)/(n+1)

SPONSORED SEARCH AUCTIONS (GSP)
───────────────────────────────
Multiple ad slots with different click-through rates (CTR):

    Slot 1: CTR = α₁ = 1.0   (most visible)
    Slot 2: CTR = α₂ = 0.5
    Slot 3: CTR = α₃ = 0.2

Advertisers have value-per-click vⱼ.

GSP Rule:
    - Rank by bid
    - Each advertiser pays bid of next advertiser below

    Example:
        Advertiser A: bid 10, value 12  →  Slot 1, pays 8
        Advertiser B: bid 8,  value 10  →  Slot 2, pays 5
        Advertiser C: bid 5,  value 7   →  Slot 3, pays 0

    Note: GSP is NOT truthful! (Unlike Vickrey)
          But has nice Nash equilibrium properties.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
5) MYERSON'S LEMMA (Optimal Auction Design)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

SETTING: SINGLE-PARAMETER MECHANISM DESIGN
──────────────────────────────────────────
- n agents, each with private value vᵢ ∈ [0, vmax]
- Allocation rule: x(v) where xᵢ(v) ∈ [0,1] = probability agent i gets item
- Payment rule: p(v) where pᵢ(v) = payment by agent i
- Agent i's utility: uᵢ = vᵢ · xᵢ - pᵢ

Goal: Design DSIC (dominant strategy incentive compatible) mechanism

MYERSON'S LEMMA (THREE PARTS)
─────────────────────────────

┌─────────────────────────────────────────────────────────────────────────┐
│  Part 1: An allocation rule x is implementable (in a DSIC mechanism)   │
│          if and only if it is MONOTONE in each agent's bid.            │
│                                                                        │
│          Monotone: xᵢ(z, v₋ᵢ) is non-decreasing in z                  │
│                                                                        │
│  Part 2: If x is monotone, there is a UNIQUE payment rule that makes  │
│          (x, p) truthful (up to a constant).                          │
│                                                                        │
│  Part 3: The payment rule is given by:                                │
│                    vᵢ                                                  │
│          pᵢ(v) = vᵢ·xᵢ(v) - ∫ xᵢ(z, v₋ᵢ) dz + pᵢ(0, v₋ᵢ)            │
│                    0                                                   │
└─────────────────────────────────────────────────────────────────────────┘

VISUAL: MONOTONE ALLOCATION
───────────────────────────
    
    xᵢ(vᵢ)
    1 ┤           ┌────────
      │           │
      │           │
    0 ┼───────────┴─────────→ vᵢ
      0          v*        
                 ↑
            threshold (reserve)
    
    This is monotone: allocation increases (or stays same) as bid increases

PAYMENT FORMULA INTERPRETATION
──────────────────────────────
```
pᵢ(vᵢ) = ∫₀^{vᵢ} z · (dxᵢ/dz) dz

       = Area under the "threshold curve"
```

For single-item auction with reserve r:
    - If vᵢ < r: xᵢ = 0, pᵢ = 0
    - If vᵢ ≥ r: xᵢ = 1, pᵢ = r (threshold payment!)

VIRTUAL VALUES
──────────────
For distribution F with density f:

    ┌────────────────────────────────────┐
    │                1 - F(v)            │
    │   φ(v) = v - ──────────            │
    │                 f(v)               │
    └────────────────────────────────────┘

Intuition: Virtual value = marginal revenue from serving bidder

For Uniform[0,1]: F(v) = v, f(v) = 1
    φ(v) = v - (1-v)/1 = 2v - 1

    v    │  φ(v)
    ─────┼───────
    0    │  -1
    0.25 │  -0.5
    0.5  │   0    ← reserve price (where φ = 0)
    0.75 │   0.5
    1    │   1

OPTIMAL AUCTION DESIGN
──────────────────────
Myerson's Optimal Auction: Allocate to maximize VIRTUAL WELFARE!

    max Σᵢ φ(vᵢ) · xᵢ    subject to feasibility

For single item: Give to bidder with highest φ(vᵢ) > 0

WORKED EXAMPLE: DERIVING OPTIMAL RESERVE
────────────────────────────────────────
Setting: Single item, 2 bidders, values i.i.d. Uniform[0,100]

Step 1: Compute virtual value
    φ(v) = v - (100-v)/1 = 2v - 100

Step 2: Find reserve price (where φ(v*) = 0)
    2v* - 100 = 0
    v* = 50

Step 3: Optimal Auction Rule
    - If max(v₁, v₂) < 50: No sale
    - Else: Sell to max(v₁, v₂), charge max(50, second-highest bid)

    Example:
        v₁ = 80, v₂ = 60
        φ(80) = 60 > 0, φ(60) = 20 > 0
        Winner: Bidder 1
        Payment: max(50, 60) = 60

        v₁ = 45, v₂ = 40
        φ(45) = -10 < 0, φ(40) = -20 < 0
        No sale!

PROOF SKETCH: WHY TRUTHFUL PAYMENT WORKS
────────────────────────────────────────
Agent i's utility from reporting vᵢ when true value is v̂ᵢ:

    uᵢ(v̂ᵢ, vᵢ) = v̂ᵢ · xᵢ(vᵢ) - pᵢ(vᵢ)

Substituting payment formula:
                                    vᵢ
    uᵢ = v̂ᵢ · xᵢ(vᵢ) - [vᵢ·xᵢ(vᵢ) - ∫ xᵢ(z) dz]
                                    0
                   vᵢ
    uᵢ = (v̂ᵢ - vᵢ)·xᵢ(vᵢ) + ∫ xᵢ(z) dz
                   0

Maximized when vᵢ = v̂ᵢ (truthful report) because xᵢ is monotone! ∎

APPLICATION TO SPONSORED SEARCH
───────────────────────────────
n advertisers, k slots with CTRs α₁ ≥ α₂ ≥ ... ≥ αₖ

Myerson payment for advertiser in slot j:
    pⱼ = αⱼ · (threshold to stay in slot j)
       = αⱼ · bⱼ₊₁   (bid of next advertiser)

This is exactly VCG pricing applied to this setting!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
6) STABLE MATCHING (Gale-Shapley Algorithm)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROBLEM: STABLE MARRIAGE
────────────────────────
- n men and n women
- Each person has a strict preference ranking over all members of opposite sex
- Goal: Find a matching with NO BLOCKING PAIR

BLOCKING PAIR: (m, w) where:
    - m and w are NOT matched to each other
    - m prefers w to his current partner
    - w prefers m to her current partner

GALE-SHAPLEY ALGORITHM (Men-Proposing)
──────────────────────────────────────
```
GALE-SHAPLEY(Men M, Women W, Preferences):
    Initialize all m ∈ M and w ∈ W as FREE
    
    While some man m is free AND hasn't proposed to every woman:
        w ← first woman on m's list he hasn't proposed to
        
        If w is free:
            (m, w) become ENGAGED
        Else if w prefers m to her current fiancé m':
            (m, w) become ENGAGED
            m' becomes FREE
        Else:
            w rejects m (m stays free)
    
    Return the set of engaged pairs
```

COMPLETE WORKED EXAMPLE
───────────────────────
Men: {A, B, C}      Women: {X, Y, Z}

Preferences (most preferred → least preferred):
    A: X > Y > Z        X: B > A > C
    B: Y > X > Z        Y: A > B > C
    C: X > Y > Z        Z: A > B > C

VISUAL REPRESENTATION:
    
    Men's Preferences:          Women's Preferences:
    ┌───┬───────────┐          ┌───┬───────────┐
    │ A │ X > Y > Z │          │ X │ B > A > C │
    │ B │ Y > X > Z │          │ Y │ A > B > C │
    │ C │ X > Y > Z │          │ Z │ A > B > C │
    └───┴───────────┘          └───┴───────────┘

STEP-BY-STEP EXECUTION:

Round 1: All men are free, propose to their first choice
    ┌─────────────────────────────────────────────────────┐
    │  A proposes to X (his #1)                           │
    │  B proposes to Y (his #1)                           │
    │  C proposes to X (his #1)                           │
    │                                                     │
    │  X receives proposals from A and C                  │
    │  X's preference: B > A > C, so X accepts A          │
    │  C is rejected                                      │
    │                                                     │
    │  Y receives proposal from B                         │
    │  Y accepts B                                        │
    │                                                     │
    │  Status: A-X (engaged), B-Y (engaged), C (free)     │
    └─────────────────────────────────────────────────────┘
    
    Engagement diagram after Round 1:
    
    Men         Women
    ┌───┐       ┌───┐
    │ A │═══════│ X │
    │ B │═══════│ Y │
    │ C │       │ Z │
    └───┘       └───┘

Round 2: Only C is free
    ┌─────────────────────────────────────────────────────┐
    │  C proposes to Y (his #2, since X rejected him)     │
    │                                                     │
    │  Y is engaged to B                                  │
    │  Y's preference: A > B > C                          │
    │  Y prefers B over C, so Y rejects C                 │
    │                                                     │
    │  Status: A-X, B-Y, C (free)                         │
    └─────────────────────────────────────────────────────┘

Round 3: C is still free
    ┌─────────────────────────────────────────────────────┐
    │  C proposes to Z (his #3, last choice)              │
    │                                                     │
    │  Z is free, so Z accepts C                          │
    │                                                     │
    │  Status: A-X, B-Y, C-Z (all matched!)               │
    └─────────────────────────────────────────────────────┘
    
    Final matching:
    
    Men         Women
    ┌───┐       ┌───┐
    │ A │═══════│ X │
    │ B │═══════│ Y │
    │ C │═══════│ Z │
    └───┘       └───┘

VERIFY STABILITY:
    Check all non-matched pairs for blocking:
    
    (A, Y): A prefers X > Y. Not blocking ✓
    (A, Z): A prefers X > Z. Not blocking ✓
    (B, X): X prefers B > A! But B prefers Y > X. Not blocking ✓
    (B, Z): B prefers Y > Z. Not blocking ✓
    (C, X): X prefers A > C. Not blocking ✓
    (C, Y): Y prefers B > C. Not blocking ✓
    
    NO BLOCKING PAIRS! Matching is stable ✓

PROPERTIES OF GALE-SHAPLEY
──────────────────────────

┌──────────────────────────────────────────────────────────────────────────┐
│  1. TERMINATION: Algorithm terminates in at most n² iterations           │
│     (Each man proposes to each woman at most once)                       │
│                                                                          │
│  2. CORRECTNESS: Output is always a stable matching                      │
│                                                                          │
│  3. MAN-OPTIMAL: Each man gets his BEST possible stable partner          │
│                                                                          │
│  4. WOMAN-PESSIMAL: Each woman gets her WORST stable partner             │
│                                                                          │
│  5. STRATEGY-PROOF for proposers: Men cannot benefit by lying            │
│                                                                          │
│  6. UNIQUENESS: If every stable matching is the same, GS finds it        │
└──────────────────────────────────────────────────────────────────────────┘

PROOF OF STABILITY (SKETCH)
───────────────────────────
Suppose (m, w) is a blocking pair in the output.

Then:
    - m prefers w to his partner w'
    - This means m proposed to w before w' (men propose in preference order)
    - w rejected m at some point
    - w only rejects when she has a better option
    - w's final partner m' must be ≥ preferred to m by w
    - So w does NOT prefer m to m'
    
    CONTRADICTION! (m, w) cannot be blocking. ∎

COMPLEXITY
──────────
Time: O(n²)  —  at most n² proposals
Space: O(n²) —  storing preferences

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
7) NASH EQUILIBRIUM (Pure and Mixed Strategies)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

GAME THEORY FUNDAMENTALS
────────────────────────
A Strategic-Form Game consists of:
    - Players: N = {1, 2, ..., n}
    - Strategy sets: Sᵢ for each player i
    - Payoff functions: uᵢ: S₁ × S₂ × ... × Sₙ → ℝ

NASH EQUILIBRIUM DEFINITION
───────────────────────────
A strategy profile s* = (s₁*, s₂*, ..., sₙ*) is a Nash Equilibrium if:

    ┌────────────────────────────────────────────────────────────────────┐
    │  For all players i and all alternative strategies sᵢ':            │
    │                                                                    │
    │      uᵢ(sᵢ*, s₋ᵢ*) ≥ uᵢ(sᵢ', s₋ᵢ*)                               │
    │                                                                    │
    │  No player can UNILATERALLY improve their payoff by deviating!   │
    └────────────────────────────────────────────────────────────────────┘

CLASSIC GAME 1: PRISONER'S DILEMMA
──────────────────────────────────
Two prisoners can either Cooperate (stay silent) or Defect (confess)

Payoff Matrix:
                        Player 2
                    C           D
              ┌─────────┬─────────┐
    Player 1  │ (-1,-1) │ (-3, 0) │  C
              ├─────────┼─────────┤
              │ (0, -3) │ (-2,-2) │  D
              └─────────┴─────────┘
              
    (payoff₁, payoff₂) where higher is better

Finding Nash Equilibrium:
    
    Check (C, C): Can P1 improve by switching to D?
        u₁(C,C) = -1, u₁(D,C) = 0  →  YES, D is better!
        (C, C) is NOT a Nash equilibrium
    
    Check (C, D): Can P1 improve? u₁(C,D) = -3, u₁(D,D) = -2 → YES
    Check (D, C): Can P2 improve? u₂(D,C) = -3, u₂(D,D) = -2 → YES
    
    Check (D, D): 
        Can P1 improve? u₁(D,D) = -2, u₁(C,D) = -3 → NO
        Can P2 improve? u₂(D,D) = -2, u₂(D,C) = -3 → NO
        
    ┌──────────────────────────────────────┐
    │  (D, D) is the unique Nash Equilibrium│
    └──────────────────────────────────────┘
    
    Paradox: Both would be better off at (C,C) but it's not stable!

CLASSIC GAME 2: MATCHING PENNIES
────────────────────────────────
Zero-sum game: one player's gain = other's loss

                        Player 2
                    H           T
              ┌─────────┬─────────┐
    Player 1  │ (1, -1) │ (-1, 1) │  H
              ├─────────┼─────────┤
              │ (-1, 1) │ (1, -1) │  T
              └─────────┴─────────┘

Finding Pure Nash Equilibrium:
    (H, H): P2 can improve by switching to T ✗
    (H, T): P1 can improve by switching to T ✗
    (T, H): P1 can improve by switching to H ✗
    (T, T): P2 can improve by switching to H ✗
    
    NO PURE NASH EQUILIBRIUM EXISTS!

MIXED STRATEGY NASH EQUILIBRIUM
───────────────────────────────
A mixed strategy is a probability distribution over pure strategies.

For Matching Pennies:
    Let P1 play H with probability p, T with probability (1-p)
    Let P2 play H with probability q, T with probability (1-q)

P1's expected payoff:
    E[u₁] = p·q·(1) + p·(1-q)·(-1) + (1-p)·q·(-1) + (1-p)·(1-q)·(1)
          = pq - p(1-q) - (1-p)q + (1-p)(1-q)
          = pq - p + pq - q + pq + 1 - p - q + pq
          = 4pq - 2p - 2q + 1

P2 is indifferent when P1 randomizes optimally:
    ∂E[u₂]/∂q = 0 implies p = 1/2

Similarly, P1 is indifferent when q = 1/2

    ┌────────────────────────────────────────────────────────────────┐
    │  Mixed Nash Equilibrium: p = 1/2, q = 1/2                      │
    │  Both players randomize 50-50 between Heads and Tails          │
    │  Expected payoff for both: 0                                   │
    └────────────────────────────────────────────────────────────────┘

CLASSIC GAME 3: BATTLE OF THE SEXES
───────────────────────────────────
Two players want to coordinate but have different preferences

                        Player 2 (Wife)
                    Opera       Football
              ┌─────────────┬─────────────┐
    Player 1  │   (3, 2)    │   (0, 0)    │  Opera
    (Husband) ├─────────────┼─────────────┤
              │   (0, 0)    │   (2, 3)    │  Football
              └─────────────┴─────────────┘

Pure Nash Equilibria:
    (Opera, Opera): Neither can improve alone ✓ NE
    (Football, Football): Neither can improve alone ✓ NE

Mixed Nash Equilibrium:
    Let husband play Opera with probability p
    Let wife play Opera with probability q
    
    Wife indifferent: 2p + 0(1-p) = 0·p + 3(1-p)
                     2p = 3 - 3p  →  p = 3/5
    
    Husband indifferent: 3q = 2(1-q)  →  q = 2/5
    
    ┌────────────────────────────────────────────────────────────────┐
    │  Three Nash Equilibria:                                        │
    │  1. (Opera, Opera)        - Pure NE                           │
    │  2. (Football, Football)  - Pure NE                           │
    │  3. p = 3/5, q = 2/5     - Mixed NE                           │
    └────────────────────────────────────────────────────────────────┘

NASH'S THEOREM
──────────────
┌──────────────────────────────────────────────────────────────────────────┐
│  Every finite game has at least one Nash equilibrium                     │
│  (possibly in mixed strategies)                                          │
│                                                                          │
│  Proof: Uses Brouwer/Kakutani fixed point theorem                        │
└──────────────────────────────────────────────────────────────────────────┘

DOMINATED STRATEGIES
────────────────────
Strategy sᵢ is STRICTLY DOMINATED if there exists sᵢ' such that:
    uᵢ(sᵢ', s₋ᵢ) > uᵢ(sᵢ, s₋ᵢ) for ALL s₋ᵢ

ITERATED ELIMINATION: Remove dominated strategies repeatedly

Example:
                        C       M       R
              ┌─────────┬───────┬───────┐
            T │ (1, 0)  │ (1,2) │ (0,1) │
              ├─────────┼───────┼───────┤
            B │ (0, 3)  │ (0,1) │ (2,0) │
              └─────────┴───────┴───────┘

    R is dominated by M for P2 (2>1, 1>0) → Eliminate R
    Now B is dominated by T for P1 → Eliminate B
    Now M dominates C for P2 → Eliminate C
    
    Result: (T, M) is the unique NE!

ZERO-SUM GAMES AND MINIMAX
──────────────────────────
In zero-sum games: u₁ + u₂ = 0

Minimax Theorem (von Neumann):
    max_p min_q E[u₁] = min_q max_p E[u₁] = value of the game

Can find NE using Linear Programming!

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
8) PUT-CALL PARITY (Options and Financial Derivatives)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

OPTIONS FUNDAMENTALS
────────────────────
CALL OPTION: Right (not obligation) to BUY asset at strike price K at time T
PUT OPTION:  Right (not obligation) to SELL asset at strike price K at time T

    Payoff at expiration T:
    
    Call payoff = max(Sₜ - K, 0)    Put payoff = max(K - Sₜ, 0)
    
         Call                              Put
    Payoff                            Payoff
    ↑       ╱                         ↑
    │      ╱                          │╲
    │     ╱                           │ ╲
    │    ╱                            │  ╲
    │───┴────→ Sₜ                     │───╲───→ Sₜ
    0   K                             0   K

PUT-CALL PARITY FORMULA
───────────────────────
For European options with same strike K and expiration T:

    ┌─────────────────────────────────────────────────────────────────┐
    │                                                                 │
    │         C - P = S₀ - K·e^(-rT)                                  │
    │                                                                 │
    │   Where:                                                        │
    │     C = Call option price (today)                               │
    │     P = Put option price (today)                                │
    │     S₀ = Current stock price                                    │
    │     K = Strike price                                            │
    │     r = Risk-free interest rate (continuous)                    │
    │     T = Time to expiration                                      │
    │                                                                 │
    └─────────────────────────────────────────────────────────────────┘

INTUITION: REPLICATING PORTFOLIOS
─────────────────────────────────
Portfolio A: Long Call + K·e^(-rT) in bonds
Portfolio B: Long Put + Long Stock

At expiration T:

    If Sₜ > K:
        Portfolio A: (Sₜ - K) + K = Sₜ
        Portfolio B: 0 + Sₜ = Sₜ           ✓ Same!
    
    If Sₜ < K:
        Portfolio A: 0 + K = K
        Portfolio B: (K - Sₜ) + Sₜ = K     ✓ Same!
    
    If Sₜ = K:
        Portfolio A: 0 + K = K
        Portfolio B: 0 + K = K             ✓ Same!

Since payoffs are identical, prices must be equal (no arbitrage):
    C + K·e^(-rT) = P + S₀
    C - P = S₀ - K·e^(-rT)

WORKED EXAMPLE 1: FINDING PUT PRICE
───────────────────────────────────
Given:
    S₀ = $100 (stock price today)
    K = $95 (strike price)
    r = 5% per year (continuous compounding)
    T = 1 year
    C = $12 (call price)

Find: Put price P

Solution:
    C - P = S₀ - K·e^(-rT)
    12 - P = 100 - 95·e^(-0.05×1)
    12 - P = 100 - 95·e^(-0.05)
    12 - P = 100 - 95×0.9512
    12 - P = 100 - 90.37
    12 - P = 9.63
    P = 12 - 9.63
    
    ┌──────────────┐
    │  P = $2.37   │
    └──────────────┘

WORKED EXAMPLE 2: ARBITRAGE OPPORTUNITY
───────────────────────────────────────
Given:
    S₀ = $50, K = $50, r = 10%, T = 0.5 years
    C = $5, P = $3

Check parity:
    C - P = 5 - 3 = 2
    S₀ - K·e^(-rT) = 50 - 50·e^(-0.05) = 50 - 47.56 = 2.44
    
    Parity violated! C - P < S₀ - K·e^(-rT)
    
    ARBITRAGE STRATEGY (Call is underpriced relative to put):
    
    ┌────────────────────────────────────────────────────────────────────┐
    │  TODAY:                                                            │
    │    • Buy call for $5                                               │
    │    • Sell put for $3                                               │
    │    • Short sell stock, receive $50                                 │
    │    • Invest $48 at risk-free rate (will be $50.44 at T)           │
    │    Net cash flow: -5 + 3 + 50 - 48 = $0                           │
    │                                                                    │
    │  AT EXPIRATION T:                                                  │
    │    If Sₜ > 50: Exercise call, pay $50, receive stock to cover     │
    │                Put expires worthless                               │
    │                Bank account: $50.44                                │
    │                Profit: $50.44 - $50 = $0.44                        │
    │                                                                    │
    │    If Sₜ < 50: Call expires worthless                             │
    │                Put exercised against you, buy stock for $50       │
    │                Use stock to cover short                            │
    │                Bank account: $50.44                                │
    │                Profit: $50.44 - $50 = $0.44                        │
    │                                                                    │
    │  RISK-FREE PROFIT: $0.44 regardless of stock price! ✓             │
    └────────────────────────────────────────────────────────────────────┘

WORKED EXAMPLE 3: COMPUTE CALL FROM PUT
───────────────────────────────────────
Given:
    S₀ = $80, K = $85, r = 8%, T = 3 months = 0.25 years
    P = $8

Find: Call price C

    C = P + S₀ - K·e^(-rT)
    C = 8 + 80 - 85·e^(-0.08×0.25)
    C = 8 + 80 - 85·e^(-0.02)
    C = 8 + 80 - 85×0.9802
    C = 8 + 80 - 83.32
    
    ┌──────────────┐
    │  C = $4.68   │
    └──────────────┘

EXTENSIONS AND NOTES
────────────────────
1. DIVIDENDS: If stock pays dividend D during option life:
       C - P = S₀ - D·e^(-rτ) - K·e^(-rT)
   where τ is time to dividend

2. AMERICAN OPTIONS: Put-call parity doesn't hold exactly
       S₀ - K ≤ C - P ≤ S₀ - K·e^(-rT)
   (Due to early exercise possibilities)

3. DISCRETE vs CONTINUOUS COMPOUNDING:
       Continuous: K·e^(-rT)
       Discrete:   K/(1+r)^T

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
9) PERCEPTRON LEARNING ALGORITHM (PLA)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROBLEM: LINEAR CLASSIFICATION
──────────────────────────────
Given: Training set {(x₁, y₁), (x₂, y₂), ..., (xₙ, yₙ)}
       where xᵢ ∈ ℝᵈ (features), yᵢ ∈ {+1, -1} (labels)

Goal: Find weight vector w such that:
       sign(w · xᵢ) = yᵢ for all i

VISUAL: LINEAR SEPARABILITY
───────────────────────────

    y (feature 2)
    ↑
    │    +           +
    │      +       +
    │        ╲ w
    │    +    ╲
    │          ╲──────────→ decision boundary: w·x = 0
    │           ╲
    │      -     ╲    -
    │         -    ╲
    │    -          ╲  -
    └──────────────────────→ x (feature 1)
    
    Points with w·x > 0 → predict +1
    Points with w·x < 0 → predict -1

THE PERCEPTRON ALGORITHM
────────────────────────
```
PERCEPTRON(training_data):
    Initialize w = 0 (zero vector)
    
    Repeat:
        For each (xᵢ, yᵢ) in training_data:
            If yᵢ · (w · xᵢ) ≤ 0:     // misclassified!
                w ← w + yᵢ · xᵢ       // UPDATE RULE
    
    Until no mistakes in a full pass
    Return w
```

UPDATE RULE INTUITION
─────────────────────

Case 1: y = +1 but w·x < 0 (predicted -1, wrong!)
    w_new = w + x
    
    Effect: w_new · x = (w + x) · x = w·x + |x|² > w·x
    The new w has a LARGER dot product with x (more positive)!

Case 2: y = -1 but w·x > 0 (predicted +1, wrong!)
    w_new = w - x
    
    Effect: w_new · x = (w - x) · x = w·x - |x|² < w·x
    The new w has a SMALLER dot product with x (more negative)!

COMPLETE WORKED EXAMPLE
───────────────────────
Training data (2D):
    Point A: x₁ = (2, 3),  y₁ = +1
    Point B: x₂ = (1, 1),  y₂ = +1
    Point C: x₃ = (-1, -1), y₃ = -1
    Point D: x₄ = (-2, -3), y₄ = -1

           y
           ↑
         3 │      A(+)
         2 │
         1 │  B(+)
         0 ├────────────→ x
        -1 │  C(-)
        -2 │
        -3 │      D(-)

Step-by-step execution:

ITERATION 1:
─────────────
w = (0, 0)

Check A: y₁(w·x₁) = (+1)·(0,0)·(2,3) = 0 ≤ 0  → MISCLASSIFIED!
    Update: w = w + y₁·x₁ = (0,0) + (2,3) = (2, 3)

    ┌─────────────────────────────────────┐
    │  w = (2, 3) after update            │
    └─────────────────────────────────────┘

Check B: y₂(w·x₂) = (+1)·(2,3)·(1,1) = 2+3 = 5 > 0 ✓ Correct

Check C: y₃(w·x₃) = (-1)·(2,3)·(-1,-1) = (-1)·(-2-3) = 5 > 0 ✓ Correct

Check D: y₄(w·x₄) = (-1)·(2,3)·(-2,-3) = (-1)·(-4-9) = 13 > 0 ✓ Correct

End of iteration 1. All points classified correctly!

FINAL RESULT:
─────────────
    ┌───────────────────────────────────────────┐
    │  w = (2, 3)                               │
    │                                           │
    │  Decision boundary: 2x + 3y = 0           │
    │  or: y = -2x/3                            │
    └───────────────────────────────────────────┘

Verification:
    A: 2(2) + 3(3) = 13 > 0 → +1 ✓
    B: 2(1) + 3(1) = 5 > 0 → +1 ✓
    C: 2(-1) + 3(-1) = -5 < 0 → -1 ✓
    D: 2(-2) + 3(-3) = -13 < 0 → -1 ✓

           y
           ↑
         3 │      A(+)
         2 │    ╱
         1 │  B╱(+)
         0 ├──╱─────────→ x
        -1 │╱C(-)     Decision boundary: 2x + 3y = 0
        -2 │
        -3 │      D(-)

HARDER EXAMPLE (Multiple updates)
─────────────────────────────────
Training data:
    x₁ = (3, 3),  y₁ = +1
    x₂ = (4, 3),  y₂ = +1
    x₃ = (1, 1),  y₃ = -1

ITERATION 1: w = (0, 0)
    x₁: y₁(w·x₁) = 0 ≤ 0 → Update: w = (0,0) + (3,3) = (3, 3)
    x₂: y₂(w·x₂) = (3,3)·(4,3) = 21 > 0 ✓
    x₃: y₃(w·x₃) = (-1)·(3,3)·(1,1) = -6 < 0  → MISCLASSIFIED!
        Update: w = (3,3) + (-1)·(1,1) = (2, 2)

ITERATION 2: w = (2, 2)
    x₁: y₁(w·x₁) = (2,2)·(3,3) = 12 > 0 ✓
    x₂: y₂(w·x₂) = (2,2)·(4,3) = 14 > 0 ✓
    x₃: y₃(w·x₃) = (-1)·(2,2)·(1,1) = -4 < 0 → MISCLASSIFIED!
        Update: w = (2,2) - (1,1) = (1, 1)

ITERATION 3: w = (1, 1)
    x₁: y₁(w·x₁) = (1,1)·(3,3) = 6 > 0 ✓
    x₂: y₂(w·x₂) = (1,1)·(4,3) = 7 > 0 ✓
    x₃: y₃(w·x₃) = (-1)·(1,1)·(1,1) = -2 < 0 → MISCLASSIFIED!
        Update: w = (1,1) - (1,1) = (0, 0)

... (this oscillates because data is NOT linearly separable!)

PERCEPTRON CONVERGENCE THEOREM
──────────────────────────────
┌──────────────────────────────────────────────────────────────────────────┐
│  If the data is linearly separable with margin γ and all points have    │
│  norm at most R, then the perceptron makes at most (R/γ)² updates.     │
│                                                                          │
│  Definitions:                                                            │
│    γ = margin = min_i |w* · xᵢ| / ||w*||  (for optimal w*)              │
│    R = max_i ||xᵢ||                                                      │
└──────────────────────────────────────────────────────────────────────────┘

PROOF SKETCH:
    1. After k updates: w^(k+1) · w* ≥ k·γ (each update increases alignment)
    2. After k updates: ||w^(k+1)||² ≤ k·R² (each update adds at most R²)
    3. Combining: cos(angle) = (w·w*)/(||w||||w*||) ≥ k·γ/(√k·R·||w*||)
    4. Since cos ≤ 1: k ≤ (R·||w*||/γ)² = (R/γ)²  ∎

BIAS TERM
─────────
To include bias b: w·x + b = 0

Trick: Augment x with 1 and w with b
    x' = (x₁, x₂, ..., xₙ, 1)
    w' = (w₁, w₂, ..., wₙ, b)
    
    Then w'·x' = w·x + b

CONNECTION TO NEURAL NETWORKS
─────────────────────────────
The perceptron is a single-layer neural network!

    x₁ ──┐
         │
    x₂ ──┼──→ [ Σ wᵢxᵢ ] ──→ [ sign ] ──→ ŷ ∈ {+1, -1}
         │
    x₃ ──┘
    
Multi-layer perceptrons (MLPs) can learn non-linear boundaries.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
APPENDIX: KEY PROOFS AND THEOREMS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

PROOF 1: BFPRT O(n) WORST-CASE GUARANTEE
────────────────────────────────────────
Claim: T(n) = T(n/5) + T(7n/10) + O(n) = O(n)

Proof by strong induction:
    Assume T(k) ≤ ck for all k < n (for some constant c)
    
    T(n) = T(n/5) + T(7n/10) + an
         ≤ c(n/5) + c(7n/10) + an
         = cn(1/5 + 7/10) + an
         = cn(9/10) + an
         = n(9c/10 + a)
    
    For T(n) ≤ cn, we need: 9c/10 + a ≤ c
                           a ≤ c/10
                           c ≥ 10a
    
    Choose c = 10a, then T(n) = O(n)  ∎

PROOF 2: GALE-SHAPLEY PRODUCES STABLE MATCHING
──────────────────────────────────────────────
Claim: The output has no blocking pair.

Proof by contradiction:
    Suppose (m, w) is a blocking pair in output μ.
    
    Let μ(m) = w' and μ(w) = m'.
    
    Since (m, w) blocks:
        1. m prefers w to w'
        2. w prefers m to m'
    
    Since m prefers w to w', m proposed to w before w' 
    (men propose in preference order).
    
    When m proposed to w:
        - Either w accepted and later replaced m with someone she prefers
        - Or w rejected m because she had someone she prefers
    
    In both cases, w's final partner m' must be preferred by w over m.
    
    This contradicts (2)!  ∎

PROOF 3: MAX-FLOW MIN-CUT THEOREM
─────────────────────────────────
Claim: max |f| = min c(S,T) over all s-t cuts

Proof:
    Part A: Any flow ≤ any cut
        For any flow f and cut (S,T):
        |f| = flow out of S - flow into S
            ≤ capacity out of S
            = c(S,T)
        
        Therefore: max flow ≤ min cut
    
    Part B: At termination, equality holds
        When Ford-Fulkerson terminates, no augmenting path exists.
        
        Define: S = {vertices reachable from s in residual graph Gf}
                T = V - S
        
        Note: t ∉ S (otherwise path exists)
        
        For any edge (u,v) with u ∈ S, v ∈ T:
            cf(u,v) = 0 (else v would be reachable)
            So f(u,v) = c(u,v) (saturated)
        
        For any edge (u,v) with u ∈ T, v ∈ S:
            cf(v,u) = 0 (else there's backward capacity)
            So f(u,v) = 0
        
        |f| = Σ f(u,v) - Σ f(u,v)  for u∈S, v∈T
            = Σ c(u,v) - 0
            = c(S,T)
        
        Therefore: max flow = min cut  ∎

PROOF 4: VICKREY AUCTION TRUTHFULNESS
─────────────────────────────────────
Claim: Bidding b = v is weakly dominant in second-price auction.

Proof:
    Fix other bidders' bids. Let h = highest other bid.
    
    Case 1: v > h (you would win by bidding truthfully)
        - Bid b = v: WIN, pay h, utility = v - h > 0
        - Bid b > h: WIN, pay h, utility = v - h (same)
        - Bid b < h: LOSE, utility = 0 (worse!)
    
    Case 2: v < h (you would lose by bidding truthfully)
        - Bid b = v: LOSE, utility = 0
        - Bid b > h: WIN, pay h, utility = v - h < 0 (worse!)
        - Bid b < h: LOSE, utility = 0 (same)
    
    Case 3: v = h
        - Any bid: either win with utility 0 or lose with utility 0
    
    In all cases, bidding v is at least as good as any other bid!  ∎

QUICK REFERENCE: COMPLEXITY SUMMARY
───────────────────────────────────
┌──────────────────────────────┬────────────────────────────────────────┐
│  Algorithm                   │  Time Complexity                       │
├──────────────────────────────┼────────────────────────────────────────┤
│  BFPRT (Median-of-medians)   │  O(n) worst-case                       │
│  Quickselect (randomized)    │  O(n) expected, O(n²) worst            │
│  Line segment intersection   │  O((n+k) log n) for k intersections    │
│  Ford-Fulkerson              │  O(E·|f*|) for max flow f*             │
│  Edmonds-Karp                │  O(VE²)                                │
│  Dinic's algorithm           │  O(V²E)                                │
│  Gale-Shapley                │  O(n²)                                 │
│  Nash equilibrium (2-player) │  LP for zero-sum, PPAD for general    │
│  Perceptron                  │  O((R/γ)² · d) for d dimensions       │
└──────────────────────────────┴────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                              END OF CONCEPTS FILE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
