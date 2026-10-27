---
layout: default
title: "Lecture 3"
---

# Python Session 3 — Slicing and Functions

**Date:** October 20, 2026  
**Session:** Make-up for Monday, October 19  
**Focus:** Negative indices, slicing, return values, and combining functions

## 1. Review Loops and Counting

Count marks greater than or equal to `60`:

```python
marks = [55, 60, 83, 42]
count = 0

for mark in marks:
    if mark >= 60:
        count += 1

print(f"count = {count}")
```

Expected output:

```text
count = 2
```

With an empty list, the loop runs zero times and the count stays `0`.

## 2. Access Items with Negative Indices

```python
marks = [55, 60, 83, 42]
```

| Item | Nonnegative index | Negative index |
|---|---:|---:|
| `55` | `0` | `-4` |
| `60` | `1` | `-3` |
| `83` | `2` | `-2` |
| `42` | `3` | `-1` |

Predict:

```python
print(marks[-1])
print(marks[-3])
print(marks[0] == marks[-4])
```

Expected output:

```text
42
60
True
```

Accessing `marks[-5]` raises `IndexError`.

**C comparison:** Negative indices do not mean “count from the end” in C. For an array of length `n`, the last item is accessed with index `n - 1`.

## 3. Select Sections with Slicing

Basic syntax:

```python
items[start:stop]
```

The start is included; the stop is excluded.

```python
marks = [55, 60, 83, 42]

print(marks[0:2])  # [55, 60]
print(marks[1:])   # [60, 83, 42]
print(marks[:-1])  # [55, 60, 83]
```

Omitting a boundary selects through the corresponding end.

Unlike individual indexing, a slice can extend beyond the list’s bounds:

```python
print(marks[:99])  # [55, 60, 83, 42]
```

Slices that select no items return an empty list:

```python
print(marks[2:2])  # []
print(marks[3:1])  # []
print(marks[4:])   # []
```

## 4. Slice Strings and Compare Mutability

Strings use the same indexing and slicing syntax:

```python
word = "coding"
new_word = "C" + word[1:]

print(word)
print(new_word)
print(word[-3:])
```

Expected output:

```text
coding
Coding
ing
```

Strings are immutable: individual characters cannot be replaced by assignment.

```python
letters = ["c", "a", "t"]
letters[0] = "b"  # Valid

word = "cat"
# word[0] = "b" would raise TypeError
```

**C comparison:** A writable character array such as `char word[] = "cat";` permits changing an individual character. A Python string does not.

## 5. Combine Slicing and Loops

Remove the first and last items, then count passing marks:

```python
marks = [10, 72, 45, 91, 60, 20]
middle = marks[1:-1]
count = 0

for mark in middle:
    if mark >= 60:
        count += 1

print(middle)
print(count)
```

Expected output:

```text
[72, 45, 91, 60]
3
```

For `marks = []`, the outputs are `[]` and `0`.

Slicing creates a new list:

```python
marks = [55, 60, 83, 42]
middle = marks[1:3]
middle[0] = 99

print(middle)  # [99, 83]
print(marks)   # [55, 60, 83, 42]
```

## 6. Add a Slice Step

Full syntax:

```python
items[start:stop:step]
```

The step controls the direction and distance between selected indices.

```python
numbers = [10, 20, 30, 40, 50, 60]

print(numbers[1:6:2])   # [20, 40, 60]
print(numbers[5:0:-2])  # [60, 40, 20]
```

Trace the indices first:

- `1:6:2` visits `1, 3, 5`.
- `5:0:-2` visits `5, 3, 1`.

Useful forms:

```python
values = [3, 6, 9, 12, 15, 18, 21]

print(values[::2])     # [3, 9, 15, 21]
print(values[::-1])    # [21, 18, 15, 12, 9, 6, 3]
print(values[5:1:-1])  # [18, 15, 12, 9]
```

If the direction does not lead toward the stop boundary, the slice is empty:

```python
print(values[1:5:-1])  # []
```

## 7. Return a Slice from a Function

```python
def without_ends(items):
    return items[1:-1]
```

Tests:

```python
print(without_ends([10, 20, 30, 40]))  # [20, 30]
print(without_ends([10]))              # []
print(without_ends([]))                # []
```

A parameter receives the argument supplied when the function is called.

## 8. Distinguish `return` and `print`

```python
def version_a(items):
    return items[1:-1]

def version_b(items):
    print(items[1:-1])

a = version_a([10, 20, 30, 40])
b = version_b([10, 20, 30, 40])

print("a:", a)
print("b:", b)
```

Expected output:

```text
[20, 30]
a: [20, 30]
b: None
```

- `print()` displays a value.
- `return` gives a value back to the caller.
- A function that finishes without an explicit return value returns `None`.

## 9. Count and Select with Functions

Count qualifying numbers:

```python
def count_at_least(numbers, threshold):
    count = 0
    for number in numbers:
        if number >= threshold:
            count += 1
    return count
```

Collect qualifying numbers:

```python
def select_at_least(numbers, threshold):
    selected = []
    for number in numbers:
        if number >= threshold:
            selected.append(number)
    return selected
```

Keep `return` after the loop so every item is checked. Empty input naturally returns `0` or `[]`.

## 10. Use Returned Values Outside a Function

Defining a function does not execute its body. Call it and store its return value:

```python
passing = select_at_least([55, 60, 83, 42], 60)

total = 0
for grade in passing:
    total += grade

print(total)
```

Expected output:

```text
143
```

The function’s local variables are not directly accessible outside it. The outside variable receives the returned value; its name does not need to match the local variable’s name.

## 11. Combine Two Functions

```python
def total_at_least(numbers, threshold):
    passing = select_at_least(numbers, threshold)
    total = 0

    for grade in passing:
        total += grade

    return total
```

Tests:

```python
print(total_at_least([55, 60, 83, 42], 60))  # 143
print(total_at_least([10, 20], 60))          # 0
print(total_at_least([], 60))                # 0
```

This reuses the filtering function instead of repeating its logic.

## 12. Review Questions

**- How do negative indices differ from negative steps?**
A negative index determines that we start counting from the end and finally arrives at a value, while a negative step means
the operation of counting is done from the end to the start.
  
**- Is a slice’s stop index included?**
Nope it isn't. It stops at the last and doesn't count the last.

**- Why can a slice return `[]` without raising an error?**
Because it simply returns an empty list.

**- How do strings and lists differ when changing individual items?**
Strings cannot be changed, while lists can.

**- What happens when a function prints a value but does not return it?**
You can't assign the function call to anything, but the stuff is still print out.

**- Why must a function be called before its result can be used?**
Or else there will be no return values.

**- How can one function use another function’s return value?**
By calling the function inside this function.

## Learning Material

Selected references:

- [University of Helsinki — More strings and lists](https://programming-26.mooc.fi/part-4/6-strings-and-lists/)
- [University of Helsinki — More functions](https://programming-26.mooc.fi/part-4/2-more-functions/)

Use selected sections and exercises to support the CS336 preparation roadmap rather than completing every MOOC topic.

## Next Step

Check retention with an independent function exercise, then begin string methods and text processing.

[← Back to home](/)
