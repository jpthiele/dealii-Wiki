# Using deal.II on Mac OS X and Linux via Spack

The deal.II suite is also available on Spack (https://github.com/LLNL/spack) -- a flexible package manager developed with High-Performance-Computing in mind. It is intended to let you build for many combinations of compiler, architectures, dependency libraries, and build configurations, all with a friendly, intuitive user interface.

## Quick installation

Add the following to `~/.bash_profile` (or equivalent)
```
SPACK_ROOT=/path/to/spack
export PATH=$SPACK_ROOT/bin:$PATH
```
`SPACK_ROOT` is the destination where you want Spack to be installed (i.e. `/Users/john/spack`).

Now clone Spack
```
git clone https://github.com/llnl/spack.git $SPACK_ROOT
```
**Make sure C/C++/Fortran compilers are in path** (if that's not the case, see [below](#install-gcc)),
and install the complete deal.II suite
```
spack install dealii
```
**DONE**! No extra (preliminary) configuration steps are needed on most Linux distributions. You may jump ahead and read [best practices using spack](#best-practices-using-spack).

For macOS read further.

## Environment Modules
Spack provides some integration with Environment Modules and Dotkit to make it easier to use the packages it installs. For a full description, read http://software.llnl.gov/spack/basic_usage.html#installing-environment-modules

To add the support for Environment Modules run
```
spack install environment-modules
```
and then add to `~/.bash_profile` (or equivalent)
```
MODULES_HOME=`spack location -i environment-modules`
source ${MODULES_HOME}/Modules/init/bash
. $SPACK_ROOT/share/spack/setup-env.sh
```

If you install `deal.II` before setting up environment modules,
the module files have to be regenerated
```
spack module refresh
```

Then run
```
spack load dealii
spack load cmake
```
Now `DEAL_II_DIR` environment variable should be set appropriately and `cmake` executable will be available in path.

## System provided packages
Spack is flexible to use both self-compiled and system provided packages. 
In most cases this is desirable for `MPI`, which is already installed on computational clusters. To configure external packages you need to edit `~/.spack/packages.yaml`. For `openmpi` this could be
```
packages:
  openmpi:
    version: [1.8.8]
    paths:
      openmpi@1.8.8%gcc@6.2.0: /opt/openmpi-1.8.8
    buildable: False
```
In order to make sure that we use to build packages `1.8.8` version of `openmpi` and not the most recent one (i.e. `2.0.2`), we specified conretization preferences with `version: [1.8.8]`.

One can also specify which packages should be used to provide `mpi`, `blas/lapack` , preferred compilers and preferred variants:
```
packages:
  all:
    compiler: [gcc,clang]
    providers:
      mpi: [openmpi]
      blas: [openblas]
      lapack: [openblas]
    boost:
      variants: +python
```

For more elaborated discussion, see [Spack documentation](http://spack.readthedocs.io/en/latest/configuration.html).

## Installing GCC
If your system does not provide any Fortran compiler or you want to have the most recent `gcc`,
you can install it by
```
spack install gcc
```

Assuming that you configured [Environment Modules](#environment-modules), load `gcc` and let Spack find the newly installed compilers:
```
spack load gcc
spack compiler find
```
Now you can install deal.II with `gcc`
```
spack install dealii%gcc
```

## Mixing GCC and Clang on OSX

At the time of writing this page, Spack does not provide a native way to mix C/C++ and Fortran compilers from different families (e.g. `gcc` and `clang`). However, that can be done by manually changing few lines of `python` code as described below:

(i) Edit `~/.spack/compilers.yaml` to provide path to gfortran compiler within the `clang@x.y.z-apple` entry:
```
compilers:
- compiler:
    modules: []
    operating_system: elcapitan
    paths:
      cc: /usr/bin/clang
      cxx: /usr/bin/clang++
      f77: /path/to/bin/gfortran
      fc: /path/to/bin/gfortran
    spec: clang@7.3.0-apple
```

(ii) Create a symlink inside clang environment
```
cd $SPACK_ROOT/lib/spack/env/clang
ln -s ../cc gfortran
```

(iii) Finally, apply the following patch to `lib/spack/spack/compilers/clang.py`
```
diff --git a/lib/spack/spack/compilers/clang.py b/lib/spack/spack/compilers/clang.py
index e406d86..cf8fd01 100644
--- a/lib/spack/spack/compilers/clang.py
+++ b/lib/spack/spack/compilers/clang.py
@@ -35,17 +35,17 @@ class Clang(Compiler):
     cxx_names = ['clang++']

     # Subclasses use possible names of Fortran 77 compiler
-    f77_names = []
+    f77_names = ['gfortran']

     # Subclasses use possible names of Fortran 90 compiler
-    fc_names = []
+    fc_names = ['gfortran']

     # Named wrapper links within spack.build_env_path
     link_paths = { 'cc'  : 'clang/clang',
                    'cxx' : 'clang/clang++',
                    # Use default wrappers for fortran, in case provided in compilers.yaml
-                   'f77' : 'f77',
-                   'fc'  : 'f90' }
+                   'f77' : 'clang/gfortran',
+                   'fc'  : 'clang/gfortran' }

     @classmethod
     def default_version(self, comp):
```

Now you can install deal.II with clang+gfortran
```
spack install dealii%clang
```

## Best practices using Spack:

Spack is complicated and flexible package manager primarily aimed at High-Performance-Computing.
Below are some examples of using Spack to build and develop deal.II:

### Info:
If you are not sure which options a given package has, simply run
```
spack info <package>
```
The output will contain available versions, variants, dependencies and description:
```
$ spack info dealii
Package:    dealii
Homepage:   https://www.dealii.org

Safe versions:
    develop    [git] https://github.com/dealii/dealii.git
    8.4.1      https://github.com/dealii/dealii/releases/download/v8.4.1/dealii-8.4.1.tar.gz
    8.4.0      https://github.com/dealii/dealii/releases/download/v8.4.0/dealii-8.4.0.tar.gz
    8.3.0      https://github.com/dealii/dealii/releases/download/v8.3.0/dealii-8.3.0.tar.gz
    8.2.1      https://github.com/dealii/dealii/releases/download/v8.2.1/dealii-8.2.1.tar.gz
    8.1.0      https://github.com/dealii/dealii/releases/download/v8.1.0/dealii-8.1.0.tar.gz

Variants:
    Name        Default   Description

    arpack      on        Compile with Arpack and PArpack (only with MPI)
    doc         off       Compile with documentation
    gsl         on        Compile with GSL
    hdf5        on        Compile with HDF5 (only with MPI)
    metis       on        Compile with Metis
    mpi         on        Compile with MPI
    netcdf      on        Compile with Netcdf (only with MPI)
    oce         on        Compile with OCE
    p4est       on        Compile with P4est (only with MPI)
    petsc       on        Compile with Petsc (only with MPI)
    slepc       on        Compile with Slepc (only with Petsc and MPI)
    trilinos    on        Compile with Trilinos (only with MPI)

Build Dependencies:
    zlib  blas  graphviz  netcdf  arpack-ng  bzip2  cmake  lapack  oce  astyle  boost  trilinos  muparser  p4est  mpi  suite-sparse  tbb  doxygen  hdf5  slepc  numdiff  metis  petsc  netcdf-cxx  gsl

Link Dependencies:
    zlib  blas  graphviz  netcdf  arpack-ng  bzip2  lapack  oce  astyle  boost  trilinos  muparser  p4est  mpi  suite-sparse  tbb  doxygen  hdf5  slepc  numdiff  metis  petsc  netcdf-cxx  gsl

Run Dependencies:
    None

Virtual packages:
    None

Description:
    C++ software library providing well-documented tools to build finite
    element codes for a broad variety of PDEs.
```

### Extra options:
One can specify extra options for packages in the deal.II suite. For example if you want to have boost with `python` module, this can be done by
```
spack install dealii@develop+mpi ^boost+python
```

If you want to specify blas/lapack/mpi implementations, this can be done similarly
```
spack install dealii@develop+mpi ^mpich
```
To check which packages implement `mpi` run
```
spack providers mpi
```
One can also specify which Blas/Lapack implementation to use. For example to build deal.II suite with `atlas` run
```
spack install dealii ^atlas
```

### Different versions coexisting:
One can easily have slightly different versions of deal.II side-by-side, e.g. to compile development version of deal.II with complex-valued PETSc and `gcc` compiler run
```
spack install dealii@develop+mpi+petsc%gcc ^petsc+complex~hypre
```
The good thing is that if you already have deal.ii built with real-valued petsc, then only `petsc`, `slepc` and `deal.ii` itself will be rebuild. Everything else (`trilinos`, `mumps`, `metis`, etc) will be reused.

One can use `environment-modules` (see above) to automatically set `DEAL_II_DIR` to the complex version: 
```
spack load dealii%gcc^petsc+complex
```

### Filesystem Views:
If you prefer to have the whole dealii suite (and possible something else) symlinked into a single path (like `/usr/local`), one can use [Filesystem Views](http://software.llnl.gov/spack/basic_usage.html#filesystem-views):
```
spack view -v symlink dealii_suite dealii@develop
```
You can also add to this view other packages, i.e.
```
spack view -v symlink dealii_suite the_silver_searcher
```


### Check before build:
It is often convenient to check which version of packages, compilers, variants etc will be used before actually starting installation. That can be done by examining the concretized spec via `spack spec` command, e.g. 
```
spack spec dealii@develop+mpi+petsc%gcc ^petsc+complex~hypre
```

### Develop using Spack
There are several ways to use Spack while contributing patches to the deal.II. The simplest is to create a [Filesystem view](#filesystem-views) for an already installed library and then compile patched version of deal.II manually by providing path to the view for each dependency. For example on macOS with `openblas`:
```
export DEAL_II_VIEW=/Users/davydden/spack/_dealii_suite
cmake -DCMAKE_FIND_FRAMEWORK=LAST -DCMAKE_INSTALL_RPATH_USE_LINK_PATH=FALSE -DCMAKE_INSTALL_RPATH=${DEAL_II_VIEW} -DCMAKE_BUILD_TYPE=DebugRelease -DDEAL_II_COMPONENT_EXAMPLES=ON -DDEAL_II_WITH_THREADS:BOOL=ON -DBOOST_DIR=${DEAL_II_VIEW} -DBZIP2_DIR=${DEAL_II_VIEW} -DLAPACK_FOUND=true -DLAPACK_INCLUDE_DIRS=${DEAL_II_VIEW}/include -DLAPACK_LIBRARIES=${DEAL_II_VIEW}/lib/libopenblas.dylib -DMUPARSER_DIR=${DEAL_II_VIEW} -DUMFPACK_DIR=${DEAL_II_VIEW} -DTBB_DIR=${DEAL_II_VIEW} -DZLIB_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_MPI:BOOL=ON -DCMAKE_C_COMPILER=${DEAL_II_VIEW}/bin/mpicc -DCMAKE_CXX_COMPILER=${DEAL_II_VIEW}/bin/mpic++ -DCMAKE_Fortran_COMPILER=${DEAL_II_VIEW}/bin/mpif90 -DGSL_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_GSL:BOOL=ON -DHDF5_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_HDF5:BOOL=ON -DP4EST_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_P4EST:BOOL=ON -DPETSC_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_PETSC:BOOL=ON -DSLEPC_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_SLEPC:BOOL=ON -DTRILINOS_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_TRILINOS:BOOL=ON -DMETIS_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_METIS:BOOL=ON -DDEAL_II_COMPONENT_DOCUMENTATION=OFF -DARPACK_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_ARPACK=ON -DDEAL_II_ARPACK_WITH_PARPACK=ON -DNETCDF_DIR=${DEAL_II_VIEW} -DOPENCASCADE_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_OPENCASCADE=ON ../
```

### MKL and Licensed software
Spack supports installation of [licensed software](http://software.llnl.gov/spack/packaging_guide.html#licensed-software). For example in order to install MKL on Linux:

1. add the `license.lic` file to `${SPACK_ROOT}/etc/spack/licenses/intel/`.
2. manually download Intel MKL archive `l_mkl_11.3.2.181.tgz`
3. `cd` to the folder with archive and run `spack install mkl@11.3.2.181`.

One can then run `spack install dealii ^mkl@11.3.2.181`.


### Freeze package versions
Currently Spack does not try to re-use already installed packages. On another hand, by default
the most recent version of a package will be installed. When updating deal.II build (for example to use the new version of `trilinos`), the combination of the two factors may lead to recompilation of many other packages used in the deal.II suite when one of the main build dependency like `cmake` has a new version.

To circumvent the problem, the user can specify preferred versions of packages in `~/.spack/packages.yaml`:
```
packages:
  cmake:
    version: [3.6.1]
  curl:
    version: [7.50.3]
  openssl:
    version: [1.0.2j]
  python:
    version: [2.7.12]
```
This settings will be taken into account during conretization process and thus will help to avoid rebuilding most of the deal.II suite when, for example, `openssl` is updated to the new version.