---
format: Defaut
title: "Lecture 2: Much quicker than I imagined?"
---
# Lecture 2 on Python: Loops, Counting, and Filtering.
## The Study Plan
**Date:** October 16, 2026  
**Planned study time:** 10:00-11:00  
**Focus:** Using `for` loops and `range`, calculating totals, counting, and filtering lists

## 1. Review List Operations

Predict the output before running:

    marks = [72, 85, 91]
    saved = marks.pop(1)
    marks.append(60)

    print(marks)
    print(saved)

Expected output:

    [72, 91, 60]
    85

Questions:

- Does `pop(1)` remove an index or a value?
- How does its return value differ from that of `remove()`?

## 2. Loop Through a List

Run:

    marks = [72, 91, 60]

    for mark in marks:
        print(mark)

On each iteration, `mark` holds the next value in the list.

Predict the output order:

    for mark in marks:
        print(mark)
        print("checked")

    print("finished")

Questions:

- Which statements repeat?
- Which statement runs only once?
- How does indentation determine the loop body?

Practice: print each mark plus `5`, then print `"done"` once after the loop. Keep the original list unchanged.

## 3. Use `range`

The start value is included; the stop value is excluded.

    range(4)       # 0, 1, 2, 3
    range(2, 5)    # 2, 3, 4

Predict the output:

    for number in range(1, 4):
        print(number * 2)

Expected output:

    2
    4
    6

Question: What happens if the loop uses `range(0)`?

## 4. Calculate a Running Total

The logic:

1. Initialize a total before the loop.
2. Visit each mark.
3. Add the mark to the current total.
4. Print the result after the loop.

Complete the blanks:

    marks = [72, 91, 60]
    total = 0

    for mark in marks:
        total = _____

    print(_____)

Expected output:

    223

Questions:

- Why is `total = 0` placed before the loop?
- What would happen if it were inside the loop?

## 5. Count Passing Marks

Use `60` as the pass mark, including marks exactly equal to `60`.

    marks = [72, 45, 91, 60, 38]
    count = 0

    for mark in marks:
        if _____:
            count = _____

    print(count)

Expected output:

    3

Distinguish between:

- Adding a mark’s value to a total.
- Adding `1` to count a qualifying item.

## 6. Filter a List

Create a new list containing only passing marks.

    marks = [72, 45, 91, 60, 38]
    passing = []

    for mark in marks:
        if _____:
            _____

    print(passing)

Expected output:

    [72, 91, 60]

Questions:

- Why do we start with an empty list?
- Which list method adds a qualifying mark?
- Does this code change the original list?

## 7. Combined Exercise

Start with:

    marks = [72, 45, 91, 60, 38]

Use one `for` loop to:

1. Calculate the total of all marks.
2. Count marks greater than or equal to `60`.
3. Collect those passing marks in a new list.
4. Print all three results after the loop.

Expected output:

    306
    3
    [72, 91, 60]

Check which updates belong inside the `if` and which should happen for every mark.

Then test with:

    marks = []

Expected output:

    0
    0
    []

Explain why the initial values remain unchanged.

## 8. Independent Practice

Start with:

    marks = [59, 60, 84, 32, 100]

Write code without referring to the previous solution:

1. Count the marks below `60`.
2. Collect them in a new list.
3. Print the count and the list.

Expected output:

    2
    [59, 32]

Boundary check: `60` should not appear in the failed marks.

## 9. Python and MATLAB Comparison

| Concept | Python | MATLAB |
|---|---|---|
| Loop body | Begins after `:` and uses indentation | Ends with `end` |
| Integers from 1 through 4 | `range(1, 5)` | `1:4` |
| Add an item at the end | `items.append(value)` for a list | `items(end + 1) = value` for a numeric row vector |

Python example:

    for mark in marks:
        print(mark)

MATLAB equivalent for a numeric row vector:

    for mark = marks
        disp(mark)
    end

## 10. Review Questions

- Does `for mark in marks` give values or indices?
- Is the stop value in `range` included?
- What is the difference between a total and a count?
- Why are the starting variables initialized before the loop?
- How does indentation affect conditional updates?
- What happens when the input list is empty?

## Scope and Next Session

Leave functions and more advanced loop features for a later session.

Skip the weekend Python session to prioritize the MATLAB midterm on October 19. Resume afterward with a short review,
then practise using these ideas in functions.

## Recap
I was totally relying on GPT today, as it didn't tell me to use the mooc lectures for this session. And I've actually used lists in this way before
and am very familiar with for and range, so today's lecture took me only 25min to complete. I was sort of surprised and asked GPT to get me more stuff.
However, it told me that I've already covered everything necessary for the day and advised me that there's no need to fill the time scope.

I was absolutely astonished.

I mean, I kept working overtime when the assigned work isn't complete, and almost always forced myself to find more things to do if current work 
is done quickly. So basically I've never enjoyed the happiness of completing stuff before time limit, which sort of lead to my tendency of procrastination.
Well, when there's no reward, only more work to do after hard work, I guess no one's gonna be so eager to finish the task anymore.

So, from now on, I'll try to take everything more easily and actually respect my own working plans. I absolutely don't have to try study ahead of 
myself. But I will adjust the bigger plan if this happens very often. Though I'll probably still work over time to finish current work...

As for tomorrow, I should be doing more practice on Python. However, I've got RC on MATLAB and mid-terms on MATLAB next Monday, so GPT advised me
to skip Python for the rest of the week and pick it up after the exam to not get confused and mix up python and MATLAB. I totally agree, so my
next update will probably also be on MATLAB review lol.

[← Back to home](/)
