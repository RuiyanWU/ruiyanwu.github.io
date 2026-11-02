---
layout: default
title: "Python lecture 7"
---

# Python Session 7 — References, Nested Lists, and Copying

**Dates:** October 30 and November 2, 2026  
**Session:** Friday's lesson, continued on Monday  
**Focus:** Mutation versus reassignment, shallow copies, nested loops, and independent list updates

## 1. Review Assignment and Copying

Predict the output before running:

```python
original = {"cat": 2, "dog": 1}
shared = original
copied = original.copy()

shared["cat"] += 1
copied["dog"] = 9

print(original)
print(shared)
print(copied)
```

Expected output:

```text
{'cat': 3, 'dog': 1}
{'cat': 3, 'dog': 1}
{'cat': 2, 'dog': 9}
```

Assignment gives the same object another name. `.copy()` creates a separate outer container.

## 2. Distinguish Mutation from Reassignment

```python
a = [10, 20]
b = a
b.append(30)
b = [99]

print(a)  # [10, 20, 30]
print(b)  # [99]
```

`append()` changes the existing list. Assigning `[99]` makes `b` refer to a new list without undoing the earlier change.

The same rule applies to function parameters:

```python
def adjust(marks):
    marks[0] = 80
    marks = [100]
    marks.append(90)

scores = [60, 70]
adjust(scores)
print(scores)  # [80, 70]
```

Only `marks[0] = 80` changes the caller's list. After reassignment, `marks` refers to a different list.

## 3. Copy, Modify, and Return

```python
def with_extra_mark(marks, extra):
    updated = marks.copy()
    updated.append(extra)
    return updated
```

Test:

```python
scores = [60, 70]
updated = with_extra_mark(scores, 90)
print(scores)   # [60, 70]
print(updated)  # [60, 70, 90]
```

Important distinction: `append()` changes a list but returns `None`.

```python
copied = scores.copy()
result = copied.append(90)
print(result)  # None
```

Therefore, `return updated.append(extra)` would return `None`. Append first, then return the list itself.

Method chaining depends on return values: `text.strip().lower()` works because `strip()` returns a string. Also, `marks.copy` refers to a method; `marks.copy()` calls it.

## 4. Access Nested Lists

```python
groups = [[60, 70], [80, 90]]
print(groups[0])     # [60, 70]
print(groups[0][1])  # 70
```

Evaluate the indexing in order: select the first inner list, then its second item.

## 5. Understand Shallow Copies

```python
groups = [[60, 70], [80, 90]]
copied = groups.copy()
copied[0].append(75)

print(groups)
print(copied)
```

Both print:

```text
[[60, 70, 75], [80, 90]]
```

The outer lists are separate, but their inner lists are shared. This is a shallow copy.

Compare replacing an outer item with changing a shared inner list:

```python
groups = [[60, 70], [80, 90]]
copied = groups.copy()
copied[0] = [100]
copied[1].append(95)

print(groups)  # [[60, 70], [80, 90, 95]]
print(copied)  # [[100], [80, 90, 95]]
```

## 6. Copy Each Inner List

For groups containing numbers, create independent inner lists:

```python
def copy_groups(groups):
    copied = []
    for group in groups:
        inner_copy = group.copy()
        copied.append(inner_copy)
    return copied
```

Each iteration copies one whole inner list. There is no need to loop over its numbers to make this copy.

```python
original = [[60, 70], [80, 90]]
copied = copy_groups(original)
copied[0].append(75)

print(original)  # [[60, 70], [80, 90]]
print(copied)    # [[60, 70, 75], [80, 90]]
```

Using `inner_copy = group` instead would share the original inner list. This helper copies two list levels; it is not a general recursive deep copy.

## 7. Modify Each Copied Group

**Reuse:** `copy_groups`.

```python
def with_extra_in_each(groups, extra):
    copied = copy_groups(groups)
    for group in copied:
        group.append(extra)
    return copied
```

Boundary tests:

```python
print(with_extra_in_each([], 99))          # []
print(with_extra_in_each([[]], 99))        # [[99]]
print(with_extra_in_each([[], [10]], 99))  # [[99], [10, 99]]
```

An empty outer list gives zero iterations. An outer list containing one empty inner list gives one iteration.

## 8. Update Existing Marks by Index

This does not update the list:

```python
for num in group:
    num += bonus
```

It assigns a new integer to `num` without storing it back in the list. To replace an item, assign through its index:

```python
for i in range(len(group)):
    group[i] += bonus
```

## 9. Adjust Marks and Cap Them at 100

**Reuse:** `copy_groups`.

```python
def adjusted_groups(groups, bonus):
    copied = copy_groups(groups)
    for group in copied:
        for i in range(len(group)):
            if group[i] + bonus <= 100:
                group[i] += bonus
            else:
                group[i] = 100
    return copied
```

Test exact and exceeded boundaries, an empty group, and input preservation:

```python
original = [[95, 98], [], [100]]
updated = adjusted_groups(original, 5)
print(original)  # [[95, 98], [], [100]]
print(updated)   # [[100, 100], [], [100]]
```

The result remains independent after the function returns: appending to `updated[0]` does not affect `original[0]`.

## 10. Count Passes in Each Group

No copying helper is needed here: this function reads the input and builds a separate result.

```python
def count_passes_by_group(groups, pass_mark):
    counts = []
    for group in groups:
        count = 0
        for mark in group:
            if mark >= pass_mark:
                count += 1
        counts.append(count)
    return counts
```

Reset the count inside the outer loop. Append it after finishing the inner loop.

```python
print(count_passes_by_group([[60, 45, 90], [], [59, 60]], 60))
# [2, 0, 1]

print(count_passes_by_group([], 60))
# []
```

## 11. Combine the Functions

**Reuse:** `copy_groups`, `adjusted_groups`, and `count_passes_by_group`.

```python
groups = [[58, 60], [], [95, 40]]
adjusted = adjusted_groups(groups, 5)
pass_count = count_passes_by_group(adjusted, 60)

print(groups)
print(adjusted)
print(pass_count)
```

Confirmed output:

```text
[[58, 60], [], [95, 40]]
[[63, 65], [], [100, 45]]
[2, 0, 1]
```

## 12. Review Questions

- How does changing an object differ from reassigning a variable?
- Why does `append()` not work as the return expression when the goal is to return a list?
- Which lists remain shared after copying only the outer list?
- Why does copying each inner list protect the original in these examples?
- Why does `num += bonus` fail to update the list, while `group[i] += bonus` works?
- Where should a per-group counter reset?
- How do `[]` and `[[]]` affect a nested loop differently?

## Keep for Future Practice

Save `copy_groups`, `with_extra_in_each`, `adjusted_groups`, and `count_passes_by_group`. Future exercises should identify required helper functions before starting.

## Next Step

Briefly review references and copying, then begin reading text files to use with the earlier text-analysis functions.

[← Back to home](/)
