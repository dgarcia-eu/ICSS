# Solutions to Exercise Sheet 4: Software

*Introduction to Computing for the Social Sciences · Prof. Dr. David Garcia*

These are model answers. Other choices are just as good if they are written
down and the tests follow from them. The questions to the client are the part
that matters most, because each one is a decision that would otherwise be
made silently, by you or by whoever writes the code. All code below was run and the
reference implementations pass every test included here.

## Exercise 1: The mean age of each survey wave

**a) Requirements.**

Functional:

1. The result gives, for every wave in the data, the mean of the valid ages
   and the number of valid ages it is based on.
2. A skipped question (`""`) does not count, neither in the sum nor in the
   number of respondents.
3. Spaces around a number are ignored: `" 27"` is 27.
4. An age is valid if it is a whole number in the age range of the survey,
   assumed 18 to 99. Anything else, such as `"340"` or `"thirty"`, is
   counted as invalid and reported, not silently dropped and not silently
   used.
5. A wave without a single valid age is reported as such, without a mean,
   and does not stop the other waves from being computed.

Non-functional: the rows are only read, never changed (safety); the result
contains counts and means, nothing about individual respondents (privacy);
the same data gives the same result, in the same order of waves
(reproducibility).

Questions to the client, with the assumed answer:

| Question | Assumed answer |
|---|---|
| What is the age range of the survey? | 18 to 99; anything outside is a typing error. |
| What should happen with ages that are not a number, or out of range? | Leave them out of the mean, and report how many there were per wave. |
| Should skipped answers count? | No; report how many there were. |
| Should the mean be rounded? | Not in the function; the report shows one decimal. |
| Should "thirty" be read as 30? | No; it is counted as invalid. Guessing would make the result depend on how well we guess. |

The lecture's `mean_age(ages)` took a clean list of ints; most of the work
here is in getting from the form's text to that list, and that is where the
requirements are.

**b) Specification.**

```python
def mean_age_by_wave(rows, low=18, high=99):
    """Return the mean of the valid ages in each wave.

    rows: a list of dicts with "wave" (int) and "age" (str, the text typed
          in the form; "" if the question was skipped).
    An age is valid if, without surrounding spaces, it is a whole number
    from low to high. "" is a skipped answer; any other text is invalid.
    Returns: a dict from each wave in rows, in increasing order, to a dict
             with "mean" (the mean of the valid ages, a float, not rounded;
             None if the wave has no valid age), "answered" (the number of
             valid ages), "skipped" and "invalid".
    An empty list gives {}. The rows are not modified.
    """
```

**c) Design.**

- `classify(text, low, high)` returns `("answered", age)`, `("skipped", None)`
  or `("invalid", None)` for one text from the form. All the rules about what
  a valid age is live here.
- `mean_age(ages)` is the lecture's function: the mean of a non-empty list of
  ints, raising `ValueError` for an empty list.
- `mean_age_by_wave(rows)` groups the valid ages by wave, counts the three
  kinds of answer, and calls `mean_age` for each wave, with `None` where it
  raises.
- `report(result)` prints one line per wave, with the mean rounded to one
  decimal and the three counts.

A **dictionary** from wave to a **list** of valid ages connects the parts:
grouping is a lookup by key, and the list collects the ages of one wave in
any order. A second dictionary from wave to the three counts keeps the
skipped and invalid answers visible. Reusing `mean_age` means the part that
was already specified and tested in the lecture is not written again.

**d) Tests.**

```python
ROWS = [{"wave": 1, "age": "34"}, {"wave": 1, "age": ""},
        {"wave": 1, "age": " 27"}, {"wave": 2, "age": "19"},
        {"wave": 2, "age": "340"}, {"wave": 2, "age": "thirty"}]

def test_example_from_the_form():
    r = mean_age_by_wave(ROWS)
    assert r[1] == {"mean": 30.5, "answered": 2, "skipped": 1, "invalid": 0}
    assert r[2] == {"mean": 19.0, "answered": 1, "skipped": 0, "invalid": 2}

def test_boundaries_of_the_age_range():
    rows = [{"wave": 1, "age": a} for a in ["18", "99", "17", "120"]]
    r = mean_age_by_wave(rows)[1]
    assert r["answered"] == 2 and r["invalid"] == 2 and r["mean"] == 58.5

def test_wave_without_valid_age():
    assert mean_age_by_wave([{"wave": 3, "age": ""}])[3]["mean"] is None

def test_empty_list():
    assert mean_age_by_wave([]) == {}

def test_counts_add_up():
    r = mean_age_by_wave(ROWS)
    assert sum(r[w][k] for w in r
               for k in ("answered", "skipped", "invalid")) == len(ROWS)

def test_rows_are_not_modified():
    rows = [{"wave": 1, "age": " 27"}]
    mean_age_by_wave(rows)
    assert rows == [{"wave": 1, "age": " 27"}]
```

