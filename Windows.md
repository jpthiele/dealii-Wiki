# Using deal.II on native Windows

For an overview of different ways to use deal.II on Windows have a look at the corresponding [FAQ entry](https://github.com/dealii/dealii/wiki/Frequently-Asked-Questions#can-i-use-dealii-on-a-windows-platform).

**Warning: please be aware that the following is experimental and you will likely encounter bugs in compilers and deal.II itself. Only continue if you are willing to experiment.** 

## Visual Studio

Since deal.II 8.4.0 we have experimental support for the newer Visual Studio C++ compilers (2013 and 2015), but this is still work in progress. Please see the following threads for more details:
- https://github.com/dealii/dealii/issues/1921

Installation instructions:

1. Download and install Visual Studio 2015 or 2017: https://www.visualstudio.com/vs/ and make sure you select the C++ compiler
2. Install cmake from https://cmake.org/download/ (pick the windows installer)
3. Extract deal.II to a folder, for example c:\dealii (or clone the git development version)
4. Configure using cmake by opening the 64bit command line shortcut and run:

    ```
    set PreferredToolArchitecture=x64
    cd c:\dealii
    mkdir build
    cd c:\dealii\build
    cmake -G "Visual Studio 15 2017 Win64" ..
    ```
    Note: Setting the tool architecture to 64 bit works around problems of the compiler or linker running out of memory and leads to much quicker compile times.
    Note: Use generator ``"Visual Studio 15 2017 Win64"`` for Visual Studio 2017 and ``"Visual Studio 14 2015 Win64"`` for Visual Studio 2015.

6. Compile and install the library by opening ``deal.II.sln`` in c:\dealii\build, pick the install target and compile. Note: you need to either compile in the same terminal as above (using ``cmake --build .``) or open ``devenv.exe`` from the same terminal, to use the 64 bit tool architecture.

7. in cmd go to one of the examples in c:\dealii\examples\step-xy:

    ```
    cmake -D DEAL_II_DIR=c:\dealii\build -G "Visual Studio 15 2017 Win64" .
    ```

8. Open the newly created solution (step-xy.sln) in that directory and compile/run/debug.

## Running build tests on Windows:

Install git and mingw (for perl etc). Then create a .bat file:
```
git pull origin master
rmdir /Q /S buildtest15
mkdir buildtest15
cd buildtest15
ctest -C Debug -DMAKEOPTS="/m:1" -DCTEST_CMAKE_GENERATOR="Visual Studio 14 2015" -S ../tests/run_buildtest.cmake -V
cd ..
```

## Cygwin / MingGW

Cygwin and forks such as MinGW and MinGW-64 are unsupported due to multiple unresolved miscompilation issues.

## Other Windows compilers

We haven't had much success with any other compiler on Windows (Intel, Borland, ...).