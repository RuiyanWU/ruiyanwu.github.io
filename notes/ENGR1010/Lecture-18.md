---
layout: default
title: "ENGR1010: Lecture 18 - Mid 2 prep."
---

# ENGR1010: Lecture 18 - Mid 2 prep.

## Don't use "==" to compare strings!
The name of a string refers not to the string itself but to its address in the memory. So when we try to use the operator **==** to determine whether or 
not the strings are the same, we're actually comparing two separate memory addresses, which are o course different!

## Strcmp returns 0 for true!
Remember, strcmp doesn't return 0 for false and 1 for true as MATLAB string compare function does. Instead, it returns 0 for same and 1 if the first's ascii 
code is greater than the second, -1 other way round.

## Condition statements!
I totally forgot about condition statements since I've never used them during programming.

**max = (a>b) ? a : b;** means that max = a when the condition (a>b) is true, and max = b when it's not.

## Switch statements!
Same as condition statements, but it's never too late to rediscover stuff especially before the exam LOL.

switch (variable or expression){
case value1:
statements1;
**break;**

case value2:
statements2;
**break;**

...

default:
statementsN;
__break;__
}

The two **break;** in bold are both necessary for the function to skip any remaining loop parts whereas the final __break;__ in italics isn't necessary
since after the default case, the block is closed by the curly brackets so no big deal even if you don't end the loop manually. But better add it for better
coding style!

## Static variables!
Static variables are very similar to global ones since they are all saved throughout the life time of the program, and both stored in the same memory 
location in **stack** instead of the allocated memory in **heap**. However, a static variable declared in a local function can only be used in the function, 
though it's memory will be preserved between multiple function calls. The global variable, on the other hand, can be accessed both in loops and out, so might
be mistakenly modified.

## Returning for a void function
In fact, functions with the return type **void** can also use **return;** in their codes to abort the execution of the program early during loops or in 
branches. However, remember to check that the function isn't actually returning anything, cuz void functions don't support return types.

## sprintf
By using **sprintf**, we can convert numbers to string and returning the number of converted characters (length of the string), function prototype like this:
**int n = sprintf(char * array_name, "format type", int number);**
This works for int ("%d"), float ("%f") and other datatypes.

## Switching by address
To actually switch the outside variables, a function has to take in memory addresses as arguments and dereference them to change the values. You cannot simply switch the pointers since no values local to the function will be returned or be able I=to impact the outside.

## constant pointers and pointer constants
In fact, using a constant pointer on an array doesn't automatically makes in unchangeable! When you use the array name such as **my_array[0] = 0;**, it is totally acceptable. However, changing it via the assigned constant pointer is not allowed. When **const char * p = array;**, you cannot use **p[0] = 0;**.

On the other hand, the use of a pointer constant is totally different. In this case the pointer is a constant and you cannot have it refer to any other array, but the array itself is changeable. **char * const p = array**.

We can also do **const char * const p = array;** where both the pointer and the array it points to are regarded as constants.

## fclose
Always remember to add fclose after we're done with a file! The function returns **0** if the file is successfully closed, and **EOF** if it isn't.

## fprintf & fscanf
These two function both have return types of **int**, the former returning the number of printed characters, whereas the latter returns the number of matched characters, or **EOF** if arriving at the end of file without reading anything matched.

## Memory allocation
When the execution of the main function is finished, all memory allocated during its execution will be freed automatically, but still we should use **free** to manually free the dynamically allocated memory during the execution of the program to avoid memory overflow, and of course to get points during the exam.

## Structure
Remember the **;** after the block of declaration of the struct!

[← Back to home](/)
