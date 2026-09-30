# GenAI - Introduction - Dictionary — Notebook Q&A Summary

## Cell 1 (markdown) — Intro to Python dictionaries
**Summary points**
- Introduces dictionaries as Python's key/value mapping type, contrasted with sequence types (lists, tuples).
- Explains keys must be immutable (strings, numbers, tuples of immutables); lists cannot be keys.
- States values are unordered key:value pairs with unique keys per dictionary.
- Notes `{}` creates an empty dict; comma-separated `key:value` pairs populate it.
- Cites the official Python tutorial as the source.

**Key Concepts**
- Mapping types / associative arrays
- Key immutability requirement
- Dictionary literal syntax `{}`

**Q&A**
- Q: Why can't a list be used as a dictionary key? A: Lists are mutable, and keys must be hashable/immutable so their hash value never changes.
- Q: Can a tuple always be used as a key? A: Only if it contains solely immutable elements (strings, numbers, other immutable tuples); a tuple holding a list cannot be a key.

## Cell 2 (code) — Basic dictionary creation and access
**Summary points**
- Creates `person` with string keys (`name`, `age`, `city`) and mixed value types.
- Prints the whole dict, then accesses individual values via `person["key"]`.
- Demonstrates the fundamental `dict[key]` indexing syntax.

**Key Concepts**
- Dictionary literal creation
- Key-based value access

**Q&A**
- Q: What does `person["age"]` return? A: `25`, the value mapped to the `age` key.
- Q: What happens if you access a key that doesn't exist with `[]`? A: It raises a `KeyError` (not shown in this cell, but a general dict behavior).

## Cell 3 (code) — Nested dictionary
**Summary points**
- `student` has a `grades` key whose value is itself a dictionary.
- Shows chained access: `student['grades']['math']` to reach a deeply nested value.
- Illustrates that dict values can be any type, including other dicts.

**Key Concepts**
- Nested/hierarchical dictionaries
- Chained key access

**Q&A**
- Q: How do you get the science grade? A: `student['grades']['science']`.
- Q: Why use nested dicts instead of flat keys like `math_grade`? A: They group related data logically and mirror hierarchical real-world structures.

## Cell 4 (code) — Dictionary with list values
**Summary points**
- `inventory` maps category names to lists of items.
- Shows that dict values can be lists, enabling one-to-many relationships.
- Prints the whole dict and each list value.

**Key Concepts**
- Dict values as collections (lists)

**Q&A**
- Q: How would you add "grape" to the fruits list? A: `inventory['fruits'].append('grape')`.

## Cell 5 (code) — Dictionary comprehension
**Summary points**
- Builds `squares = {x: x**2 for x in range(5)}` mapping numbers to their squares.
- Demonstrates concise dict construction via comprehension syntax.

**Key Concepts**
- Dictionary comprehension

**Q&A**
- Q: What is the output? A: `{0: 0, 1: 1, 2: 4, 3: 9, 4: 16}`.
- Q: How would you filter to only even x? A: Add `if x % 2 == 0` at the end (shown later in cell 44).

## Cell 6 (code) — Dictionary with mixed value types
**Summary points**
- `info` combines a string, boolean, and float as values under different keys.
- Confirms dicts don't require uniform value types.

**Key Concepts**
- Heterogeneous value types in a single dict

**Q&A**
- Q: What type is `info['is_active']`? A: `bool` (`True`).

## Cell 7 (code) — Adding a key-value pair
**Summary points**
- Adds `person["job"] = "Engineer"` to the existing `person` dict.
- Shows that assigning to a new key inserts it (dicts are mutable/dynamic).

**Key Concepts**
- Dict mutability, key insertion via assignment

**Q&A**
- Q: What happens if "job" already existed? A: Its value would be overwritten instead of a new key being added.

## Cell 8 (code) — Updating values
**Summary points**
- Updates `person["age"]` from 25 to 26 using assignment.
- Same syntax as insertion — assignment either creates or overwrites a key.

