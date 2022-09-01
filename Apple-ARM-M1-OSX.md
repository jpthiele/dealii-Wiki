This document describes installation instructions for deal.II for Apple ARM devices (M1 and similar, including the mac mini and macbook pro).

## 1. Set up system

Open a native terminal and check that typing ``machine`` returns ``arm64e``, if not, you are running under Rosetta emulation. 

Now type ``clang`` to trigger the installation of the command line tools.

Then install homebrew as detailed on https://brew.sh/

## 2. Install compiler and MPI

``brew install cmake open-mpi gcc@11``

## A Note about compilers choice

You now have the following compilers:
1. clang / clang++ (in /usr/bin): this is the system clang compiler
2. gcc / g++ (in /usr/bin): these are symlinks to clang
3. gcc-11 / g++-11 (/opt/homebrew/bin): installed by homebrew (version 11 when I wrote this)

You can compile with gcc 11 but this is not recommended (see the bottom of this document for more info). By default, mpicxx will use ``gcc``, which is actually ``clang``.

## 3. Install deal.II and dependencies

```
git clone https://github.com/dealii/candi
cd candi
./candi.sh -j 8 --packages="hdf5 p4est trilinos dealii"
```
Note: If you need additional dependencies, try them at your own risk. You might need to switch to the ``master`` candi branch (at least at the time this guide was written).


## Why not gcc?

We strongly recommend using clang as gcc is building generic ARM executables that are not optimized for the M1 and you might run into linker errors depending on how many dependencies you have enabled. The errors look similar to
```
ld: b(l) ARM64 branch out of range (135252872 max is +/-128MB): from __ZN5boost7archive17archive_exceptionC2ENS1_14exception_codeEPKcS4_ (0x00019B08) to ___assert_rtn@0x00000000 (0x08116BA4) in '__ZN5boost7archive17archive_exceptionC2ENS1_14exception_codeEPKcS4_' from ../bundled/boost-1.70.0/libs/serialization/src/CMakeFiles/obj_boost_serialization_debug.dir/archive_exception.cpp.o for architecture arm64
```
If you want to try, enable gcc like this before setting up your system with candi:
```
export OMPI_FC=gfortran-11;export OMPI_CC=gcc-11;export OMPI_CXX=g++-11
```