The boundary test checks both sides of both limits, 17 and 18, 99 and 120,
because `<` instead of `<=` is the most common way to get a range wrong.

**e) Implementation.** On the example rows, the coder's function stops with
`ValueError: invalid literal for int() with base 10: 'thirty'`. That is the
good failure: it is loud. Without the last row it returns
`{1: 20.33, 2: 179.5}`, and both numbers are wrong in the quiet way:

- `int(row["age"] or 0)` turns the skipped answer `""` into an age of 0, and
  the respondent is counted. Wave 1 gets $(34 + 0 + 27) / 3 = 20.3$ instead of
  $(34 + 27) / 2 = 30.5$. This is the bug of the lecture's first
  `mean_age`, in a new form.
- `"340"` is a valid int, so the typing error enters the mean: wave 2 gets
  $(19 + 340) / 2 = 179.5$.
- `int(" 27")` is 27, because Python's `int` ignores surrounding spaces, so
  this case happens to work.

Against the tests of d): `test_example_from_the_form` and `test_counts_add_up`
fail with the `ValueError` on `"thirty"`; `test_boundaries_of_the_age_range` and
`test_wave_without_valid_age` fail because the function returns a bare float
instead of the specified dictionary, and on content it would have failed too,
with a mean of 63.5 that includes 17 and 120, and a mean of 0.0 for a wave in
which nobody answered. `test_empty_list` and `test_rows_are_not_modified` pass.
A function that fails on the format alone would be easy to fix. The tests are for the content
failures.

**f) Maintenance.**

- The number of people who answered is already in the result (`"answered"`),
  because the requirements asked for it; only `report` has to show it.
- The median is a new function, `median_age(ages)`, next to `mean_age`, with
  its own specification (what is the median of an even number of ages?) and
  tests, including one with an even and one with an odd number of ages.
  `mean_age_by_wave` gets one more key, `"median"`, and its tests one more
  expected value each.
- The drop-down menu changes what `classify` receives: text that is always a
  valid number or `""`. The function does not have to change, and its tests
  stay, because the old data from waves 1 and 2 still has typed ages. A new
  test checks a value exactly as the drop-down writes it, for example
  `"99+"` if the menu has such an option, which the client has to be asked
  about.
- A third wave needs no change at all: waves are keys of a dictionary, not
  fixed in the code. A test with three waves confirms it.
- All the tests written for d) are run again after the change, as
  *regression tests*: they check that the new version still does what the
  old one did.

## Exercise 2: How often do parties talk about climate?

**a) Requirements.**

Functional:

1. For every party and year, the result gives the number of speeches that
   mention the topic and the number of all speeches, so that the share of
   speeches about the topic can be compared between parties of different
   size.
2. A speech mentions the topic if its text contains one of a list of terms,
   ignoring upper and lower case. The list is an input, not part of the code.
3. A speech counts once, however often it mentions the topic.
4. Parties and years with speeches but no mention appear with 0, so that
   "no mention" is distinguishable from "no data".
5. Dates that are not of the form `YYYY-MM-DD` are an error and stop the
   program, because a wrong year moves speeches between the years that the
   client wants to compare.

Non-functional: the result is reproducible: the same speeches and the same
list of terms give the same table, also on another computer, and the list of
terms is saved with every result (reproducibility). It runs on all speeches
of ten years in reasonable time, so it reads the text only once
(performance). The speech data is only read (safety). Speeches in Parliament
are public, so privacy is not the main concern here, although the speaker
names are not needed for the result and do not have to appear in it.

Questions to the client, with the assumed answer:

| Question | Assumed answer |
|---|---|
| Which words count as "about climate"? | Start with `"climate"`; the list is a parameter and can grow. |
| Count mentions or speeches? | Speeches: one long speech should not outweigh many short ones. |
| Absolute numbers or shares? | Both: the number of speeches mentioning the topic and the number of all speeches, so shares are one division away. Parties with more members give more speeches. |
| What about "business climate" or "a climate of fear"? | They count; we read a random sample of 50 matched speeches to estimate how often that happens, and report it. |
| What about speeches by the Speaker of the House, who chairs the debate? | Leave them out when reading the data: they are procedural, not party positions. |
| Which party does an MP who changed party count for? | The party at the date of the speech, as in the data. |

The first four rows are the measurement. Each choice changes the numbers,
and two people with the same data and different choices can reach different
conclusions about whether climate grew as a topic after 2019. That is why
they belong in the specification, where they can be read and criticized,
rather than in code that a coder chose alone.

**b) Specification.**

```python
def topic_by_party_year(speeches, terms):
    """Count the speeches that mention a topic, per party and year.

    speeches: a list of dicts with "party" (str), "date" (str, "YYYY-MM-DD")
              and "text" (str).
    terms: the words and phrases that mark the topic, in lower case,
           e.g. ("climate", "global warming", "net zero").
    A speech mentions the topic if its text, in lower case, contains one of
    the terms. A speech counts once, however often it mentions the topic.
    Returns: a dict from (party, year) to (mentioning, total), the number of
             speeches that mention the topic and the number of all speeches,
             for every party and year with at least one speech; year is an int.
    Raises: ValueError if a date is not of the form YYYY-MM-DD.
    The speeches are not modified, and their order does not change the result.
    An empty list gives {}.
    """
```

**c) Design.**

- `read_speeches(path)` reads the data into the list of dictionaries and
  leaves out the procedural speeches of the Speaker of the House.
- `mentions(text, terms)` returns `True` if the text, in lower case, contains
  one of the terms. This one line is the measurement of the concept, and
  keeping it in its own function means it can be tested, read and changed in
  one place.
- `topic_by_party_year(speeches, terms)` as specified.
- `report(counts, terms)` writes the table of shares per party and year,
  together with the list of terms and the date of the run.

A **dictionary** keyed by the pair (party, year) holds the counts: every
speech is one lookup and one update, and the pair is a valid key because a
tuple cannot change (session 3: keys must not change while they are in the
table). The terms are a tuple, since their order does not matter and they do
not change during a run; a **set** of terms works just as well. Reading the
text once per speech and checking all terms there keeps the running time
proportional to the amount of text.

**d) Tests.**

```python
CLIMATE = ("climate",)

def sp(party, date, text):
    return {"party": party, "date": date, "text": text}

def test_typical():
    s = [sp("Labour", "2019-05-01", "We must act on climate."),
         sp("Labour", "2019-06-01", "Schools need money.")]
    assert topic_by_party_year(s, CLIMATE) == {("Labour", 2019): (1, 2)}

def test_case_is_ignored():
    s = [sp("Labour", "2019-05-01", "The Climate Change Act")]
    assert topic_by_party_year(s, CLIMATE) == {("Labour", 2019): (1, 1)}

def test_a_speech_counts_once():
    s = [sp("Green", "2020-01-01", "climate, climate, climate")]
    assert topic_by_party_year(s, CLIMATE) == {("Green", 2020): (1, 1)}

def test_years_without_mentions_appear():
    s = [sp("SNP", "2016-03-03", "Fishing quotas.")]
    assert topic_by_party_year(s, CLIMATE) == {("SNP", 2016): (0, 1)}

def test_phrases():
    s = [sp("Labour", "2021-01-01", "Global warming is real."),
         sp("Labour", "2021-02-01", "A global market.")]
    assert topic_by_party_year(s, ("global warming",)) == {("Labour", 2021): (1, 2)}

def test_bad_date_raises():
    with pytest.raises(ValueError):
        topic_by_party_year([sp("Labour", "01.05.2019", "climate")], CLIMATE)

def test_empty():
    assert topic_by_party_year([], CLIMATE) == {}

def test_order_does_not_matter():
    s = [sp("Labour", "2019-05-01", "climate"),
         sp("Conservative", "2019-05-02", "tax")]
    assert topic_by_party_year(s, CLIMATE) == topic_by_party_year(s[::-1], CLIMATE)

def test_input_is_not_modified():
    s = [sp("Labour", "2019-05-01", "Climate")]
    topic_by_party_year(s, CLIMATE)
    assert s == [sp("Labour", "2019-05-01", "Climate")]
```