**Key Concepts**
- In-place update via key assignment

**Q&A**
- Q: Does this create a new dict object? A: No, it mutates `person` in place.

## Cell 9 (code) — Removing a key with `del`
**Summary points**
- `del person["city"]` removes the key-value pair entirely.
- Prints `person` afterward to confirm removal.

**Key Concepts**
- `del` statement for dict key removal

**Q&A**
- Q: What error occurs if you `del` a nonexistent key? A: `KeyError`.

## Cell 10 (code) — `get()` method
**Summary points**
- Compares `person.get("job")` with `person["job"]`.
- `.get()` is safer since it returns `None` (or a default) instead of raising `KeyError` for missing keys.

**Key Concepts**
- `dict.get()` safe access pattern

**Q&A**
- Q: Why prefer `.get()` over `[]` for uncertain keys? A: It avoids a `KeyError` crash, returning `None` or a default instead.

## Cell 11 (code) — Checking key existence with `in`
**Summary points**
- Uses `if "name" in person` to check membership before accessing.
- Second example checks a nonexistent key ("research") to show the `else` branch.
- Demonstrates the idiomatic existence check pattern in Python.

**Key Concepts**
- Membership testing (`in`) on dict keys

**Q&A**
- Q: Does `in` check keys or values by default? A: Keys only, for plain dict membership testing.

## Cell 12 (code) — Iterating over keys
**Summary points**
- `for key in person` iterates keys by default (implicit `.keys()`).
- Simplest iteration form for a dictionary.

**Key Concepts**
- Default dict iteration (keys)

**Q&A**
- Q: Is `for key in person` equivalent to `for key in person.keys()`? A: Yes, functionally identical.

## Cell 13 (code) — Iterating over values
**Summary points**
- Uses `person.values()` to loop over only the values, ignoring keys.

**Key Concepts**
- `dict.values()` view

**Q&A**
- Q: Can you get the key from within this loop? A: Not directly — you'd need `.items()` instead.

## Cell 14 (code) — Iterating over key-value pairs with `.items()` and f-strings
**Summary points**
- `for key, value in person.items()` unpacks both key and value together.
- Uses an f-string (`f"{key}: {value}"`) to format output.
- Inline comments explain that f-strings are expressions evaluated at runtime, unlike static string literals.

**Key Concepts**
- `dict.items()`, tuple unpacking, f-strings

**Q&A**
- Q: What does the `f` prefix do? A: Marks the string as an f-string, letting `{}` placeholders evaluate expressions at runtime.
- Q: Why is `.items()` useful here instead of `.keys()` alone? A: It gives access to both key and value in one iteration, avoiding a second lookup like `person[key]`.

## Cell 15 (code) — `pop()` to remove and return a value
**Summary points**
- `person.pop("job")` removes the key and returns its value in one step.
- Differs from `del` in that it also returns the removed value.

**Key Concepts**
- `dict.pop()`

**Q&A**
- Q: What if the key doesn't exist and no default is given to `pop()`? A: Raises `KeyError`.

## Cell 16 (code) — `clear()`
**Summary points**
- `person.clear()` empties the dictionary in place, leaving `{}`.

**Key Concepts**
- `dict.clear()`

**Q&A**
- Q: Does `clear()` delete the variable `person`? A: No, it empties the dict object but the variable still references it.

## Cell 17 (code) — Creating a dict with `dict()` and keyword args
**Summary points**
- `colors = dict(red=255, green=0, blue=0)` builds a dict using keyword arguments instead of literal braces.

**Key Concepts**
- `dict()` constructor with kwargs

**Q&A**
- Q: What's a limitation of this style? A: Keys must be valid Python identifiers (can't use spaces or start with digits).

## Cell 18 (code) — `fromkeys()`
**Summary points**
- `dict.fromkeys(keys, 1)` creates a dict where every key in the list shares the same default value.
- Repeats with a different default (22) to show reusability of the same key list.

