# Solutions to Exercise Sheet 1: Introduction

*Introduction to Computing for the Social Sciences*

---

## Exercise 1: Selection sort

In every iteration of the outer loop the inner loop compares `A[smallest]` with
each of `A[i+1] … A[n]`, so the comparisons are the same on every list: with
$n = 6$ they are $5 + 4 + 3 + 2 + 1 = 15$, and with $n = 4$ they are
$3 + 2 + 1 = 6$. What changes from list to list is which position holds the
minimum and whether a swap happens. The tables show the array after every
iteration of the outer loop; "min at" is the position found by the inner loop,
and a swap with itself is not a swap.

a) `[4, 2, 5, 3, 1, 6]`

| $i$ | compared with `A[smallest]` | min at | swap | array after |
|---|---|---|---|---|
| 1 | positions 2 to 6 | 5 (value 1) | 1 ↔ 5 | `[1, 2, 5, 3, 4, 6]` |
| 2 | positions 3 to 6 | 2 (value 2) | none | `[1, 2, 5, 3, 4, 6]` |
| 3 | positions 4 to 6 | 4 (value 3) | 3 ↔ 4 | `[1, 2, 3, 5, 4, 6]` |
| 4 | positions 5 to 6 | 5 (value 4) | 4 ↔ 5 | `[1, 2, 3, 4, 5, 6]` |
| 5 | position 6 | 5 (value 5) | none | `[1, 2, 3, 4, 5, 6]` |

Three swaps, fifteen comparisons.

b) `[6, 5, 4, 3, 2, 1]`

| $i$ | min at | swap | array after |
|---|---|---|---|
| 1 | 6 (value 1) | 1 ↔ 6 | `[1, 5, 4, 3, 2, 6]` |
| 2 | 5 (value 2) | 2 ↔ 5 | `[1, 2, 4, 3, 5, 6]` |
| 3 | 4 (value 3) | 3 ↔ 4 | `[1, 2, 3, 4, 5, 6]` |
| 4 | 4 (value 4) | none | `[1, 2, 3, 4, 5, 6]` |
| 5 | 5 (value 5) | none | `[1, 2, 3, 4, 5, 6]` |

Three swaps, fifteen comparisons. The reversed list is sorted after three
swaps because every swap places two elements at once.

c) `[4, 2, 3, 3, 1, 6]`

| $i$ | min at | swap | array after |
|---|---|---|---|
| 1 | 5 (value 1) | 1 ↔ 5 | `[1, 2, 3, 3, 4, 6]` |
| 2 | 2 (value 2) | none | `[1, 2, 3, 3, 4, 6]` |
| 3 | 3 (value 3) | none | `[1, 2, 3, 3, 4, 6]` |
| 4 | 4 (value 3) | none | `[1, 2, 3, 3, 4, 6]` |
| 5 | 5 (value 4) | none | `[1, 2, 3, 3, 4, 6]` |

One swap, fifteen comparisons. Note the strict comparison `A[j] < A[smallest]`:
with two equal values 3 the first one found stays the minimum, which is why the
duplicate is not moved.

d) `[1, 2, 3, 4]`

| $i$ | min at | swap | array after |
|---|---|---|---|
| 1 | 1 | none | `[1, 2, 3, 4]` |
| 2 | 2 | none | `[1, 2, 3, 4]` |
| 3 | 3 | none | `[1, 2, 3, 4]` |

No swap, six comparisons.

**The questions.** You do not need more comparisons for some lists than for
others. The loops run to their bounds whatever the contents, so the number of
comparisons depends only on the length $n$ and is $n(n-1)/2$. The number of
swaps depends on the contents, though. What influences the total
work is $n$, and it grows with the square of $n$: doubling the list
quadruples the comparisons. To make it faster you would have to change the
method rather than the list. Stopping early does not help selection sort, since
it cannot tell that the rest is already sorted without scanning it.

## Exercise 2: I Can't Believe It Can Sort

The array after each iteration of the outer loop, on `[4, 2, 5, 3, 1, 6]`:

