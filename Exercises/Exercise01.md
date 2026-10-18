# Exercise Sheet 1: Introduction

*Introduction to Computing for the Social Sciences*

**How to use exercise sheets.** The firs Exercises (here 1 and 2) are the ones we work through together
in the exercise session. Work on the exercises before and bring your attempts.
Solutions to the exercises can be found on Github but are not linked here.
Do not check the solutions until after we have tried solving them in class.
Additional practice exercises are provided in the end for you to practice more. You can ask questions about them in class if there is time.

Everything here is pen-and-paper. Notation follows the
[course conventions](../Transcripts/00-Conventions.md); remember that
**pseudocode arrays are indexed from 1**.

---

## Exercise 1: Selection sort

```
Procedure SELECTION-SORT(A, n)
Inputs:
      A: an array
      n: the number of elements in A to sort
Result: The elements of A are sorted into nondecreasing order

For i = 1 to n - 1:
      smallest ← i
      For j = i + 1 to n:
            If A[j] < A[smallest]
                  then smallest ← j
      swap A[i] with A[smallest]
```

Consider the selection-sort algorithm above. Show how it sorts each of the
following lists. Write down the state of the array after every iteration of the
outer loop, and mark which entries were compared and which were swapped.

a) `[4, 2, 5, 3, 1, 6]`
b) `[6, 5, 4, 3, 2, 1]`
c) `[4, 2, 3, 3, 1, 6]`
d) `[1, 2, 3, 4]`

Then answer:

- Do you need more steps for some lists than for others?
- What do you think influences the number of steps?
- Can you imagine how to make it faster?

---

## Exercise 2: I Can't Believe It Can Sort

```
Procedure ICANTBELIEVEITCANSORT(A, n)
Inputs:
      A: an array
      n: the number of elements in A to sort
Result: The elements of the array are sorted into nondecreasing order

For i = 1 to n:
      For j = 1 to n:
            If A[i] < A[j]
                  then swap A[i] with A[j]
```

Consider the algorithm above. Show how it sorts the following list:

$$[4,\; 2,\; 5,\; 3,\; 1,\; 6]$$

Then answer:

- Can you describe the way the algorithm works in your own words?
- Why is it surprising?

> Look at the comparison on line 3 very carefully and compare it with the one in
> selection sort. This algorithm was published in 2021 (Fung, 2021) precisely
> because it looks like it should be wrong.

---

## Additional practice

### A1. Reading a definition

Zelle defines a computer as *"a machine that stores and manipulates information
under the control of a changeable program"*. For each device below, say which
parts of the definition it satisfies and conclude whether it is a computer:

a) an abacus
b) a pocket calculator 
c) a mechanical music box
d) a modern washing machine
e) a slide rule

### A2. Counting operations in selection sort

For an array of $n$ elements:

a) How many times does the **outer** loop body run?
b) For a fixed $i$, how many comparisons does the **inner** loop make?
c) How many comparisons does the whole algorithm make, as a function of $n$?
d) Does the answer to (c) depend on the *contents* of the array? Why is that
   unusual?

### A3. Algorithm versus program

Write, in pseudocode, an algorithm that takes an array $A$ of $n$ numbers and
returns the **largest** one. Then translate it into Python. Then answer: how
many *different programs* could implement your one algorithm?

---

## Sources

- **Cormen, T. H. (2013).** *Algorithms Unlocked*
- **Fung, S. P. Y. (2021).** "Is this the simplest (and most surprising) sorting
  algorithm ever?" *arXiv:2110.01111*. <https://arxiv.org/abs/2110.01111>,
  The algorithm in exercise 2, with a proof that it works.
  A short video explanation: <https://www.youtube.com/watch?v=kPRA0W1kECg>
- **Zelle, J. M. (2016).** *Python Programming: An Introduction to Computer
  Science*