**Key Concepts**
- `dict.fromkeys()` class method

**Q&A**
- Q: Is there a risk using `fromkeys()` with a mutable default (e.g., a list)? A: Yes — all keys would share the *same* mutable object, so modifying one affects all.

## Cell 19 (code) — Merging dictionaries
**Summary points**
- First line `{*dict1, *dict2}` creates a set of merged **keys** only (using `*` unpacking into a set), but this result is immediately discarded/overwritten.
- Second line `{**dict1, **dict2}` is the actual dict merge — the `**` unpacking pattern combines key-value pairs, with later dicts overriding earlier ones on key conflicts.
- Only the second assignment's result (`merged`) is used and printed.

**Key Concepts**
- Dict unpacking (`**`) vs set unpacking (`*`)
- Key-collision override behavior

**Q&A**
- Q: What does the first line (`{*dict1, *dict2}`) actually produce and why doesn't it appear in the final output? A: A set of the two dicts' keys; it's overwritten by the next line assigning to the same variable.
- Q: If `dict1` and `dict2` shared a key with different values, which wins in `{**dict1, **dict2}`? A: `dict2`'s value, since later unpacking overrides earlier keys.

## Cell 20 (code) — Dictionary length
**Summary points**
- `len(merged)` returns the number of key-value pairs in `merged`.

**Key Concepts**
- `len()` on dictionaries

**Q&A**
- Q: What does `len()` count — keys, values, or pairs? A: The number of key-value pairs (equivalently, number of keys).

## Cell 21 (code) — Integer keys
**Summary points**
- `numbers = {1: "one", 2: "two", 3: "three"}` shows keys don't have to be strings.

**Key Concepts**
- Non-string (integer) keys

**Q&A**
- Q: Could you mix int and str keys in one dict? A: Yes, dicts allow heterogeneous key types as long as each is immutable/hashable.

## Cell 22 (code) — Tuple keys
**Summary points**
- `coordinates` uses `(0,0)` and `(1,2)` tuples as keys, representing points.
- Confirms tuples of immutable elements are valid, hashable keys.

**Key Concepts**
- Tuple keys, hashability

**Q&A**
- Q: Why does this work with tuples but not lists? A: Tuples are immutable and hashable; lists are mutable and unhashable.

## Cell 23 (code) — Mixed data type values
**Summary points**
- `profile` dict has string, int, list, and bool values together.
- Reinforces that dict values can be any type without restriction.

**Key Concepts**
- Heterogeneous values

## Cell 24 (code) — `dict()` constructor with kwargs (again)
**Summary points**
- `user = dict(username="admin", password="1234")` — another keyword-argument construction example.

**Key Concepts**
- `dict()` constructor

## Cell 25 (code) — `fromkeys()` with a shared default value
**Summary points**
- `defaults = dict.fromkeys(["a","b","c"], "default")` gives all three keys the same string default.
- Accesses each key individually afterward to confirm the value.

**Key Concepts**
- `dict.fromkeys()` with immutable default

## Cell 26 (code) — Counting word frequency
**Summary points**
- Splits a sentence into words and builds a frequency counter using `word_count.get(word, 0) + 1`.
- Prints intermediate state after each word to visualize how the counter builds up.
- Classic pattern for counting occurrences without external libraries (e.g., `collections.Counter`).

**Key Concepts**
- Frequency counting pattern, `.get()` with default for accumulation

**Q&A**
- Q: Why use `.get(word, 0)` instead of `word_count[word]`? A: It avoids a `KeyError` on the first occurrence of a new word by defaulting to 0.
- Q: What's an alternative, more idiomatic way to count words? A: `collections.Counter(sentence.split())`.

## Cell 27 (code) — Dictionary with functions (lambdas) as values
**Summary points**
- `operations` maps operator names to lambda functions (`add`, `multiply`, `divide`, `power`).
- Calls each lambda via `operations["add"](3, 4)` style, showing dicts can act as dispatch tables.
- Note: `divide` and `power` are defined as `lambda x, y: y / x` and `y**x` (arguments reversed from name intuition).

