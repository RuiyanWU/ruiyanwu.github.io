---
format: Defaut
title: "Lecture 1: First self-studied Python class!"
---
# Lecture 1 on Python: Lists Basics.
## The Study Plan
**Date:** October 12, 2026  
**Planned study time:** 19:30–20:30  
**Focus:** Creating, accessing, and modifying lists
**Learning Method** I generated a study plan using ChatGPT, combing the practice questions it offered along with the official practices given
 by the mooc courses. So mainly I followed the course to learn the functions, and deepened my understanding with GPT.

## Learning Material

[University of Helsinki — Lists](https://programming-26.mooc.fi/part-4/3-lists/)

Read one short section at a time, run its examples, and modify them before moving on.

## 1. Create a List

Create `lists_day1.py` and run:

    marks = [72, 85, 91, 68, 85]
    print(marks)

Expected output:

    [72, 85, 91, 68, 85]

## 2. Access Items and Find the Length

Predict the output before running each line:

    print(marks[0])
    print(marks[2])
    print(marks[-1])
    print(len(marks))

Questions:

- Which index accesses `68`?
- What is the last nonnegative index in this list?
- What happens when accessing `marks[5]`? Why?

## 3. Change and Add Items

Predict the resulting list, then run:

    marks[0] = 74
    marks.append(90)
    print(marks)

Explain the difference between replacing an existing item and appending a new one.

## 4. Remove Items

Run each example with a fresh list:

    numbers = [10, 20, 30, 20]
    numbers.pop(1)
    print(numbers)

    numbers = [10, 20, 30, 20]
    numbers.remove(20)
    print(numbers)

Questions:

- How do `pop()` and `remove()` choose which item to remove?
- Why do these two examples produce the same list?
- What would `pop(20)` do on the original four-item list?

## 5. Independent Exercise

Start with:

    marks = [72, 85, 91, 68, 85]

Write code to:

1. Print the whole list.
2. Print its length.
3. Print its first and last items.
4. Change `68` to `75`.
5. Append `88`.
6. Remove the first occurrence of `85`.
7. Print the final list and its length.

Expected final list:

    [72, 91, 75, 85, 88]

Expected length: `5`.

Print the list after each change when debugging.

## 6. Output Prediction

Predict the final output before running:

    items = [4, 7, 4]
    items.append(9)
    items[1] = 8
    items.remove(4)
    print(items)

## 7. Review Questions

- What is a list?
- What is an index?
- What does `len()` return?
- What does `append()` do?
- How do `pop()` and `remove()` differ?
- Which part needs more practice?

## Scope

Leave insertion at a specific position, sorting, and the later lesson sections for another session.

## Recap

The GPT generated questions were quite simple, but the course's practice program were more of like a miniature project using skills other than the stuff
I just learned. In fact, I found that the learning wasn't too hard, but the actual application of the new learned skills took much longer than I imagined. 
My original schedule was from 19:30 to 20:30, but I didn't stop till 9:15, fully lost in the writing of programs. It's much more fun than I've ever imagined!
Like I was slowly picking back all the python skills I thought I've forgot and it was soooo funny. Can't wait to start session 2!

## About MATLAB

However, after learning MATLAB, I discovered that there are many differences between MATLAB and Python ToT. For example, I totally for got about the
_print(f"string{}")_ thing and went for _fprintf_ LOL.

More over, the _list_ in Python is quite similar to the vectors in MATLAB, but the printing of the list in python comes with the the brackets, but
the vectors come without brackets.

There are also many things to mark, for example, the first item in Python is indexed as **list[0]**, just like in C, but in MATLAB is the **A(1)**,
the last is **list[-1]** in Python, and **A(end)** in MATLAB. The index of the items are put in **brackets []** in Python and **parenthesis ()** in MATLAB.

