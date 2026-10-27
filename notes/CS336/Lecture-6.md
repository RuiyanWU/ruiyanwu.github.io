---
layout: default
title: "Python lecture 6"
---

# Python Session 6 — Dictionary Lookups, Merging, and Copying

**Date:** October 27, 2026  
**Session:** Make-up for Monday, October 26  
**Focus:** `get()`, `.items()`, merging counts, and assignment versus copying

## 1. Review Frequency Counting

Write a function that accepts an already-cleaned list:

```python
def count_words(words):
    counts = {}
    for word in words:
        if word in counts:
            counts[word] += 1
        else:
            counts[word] = 1
    return counts
```

Tests:

```python
print(count_words(["go", "stop", "go", "wait", "stop", "go"]))
# {'go': 3, 'stop': 2, 'wait': 1}

print(count_words([]))
# {}
```

The dictionary’s length is the number of distinct words.

## 2. Handle Missing Keys with `get()`

Direct lookup of a missing key raises `KeyError`:

```python
counts = {"go": 3, "stop": 2}
# counts["wait"] would raise KeyError
```

Supply a fallback with `get()`:

```python
print(counts.get("go", 0))    # 3
print(counts.get("wait", 0))  # 0
print(counts)
# {'go': 3, 'stop': 2}
```

`get()` alone does not insert a missing key.

## 3. Simplify Counting

```python
def count_words(words):
    counts = {}
    for word in words:
        counts[word] = counts.get(word, 0) + 1
    return counts
```

The right-hand side reads the existing count or `0`, then adds `1`. The assignment stores the result and can create a new entry.

## 4. Loop Over Keys and Values Together

```python
counts = {"cat": 2, "dog": 5, "bird": 1}

for word, frequency in counts.items():
    if frequency >= 2:
        print(word, frequency)
```

Expected output:

```text
cat 2
dog 5
```

`.items()` supplies key–value pairs. The first variable receives the key; the second receives its value.

Remember the parentheses:

- `counts.items` refers to the method.
- `counts.items()` calls the method.

## 5. Merge Two Dictionaries of Counts

Requirements:

1. Add frequencies for matching words.
2. Include words present in only one input.
3. Return a new dictionary.
4. Leave both inputs unchanged.

Initial working version:

```python
def merge_counts(first, second):
    new = {}

    for word, frequency in first.items():
        new[word] = new.get(word, 0) + frequency

    for word, frequency in second.items():
        new[word] = new.get(word, 0) + frequency

    return new
```

Test:

```python
print(merge_counts(
    {"cat": 2, "dog": 1},
    {"dog": 3, "bird": 2}
))
```

Expected output:

```text
{'cat': 2, 'dog': 4, 'bird': 2}
```

Important debugging lesson: read the accumulated count from `new`. Reading from the input dictionary and adding its own frequency would double that input value rather than accumulate the merged total.

## 6. Assignment Does Not Copy a Dictionary

```python
a = {"dog": 1}
b = a
b["bird"] = 2

print(a)
print(b)
```

Both print:

```text
{'dog': 1, 'bird': 2}
```

`a` and `b` refer to the same dictionary.

## 7. Create a Separate Dictionary with `.copy()`

```python
a = {"cat": 2}
b = a.copy()
b["cat"] += 1

print(a)  # {'cat': 2}
print(b)  # {'cat': 3}
```

Distinction:

- `b = a`: another name for the same dictionary.
- `b = a.copy()`: a separate dictionary.

`.copy()` makes a shallow copy. This is sufficient for these dictionaries of integer counts; nested mutable values require further care.

## 8. Simplify the Merge Function

Copy the first dictionary, then loop only over the second:

```python
def merge_counts(first, second):
    new = first.copy()

    for word, frequency in second.items():
        new[word] = new.get(word, 0) + frequency

    return new
```

Boundary tests:

```python
print(merge_counts({}, {"cat": 2}))
# {'cat': 2}

print(merge_counts({}, {}))
# {}
```

## 9. Verify Input Independence

```python
first = {"cat": 2, "dog": 1}
combined = merge_counts(first, {})
combined["cat"] += 1

print(first)
```

Confirmed output:

```text
{'cat': 2, 'dog': 1}
```

Even when the loop runs zero times, `.copy()` still creates a separate dictionary.

## 10. Review Questions

**- Does `get()` insert a missing key?**
Nope, it simply returns the default value.

**- Why does `.items()` need parentheses?**
To actually call the function instead of just naming it.

**- When merging, why do we read the accumulated value from the output dictionary?**
If we read the value from the original dictionary and add the read value again, we'll be counting stuff twice.

**- How does assignment differ from copying?**
Assignment gives the same dictionary different name, but modification done to either name will affect the dictionary,
whereas copying creates a new dictionary and will not affect the original.

**- What happens when the second input is empty?**
The loop is never executed.

**- How can a test verify that the original dictionary remains unchanged?**
By printing the original dictionary out again.


## Next Step

Review assignment versus copying, then explore references and nested data.

[← Back to home](/)
