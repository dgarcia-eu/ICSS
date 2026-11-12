# Exercise Sheet 4: Software

*Introduction to Computing for the Social Sciences · Prof. Dr. David Garcia*

Exercises 1 and 2 will be covered in the exercise session. The *Additional practice* section is for self-study and to be covered if time allows. 

---

## How to work on these exercises

Each exercise is a request from a client, the kind of message a colleague
might send you. You follow it through the software engineering flow of the
lecture, from requirements to maintenance. You do not need a computer. The
code is short, and the point of the exercise is everything around it.

a) **Requirements.** Write down what the software must achieve. Give at least
   three functional requirements and at least one non-functional requirement.
   List the questions you would ask the client before starting, and for each
   question, the answer you will assume if the client is not available.

b) **Specification.** Write the specification of the main function as a
   docstring. State the inputs and what may be assumed about them, the
   result, what happens in edge cases and errors, and what the function must
   never do.

c) **Design.** Split the program into two to four small functions, with one
   line each on what it does. Say which data structures connect them and why.

d) **Tests.** Write at least five test cases for the main function. Include a
   typical case, a boundary or empty case, a case with missing or invalid
   data, a case for something that must never happen, and one more of your
   choice. Write each as a pytest test, or as a table with input and expected
   result.

e) **Implementation.** The exercise gives code that a coder wrote from the client's message alone, without your
   specification. Run your tests on it with pen and paper. Which pass, which fail, and
   why?

f) **Maintenance.** The exercise ends with a second message from the client.
   Which of your functions change, which stay as they are, and which tests do
   you add or change?

There is more than one good answer to every step. What matters is that each
choice is written down and that the tests follow from the specification.

---

## Exercise 1: The mean age of each survey wave

> *"Hi! Could you write something that gives me the mean age of the
> respondents in each wave of our survey? The ages come straight from the
> online form. Thanks, Lea"*

The form data can be read into a list of dictionaries, one per respondent.
Each dictionary holds the wave as a number and the age as the text that was
typed, with `""` if the question was skipped.

```python
rows = [{"wave": 1, "age": "34"}, {"wave": 1, "age": ""},
        {"wave": 1, "age": " 27"}, {"wave": 2, "age": "19"},
        {"wave": 2, "age": "340"}, {"wave": 2, "age": "thirty"}]
```

Work through steps a) to f). The lecture specified `mean_age(ages)` for a
clean list of ages. Here the ages arrive as text, and per wave. In step a),
think about the entries `""`, `" 27"`, `"340"` and `"thirty"`.

For step e), this is the coder's function.

```python
def mean_age_by_wave(rows):
    total, count = {}, {}
    for row in rows:
        w = row["wave"]
        total[w] = total.get(w, 0) + int(row["age"] or 0)
        count[w] = count.get(w, 0) + 1
    return {w: total[w] / count[w] for w in total}
```

Also say what it returns, or what happens, on the example `rows` above.

For step f), two months later the client writes again.

> *"Great, thanks! Could you also give me the number of people who answered,
> and the median next to the mean? From now on the form has a drop-down menu
> for the age, and we are adding a third wave. Lea"*

---

## Exercise 2: How often do parties talk about climate?

> *"Hi! I have all speeches in the UK House of Commons from 2015 to 2024,
> with the speaker's name, party, date and text. Could you write something
> that tells me how often each party talks about climate, per year? I want to
> see whether the topic grew after 2019. Best, Tom"*

The speeches can be read into a list of dictionaries, one per speech.

```python
speeches = [
    {"speaker": "A. Smith", "party": "Labour", "date": "2019-05-01",
     "text": "The Climate Change Act was a start, but..."},
    {"speaker": "B. Jones", "party": "Conservative", "date": "2019-05-01",
     "text": "The business climate in my constituency..."},
    ...]
```

Work through steps a) to f). In step a), think about what "talks about
climate" means. Which words count? Does one speech with ten mentions count
more than ten speeches with one mention each? What about the two example
texts? And parties differ in how many speeches they give. Note that the
specification is where the measurement of a social science concept, here the
salience of a topic, gets decided.

For step e), the client's message went, as it was, to two coders, A and B.
Each wrote a function from it.

