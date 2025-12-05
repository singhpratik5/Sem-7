AGA Major — Comprehensive Question Bank with Full Solutions
═══════════════════════════════════════════════════════════════════════════════

This file contains exam-style questions (numerical and proofs) with complete
step-by-step solutions, diagrams, and verification.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 1: DETERMINISTIC MEDIAN FINDING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q1.1 [PROOF]: Prove that median-of-medians guarantees at least 3n/10      ║
║       elements on each side of the pivot.                                  ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Let n elements be divided into ⌈n/5⌉ groups of 5.

Step 1: Count groups with median ≥ pivot M
        At least half of the ⌈n/5⌉ medians are ≥ M (by definition of median)
        Number of such groups ≥ ⌈⌈n/5⌉/2⌉

Step 2: Count elements ≥ M in each such group
        In a sorted group of 5: [a, b, m, d, e]
        If m ≥ M, then d ≥ m ≥ M and e ≥ m ≥ M
        So at least 3 elements are ≥ M per group

Step 3: Total elements ≥ M
        ≥ 3 × ⌈⌈n/5⌉/2⌉
        ≥ 3 × (n/10 - 1)
        = 3n/10 - 3
        ≈ 3n/10 for large n

Visual proof:
    Groups:     G₁    G₂    G₃    G₄    G₅    (suppose 5 groups)
    Medians:    m₁    m₂    m₃    m₄    m₅
    
    Sorted medians: m₂ < m₅ < m₃ < m₁ < m₄
                              ↑
                         M = m₃ (median of medians)
    
    Groups with median ≥ M: G₃, G₁, G₄ (3 groups = half of 5)
    
    In each such group, 3 elements are ≥ median:
    
        G₃: [_, _, m₃, *, *]     3 elements ≥ M
        G₁: [_, _, m₁, *, *]     3 elements ≥ m₁ ≥ M
        G₄: [_, _, m₄, *, *]     3 elements ≥ m₄ ≥ M
    
    Total ≥ M: at least 9 elements = 3 × 3 = 9 ≈ 3(25)/10 ✓

Similarly, at least 3n/10 elements are ≤ M.  ∎

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q1.2 [NUMERICAL]: Run BFPRT on A = [31, 12, 47, 25, 63, 18, 55, 41, 9, 33]║
║       to find the 6th smallest element. Show all steps.                    ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Input: A = [31, 12, 47, 25, 63, 18, 55, 41, 9, 33], k = 6, n = 10

Step 1: Divide into groups of 5
    Group 1: [31, 12, 47, 25, 63]
    Group 2: [18, 55, 41, 9, 33]

Step 2: Sort each group and find medians
    Group 1 sorted: [12, 25, 31, 47, 63] → median = 31
    Group 2 sorted: [9, 18, 33, 41, 55]  → median = 33
    
    Medians M = [31, 33]

Step 3: Find median of medians
    M sorted: [31, 33]
    Median of M = 31 (taking lower median for n=2)
    
    Pivot p = 31

Step 4: Partition A around p = 31
    L = {elements < 31} = {12, 25, 18, 9} → |L| = 4
    E = {elements = 31} = {31}           → |E| = 1  
    R = {elements > 31} = {47, 63, 55, 41, 33} → |R| = 5

    Visualization:
    
    Original: [31, 12, 47, 25, 63, 18, 55, 41, 9, 33]
                ↓
    Partitioned:
    
    L = [12, 25, 18, 9]   E = [31]   R = [47, 63, 55, 41, 33]
    (4 elements)          (1)        (5 elements)
    
    Positions: 1-4         5          6-10

Step 5: Determine which partition contains k=6
    |L| = 4, |L| + |E| = 5
    
    k = 6 > 5, so answer is in R
    
    Recurse: SELECT(R, k - |L| - |E|) = SELECT(R, 6 - 4 - 1) = SELECT(R, 1)

Step 6: Recursive call - SELECT([47, 63, 55, 41, 33], 1)
    n = 5, so we can just sort and pick:
    
    Sorted R: [33, 41, 47, 55, 63]
    1st element = 33

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: The 6th smallest element is 33                                    │
└────────────────────────────────────────────────────────────────────────────┘

Verification:
    A sorted: [9, 12, 18, 25, 31, 33, 41, 47, 55, 63]
               1   2   3   4   5   6   7   8   9  10
    
    Position 6 = 33 ✓

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q1.3 [COMPLEXITY]: Show the recurrence T(n) = T(n/5) + T(7n/10) + cn      ║
║       solves to T(n) = O(n).                                               ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Claim: T(n) ≤ 10cn for all n ≥ 1

Proof by strong induction:

Base case: For small n, T(n) ≤ c' (constant) ≤ 10cn ✓

Inductive step: Assume T(k) ≤ 10ck for all k < n

    T(n) = T(n/5) + T(7n/10) + cn
         ≤ 10c(n/5) + 10c(7n/10) + cn      [by induction hypothesis]
         = 2cn + 7cn + cn
         = 10cn
         = 10cn ✓

Therefore T(n) = O(n).  ∎

Why does this work?
    n/5 + 7n/10 = 2n/10 + 7n/10 = 9n/10 < n
    
    The recursive calls only look at 90% of elements!
    
    ┌─────────────────────────────────────────────┐
    │           n                                 │
    │          ╱ ╲                                │
    │        n/5  7n/10                           │
    │         │     │                             │
    │   Total work per level = cn + 0.9cn + ...   │
    │   Geometric series → O(n)                   │
    └─────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 2: LINE SEGMENT INTERSECTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q2.1 [ALGORITHM]: Describe the sweep-line algorithm for detecting any     ║
║       intersection among n segments. Give time complexity.                 ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

ALGORITHM DESCRIPTION:

```
SWEEP-LINE-INTERSECTION(Segments S):
    1. Initialize:
       - Event Queue Q (priority queue by x-coordinate)
       - Status Structure T (balanced BST by y-coordinate)
    
    2. For each segment s = (p_left, p_right):
       - Insert LEFT_ENDPOINT(s) into Q
       - Insert RIGHT_ENDPOINT(s) into Q
    
    3. While Q is not empty:
       event = ExtractMin(Q)
       
       CASE LEFT_ENDPOINT of segment s:
           Insert s into T (at its y-coordinate on sweep line)
           Let above = predecessor of s in T
           Let below = successor of s in T
           If Intersects(s, above): return TRUE
           If Intersects(s, below): return TRUE
       
       CASE RIGHT_ENDPOINT of segment s:
           Let above = predecessor of s in T
           Let below = successor of s in T
           Delete s from T
           If Intersects(above, below): return TRUE
       
       CASE INTERSECTION of s1, s2:
           return TRUE  (for detection only)
    
    4. Return FALSE (no intersection found)
```

DIAGRAM:

    Sweep line →              Status T at position x:
    │                         ┌─────────────────┐
    │     s3                  │ T (BST by y):   │
    │    ╱                    │    s3 (y=5)     │
    │   ╱  s1                 │    s1 (y=3)     │
    │──╱──╳──────             │    s2 (y=1)     │
    │     s2                  └─────────────────┘
    │
    └─────────→ x
    
    At x = sweep position, only s1, s2, s3 are active.
    We only check neighbors for intersection!

TIME COMPLEXITY:
    - n insertions into Q: O(n log n)
    - n deletions from Q: O(n log n)
    - n insertions/deletions from T: O(n log n)
    - Each neighbor check: O(1)
    
    Total: O(n log n)

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q2.2 [NUMERICAL]: Given segments, find all intersections using sweep line.║
║       s1: (0, 0) → (6, 6)                                                  ║
║       s2: (0, 6) → (6, 0)                                                  ║
║       s3: (0, 3) → (6, 3)                                                  ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

First, visualize the segments:

    y
    6 ┤ s2 start                    s1 end
      │  ╲                          ╱
    5 │   ╲                        ╱
      │    ╲                      ╱
    4 │     ╲                    ╱
      │      ╲                  ╱
    3 ├───────╲────────────────╱───── s3 (horizontal)
      │        ╲  ●          ╱
    2 │         ╲ (3,3)     ╱
      │          ╲        ╱
    1 │           ╲      ╱
      │            ╲    ╱
    0 ├─────────────╲──╱─────────────→ x
      0  1  2  3  4  5  6

Events (sorted by x):
    x = 0: LEFT(s1), LEFT(s2), LEFT(s3)  [y = 0, 6, 3]
    x = 6: RIGHT(s1), RIGHT(s2), RIGHT(s3)

STEP-BY-STEP EXECUTION:

Event x = 0, LEFT(s1) at y = 0:
    T = {s1}
    No neighbors, no checks.

Event x = 0, LEFT(s2) at y = 6:
    T = {s2, s1}  (s2 above s1 at x=0)
    Check (s2, s1):
        Do they intersect? Need orientation test...
        
        s1: (0,0)→(6,6), s2: (0,6)→(6,0)
        
        Orientation(0,0, 6,6, 0,6) = (6-0)(0-6) - (6-0)(6-6) = -36 (CW)
        Orientation(0,0, 6,6, 6,0) = (6-0)(6-6) - (6-0)(0-6) = 36 (CCW)
        Different orientations → segments straddle each other!
        
        Similarly check other pair → YES, INTERSECT!
        
        Compute intersection:
            s1: y = x
            s2: y = -x + 6
            x = -x + 6 → x = 3, y = 3
        
        Insert INTERSECTION(s1, s2) at (3, 3) into Q

