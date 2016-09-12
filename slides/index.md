<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" -->
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
  + but not until **later** in semester

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" -->
## Outline for today
+ **Algorithmic** analysis
  + **Insertion** sort
+ Discrete **math** review
  + **Logic** and proofs
  + Monotonicity, limits, iterated functions
+ **Asymptotic** notation: &Theta;, O, &Omega;, o, &omega;
  + **Proving** asymptotic bounds

---
## What is an algorithm?
+ Precise **process** for solving a problem:
  + Input &rarr; Compute &rarr; Output
+ Various languages for **expression**:
  + English, pseudocode, UML diagrams, etc.
+ Programming languages for **implementation**:
  + Python, C, Java, etc.
+ Focus: not **toolkits** but **problem solving**

---
## Algorithmic complexity
+ How many machine **instr** to **execute**
  + As function of input **size**
  + Ignoring **constant** factors
+ Depends on machine **architecture**
  + CPUs generally **sequential**
  + GPUs are massively **parallel**
+ **Running time** is more complex than this
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
## Problem definition: Sorting
+ Input: **array** of key-value pairs
  + wlog, assume **keys** are *1* ... *n*
  + **values** (payload) can be any data
+ Output: array **sorted** by key
  + **in-place**: modify original array
  + **out-of-place**: return a copy
+ In standard **libraries**:
  + Python: `sort()`, `sorted()`
  + C++/Java: `sort()`
  + **How** do they do it?

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/pixabay-726797-cards_monalisa.jpg" -->
## Insertion sort: hand of cards

<div class="imgbox"><div data-markdown>
```
insertion_sort(A, n):
  for j = 2 to n:
    key = A[j]
    i = j - 1
    while i > 0 and A[i] > key:
      A[i+1] = A[i]
      i = i - 1
    A[i+1] = key
```
</div><div data-markdown>

| In: |  *E*  | **B** |   D   |   F   |   A   |   C   |
|-----|-------|-------|-------|-------|-------|-------|
| j=3 |   B   |  *E*  | **D** |   F   |   A   |   C   |
| j=4 |   B   |   D   |  *E*  | **F** |   A   |   C   |
| j=5 |  *B*  |  *D*  |  *E*  |  *F*  | **A** |   C   |
| j=6 |   A   |   B   |  *D*  |  *E*  |  *F*  | **C** |
| Out:|   A   |   B   |   C   |   D   |   E   |   F   |

</div></div>

>>>
Not the **fastest** solution, but pretty **easy** to code

---
## Proof of correctness
+ Loop **invariant**:
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
      i = i - 1
    A[i+1] = key
```

---
## Complexity analysis
Let \`t_j\` be the number of times the loop condition is checked
in the inner "while" loop:

```
insertion_sort(A, n):
  for j = 2 to n:               # c0 * n
    key = A[j]                  # c1 * (n-1)
    i = j - 1                   # c2 * (n-1)
    while i > 0 and A[i] > key: # c3 * sum (t_j)
      A[i+1] = A[i]             # c4 * sum (t_j - 1)
      i = i - 1                 # c5 * sum (t_j - 1)
    A[i+1] = key                # c6 * (n-1)
