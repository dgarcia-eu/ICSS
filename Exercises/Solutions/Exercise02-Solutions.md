# Solutions to Exercise Sheet 2: Information Coding

*Introduction to Computing for the Social Sciences · Prof. Dr. David Garcia*

---

## Exercise 1: Tricked dice

a) The die is not fair, so a balanced tree of three questions is not optimal:
the two results with probability $1/5$ deserve fewer questions than the six
with probability $1/10$. Build the tree by repeatedly joining the two least
probable groups (Huffman's rule). Pairing the six $1/10$ faces gives three
groups of probability $1/5$, so together with faces 7 and 8 there are five
groups of $1/5$; joining them puts 7 and 8 and one pair at depth 2 and the
other two pairs at depth 3, that is, their faces at depth 4. One valid diagram,
with "yes" as the left branch and the codeword of each face written under it:

![](../figures/sol-02-dice-tree.svg)

Any tree with the same profile of depths (two faces at depth 2, two at depth
3, four at depth 4) is equally good; which faces go where is arbitrary.

b) The average number of questions is $\sum_i p_i d_i$, the probability of each
result times the number of questions it needs:
$$2 \cdot \tfrac{1}{5} \cdot 2 + 2 \cdot \tfrac{1}{10} \cdot 3 + 4 \cdot \tfrac{1}{10} \cdot 4 = 0.8 + 0.6 + 1.6 = 3.0 \text{ questions.}$$
A fair 8-sided die needs exactly 3 questions with a balanced tree, and this
biased die needs 3.0 on average with the tree above. The entropy of the
distribution, $6 \cdot \tfrac{1}{10}\log_2 10 + 2 \cdot \tfrac{1}{5}\log_2 5
\approx 2.92$ bits, is the lower bound, so no tree can do much better.

c) Write the answers as bits, "yes" as 1 and "no" as 0. Reading the tree from
the root, the codewords are: 7 = `11`, 8 = `10`, 5 = `011`, 6 = `010`,
1 = `0011`, 2 = `0010`, 3 = `0001`, 4 = `0000`. The sequence `1 1 8 2 3` becomes

```
0011 0011 10 0010 0001
```

18 bits for five results. A fixed code of 3 bits per result would use 15 bits
here, because this particular sequence is made of rare results; over a long
sequence of rolls the variable-length code uses 3.0 bits per result on average,
and it would beat any fixed code on a die biased more strongly than this one.

## Exercise 2: Conversion to octal

a) $1101111_2$: octal digits are groups of three bits, counted from the right:
`1 101 111` → $1, 5, 7$ → $\mathbf{157_8}$. Check: $64 + 40 + 7 = 111 =
1101111_2$.

b) $101_{16}$: each hexadecimal digit is four bits, `0001 0000 0001`, that is
$100000001_2$; regroup in threes from the right, `100 000 001` → $\mathbf{401_8}$.
Check: $101_{16} = 256 + 1 = 257$ and $401_8 = 256 + 1 = 257$.

c) $567_{10}$: repeated division by 8, reading the remainders upwards:
$567 = 70 \cdot 8 + 7$, $70 = 8 \cdot 8 + 6$, $8 = 1 \cdot 8 + 0$, $1 = 0 \cdot 8 + 1$
→ $\mathbf{1067_8}$. Check: $512 + 0 + 48 + 7 = 567$.

## Exercise 3: IEEE 754

Sign: positive, $S = 0$.

Integer part: $33 = 100001_2$. Fractional part, by repeated doubling:
$0.1 \to 0.2 \to 0.4 \to 0.8 \to 1.6 \to 1.2 \to 0.4 \to \ldots$, which gives
the digits $0.0\,0011\,0011\,0011\ldots$, repeating forever. So
$$33.1 = 100001.000110011001100\ldots_2 = 1.00001000110011001100\ldots_2 \times 2^5 .$$

Exponent: $5 + 127 = 132 = 10000100_2$.

Mantissa: the 23 bits after the leading 1 are `00001 000 1100 1100 1100 110`,
and the bits that follow begin with `0110…`, so the 23-bit mantissa is rounded
down: `00001000110011001100110`.

Putting the three fields together:

```
0 10000100 00001000110011001100110
0100 0010 0000 0100 0110 0110 0110 0110  =  0x42046666
```

**Is it exactly 33.1?** No. $33.1 = 331/10$ and the denominator has a factor of
5, so the binary expansion never terminates and the 23-bit mantissa cuts it
off. The value actually stored is $33.09999847\ldots$, slightly below 33.1,
because the rounding went down. Only fractions whose denominator is a power of
two (the dyadic rationals, like 33.125 = 265/8) are stored exactly.

## Additional practice

### A1. Base conversions, mixed