Event x = 0, LEFT(s3) at y = 3:
    T = {s2, s3, s1}  (at x=0: s2 at y=6, s3 at y=3, s1 at y=0)
    Check (s3, s2): s3 starts at (0,3), s2 at (0,6)
        Lines: s3: y = 3, s2: y = -x + 6
        Intersection: 3 = -x + 6 → x = 3
        Point (3, 3) is on both segments ✓
        
        Insert INTERSECTION(s3, s2) at (3, 3)  [already in Q]
    
    Check (s3, s1): s3: y = 3, s1: y = x
        Intersection: 3 = x → x = 3
        Point (3, 3) is on both segments ✓
        
        Insert INTERSECTION(s3, s1) at (3, 3)  [already in Q]

Event x = 3, INTERSECTION(s1, s2, s3):
    Report intersection at (3, 3)!
    
    All three segments meet at (3, 3):
    
              s2
               ╲
        s3 ─────●───── s3
               ╱
              s1

    Swap s1 and s2 in T (they cross here)
    T = {s1, s3, s2}  (for x > 3: s1 above s3 above s2)
    
    Check new neighbors: (s1, s3) and (s3, s2) already intersect at (3,3)

Remaining events at x = 6: endpoints, clean up

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: All three segments intersect at the single point (3, 3)          │
│                                                                            │
│  Intersection points found: {(3, 3)}                                       │
│  Intersecting pairs: (s1, s2), (s1, s3), (s2, s3)                         │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q2.3 [NUMERICAL]: Do segments (1,2)→(4,5) and (2,1)→(5,4) intersect?      ║
║       Use the orientation test.                                            ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Segments:
    s1: A(1,2) → B(4,5)
    s2: C(2,1) → D(5,4)

    y
    5 ┤        B
      │       ╱
    4 ┤      ╱    D
      │     ╱   ╱
    3 ┤    ╱   ╱
      │   ╱  ╱
    2 ┤  A  ╱
      │    ╱
    1 ┤   C
      │
    0 ┼────────────→ x
      0 1 2 3 4 5

Orientation Test:
    orientation(P, Q, R) = (Q.x - P.x)(R.y - P.y) - (Q.y - P.y)(R.x - P.x)
    
    Positive → counterclockwise (CCW)
    Negative → clockwise (CW)
    Zero → collinear

Test 1: orientation(A, B, C) = ?
    A = (1,2), B = (4,5), C = (2,1)
    = (4-1)(1-2) - (5-2)(2-1)
    = 3(-1) - 3(1)
    = -3 - 3
    = -6 (CW) ✓

Test 2: orientation(A, B, D) = ?
    A = (1,2), B = (4,5), D = (5,4)
    = (4-1)(4-2) - (5-2)(5-1)
    = 3(2) - 3(4)
    = 6 - 12
    = -6 (CW) ✓

Since orientation(A,B,C) and orientation(A,B,D) have SAME sign,
C and D are on the SAME side of line AB.

    s1 line
      ╲
       ╲  C and D both here (same side)
        ╲

Test 3: orientation(C, D, A) = ?
    C = (2,1), D = (5,4), A = (1,2)
    = (5-2)(2-1) - (4-1)(1-2)
    = 3(1) - 3(-1)
    = 3 + 3
    = 6 (CCW) ✓

Test 4: orientation(C, D, B) = ?
    C = (2,1), D = (5,4), B = (4,5)
    = (5-2)(5-1) - (4-1)(4-2)
    = 3(4) - 3(2)
    = 12 - 6
    = 6 (CCW) ✓

orientation(C,D,A) and orientation(C,D,B) have SAME sign.
A and B are on the SAME side of line CD.

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: The segments do NOT intersect.                                    │
│                                                                            │
│  For intersection, we need:                                                │
│    - C and D on OPPOSITE sides of AB (different signs)                    │
│    - A and B on OPPOSITE sides of CD (different signs)                    │
│                                                                            │
│  Here both tests show same side → NO INTERSECTION                          │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 3: NETWORK FLOW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q3.1 [NUMERICAL]: Find max flow using Edmonds-Karp (BFS) on the network:  ║
║       s → a (cap 16), s → b (cap 13)                                       ║
║       a → b (cap 10), a → c (cap 12)                                       ║
║       b → a (cap 4), b → d (cap 14)                                        ║
║       c → b (cap 9), c → t (cap 20)                                        ║
║       d → c (cap 7), d → t (cap 4)                                         ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Network Diagram:

                    16           12
              ┌─────────→ a ─────────→ c
              │          ↑↓10         │↓9
              │           4           │    20
           s  │                       │────────→ t
              │           14          │↑7       ↑
              │    13    ┌───→ d ─────┘    4    │
              └─────────→ b ──────────────────→─┘
                                          

Let's trace Edmonds-Karp (BFS finds shortest augmenting paths):

ITERATION 1:
═══════════
BFS from s: s → a → c → t  (length 3)

    s ──16──→ a ──12──→ c ──20──→ t

Bottleneck = min(16, 12, 20) = 12
Augment by 12

Flow after iteration 1:
    f(s,a) = 12, f(a,c) = 12, f(c,t) = 12
    |f| = 12

Residual graph:
              ┌───4────→ a ──────────→ c
              │         ↑↓10          │↓9
              │←12       4           │    8
           s  │                      │←12───→ t
              │          14          │↑7      ↑
              │    13   ┌───→ d ─────┘   4    │
              └────────→ b ─────────────────→─┘

ITERATION 2:
═══════════
BFS from s: s → b → d → t  (length 3)

    s ──13──→ b ──14──→ d ──4──→ t

Bottleneck = min(13, 14, 4) = 4
Augment by 4

Flow after iteration 2:
    f(s,a) = 12, f(a,c) = 12, f(c,t) = 12
    f(s,b) = 4, f(b,d) = 4, f(d,t) = 4
    |f| = 16

ITERATION 3:
═══════════
BFS from s: s → b → d → c → t  (length 4)

    s ──9───→ b ──10──→ d ──7──→ c ──8──→ t

Bottleneck = min(9, 10, 7, 8) = 7
Augment by 7

Flow after iteration 3:
    f(s,a) = 12, f(a,c) = 12, f(c,t) = 19
    f(s,b) = 11, f(b,d) = 11, f(d,t) = 4, f(d,c) = 7
    |f| = 23

ITERATION 4:
═══════════
BFS from s: s → b → a → c → t  (length 4)

    s ──2───→ b ──4──→ a ──0──→ c ... (a→c saturated!)

No valid path through a→c. Try another:
s → b → d → c → t?
    s: 2 to b
    b → d: 3 remaining
    d → c: 0 remaining (saturated!)

BFS finds: s → a → b → d → c → t
    s ──4───→ a ──10──→ b ──3──→ d ──0... (d→c saturated)

Actually let me recalculate the residual more carefully:

After iteration 3, residual capacities:
    s→a: 16-12=4, a→s: 12
    s→b: 13-11=2, b→s: 11
    a→c: 12-12=0, c→a: 12
    a→b: 10, b→a: 4
    b→d: 14-11=3, d→b: 11
    c→t: 20-19=1, t→c: 19
    d→c: 7-7=0, c→d: 7
    d→t: 4-4=0, t→d: 4
    c→b: 9

ITERATION 4:
═══════════
BFS from s finds: s → a → b → d → c (back edge) → ... (stuck)

Try: s → b → d (stuck - no forward capacity)
Try: s → a → b → c (via c→b backward?) 

BFS: s → a → b → c → t
     s ─4─→ a ─10→ b ... wait, need b→c

Actually: s → b → c (backward from c→b=9) → t
But need s → b first: capacity 2
Then b → c? No direct edge. c→b exists with cap 9.

BFS explores:
    s: neighbors a(4), b(2)
    From a: neighbors c(0-blocked), b(10)
    From b: neighbors a(4), d(3)
    ...

Valid path found: s → a → b → c → t  (using a→b forward, c→b backward as b→c)
Wait, c→b means we can push flow FROM c TO b, not the other direction.

Let me redo with residual edges properly:

s → b (cap 2)
b has: b→a(4), b→d(3)
a has: a→b(10), a→s(12) [backward]
...

BFS path: s → a → b → d → t? 
    s→a: 4, a→b: 10, b→d: 3, d→t: 0 BLOCKED

BFS path: s → b → a → c → t?
    s→b: 2, b→a: 4, a→c: 0 BLOCKED

Let me find augmenting path via backward edges:
s → a → c [backward: c→a has cap 12] ... not helpful

After more careful analysis, one more augmentation:
s → b → d → c [using c→d backward=7]... no wait, that doesn't help reach t.

Actually: s → b → c(backward) → t
Need b→c forward, which requires c→b backward flow.
Since c→b = 9 (unused), we can route s→b→c→t if there's flow on c→b to cancel.

But c→b is not yet used, so no backward edge from b to c.

Let me try: s → a → b → c → t using residual properly.

After careful analysis:

ITERATION 4: s → b → a → c → t via backwards
    s→b=2, b→a=4, a→c=0 (blocked)

ITERATION 4: s → a → b → c → t
    Need flow from b to c. c→b=9 original, not yet used.
    Can we use backward edge? Only if there's flow on c→b already.

Since c→b original edge has cap 9 and no flow yet, we CAN use it!
s → a(4) → ... hmm, a has no edge to c with capacity.

Wait, original edges reread:
    c → b (cap 9) exists
    So residual: c→b: 9, b→c: 0 (backward)

For flow to go b→c, we need to have used c→b first... chicken/egg.

Final path check: s → b → c → t
    But b→c doesn't exist! Only c→b.

After iteration 3, we need to check if there are more augmenting paths.

Actually, I'll verify by computing min-cut:

Reachable from s in residual after iteration 3:
    s → a (4 capacity) ✓
    a → b (10) ✓  
    b → d (3) ✓
    b → a (4) 
    d → b (11 backward)
    
    Can we reach t? 
    d → t: 0, d → c: 0
    c is reachable? Only via a→c (0) or d→c (0). NO.
    t is not reachable.