```

Summation notation: \`sum_2^n t_j = t_2 + t_3 + ... + t_n\`

---
## Best vs. worst case
+ **Best** case is if input is **pre-sorted**:
  + Still need to **scan** to verify sorted,
  + But inner **while** loop only has 1 iteration: \`t_j=1\`
  + Total complexity: T(n) = a n + b, for some a, b
  + **Linear** in n
+ **Worst** case: input is in **reverse** order!
  + Inner "while" loop always max iterations: \`t_j=j\`
  + Calculate **total** complexity T(n):
  + Pick a line in inner loop, e.g., **line 5**: `A[i+1] = A[i]`
  + Complexity of other lines **similar**

---
## Worst case complexity
+ **Total** complexity for line 5, worst-case:
  \` c_4 sum_2^n(t_j-1) = c_4 sum_2^n(j-1) \`
  \` = c_4 (n-1)n/2 = (c_4/2)n^2 - (1/2)n \`
  + **Quadratic** in n
+ **Average** case: input is random, \`t_j=j/2\` on average
  + Still quadratic (only changes by a **constant** factor)

---
## Theta (&Theta;) notation
+ Insertion sort, line 5: \` (c_4 / 2) n^2 - (1/2)n \`
+ **Constants** \`c_1, c_2, ...\` may vary for different computers
  + As n gets **big**, constants become **irrelevant**
  + Even the n term is dominated by the \`n^2\` term
+ Complexity of insertion sort is on **order** of \`n^2\`
  + Notation: \`T(n) = Theta(n^2)\` ("theta")
+ \`Theta(1)\` means an algorithm runs in **constant time**
  + i.e., does not depend on **size** of input
+ We'll define &Theta; more **precisely** later today

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" -->
## Outline for today
+ **Algorithmic** analysis
  + **Insertion** sort
+ Discrete **math** review
  + **Logic** and proofs
  + Monotonicity, limits, iterated functions
+ **Asymptotic** notation: &Theta;, O, &Omega;, o, &omega;
  + **Proving** asymptotic bounds

---
## Logic notation
+ \`not A\` (or !A): "**not** A"
  + e.g., let A = "*it is Tue*": then \`not A\` = "*it is not Tue*"
+ A &rArr; B: "**implies**", "if A, then B"
  + e.g., let B = "*meatloaf*":
  + then A &rArr; B = "*if Tue, then meatloaf*"
+ A &hArr; B: if and only if ("**iff**"):
  + **equivalence**: (A &rArr; B) and (B &rArr; A)
+ **John 14:15**: "*If you love me, keep my commands*"
+ **v21**: "*Whoever keeps my commands loves me*"
+ **v24**: "*He who does not love me will not obey my teaching*"

---
## Logic notation: &forall; and &exist;
+ &forall;: "**for all**",
  + e.g., "&forall; day: meal(day) = meatloaf"
  + "*For all days, the meal on that day is meatloaf*"
+ &exist;: "there **exists**" (not necessarily unique)
  + e.g., "&exist; day: meal(day) = meatloaf"
  + "*There exists a day on which the meal is meatloaf*"

---
## Logic: contrapos and converse
+ **Contrapositive** of "A &rArr; B" is \`not B => not A\`
  + **Equivalent** to original statement
  + "*If Tue, then meatloaf*" &hArr; <br/> "*if not meatloaf, then not Tue*"
+ **Converse** of "A &rArr; B" is "\`not A => not B\`"
  + **Not** equivalent to original statement!
  + "*if not Tue, then not meatloaf*"

---
## Monotonicity
+ f(x) is **monotone increasing** iff: *x &lt; y &rArr; f(x) &le; f(y)*
  + Also called "non-decreasing"
  + Can be **flat**
+ f(x) is **strictly increasing** iff: *x &lt; y &rArr; f(x) &lt; f(y)*
  + note inequality is **strict**
+ "*a mod n*" is the **remainder** of a when divided by n
  + e.g., 17 mod 5 = 2 (in Python: `17 % 5`)

---
## Limits
+ Formal definitions involve &forall; and &exist;
+ \` lim_(x->a) f(x) = b \`: "**limit** of f(x) as x goes to a"
  + &forall; &epsilon; &gt; 0, &exist; &delta; &gt; 0:
    *|x-a| &lt; &delta;* &rArr; *|f(x)-b| &lt; &epsilon;*
  + When *x* is "close" to *a*, then *f(x)* is "close" to *b*
+ \` lim_(n->oo) f(n) = b \`: "**limit** of f(n) as n goes to infinity":
  + &forall; &epsilon; &gt; 0, &exist; n0:
    *n &gt; n0* &rArr; *|f(n)-b| &lt; &epsilon;*
  + When *n* is "big", *f(n)* is "close" to *b*

---
## Iterated functions (recursion)
+ \` f^((i))(x) \`: function *f*, **applied** *i* times to *x*:
  f(f(f(... f(x) ...)))
  + **Not** the same as \` f^i(x) = (f(x))^i \`
+ e.g., \` log^((2))(1000) \` = log(log(1000)) = log(3) &asymp; 0.477
  + But \` log^2(1000) = (log(1000))^2 = 3^2 \` = 9
+ By convention, \` f^((0))(x) = x \` (apply *f* **zero** times)

---
## Iterated log: log\*(n)
+ \` log^**(n) = min(i>=0: log^((i))(n)<=1) \`
+ \# **times** *log* needs to be applied to *n* until the result is *&le;1*
+ e.g., let lg = \` log_2 \`:
  + then \` text(lg)^**(16) = 3 \`, because
  + lg(lg(lg(16))) = lg(lg(4)) = lg(2) = 1

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-_jN5_9zu8DM-sunflower.jpg" -->
## Fibonacci sequence
+ The n-th **Fibonacci number** is \`F\_n=F\_(n-1) + F\_(n-2)\`
  + Start with \`F_0=0, F_1=1:\`
  + \`F_n=0, 1, 1, 2, 3, 5, 8, 13, 21, ...\`
  + (Lucas numbers start with \`F_0=2\`)
+ Shows up all over **nature**
  + Num of **spirals** on sunflowers, pinecones, etc.
  + [Vi Hart video: Doodling in Math ![YouTube](static/img/vi-fib.jpg)](https://www.youtube.com/watch?v=ahXIMUkSXX0)

---
<!-- .slide: data-background-image="static/bg/nautilus-cutaway-shell.jpg" -->
## The Golden ratio &phi;
+ &phi; is the solution to the equation \`x^2 = x+1\`
  + \` phi = (1 +- sqrt 5)/2 \`
  + Actually, two solutions: &phi; and its **conjugate**, \`bar phi\`
  + &phi; &asymp; 1.61803, and \`bar phi\` &asymp; -0.61803
+ Also shows up all over **nature**
  + Dimensions of **Nautilus** seashells, spiral **galaxies**, etc.
  + **Aspect ratio** in architecture, e.g., Parthenon

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-_jN5_9zu8DM-sunflower.jpg" -->
## Fibonacci + golden ratio
+ Can **prove** *(#3.2-7)* that \`F\_n = (phi^n - (bar phi)^n)/sqrt 5\`
+ Second term is **fractional**: \` |(bar phi)^n|/sqrt 5 < 1/2\`
+ Thus: \`F\_n = |\_\_ phi^n/sqrt 5 + 1/2 \_\_| = text(round)( phi^n / sqrt 5) \`
+ i.e., Fibonacci grows **exponentially**!

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" -->
## Outline for today
+ **Algorithmic** analysis
  + **Insertion** sort
+ Discrete **math** review
  + **Logic** and proofs
  + Monotonicity, limits, iterated functions
+ **Asymptotic** notation: &Theta;, O, &Omega;, o, &omega;
  + **Proving** asymptotic bounds

---
## Asymptotic growth: &Theta;, O, &Omega;
+ Behaviour "in the limit" (big n)
+ Define **Theta** as class of functions: f(n) &isin; *&Theta;(g(n))* iff
  + &exist; c1, c2, n0: &forall; n &gt; n0,
  0 &le; *c1 g(n)* &le; *f(n)* &le; *c2 g(n)*
  + *f* is "**sandwiched**" between two multiples of *g*:
    c1 g(n) and c2 g(n)
+ "Big O": specify only **upper** bound: f(n) &isin; *O(g(n))* iff
  + &exist; c2, n0: &forall; n &gt; n0, 0 &le; *f(n)* &le; *c2 g(n)*
  + e.g., \`Theta(n^2) sub O(n^2) sub O(n^3) \`
+ "Big Omega": &Omega;(g(n)) specifies only the **lower** bound
  + Think of other examples?

---
## Proving asymptotic growth
+ **(p.52 #3.1-2)** &forall; a, b &gt; 0, prove: \`(n+a)^b in Theta(n^b) \`
+ From definition: we need to **find** \`n_0, c_1, c_2\` such that
  \` forall n > n_0: c_1 n^b <= (n+a)^b <= c_2 n^b \`
+ i.e., find constants so we can **sandwich** \`(n+a)^b\` in between
  two multiples of \`n^b\`

---
## Prove: (n+a)^b &isin; &Theta;(n^b)
+ **Observe** that *n+a &ge; n/2*, as long as n &gt; 2|a|
  + Also, *n+a &le; 2n*, as long as n &gt; |a|
  + Hence, *n+a* is **sandwiched** by *n/2* and *2n* (if n &gt; 2|a|)
+ **Raise** to the *b* power (\`x^b\` is **monotone** if x &gt; 1, b &gt; 0)
  + Thus, \`(n/2)^b <= (n+a)^b <= (2n)^b\` (for n &gt; 2|a|)
+ So we select \`n_0 = 2|a|, c_1 = 2^(-b), c_2 = 2^b\`
  + This **proves** the Theta bound.

---
## Asymptotic shorthand
+ &Theta;(g) is a **class** of functions
  + But for convenience, some **short-hand** notation:
+ When &Theta; (et al) are on the **right** side of =:
  + It means "there **exists**" \`f in Theta(g)\`
  + e.g., \` 2n^2 + 3n = Theta(n^2) \`
+ When &Theta; (et al) are on the **left** side of =:
  + It means "**for all**" \`f in Theta(g)\`
  + e.g., \` 4n^2 + Theta(n log(n)) = Theta(n^2) \`
  + True for **any** function in \`Theta(n log(n))\`

---
## Asymptotic domination: o, &omega;
+ "**Little o**": like a strict **less than** inequality: f &isin; *o(g)* iff
  + &forall; c > 0 &exist; n0: &forall; n > n0, 0 &le; *f(n)* < *c g(n)*
  + i.e., the **limit** of *f(n)/g(n)* &rarr; 0 as n &rarr; &infin;
+ "**Little omega**": like a strict **greater than**: f &isin; *&omega;(g)* iff
  + &forall; c > 0 &exist; n0: &forall; n > n0, 0 &le; *c g(n)* < *f(n)*
  + i.e., the **limit** of *f(n)/g(n)* &rarr; &infin; as n &rarr; &infin;

---
## Examples of o and &omega;
+ **Little o**: \`n^1.999 in o(n^2)\`, and \`n^2/log(n) in o(n^2) \`
  + but \` n^2/10000 notin o(n^2) \`
+ **Little omega**: \` n^2.0001 in omega(n^2) and n^2 log(n) = omega(n^2) \`

---
## Useful math identities
+ All *logs* are the **same** up to a constant factor:
  + \` log\_a(n) = (1/log\_b(a)) log\_b(n) \`
  + So we often use *lg* = \` log_2 \` for convenience
+ \` Theta(1) sub o(log(n)) sub o(n) sub o(n^(p)) sub o(p^n) \`
  + For any constant p &gt; 1
+ In fact, &forall; a>1, b: \`lim_(n->oo) n^b / a^n = 0 \`
  + Hence, \` n^b in o(a^n) \`
  + *"Exponentials dominate polynomials"*

---
## Stirling's approximation
+ **Factorial**: n! = n(n-1)(n-2)...(2)(1)
  + Number of **permutations** of n distinct objects
+ **Stirling's approx**:
  \` n! = sqrt(2pi n)(n/e)^n (1+Theta(1/n)) \`
+ Hence, \` log(n!) in Theta(n log(n)) \`

---
## Example asymptotic proof
+ **(p.62 #3-3)**: Prove: \`(log n)! in omega(n^3)\`
+ **Approach**: take *log* of both sides (log is monotone)
+ **Left side**: use Stirling:
  \` n! = sqrt(2pi n)(n/e)^n (1+Theta(1/n)) \`
  + So log(n!) &isin; &Theta;( *n log(n)* )
  + Now **substitute** log(n) for n, using monotonicity of log:
    + So log((log n)!) &isin; &Theta;( *(log n) log(log n)* )

---
## Prove: (log n)! = &omega;(n^3)
+ ... so: log((log n)!) &isin; &Theta;( *(log n) log(log n)* )
+ **Right side**: \`log(n^3) = 3log n\`
  + This is **close** to the left side, with *3* instead of *log(log n)*
  + But we only need an **&omega;** bound, and *log(log n)* &isin; &omega;(*3*)
+ **Combining**: \`log((log n)!) in Theta( (log n) log(log n) )\`
  + \` = omega( (log n) 3 ) = omega(log(n^3)) \`
+ So by **monotonicity**, \`(log n)! in omega(n^3)\`

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" -->
## Outline for today
+ **Algorithmic** analysis
  + **Insertion** sort
+ Discrete **math** review
  + **Logic** and proofs
  + Monotonicity, limits, iterated functions
+ **Asymptotic** notation: &Theta;, O, &Omega;, o, &omega;
  + **Proving** asymptotic bounds

---
<!-- .slide: data-background-image="http://sermons.seanho.com/img/bg/unsplash-NEgEJmN3JZo-boardwalk_grass.jpg" class="empty" -->