No test checks that "business climate" is left out, because the
specification says it counts. Tests check the specification, not the
concept; whether the specification measures the concept well is checked
differently, by reading a sample of matched speeches.

**e) Implementation.** The two functions answer two different questions:

- **Coder A** counts *mentions* of the lower-case string `"climate"`. It
  misses `"Climate"` at the start of a sentence or in "Climate Change Act",
  counts a speech with ten mentions ten times, and keeps the year as a
  string.
- **Coder B** counts *speeches* that contain `"climate"` in any case, which
  matches the specification better, keeps the year as an int, but leaves out
  every party and year without a mention, so "no mention" and "no data" look
  the same.
- Neither counts the speeches that do *not* mention the topic, so neither
  gives shares. The party with the most MPs gives the most speeches and will
  look like the party that talks most about climate, whatever its interest in
  the topic. That is the most serious error, and no error message points to
  it.
- Neither has a list of terms, so 'global warming' cannot be added without
  changing the code.

They give different results on any speech with "Climate" in capitals (A: 0,
B: 1) and on any speech with more than one mention (A: the number of
mentions, B: 1). On real speeches, both happen all the time.

Against the tests of d): A fails six, all but `test_empty`,
`test_order_does_not_matter` and `test_input_is_not_modified`; on
`test_years_without_mentions_appear` it is right on content, since it keeps
the key with 0, but it returns a count instead of a pair and the year as a
string. B fails five, and passes
`test_bad_date_raises` only by accident: `int("01.0")` raises a `ValueError`,
although no line of B checks the date. Several failures are about the format
of the result; the ones that matter are the capital letters (A), the
repeated mentions (A), the missing zeros (B) and the missing totals (both).

What this means: the same message, given to two coders, gave two programs
that measure two different things. It is not a bug of one of the two
programs; the message left the measurement open, and each coder closed it in
a different way, without telling anyone. The same happens when one coding
agent gets the same message twice, the **flakiness** of slide 7. A result
produced this way cannot be reproduced from the message, only from the code,
and nobody decided what was measured. The specification of b) closes the question once,
in writing, and the tests check every implementation against it, whoever or
whatever wrote it.

**f) Maintenance.**

- Adding "global warming" and "net zero" changes no code at all, because the
  terms are a parameter: `terms = ("climate", "global warming", "net zero",
  "net-zero")`. The hyphenated form is needed because "net zero" is often
  written "net-zero", and a substring match does not see the two as the same.
  The old results, with `("climate",)` alone, have to be kept and labeled with
  their list of terms, so that the paper can say which numbers come from which
  list.
- Running on the 2025 speeches changes nothing if the data has the same
  format. A test with one speech in the new format, read with
  `read_speeches`, checks that this is the case before the full run.
- The colleague's migration topic is the same program with a different list
  of terms. It is also a warning: a substring match for `"migration"` also
  finds "immigration" and "emigration", and whether that is wanted is a new
  requirement for that colleague to answer. If the answer is no, `mentions`
  needs whole-word matching, a change to the specification, then to the
  tests, then to the code, in that order.
- All existing tests are rerun after every change. A new test for
  `test_phrases` with the hyphenated "net-zero" records the reviewer's point,
  so the next version cannot lose it.

## Additional practice

### A1. Functional or non-functional?

a) functional. b) non-functional, performance. c) non-functional, privacy.
d) functional: it says what output (an error) the program gives for an input.
e) non-functional, safety. f) non-functional, reproducibility.
g) non-functional, security.

### A2. The questions a coder will not ask

Open questions: in which format are the birth dates written (`"1990-03-05"`,
`"05.03.1990"`, or date objects)? Age at which date: today, or the date of the
interview? Age in completed years, or with decimals? What happens with a
missing birth date? What with an impossible one, in the future or giving an
age over 120? Is the data modified, or is a new list returned?

