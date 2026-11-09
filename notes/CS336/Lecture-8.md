---
layout: default
title: "Python lecture 8"
---

# Python Session 8 — Reading Files and Processing Text

**Dates:** November 7–9, 2026  
**Session:** Friday's file-reading lesson, continued across the weekend and Monday  
**Focus:** Reading whole files and individual lines, numeric conversion, file position, and reusable file-analysis functions

## Learning Material

[University of Helsinki — Reading files](https://programming-26.mooc.fi/part-6/1-reading-files/)

Selected sections: “Reading data from a file” and “Going through the contents of a file,” plus the “Largest number” exercise. Leave CSV processing for later.

## 1. Read a Text File

Create `sample.txt` containing:

```text
Go, go!
STOP stop stop wait.
```

Open it using its full path:

```python
path = "/Users/yolanda/Documents/SJTU/26FALL/336 prerequisites/sample.txt"

with open(path, "r", encoding="utf-8") as file:
    text = file.read()

print(text)
```

- `"r"` means read mode.
- UTF-8 specifies how the file's bytes are decoded into text.
- `.read()` returns the remaining contents as one string, including line breaks.
- `with` closes the file automatically when execution leaves its block.

An absolute path identifies the file independently of the terminal's current working directory. A relative path such as `"sample.txt"` is resolved from the current working directory, which is not always the script's folder.

## 2. Reuse the Text-Analysis Functions

**Keep:** `clean_words`, `word_counts`, `frequent_words`, and `word_report` from earlier sessions.

```python
word_report(text, 2)
```

Confirmed output:

```text
Total words: 6
Distinct words: 3
go 2
stop 3
wait 1
Frequent words: ['go', 'stop']
```

`split()` with no argument treats the newline as whitespace, so words can be counted across multiple lines.

## 3. Compare Numbers, Not Numeric Strings

The MOOC's “Largest number” exercise reads integers from `numbers.txt`, one per line.

The first attempt compared strings and returned `45` instead of `1100`:

```python
"45" > "1100"  # True: compares characters
45 > 1100      # False: compares numbers
```

Convert each line before comparing:

```python
num = int(line)
```

`int()` accepts surrounding whitespace, including a trailing newline. Removing `"\n"` first is unnecessary here.

Initialize the maximum from actual file data rather than assuming it is at least zero:

```python
def largest():
    with open("numbers.txt", encoding="utf-8") as numbers:
        first = 0
        for line in numbers:
            num = int(line)
            if first == 0:
                biggest = num
                first = 1
            if num > biggest:
                biggest = num
    return biggest
```

This version assumes a nonempty file containing valid integers. For an empty file, `biggest` is never assigned; empty-file behavior would need to be specified separately.

Boundary test to check first-number initialization:

```text
-20
-3
-100
```

Expected result: `-3`.

Avoid naming a variable `max`, which would hide Python's built-in function with that name in the same scope.

## 4. Return File Contents from a Function

```python
def read_text(path):
    with open(path, "r", encoding="utf-8") as file:
        text = file.read()
    return text
```

The file closes before the return, but the string remains available.

```python
text = read_text(path)
word_report(text, 2)
```

For an empty file, `read_text` returns `""`. The report then displays zero total words, zero distinct words, and an empty frequent-word list.

## 5. Inspect Newlines with `repr()`

```python
with open(path, encoding="utf-8") as file:
    for line in file:
        print(repr(line))
```

Observed output for `sample.txt`:

```text
'Go, go!\n'
'STOP stop stop wait.'
```

`repr()` exposes hidden characters such as newlines. The final line has no trailing newline, which is valid.

```python
print(repr(""))    # ''
print(repr("\n"))  # '\n'
```

## 6. Count All Lines

```python
def count_lines(path):
    count = 0
    with open(path, "r", encoding="utf-8") as file:
        for line in file:
            count += 1
    return count
```

Confirmed results:

- `sample.txt`: `2`
- `empty.txt`: `0`

A blank line still counts as a line. The function returns the result so other code can use it; printing belongs at the call site.

## 7. Count Nonblank Lines

```python
def count_nonblank_lines(path):
    count = 0
    with open(path, "r", encoding="utf-8") as file:
        for line in file:
            content = line.strip()
            if content:
                count += 1
    return count
```

`strip()` removes surrounding whitespace:

```python
"\n".strip()       # ''
"   \n".strip()    # ''
"Hello\n".strip()  # 'Hello'
```

An empty string is false in a condition; a nonempty string is true. For strings, `if content:` has the same effect here as `if content != "":`.

Checking `content != "\n"` after stripping would incorrectly accept blank lines because `"" != "\n"` is true.

Important distinction: numeric `0` is false, but the nonempty string `"0"` is true. A line containing `0` counts as nonblank.

## 8. Combine the Helpers into a File Report

**Reuse:** `count_lines`, `count_nonblank_lines`, `read_text`, and `word_report`, including the earlier helpers called by `word_report`.

```python
def file_report(path, minimum):
    print(f"Total lines: {count_lines(path)}")
    print(f"Nonblank lines: {count_nonblank_lines(path)}")
    word_report(read_text(path), minimum)
```

For `lines_test.txt` containing `Hello`, an empty line, a spaces-only line, and `Python`, the report with `minimum=1` prints:

```text
Total lines: 4
Nonblank lines: 2
Total words: 2
Distinct words: 2
hello 1
python 1
Frequent words: ['hello', 'python']
```

For an empty file:

```text
Total lines: 0
Nonblank lines: 0
Total words: 0
Distinct words: 0
Frequent words: []
```

Each helper opens the file separately. This is acceptable for the small practice files; it means the report reads the same file multiple times.

## 9. Understand File Position

```python
with open(path, encoding="utf-8") as file:
    first = file.read()
    second = file.read()
```

The first call reads to the end. The second returns `""`; it does not automatically restart. Reopening the file starts a fresh read from the beginning.

Reading one line also advances the position:

```python
with open(path, encoding="utf-8") as file:
    first_line = file.readline()
    remaining = file.read()
```

For `sample.txt`:

```python
first_line == "Go, go!\n"
remaining == "STOP stop stop wait."
```

Three successive `readline()` calls return the first line, the second line, then `""`.

- `"\n"` means a blank line was read.
- `""` from `readline()` means end-of-file.

## 10. Return the First Nonblank Line

```python
def first_nonblank_line(path):
    with open(path, "r", encoding="utf-8") as file:
        for line in file:
            content = line.strip()
            if content:
                return content
        return ""
```

The first return exits as soon as a match is found. The final return runs only if no nonblank line was found. The file closes even when returning from inside the `with` block.

Confirmed tests:

- Blank lines followed by `"  Hello!  "`: returns `"Hello!"`.
- Whitespace-only file: returns `""`.
- Empty file: returns `""`.

Use `print(repr(first_nonblank_line(path)))` to make empty results visible.

## 11. Collect All Nonblank Lines

```python
def nonblank_lines(path):
    nonblank = []
    with open(path, "r", encoding="utf-8") as file:
        for line in file:
            content = line.strip()
            if content:
                nonblank.append(content)
    return nonblank
```

Unlike the previous function, this one must finish its loop before returning.

Confirmed tests:

- Normal text mixed with blank lines: returns the stripped nonblank lines in order.
- Whitespace-only file: returns `[]`.
- Empty file: returns `[]`.

`first_nonblank_line` returns a string; `nonblank_lines` returns a list of strings.

## 12. Review Questions

**- What does `.read()` return for an empty file?**
An empty sting "".

**- When does `with` close the file?**
When the block ends, the file is closed automatically, unlike in C where you have to use fclose.

**- Why must numeric strings be converted before numeric comparison?**
Or else they will be compared as strings, which compares the Ascii value of each character separately.

**- Why can initializing a maximum to zero fail?**
Cuz sometimes all the rest might be minus.

**- How does `"\n"` differ from `""` when using `readline()`?**
**"\n"** is actually read, meaning that there's a empty line but with a line break, whereas **""** literally means there's nothing stored.

**- What happens when `.read()` is called twice on the same open file?**
The second call returns nothing since we have already arrived at the end of the readable file.

**- Why does `if content:` accept `"0"` but reject `""`?**
Cuz in Python, "0" is True whereas "" the empty string is False.

**- Why does finding the first match require a different return placement from collecting all matches?**
Cuz you can return right after finding the match in the former but have to actually go through the whole file for latter.

## Keep for Future Practice

Save `largest`, `read_text`, `count_lines`, `count_nonblank_lines`, `file_report`, `first_nonblank_line`, and `nonblank_lines`.

Also keep the earlier dependencies: `clean_words`, `word_counts`, `frequent_words`, and `word_report`.

## Next Step

Briefly review file reading, then learn to write results to files.

[← Back to home](/)
