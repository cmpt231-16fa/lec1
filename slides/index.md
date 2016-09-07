# CMPT231
## Data Structures and Algorithms
### Dr. Sean Ho

>>>
Speaker notes go here.

---
<!-- .slide: data-background-iframe="https://cmpt231-16fa.github.io/" -->

>>>
+ Website: **static** content (schedule, HW assignments, etc.)
+ myCourses: HW **submissions**
+ 9 **HW**, roughly 1/wk
  + due **Thu 10pm** at myCourses
  + **coding** in later HWs
+ 1 **midterm** 25Oct, 1 **final**

---
## What you need to succeed in 231
+ **Tinkerer's** heart (self-motivated)
+ Discrete **math** (e.g., MATH150)
  + Logic, **proofs**
+ Comfortable **coding** environment
  + Python, C++, Java, etc.
  + not until **later** in semester

---
## Outline for today
+ **Algorithmic** analysis
  + **Insertion** sort
+ Discrete **math** review
  + Monotonicity, limits, iterated func
  + Proofs
+ **Asymptotic** notation: O(n), &Omega;(n)

---
## What is an algorithm?
+ Precise **process** for solving a problem:
  + Input &rarr; Compute &rarr; Output
+ Various languages for **expression**:
  + English, pseudocode, UML diagrams, etc.
+ Programming languages for **implementation**:
  + Python, C, Java, etc.
+ Focus not on **toolkits** but **problem solving**

---
## Algorithmic complexity
+ How many machine **instr** to **execute**
  + As function of input **size**
  + Ignoring **constant** factors
+ Depends on machine **architecture**
  + CPUs generally **sequential**
  + GPUs are massively **parallel**
+ **Running time** is more complex
  + Cache / **memory** very important

---
## Basic machine model
+ Our simple CPU **instruction set**:
  + **Arith**: +, -, \*, /, &lt;, &gt;, &ne;
  + **Data**: `load` (read), `store` (write), copy
  + **Control**: `if`/`else`, `for`/`while`, functions
  + **Types**: char, int, float
  + Data **Structures**: pointers, fixed-length arrays
    + but **not** Python list / STL vec!
+ Assume each takes **constant** time

---
## Task definition: Sorting
+ Input: **array** of key-value pairs
  + wlog, assume **keys** are 1 ... n
  + **values** (payload): any data
+ Output: array **sorted** by key
  + **in-place**: modify original array
  + **out-of-place**: return a copy
+ In standard **libraries**:
  + Python: `sort()`, `sorted()`
  + C++/Java: `sort()`
  + **How** do they do it?

---
## One algo: insertion sort
```
insertion_sort(A, n):
  for j = 2 to n:
    key = A[j]
    i = j - 1
    while i > 0 and A[i] > key:
      A[i+1] = A[i]
      i = i -1
    A[i+1] = key
```

+ Like a hand of **cards**
+ Loop **invariant**:
  + `A[1 .. j-1]` are in sorted order
+ Check: **before**, **during**, and **after** loop

---
## Complexity of insertion sort
Let \`t_j\` be num times `while` cond is checked

```
insertion_sort(A, n):
  for j = 2 to n:               # `c_0*n`
    key = A[j]                  # `c_1*(n-1)`
    i = j - 1                   # `c_2*(n-1)`
    while i > 0 and A[i] > key: # `c_3*sum_2^n t_j`
      A[i+1] = A[i]             # `c_4*sum_2^n (t_j-1)`
      i = i -1                  # `c_5*sum_2^n (t_j-1)`
    A[i+1] = key                # `c_6*(n-1)`
```

Summation notation: \`sum_2^n t_j = t_2 + t_3 + ... + t_n\`