**Key Concepts**
- Dicts as dispatch/lookup tables, lambda functions, functions as first-class values

**Q&A**
- Q: What does `operations["divide"](3, 4)` compute? A: `4 / 3` (since the lambda is `y / x`), not `3 / 4`.
- Q: Why might a dict-of-functions be useful in real code? A: It replaces long if/elif chains with a fast key-based lookup for dynamic dispatch.

## Cell 28 (code) — `.keys()` and `.values()`
**Summary points**
- `car` dict introduced; prints `car.keys()` and `car.values()` view objects.

**Key Concepts**
- `dict.keys()`, `dict.values()` view objects

**Q&A**
- Q: Are `.keys()`/`.values()` lists? A: No, they're dynamic view objects that reflect live changes to the dict.

## Cell 29 (code) — `update()`
**Summary points**
- `car.update({"color": "blue", "year": 2021})` adds a new key and overwrites an existing one (`year`) in a single call.

**Key Concepts**
- `dict.update()` for bulk add/overwrite

## Cell 30 (code) — `setdefault()` on empty dict
**Summary points**
- `settings.setdefault("theme", "dark")` inserts `"theme": "dark"` since the key didn't exist.

**Key Concepts**
- `dict.setdefault()` insert-if-missing behavior

## Cell 31 (code) — `setdefault()` when key exists
**Summary points**
- Calling `person.setdefault("name", "Unknown")` on an existing key returns the current value ("Alice") without changing it.
- Demonstrates that `setdefault()` never overwrites existing values.

**Key Concepts**
- `setdefault()` no-overwrite guarantee

**Q&A**
- Q: What is `name` after this call? A: `"Alice"` (unchanged, since `name` already existed).

## Cell 32 (code) — `setdefault()` when key doesn't exist
**Summary points**
- `person.setdefault("city", "New York")` since `city` is missing, it's inserted with the default and returned.
- Contrasts directly with cell 31's existing-key case.

**Key Concepts**
- `setdefault()` insert behavior

## Cell 33 (code) — Sorting dict by keys
**Summary points**
- `scores` dict has mixed-case names; `sorted(scores.items())` sorts by key (tuple comparison starts with key).
- Wraps result back into a `dict()` to produce a new sorted dictionary.
- Note: Python 3.7+ dicts preserve insertion order, so this produces a genuinely key-ordered dict.

**Key Concepts**
- Sorting `dict.items()`, dict insertion order preservation

**Q&A**
- Q: Why does "Apple" sort after "Alice" but capital letters generally sort before lowercase? A: Sorting uses ASCII/Unicode code points, where uppercase letters (A-Z) come before lowercase (a-z), so "Apple" (all mixed-case) sorts per character codes, not alphabetically case-insensitive.

## Cell 34 (code) — Sorting dict by values
**Summary points**
- `sorted(scores.items(), key=lambda item: item[1])` sorts by the value (second tuple element) instead of the key.
- Comment suggests experimenting with `item[0]` to compare against key-based sorting.
- Explains lambdas as small anonymous functions used as sort keys.

**Key Concepts**
- Custom sort key functions, lambda as sort key extractor

**Q&A**
- Q: What would change if `key=lambda item: item[0]` were used instead? A: It would sort by name (key) instead of score (value), equivalent to cell 33's behavior.

## Cell 35 (code) — List of dictionaries
**Summary points**
- `classroom` is a list where each element is a dict representing a student.
- Shows a common data pattern: list of records, each a dict of fields.
- Accesses individual student dicts via list indexing (`classroom[0]`).

**Key Concepts**
- List of dicts pattern (record-style data)

## Cell 36 (code) — Extracting names via explicit loop
**Summary points**
- Iterates `classroom`, appending each `student["name"]` to a `names` list manually.
- The "traditional" imperative approach, contrasted with comprehensions in the next cells.

