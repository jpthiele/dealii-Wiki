This documents the testing infrastructure and who it is administering it.

# Continuous Integration

All pull-requests on github are tested with the following machines and their status is reported on the pull request itself:

1. **indentation** (name: ``continuous-integration/travis-ci/pr``): Checks the indentation using astyle by running ``./contrib/utilities/check_indentation.sh`` and checking that there are no changes detected. Runs on https://travis-ci.org/. Maintained by: deal.II developers

2. **tjhei-alpha**: Configures deal.II with clang and MPI (see more info below) and runs a subset of the testsuite. Maintained by: https://github.com/tjhei

   Note: PRs are not run automatically but need to be approved by one of the developers by having a comment in the PR that contains the text ``/run-tests``. If this comment is found (or the PR is created by one of the developers), this PR will be continuously monitored for changes and tested as needed.

3. **tjhei-gcc-serial**: Configures deal.II with gcc without MPI (see more info below) and runs all tests of the testsuite. Maintained by: https://github.com/tjhei

   Note: PRs are not run automatically but need to be approved by one of the developers by having a comment in the PR that contains the text ``/run-tests``. If this comment is found, this PR will be continuously monitored for changes and tested as needed.

# Regression testing

In addition to testing pull requests, we also continuously have machines execute the current master branch with different configurations. The results are reported at https://cdash.kyomu.43-1.org and documentation can be found at https://www.dealii.org/developer/developers/testsuite.html#build_tests

We currently have the following machines running:

1. **tester**: The only machine in the "Regression tests" category on cdash. Many configurations. Located in Minneapolis and maintained by https://github.com/tamiko
2. **simserv04**: The only machine in the "Continuous" category on cdash. Many configurations. Located in Heidelberg and maintained by https://github.com/tamiko
3. **simserv02**: Running the Intel compiler. Maintained by https://github.com/masterleinad
4. **davyddenubuntu**: A configuration with all dependencies but CUDA enabled and built via [Spack](https://github.com/dealii/dealii/wiki/deal.II-in-Spack) with and without optimization flags on Ubuntu 16.04 with GCC 5.4.0. Maintained by https://github.com/davydden

## Details on **tjhei-alpha**

This is the current configuration:
```
#        CMAKE_BUILD_TYPE:       Debug
#        BUILD_SHARED_LIBS:      ON
#        CMAKE_INSTALL_PREFIX:   /usr/local
#        CMAKE_SOURCE_DIR:       /home/bob/source
#                                (version 9.0.0-pre, shortrev 8e44574)
#        CMAKE_BINARY_DIR:       /home/bob/build-clang
#        CMAKE_CXX_COMPILER:     Clang 3.4.0 on platform Linux x86_64
#                                /usr/bin/clang++
#
#  Configured Features (DEAL_II_ALLOW_BUNDLED = ON, DEAL_II_ALLOW_AUTODETECTION = ON):
#      ( DEAL_II_WITH_64BIT_INDICES = OFF )
#      ( DEAL_II_WITH_ADOLC = OFF )
#      ( DEAL_II_WITH_ARPACK = OFF )
#      ( DEAL_II_WITH_ASSIMP = OFF )
#        DEAL_II_WITH_BOOST set up with bundled packages
#      ( DEAL_II_WITH_BZIP2 = OFF )
#      ( DEAL_II_WITH_CUDA = OFF )
#      ( DEAL_II_WITH_CXX14 = OFF )
#      ( DEAL_II_WITH_CXX17 = OFF )
#      ( DEAL_II_WITH_GSL = OFF )
#        DEAL_II_WITH_HDF5 set up with external dependencies
#        DEAL_II_WITH_LAPACK set up with external dependencies
#        DEAL_II_WITH_METIS set up with external dependencies
#        DEAL_II_WITH_MPI set up with external dependencies
#        DEAL_II_WITH_MUPARSER set up with bundled packages
#      ( DEAL_II_WITH_NANOFLANN = OFF )
#      ( DEAL_II_WITH_NETCDF = OFF )
#        DEAL_II_WITH_OPENCASCADE set up with external dependencies
#        DEAL_II_WITH_P4EST set up with external dependencies
#        DEAL_II_WITH_PETSC set up with external dependencies
#        DEAL_II_WITH_SLEPC set up with external dependencies
#      ( DEAL_II_WITH_SUNDIALS = OFF )
#        DEAL_II_WITH_THREADS set up with bundled packages
#        DEAL_II_WITH_TRILINOS set up with external dependencies
#        DEAL_II_WITH_UMFPACK set up with bundled packages
#        DEAL_II_WITH_ZLIB set up with external dependencies
```

## Details on **tjhei-gcc-serial**

This is the current configuration:
```
#        CMAKE_BUILD_TYPE:       Debug
#        BUILD_SHARED_LIBS:      ON
#        CMAKE_INSTALL_PREFIX:   /usr/local
#        CMAKE_SOURCE_DIR:       /home/bob/source
#                                (version 9.0.0-pre, shortrev 8e44574)
#        CMAKE_BINARY_DIR:       /home/bob/build-gcc
#        CMAKE_CXX_COMPILER:     GNU 4.8.4 on platform Linux x86_64
#                                /usr/bin/c++
#
#  Configured Features (DEAL_II_ALLOW_BUNDLED = ON, DEAL_II_ALLOW_AUTODETECTION = ON):
#      ( DEAL_II_WITH_64BIT_INDICES = OFF )
#      ( DEAL_II_WITH_ADOLC = OFF )
#      ( DEAL_II_WITH_ARPACK = OFF )
#      ( DEAL_II_WITH_ASSIMP = OFF )
#        DEAL_II_WITH_BOOST set up with bundled packages
#      ( DEAL_II_WITH_BZIP2 = OFF )
#      ( DEAL_II_WITH_CUDA = OFF )
#      ( DEAL_II_WITH_CXX14 = OFF )
#      ( DEAL_II_WITH_CXX17 = OFF )
#      ( DEAL_II_WITH_GSL = OFF )
#      ( DEAL_II_WITH_HDF5 = OFF )
#        DEAL_II_WITH_LAPACK set up with external dependencies
#        DEAL_II_WITH_METIS set up with external dependencies
#      ( DEAL_II_WITH_MPI = OFF )
#        DEAL_II_WITH_MUPARSER set up with bundled packages
#      ( DEAL_II_WITH_NANOFLANN = OFF )
#      ( DEAL_II_WITH_NETCDF = OFF )
#        DEAL_II_WITH_OPENCASCADE set up with external dependencies
#      ( DEAL_II_WITH_P4EST = OFF )
#      ( DEAL_II_WITH_PETSC = OFF )
#      ( DEAL_II_WITH_SLEPC = OFF )
#      ( DEAL_II_WITH_SUNDIALS = OFF )
#        DEAL_II_WITH_THREADS set up with bundled packages
#      ( DEAL_II_WITH_TRILINOS = OFF )
#        DEAL_II_WITH_UMFPACK set up with bundled packages
#        DEAL_II_WITH_ZLIB set up with external dependencies
```