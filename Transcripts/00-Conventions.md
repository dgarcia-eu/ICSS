# Notation and pseudocode conventions

*Introduction to Computing for the Social Sciences*

Lecture transcripts, exercise sheets and slides in this course
use the conventions below. They are collected here so that you only have to
learn them once, and if something in a transcript looks unfamiliar, this is
the page to check to find guidance. The pseudocode style follows Cormen's *Algorithms
Unlocked* (2013), which is the book most of our pseudocode is adapted from.
Where a topic has its own established notation, as bit strings, graphs and
automata do, we use the notation that is standard in that field rather than
inventing our own.

---

## 1. Pseudocode

Pseudocode is written for humans, not for machines. It is not Python, and it
does not have to run. The test of good pseudocode is simple: another person
can follow your instructions with pen and paper and solve the problem, and
can then translate them into a program.

### Layout

Every procedure starts with a header that names it and lists what goes in (input)
and what comes out (result). The name of a procedure is written in capital letters
with hyphens between words, as in SELECTION-SORT, BINARY-SEARCH or
DFS-VISIT, and it is used in the same form when the procedure is called from
another one. Blocks are marked by indentation only; there are no braces, no
`begin` and no `end`, and no punctuation at the end of a line. Keywords
(`For`, `While`, `If`, `then`, `else`, `return`) are written in bold in the
slides and in `**bold**` here.

```
Procedure NAME(argument, argument, …)
Inputs:
      argument: what it is
Data: auxiliary variables, if the procedure uses any
Result: what the procedure produces or changes
      <body>
```

### Statements

| Notation | Meaning |
|---|---|
| `x ← y` | assignment: store the value of `y` in `x` |
| `x = y` | comparison: is `x` equal to `y`? |
| `x ≠ y`, `x ≤ y`, `x ≥ y` | the other comparisons |
| `A[i]` | element `i` of array `A`; `A[p..r]` the subarray from `p` to `r` |
| `swap A[i] with A[j]` | exchange the two values |
| `# text` | comment |
| `⌊x⌋`, `⌈x⌉` | floor and ceiling |
| `a mod b` | remainder of the integer division of `a` by `b` |
| `∅` | the empty set; `x ∈ S` membership |
| `∞` | infinity, used for "no distance yet" and for sentinels |
| `NIL` | no value, used for a missing parent or an empty table entry |

### Control flow

```
For i = 1 to n:
      <body, repeated for i = 1, 2, …, n>

For each vertex u in G.V:
      <body, repeated once per element>

While <condition>:
      <body, repeated as long as the condition holds>

If <condition>
      then <statement>
      else <statement>

return <value>          # leaves the procedure immediately
```

Recursive calls and calls to other procedures are written like any other
statement, as in `MERGE-SORT(A, p, q)` or `ENQUEUE(Q, v)`; the operations of
the standard data structures are ENQUEUE and DEQUEUE for a queue, PUSH and
POP for a stack.

### Indices start at 1

In pseudocode, arrays are indexed from 1 to `n`. In Python they are indexed
from 0 to `n-1`. This is a deliberate difference and a classic source of
off-by-one errors, so we always say which convention we are in: pseudocode,
mathematics, R and MATLAB count from 1, and Python, C and Java count from 0.
When a transcript shows the same algorithm twice, the pseudocode uses 1-based
indices and the Python uses 0-based ones. Compare them carefully; that is part
of the exercise.

### Example comparing indices

The same algorithm in both conventions:

```
Procedure SELECTION-SORT(A, n)
Inputs:
      A: an array
      n: the number of elements in A to sort
Result: the elements of A are sorted into nondecreasing order
For i = 1 to n - 1:
      smallest ← i
      For j = i + 1 to n:
            If A[j] < A[smallest]
                  then smallest ← j
      swap A[i] with A[smallest]
```

```python
def selection_sort(A):
    n = len(A)
    for i in range(n - 1):            # i = 0 … n-2
        smallest = i
        for j in range(i + 1, n):     # j = i+1 … n-1
            if A[j] < A[smallest]:
                smallest = j
        A[i], A[smallest] = A[smallest], A[i]
    return A
```

---

## 2. Numbers and bits

| Notation | Meaning |
|---|---|
| $47_8$, $39_{10}$, $100111_2$ | a number with its base as a subscript |
| `0x23A7` | hexadecimal, C/Python style |
| $1011\,1110_2$ | thin spaces group bits in fours for readability |
| $x_k$, $x_{-k}$ | the digit with weight $\beta^k$, or $\beta^{-k}$ after the point |
| $a \equiv b \pmod m$ | $a$ and $b$ have the same remainder when divided by $m$ |
| $\square$ or `#` | a blank cell on a Turing machine tape |

