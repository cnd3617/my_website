---
categories:
- ""
- ""
date: "2017-10-31T22:26:09-05:00"
description: Lorem Etiam Nullam
draft: false
image: C++.jpeg
keywords: ""
slug: magna
subtitle: 2018-2019
title: C Compiler & MIPS CPU Emulator
---

# Overview

Built a C to MIPS Compiler and an emulator that executes binary files and simulates actions on CPU registers and disk memory.     
Obtained 83% – C++, MIPS

See the code on [github](https://github.com/cnd3617?tab=repositories).

Refelection:

# Project and Time Management

## Important Note

We started our project as soon as possible, to avoid being pressured at the end of the term and not be disturbed by clashing deadlines. Therefore, we worked on another repository, which you can find on Imperial College's GitHub (`gilet_jaune`) where there is the detailed about the commit logs.

## Milestones Review

1.  Basic Lexer

Estimated time: 5

Actual effort: 10

Coding the first version of the lexer consists of identifying all the tokens used in C and writing regular expressions for integers and identifiers. This task took us longer than expected as we forgot to initially take a step back from our project. We had to have a clear view of how to design our compiler. We started by listing all the tokens used in the different features of the C language. Then, we wrote the code.

Conclusion: It is better not to start too soon and plan before starting to code. Understanding how the compiler works as a whole was particularly important. It cost us 5 hours.

2.  Improved Lexer

Estimated time: 5

Actual effort: 1

This task took us less time than expected. Indeed, as we increased the number of hours for the first milestone, we produced a more complete Lexer than expected. We only had to specify the regular expressions for strings, integers, hexadecimals, float and identifiers.

Conclusion: It is better to plan effectively. We reduced the time spent of 4 hours compared to what was expected. We respected the time we gave us for the lever.

3.  Basic Parser

Estimated time: 20

Actual effort: 25

We spent 10 hours understanding the grammar of C and the structure of all the features. We decided to create a parser that would work for all the features of C89 so that we will be free to implement advanced features in code generation without changing the way we build the AST. After this was done, we then started to write it. Writing a basic parser that implemented most expressions took us 10 hours. We executed this task efficiently. However, without the AST objects, we could not test our design.

Conclusion: The planning was effective but we did not follow a test-driven design as we could not test the code. We might have to solve a lot of bugs later.

4.  Improved Parser and AST generation

Estimated time: 30

Actual Effort: 50

We under-estimated this task. It was one of the greatest challenge for us. We wanted to test our parser before starting to design the Python translator. To do this, we wrote an AST which was supposed to print any code inputted from C. This was done quickly as it was quite a straightforward task, it took us 20 hours. We used the tests given for the translator to test the parser. We faced various issues: wrong types in the parser (parameters), wrong linking in the parser (syntax errors) etc. Fixing them took us 30 hours.

Conclusion: Doing things step by step was a good idea. We can now move on with a fully functional parser. We went over the time estimation as we did not think the testing would be that long.

5.  Python Translation and Global Variables

Estimated time: 30

Actual Effort: 30

It was easy to move on to python translation as we now have a functional parser that supports all the features requested. We added to our abstract syntax tree a translating function. To make it easier to read the code we changed the structure of our files. We divided the really long file into many different files for each feature. It will help us to continue the project and to debug our code. We had a hard time to understand the principles behind global variables.

We tried to implement them following this hypothesis:

-  If the function is the main, no need to declare the global variables

- Otherwise, do it.

That turned out to make us pass all the tests from the Python formative.

Conclusion: Organisation is important, the more we stay focused on the project, the better we are at it. Working on it our full time is great, we begin to see the results.

6.  MIPS Code Generation

Estimated time: 140

Actual Effort: 205

We begin the code generation part thinking it would be comparable to the python translation, we quickly understood it is not. We started methodically, first the expressions, then the conditions, then the statement, then the function. After a full week of intense work, we could see some results and MIPS code generated,  it felt good. We quickly decided to take care of register and memory allocation. After this was done, generated code was greatly improved. We were checking our results with https://godbolt.org/ (C to MIPS compiler). Another full week working on it helped us to get to the point where most of the ‘Intermediate features’ were done. We spent, these two weeks, much more time than we should have. However, the more we were coding the better the compiler, so we kept on going. We started on using ‘qemu-mips’ to create our own test-cases and fixed the different issues we were having. It was hard. Finally, we could see the progress with each of the test-cases we would pass.

Conclusion: Test-driven software design made working on the project more fun and interesting even when it was really difficult. The challenge of passing more tests, kept us working on the compiler. However, we believe we could have shortened the time spent on these parts. We could have planned the implementation of more advanced features and write code that was future-proof. We spent quite a lot of time revising what we implemented before.

7.  Codegen of Functions and Arrays

Estimated time: 70

Actual Effort: 40

We continued working on the compiler and chose one part each of us will do for the harder features. We chose to develop function calls with more than 4 parameters, arrays and enum. Arrays required us to be better at allocating memory from the stack. To make them work, we had to change the way our AST was producing the code generation. We also had to think about pointer arithmetics. We did not have any experience using enum so we had to learn about them first. The function calls with more than 4 parameters were hard. It was a real challenge to understand how to store the added parameters. This took us quite a lot of time for all these advanced features (3 days). This gave us a better understanding of how the stack and memory work. Together, we finally added some small feature (global variables in MIPS, pointers…).

Conclusion: We learned a lot by working each of us on different parts of the project. However, together we are more efficient. We debate a lot and question the decision we make to be sure not to go the wrong decision. This task was still done quicker than expected.

8.  Clean-up

Estimated time: 20

Actual Effort: 30

One element was changed in the spec: the way global variables are used in Python. We were wrong about their implementation. We then changed them accordingly to the spec. Additionally, we took time for formatting the outputs (putting the /t where needed, formatting our code…). It was done efficiently, but the unexpected change in the spec modified the effort planed.

Conclusion: We lost time due to the globals. This is the downside of starting early. We should have looked more before making assumptions.

## Conclusion

The project taught us a lot about software design and development. It gave us the opportunity to use various coding languages, but also understand the way a language is defined. Also, it was great to work in pair as we could split the work but also work side by side when needed. Starting earlier was a good choice:

-  We learned a lot, could apply what we were learning from the lectures in real time.

-  Managed our time so the end of the term would not be too stressful.

-  Could apply our learning from the labs exercise in real time.

-  Although the specs changed over the weeks, it helped us to be more flexible which is a great quality to have when producing software in a professional environment.

Time Management:

Planned = 320 hours

Actual = 391 hours

Ratio = 1.22

We exceeded our prediction. We believe we could have reduced the time spent by having a better designed plan in the beginning. However, we are still happy to be finished with one of our biggest and most interesting coding projects.