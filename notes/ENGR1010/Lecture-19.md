---
layout: default
title: "ENGR1010: Lecture 19 - C++ pointers"
---

# ENGR1010: Lecture 19 - C++ pointers
## Key takeaways
Ct+ library and namespace;
Console I/O;
Object-Oriented Programming;
Class Definition and Implementation;
C++ Classes: string and vector;
C++11 Features: auto and range for.

## Comments
In C, we do comments using **/*...*/**, and in C++, we do comments using **//**.
However, we can in fact do comments in both C and C++ using any of the two, that is to say, **/*...*/** also works in C++ and **//** in C.

## Operator chaining
When I first learned about C++, I had this question:
**Why can we use one single "cout" for multiple "<<"s?**

<p align="center">
  <img src="images/101-lecture19-P1.png" alt="Lecture 19 picture" width="400">
</p>

In fact, it is because the **<<** operator in C++ returns the output stream itself (cout), allowing another << operation to follow immediately.
So there is technically a **cout** in front of every **<<** returned by the previous function call.

## Function overloading
We've learned in class that we can use the same function name for parameters of different data types. For example:

**int func(int a) {return ++a;}
int func(double a) {return (int) a + 2}**

However, what if we have two functions under the same name with the same parameter but with two different output data types? Will it work?

NOPE! In fact the program won't even compile successfully, cuz the program won't be able to know which function to call during the execution.

## Inheritance
So this is an interesting feature in C++ that I've never seen in C before. You can sort of reuse the members defined in a base class when you inherit from 
it to to create a derived class, so you don't have to define everything all over again.

[← Back to home](/)