**Key Concepts**
- Manual accumulation loop pattern

## Cell 37 (code) — Extracting names via list comprehension
**Summary points**
- `[student["name"] for student in classroom]` achieves the same result as cell 36 more concisely.
- Also extracts `grade` values the same way, showing the pattern generalizes to any field.

**Key Concepts**
- List comprehension as loop replacement

**Q&A**
- Q: Why is the comprehension version generally preferred? A: It's more concise, often faster, and clearly expresses "transform each item" intent.

## Cell 38 (code) — Extracting names/grades via `map()`
**Summary points**
- `list(map(lambda student: student["name"], classroom))` is a third way to extract the same data.
- Shows three equivalent approaches (loop, comprehension, map) side-by-side across cells 36-38.

**Key Concepts**
- `map()` with lambda as a functional alternative to comprehensions

**Q&A**
- Q: Which of loop/comprehension/map is most Pythonic for this task? A: List comprehension is generally considered most readable and idiomatic in Python for simple transformations.

## Cell 39 (code) — Nested dictionary access (school)
**Summary points**
- `school` maps class names to dicts with `students` count and `teacher` name.
- Demonstrates multiple chained accesses (`school["classA"]["teacher"]`).

**Key Concepts**
- Nested dict navigation

## Cell 40 (code) — Checking for a key (car)
**Summary points**
- `if "model" in car:` re-uses the `car` dict defined earlier to check key existence.
- Comment reminds the reader that `car` was defined in an earlier cell — highlighting notebook cross-cell state dependency.

**Key Concepts**
- Membership check, cross-cell state in notebooks

**Q&A**
- Q: Why must this cell run after the `car` definition cell? A: Jupyter notebooks share a single kernel state; `car` must already exist in memory from a prior cell execution.

## Cell 41 (code) — Dictionary with `None` value
**Summary points**
- `config = {"debug": True, "logging": None}` shows `None` is a valid dict value representing "no value"/unset.

**Key Concepts**
- `None` as a sentinel value

## Cell 42 (code) — `popitem()`
**Summary points**
- `car.popitem()` removes and returns the *last inserted* key-value pair (LIFO order in Python 3.7+).

**Key Concepts**
- `dict.popitem()` LIFO removal

**Q&A**
- Q: Which pair is removed by `popitem()`? A: The most recently inserted one, since Python 3.7+ dicts maintain insertion order.

## Cell 43 (code) — Merging with `update()` vs `.copy()`
**Summary points**
- `dict_a.update(dict_b)` mutates `dict_a` in place, overwriting the shared key `"x"`.
- The comment "this is the problem with dictionary, it updates the original dict" refers to `dict_a` (the receiver) being mutated — `dict_b` itself is *not* changed.
- `dict_c = dict_b.copy()` creates a shallow copy so that updating `dict_c` afterward doesn't affect `dict_b`.
- Contrasts mutation of the target dict vs. safe copying before modification.

**Key Concepts**
- In-place mutation via `update()`, shallow copy (`.copy()`) to avoid side effects

**Q&A**
- Q: Does `dict_a.update(dict_b)` change `dict_b`? A: No — only `dict_a` (the dict being updated) is modified; `dict_b` stays intact.
- Q: Why use `.copy()` before further updates on `dict_c`? A: To avoid mutating the original `dict_b` when adding new keys, since a copy is an independent object.

## Cell 44 (code) — Dict comprehension with a condition
**Summary points**
- `{x: x**2 for x in range(10) if x % 2 == 0}` filters to only even numbers before squaring.
- Extends cell 5's comprehension with a conditional filter clause.

**Key Concepts**
- Conditional dict comprehension

## Cell 45 (code) — Deeply nested dictionary (company)
**Summary points**
- `company` has 3+ levels of nesting: departments → engineering → employees/manager.
- Shows progressively deeper chained access, ending at a list element (`employees[0]`).

