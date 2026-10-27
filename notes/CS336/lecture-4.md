---
layout: default
title: "Lecture 4 on Python"
---

# Python Session 4 — String Methods and Text Cleaning

**Date:** October 23, 2026  
**Focus:** Reviewing functions and slicing; cleaning and filtering text  
**Status:** Final checks continued on October 24

## 1. Review Functions and Slicing

Write a function that returns values within an inclusive interval:

```python
def select_between(numbers, low, high):
    between = []
    for number in numbers:
        if low <= number <= high:
            between.append(number)
    return between
```

Call the function and reverse the returned list:

```python
selected = select_between([40, 60, 75, 90, 100], 60, 90)
print(selected[::-1])
```

Expected output:

```text
[90, 75, 60]
```

Boundary reminder: `selected[-1:0:-1]` excludes index `0`, so it would omit the first item.

**C comparison:** Python supports `low <= number <= high`. In C, write `low <= number && number <= high`.

## 2. Clean String Boundaries and Letter Case

```python
text = "  Hello PYTHON  "
cleaned = text.strip().lower()
```

Expected value of `cleaned`:

```text
hello python
```

Key points:

- `strip()` removes whitespace from both ends.
- `lower()` converts letters to lowercase.
- Both return new strings; the original string stays unchanged.
- Parentheses are required to call a method: `text.strip()`, not `text.strip`.

## 3. Split Text into Words

```python
text = "  Hello   PYTHON  "
words = text.lower().split()
print(words)
```

Expected output:

```text
['hello', 'python']
```

With no argument, `split()` separates on whitespace, handles repeated whitespace, and ignores whitespace at the ends.

```python
print("   ".split())  # []
print("".split())     # []
```

A separate `strip()` call is unnecessary before this form of `split()`.

## 4. Return Cleaned Words from a Function

Initial version:

```python
def clean_words(text):
    cleaned = text.lower().split()
    return cleaned
```

Build another function that selects words by length:

```python
def select_long_words(text, min_length):
    cleaned = clean_words(text)
    long_words = []
    for word in cleaned:
        if len(word) >= min_length:
            long_words.append(word)
    return long_words
```

Test:

```python
print(select_long_words("  I LOVE learning Python  ", 4))
# ['love', 'learning', 'python']
```

Repeated words and their original order are preserved.

## 5. Handle Punctuation

`split()` does not remove punctuation:

```python
"Hi, a cat!".split()
# ['Hi,', 'a', 'cat!']
```

Consequently, `"cat!"` has length `4`.

To clean word boundaries, apply `strip(".,!?")` to each word after splitting:

```python
def clean_words(text):
    words = text.lower().split()
    cleaned = []

    for word in words:
        word = word.strip(".,!?")
        if word != "":
            cleaned.append(word)

    return cleaned
```

Test:

```python
print(clean_words("  Hi, CAT! ... don't  "))
# ['hi', 'cat', "don't"]

print(clean_words("!!! ..."))
# []
```

Stripping the whole sentence before splitting cannot remove punctuation between its words.

## 6. Distinguish Empty Values

| Value | Meaning | Length |
|---|---|---:|
| `""` | Empty string | 0 |
| `[]` | Empty list | 0 |
| `[""]` | List containing one empty string | 1 |

For punctuation-only words:

1. Stripping produces `""`.
2. The condition rejects that empty string.
3. No item is appended.
4. The function returns an empty list if no words remain.

## 7. Review Questions

**- Why must `strip()` be called with parentheses?**
  Or else it will only name the function and won't call it. I kept making this mistake for many functions, so really should keep in mind.
  
**- Why does `split()` not remove punctuation?**
  Because split recognize the punctuation as part of the word before.
  
**- Why should punctuation cleaning happen inside the word loop?**
  If it's outside the loop, punctuation in the sentence like commas won't be cleaned.

**- How do `""` and `[]` differ?**
  Well, one of them is an empty string, and the other a empty list.

**- Why does changing `clean_words` also change the behavior of `select_long_words`?**
  Because the latter calls the former when executed.

## Scope

The cleaner handles only `.,!?` at word boundaries. It is not a general-purpose punctuation remover or language-model tokenizer.

[← Back to home](/)