| $i$ | swaps during the inner loop | array after |
|---|---|---|
| 1 | $j=3$: 4 < 5, swap → `[5,2,4,…]`; $j=6$: 5 < 6, swap | `[6, 2, 4, 3, 1, 5]` |
| 2 | $j=1$: 2 < 6, swap | `[2, 6, 4, 3, 1, 5]` |
| 3 | $j=2$: 4 < 6, swap | `[2, 4, 6, 3, 1, 5]` |
| 4 | $j=2$: 3 < 4, swap → `[2,3,6,4,…]`; $j=3$: 4 < 6, swap | `[2, 3, 4, 6, 1, 5]` |
| 5 | $j=1$: 1 < 2, swap → `[1,3,4,6,2,5]`; then 2 < 3, 3 < 4, 4 < 6: three more swaps | `[1, 2, 3, 4, 6, 5]` |
| 6 | $j=5$: 5 < 6, swap | `[1, 2, 3, 4, 5, 6]` |

**How it works.** Look at what is true after the outer loop has finished
iteration $i$: the prefix `A[1..i]` is sorted, and `A[i]` holds the largest
element of the whole array. The trace shows it: after $i = 1$ the maximum 6 sits
in position 1; after $i = 2$ the prefix is `[2, 6]`; after $i = 3$ it is
`[2, 4, 6]`, and so on. When the inner loop runs for a new $i$, the comparisons
with $j < i$ first insert `A[i]` into its place in the sorted prefix by a chain
of swaps, and the comparisons with $j > i$ then pull the largest of the
remaining elements into position $i$. So the algorithm is an insertion sort in
disguise, one that also carries the maximum along.

**Why it is surprising.** The comparison on line 3 looks wrong. Selection sort
swaps when `A[j] < A[smallest]` in order to move small elements to the front. However,
this algorithm swaps when `A[i] < A[j]`, which looks like it should sort in
decreasing order, and it compares every pair $(i, j)$ including $j > i$ and
$j = i$ (where nothing happens). It has no notion of a sorted and an unsorted
part, no `smallest` variable and no `min` search, and it still terminates with
the array in nondecreasing order. Fung (2021) published it precisely because
every reader's first reaction is that it cannot be correct, and the proof that
it is takes a page.

## Additional practice

### A1. Reading a definition

a) **Abacus**: it stores and manipulates information, but a person does the
manipulating. Not automatic and not programmable, so not a computer.
b) **Calculator**: it stores and manipulates information automatically, but the
set of operations is fixed, so it is not programmable. Not a computer, although
a programmable calculator is one.
c) **Music box**: it is automatic, and it even has a changeable "program" if the
cylinder can be swapped, but it does not store or manipulate information in any
useful sense; it produces sound. It is a borderline case, and the useful answer
is to say why it is borderline.
d) **Washing machine**: it contains a small computer. It stores information (the
selected program, sensor readings), manipulates it, works automatically, and its
firmware is changeable. It is a computer with a very narrow user interface.
e) **Slide rule**: a person does everything. Not a computer.

### A2. Counting operations in selection sort

a) $n - 1$ times, for $i = 1, \dots, n-1$.
b) The inner loop runs for $j = i+1, \dots, n$, so it makes $n - i$ comparisons.
c) $\sum_{i=1}^{n-1} (n-i) = (n-1) + (n-2) + \dots + 1 = \dfrac{n(n-1)}{2}$.
d) No: it makes exactly the same number of comparisons on a sorted array as on
a reversed one. That is unusual, because most sorting algorithms have a best
case that is much faster than their worst case; insertion sort in session 8 is
the standard example. The swap count of selection sort varies a little, but the
comparison count is fixed. In session 8 we will say this as: selection sort is
$\Theta(n^2)$ in the best, the average and the worst case.

### A3. Algorithm versus program

```
Procedure MAXIMUM(A, n)
Inputs:
      A: an array
      n: the number of elements in A
Result: the largest element of A
largest ← A[1]
For i = 2 to n:
      If A[i] > largest
            then largest ← A[i]
return largest
```

```python
def maximum(A):
    largest = A[0]              # note the index shift
    for i in range(1, len(A)):
        if A[i] > largest:
            largest = A[i]
    return largest
```

There is an unbounded number of programs that implement this one algorithm. Different languages,
different variable names, iterating with an index or directly over the
elements, recursion instead of a loop: all of these are the same algorithm
implemented differently. This is exactly why we write pseudocode: it names the
method, not one of its implementations.