Rewritten instructions: *"Write `ages(birth_dates, interview_dates)`. Both are
lists of strings `"DD.MM.YYYY"` of the same length. Return a list of ints: the
age in completed years on the interview date. Return None for a missing birth
date and raise ValueError for a birth date after the interview date. Do not
modify the input lists; use only the standard library."*

### A3. Which test is missing?

Not tested: that `None` answers are ignored, that the `ValueError` is raised,
and that the list is not modified. Missing tests:

```python
def test_missing_answers_are_ignored():
    assert share_yes(["yes", None, "no", None]) == 0.5

def test_no_valid_answer_raises():
    with pytest.raises(ValueError):
        share_yes([None, None])

def test_input_is_not_modified():
    answers = ["yes", None]
    share_yes(answers)
    assert answers == ["yes", None]
```

A wrong implementation that passes the two given tests:
`return answers.count("yes") / len(answers)`. It gives 0.75 and 1.0 on the
given cases, but 0.25 instead of 0.5 on the first missing test, because it
divides by all answers including `None`; it returns 0.0 instead of raising
`ValueError` for `[None, None]`, and raises `ZeroDivisionError` for `[]`. Two passing tests said nothing about
the lines of the specification they did not cover.

### A4. Counting survey answers

**a) Requirements.**

Functional:

1. The result gives, for each of the four options, the number of respondents
   who chose it, also when that number is 0.
2. Skipped questions (`None`) are counted separately, as "no answer", and not
   as one of the options.
3. Answers that differ only in upper and lower case or surrounding spaces
   count as the same option: `"Bus"` and `" bus"` are `"bus"`.
4. Any other answer, such as `"train"` or a typo, is counted as "invalid",
   so that all answers are accounted for and the counts add up to the number
   of respondents.

Non-functional: the list of answers is not modified (safety); the result
contains only counts, nothing that identifies a respondent (privacy); the
options always appear in the same order, so that two runs can be compared
(reproducibility).

Questions to the client, with the assumed answer:

| Question | Assumed answer |
|---|---|
| Are the answers always written exactly as the options? | No; compare ignoring case and spaces. |
| What should happen with answers that are not one of the options? | Count them as "invalid" and show the count. |
| Should skipped questions appear in the result? | Yes, as "no answer". |
| Counts or percentages? | Counts; percentages are one division away. |
| In which order should the options appear? | In the order of the questionnaire. |

**b) Specification.**

```python
def count_answers(answers, options=("car", "bus", "bike", "walk")):
    """Count how many respondents chose each option.

    answers: a list; each element is a string or None (question skipped).
             Strings are compared ignoring upper/lower case and surrounding spaces.
    options: the valid options, in the order in which they are reported.
    Returns: a dict with one key per option, in the order of options, followed by
             the keys "no answer" (for None) and "invalid" (for any other string).
             Every key is present, with 0 if no answer falls into it, and the
             counts add up to len(answers).
    The list is not modified. An empty list gives 0 for every key.
    """
```

**c) Design.**

- `normalize(answer)` returns the answer in lower case without surrounding
  spaces, or `None` for a skipped question.
- `count_answers(answers, options)` creates a dictionary with every key set
  to 0, then adds 1 to the right key for each normalized answer.
- `report(counts)` prints one line per key, in order.

A **dictionary** from option to count connects them: each answer is a lookup
by key, and the keys can be created in questionnaire order before counting, so
options nobody chose still appear. A set of the valid options makes the check
"is this answer an option?" a constant-time lookup, although with four options
a tuple is just as good.

**d) Tests.**

```python
def test_typical():
    assert count_answers(["bus", "bike", "bus"]) == \
        {"car": 0, "bus": 2, "bike": 1, "walk": 0, "no answer": 0, "invalid": 0}

def test_case_and_spaces_are_ignored():
    counts = count_answers(["Bus", " bike "])
    assert counts["bus"] == 1 and counts["bike"] == 1

def test_missing_and_invalid():
    counts = count_answers([None, "train", "car"])
    assert counts["no answer"] == 1 and counts["invalid"] == 1 and counts["car"] == 1

def test_empty():
    assert all(n == 0 for n in count_answers([]).values())

def test_counts_add_up():
    answers = ["bus", "Bus", None, "car", "x"]
    assert sum(count_answers(answers).values()) == len(answers)

def test_input_is_not_modified():
    answers = ["Bus", None]
    count_answers(answers)
    assert answers == ["Bus", None]
```