A *bit* is one binary digit. A *byte* is 8 bits. When we say that a code has
3 bits per symbol, we mean that each symbol is written with three binary
digits.

---

## 3. Data structures

| Notation | Meaning |
|---|---|
| $A$ | an array; $A[i]$ its $i$-th element; $n$ its size |
| $L$, $R$ | the sorted (left) and unsorted (right) parts of a list being sorted |
| $k$ | a key; $h(k)$ its hash value; $K$ the set of possible keys |
| $m$ | the number of slots in a hash table |
| $Q$ | a queue; $S$ a stack |

Tree pictures use one color scheme throughout the course: a gray node is the
root, a white node an internal node, and a green node a leaf.

---

## 4. Graphs

A graph is a pair $G = (V, E)$:

| Notation | Meaning |
|---|---|
| $V$ | the set of vertices (also called nodes) |
| $E$ | the set of edges |
| $\|V\|$, $\|E\|$ | how many vertices and edges there are |
| $\{u, v\}$ | an undirected edge between $u$ and $v$ |
| $(u, v)$ | a directed edge from $u$ to $v$ |
| $w(u,v)$ | the weight of the edge from $u$ to $v$ |
| $\text{neighbors}(u)$ | the vertices reachable from $u$ in one step |

In the search algorithms (BFS in session 5, DFS in session 6) each vertex $u$
carries:

| Notation | Meaning |
|---|---|
| $\text{color}[u]$ | WHITE = not seen, GRAY = seen, BLACK = finished |
| $d[u]$ | BFS: distance from the source. DFS: discovery time |
| $f[u]$ | DFS only: finishing time |
| $\pi[u]$ | the parent of $u$ in the search tree, or NIL |

We always write $\pi$ (pi) for the parent, never "prev" or "from". When the
algorithm has a choice of which vertex to visit next, take them in
alphabetical order. That makes your answer comparable with everyone else's.

---

## 5. Complexity

| Notation | Meaning |
|---|---|
| $n$ | the size of the input; for graphs, $\|V\|$ and $\|E\|$ |
| $T(n)$ | the running time of an algorithm on an input of size $n$ |
| $O(g)$ | the set of functions that grow no faster than $g$: an upper bound |
| $\Omega(g)$ | grow no slower than $g$: a lower bound |
| $\Theta(g)$ | both at once: a tight bound |

Formally, $f \in O(g)$ if there are positive constants $c$ and $n_0$ such
that $0 \le f(n) \le c\,g(n)$ for all $n \ge n_0$. Everyone writes $f = O(g)$
instead of $f \in O(g)$; it is an abuse of notation, it is completely
standard, and we use it too. Logarithms in $O(\cdot)$ are written without a
base, because the base only changes a constant factor; when the base matters,
as when counting the levels of a recursion, we write $\log_2 n$. The ordering
you should know by heart is

$$O(1) \subset O(\log n) \subset O(n) \subset O(n \log n) \subset O(n^2) \subset O(2^n) \subset O(n!).$$

---

## 6. Automata, Turing machines, and formal languages

| Notation | Meaning |
|---|---|
| $\Sigma$ | the input alphabet |
| $\Sigma^*$ | all finite strings over $\Sigma$, including the empty one |
| $\varepsilon$ | the empty word |
| $Q$ | the set of states; $q_0$ the start state |
| $\delta$ | the transition function |
| $X$ | the set of accept states of an automaton |
| $F$ | the set of final states of a Turing machine |
| $\Gamma$ | a Turing machine's working alphabet ($\Sigma \subset \Gamma$) |
| $\mathcal{P}(Q)$ | the power set of $Q$, used by nondeterministic transition functions |

A DFA is a 5-tuple $M = (Q, \Sigma, \delta, q_0, X)$ and a Turing machine is
a 7-tuple $TM = (Q, \Sigma, \Gamma, \delta, q_0, \square, F)$. In diagrams,
the start state has an incoming arrow from nowhere and accept states are
drawn with a double circle.

---

## 7. Machine learning

| Notation | Meaning |
|---|---|
| $X$ | the features (input variables); $x$ one feature vector |
| $Y$ | the target (what we predict); $y$ one true value |
| $\hat{y}$ | a predicted value; the hat always means "estimated" |
| $f$ | the model, so that $\hat{y} = f(x)$ |
| $d$ | the number of features |
| $n$ | the number of examples |
| $K$ | the number of clusters; $k$ the number of neighbors in kNN |
| $TP$, $FP$, $FN$ | true positives, false positives, false negatives; $F_1$ the harmonic mean of precision and recall |


