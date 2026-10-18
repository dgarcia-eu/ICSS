# Session 1: Introduction

*Introduction to Computing for the Social Sciences · lecture transcript*

> This is the companion text for the first lecture. It covers everything on the
> slides plus the explanations that were given out loud, so you can study from
> it alone. Notation follows [the course conventions](00-Conventions.md).

---

## Contents

1. [Why a computing course in a social science degree](#1-why-a-computing-course-in-a-social-science-degree)
2. [What the course is about](#2-what-the-course-is-about)
3. [What is a computer?](#3-what-is-a-computer)
4. [What is computer science?](#4-what-is-computer-science)
5. [What is an algorithm?](#5-what-is-an-algorithm)
6. [Computing as an artificial science](#6-computing-as-an-artificial-science)
7. [A short history](#7-a-short-history)
8. [The subfields of computer science](#8-the-subfields-of-computer-science)
9. [Check yourself](#9-check-yourself)
10. [References](#10-references)

---

## 1. Why a computing course in a social science degree

### The digital society

More and more of what people do passes through digital technologies. Looking
for a job, talking to a family member on the phone, sharing open source code,
posting a video on a social networking site, paying at the supermarket with a
card, or chatting with your boss on a company messenger are ordinary actions
that a few decades ago left no record at all, and that today leave a record in
the database of some company. The growth in adoption and evolution of these
technologies is the *digital transformation* of society, and it has three
consequences that matter for this course. 

The first is *datafication*: digital
technologies generate data traces that persist and can be analyzed. A phone
call in 1990 left nothing behind but a line on a bill, while a message today is
a timestamped, addressed and searchable object, so social scientists have gained
access to behavioral records at a scale that survey research could never reach.

The second consequence is that code mediates behavior. How people interact with
a technology is determined by the software running on it, and the design of that
software implements *algorithms*, that is, descriptions of the rules by which
the technology operates. If you want to explain why people behave in a certain
way on a platform, part of the explanation is in the algorithms of the platform.

The third consequence is that the technologies use the data produced by humans
inside their own algorithms, for example in machine learning models that
personalize content or make predictions. The output of such a model changes what
people see, which changes what they do, which changes the data on which the next
model is trained.

### The computational turn in research

Computational methods are a precondition for working with this kind of data,
not a convenience. They give us:

- automation and scalability, because nobody can process millions of records by hand
- robustness, because with code we can run the same analysis again under different assumptions or parameters and see
whether the conclusion survives
- and reproducibility, because code is a precise and shareable statement of what we did, which somebody else can run. 

The problem
is that most social scientists arrive with a working knowledge of high-level
software such as Excel, SPSS or STATA and no understanding of the tools
underneath. That is enough until the research question gets harder. At that
point you need to automate the retrieval and handling of data, to design an
algorithm that finishes before the deadline and to run it on some computational
infrastructure, and to write software that implements a method that nobody has
packaged yet. This course exists to give you the fundamental understanding that
makes those advanced skills possible.

### Why understanding computation is not just technical

Understanding computation is also a requirement for understanding several of the
debates that social scientists care about, because at bottom they are debates
about computation. The design of a platform and of its ranking algorithms
shapes the behavior we observe on it, and some platforms contain non-human
actors, such as bots, that appear in the data as if they were people (*platform
biases*). Not everyone can access the same data, and research based on data that
only one company can grant may be impossible for anyone else to reproduce
(*data gatekeepers*). Talking online is not the same as talking offline: people
have purposes when they post, and those purposes can distort the behavior we
measure (*performative behavior*). Not everyone leaves digital traces, and both
measurement methods and predictive models can carry biases that lead to
discriminatory outcomes (*representativity issues*). Data is people, so privacy
expectations and the downstream consequences of a technology raise ethical
questions that are genuinely new (*privacy and ethics*). Finally, very large
datasets make black-box prediction easy and understanding hard, since a model
can be accurate and still explain nothing (*the data deluge*). You cannot
evaluate any of these claims without knowing what an algorithm is, what a model
does, and what it means when someone says that the computer decided.

---

## 2. What the course is about

The overarching goal of the course is that you become able to solve problems
computationally. More concretely, by the end of the course you should be able to
appreciate, understand and use the ideas at the core of computer science and
programming, to show familiarity with the major algorithms and data structures,
to use computational techniques for problems in the social sciences, and to use
Python for simple computational tasks. The theory part covers the principles of
information coding, computer systems and information storage, data types, data
structures and algorithms, the main programming paradigms, and the basic
concepts of databases and parallel programming. Concepts and theory are taught
in the lectures, and the assignments of the practical part rely wherever
possible on real-world examples and data.

One warning is worth repeating from the lecture: practice and self-learning are
essential in computing. Knowing the concepts is not the same as being able to
use them. The pen-and-paper exercises exist because writing a trace by hand is
what turns having seen selection sort into being able to reason about selection
sort, and the same holds for every other topic in the course.

---

## 3. What is a computer?

Zelle (2016, chapter 1) defines a computer as *a machine that stores and
manipulates information under the control of a changeable program*. Each part
of the definition excludes something. An abacus stores and manipulates
information, but a person moves the beads, so it is not a machine in the
required sense: nothing happens automatically. A pocket calculator is
automatic, but it is not programmable; you can change the numbers you feed it,
but not the kinds of things it can do. What makes a computer a computer is the
changeable program, which lets the same physical device become a word
processor, a statistics package or a game depending on which program it is
running. This is why the word covers such a varied list of objects today:
personal computers, servers, smartphones, fridges, photo cameras and watches
are all the same kind of machine.

| Device | Stores and manipulates information? | Automatic? | Programmable? | A computer? |
|---|---|---|---|---|
| Abacus | yes | no, a person moves the beads | no | no |
| Pocket calculator | yes | yes | no, the operations are fixed | no |
| Laptop, phone, smart fridge, camera, watch | yes | yes | yes | yes |

The idea of a stored, changeable program is less obvious than it looks. It only
became standard in the 1940s. A
machine that can run any program is a very different object from a machine that
does one job.

---

## 4. What is computer science?

Computer science is the scientific and practical approach to computation. It
studies how to enable the collection, representation, processing, storage and
access to information, and it includes the theory of computation, the study of
algorithmic processes, and the design of computational machines. A line that is
usually attributed to Edsger W. Dijkstra makes the point sharply: *computer
science is no more about computers than astronomy is about telescopes*. The
telescope is the instrument and the stars are the subject, and in the same way
the computer is the instrument and computation is the subject. This course is
about computation, which is why a large part of it involves no computer at all
and can be done with pen and paper.

---

## 5. What is an algorithm?

An *algorithm* is a self-contained, step-by-step set of operations to solve a
given problem. Algorithms perform calculation, data processing and automated
reasoning tasks. Both parts of the definition matter. Self-contained means that
the algorithm does not rely on any outside cleverness, only on the steps that
are written down. Step-by-step means that each step is unambiguous, so that two
people following the same algorithm get the same result. We will make this much
more precise in session 5, where we discuss the properties an algorithm must
have, and in session 9, where the word gets a formal definition in terms of
Turing machines. For now, an example is the best introduction.

### Example: selection sort

The problem is to sort a list of elements into nondecreasing order. The idea of
selection sort is to find the smallest element and put it first, then to find
the smallest of what is left and put it second, and so on. The algorithm is the
set of step-by-step operations that carries out that idea:

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

The same algorithm can be written in Python. Note the shift in the indices,
which is explained in the [conventions](00-Conventions.md): pseudocode counts
from 1 and Python counts from 0.

```python
def selection_sort(A, n):
    for i in range(n - 1):
        smallest = i
        for j in range(i + 1, n):
            if A[j] < A[smallest]:
                smallest = j
        # swap
        temp = A[i]
        A[i] = A[smallest]
        A[smallest] = temp
    return A
```

### Example of algorithm trace

Let us sort the array `[5, 2, 9, 1]`. In the table, the vertical bar separates
the part that is already sorted from the rest.

| step | array | what happens |
|---|---|---|
| start | `| 5 2 9 1` | nothing sorted yet |
| i = 1 | `1 | 2 9 5` | the smallest of positions 1 to 4 is 1, at position 4; swap it with position 1 |
| i = 2 | `1 2 | 9 5` | the smallest of positions 2 to 4 is 2, already in place; it is swapped with itself |
| i = 3 | `1 2 5 | 9` | the smallest of positions 3 to 4 is 5, at position 4; swap it with position 3 |
| done | `1 2 5 9` | the last element must already be the largest |

Two observations from this trace will be useful later in the course. The outer
loop runs $n-1$ times and not $n$, because once $n-1$ elements are in place the
last one has nowhere else to go. And the number of comparisons does not depend
on the input at all: it is always $(n-1) + (n-2) + \dots + 1$, so selection
sort takes the same time on a list that is already sorted as on a shuffled one.
We will count this properly in session 8.

### Pseudocode versus code

We wrote the algorithm twice, and the two versions have different purposes.
The pseudocode is for people and the Python is for a machine. Pseudocode lets
us state the idea without deciding things that do not matter yet, such as which
language to use, which data structure holds the elements, or whether indices
start at 0 or at 1. Leaving those decisions open is deliberate, because it
keeps the description of the method separate from the details of one
implementation. Session 5 is about writing good pseudocode.

---

## 6. Computing as an artificial science

Herbert Simon (1916–2001) was a political scientist whose work was influential
enough in economics to win him the Nobel Prize in 1978, and influential enough
in computer science to win him the Turing Award in 1975. In *The Sciences of
the Artificial* (1969) he draws a distinction that is useful for placing this
course. The natural sciences study what exists independently of us: physics
studies matter and biology studies organisms. The artificial sciences study
what humans create, such as economies, institutions and programs. The
consequence that Simon emphasizes is that the artificial sciences are not only
descriptive but also *prescriptive*: they ask not only how things are but what
we should do. Economists look for a tax incentive that drives innovation,
sociologists look for a policy that reduces gender inequality, and computer
scientists look for the program that lets a robot find its way out of a maze,
or that decides whom to interview for a job, whom to grant parole, or where to
place refugees in a country.

Prescriptive sciences study phenomena with a purpose, and computation has a
purpose too, usually stated as solving a problem. This gives computer science a
two-level structure that will recur during the whole term. The first level is
designing and testing methods, that is, building an algorithm and checking that
it works. The second level is methodology, the systematic study of methods and
their properties: whether an algorithm is correct, whether it terminates, how
fast it is, and whether it is the best possible. The second level is what makes
computer science a science rather than a craft, and it is where most of this
course lives. Notice also the political weight of Simon's examples. If computer
scientists design the program that decides who gets parole, then a design
decision is a policy decision. This is the thread that runs through the ethics
material of session 10 and the applications of session 13.

---

## 7. A short history

You are not asked to memorize dates. The point of the timeline is that each
entry adds one idea, and that the ideas accumulate.

| When | What | The idea it adds |
|---|---|---|
| c. 300 BC | Euclid's algorithm | a procedure can be stated exactly and will always terminate with the right answer (we implement it in session 6) |
| c. 820 | al-Khwarizmi's book on Hindu-Arabic numerals | systematic manipulation of symbols; his name becomes the word algorithm (session 5) |
| 1305 | Ramon Llull's mechanical logic | reasoning itself might be mechanized |
| 1837 | Babbage's Analytical Engine | a machine that is programmed rather than built for one task |
| 1843 | Ada Lovelace's program for the Analytical Engine | the first published program |
| 1941 | Zuse's Z3 | the first working programmable computer |
| 1944 | Colossus Mark 2 | electronic computing applied to a real problem, breaking the Lorenz cipher |
| 1954 | TRADIC at Bell Labs | transistors: small, reliable and cheap enough to spread |
| 1969 | the first message on the ARPANET | computers as a network rather than isolated machines |
| 1972 | the C programming language | portable systems programming (session 4) |
| 1981 | the IBM PC | computing as a consumer good |
| 1990 | the World Wide Web at CERN | information, not just computation, as the main application |
| 1997 | Deep Blue beats Garry Kasparov | search plus hardware beats human expertise in a bounded domain |
| 2016 | AlphaGo beats Lee Sedol | learned evaluation beats hand-written rules (session 10) |
| 2022 | ChatGPT | generative models reach the general public |

---

## 8. The subfields of computer science

A rough map of the discipline helps to place the sessions of this course.
Computer engineering covers microprocessor technology, computer architecture
and the communication between machines; we touch on it in sessions 2 and 4.
Theoretical computer science covers automata theory and formal languages,
complexity theory and quantum computing; sessions 8 and 9 belong here. Applied
computer science covers programming languages, algorithms and data structures,
operating systems, databases and software development, which is the content of
sessions 3 to 7, 11 and 12. Finally, there are interdisciplinary fields that
apply computing to the problems of another discipline: business informatics,
computer vision, artificial intelligence, computational linguistics,
bioinformatics, medical informatics, media informatics, human-computer
interaction, and computational social science, which is the reason you are in
this course and the subject of sessions 10 and 13.

---

## 9. Check yourself

Answers below; try first.

1. Is a mechanical clock a computer? What about a modern digital watch?
2. Selection sort on `[3, 1, 2]`: how many comparisons does the algorithm make?
   How many swaps?
3. Why does the outer loop of selection sort stop at $n-1$ instead of $n$?
4. In your own words, what is the difference between an algorithm and a program?
5. Give one example, from your own field, of a prescriptive question in
   Simon's sense.

<details>
<summary>Answers</summary>

1. Neither. Both store and manipulate information automatically, but neither
   runs a changeable program: you cannot make a clock compute your tax return.
   A smart watch, on the other hand, is a computer.
2. For $i=1$ the algorithm compares positions 2 and 3 against the current
   smallest, which is 2 comparisons; for $i=2$ it compares position 3, which is
   1 comparison. The total is 3, which is $(n-1)+(n-2) = 2+1$. There are 2
   swaps, one per iteration of the outer loop, although one of them may swap an
   element with itself.
3. After $n-1$ elements have been placed, only one element remains, and
   everything smaller than it has already been removed, so it is necessarily
   the largest and is already in the right place.
4. An algorithm is the method: a language-independent description of the
   steps. A program is one implementation of an algorithm in a particular
   language for a particular machine. One algorithm can have many programs.
5. Open. Two examples: which electoral system should a country adopt, and which
   moderation rule should a platform apply. Both ask what we should do, and both
   can be studied by building and testing designs rather than only by observing.

</details>

---

## 10. References

Cited in this transcript, and where to go next.

- **Zelle, J. M. (2016).** *Python Programming: An Introduction to Computer
  Science*, 3rd edition. Franklin, Beedle & Associates. 
- **Simon, H. A. (1969).** *The Sciences of the Artificial*. MIT Press.
- **Cormen, T. H. (2013).** *Algorithms Unlocked*. MIT Press.
- **Evans, D. (2011).** *Introduction to Computing: Explorations in Language,
  Logic, and Machines*. Creative Commons, free at <https://computingbook.org/>.
  The natural next step if you want to go deeper than the lectures.
- **The Dijkstra quotation.** The line about astronomy and telescopes is
  universally attributed to Edsger W. Dijkstra, but it does not appear in his
  writings. Its earliest documented appearance in print is as an anonymous
  saying in George Johnson's *Machinery of the Mind* (1986), and it has also
  been credited to Alan Perlis. Treat it as a proverb of the field rather than
  as a citation.
- **Lovelace, A. (1843).** "Notes by the Translator", appended to L. F.
  Menabrea, *Sketch of the Analytical Engine Invented by Charles Babbage*. Note
  G contains the Bernoulli-number program.
  <https://www.fourmilab.ch/babbage/sketch.html>