```python
def climate_counts(speeches):              # coder A
    counts = {}
    for s in speeches:
        key = (s["party"], s["date"][:4])
        counts[key] = counts.get(key, 0) + s["text"].count("climate")
    return counts

def climate_counts(speeches):              # coder B
    counts = {}
    for s in speeches:
        if "climate" in s["text"].lower():
            key = (s["party"], int(s["date"][:4]))
            counts[key] = counts.get(key, 0) + 1
    return counts
```

Run your tests on both. Then answer two questions. What do the two functions
measure, and on which kind of input do they give different results? What does
this mean for a result whose coder worked from the client's message instead
of a specification?

For step f), six months later the client writes again.

> *"Reviewer 2 says that 'climate' alone misses a lot. Can we add 'global
> warming' and 'net zero'? And can you run it on the 2025 speeches too? A
> colleague also wants the same numbers for migration. Tom"*

---

## Additional practice

### A1. Functional or non-functional?

For each requirement, say whether it is functional or non-functional. For the
non-functional ones, say which kind it is (security, privacy, safety,
performance or reproducibility).

a) The mean is computed only over the answered questions.
b) The program finishes in under a minute on the full survey of 50,000
   respondents.
c) The output file contains no names or email addresses.
d) An empty input list raises a `ValueError`.
e) The original data file is never overwritten.
f) Running the analysis twice on the same data gives the same result, also on
   a colleague's computer.
g) Only the course instructors can see the grades of all students.

### A2. The questions a coder will not ask

A colleague gives a coder, a person or a coding agent, the instructions
*"write a function that computes the age of each respondent from their birth
date"*. Write five questions whose answers the instructions leave open, and
rewrite the instructions so that they answer them.

### A3. Which test is missing?

The specification of `share_yes(answers)` says: *"answers is a list of "yes",
"no" or None. Returns the share of "yes" among the "yes" and "no" answers.
Raises ValueError if there is no "yes" or "no" answer. The list is not
modified."* These are the tests.

```python
def test_typical():
    assert share_yes(["yes", "no", "yes", "yes"]) == 0.75

def test_all_yes():
    assert share_yes(["yes", "yes"]) == 1.0
```

Which lines of the specification are not tested? Write the missing tests.
Then write an implementation that passes both tests above and is still wrong.

### A4. Counting survey answers

For A4 to A6, work through steps a) to d) of the session exercises.

*"Our survey has a question about how people get to work, with the options
'car', 'bus', 'bike' and 'walk'. Could you write something that tells me
how many people chose each option? I'll send you the answers as a list."*
The answers arrive as a list such as
`["bus", "bike", None, "car", "bus", "Bus"]`, with `None` where a
respondent skipped the question.

### A5. The seminar waiting list

*"Our seminar has 20 places and always more people who want in. People
register with their student ID. The first 20 get a place, the others go on
a waiting list, and when someone drops out, the next person from the
waiting list moves up."* Specify and test the two functions
`register(student_id)` and `drop(student_id)`, and say which data structure
holds the participants and which holds the waiting list.

### A6. Grades from exercise points

*"I have the exercise points of every student: up to 10 points for each of
the 12 sheets. Students pass the exercise part if they have at least 50% of
the points. Could you write something that gives me, for every student, the
total and whether they passed? The points are in a table that the teaching
assistants keep updating."* The table can be read into a dictionary from
student ID to a list of points, such as `{"01/123": [10, 8, None, 7]}`,
with `None` for a sheet that was not handed in.

---

## Sources

- **Zelle, J. M. (2016).** *Python Programming: An Introduction to Computer
  Science*, 3rd ed., ch. 2, 6–8, the reading for this session.
- **Guttag, J. V. (2016).** *Introduction to Computation and Programming Using
  Python*, 2nd ed., MIT Press, ch. 6 (testing and debugging).
- **pytest documentation.** <https://docs.pytest.org/>
- **PEP 257, Docstring Conventions.** <https://peps.python.org/pep-0257/>
- **Hansard**, the official record of the UK Parliament, publishes the
  speeches of the House of Commons at <https://hansard.parliament.uk/>.