Cut: S = {s, a, b, d}, T = {c, t}

Min cut capacity = edges from S to T:
    a→c: 12
    d→c: 7
    d→t: 4
    Total: 12 + 7 + 4 = 23

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Maximum Flow |f*| = 23                                                    │
│                                                                            │
│  Final flow assignment:                                                    │
│    s→a: 12, s→b: 11                                                       │
│    a→c: 12                                                                │
│    b→d: 11                                                                │
│    d→c: 7, d→t: 4                                                         │
│    c→t: 19                                                                │
│                                                                            │
│  Min cut: ({s,a,b,d}, {c,t}) with capacity 23                             │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q3.2 [PROOF]: Prove the Max-Flow Min-Cut Theorem.                          ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

THEOREM: In any flow network, max flow value = min cut capacity.

PROOF:

PART 1: Any flow ≤ Any cut (Weak Duality)
─────────────────────────────────────────
Let f be any flow and (S, T) be any s-t cut (s ∈ S, t ∈ T).

    |f| = (flow out of s) - (flow into s)
        = (flow out of S) - (flow into S)     [by flow conservation]
        = Σ f(u,v) - Σ f(u,v)                 [u∈S, v∈T for first sum]
                                              [u∈T, v∈S for second sum]
        ≤ Σ c(u,v)                            [since f(u,v) ≤ c(u,v)]
        = c(S, T)
    
    Therefore: |f| ≤ c(S, T) for any flow f and any cut (S, T).
    
    This implies: max|f| ≤ min c(S, T)

PART 2: When Ford-Fulkerson terminates, equality holds (Strong Duality)
───────────────────────────────────────────────────────────────────────
When Ford-Fulkerson terminates, there is no augmenting path in Gf.

Define: S = {v ∈ V : v is reachable from s in Gf}
        T = V - S

Claim: s ∈ S, t ∈ T
    - s ∈ S trivially (s reaches itself)
    - t ∉ S (if t ∈ S, there would be an augmenting path, contradiction)

Claim: For all (u,v) with u ∈ S, v ∈ T: f(u,v) = c(u,v)
    - If f(u,v) < c(u,v), then cf(u,v) > 0
    - This means there's a residual edge u → v
    - Since u ∈ S (reachable from s), v would be reachable too
    - Contradiction since v ∈ T

Claim: For all (u,v) with u ∈ T, v ∈ S: f(u,v) = 0
    - If f(u,v) > 0, then cf(v,u) > 0 (backward edge)
    - Since v ∈ S, u would be reachable via this backward edge
    - Contradiction since u ∈ T

Now compute:
    |f| = Σ f(u,v) - Σ f(u,v)    [u∈S,v∈T and u∈T,v∈S]
        = Σ c(u,v) - 0           [by claims above]
        = c(S, T)

Therefore: max flow = c(S, T) ≥ min cut

Combined with Part 1: max flow = min cut  ∎

VISUAL SUMMARY:
    
    ┌─────────────────────────────────────────────────────────────┐
    │                                                             │
    │     S (reachable from s)        │      T (not reachable)    │
    │     ┌──────────────────┐        │      ┌─────────────────┐  │
    │     │    s ──→ a       │ ══════>│      │    c ──→ t      │  │
    │     │    │     │       │saturated      │    ↑            │  │
    │     │    ↓     ↓       │ edges  │      │    │            │  │
    │     │    b ←── d       │        │      └────│────────────┘  │
    │     └──────────────────┘        │           │               │
    │                                 │     no backward           │
    │                                 │     flow to S             │
    └─────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q3.3 [APPLICATION]: Reduce bipartite matching to max flow. Given bipartite║
║       graph with L={1,2,3}, R={A,B,C}, edges: (1,A),(1,B),(2,B),(3,B),(3,C)║
║       Find maximum matching.                                               ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Construct flow network
─────────────────────────────
Add source s connected to all left vertices (capacity 1)
Add sink t connected from all right vertices (capacity 1)
Original edges have capacity 1

    Network:
    
              1             1
         s ────→ 1 ────→ A ────→ t
          \      │╲      ↑      ↗
         1 \     │ ╲1    │1    / 1
            \    │  ╲    │    /
             ╲   ↓   ╲   │   /
         s ────→ 2 ───→ B ────→ t
              1     1   ↑      ↗
                       │1    / 1
                       │    /
              1     1  │   /
         s ────→ 3 ───→─┘  /
              \          /
             1 \   1    / 1
                ╲      /
         s ────→ 3 ──→ C ────→ t

    Simplified:
    
              ┌──→ A ──┐
              │        │
    s ──→ 1 ──┼──→ B ──┼──→ t
              │   ↑    │
    s ──→ 2 ──┘   │    │
                  │    │
    s ──→ 3 ──────┴──→ C ──→ t

Step 2: Run Ford-Fulkerson
──────────────────────────

Iteration 1: Path s → 1 → A → t
    Augment by 1
    Flow: f(s,1)=1, f(1,A)=1, f(A,t)=1

Iteration 2: Path s → 2 → B → t
    Augment by 1
    Flow: f(s,2)=1, f(2,B)=1, f(B,t)=1

Iteration 3: Path s → 3 → C → t
    Augment by 1
    Flow: f(s,3)=1, f(3,C)=1, f(C,t)=1

Iteration 4: No more augmenting paths (all s→L edges saturated)

Step 3: Extract matching from flow
──────────────────────────────────
Edges with flow = 1:
    (1, A), (2, B), (3, C)

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Maximum matching: {(1,A), (2,B), (3,C)}                                  │
│  Size: 3 (perfect matching!)                                              │
│                                                                            │
│  Visualization:                                                            │
│      1 ═══ A                                                              │
│      2 ═══ B                                                              │
│      3 ═══ C                                                              │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 4: AUCTIONS (FIRST-PRICE, SECOND-PRICE, GSP)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q4.1 [PROOF]: Prove that truthful bidding is weakly dominant in Vickrey   ║
║       (second-price) auctions.                                             ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

SETUP:
    - Bidder has true private value v
    - Bidder considers bidding b (possibly ≠ v)
    - h = highest bid among other bidders

CLAIM: Bidding b = v is weakly dominant (always at least as good as any other bid)

PROOF BY CASE ANALYSIS:

    ┌─────────────────────────────────────────────────────────────────────────┐
    │  CASE 1: v > h (Bidder would win by bidding truthfully)                │
    │                                                                         │
    │  Subcase 1a: Bid b ≥ h (still wins)                                    │
    │      Winner: Yes                                                        │
    │      Payment: h (second-highest)                                        │
    │      Utility: v - h > 0                                                │
    │      Same as bidding v ✓                                               │
    │                                                                         │
    │  Subcase 1b: Bid b < h (loses!)                                        │
    │      Winner: No                                                         │
    │      Utility: 0                                                         │
    │      WORSE than bidding v (would have gotten v - h > 0) ✗              │
    └─────────────────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────────────────┐
    │  CASE 2: v < h (Bidder would lose by bidding truthfully)               │
    │                                                                         │
    │  Subcase 2a: Bid b ≤ h (still loses)                                   │
    │      Winner: No                                                         │
    │      Utility: 0                                                         │
    │      Same as bidding v ✓                                               │
    │                                                                         │
    │  Subcase 2b: Bid b > h (wins!)                                         │
    │      Winner: Yes                                                        │
    │      Payment: h                                                         │
    │      Utility: v - h < 0 (NEGATIVE!)                                    │
    │      WORSE than bidding v (would have gotten 0) ✗                      │
    └─────────────────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────────────────┐
    │  CASE 3: v = h (Tie case)                                              │
    │                                                                         │
    │  Any bid: Either win with utility v - h = 0, or lose with utility 0    │
    │  Indifferent ✓                                                         │
    └─────────────────────────────────────────────────────────────────────────┘

CONCLUSION:
    In every case, bidding b = v gives utility ≥ any other bid.
    Therefore, truthful bidding is WEAKLY DOMINANT.  ∎

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q4.2 [NUMERICAL]: 4 bidders with values v₁=100, v₂=80, v₃=60, v₄=40.     ║
║       Compare outcomes in First-Price vs Second-Price auctions.            ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

SECOND-PRICE (VICKREY) AUCTION:
───────────────────────────────
In equilibrium: Everyone bids truthfully

    Bidder 1: bids 100
    Bidder 2: bids 80
    Bidder 3: bids 60
    Bidder 4: bids 40

    Winner: Bidder 1 (highest bid)
    Payment: 80 (second-highest bid)
    
    Utilities:
        u₁ = 100 - 80 = 20
        u₂ = u₃ = u₄ = 0
    
    Seller Revenue: $80

FIRST-PRICE AUCTION (assuming Uniform[0, 100], using b(v) = (n-1)/n × v):
─────────────────────────────────────────────────────────────────────────

With n = 4 bidders: b(v) = 3v/4

    Bidder 1: bids 3(100)/4 = 75
    Bidder 2: bids 3(80)/4 = 60
    Bidder 3: bids 3(60)/4 = 45
    Bidder 4: bids 3(40)/4 = 30

    Winner: Bidder 1 (highest bid)
    Payment: 75 (own bid)
    
    Utilities:
        u₁ = 100 - 75 = 25
        u₂ = u₃ = u₄ = 0
    
    Seller Revenue: $75

┌────────────────────────────────────────────────────────────────────────────┐
│  COMPARISON:                                                               │
│                                                                            │
│                      │  Second-Price  │  First-Price  │                   │
│  ────────────────────┼────────────────┼───────────────┤                   │
│  Winner              │   Bidder 1     │   Bidder 1    │                   │
│  Winner's payment    │      $80       │      $75      │                   │
│  Winner's utility    │      $20       │      $25      │                   │
│  Seller revenue      │      $80       │      $75      │                   │
│  Dominant strategy?  │      YES       │       NO      │                   │
│                                                                            │
│  Note: In expectation (Revenue Equivalence), both yield same revenue!     │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q4.3 [NUMERICAL]: Sponsored Search Auction (GSP). 3 slots with CTRs       ║
║       α₁=1.0, α₂=0.75, α₃=0.5. Advertisers bid: A=$10, B=$8, C=$6, D=$4. ║
║       Find allocation and payments.                                        ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

