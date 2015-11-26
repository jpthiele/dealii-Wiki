# Using deal.II on native Windows

For an overview of different ways to use deal.II on Windows have a look at the corresponding [FAQ entry](https://code.google.com/p/dealii/wiki/FrequentlyAskedQuestions#Can_I_use_deal.II_on_a_Windows_platform?).

**Warning: please be aware that the following is experimental and you will likely encounter bugs in compilers and deal.II itself. Only continue if you are an experienced developer and are willing to experiment. We *strongly* recommend other options listed in the FAQ.** 

This page discusses how to compile and run deal.II on native Windows. We currently have experimental support for the gcc compiler ported by the [Cygwin64](http://www.cygwin.com/) and [MinGW-w64](http://mingw-w64.sourceforge.net/) project. Please note, that currently native Windows platforms aren't officially supported, so expect an a "told you so" as answer if something explodes in a funny way ;-)

## Cygwin

deal.II has been developed with a Unix-like environment in mind. Windows does not usually provide something like this, but it can be added to windows through the [Cygwin system](http://www.cygwin.com/). You should always use a current Cygwin distribution.

### What do I need to do to get it running?

 1. Install the 64bit installer `setup-x86_64.exe` from [1. Install the following additional packages (and its recommended dependencies):
    - cmake
    - make
    - gcc-g++
    - subversion
    - libboost-devel
    - liblapack-devel
    - libumfpack-devel
 1. fire up the Cygwin terminal and proceed with the download and installation process normally. See the [http://www.dealii.org/developer/readme.html readme](http://cygwin.com/]).

## MinGW-w64

If you do not want to use Cygwin but rather a native Windows compiler the [MinGW-w64](http://mingw-w64.sourceforge.net/) project might be of interest for you.

### Compile from source

Due to the fact that this is a highly involved procedure that requires some knowledge, only a rough outline is given:

 1. Download the online installer from [1. Install a compiler with version 4.8.`*`, target `x86_64` and thread model `TODO`
 1. Do not forget to set your path environment variable to the bin folder of your Mingw-w64 installation
 1. Download and install [http://cmake.org CMake](http://mingw-w64.sourceforge.net/].).
 1. Compile and install deal.II

## Other Windows compilers

If you want to use any of the genuine Windows compilers, such as Visual C++, or Borland C++ this is more difficult. See the entry on this topic in the FrequentlyAskedQuestions.