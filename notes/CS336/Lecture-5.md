---
layout: default
title: "Python lecture 5
---

# Python Session 5 — Text Reports and Word Frequencies

**Date:** October 24, 2026  
**Focus:** Combining functions, joining text, calculating summaries, and introducing dictionaries

## 1. Finish the Text-Cleaning Review

`strip()` removes specified characters only at the ends:

```python
print("'hello'".strip("'"))  # hello
print("don't".strip("'"))    # don't
```

An internal apostrophe remains even when it is included among the stripping characters.

## 2. Join Words into a String

```python
words = ["hello", "python"]
print(" ".join(words))
```

Expected output:

```text
hello python
```

The separator appears only between items:

```python
"-".join(["red", "green", "blue"])  # 'red-green-blue'
" ".join(["cat"])                  # 'cat'
" ".join([])                       # ''
```

An empty list produces an empty string, not a string containing a space.

## 3. Reuse Functions to Process Text

Normalize text using the existing cleaner:

```python
def normalize_text(text):
    cleaned = clean_words(text)
    return " ".join(cleaned)
```

Create a sentence containing only words that meet a length threshold:

```python
def long_word_sentence(text, min_length):
    long_words = select_long_words(text, min_length)
    return " ".join(long_words)
```

Tests:

```python
print(normalize_text("  Hi, CAT! ... don't STOP!  "))
# hi cat don't stop

print(long_word_sentence("looks, like , it!!!", 3))
# looks like

print(long_word_sentence("looks, like , it!!!", 2))
# looks like it
```

Cleaning happens before length filtering: `"it!!!"` becomes `"it"`, whose length is `2`.

## 4. Calculate Average Word Length

Algorithm:

1. Clean the text.
2. Return `0` if no words remain.
3. Add the lengths of the cleaned words.
4. Divide by the number of words.

```python
def average_word_length(text):
    cleaned = clean_words(text)
    if cleaned == []:
        return 0

    total_length = 0
    for word in cleaned:
        total_length += len(word)

    return total_length / len(cleaned)
```

Tests:

```python
print(average_word_length("Hi, Python!"))  # 4.0
print(average_word_length("cat cat a"))   # 2.3333333333333335
print(average_word_length("!!! ..."))     # 0
print(average_word_length("sum, max,,,, min... max!!!"))
# 3.0
```

Use a name such as `total_length` instead of `sum`, which is already a Python built-in function.

**C comparison:** Python’s `/` returns a floating-point result for integer operands; C integer division behaves differently.

## 5. Count Exact Word Matches

```python
def count_word(text, target):
    cleaned = clean_words(text)
    count = 0
    for word in cleaned:
        if word == target:
            count += 1
    return count
```

Assume `target` is already lowercase and punctuation-free.

Independent test:

```python
print(count_word("catfood, cattreat, catlady", "cat"))
# 0
```

Equality checks whole words, not substrings.

## 6. Compare Two Texts

Build `compare_texts(text_a, text_b)` to display:

- Normalized text.
- Cleaned word count.
- Average word length.
- Which text has more words, or whether the counts tie.

Important correction: compare word counts, not average word lengths.

```python
compare_texts("elephant", "a tiny cat")
```

The second text has more words even though the first has a larger average word length.

Also test equal counts and punctuation-only inputs.

## 7. Introduce Dictionaries

A dictionary associates keys with values:

```python
counts = {"cat": 2, "dog": 1}
```

```python
counts["cat"] += 1
counts["bird"] = 1
```

Key points:

- `counts["cat"]` looks up a value.
- `"cat" in counts` checks whether a key exists.
- `len(counts)` counts distinct keys.
- `{}` creates an empty dictionary.

## 8. Build Word Frequencies

```python
def word_counts(text):
    cleaned = clean_words(text)
    counts = {}

    for word in cleaned:
        if word in counts:
            counts[word] += 1
        else:
            counts[word] = 1

    return counts
```

Test:

```python
print(word_counts("Go, go! STOP. go"))
# {'go': 3, 'stop': 1}
```

There are four words in total and two distinct words.

A plain dictionary loop visits each key once:

```python
for word in counts:
    print(word, counts[word])
```

## 9. Select Frequent Words

```python
def frequent_words(text, minimum):
    counts = word_counts(text)
    selected = []

    for word in counts:
        if counts[word] >= minimum:
            selected.append(word)

    return selected
```

Each qualifying word appears once because the loop visits each dictionary key once.

```python
print(frequent_words("cat cat dog dog dog bird", 2))
# ['cat', 'dog']
```

## 10. Build a Word Report

```python
def word_report(text, minimum):
    cleaned = clean_words(text)
    print(f"Total words: {len(cleaned)}")

    counts = word_counts(text)
    print(f"Distinct words: {len(counts)}")

    for word in counts:
        print(word, counts[word])

    print(f"Frequent words: {frequent_words(text, minimum)}")
```

For:

```python
word_report("Go, go! STOP stop stop wait.", 2)
```

Expected output:

```text
Total words: 6
Distinct words: 3
go 2
stop 3
wait 1
Frequent words: ['go', 'stop']
```

For punctuation-only input, expect zero total words, zero distinct words, no frequency lines, and `[]` for frequent words.

## 11. Review Questions

**- Where does `join()` place its separator?**
Between each item in the list.

**- Why must average calculations handle empty input?**
Or else we might me deciding by zero, which will raise an error.

**- How do total word count and distinct word count differ?**
Total word count counts "cat", "cat" twice, while distinct word count only count once.

**- Why does a dictionary loop visit a repeated word only once?**
Because all the same words are stored under the same entry.

**- When should a function return a value, and when is printing appropriate?**
When we want to assign the value to a variable, we return it. If we just want to show it on the screen, printing is enough.


## Next Step

Practise missing dictionary keys, `get()`, and iterating over key–value pairs.

[← Back to home](/)
