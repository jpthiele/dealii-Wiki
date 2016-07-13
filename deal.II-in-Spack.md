# Using deal.II on Mac OS X and Linux via Spack

The deal.II suite is also available on Spack (https://github.com/LLNL/spack).

## Download and configure Spack

Add the following to `~/.bash_profile` (or equivalent)
```
SPACK_ROOT=/path/to/spack
export PATH=$SPACK_ROOT/bin:$PATH
```
`SPACK_ROOT` is the destination where you want Spack to be installed (i.e. `/Users/john/spack`).

Now clone Spack [1]
```
git clone https://github.com/llnl/spack.git $SPACK_ROOT
```

Spack provides some integration with Environment Modules and Dotkit to make it easier to use the packages it installs. For a full description, read http://software.llnl.gov/spack/basic_usage.html#installing-environment-modules

To add the support for Environment Modules, first run
```
spack install environment-modules
```
and then add to `~/.bash_profile` (or equivalent)
```
MODULES_HOME=`spack location -i environment-modules`
source ${MODULES_HOME}/Modules/init/bash
. $SPACK_ROOT/share/spack/setup-env.sh
```

Spack is flexible to use both self-compiled and system provided packages. One can also specify which packages should be used for `mpi`, `blas/lapack` and alike. For more details, see [Spack documentation](http://software.llnl.gov/spack/features.html). Below is a self-explanatory example of a configuration file `~/.spack/package.yaml` to use `openblas`, `openmpi` and system provided `python`:
```
packages:
  all:
    compiler: [gcc,clang]
    providers:
      mpi: [openmpi]
      blas: [openblas]
      lapack: [openblas]
  python:
    version: [2.7.11]
    paths:
      python@2.7.11: /usr
```

## Install and use deal.II
Make sure C/C++/Fortran compilers are in path (if that's not the case, see below),
and install the complete deal.II suite
```
spack install dealii
```
In order to use `deal.II` first do 
```
spack load dealii
spack load cmake
```
Now `DEAL_II_DIR` environment variable should be set appropriately and `cmake` executable will be available in path.

## Install GCC
OSX by default does not provide any Fortran compiler.
One can get it from GCC

```
spack install gcc
```

Now load `gcc` and let Spack find the newly installed compilers:
```
spack load gcc
spack compiler find
```
Finally you can install deal.II with `gcc`
```
spack install dealii%gcc
```

## Mixing GCC and Clang on OSX

At the time of writing this page, Spack does not provide a native way to mix C/C++ and Fortran compilers from different families. However, that can be done by manually changing few lines of `python` code as described below:

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

(ii) Create a symlink inside clang environement
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

Spack is complicated and flexible package manager primarily aimed at High-Perfomance-Computing.
Below are some examples of using Spack to build and develop deal.II:

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

### Different versions coexisting:
One can easily have slightly different versions of deal.II side-by-side, e.g. to compile development version of deal.II with complex-valued PETSc and `gcc` compiler run
```
spack install dealii@develop+mpi+petsc%gcc ^petsc+complex~hypre
```
The good thing is that if you already have deal.ii built with real-valued petsc, then only `petsc`, `slepc` and `deal.ii` itself will be rebuild. Everything else (`trilinos`, `mumps`, `metis`, etc) will be reused.

One can use `environement-modules` (see above) to automatically set `DEAL_II_DIR` to the complex version: 
```
spack load dealii%gcc^petsc+complex
```

### Filesystem Views:
If you prefer to haave the whole dealii suite (and possible something else) symlinked into a single path (like `/usr/local`), one can use [Filesystem Views](http://software.llnl.gov/spack/basic_usage.html#filesystem-views):
```
spack view -v symlink dealii_suite dealii@develop
```

### Check before build:
It is often convenient to check which version of packages, compilers, variants etc will be used before actually starting installation. That can be done by building the conretized spec via, for example, 
```
spack spec dealii@develop+mpi+petsc%gcc ^petsc+complex~hypre
```

### Develop using Spack
There are several ways to use Spack while contributing patches to the deal.II. The simplest is to create a Filesystem vew (see above) for an already installed library and then compile patched version of deal.II manually by providing path to the view for each dependency. For example, if `dealii` view is located in `/Users/davydden/spack/_dealii_suite`, then one could configure deal.II by
```
cmake -DCMAKE_FIND_FRAMEWORK=LAST -DCMAKE_INSTALL_RPATH_USE_LINK_PATH=FALSE -DCMAKE_INSTALL_RPATH=/Users/davydden/spack/_dealii_suite -DCMAKE_BUILD_TYPE=DebugRelease -DDEAL_II_COMPONENT_EXAMPLES=ON -DDEAL_II_WITH_THREADS:BOOL=ON -DBOOST_DIR=/Users/davydden/spack/_dealii_suite -DBZIP2_DIR=/Users/davydden/spack/_dealii_suite -DLAPACK_FOUND=true -DLAPACK_INCLUDE_DIRS="/Users/davydden/spack/_dealii_suite/include;/Users/davydden/spack/_dealii_suite/include"   -DLAPACK_LIBRARIES="/Users/davydden/spack/_dealii_suite/lib/libopenblas.dylib" -DMUPARSER_DIR=/Users/davydden/spack/_dealii_suite -DUMFPACK_DIR=/Users/davydden/spack/_dealii_suite -DTBB_DIR=/Users/davydden/spack/_dealii_suite -DZLIB_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_MPI:BOOL=ON -DCMAKE_C_COMPILER=/Users/davydden/spack/_dealii_suite/bin/mpicc -DCMAKE_CXX_COMPILER=/Users/davydden/spack/_dealii_suite/bin/mpic++ -DCMAKE_Fortran_COMPILER=/Users/davydden/spack/_dealii_suite/bin/mpif90 -DGSL_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_GSL:BOOL=ON -DHDF5_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_HDF5:BOOL=ON -DP4EST_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_P4EST:BOOL=ON -DPETSC_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_PETSC:BOOL=ON -DSLEPC_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_SLEPC:BOOL=ON -DTRILINOS_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_TRILINOS:BOOL=ON -DMETIS_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_METIS:BOOL=ON -DDEAL_II_COMPONENT_DOCUMENTATION=OFF -DARPACK_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_ARPACK=ON -DDEAL_II_ARPACK_WITH_PARPACK=ON -DNETCDF_FOUND=true -DNETCDF_LIBRARIES="/Users/davydden/spack/_dealii_suite/lib/libnetcdf_c++.dylib;/Users/davydden/spack/_dealii_suite/lib/libnetcdf.dylib" -DNETCDF_INCLUDE_DIRS=/Users/davydden/spack/_dealii_suite/include -DOPENCASCADE_DIR=/Users/davydden/spack/_dealii_suite -DDEAL_II_WITH_OPENCASCADE=ON ../
```


------
[1] The installation instructions were last time tested on Ubuntu 14.04 LTS with gcc 6.1 and Spack commit  https://github.com/LLNL/spack/commit/3ab56a188e83054420d9004be1c6d07276c91375