**Key Concepts**
- Multi-level nested dict traversal

## Cell 46 (code) — Practice dict definition (company, extended)
**Summary points**
- Defines an expanded `company` dict with `engineering`, `marketing`, and `hr` departments, each with nested `projects`/`campaigns`/`policies`.
- No print statements — sets up data for the following practice-access cells (47-54).
- Serves as a "comprehension check" exercise dataset per the inline comment.

**Key Concepts**
- Complex nested data modeling for practice exercises

## Cells 47-54 (code) — Guided access exercises on `company`
**Summary points**
- Each cell prints one specific nested value: all departments (47), engineering dict (48), manager (49), employees list (50), first employee (51), AI project details (52), Digital campaign budget (53), second HR policy (54).
- Together they form a step-by-step tutorial on drilling into deeply nested structures.
- Reinforces syntax for chaining `[]` across dicts and lists.

**Key Concepts**
- Progressive nested-access practice

**Q&A**
- Q: What does cell 53's `company["departments"]["marketing"]["campaigns"]["Digital"]["budget"]` return? A: `50000`.
- Q: What does cell 54's `["policies"][1]` return? A: `"Flexible Hours"` (the second item in the HR policies list).

## Cell 55 (code) — Another practice dict (university)
**Summary points**
- Defines a `university` dict with `faculties` → `science`/`arts` → `departments` → subjects with `professors`, `courses`, `students`.
- Sets up data for the following access-practice cells (56-61); no output itself.

**Key Concepts**
- Multi-branch nested dict modeling

## Cells 56-61 (code) — Guided access exercises on `university`
**Summary points**
- Prints progressively specific values: all faculties (56), science faculty (57), physics professors (58), World History instructor (59), second chemistry student (60), Logic course credits (61).
- Mirrors the company exercise pattern but with a different domain (academic structure).

**Key Concepts**
- Nested dict/list combined traversal

**Q&A**
- Q: What does cell 60's `["students"][1]` return? A: `"Meera"` (second student in chemistry).

## Cell 62 (code) — "JSON SCRIPT" cell
**Summary points**
- Contains a raw Python dict literal (double-quoted keys, matching JSON syntax) with no variable assignment.
- Despite the "JSON SCRIPT" comment, this is just a Python dict expression, **not** actual JSON parsing — no `json` module is involved.
- As the sole expression in the cell, Jupyter auto-displays its `repr()` as output.
- Purpose is illustrative: showing how JSON-like structures map directly to Python dict syntax.

**Key Concepts**
- Python dict literal vs. true JSON (structural similarity, but different types)

**Q&A**
- Q: Is this cell actually parsing JSON? A: No — it's a plain Python dict literal; nothing here calls `json.loads` or reads a file.

## Cell 63 (code) — Loading `university.json` — **likely errors**
**Summary points**
- Attempts `open('university.json', 'r')` and `json.load(f)`.
- **This cell will error**: no `university.json` file exists in the ML/01.02.2026 folder (confirmed absent from the directory), so `open()` raises `FileNotFoundError`.
- Additionally, `import json` is never present anywhere earlier in the notebook, so even if the file existed, `json.load` would raise a `NameError`.
- Flagged explicitly per review rules rather than assuming it ran successfully.

**Key Concepts**
- Reading external JSON files (`json.load`), file I/O with `open()`

**Q&A**
- Q: Why would this cell fail if run as-is? A: The referenced file `university.json` doesn't exist in the notebook's folder, and the `json` module was never imported.
- Q: What two fixes would be needed to make it work? A: Create/export a valid `university.json` file in the same folder, and add `import json` before this cell.

## Cell 64 (code) — "Try this out" practice redefinition
**Summary points**
- Re-defines the same `university` dict structure as cell 55 (duplicate), intended as a fresh starting point for reader practice.
- No print statements — the reader is expected to write their own access expressions.

**Key Concepts**
- Practice/exercise scaffolding

