---
layout: default
title: "ENGR1010: lecture 12 - Arrays? NOT HURRAY."
---

# ENGR1010: lecture 12 - Arrays? NOT HURRAY.

## Key takeaways from the lecture:
Array basics;
Array and function;
C string: char array;
C-string processing functions.

## When should we add the brackets?
We only add brackets when we stating the prototype and at the beginning of the function implementation, to state that this is a array,
not just another char/double/integer variable. And when we refer to a certain value in the array, we also put the index inside the brackets to 
specify which value to choose.

However, when we actually put the array as an argument in a function call, we don't need to add the brackets, since we already know which type it is.

## Character Arithmetics
1. When you want to change a lower-case character for example x to the upper-case X, you can use **x += 'a' - 'A'**, since this derives from
**'x' - 'X' = 'a' - 'A'**.
2. When you want to determine whether this character **ch** is a digit, you can use **if ch<='9' && ch>='0'**.
3. When you want to convert a digit character to a integer, you can use **ch = ch - '0'**.

**BE WARE** Remember the **''** around the characters especially when they look like a number!!! Or else you'll literally be comparing actual
0s with character '0', which won't work the way you want it.

## Call by Value V.S. Call by Address
When doing call by value to ordinary variables, we assign the value of the input argument to a function parameter, and we can only change the value
of the parameter, not the input argument itself.

However, when we do call by address usually to arrays, the input argument is taken as memory address, not the array itself. So when you change the 
array inside the function, the original array is also changed! Because the parameter in the function and the input argument refers to the same
array, instead of a copy of array.

## Str commands
### strcat
When using **strcat**, the **\0** is overwritten and re-added at the end of the second string. However, what if we wanna add **\n** at the end to change to a new line? Should it go before or after the **\0**?
In fact, it should go before as **\0** declares the end of a string, so the **\n** in **\0\n** is ignored automatically, whereas in **\n\0** we first change to a new line then end the string.

### strcmp
When using **strcmp**, we compare the values of the two strings. However, if we do **string1==string2**, the answer is almost always 0! Because **==** doesn't compare the value of the character, instead it compares the address of each string in memory, which is of course different for every string.

[← Back to home](/)
