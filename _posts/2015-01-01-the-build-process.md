---
layout: post
title:  "The Build Process - C/C++"
date:   2015-01-01 21:00:00 +0530
tags: [Concept, C/C++]
published: true
comments: true
---
Originally posted to [HackerEarth](https://www.hackerearth.com/practice/notes/build-process-cc/).

### Introduction

With all the logic, manipulation and graphics worked out for several problems worked on. One must be curious to know how it works within.

Sequence of Events : **_Editor_** => **Type the Code** => **Press Build & Run** => **Wait**[Build Process]... => **Prompt/Window**

Ever noticed what’s going on while those 30 seconds or 20 seconds or maybe 10 seconds or 1 second of execution of your application until you see the prompt or a window appears on the screen.[WAIT..].

We often do miss on the abstract processes, right? One of them is The Build Process in C. The basic five phases of the process include:

![BuildProcess]({{ site.baseurl }}/assets/projects/blob/build_process.png){: class="center_85" }

### PreProcessing

Significant from the ‘#’ symbol also known as the preprocessor symbol in C. In this phase the "helloworld.c" source code file is preprocessed and the expanded source code is generated based in the directives like #define, #include, #ifdef, etc. with "helloworld.i" extension. 

This is the very same time when your `#define MAX 10000007` is replaced in the code before the syntactical check. i.e the defined constant is searched and matching tokens are replaced with the given expression. Also, widely used as in “#include” causes the preprocessor to paste the contents of “stdio.h” into the source code file at the location of the #include statement

```cpp
#include <stdio.h>
#define NEXTLINE printf("\n");

int main()
{
printf("The text in first line.");
NEXTLINE
printf("The text in the next line.");
return 0;
}
```
goes similar to this...

```
....
printf("The text in first line.");
printf("\n");
printf("The text in the next line.");
...
```

--------

### Compilation

The second stage includes the identification of syntax errors in expanded source code "helloworld.i" If found; the syntax errors are listed on the terminal with warnings and come back for corrections. 

On the other hand, error-free code is translated by the Compiler into an equivalent assembly language program with `helloworld.asm` or `helloworld.s` file extension. Different processors support a different sets of assembly instructions using which they can be programmed hence the same program on a compilation with Core i3 would generate differently ".asm" file than the one on a compilation with Core i5.

---------

### Assembling

Once the assembler code is generated it is then translated from "helloworld.asm" file to relocatable object code "helloworld.obj" or "helloworld.o". Its relocatable since no specific memory address has yet been assigned to the code and data sections in this relocatable code and all the addresses are relative offsets. 

The '.obj' file created is a specially formatted binary file which consists of a header and several sections. The header defines describes the sections that follow it which are :
- text section : consists of machine language code equivalent of the expanded source code.
- data section : contains global variables and their initialized values
- block started by symbols : known as BSS includes the uninitialized global variables. - symbol table : contains information about the symbols found during assembling of the program e.g. names,types and size of global variables etc.

The relocatable code "helloworld.obj" consists of machine language instructions but cannot be executed directly since external functions like `printf()`,`scanf()`, etc. are not present in them. You can give this one a try by visiting the obj>debug folder of any project you are working on.

**_Note_** - Any variable in an '.obj' file can be used in another '.obj' file as well as a function used in one '.obj' file can be defined in another ‘.obj’ file. This though leaves the symbol table incomplete, the references to such variables and functions are resolved by the linker.

-------

### Linking

The Linker plays a vital role in being the final stage in the creation of the well-known file “helloworld.exe”. It does followings -

it finds the definition of all external functions and global variables both from other '.obj' files and external libraries
it combines data sections of different '.obj' files into a single data section and
combines code sections of different '.obj' into a single code section.
Re-adjustment of addresses if required is done at the time of linking. The “helloworld.exe” file includes all machine language code from all of the input object files in its text section. During linking if the linker finds any library name misspelled, it stops the linking process and doesn't create an executable file.

-------

### Loading

Once the `helloworld.exe` is created by the linker and stored on the secondary storage. Upon execution its bought to the RAM by an Operating System component called Program Loader which places the executable anywhere in memory according to the availability.  All addresses in the “helloworld.exe” file code are realtime and the data are position-independent.

**_Note_** – Both “helloworld.exe” and “helloworld.obj” are formatted binary files and can't be used inter-platforms. e.g. Windows use Portable Executable(PE) while Linux uses Executable and Linking Format(ELF) hence .EXE file created in either cannot be used in the other.

```cpp
     ...[END WAIT] And the prompt thereby appears in front of the user.
```