## Cells 65-66 (code) — Empty cells
**Summary points**
- Both cells are blank, likely left as scratch space for the reader's own practice code following cell 64's exercise.

## Cell 67 (code) — `students_info` dataset
**Summary points**
- Defines a dict of 5 students, each mapping to a nested dict of `age`, `major`, `gpa`.
- Serves as the dataset for the analytical mini-exercises in cells 68-77.

**Key Concepts**
- Dict-of-dicts as a lightweight "records" dataset

## Cell 68 (code) — Count of students
**Summary points**
- `Q1 = len(students_info)` counts the number of top-level keys (students).

**Key Concepts**
- `len()` on a dict of records

## Cell 69 (code) — Unique majors
**Summary points**
- `set(student['major'] for student in students_info.values())` collects majors, then `list()` converts to a list.
- Uses a generator expression inside `set()` for deduplication.

**Key Concepts**
- Generator expressions, `set()` for deduplication

**Q&A**
- Q: Why wrap the generator in `set()` before `list()`? A: `set()` removes duplicate majors; converting straight to `list()` would keep duplicates.

## Cell 70 (code) — Student with highest GPA
**Summary points**
- `max(students_info, key=lambda x: students_info[x]['gpa'])` finds the key (student name) whose GPA is highest.
- Iterates over dict keys by default, using a lambda to look up the comparison value.

**Key Concepts**
- `max()` with custom `key=` function on dict keys

**Q&A**
- Q: Why does `max(students_info, ...)` iterate names instead of dicts? A: Iterating a dict directly yields its keys, so `x` is a student name, and the lambda looks up their GPA for comparison.

## Cell 71 (code) — Students older than 20
**Summary points**
- `[name for name, info in students_info.items() if info['age'] > 20]` filters names by a condition on nested data.

**Key Concepts**
- Filtering nested dict data with list comprehension + `.items()`

## Cell 72 (code) — Sort students by GPA descending
**Summary points**
- `sorted(students_info.items(), key=lambda x: x[1]['gpa'], reverse=True)` returns a list of `(name, info)` tuples ordered by GPA.
- `reverse=True` flips ascending to descending order.

**Key Concepts**
- Sorting `.items()` tuples by a nested field, `reverse` parameter

## Cell 73 (code) — Average age
**Summary points**
- `sum(info['age'] for info in students_info.values()) / len(students_info)` computes the mean age using a generator expression and division.

**Key Concepts**
- Aggregation via `sum()`/`len()` over generator expressions

## Cell 74 (code) — Name+major for high-GPA students
**Summary points**
- `[(name, info['major']) for name, info in students_info.items() if info['gpa'] > 3.7]` builds a filtered list of tuples.
- Combines filtering and projection (selecting specific fields) in one comprehension.

**Key Concepts**
- Combined filter + projection comprehension

## Cell 75 (code) — Check all GPAs above threshold
**Summary points**
- `all(info['gpa'] > 3.0 for info in students_info.values())` returns a single boolean for a global condition.

**Key Concepts**
- `all()` built-in with generator expression

## Cell 76 (code) — Youngest student
**Summary points**
- `min(students_info, key=lambda x: students_info[x]['age'])` mirrors cell 70's pattern but for minimum age.

**Key Concepts**
- `min()` with custom key function

## Cell 77 (code) — New dict of name→GPA
**Summary points**
- `{name: info['gpa'] for name, info in students_info.items()}` projects the original nested dict into a flatter one containing only GPAs.

**Key Concepts**
- Dict comprehension for data reshaping/projection

## Cell 78 (code) — Practice questions (comments only)
**Summary points**
- Contains 10 numbered questions as comments (no executable code) for the reader to solve using `students_info`.
- Covers lookups, updates, additions, removals, and aggregate calculations — a self-test recap of the whole notebook's dict operations.
- Intentionally left unimplemented as a practice/homework cell.

**Key Concepts**
- Self-directed practice exercise