a) $205 : 2$ gives the remainders 1, 0, 1, 1, 0, 0, 1, 1, read upwards →
$1100\,1101_2$. Grouped in fours, `1100 1101` → $\texttt{0xCD}$.
b) Each hex digit is four bits: B = 1011, E = 1110, E = 1110, F = 1111 →
$1011\,1110\,1110\,1111_2$. Regroup in threes from the right,
`1 011 111 011 101 111` → $137357_8$.
c) $4 \times 8^0 + 5 \times 8^1 + 7 \times 8^2 = 4 + 40 + 448 = 492_{10}$.
d) $0.8125 \times 2 = 1.625 \to 1$; $0.625 \times 2 = 1.25 \to 1$;
$0.25 \times 2 = 0.5 \to 0$; $0.5 \times 2 = 1.0 \to 1$. So $0.1101_2$.
Check: $0.5 + 0.25 + 0.0625 = 0.8125$.
e) $0.2 \times 2 = 0.4 \to 0$; $0.4 \times 2 = 0.8 \to 0$; $0.8 \times 2 = 1.6 \to 1$;
$0.6 \times 2 = 1.2 \to 1$; $0.2 \times 2 = 0.4 \to 0$, and we are back at 0.2,
so the expansion repeats forever: $0.\overline{0011}_2$. The loop of
FRACTION-TO-BINARY never terminates. Any finite float is therefore an
approximation of 0.2, which is the root cause of `0.1 + 0.2 != 0.3`.

### A2. Two's complement arithmetic

a) $45 = 0010\,1101$; invert → $1101\,0010$; add 1 → $1101\,0011$.
b) $0010\,1101 + 1101\,0011 = 1\,0000\,0000$. The ninth bit is discarded, since
there are only 8, leaving `0000 0000` = 0. This carry-and-discard is exactly
why two's complement is used.
c) $100 = 0110\,0100$ and $50 = 0011\,0010$; the sum is $1001\,0110$. Interpreted
as two's complement this is $-128 + 16 + 4 + 2 = -106$, not 150. This is an
overflow: 150 does not fit in 8 signed bits. The arithmetic is correct bit by
bit; it is the interpretation that breaks.
d) Largest $0111\,1111 = 127$; smallest $1000\,0000 = -128$. The range is
asymmetric because zero takes one of the $2^8 = 256$ patterns from the positive
side, leaving 127 positive numbers, one zero and 128 negative numbers.

### A3. Reading IEEE 754

a) $S = 0$; $E = 1000\,0010_2 = 130$, so $e = 130 - 127 = 3$;
$m = 1 + 2^{-1} + 2^{-3} = 1.625$; value $= 1.625 \times 2^3 = 13$.
b) $S = 1$; $E = 0111\,1110_2 = 126$, so $e = -1$; $m = 1$;
value $= -1 \times 2^{-1} = -0.5$.
c) Exponent and mantissa both zero: this is the special pattern for **zero**
(specifically $+0$; with the sign bit set it would be $-0$).

### A4. Hexadecimal without a calculator

a) Align the digits first, since `0xC0C0A` has one digit fewer:

```
   C 0 F F E E
 - 0 C 0 C 0 A
   -----------
   B 4 F 3 E 4
```

So $\texttt{0xC0FFEE} - \texttt{0xC0C0A} = \texttt{0xB4F3E4}$. In decimal,
$12648430 - 789514 = 11858916 = \texttt{0xB4F3E4}$. The quickest check is to
add the answer back to `0xC0C0A`.
b) `0x100`: `0xFF` is 255, adding one carries out of both digits.
c) It is the UTF-8 **byte order mark** (BOM). It carries no textual content, but
a program that does not expect it reads it as part of the first field, which is
why a CSV column sometimes ends up named `﻿id` instead of `id`. A very common
and very annoying real-world encoding bug.

### A5. Information and encoding

a) Yes. With 16 equally likely outcomes you need 4 bits each. But if you
concentrate the probability, say outcome 1 has probability close to 1 and the
other 15 share what is left, then a code that gives outcome 1 a 1-bit codeword
and the rest longer ones has an average well below 2 bits. The general
statement is that the achievable average is Shannon's entropy
$H = -\sum_i p_i \log_2 p_i$, which is 4 bits for the fair die and can be made
arbitrarily small by biasing. "On average" is essential: no code can give every
one of 16 outcomes a 2-bit codeword, since there are only 4 of those.

b) Ask "is it option 1?" first: `1` → option 1 (1 bit). If not, two more
questions distinguish the remaining three: `01` → option 2, `001` → option 3,
`000` → option 4. The average is
$0.7 \times 1 + 0.1 \times 2 + 0.1 \times 3 + 0.1 \times 3 = 1.6$ bits, against
2 bits for the fixed-length code, a 20% saving. The entropy here is about 1.357
bits, so a better code exists, but no code can beat that bound.

### A6. Bit manipulation

a) $26 = 11010_2$. `26 >> 2` $= 110_2 = 6$, that is, $\lfloor 26/4 \rfloor$.
`26 << 3` $= 11010000_2 = 208 = 26 \times 8$. Shifting by $k$ multiplies or
integer-divides by $2^k$.
b) `(1234567890123456789 >> 22) + 1288834974657` = 1583178896824 ms since the
Unix epoch, which is **2020-03-02, 19:54:56 UTC**; check with
`datetime.fromtimestamp(1583178896.824, timezone.utc)`. The `>> 22` discards
the 12 sequence bits and 10 machine bits.
c) `x & 0b100` (equivalently `x & 4`), which is non-zero exactly when that bit
is set. Masking with a single 1-bit is the standard idiom.
