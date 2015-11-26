# Using deal.II on native Windows

For an overview of different ways to use deal.II on Windows have a look at the corresponding [FAQ entry](https://code.google.com/p/dealii/wiki/FrequentlyAskedQuestions#Can_I_use_deal.II_on_a_Windows_platform?).

**Warning: please be aware that the following is experimental and you will likely encounter bugs in compilers and deal.II itself. Only continue if you are an experienced developer and are willing to experiment. We *strongly* recommend other options listed in the FAQ.** 

This page discusses how to compile and run deal.II on native Windows. We currently have experimental support for the gcc compiler ported by the [Cygwin64](http://www.cygwin.com/) and [MinGW-w64](http://mingw-w64.sourceforge.net/) project. Please note, that currently native Windows platforms aren't officially supported, so expect an a "told you so" as answer if something explodes in a funny way ;-)

## Visual Studio

There has been some progress with newer Visual Studio C++ compilers, but this is still work in progress. Please see the following threads for more details:
- https://github.com/dealii/dealii/issues/1681

## Cygwin / MingGW

Cygwin and forks such as MinGW and MinGW-64 are unsupported due to multiple unresolved miscompilation issues.

## Other Windows compilers

We haven't had much success with any other compiler on Windows (Intel, Borland, ...).