**Q&A**
- Q: How would you answer question 4 ("Add Farhan...")? A: `students_info["Farhan"] = {"age": 24, "major": "Engineering", "gpa": 3.6}`.
- Q: How would you answer question 8 ("Remove Esha")? A: `students_info.pop("Esha")` or `del students_info["Esha"]`.

---

## Notebook-Level Review

**Overall Summary**
This notebook is a hands-on tutorial on Python dictionaries, progressing from basic creation/access/mutation (cells 2-32), through sorting and multi-structure combinations (dicts of lists, lists of dicts, functions-as-values) (cells 33-45), into deeply nested real-world-style data modeling exercises (`company`, `university`) with guided access practice (cells 46-64). It closes with a JSON-vs-dict-literal aside (with one cell that will error due to a missing file/import), and a final analytical mini-project (`students_info`) using comprehensions, `sorted()`, `max()`/`min()`, and `all()` to practice data-analysis-style queries on dict data, ending in open-ended self-test questions. Overall it builds from dictionary fundamentals to applying them for lightweight structured-data analysis, a natural precursor to using pandas/JSON in later notebooks.

**Concept Map**
- **Core dict operations**: creation (`{}`, `dict()`), access (`[]`, `.get()`), mutation (`=`, `update()`, `setdefault()`), deletion (`del`, `pop()`, `popitem()`, `clear()`)
- **Iteration**: `.keys()`, `.values()`, `.items()`, implicit key iteration
- **Construction shortcuts**: `dict.fromkeys()`, keyword-argument `dict()`, dict comprehensions (with/without conditions)
- **Key types & hashability**: strings, ints, tuples (valid) vs. lists (invalid)
- **Combining dicts**: `**` unpacking merge, `.update()` mutation vs. `.copy()` for safety
- **Composite structures**: dict-of-dicts (nesting), dict-of-lists, list-of-dicts, dict-of-functions (dispatch tables)
- **Functional patterns**: lambdas as sort keys / dispatch values, `map()`, generator expressions, `sorted()`/`max()`/`min()`/`all()` with `key=`
- **Data analysis mini-patterns**: counting frequencies, filtering, projection, aggregation (sum/average), sorting nested records
- **JSON connection**: dict literals resembling JSON, file I/O with `json.load` (the one broken/unimplemented example)

**Mixed Q&A Quiz**
1. Q: Both cell 19 (merging with `**`) and cell 43 (`.update()`) combine dictionaries — what's the key difference in mutation behavior between them? A: `{**dict1, **dict2}` creates a brand-new dict, leaving both originals untouched; `dict_a.update(dict_b)` mutates `dict_a` in place while leaving `dict_b` untouched.
2. Q: Cells 33/34 sort a dict by key vs. by value, and cell 72 sorts `students_info` by GPA — what do all three have in common syntactically? A: They all use `sorted(dict.items(), key=lambda item: ...)`, choosing which tuple element (`item[0]` for key, `item[1]` or `item[1]['field']` for value) drives the ordering.
3. Q: How do the `company`/`university` nested-access exercises (cells 45-61) relate to the `json.load` cell (63)? A: Nested dicts built directly in Python mirror the structure you'd get from parsing a real JSON file — the notebook demonstrates the pattern manually before (attempting to) show loading equivalent data from disk.
4. Q: Cells 36-38 show three ways (loop, comprehension, `map()`) to extract names from `classroom` — which pattern reappears in cells 69/71/74 on `students_info`? A: List comprehensions with `.items()`/`.values()`, extended with filtering conditions (`if info['gpa'] > 3.7`) and projections (`(name, info['major'])`).
5. Q: If you wanted to fix cell 63 to actually work, what would you need from earlier/other cells in this notebook (or elsewhere)? A: An `import json` statement, plus a real `university.json` file on disk containing data structurally equivalent to the `university` dict defined in cells 55/64 — since none of that file-writing ever happens in this notebook, the university dict would need to be serialized with `json.dump()` first.
