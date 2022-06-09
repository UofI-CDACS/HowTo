# Doug's Tips for programming in C++

C++ is a complex language. Writing C++ code can be a daunting task, especially when starting out.
Some things can be particularly hard to figure out. Here is a collection of tips for dealing with various issues.

## Trustworthy Up to Date Documentation

I usually use https://en.cppreference.com/. I find this source to be accurate. Keep an eye out for which versions of C++ a feature is available.

There are other sources. Find one you like. There are many books available as well. I used to have several that I used but have found that online sources are much more convenient.

### Stackoverflow

https://stackoverflow.com/ is a source with a lot of good and bad information. Be careful as many answers may be good for older versions of C++ but not more modern versions. 

Often there is more than one good answer and more than one poor answer. It is a good practice to view every answer with a healthy amount of skepticism. 

## Pick a Standard

C++ is an evolving language. What was true for C++ 98, 03 etc. may be different in a more modern version. Generally features move forward, but some will disappear. Currently I like to use C++ 17. 

Anything before C++ 11 is probably not a good idea. C++ 11 provided a huge step forward in usability. 

Newer versions are available, but I have found that it is best to let technology settle for awhile. At the time I'm writing this C++ 23 is on the horizon, C++ 20 is still a bit early to be using as some features may not be supported by all compilers on all platforms.

### Stay Informed

It is not necessary to learn every feature of C++. However, having a good idea of what is available and how it might be useful is worthwhile. When you have a problem to solve look for a C++ solution and use that as an opportunity to learn more.

Articles about new versions of C++ are usually worth reading to get an idea of what is coming.

## Understanding Compile Errors

C++ is notorious for very difficult to understand compilation error messages.

There are several simple things that can be done.

* Keep your code as simple as possible
* Compile and test incrementally
   * Fix every warning and error before writing more code
   * Test your compiled code before writing more code
   * Write unit tests for your code, even incomplete unit tests provide help in diagnosing newly introduced problems.
* When you see a compile error or warning
  * Try fixing the first one you see and compiling again. Often, especially if it is in a header file one error can be responsible for most of the messages.
  * Sometimes the first error reported isn't the real problem. If the first error doesn't have an obvious solution, try fixing the second error.
  * Usually fix one error at a time then recompile, however, when you start getting a handle on the problems you can often fix many similar errors through search and replace.
* The more errors you see, the more quickly you will be able to fix new errors.
* As errors are fixed the number of remaining errors often drops very quickly.
* Sometimes compiling with a different compiler can help.
  * Visual Studio, XCode, GCC, and CLANG are options.
* Have someone else look at the code. Even if they aren't expert at C++ they may be able to spot the problem.
* Use a divide and conquer approach, Errors may not be on the line that was indicated.
  * Comment out code.
  * Move it around. 
  * Put expressions into intermediate variables rather than passing them as an argument.
* Check for typos. I often transpose characters when I type.
  * Make sure the data types are correct. 
  * Look for const in the error message and make sure that matches the prototype
  * Try to use distinctive names for types. Similar names can be easily confused.


## Document your code

I use Doxygen to generate documentation. The act of writing documentation so that an API can be understood without looking at the actual code helps to reduce confusion. It is a way to look at your code from a different point of view.d

## Use a Code Spell Checker

In addition to avoiding incessant mocking from your colleagues this is a good way of spotting coding problems. I have recently been using [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker) in vscode. 

## Use Static Code Analysis

[cppcheck](https://cppcheck.sourceforge.io/) and [clang-tidy](https://clang.llvm.org/extra/clang-tidy/) are two more modern ones. Both will find a lot of common problems and both will create false positve results. There are ways to suppress errors in both using comments and configuration options. 

### Fix Compile Warnings

I normally turn compile warnings as high as possible. Most of the time these are good and easy to fix. Occasionally, I will need to turn off a warning. Most of the time warnings coming from external libraries are the biggest issue, but the practice of fixing all warnings is becomming more commonplace so I see this problem less than in the past. There are usually ways using #pragma to supress warning in a library header file. Sometimes I will write my own wrapper header file around problematic headers.

## Use a Source Control Tool like Git.

Regardless of the number of people working together on a project this is a good idea. Git repositories can be local to a machine, but it is best if the main repository is hosted in the cloud. GitHub is a good option.

I also use AWS. Originally this was due to GitHub not allowing free private repositories. But it is also a good option if you don't trust GitHub privacy.

When tracking down problems sometimes things break and it is good to be able to track code differences over time. If something worked at one revision and doesn't at the next then looking at what changed between revisions can help to identify the problem.

## Do Informal Code Reviews

If you are in an environment where you can get someone to look at your code an informal review of the code is useful.

The way I like to do this is to sit down with someone and using the source control diff tool walk through and explain the changes that were made. 

The person doing the review will try to spot any obvious problems and point them out.

It is more likely that as you explain your changes you will spot a problem. 

## Keep Your Toolchain Stable

The closer you get to a final version of your software the more important it becomes to reduce the number of possible sources for failure. Unless you need a fix for one of the tools you are using to finish your project it is better not to change tools. 

The flip side of this is security and bug fixes to libraries. Updating things on a regular basis is still a good idea, but make sure you can validate that an update didn't break things.