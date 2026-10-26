---
layout: default
title: "ENGR 1010: Mid-terms recap."
---

# ENGR 1010: Mid-terms recap.
# MID-TERMS IS COMING! (and is gone)

## Pre-exam RC (Big RC)
The Big RC is really just **HUGE**. Like there were four parts, each presented by one TA, and separate exercises for each part 
(not to mention that part four is basically just a loooot of really hard questions so...) And since two classes had this together, there was 'bout
two hundred ppl packed into one classroom which made it really hard to ask questions and stuff.

Before the RC I went over most of the materials by myself (as what I always do for ENGR1010) in case there's smth that I absolutely can't understand.
But just reading through everything took me like two days' free time, and I totally couldn't figure out many questions on my own.

The RC itself wasn't much better, but at least I survived it and left with A LOT of new-found understanding of MATLAB so definitely worth it.

## The EXAM!
However, the exam itself was sooo much better than I expected. I literally had 20 minutes to go over my answers (and found one miscapitalization,
two missed semi-colons and one end.) Thank god I had time to double-check. I mean pen and paper and me, we're definitely Turing complete, but that doesn't mean I'm automatically generating the output of the codes in my mind.

## The grades and where I could have done better
The grades came out RIO fast, and I went paper checking despite being quite satisfied with them (well, it's definitely not gonna get me an A+, but 95+ is 
much better than my initial hope of not failing). Turns out I made 3 mistakes of different kind: 

First on the printing of a vector: when we do _fprintf("%d",A)_ and A is a column vector, A is not printed out as a column, but the values in A will be
printed out one by one in a row. Because %d does not take a full vector, just separate scalars.

Second was again on printing, but this time on printing an integer. In fact, I totally forgot that you cannot print _075_ as an integer. It's automatically
turned into _75_ as we omit all trailing zeros.

Last is about short MATLAB commands. Turns out a **while** statement is only complete with a **end** at the end, or else it won't be executed! I actually 
spent a long time debating whether to add the _end_ or not during the exam, but decided against it as the Professor said we could only write one line(?

Anyways, everything went better than planned, and hopefully I'll enjoy C as much as I enjoyed MATLAB!

[← Back to home](/)
