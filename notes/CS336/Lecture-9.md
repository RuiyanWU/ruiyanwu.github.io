---
layout: default
title: "Python lecture 9"
---

# Python Session 9 — Writing Files and Handling Missing Files

**Date:** November 10, 2026  
**Focus:** Writing and appending text, saving word counts, and introducing exception handling

## Learning Material

[University of Helsinki — Writing files](https://programming-26.mooc.fi/part-6/2-writing-files/)

Completed the writing-files reading and exercises, including the diary practice, then applied the ideas in an independent word-count export task.

## 1. Review File Position

```python
with open(path, encoding="utf-8") as file:
    first = file.read()
    second = file.read()
```

The first call reads to the end. The second returns `""` because the file position is already at the end; reading does not restart automatically.

## 2. Choose a File Mode

| Mode | Purpose | If the file does not exist |
|---|---|---|
| `"r"` | Read existing contents | Raises `FileNotFoundError` |
| `"w"` | Write, replacing existing contents | Creates the file |
| `"a"` | Append to existing contents | Creates the file |

Opening an existing file in `"w"` mode clears its contents immediately, even before calling `write()`.

```python
with open("practice_output.txt", "w", encoding="utf-8") as file:
    file.write("Hello\n")
    file.write("Python\n")
```

`write()` takes a string and does not add a newline automatically. Include `"\n"` when a new line is needed.

## 3. Create a Diary File Without Erasing Entries

To ensure `diary.txt` exists before attempting to read it:

```python
with open("diary.txt", "a", encoding="utf-8") as diary:
    pass
```

`pass` does nothing. Opening in append mode creates a missing file while preserving an existing file's contents.

Diary debugging points:

- `input()` returns a string: compare choices with `"0"`, `"1"`, and `"2"`, or convert them to integers.
- Ask for a new menu choice inside the loop so the user can change actions or quit.
- Keep startup reading outside the repeated menu loop if it should happen only once.
- When an entry already includes its newline, use `print(entry, end="")` to avoid adding another.

Example of saving an entry:

```python
with open("diary.txt", "a", encoding="utf-8") as diary:
    entry = input("Diary entry: ")
    diary.write(entry + "\n")
```

## 4. Restore the Text-Processing Helpers

**Keep and reuse:** `read_text`, `clean_words`, and `word_counts`.

```python
def read_text(path):
    with open(path, "r", encoding="utf-8") as file:
        return file.read()


def clean_words(text):
    words = text.lower().split()
    cleaned = []

    for word in words:
        word = word.strip(".,!?")
        if word:
            cleaned.append(word)

    return cleaned


def word_counts(text):
    counts = {}

    for word in clean_words(text):
        counts[word] = counts.get(word, 0) + 1

    return counts
```

Return types:

- `read_text`: string.
- `clean_words`: list of strings.
- `word_counts`: dictionary mapping words to integer frequencies.

## 5. Save Word Counts to a File

Independent task: read a text file, count its cleaned words, and write one word–count pair per line to a separate output file.

```python
def save_word_counts(input_path, output_path):
    text = read_text(input_path)
    counts = word_counts(text)

    with open(output_path, "w", encoding="utf-8") as output:
        for word, frequency in counts.items():
            output.write(f"{word}: {frequency}\n")
```

Use different input and output paths. Opening the input path in write mode would erase the original file.

For input:

```text
Go, go! STOP. go
```

Expected output file:

```text
go: 3
stop: 1
```

The original implementation looped over keys and used `counts[word]`, which also works. `.items()` makes the word and its frequency available together.

An explicit final `return` is optional. This function's purpose is saving a file, so returning `None` is appropriate.

## 6. Test Writing Behavior

The supplied test runner checked the following cases. All passed.

| Case | Input | Expected result |
|---|---|---|
| Repeated words | `Go, go! STOP. go` | `go: 3`, `stop: 1` |
| Case and punctuation | `Cat! CAT, dog?` | `cat: 2`, `dog: 1` |
| Punctuation only | `!!! ... ???` | Empty output file |
| Empty input | Empty file | Empty output file |
| Whole-word counting | `cat cats catfood cat` | Separate counts for `cat`, `cats`, and `catfood` |
| Multiple lines and blank lines | `Hello world!`, blank line, `hello` | `hello: 2`, `world: 1` |

Additional checks:

- Running the function twice does not duplicate output.
- Replacing the input with `bird` leaves only `bird: 1` in the next output.
- Input file contents remain unchanged.

This connects the complete process: read text, clean it, count words, then save the result.

## 7. Catch a Missing-File Error

```python
print("Before")

try:
    text = read_text("definitely_missing_practice_file.txt")
    print("Read successfully")
except FileNotFoundError:
    print("File missing")

print("After")
```

If the file is missing, the output is:

```text
Before
File missing
After
```

Python skips the remaining statements in the `try` block after the error, runs the matching handler, and continues after the whole block. This handler does not create the file.

## 8. Distinguish Missing and Empty Files

**Reuse:** `read_text`.

```python
def try_read_text(path):
    try:
        text = read_text(path)
        return text
    except FileNotFoundError:
        return None
```

| Situation | Return value |
|---|---|
| File contains text | The text as a string |
| File exists but is empty | `""` |
| File is missing | `None` |

Catch the specific exception expected here. Other errors are not automatically handled by `except FileNotFoundError`.

## 9. Check Specifically for `None`

```python
text = try_read_text(path)

if text is None:
    print("Missing file")
else:
    print("File found")
```

For an existing empty file, this prints `File found`.

`if not text:` would treat both `None` and `""` as false values, losing the distinction. Use `is None` for this check. `== None` worked with the values in this exercise, but `is None` is the standard form.

## 10. Restore the Word-Report Helpers

**Reuse:** `clean_words` and `word_counts`.

```python
def frequent_words(text, minimum):
    counts = word_counts(text)
    selected = []

    for word, frequency in counts.items():
        if frequency >= minimum:
            selected.append(word)

    return selected


def word_report(text, minimum):
    counts = word_counts(text)

    print(f"Total words: {len(clean_words(text))}")
    print(f"Distinct words: {len(counts)}")

    for word, frequency in counts.items():
        print(word, frequency)

    print(f"Frequent words: {frequent_words(text, minimum)}")
```

## 11. Report Only When the File Exists

**Reuse:** `try_read_text` and `word_report`, including their helper dependencies.

```python
def report_if_exists(path, minimum):
    result = try_read_text(path)

    if result is None:
        print("File not found")
    else:
        word_report(result, minimum)
```

All three tests passed:

- Normal file: prints its word report.
- Empty file: prints zero total words, zero distinct words, and an empty frequent-word list.
- Missing file: prints `File not found`.

## 12. Review Questions

**- How do `"w"` and `"a"` affect an existing file?**
"w" automatically erases all existing content, while "a" just adds after.

**- Which modes create a file if it does not exist?**
"w" and "a".

**- Does `write()` add a newline automatically?**
Nope, we have to add that manually.

**- Why must input and output paths differ in the word-count export task?**
Or else we'll be replacing the input by the output.

**- What happens to the remaining statements in a `try` block when an exception occurs?**
They aren't executed. Just ignored lol.

**- Why return `None` for a missing file instead of `""`?**
So we can distinguish an empty file from a non-existent one.

**- Why does `text is None` distinguish cases that `if not text` combines?**
Cuz **if not text** treats **None** and **""** as the same, both being __Not Text__, whereas **is None** specifies that only **None** counts.

**- What do repeated-run and input-preservation tests check?**
Repeated-run tests check that rerunning the program doesn’t duplicate the output. Input-preservation tests check that the original input file remains unchanged.

## Keep for Future Practice

Save `save_word_counts`, `try_read_text`, and `report_if_exists`, along with the diary exercise.

Keep their dependencies: `read_text`, `clean_words`, `word_counts`, `frequent_words`, and `word_report`.

## Next Step

Briefly review file writing and missing-file handling, then practise invalid numeric input and `ValueError`.

[← Back to home](/)