The example from the exercise, `["bus", "bike", None, "car", "bus",
"Bus"]`, gives bus 3, bike 1, car 1, walk 0, no answer 1, invalid 0. The
version a coder writes from the one-line request alone often gives bus
2 and a separate key `"Bus"` with 1, and fails on `None` or counts it as a
key: both are caught by the second and third tests.

### A5. The seminar waiting list

**a) Requirements.**

Functional:

1. The first 20 students who register get a place, in the order in which they
   register.
2. Everyone after that goes to the end of the waiting list.
3. When a student with a place drops out, the first student on the waiting
   list gets the place.
4. A student on the waiting list can also drop out; the others keep their
   order.
5. A student ID can be registered only once, either with a place or on the
   waiting list.
6. Dropping an ID that is not registered is reported as an error and changes
   nothing.

Non-functional: first come, first served is strictly kept, since the
waiting list is a question of fairness and students will complain about any
exception; the list of registrations is never lost when the program stops, for
example by saving it after every change (safety); a list shown to the students
contains only IDs, no names (privacy).

Questions to the client, with the assumed answer:

| Question | Assumed answer |
|---|---|
| What if someone registers twice? | The second registration is refused; their position does not change. |
| Can someone on the waiting list drop out? | Yes. |
| Is the number of places always 20? | For this seminar; the program takes it as a parameter. |
| Does someone who moves up have to confirm the place? | No, they get it directly. |
| Who sees the lists? | The instructor sees everything; students see only IDs. |

**b) Specification.** The state of the seminar is kept in one dictionary,
`seminar = {"capacity": 20, "places": [], "waiting": deque()}`, which both
functions receive.

```python
def register(seminar, student_id):
    """Register a student for the seminar.

    seminar: a dict with "capacity" (int), "places" (list of IDs, in order of
             registration) and "waiting" (deque of IDs, first in line first).
    student_id: a string.
    Result: if fewer than capacity students have a place, student_id is added to
            the end of "places" and "place" is returned; otherwise it is added to
            the end of "waiting" and "waiting" is returned.
    Raises: ValueError if student_id is already in "places" or "waiting";
            nothing is changed in that case.
    Never: more than capacity IDs in "places"; an ID twice in the seminar.
    """

def drop(seminar, student_id):
    """Remove a student from the seminar.

    seminar: as for register; student_id: a string.
    Result: if student_id has a place, it is removed, and the first ID of
            "waiting", if any, is moved to the end of "places". If student_id is
            on the waiting list, it is removed and the order of the others is kept.
    Raises: ValueError if student_id is not registered; nothing is changed.
    """
```

**c) Design.**

- `new_seminar(capacity)` returns the empty dictionary described above.
- `register(seminar, student_id)` and `drop(seminar, student_id)` as
  specified.
- `show(seminar)` prints the places and the waiting list, in order.

The places are a **list**, since their order (order of registration) is kept
and there are at most 20. The waiting list is a **queue** (`collections.deque`),
because it is first in, first out: new students join at the end, the student
who moves up leaves from the front. A stack would give the place to the most
recent student, the unfair outcome of serving the newest request first. With hundreds of registrations a **set** of all registered IDs
would make the duplicate check fast; with a seminar, searching the list and the
queue is fine. Making the capacity a parameter is a design decision for the
tests: they can use a seminar with 2 places instead of 20.

**d) Tests.**

```python
def test_first_come_get_places():
    s = new_seminar(2)
    assert [register(s, x) for x in ["a", "b", "c"]] == ["place", "place", "waiting"]
    assert s["places"] == ["a", "b"] and list(s["waiting"]) == ["c"]

def test_first_on_waiting_list_moves_up():
    s = new_seminar(2)
    for x in ["a", "b", "c", "d"]:
        register(s, x)
    drop(s, "a")
    assert s["places"] == ["b", "c"] and list(s["waiting"]) == ["d"]

def test_drop_from_waiting_list_keeps_order():
    s = new_seminar(1)
    for x in ["a", "b", "c", "d"]:
        register(s, x)
    drop(s, "c")
    assert s["places"] == ["a"] and list(s["waiting"]) == ["b", "d"]

def test_register_twice_is_refused():
    s = new_seminar(2)
    register(s, "a")
    with pytest.raises(ValueError):
        register(s, "a")
    assert s["places"] == ["a"] and list(s["waiting"]) == []

def test_drop_unknown_is_refused():
    s = new_seminar(2)
    with pytest.raises(ValueError):
        drop(s, "z")

def test_never_more_places_than_capacity():
    s = new_seminar(2)
    for x in "abcdefg":
        register(s, x)
    assert len(s["places"]) == 2
```