GSP RULE:
    1. Rank advertisers by bid (highest first)
    2. Assign to slots in order
    3. Each advertiser pays the bid of the next advertiser below

Step 1: Rank by bid
    A: $10 → Slot 1
    B: $8  → Slot 2
    C: $6  → Slot 3
    D: $4  → No slot (4th place)

Step 2: Compute payments (pay next advertiser's bid)
    Slot 1 (A): pays B's bid = $8 per click
    Slot 2 (B): pays C's bid = $6 per click
    Slot 3 (C): pays D's bid = $4 per click

Step 3: Compute total payments (payment × CTR)
    A pays: $8 × 1.0 = $8.00 per impression
    B pays: $6 × 0.75 = $4.50 per impression
    C pays: $4 × 0.5 = $2.00 per impression

Step 4: Compute utilities (assume value = bid for simplicity)
    Actually, let's say values are v_A=$12, v_B=$10, v_C=$8 (higher than bids)
    
    u_A = (v_A - payment per click) × CTR = (12 - 8) × 1.0 = $4.00
    u_B = (v_B - payment per click) × CTR = (10 - 6) × 0.75 = $3.00
    u_C = (v_C - payment per click) × CTR = (8 - 4) × 0.5 = $2.00

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  ┌─────────────┬──────┬─────────┬──────────────┬─────────────────────┐    │
│  │ Advertiser  │ Slot │  CTR    │ Pays/click   │ Total payment       │    │
│  ├─────────────┼──────┼─────────┼──────────────┼─────────────────────┤    │
│  │     A       │  1   │  1.00   │    $8        │  $8.00/impression   │    │
│  │     B       │  2   │  0.75   │    $6        │  $4.50/impression   │    │
│  │     C       │  3   │  0.50   │    $4        │  $2.00/impression   │    │
│  │     D       │  -   │   -     │     -        │       -             │    │
│  └─────────────┴──────┴─────────┴──────────────┴─────────────────────┘    │
│                                                                            │
│  Total seller revenue per impression: $8 + $4.50 + $2 = $14.50            │
│                                                                            │
│  Note: GSP is NOT truthful! Advertisers may benefit from bid shading.     │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 5: MYERSON'S LEMMA
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q5.1 [NUMERICAL]: Bidders have values i.i.d. from Uniform[0, 100].        ║
║       Compute virtual value φ(v) and find optimal reserve price.           ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

For Uniform[0, 100]:
    F(v) = v/100  (CDF)
    f(v) = 1/100  (PDF)

Virtual value formula:
    φ(v) = v - (1 - F(v))/f(v)
         = v - (1 - v/100)/(1/100)
         = v - 100(1 - v/100)
         = v - (100 - v)
         = 2v - 100

Virtual value table:
    ┌───────┬─────────┬──────────────────────────────┐
    │   v   │  φ(v)   │         Interpretation       │
    ├───────┼─────────┼──────────────────────────────┤
    │   0   │  -100   │ Negative (don't sell to)     │
    │  25   │   -50   │ Negative                     │
    │  50   │    0    │ Break-even (reserve price!)  │
    │  75   │   50    │ Positive (sell to)           │
    │ 100   │  100    │ Positive                     │
    └───────┴─────────┴──────────────────────────────┘

    φ(v)
    100 ┤                        ╱
        │                      ╱
     50 ┤                    ╱
        │                  ╱
      0 ┼────────────────╳─────────→ v
        │              ╱   50
    -50 ┤            ╱
        │          ╱
   -100 ┤        ╱
              0

Optimal reserve price:
    Set φ(v*) = 0
    2v* - 100 = 0
    v* = 50

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Virtual value: φ(v) = 2v - 100                                           │
│  Optimal reserve price: v* = 50                                           │
│                                                                            │
│  Optimal Auction Rule:                                                     │
│    • If max(v₁, v₂, ..., vₙ) < 50: NO SALE                                │
│    • Otherwise: Sell to highest bidder                                     │
│    • Charge: max(50, second-highest bid)                                  │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q5.2 [NUMERICAL]: Two bidders with values v₁=70, v₂=40 from Uniform[0,100]║
║       Apply Myerson's optimal auction. Who wins? What's the payment?       ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Compute virtual values
    φ(v₁) = φ(70) = 2(70) - 100 = 40
    φ(v₂) = φ(40) = 2(40) - 100 = -20

Step 2: Check reserve price condition
    Reserve price = 50 (from Q5.1)
    
    v₁ = 70 ≥ 50 ✓ (above reserve)
    v₂ = 40 < 50 ✗ (below reserve)

Step 3: Determine winner
    Only bidder 1 has v ≥ reserve price
    Winner: Bidder 1

Step 4: Compute payment
    Bidder 1 pays the THRESHOLD to win:
    
    Threshold = max(reserve, highest other bid that's ≥ reserve)
              = max(50, -) 
              = 50  (since bidder 2 is below reserve)

    Alternatively, using payment formula:
    p₁ = ∫₀^{v₁} [indicator that bidder 1 wins at value z] dz
       = ∫₅₀^{70} 1 dz   (wins only when z ≥ 50)
       = 70 - 50 = 20... 
       
    Wait, that's the utility. Payment = v - utility = 70 - 20 = 50. ✓

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Winner: Bidder 1                                                          │
│  Payment: $50 (the reserve price)                                          │
│  Bidder 1's utility: 70 - 50 = $20                                        │
│  Bidder 2's utility: $0 (lost)                                            │
│  Seller revenue: $50                                                       │
│                                                                            │
│  Note: Without reserve, Vickrey would charge $40, losing $10 revenue!     │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q5.3 [PROOF]: State Myerson's Lemma and derive the payment formula.        ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

MYERSON'S LEMMA:

Part 1 (Monotonicity):
    An allocation rule x(·) is implementable (has a truthful payment rule)
    if and only if it is MONOTONE:
    
        xᵢ(z, v₋ᵢ) is non-decreasing in z
    
    (Higher bid → higher probability of winning)

Part 2 (Uniqueness):
    If x(·) is monotone, there is a UNIQUE payment rule p(·) that makes
    (x, p) a truthful mechanism (up to an additive constant).

Part 3 (Payment Formula):
    The payment for a monotone allocation x is:
    
    ┌─────────────────────────────────────────────────────────────────────┐
    │                         vᵢ                                          │
    │   pᵢ(vᵢ) = vᵢ·xᵢ(vᵢ) - ∫  xᵢ(z) dz                                │
    │                         0                                           │
    │                                                                     │
    │   (assuming pᵢ(0) = 0)                                             │
    └─────────────────────────────────────────────────────────────────────┘

DERIVATION OF PAYMENT FORMULA:

For truthfulness, bidder with value v̂ reporting v must get:
    u(v̂, v) = v̂ · x(v) - p(v)

For truthful reporting to be optimal:
    ∂u(v̂, v)/∂v |_{v=v̂} = 0
    
    v̂ · x'(v̂) - p'(v̂) = 0
    p'(v̂) = v̂ · x'(v̂)

Integrate both sides:
    p(v) - p(0) = ∫₀^v z · x'(z) dz

Integration by parts: ∫ z·x'(z) dz = z·x(z) - ∫ x(z) dz

    p(v) = v·x(v) - ∫₀^v x(z) dz + p(0)

Setting p(0) = 0:
    
    ┌─────────────────────────────────────────────────────────────────────┐
    │   p(v) = v·x(v) - ∫₀^v x(z) dz                                     │
    │                                                                     │
    │   Interpretation: Payment = (value × allocation) - utility          │
    │                   where utility = ∫₀^v x(z) dz                     │
    └─────────────────────────────────────────────────────────────────────┘

VISUAL INTERPRETATION:

    x(v)
    1 ┤           ┌──────────  (allocation jumps to 1 at threshold)
      │           │
      │           │
      │    Area = │
      │   Utility │
      │           │
    0 ┼───────────┴──────────→ v
      0         v*       v
    
    Payment = v·x(v) - shaded area
            = v·1 - (v - v*)
            = v*
    
    The bidder pays the THRESHOLD value v* at which they start winning!  ∎

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 6: STABLE MATCHING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q6.1 [NUMERICAL]: Run Gale-Shapley on the following preferences:           ║
║       Men: m1: w2>w1>w3, m2: w1>w2>w3, m3: w1>w3>w2                        ║
║       Women: w1: m1>m2>m3, w2: m3>m1>m2, w3: m1>m3>m2                      ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Preference Tables:

    Men's Preferences:           Women's Preferences:
    ┌─────┬─────────────┐       ┌─────┬─────────────┐
    │ m1  │ w2 > w1 > w3│       │ w1  │ m1 > m2 > m3│
    │ m2  │ w1 > w2 > w3│       │ w2  │ m3 > m1 > m2│
    │ m3  │ w1 > w3 > w2│       │ w3  │ m1 > m3 > m2│
    └─────┴─────────────┘       └─────┴─────────────┘

ROUND 1: All men propose to their first choice
══════════════════════════════════════════════

    m1 proposes to w2 (his #1)
    m2 proposes to w1 (his #1)
    m3 proposes to w1 (his #1)

    w2 receives: {m1}
        → Accepts m1 (only proposal)
    
    w1 receives: {m2, m3}
        w1's preference: m1 > m2 > m3
        → Accepts m2 (prefers m2 over m3)
        → Rejects m3

    Status after Round 1:
        m1 ═══ w2 (engaged)
        m2 ═══ w1 (engaged)
        m3 --- FREE

ROUND 2: Free men propose to next choice
═════════════════════════════════════════

    m3 proposes to w3 (his #2, since w1 rejected him)
    
    w3 receives: {m3}
        → Accepts m3 (only proposal)

    Status after Round 2:
        m1 ═══ w2
        m2 ═══ w1
        m3 ═══ w3
        
    ALL MATCHED!

FINAL MATCHING:
───────────────

    ┌────────────────────────────────────────────┐
    │                                            │
    │     m1 ═══════════ w2                     │
    │                                            │
    │     m2 ═══════════ w1                     │
    │                                            │
    │     m3 ═══════════ w3                     │
    │                                            │
    └────────────────────────────────────────────┘

STABILITY VERIFICATION:
───────────────────────
Check all non-matched pairs for blocking:

    (m1, w1): m1 has w2. m1's pref: w2 > w1. m1 doesn't prefer w1. NOT BLOCKING ✓
    
    (m1, w3): m1 has w2. m1's pref: w2 > w1 > w3. m1 doesn't prefer w3. NOT BLOCKING ✓
    
    (m2, w2): m2 has w1. m2's pref: w1 > w2. m2 doesn't prefer w2. NOT BLOCKING ✓
    
    (m2, w3): m2 has w1. m2's pref: w1 > w2 > w3. m2 doesn't prefer w3. NOT BLOCKING ✓
    
    (m3, w1): m3 has w3. m3's pref: w1 > w3. m3 DOES prefer w1!
             But w1 has m2. w1's pref: m1 > m2 > m3. w1 doesn't prefer m3. NOT BLOCKING ✓
    
    (m3, w2): m3 has w3. m3's pref: w1 > w3 > w2. m3 doesn't prefer w2. NOT BLOCKING ✓

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: Stable matching = {(m1,w2), (m2,w1), (m3,w3)}                     │
│                                                                            │
│  Properties:                                                               │
│  • Man-optimal: Each man gets his best achievable partner                 │
│  • Woman-pessimal: Each woman gets her worst stable partner               │
│  • m1 gets w2 (his #1), m2 gets w1 (his #1), m3 gets w3 (his #2)         │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q6.2 [PROOF]: Prove that Gale-Shapley always produces a stable matching.   ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

THEOREM: The Gale-Shapley algorithm outputs a stable matching.

PROOF:

Part 1: The output is a valid matching (everyone matched)
─────────────────────────────────────────────────────────
Claim: No man is left unmatched at termination.

Suppose man m is unmatched when algorithm terminates.
    → m must have been rejected by all n women
    → Each woman who rejected m must be engaged to someone she prefers
    → Since women never become unengaged (only upgrade), all n women are matched
    → But then all n men must be matched (|matched men| = |matched women| = n)
    → Contradiction!

Therefore, everyone is matched. ✓

Part 2: The matching is stable (no blocking pairs)
──────────────────────────────────────────────────
Suppose for contradiction that (m, w) is a blocking pair in output μ.

Let μ(m) = w' and μ(w) = m'.

Since (m, w) is blocking:
    (i)  m prefers w to w'
    (ii) w prefers m to m'

From (i): m prefers w to w'
    → m proposed to w before proposing to w' (men propose in preference order)
    → At some point, m proposed to w

When m proposed to w, one of two things happened:
    Case A: w rejected m immediately (she had someone better)
    Case B: w accepted m, then later rejected m for someone better

In both cases:
    → w had a partner she prefers to m at the time of rejection
    → Women never "downgrade" — they only accept better proposals
    → w's final partner m' is at least as preferred as whoever caused the rejection
    → w prefers m' to m

But this contradicts (ii)!

Therefore, no blocking pair exists. ∎

VISUAL PROOF SUMMARY:
────────────────────

    Timeline of m's proposals:
    
    m proposes: w₁ → w₂ → ... → w → ... → w' = μ(m)
                (rejected)    (rejected)  (final partner)
                
    Since m prefers w to w', m proposed to w first.
    
    When m proposed to w:
        w was engaged to m'' who she prefers to m (or rejected for m'' later)
        w's final partner m' ≥ m'' > m (in w's preference)
        
    Contradiction: (ii) says w prefers m to m', but we showed m' > m.

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q6.3 [CONCEPTUAL]: Explain why GS is man-optimal and woman-pessimal.       ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

DEFINITION: 
    Let S = set of all stable matchings
    For man m, let best(m) = best partner m can get in any stable matching
    For woman w, let worst(w) = worst partner w can get in any stable matching

CLAIM 1: GS gives each man his best stable partner
──────────────────────────────────────────────────
Proof by contradiction:

Suppose some man m is rejected by best(m) = w during GS.
Let this be the FIRST such rejection (first time any man is rejected by his best).

When m was rejected by w:
    → w preferred another man m' to m
    → m' had proposed to w before proposing to anyone he likes less

Since this is the first "best-partner rejection":
    → m' has not yet been rejected by anyone he prefers to w
    → w is at least as good as best(m') for m'
    → But m' proposed to w, so w ≤ best(m')
    → Therefore w = best(m')

Now consider the pair (m, w):
    → m's best stable partner is w
    → So there exists a stable matching μ where μ(m) = w
    
In matching μ:
    → w is matched to m
    → But w prefers m' (from GS), and m' considers w as good as his best
    → (m', w) could block μ!
    
This contradicts μ being stable. ∎

CLAIM 2: GS gives each woman her worst stable partner
─────────────────────────────────────────────────────
Proof:

Let μ_GS be the GS output and μ_S be any other stable matching.
For woman w, let μ_GS(w) = m and μ_S(w) = m'.

Want to show: w prefers m' to m (or is indifferent), i.e., m is worst.

Consider pair (m, w') where w' = μ_S(m) in the other matching.

In μ_GS: m is matched to w = best(m) (by Claim 1)
In μ_S:  m is matched to w'

Since w = best(m): m prefers w to w' (or equal)

If w also preferred m to m' = μ_S(w):
    → (m, w) would block μ_S
    → Contradiction since μ_S is stable

Therefore: w does NOT prefer m to m'
          → w weakly prefers m' to m
          → m is among w's worst stable partners ∎

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 7: NASH EQUILIBRIUM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q7.1 [NUMERICAL]: Find all Nash equilibria (pure and mixed) for:           ║
║                          Player 2                                          ║
║                      L           R                                         ║
║        Player 1  ┌─────────┬─────────┐                                    ║
║               T  │ (3, 3)  │ (0, 5)  │                                    ║
║                  ├─────────┼─────────┤                                    ║
║               B  │ (5, 0)  │ (1, 1)  │                                    ║
║                  └─────────┴─────────┘                                    ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Find Pure Nash Equilibria
─────────────────────────────────

Check each cell:

(T, L) = (3, 3):
    P1 deviates to B: gets 5 > 3. Can improve! NOT NE ✗

(T, R) = (0, 5):
    P1 deviates to B: gets 1 > 0. Can improve! NOT NE ✗

(B, L) = (5, 0):
    P1 deviates to T: gets 3 < 5. No improvement.
    P2 deviates to R: gets 1 > 0. Can improve! NOT NE ✗

(B, R) = (1, 1):
    P1 deviates to T: gets 0 < 1. No improvement.
    P2 deviates to L: gets 0 < 1. No improvement.
    NASH EQUILIBRIUM ✓

Pure NE: {(B, R)}

Step 2: Find Mixed Nash Equilibrium
───────────────────────────────────

Let P1 play T with probability p, B with probability (1-p)
Let P2 play L with probability q, R with probability (1-q)

For P2 to be indifferent (willing to mix):
    E[P2 plays L] = E[P2 plays R]
    
    P2's payoff from L: 3p + 0(1-p) = 3p
    P2's payoff from R: 5p + 1(1-p) = 5p + 1 - p = 4p + 1
    
    Set equal: 3p = 4p + 1
              -p = 1
               p = -1  ???
    
    This is invalid! (probability can't be negative)
    
Let me recalculate...

    P2's payoff from L: p·3 + (1-p)·0 = 3p
    P2's payoff from R: p·5 + (1-p)·1 = 5p + 1 - p = 4p + 1
    
    For P2 indifferent: 3p = 4p + 1 → p = -1 (impossible)

This means P2 strictly prefers R for any p ∈ [0,1]!
    (4p + 1 > 3p for all p ≥ 0)

Let's verify:
    If p = 0: L gives 0, R gives 1 → R better
    If p = 1: L gives 3, R gives 5 → R better
    
P2 always prefers R! So in any equilibrium, P2 plays R.

Given P2 plays R:
    P1's payoff from T: 0
    P1's payoff from B: 1
    P1 prefers B.

So the ONLY equilibrium is (B, R)!

But wait, let me check for P1's indifference condition too:

For P1 to be indifferent:
    E[P1 plays T] = E[P1 plays B]
    
    P1's payoff from T: 3q + 0(1-q) = 3q
    P1's payoff from B: 5q + 1(1-q) = 5q + 1 - q = 4q + 1
    
    Set equal: 3q = 4q + 1
               q = -1 (impossible!)

Similarly, P1 strictly prefers B for any q!

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  This game has a UNIQUE Nash Equilibrium:                                  │
│                                                                            │
│  Pure NE: (B, R) with payoffs (1, 1)                                      │
│  Mixed NE: None (both players have dominant strategies!)                  │
│                                                                            │
│  Note: B strictly dominates T for P1                                      │
│        R strictly dominates L for P2                                      │
│        This is a Prisoner's Dilemma variant!                              │
│        (3,3) Pareto dominates (1,1) but is not stable                    │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q7.2 [NUMERICAL]: Compute mixed NE for Matching Pennies:                   ║
║                          Player 2                                          ║
║                      H           T                                         ║
║        Player 1  ┌─────────┬─────────┐                                    ║
║               H  │ (1, -1) │ (-1, 1) │                                    ║
║                  ├─────────┼─────────┤                                    ║
║               T  │ (-1, 1) │ (1, -1) │                                    ║
║                  └─────────┴─────────┘                                    ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Check for Pure NE
─────────────────────────
(H, H): P2 deviates to T: -1 → 1. Improves! NOT NE
(H, T): P1 deviates to T: -1 → 1. Improves! NOT NE
(T, H): P1 deviates to H: -1 → 1. Improves! NOT NE
(T, T): P2 deviates to H: -1 → 1. Improves! NOT NE

NO PURE NASH EQUILIBRIUM!

Step 2: Find Mixed NE
─────────────────────
Let p = P(P1 plays H), q = P(P2 plays H)

For P1 to be indifferent (willing to randomize):
    E[u₁ | H] = E[u₁ | T]
    
    E[u₁ | H] = q(1) + (1-q)(-1) = q - 1 + q = 2q - 1
    E[u₁ | T] = q(-1) + (1-q)(1) = -q + 1 - q = 1 - 2q
    
    Set equal: 2q - 1 = 1 - 2q
              4q = 2
              q = 1/2

For P2 to be indifferent:
    E[u₂ | H] = E[u₂ | T]
    
    E[u₂ | H] = p(-1) + (1-p)(1) = -p + 1 - p = 1 - 2p
    E[u₂ | T] = p(1) + (1-p)(-1) = p - 1 + p = 2p - 1
    
    Set equal: 1 - 2p = 2p - 1
              2 = 4p
              p = 1/2

Step 3: Verify and compute expected payoffs
───────────────────────────────────────────
At p = q = 1/2:

E[u₁] = p·q·(1) + p·(1-q)·(-1) + (1-p)·q·(-1) + (1-p)·(1-q)·(1)
      = (1/4)(1) + (1/4)(-1) + (1/4)(-1) + (1/4)(1)
      = 1/4 - 1/4 - 1/4 + 1/4
      = 0

Similarly, E[u₂] = 0 (zero-sum game)

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Mixed Nash Equilibrium:                                                   │
│    P1 plays H with probability 1/2, T with probability 1/2                │
│    P2 plays H with probability 1/2, T with probability 1/2                │
│                                                                            │
│  Expected payoffs: E[u₁] = 0, E[u₂] = 0                                   │
│                                                                            │
│  Diagram:                                                                  │
│       q (P2 plays H)                                                      │
│    1  ┤────────────────                                                   │
│       │ P1 prefers T  │                                                   │
│   1/2 ┤ ─ ─ ─ ● ─ ─ ─ ← Nash equilibrium (1/2, 1/2)                      │
│       │ P1 prefers H  │                                                   │
│    0  ┼───────────────┴────→ p (P1 plays H)                              │
│       0      1/2      1                                                   │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q7.3 [NUMERICAL]: Find all NE for Battle of the Sexes:                     ║
║                          Player 2 (Wife)                                   ║
║                    Opera       Football                                    ║
║     Player 1   ┌───────────┬───────────┐                                  ║
║     (Husband)  │  (3, 2)   │  (0, 0)   │  Opera                          ║
║                ├───────────┼───────────┤                                  ║
║                │  (0, 0)   │  (2, 3)   │  Football                       ║
║                └───────────┴───────────┘                                  ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Find Pure NE
────────────────────
(Opera, Opera) = (3, 2):
    Husband deviates to Football: 0 < 3. No improvement.
    Wife deviates to Football: 0 < 2. No improvement.
    NASH EQUILIBRIUM ✓

(Opera, Football) = (0, 0):
    Husband deviates to Football: 2 > 0. Improves! NOT NE

(Football, Opera) = (0, 0):
    Wife deviates to Football: 3 > 0. Improves! NOT NE

(Football, Football) = (2, 3):
    Husband deviates to Opera: 0 < 2. No improvement.
    Wife deviates to Opera: 0 < 3. No improvement.
    NASH EQUILIBRIUM ✓

Pure NE: {(Opera, Opera), (Football, Football)}

Step 2: Find Mixed NE
─────────────────────
Let p = P(Husband plays Opera)
Let q = P(Wife plays Opera)

Husband indifferent:
    E[Opera] = E[Football]
    3q + 0(1-q) = 0·q + 2(1-q)
    3q = 2 - 2q
    5q = 2
    q = 2/5

Wife indifferent:
    E[Opera] = E[Football]
    2p + 0(1-p) = 0·p + 3(1-p)
    2p = 3 - 3p
    5p = 3
    p = 3/5

Expected payoffs at mixed NE:
    E[u₁] = 3q = 3(2/5) = 6/5 = 1.2
    E[u₂] = 2p = 2(3/5) = 6/5 = 1.2

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: Three Nash Equilibria                                             │
│                                                                            │
│  1. Pure NE: (Opera, Opera)                                               │
│     Payoffs: (3, 2)                                                       │
│     Husband-preferred equilibrium                                          │
│                                                                            │
│  2. Pure NE: (Football, Football)                                         │
│     Payoffs: (2, 3)                                                       │
│     Wife-preferred equilibrium                                             │
│                                                                            │
│  3. Mixed NE: p = 3/5, q = 2/5                                            │
│     Husband: Opera w.p. 3/5, Football w.p. 2/5                            │
│     Wife: Opera w.p. 2/5, Football w.p. 3/5                               │
│     Payoffs: (1.2, 1.2)                                                   │
│     Note: Mixed NE has LOWER payoffs for both! (coordination failure)     │
│                                                                            │
│  Visualization:                                                            │
│       q                                                                    │
│    1  ┤─●────────────── (Opera, Opera) = pure NE                          │
│       │  ╲                                                                │
│   2/5 ┤───●──────────── Mixed NE (3/5, 2/5)                              │
│       │    ╲                                                              │
│    0  ┼─────────●────── (Football, Football) = pure NE                   │
│       0   3/5   1  p                                                      │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 8: PUT-CALL PARITY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q8.1 [NUMERICAL]: Given S₀ = $50, K = $52, r = 6%, T = 6 months,          ║
║       C = $3. Find the put price P using put-call parity.                  ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Given:
    S₀ = $50 (current stock price)
    K = $52 (strike price)
    r = 6% = 0.06 (annual risk-free rate)
    T = 6 months = 0.5 years
    C = $3 (call price)

Put-Call Parity Formula:
    C - P = S₀ - K·e^(-rT)

Rearrange for P:
    P = C - S₀ + K·e^(-rT)

Step 1: Calculate K·e^(-rT)
    K·e^(-rT) = 52 × e^(-0.06 × 0.5)
              = 52 × e^(-0.03)
              = 52 × 0.97045
              = $50.46

Step 2: Calculate P
    P = C - S₀ + K·e^(-rT)
    P = 3 - 50 + 50.46
    P = $3.46

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER: Put price P = $3.46                                               │
│                                                                            │
│  Verification:                                                             │
│    C - P = 3 - 3.46 = -0.46                                               │
│    S₀ - K·e^(-rT) = 50 - 50.46 = -0.46 ✓                                  │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q8.2 [NUMERICAL]: S₀ = $100, K = $100, r = 5%, T = 1 year.                ║
║       Call price C = $12, Put price P = $5. Is there arbitrage?           ║
║       If yes, construct the arbitrage strategy.                            ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Step 1: Check Put-Call Parity
─────────────────────────────

Left side: C - P = 12 - 5 = $7

Right side: S₀ - K·e^(-rT) = 100 - 100·e^(-0.05×1)
                           = 100 - 100×0.9512
                           = 100 - 95.12
                           = $4.88

    C - P = $7.00
    S₀ - K·e^(-rT) = $4.88
    
    7.00 ≠ 4.88 → PARITY VIOLATED!

Step 2: Identify mispricing direction
─────────────────────────────────────
    C - P > S₀ - K·e^(-rT)
    
    This means: Call is OVERPRICED relative to put
                (or equivalently, put is UNDERPRICED)

Step 3: Construct Arbitrage Strategy
────────────────────────────────────

    ┌─────────────────────────────────────────────────────────────────────┐
    │  TODAY (t = 0):                                                     │
    │                                                                     │
    │  • SELL (write) the call: receive $12                              │
    │  • BUY the put: pay $5                                             │
    │  • BUY the stock: pay $100                                         │
    │  • BORROW K·e^(-rT) = $95.12 at risk-free rate                     │
    │                                                                     │
    │  Net cash flow: +12 - 5 - 100 + 95.12 = +$2.12                     │
    │                                                                     │
    │  This is RISK-FREE PROFIT at t=0!                                  │
    └─────────────────────────────────────────────────────────────────────┘

    ┌─────────────────────────────────────────────────────────────────────┐
    │  AT EXPIRATION (t = T):                                             │
    │                                                                     │
    │  Case A: S_T > K (stock price > $100)                              │
    │    • Call is exercised against you: sell stock for K = $100        │
    │    • Put expires worthless                                         │
    │    • Repay loan: $95.12 × e^(0.05) = $100                         │
    │    Net: $100 - $100 = $0                                           │
    │                                                                     │
    │  Case B: S_T < K (stock price < $100)                              │
    │    • Call expires worthless                                        │
    │    • Exercise put: sell stock for K = $100                         │
    │    • Repay loan: $100                                              │
    │    Net: $100 - $100 = $0                                           │
    │                                                                     │
    │  Case C: S_T = K (stock price = $100)                              │
    │    • Both options at-the-money (exercise either)                   │
    │    Net: $0                                                          │
    │                                                                     │
    │  IN ALL CASES: Net cash flow at T = $0                             │
    └─────────────────────────────────────────────────────────────────────┘

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  YES, there is an arbitrage opportunity!                                   │
│                                                                            │
│  Arbitrage profit: $2.12 (received today, risk-free)                      │
│                                                                            │
│  Strategy summary:                                                         │
│    • Sell call (+$12)                                                     │
│    • Buy put (-$5)                                                        │
│    • Buy stock (-$100)                                                    │
│    • Borrow PV(K) (+$95.12)                                               │
│    ─────────────────────────                                              │
│    Net: +$2.12 profit                                                     │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q8.3 [PROOF]: Derive Put-Call Parity using no-arbitrage argument.          ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

SETUP: European call and put with same strike K and expiration T

PORTFOLIO A: Long Call + K·e^(-rT) in risk-free bonds
PORTFOLIO B: Long Put + Long Stock

CLAIM: Portfolios A and B have identical payoffs at time T

PROOF:

At time T, the bond grows to K·e^(-rT) × e^(rT) = K

    ┌───────────────────────────────────────────────────────────────────────┐
    │  CASE 1: S_T > K (stock finishes above strike)                       │
    │                                                                       │
    │  Portfolio A:                                                         │
    │    Call payoff: max(S_T - K, 0) = S_T - K                            │
    │    Bond value: K                                                      │
    │    Total: (S_T - K) + K = S_T                                        │
    │                                                                       │
    │  Portfolio B:                                                         │
    │    Put payoff: max(K - S_T, 0) = 0                                   │
    │    Stock value: S_T                                                   │
    │    Total: 0 + S_T = S_T                                              │
    │                                                                       │
    │  A = B = S_T ✓                                                       │
    └───────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────────────────────────────────────────────────┐
    │  CASE 2: S_T < K (stock finishes below strike)                       │
    │                                                                       │
    │  Portfolio A:                                                         │
    │    Call payoff: max(S_T - K, 0) = 0                                  │
    │    Bond value: K                                                      │
    │    Total: 0 + K = K                                                  │
    │                                                                       │
    │  Portfolio B:                                                         │
    │    Put payoff: max(K - S_T, 0) = K - S_T                             │
    │    Stock value: S_T                                                   │
    │    Total: (K - S_T) + S_T = K                                        │
    │                                                                       │
    │  A = B = K ✓                                                         │
    └───────────────────────────────────────────────────────────────────────┘

    ┌───────────────────────────────────────────────────────────────────────┐
    │  CASE 3: S_T = K (at-the-money at expiration)                        │
    │                                                                       │
    │  Portfolio A: 0 + K = K                                              │
    │  Portfolio B: 0 + K = K                                              │
    │                                                                       │
    │  A = B = K ✓                                                         │
    └───────────────────────────────────────────────────────────────────────┘

BY NO-ARBITRAGE:
    Since A and B have identical payoffs in ALL states at time T,
    they must have identical prices at time 0.

    Price(A) = Price(B)
    C + K·e^(-rT) = P + S₀
    
    ┌─────────────────────────────────────────┐
    │                                         │
    │      C - P = S₀ - K·e^(-rT)            │
    │                                         │
    │      PUT-CALL PARITY  ∎                │
    │                                         │
    └─────────────────────────────────────────┘

INTUITION DIAGRAM:

    Payoff at T
         ↑
         │         ╱ Portfolio A (Call + Bond)
         │        ╱
       K ├───────●───────────
         │      ╱│
         │     ╱ │
         │    ╱  │    Portfolio B (Put + Stock)
         │   ╱   │   ╱
         │  ╱    │  ╱
         │ ╱     │ ╱
         ├───────┼╱─────────────→ S_T
         0       K
    
    Both portfolios have the same "hockey stick" payoff!
    - Below K: worth K
    - Above K: worth S_T

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 9: PERCEPTRON LEARNING ALGORITHM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q9.1 [NUMERICAL]: Run Perceptron on the following labeled points:          ║
║       (+1): (3, 1), (2, 2), (1, 3)                                         ║
║       (-1): (-1, -1), (-2, 0), (0, -2)                                     ║
║       Start with w = (0, 0). Show all updates until convergence.           ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

Training data:
    Point 1: x₁ = (3, 1),   y₁ = +1
    Point 2: x₂ = (2, 2),   y₂ = +1
    Point 3: x₃ = (1, 3),   y₃ = +1
    Point 4: x₄ = (-1, -1), y₄ = -1
    Point 5: x₅ = (-2, 0),  y₅ = -1
    Point 6: x₆ = (0, -2),  y₆ = -1

Visualization:
    
       y
       ↑
     3 │     ●₃
       │   ●₂
     1 │     ●₁                 Legend:
     0 ├─●₅─────────→ x         ● = +1
       │   ○₄                   ○ = -1
    -1 │                        
    -2 │ ○₆
       │
      -2 -1  0  1  2  3

PERCEPTRON ALGORITHM:
────────────────────

w = (0, 0)

ITERATION 1:
────────────

Check x₁ = (3, 1), y₁ = +1:
    w · x₁ = (0,0) · (3,1) = 0
    y₁ · (w · x₁) = (+1)(0) = 0 ≤ 0 → MISCLASSIFIED!
    
    Update: w = w + y₁ · x₁ = (0,0) + (3,1) = (3, 1)
    
    ┌─────────────────────────────────────────────┐
    │  w = (3, 1) after update 1                  │
    └─────────────────────────────────────────────┘

Check x₂ = (2, 2), y₂ = +1:
    w · x₂ = (3,1) · (2,2) = 6 + 2 = 8
    y₂ · (w · x₂) = (+1)(8) = 8 > 0 → Correct ✓

Check x₃ = (1, 3), y₃ = +1:
    w · x₃ = (3,1) · (1,3) = 3 + 3 = 6
    y₃ · (w · x₃) = (+1)(6) = 6 > 0 → Correct ✓

Check x₄ = (-1, -1), y₄ = -1:
    w · x₄ = (3,1) · (-1,-1) = -3 - 1 = -4
    y₄ · (w · x₄) = (-1)(-4) = 4 > 0 → Correct ✓

Check x₅ = (-2, 0), y₅ = -1:
    w · x₅ = (3,1) · (-2,0) = -6 + 0 = -6
    y₅ · (w · x₅) = (-1)(-6) = 6 > 0 → Correct ✓

Check x₆ = (0, -2), y₆ = -1:
    w · x₆ = (3,1) · (0,-2) = 0 - 2 = -2
    y₆ · (w · x₆) = (-1)(-2) = 2 > 0 → Correct ✓

End of Iteration 1: 1 update made

ITERATION 2:
────────────

Check all points with w = (3, 1):

x₁: w · x₁ = 9 + 1 = 10, y₁(w·x₁) = 10 > 0 ✓
x₂: w · x₂ = 6 + 2 = 8, y₂(w·x₂) = 8 > 0 ✓
x₃: w · x₃ = 3 + 3 = 6, y₃(w·x₃) = 6 > 0 ✓
x₄: w · x₄ = -3 - 1 = -4, y₄(w·x₄) = 4 > 0 ✓
x₅: w · x₅ = -6 + 0 = -6, y₅(w·x₅) = 6 > 0 ✓
x₆: w · x₆ = 0 - 2 = -2, y₆(w·x₆) = 2 > 0 ✓

ALL CORRECT! No updates needed.

ALGORITHM CONVERGES!

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Final weight vector: w = (3, 1)                                          │
│  Total updates: 1                                                          │
│                                                                            │
│  Decision boundary: 3x + y = 0  (or y = -3x)                              │
│                                                                            │
│  Classification rule:                                                      │
│    If 3x + y > 0 → predict +1                                             │
│    If 3x + y < 0 → predict -1                                             │
│                                                                            │
│  Visualization:                                                            │
│       y                                                                    │
│       ↑     ●₃                                                            │
│     3 │    ╱                                                              │
│       │  ●₂    (+1 region)                                                │
│     1 │ ╱  ●₁                                                             │
│     0 ├╱○₅─────────→ x                                                    │
│       │╲ ○₄                                                                │
│    -1 │ ╲   (-1 region)                                                   │
│    -2 │○₆╲                                                                │
│             Decision boundary: y = -3x                                     │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q9.2 [NUMERICAL]: Same as Q9.1 but add point (0, 0) with label +1.        ║
║       What happens?                                                        ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

New training data includes: x₇ = (0, 0), y₇ = +1

The data is now NOT linearly separable!

Proof: Point (0,0) with label +1 is surrounded by:
    - (-1,-1) with label -1
    - (-2, 0) with label -1
    - (0, -2) with label -1

Any line through origin that separates the +1 points from -1 points
must put (0,0) on the +1 side, but (0,0) is at the boundary.

Actually, let's check more carefully:

If we try w = (3, 1):
    w · x₇ = (3,1) · (0,0) = 0
    y₇ · (w · x₇) = (+1)(0) = 0 ≤ 0 → MISCLASSIFIED!
    
    Update: w = (3,1) + (0,0) = (3, 1)  [no change!]

The algorithm will oscillate:
    x₇ = (0,0) → w · x = 0 always → always "misclassified" 
    But update w + y·(0,0) = w (no change)

Actually, this is a degenerate case. Let me reconsider...

For ANY weight vector w, we have w · (0,0) = 0.
So sign(w · (0,0)) is undefined (or 0).

The perceptron can never correctly classify the origin!

More generally, if we require strict inequality (y·(w·x) > 0), the origin
can never be correctly classified for any label.

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  The algorithm will NOT converge!                                          │
│                                                                            │
│  Problem: The point (0, 0) always gives w·x = 0, which is a              │
│  misclassification (since y·(w·x) = 0 ≤ 0).                              │
│                                                                            │
│  However, the update w = w + y·(0,0) = w doesn't change w!               │
│                                                                            │
│  Result: Infinite loop, never terminates.                                  │
│                                                                            │
│  Solution: Add bias term! Use w·x + b, where b ≠ 0 for origin.           │
└────────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q9.3 [PROOF]: State and prove the Perceptron Convergence Theorem.          ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

THEOREM (Perceptron Convergence):
    If the training data is linearly separable with:
    - Margin γ (minimum distance from any point to the optimal hyperplane)
    - Maximum norm R = max_i ||x_i||
    
    Then the perceptron makes at most (R/γ)² updates before converging.

PROOF:

Let w* be the optimal separating hyperplane with ||w*|| = 1.
By assumption, for all i: y_i(w* · x_i) ≥ γ

Let w^(t) be the weight after t updates.
Initialize w^(0) = 0.

CLAIM 1: w^(t) · w* ≥ t·γ (dot product with optimal grows)
─────────────────────────────────────────────────────────

After each update on misclassified point (x_i, y_i):
    w^(t+1) = w^(t) + y_i · x_i

    w^(t+1) · w* = w^(t) · w* + y_i · (x_i · w*)
                 ≥ w^(t) · w* + γ    [since y_i(w* · x_i) ≥ γ]

By induction: w^(t) · w* ≥ t·γ

CLAIM 2: ||w^(t)||² ≤ t·R² (norm grows slowly)
──────────────────────────────────────────────

||w^(t+1)||² = ||w^(t) + y_i · x_i||²
             = ||w^(t)||² + 2y_i(w^(t) · x_i) + ||x_i||²
             
Since we only update when misclassified: y_i(w^(t) · x_i) ≤ 0

             ≤ ||w^(t)||² + 0 + R²
             = ||w^(t)||² + R²

By induction: ||w^(t)||² ≤ t·R²

COMBINING CLAIMS:
─────────────────

Using Cauchy-Schwarz: w^(t) · w* ≤ ||w^(t)|| · ||w*|| = ||w^(t)||

From Claim 1: t·γ ≤ w^(t) · w* ≤ ||w^(t)||
From Claim 2: ||w^(t)|| ≤ √(t·R²) = √t · R

Therefore:
    t·γ ≤ √t · R
    √t ≤ R/γ
    t ≤ (R/γ)²

┌────────────────────────────────────────────────────────────────────────────┐
│  CONCLUSION:                                                               │
│                                                                            │
│  The perceptron makes at most (R/γ)² updates before finding a             │
│  separating hyperplane.                                                    │
│                                                                            │
│  Key insight: The number of updates depends only on the "quality"         │
│  of the data (R/γ), not on the number of points n!                       │
│                                                                            │
│  ┌─────────────────────────────────────────────────────────────────────┐  │
│  │                                                                     │  │
│  │             Maximum updates ≤ (R/γ)²                               │  │
│  │                                                                     │  │
│  │  where R = max ||x_i|| and γ = margin                              │  │
│  │                                                                     │  │
│  └─────────────────────────────────────────────────────────────────────┘  │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
SECTION 10: MIXED/GENERAL QUESTIONS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q10.1: Compare BFPRT vs Randomized Quickselect for median finding.        ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

┌──────────────────────────────────────────────────────────────────────────┐
│                   │  BFPRT (Deterministic)  │  Randomized Quickselect   │
├──────────────────────────────────────────────────────────────────────────┤
│  Time Complexity  │  O(n) worst-case        │  O(n) expected            │
│                   │                         │  O(n²) worst-case         │
├──────────────────────────────────────────────────────────────────────────┤
│  Pivot Selection  │  Median-of-medians      │  Random element           │
│                   │  (deterministic)        │                           │
├──────────────────────────────────────────────────────────────────────────┤
│  Constants        │  Higher (~20n)          │  Lower (~4n)              │
├──────────────────────────────────────────────────────────────────────────┤
│  Practical Speed  │  Slower in practice     │  Faster in practice       │
├──────────────────────────────────────────────────────────────────────────┤
│  Guarantee        │  Always O(n)            │  High probability         │
├──────────────────────────────────────────────────────────────────────────┤
│  Use Case         │  Need guaranteed        │  Average-case performance │
│                   │  worst-case bounds      │  matters more             │
└──────────────────────────────────────────────────────────────────────────┘

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q10.2: Prove that bipartite matching reduces to max flow correctly.       ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

REDUCTION:
    Given bipartite graph G = (L ∪ R, E)
    
    Construct flow network G':
        - Add source s, sink t
        - Edge (s, ℓ) with capacity 1 for all ℓ ∈ L
        - Edge (ℓ, r) with capacity 1 for all (ℓ, r) ∈ E
        - Edge (r, t) with capacity 1 for all r ∈ R

CORRECTNESS:

Claim 1: Matching of size k → Flow of value k
    Given matching M with |M| = k:
    Set f(s,ℓ) = f(ℓ,r) = f(r,t) = 1 for each (ℓ,r) ∈ M
    This is a valid flow:
        - Capacity constraints: all edges have cap 1, flow ≤ 1 ✓
        - Conservation: each matched vertex has 1 in, 1 out ✓
    Flow value = k

Claim 2: Integral flow of value k → Matching of size k
    Given integral max flow f with |f| = k:
    Define matching: M = {(ℓ,r) : f(ℓ,r) = 1}
    This is a valid matching:
        - Each ℓ has at most 1 outgoing flow (from s→ℓ capacity 1)
        - Each r has at most 1 incoming flow (from r→t capacity 1)
        - So each vertex matched to at most one partner ✓
    |M| = |f| = k

CONCLUSION: Max matching size = Max flow value ∎

╔═══════════════════════════════════════════════════════════════════════════╗
║ Q10.3 [NUMERICAL]: Revenue comparison - Vickrey vs Myerson optimal.       ║
║       2 bidders, values uniform on [0, 100]. Compare expected revenues.   ║
╚═══════════════════════════════════════════════════════════════════════════╝

SOLUTION:

VICKREY AUCTION (no reserve):
    Winner: Highest bidder (truthful)
    Payment: Second-highest bid
    
    E[Revenue] = E[2nd highest of 2 uniform[0,100] values]
               = E[min(V₁, V₂)]
    
    For i.i.d. uniform[0,100], with n=2:
        E[min] = 100/(n+1) = 100/3 ≈ $33.33

MYERSON OPTIMAL AUCTION (reserve = 50):
    Winner: Highest bidder IF value ≥ 50
    Payment: max(50, second-highest bid)
    
    E[Revenue] = P(at least one v ≥ 50) × E[payment | sale]
    
    P(at least one ≥ 50) = 1 - P(both < 50) = 1 - (0.5)² = 0.75
    
    E[payment | sale]:
        - If both ≥ 50: pay second-highest = E[min(V₁,V₂) | both ≥ 50]
          For truncated uniform[50,100]: E[min] = 50 + (100-50)/(2+1) = 50 + 50/3 = 66.67
        - If exactly one ≥ 50: pay 50 (reserve)
        
    P(both ≥ 50) = (0.5)² = 0.25
    P(exactly one ≥ 50) = 2(0.5)(0.5) = 0.50
    
    E[Revenue] = 0.25 × 66.67 + 0.50 × 50 + 0.25 × 0
               = 16.67 + 25 + 0
               = $41.67

┌────────────────────────────────────────────────────────────────────────────┐
│  ANSWER:                                                                   │
│                                                                            │
│  Vickrey (no reserve):    E[Revenue] = $33.33                             │
│  Myerson optimal:         E[Revenue] = $41.67                             │
│                                                                            │
│  Revenue increase from optimal reserve: 25%!                              │
│                                                                            │
│  Key insight: The reserve price of 50 filters out low-value bidders,      │
│  extracting more surplus when there IS a high-value bidder.               │
└────────────────────────────────────────────────────────────────────────────┘

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
                     END OF QUESTION BANK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

QUICK REFERENCE - FORMULAS TO REMEMBER:

┌──────────────────────────────────────────────────────────────────────────────┐
│  TOPIC                │  KEY FORMULA                                         │
├──────────────────────────────────────────────────────────────────────────────┤
│  BFPRT Recurrence     │  T(n) = T(n/5) + T(7n/10) + O(n) = O(n)             │
├──────────────────────────────────────────────────────────────────────────────┤
│  Orientation Test     │  (B-A) × (C-A) = (Bx-Ax)(Cy-Ay) - (By-Ay)(Cx-Ax)   │
├──────────────────────────────────────────────────────────────────────────────┤
│  Max-Flow Min-Cut     │  max |f| = min c(S,T) over all s-t cuts            │
├──────────────────────────────────────────────────────────────────────────────┤
│  Vickrey Payment      │  Winner pays second-highest bid                     │
├──────────────────────────────────────────────────────────────────────────────┤
│  FP Equilibrium       │  b(v) = (n-1)/n × v for n bidders, Uniform[0,1]    │
├──────────────────────────────────────────────────────────────────────────────┤
│  Virtual Value        │  φ(v) = v - (1-F(v))/f(v)                          │
├──────────────────────────────────────────────────────────────────────────────┤
│  Myerson Payment      │  p(v) = v·x(v) - ∫₀^v x(z)dz                       │
├──────────────────────────────────────────────────────────────────────────────┤
│  Put-Call Parity      │  C - P = S₀ - Ke^(-rT)                             │
├──────────────────────────────────────────────────────────────────────────────┤
│  Perceptron Update    │  w = w + y·x (when y(w·x) ≤ 0)                     │
├──────────────────────────────────────────────────────────────────────────────┤
│  Perceptron Bound     │  ≤ (R/γ)² updates                                  │
└──────────────────────────────────────────────────────────────────────────────┘

Good luck on your exam! 📚
