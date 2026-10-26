# Exercise Sheet 2: Information Coding

*Introduction to Computing for the Social Sciences · Prof. Dr. David Garcia*

---

## Exercise 1: Tricked dice

Consider a tricked 8-sided die such that the probabilities of the results are:

| result | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|---|---|---|---|---|---|---|---|---|
| probability | 1/10 | 1/10 | 1/10 | 1/10 | 1/10 | 1/10 | 1/5 | 1/5 |

a) Write a diagram of yes/no questions that determines the result of a roll.
b) Based on your diagram, how many questions on average will you need to ask to
   know the result of a roll?
c) Based on your diagram, encode in binary the sequence of results
   `1 1 8 2 3`, writing a "yes" answer as 1 and a "no" answer as 0.

(Hint: Think about which results you should ask about **first**. A fair die is best encoded by a balanced tree but this one is not fair.)

---

## Exercise 2: Conversion to octal

Convert the following numbers to octal notation, showing how you do it.

a) $1101111_2$
b) $101_{16}$
c) $567_{10}$

---

## Exercise 3: IEEE 754

Encode the number **33.1** using the single-precision IEEE 754 floating-point
standard.

*(Note: this 32-bit standard has 1 sign bit, 8 bits for the exponent with a bias
of 127, and a 23-bit mantissa with an implicit leading 1.)*

Then answer: is the value you encoded exactly 33.1? Explain.

---

## Additional practice

### A1. Base conversions, mixed

Convert, showing your method each time:

a) $205_{10}$ to binary, then to hexadecimal
b) $\texttt{0xBEEF}$ to binary, then to octal
c) $754_8$ to decimal
d) $0.8125_{10}$ to binary
e) $0.2_{10}$ to binary, what goes wrong?

### A2. Two's complement arithmetic

Work in **8-bit two's complement** throughout.

a) Write $-45$ in binary.
b) Compute $45 + (-45)$ in binary and confirm the result.
c) Compute $100 + 50$ in binary. What is the answer? Is it correct?
d) What is the largest and the smallest number representable? Why is the range
   asymmetric?

### A3. Reading IEEE 754

Decode the following single-precision values into decimal:

a) `0|10000010|10100000000000000000000`
b) `1|01111110|00000000000000000000000`
c) `0|00000000|00000000000000000000000`

### A4. Hexadecimal without a calculator

a) Which hexadecimal number must you subtract from `0xC0FFEE` to get `0xC0C0A`?
b) Compute `0xFF + 0x01` in hexadecimal.
c) You see the byte sequence `0xEF 0xBB 0xBF` at the start of a text file. Look
   it up: what is it, and why might it break a program that reads the file?

### A5. Information and encoding

a) You can bias a 16-sided die, i.e. change the probabilities of its results. Can
   you do it in a way that makes it possible to store the results with only two
   bits per result on average? Reason your answer.
b) A survey has a question with four answer options, chosen with probabilities
   0.7, 0.1, 0.1, 0.1. Design a prefix code for it and compute the average number
   of bits per answer. Compare with the fixed-length code (2 bits each).

### A6. Bit manipulation

a) What is `26 >> 2`? What is `26 << 3`? Explain what each does arithmetically.
b) Given a tweet ID `1234567890123456789`, recover the posting timestamp using
   the snowflake layout from the lecture.
c) You want to test whether the third-lowest bit of an integer `x` is set. Write
   the expression.

---

## Sources

- **Evans, D. (2011).** *Introduction to Computing*, ch. 1.
  <https://computingbook.org/>
- **Shannon, C. E. (1948).** "A Mathematical Theory of Communication".
  *Bell System Technical Journal* 27, 379–423.: Background for A5; the entropy
  bound quoted there is his.
- **Goldberg, D. (1991).** "What Every Computer Scientist Should Know About
  Floating-Point Arithmetic". *ACM Computing Surveys* 23(1).
  <https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html>,
  Background for A3.
- **Twitter snowflake IDs** (A6): <https://github.com/oduwsdl/tweetedat>