### A6. Grades from exercise points

**a) Requirements.**

Functional:

1. For every student in the table, the result gives the total points and
   whether they passed.
2. A sheet that was not handed in (`None`) counts as 0 points.
3. A student passes if the total is at least half of the points possible; a
   total of exactly 50% passes ("at least").
4. Points outside 0 to 10 are an error in the table and are reported, not
   silently used.

Non-functional: the table that the teaching assistants keep updating is only
read, never written (safety); the grades of all students are seen only by the
instructor (security and privacy); the same table gives the same result
(reproducibility).

The central question is what "50% of the points" means while the course is
still running. Possible answers: 50% of all 120 points of the course, which
nobody passes before the end of the semester; or 50% of the points of the
sheets graded so far, which lets the instructor see who is at risk during the
semester. The solution below takes the second, because the table "keeps being
updated", and states it in the specification so the client can disagree.

| Question | Assumed answer |
|---|---|
| 50% of all 12 sheets, or of the sheets graded so far? | Of the sheets graded so far: 10 points times the number of entries. |
| Does exactly 50% pass? | Yes. |
| Do missing sheets count as 0, or are they left out? | As 0. |
| Can there be half points? | Yes; points are numbers from 0 to 10. |
| What if two students have lists of different length? | That is an error in the table and is reported. |

**b) Specification.**

```python
def exercise_results(points):
    """Return the total points and the pass decision of every student.

    points: a dict from student ID (str) to a list of points, one entry per
            sheet graded so far, all lists of the same length; each entry is a
            number from 0 to 10, or None for a sheet not handed in.
    Returns: a dict from student ID to a pair (total, passed): total is the sum
             of the points, with None counting as 0; passed is True if total is
             at least half of 10 times the number of entries.
    Raises: ValueError if an entry is outside 0 to 10, or if the lists differ in
            length.
    The dict and its lists are not modified. An empty dict gives {}.
    """
```

**c) Design.**

- `read_table(path)` reads the file into the dictionary above; it opens the
  file only for reading.
- `total(sheet_points)` returns the sum of one student's points, with `None`
  as 0, and raises `ValueError` for an entry outside 0 to 10.
- `passed(total_points, n_sheets)` returns whether `total_points` is at least
  half of `10 * n_sheets`.
- `exercise_results(points)` checks the lengths and builds the result with
  the two functions above.

A **dictionary** from student ID to a **list** of points: the instructor looks
students up by ID, and each student's points keep the order of the sheets. The
result is again a dictionary keyed by student ID. Keeping `passed` as its own
function puts the pass rule in one place, which is the line the client is most
likely to change.

**d) Tests.**

```python
def test_typical_pass_and_fail():
    r = exercise_results({"A": [10, 8, 7], "B": [2, 3, 4]})
    assert r == {"A": (25, True), "B": (9, False)}

def test_exactly_half_passes():
    assert exercise_results({"A": [5, 5]}) == {"A": (10, True)}

def test_just_below_half_fails():
    assert exercise_results({"A": [5, 4.5]}) == {"A": (9.5, False)}

def test_missing_sheet_counts_as_zero():
    assert exercise_results({"A": [10, None]}) == {"A": (10, True)}

def test_points_out_of_range_raise():
    with pytest.raises(ValueError):
        exercise_results({"A": [11, 5]})

def test_lists_of_different_length_raise():
    with pytest.raises(ValueError):
        exercise_results({"A": [10, 10], "B": [10]})

def test_empty():
    assert exercise_results({}) == {}

def test_input_is_not_modified():
    points = {"A": [10, None]}
    exercise_results(points)
    assert points == {"A": [10, None]}
```

The boundary tests, exactly half and just below, are where an implementation
most often goes wrong: writing `>` instead of `>=` fails the second test, and
leaving `None` out of the count of sheets would let a student pass with half
the points of only the sheets they handed in.
