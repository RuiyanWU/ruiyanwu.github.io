---
layout: default
title: "ENGR1010: lecture 3 My questions & thinking"
---

# ENGR1010: lecture 3 My questions & thinking

## Key take aways of the lecture:
Matrices and arrays;
M-file and functions;
Data types;
Characters and strings.

## My questions
**1. When we delete rows and columns, we use** _A(m,:)=[ ]_. **Then what if we put** _A(m,n)=[ ]_? **Will one arbitrary block be deleted?**

No, in MATLAB, a null assignment can have only one non-colon index.
Indeed, if we only delete one arbitrary block of the matrix, the others left can't actually form a matrix anymore, 
so the command itself is invalid.

**2. When we use** _str2double(3.14159)_ **command, due to preference of MATLAB, the output would be** _3.1416_ **in float point instead of** _3.14159_.
**But when used in calculation, will it still be 3.14159?**

Yes, the precision isn't changed, it's just the way of MATLAB showing numbers, not how its stores them.

[← Back to home](/)
