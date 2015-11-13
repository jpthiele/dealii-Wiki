This page summarises known issues when compiling `deal.ii` on different platforms and compilers.

# Compilers:
## Intel

1. `boost::signals2::detail::call_with_tuple_args<R>::operator`.
   It is known that Intel compilers with Intel MPI may lead to a cryptic error when compiling `deal.ii` with either bundled or external `boost` library. The workaround is to use: `Intel 15` and `boost 1.59`. The issue does not seem to appear when Intel compilers are combined with `Open-MPI`.

2. `DEAL_II_HAVE_CXX11_TYPE_TRAITS`. To have `C++11` support with Intel compilers one has to have recent system gcc as ["if you have gcc version 4.6 on your system, icc behaves like gcc 4.6, with the compatible features and behaviors"](https://software.intel.com/en-us/node/522750). If system's default `gcc` is too old, one may need to load a newer one (e.g. `module load gcc/4.9.2` on `CentOS`).

3. `catastrophic error: #error directive: "SEEK_SET is #defined but must not be for the C++ binding of MPI.` The issue appears when compiling Trilinos. The workaround is to add `-DMPICH_IGNORE_CXX_SEEK` to `CMAKE_CXX_FLAGS. For further details see [this known issues of intel compilers](https://software.intel.com/en-us/articles/intel-mpi-library-for-linux-running-list-of-known-issues#A3).

## GNU


# Platforms:
## Linux

## Windows

## OS-X