---
layout: post
title:  "C Programming before and beyond main()"
date:   2015-02-11 21:00:00 +0530
tags: [Concept, C/C++]
published: true
comments: true
---
Originally posted to [HackerEarth](https://www.hackerearth.com/practice/notes/c-program-callling-a-function-before-main/).

### Introduction

There are few ways to use a function and execute some statements before and after main()

With GCC family of C compilers, we can mark some functions to execute before and after `main()`.
So some startup code can be executed before main() starts, and some cleanup code can be executed after main() ends. 

### Example

In the following program, `startupfun()` is called before main() and `cleanupfun()` is called after main().

```c++
#include<stdio.h>
/* Apply the constructor attribute to startupfun() so that it is executed before main() */
void startupfun (void) __attribute__ ((constructor));

/* Apply the destructor attribute to cleanupfun() so that it is executed after main() */
void cleanupfun (void) __attribute__ ((destructor));

void startupfun (void)
{    
   printf ("Startup method before main()\n");   
}

void cleanupfun (void)
{    
   printf ("Cleanup method after main()\n");    
}

int main (void)
{
  printf ("Hi! Check This Out.\n");
  return 0;
}
```
Got to learn this from [here](https://www.geeksforgeeks.org/functions-that-are-executed-before-and-after-main-in-c/).

--------------------------------------

### Pragma Directive

Using `#pragma` directive for special purpose of turning certain features on or off has been a practice. Though the same can be used with Turbo C/C++ compiler [compiler dependent] to allow a function to start before and end after main(). More than one function called before main should be defined in the reverse order of required execution.

```c++
#include<stdio.h>
void fun_before();
void fun_after();

/* Initializing methods for compiler to deal with at startup in reverse order and exit point of main method.  */

#pragma startup fun_before
#pragma startup fun_start
#pragma exit fun_after

int main()
{
    printf("Within the main!");
    return 0;
}

/* Declaring the start */
void fun_start()
{
    printf("At the starting of the game!");
}

/* Next pragma before the main */
void fun_before()
{
    printf("Lets go to the main()");
}

/* Ending all of it */
void fun_after()
{
    printf("Out of the main()");
}
```

Just to make sure we don't end up with syntactical errors these functions should not return or receive any value.