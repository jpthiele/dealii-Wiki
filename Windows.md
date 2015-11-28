# Using deal.II on native Windows

For an overview of different ways to use deal.II on Windows have a look at the corresponding [FAQ entry](https://code.google.com/p/dealii/wiki/FrequentlyAskedQuestions#Can_I_use_deal.II_on_a_Windows_platform?).

**Warning: please be aware that the following is experimental and you will likely encounter bugs in compilers and deal.II itself. Only continue if you are an experienced developer and are willing to experiment. We *strongly* recommend other options listed in the FAQ.** 

## Visual Studio

There has been some progress with newer Visual Studio C++ compilers, but this is still work in progress. Please see the following threads for more details:
- https://github.com/dealii/dealii/issues/1921

## Cygwin / MingGW

Cygwin and forks such as MinGW and MinGW-64 are unsupported due to multiple unresolved miscompilation issues.

## Other Windows compilers

We haven't had much success with any other compiler on Windows (Intel, Borland, ...).