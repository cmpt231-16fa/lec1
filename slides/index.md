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

Analogy: sorting a hand of **cards** one by one

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

>>>
TODO: illustration

---
## Loop invariant
+ Property that is true **before**, **during**, and **after** loop
+ For insertion sort: at each iteration of loop,
  + part of array `A[1 .. j-1]` is in **sorted** order

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

---
## Complexity of insertion sort
Let \`t_j\` be num times `while` cond is checked

```
insertion_sort(A, n):
  for j = 2 to n:               # c0 * n
    key = A[j]                  # c1 * (n-1)
    i = j - 1                   # c2 * (n-1)
    while i > 0 and A[i] > key: # c3 * sum( t_j )
      A[i+1] = A[i]             # c4 * sum (t_j - 1)
      i = i -1                  # c5 * sum (t_j - 1)
    A[i+1] = key                # c6 * (n-1)
```

Summation notation: \`sum_2^n t_j = t_2 + t_3 + ... + t_n\`

---
## Worst-case complexity
+ **Best** case is if input is **pre-sorted**:
  + Still need to scan, but all \`t_j=1\`
  + Total complexity: \`T(n)=a\*n+b\`, for some a, b
  + **Linear** in n
+ **Worst** case: input is in **reverse** order!
  + Inner "while" loop always max iterations: \`t_j=j\`
  + Total complexity for line 5:
    \` c_4 * sum_2^n(t_j-1) = c_4*sum_2^n(j-1)
    = c_4*(n-1)*n/2 = (c_4/2)n^2 - n/2 \`
  + **Quadratic** in n
+ **Average** case: input is random, \`t_j=j/2\` on average
  + Still **quadratic** in n

---
## Theta (&Theta;) notation
+ e.g., line5: \` (c_4 / 2) n^2 - n/2 \`
+ **Constants** \`c_1, c_2, ...\` may vary for different computers
  + As n gets **big**, constants are irrelevant
  + Even the n term is dominated by the \`n^2\` term
+ Complexity of insertion sort is on **order** of \`n^2\`
  + Notation: \`T(n) = Theta(n^2)\` ("big theta")
+ \`Theta(1)\` means an algorithm runs in **constant time**
  + i.e., does not depend on **size** of input

---
## Discrete math review
+ f(x) is **monotone increasing** iff: x &lt; y &Rarr; f(x) &le; f(y)
  + Also called "non-decreasing"
+ f(x) is **strictly increasing** iff: x &lt; y &Rarr; f(x) &lt; f(y)
+ "a mod n" is the **remainder** of a when divided by n
  + e.g., 17 mod 5 = 2 (in Python: `17 % 5`)
+ \` lim_(x->a) f(x) = b \`: "**limit** of f(x) as x goes to a is b"
  + \` forall epsi > 0, exists delta > 0: |x-a| < delta => |f(x)-b| < epsi \`
+ \` lim_(n->oo) f(n) = b \`: "**limit** of f(n) as n goes to infinity is b":
  + \` forall epsi > 0, exists n_0: n > n_0 => |f(n)-b| < epsi \`

---
## Iterated functions (recursion)
+ \` f^((i))(x) \`: function f, applied i times to x:
  f(f(f(... f(x) ...)))
  + **Not** the same as \` f^i(x) = (f(x))^i \`
+ e.g., \` log^((2))(1000) = log(log(1000)) = log(3) ~~ 0.477 \`
  + But \` log^2(1000) = (log(1000))^2 = 3^2 = 9 \`
+ Define \` f^((0))(x) = x \` (i.e., apply f zero times)
+ **Iterated log**: \` log^**(n) = min(i>=0: log^((i))(n)<=1) \`
  + \# times log needs to be applied to n until the result is &le;1
  + e.g., let \` lg = log_2 \`: then \` lg^**(16) = 3 \` because
    \` lg(lg(lg(16))) = lg(lg(4)) = lg(2) = 1 \`

---
## Fibonacci and golden ratio
+ The n-th **Fibonacci number** is \`F_ n = F_ (n-1) + F_ (n-2)\`
  + Start with \`F_0=0, F_1=1\`: 0, 1, 1, 2, 3, 5, 8, 13, 21, ...
  + (Lucas numbers start with \`F_0=2\`)
+ The **Golden ratio** &phi; (and its conjugate, \`bar phi\`)
  satisfy \`x^2 = x+1\`
  + \` phi = (1 +- sqrt 5)/2 ~~ 1.61803 and -0.61803 \`
+ Can prove (#3.2-7) that \`F_ n = (phi^n (bar phi)^n)/sqrt 5\`
  + Second term is fractional: \` |(bar phi)^n|/sqrt 5 < 1/2\`
  + So can write \` F_ n = |_ phi^n/sqrt 5 + 1/2 _| = text round( phi^n / sqrt 5) \`
  + i.e., Fibonacci grows **exponentially**!

---
## Mathematical logic (notation)
+ \`not A\` (or !A): "**not** A"
  + if A = "it is Tue", then \`not A\` = "it is not Tue"
+ A &Rarr; B: "**implies**", "if A, then B"
  + if B = "meatloaf", then A &Rarr; B = "if Tue, then meatloaf"
+ A &Harr; B: if and only if ("**iff**")
  + (A &Rarr; B) and (B &Rarr; A)
+ \`forall\`: "for all"
  + ""

---
## Logic
+ **Contrapositive** of "A &Rarr; B" is \`not B => not A\`
  + **Equivalent** to original statement
  + "If Tue, then meatloaf" &Harr; "if not meatloaf, then not Tue"
+ **Converse** of "A &Rarr; B" is "\`not A => not B\`"
  + **Not** equivalent to original statement!
  + "if not Tue, then not meatloaf"


---
## Asymptotic growth
+ Behaviour "in the limit" (big n)
+ Definition of **Theta** as a class of functions:
  + \` f(n) in Theta(g(n)) iff exists c_1, c_2, n_0 so
    0 <= c_1 g(n) <= f(n) <= c_2 g(n), forall n > n_0 \`
  + f(n) is "sandwiched" between two multiples of g(n),
    \`c_1 g(n)\` and \`c_2 g(n)\`
+ "Big O": O(g(n)) specifies only the **upper** bound:
  + \` f(n) in O(g(n)) iff exists c_2, n_0 so
    0 <= f(n) <= c_2 g(n), forall n > n_0 \`
  + e.g., \`Theta(n^2) sub O(n^2) sub O(n^3) \`
+ "Big Omega": &Omega;(g(n)) specifies only the **lower** bound
  + Think of other examples?

>>>
TODO: graph

---
## Asymptotic proofs
+ **(p.52 #3.1-2)** Prove: \` forall a, b>0: (n+a)^b = Theta(n^b) \`
