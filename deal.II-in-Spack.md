# Using deal.II on Mac OS X and Linux via Spack

The deal.II suite is also available on Spack (https://github.com/LLNL/spack) -- a flexible package manager developed with High-Performance-Computing in mind. It is intended to let you build for many combinations of compiler, architectures, dependency libraries, and build configurations, all with a friendly, intuitive user interface.

For a short overview of features and motivation, we recommend this short presentation https://tgamblin.github.io/files/Gamblin-Spack-SC15-Talk.pdf

## Quick installation on the desktop

Add the following to `~/.bashrc` (or equivalent)
```
export SPACK_ROOT=/path/to/spack
PATH="$SPACK_ROOT/bin:$PATH"
```
`SPACK_ROOT` is the destination where you want Spack to be installed (i.e. `$HOME/spack`).

Now clone Spack
```
cd $SPACK_ROOT
git clone https://github.com/llnl/spack.git .
git checkout develop
# the following commit was tested on:
# - Ubuntu16.04+gcc5.4.0 PC
# - centos7+gcc4.8.5 cluster
git reset --hard 1124bdc99ee84c26201c40536d9b04dac74d7f6a
```

**Make sure C/C++/Fortran compilers are in path** (on Ubuntu you need to `sudo apt-get install gfortran`, on macOS you can compile `gcc` with spack, see [below](#install-gcc)), and you have **curl** (`sudo apt-get install curl`) to download packages. Then install the complete deal.II suite
```
spack install dealii
```
**DONE**! No extra (preliminary) configuration steps are needed on most Linux distributions. **IMPORTANT:** If you compile deal.II on a cluster, see the next section on how to use externally provided MPI implementation instead.

Before configuring your project you need to set `DEAL_II_DIR` by 
```
export DEAL_II_DIR=$(spack location -i dealii)
```
You may jump ahead and read [best practices using spack](#best-practices-using-spack). Also a good starting point is [Getting Started Guide](http://spack.readthedocs.io/en/latest/getting_started.html).


## Installation example on a Centos7 cluster (Emmy cluster of RRZE, Erlangen, Germany)
In order to use Spack on a cluster, there are two options: (1) you are a sysadmin and you know hardware details and can use Spack to properly configure and build MPI providers (e.g. `openmpi`); (2) you are a user and you need to make Spack use MPI provided on your cluster. Below we consider the latter case.

Here is a brief step-by-step instruction to install deal.II on [Emmy cluster](https://www.rrze.fau.de/dienste/arbeiten-rechnen/hpc/systeme/emmy-cluster.shtml#access) of RRZE, Erlangen, Germany:

(1) Download spack
```
module load git
mkdir $WOODYHOME/spack
cd $WOODYHOME/spack
git clone https://github.com/llnl/spack.git $WOODYHOME/spack
git reset --hard 1124bdc99ee84c26201c40536d9b04dac74d7f6a
export PATH=$WOODYHOME/spack/bin:$PATH
```
(2) Load `openmpi` and let Spack find GCC compiler which is also loaded as a dependency:
```
module load openmpi/2.0.1-gcc
spack compiler find
```
(3) Add `openmpi` as an external package, along with `python` and a few other self explanatory setting for `deal.ii`. That is done by adding the following to `~/.spack/linux/packages.yaml`
```
packages:
  all:
    compiler: [gcc]
    providers:
      mpi: [openmpi]
      blas: [openblas]
      lapack: [openblas]
  python:
    version: [2.7.12]
    paths:
      python@2.7.12: /usr/
    buildable: False
  openmpi:
    version: [2.0.1]
    paths:
      openmpi@2.0.1%gcc@4.8.5: /apps/OpenMPI/2.0.1-gcc/
    buildable: False
  dealii:
    version: [develop]
    variants: +optflags~python
```
Those paths are the location where external packages can be found (i.e. `<prefix>` instead of `<prefix>/bin` or `<prefix>/lib`).
(4) Now install deal.II:  `spack install dealii`.

Note that we specifically build deal.II without `python` wrappers. Otherwise deal.II would be linked against system provided `python` which itself may be linked against system provided `zlib`. As a result we may have a mixture of Spack's build `zlib` and system provided `zlib`, which is certainly not what we want.


## Environment Modules
Spack provides some integration with Environment Modules and Dotkit to make it easier to use the packages it installs. For a full description, read http://spack.readthedocs.io/en/latest/getting_started.html#environment-modules

To add the support for Environment Modules run
```
spack install environment-modules
```
Get the path to the prefix of `environment-modules` by:
```
spack location --install-dir environment-modules
```
and then add to `~/.bashrc` (or equivalent)
```
MODULES_HOME=/path/to/environment-modules
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
Now `DEAL_II_DIR` environment variable should be set appropriately and `cmake` executable will be available in path. Keep in mind that `spack load dealii` will also set `LD_LIBRARY_PATH` accordingly, this may or may not be what you need. An alternative is to use `export DEAL_II_DIR=$(spack location -i dealii)`.

## System provided packages
Spack is flexible to use both self-compiled and system provided packages. 
In most cases this is desirable for `MPI`, which is already installed on computational clusters. To configure external packages you need to edit `~/.spack/linux/packages.yaml`. For `openmpi` this could be
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

For more elaborated discussion, see [Configuration Files in Spack](http://spack.readthedocs.io/en/latest/configuration.html).

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

If you are on the mac, read the following instructions on [Mixed Toolchains](http://spack.readthedocs.io/en/latest/getting_started.html#mixed-toolchains).

On following these instructions, you will be able to install deal.II with clang+gfortran
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
CMakePackage:    dealii
Homepage:        https://www.dealii.org

Safe versions:
    develop    [git] https://github.com/dealii/dealii.git
    8.4.2      https://github.com/dealii/dealii/releases/download/v8.4.2/dealii-8.4.2.tar.gz
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
    int64       off       Compile with 64 bit indices support
    metis       on        Compile with Metis
    mpi         on        Compile with MPI
    netcdf      on        Compile with Netcdf (only with MPI)
    oce         on        Compile with OCE
    p4est       on        Compile with P4est (only with MPI)
    petsc       on        Compile with Petsc (only with MPI)
    python      on        Compile with Python bindings
    slepc       on        Compile with Slepc (only with Petsc and MPI)
    trilinos    on        Compile with Trilinos (only with MPI)

Installation Phases:
    cmake    build    install

Build Dependencies:
    arpack-ng  blas  boost  bzip2  cmake  doxygen  graphviz  gsl  hdf5  lapack  metis  mpi  muparser  netcdf  netcdf-cxx  oce  p4est  petsc  python  slepc  suite-sparse  tbb  trilinos  zlib

Link Dependencies:
    arpack-ng  blas  boost  bzip2  doxygen  graphviz  gsl  hdf5  lapack  metis  mpi  muparser  netcdf  netcdf-cxx  oce  p4est  petsc  python  slepc  suite-sparse  tbb  trilinos  zlib

Run Dependencies:
    None

Virtual Packages:
    None

Description:
    C++ software library providing well-documented tools to build finite
    element codes for a broad variety of PDEs.
```

A lot of `spack` commands have help, for example
```
$spack install -h
usage: spack install [-h] [--only {package,dependencies}] [-j JOBS]
                     [--keep-prefix] [--keep-stage] [-n] [-v] [--fake]
                     [--clean | --dirty] [--run-tests] [--log-format {junit}]
                     [--log-file LOG_FILE]
                     ...

positional arguments:
  package               spec of the package to install

optional arguments:
  -h, --help            show this help message and exit
  --only {package,dependencies}
                        select the mode of installation. the default is to
                        install the package along with all its dependencies.
                        alternatively one can decide to install only the
                        package or only the dependencies
  -j JOBS, --jobs JOBS  explicitly set number of make jobs. default is #cpus
  --keep-prefix         don't remove the install prefix if installation fails
  --keep-stage          don't remove the build stage if installation succeeds
  -n, --no-checksum     do not check packages against checksum
  -v, --verbose         display verbose build output while installing
  --fake                fake install. just remove prefix and create a fake
                        file
  --clean               clean environment before installing package
  --dirty               do NOT clean environment before installing
  --run-tests           run package level tests during installation
  --log-format {junit}  format to be used for log files
  --log-file LOG_FILE   filename for the log file. if not passed a default
                        will be used
```

### Extra options:
One can specify extra options for packages in the deal.II suite. For example if you want to have boost with `python` module, this can be done by
```
spack install dealii@develop+mpi~python ^boost+python
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

### Compiler flags
You can specify compiler flags on the command line as
```
spack install dealii cppflags="-march=native -O3"
```
Note that these flags will be inherited by dependencies such as `petsc`, `trilinos`, etc. Same can be done by declaring these flags in `~/.spack/compilers.yaml`:
```
compilers:
- compiler:
    modules: []
    operating_system: centos6
    paths:
      cc: /usr/bin/gcc
      cxx: /usr/bin/g++
      f77: /usr/bin/gfortran
      fc: /usr/bin/gfortran
    flags:
      cflags: -O3 -fPIC
      cxxflags: -O3 -fPIC
      cppflags: -O3 -fPIC
    spec: gcc@4.7.2
```

If you want to use flags for `dealii` only, you can first build all the dependencies without flags and then build `dealii` with custom flags:
```
spack install --only dependencies dealii
spack install dealii cppflags="-march=native -O3"
```

See this [google forum topic](https://groups.google.com/forum/?fromgroups#!topic/dealii/3Yjy8CBIrgU) for discussion on which flags to use. You can also use `spack install dealii+optflags` to enable extra optimization flags in release build.

### Different versions coexisting:
One can easily have slightly different versions of deal.II side-by-side, e.g. to compile development version of deal.II with complex-valued PETSc and `gcc` compiler run
```
spack install dealii@develop+mpi+petsc~int64%gcc ^petsc+complex~hypre
```
The good thing is that if you already have deal.ii built with real-valued petsc, then only `petsc`, `slepc` and `deal.ii` itself will be rebuild. Everything else (`trilinos`, `mumps`, `metis`, etc) will be reused.

One can use `environment-modules` (see above) to automatically set `DEAL_II_DIR` to the complex version: 
```
spack load dealii@develop+mpi+petsc~int64%gcc ^petsc+complex~hypre
```

### Filesystem Views:
If you prefer to have the whole dealii suite (and possible something else) symlinked into a single path (like `/usr/local`), one can use [Filesystem Views](http://spack.readthedocs.io/en/latest/workflows.html#filesystem-views):
```
spack view -v symlink dealii_suite dealii@develop
```
You can also add to this view other packages, i.e.
```
spack view -v symlink dealii_suite the-silver-searcher
```


### Check before build:
It is often convenient to check which version of packages, compilers, variants etc will be used before actually starting installation. That can be done by examining the concretized spec via `spack spec` command, e.g. 
```
spack spec dealii@develop+mpi+petsc~int64%gcc ^petsc+complex~hypre
```

### Develop using Spack
There are several ways to use Spack while contributing patches to the deal.II. The simplest is to create a [Filesystem view](#filesystem-views) for an already installed library and then compile patched version of deal.II manually by providing path to the view for each dependency. For example on macOS with `openblas`:
```
export DEAL_II_VIEW=/Users/davydden/spack/_dealii_suite
cmake -DCMAKE_FIND_FRAMEWORK=LAST -DCMAKE_INSTALL_RPATH_USE_LINK_PATH=FALSE -DCMAKE_INSTALL_RPATH=${DEAL_II_VIEW} -DCMAKE_BUILD_TYPE=DebugRelease -DDEAL_II_COMPONENT_EXAMPLES=ON -DDEAL_II_WITH_THREADS:BOOL=ON -DBOOST_DIR=${DEAL_II_VIEW} -DBZIP2_DIR=${DEAL_II_VIEW} -DLAPACK_FOUND=true -DLAPACK_INCLUDE_DIRS=${DEAL_II_VIEW}/include -DLAPACK_LIBRARIES=${DEAL_II_VIEW}/lib/libopenblas.dylib -DMUPARSER_DIR=${DEAL_II_VIEW} -DUMFPACK_DIR=${DEAL_II_VIEW} -DTBB_DIR=${DEAL_II_VIEW} -DZLIB_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_MPI:BOOL=ON -DCMAKE_C_COMPILER=${DEAL_II_VIEW}/bin/mpicc -DCMAKE_CXX_COMPILER=${DEAL_II_VIEW}/bin/mpic++ -DCMAKE_Fortran_COMPILER=${DEAL_II_VIEW}/bin/mpif90 -DGSL_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_GSL:BOOL=ON -DHDF5_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_HDF5:BOOL=ON -DP4EST_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_P4EST:BOOL=ON -DPETSC_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_PETSC:BOOL=ON -DSLEPC_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_SLEPC:BOOL=ON -DTRILINOS_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_TRILINOS:BOOL=ON -DMETIS_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_METIS:BOOL=ON -DDEAL_II_COMPONENT_DOCUMENTATION=OFF -DARPACK_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_ARPACK=ON -DDEAL_II_ARPACK_WITH_PARPACK=ON -DNETCDF_DIR=${DEAL_II_VIEW} -DOPENCASCADE_DIR=${DEAL_II_VIEW} -DDEAL_II_WITH_OPENCASCADE=ON ../
```
Here we specify locations of libraries in `${DEAL_II_VIEW}` and also point to `mpi` compilers by `${DEAL_II_VIEW}/bin/mpicc` and alike. Keep in mind that the autodetection of `LAPACK` is turned off and therefore we specified full path to libs as `${DEAL_II_VIEW}/lib/libopenblas.dylib`. You would need to adjust the suffix if you are on Linux.

### Keep the stage to run unit tests
By default, the build folder (aka `stage`) for each package is created in a temporary system dependent location.
This gets purge by system on next restart.
If you want to keep the stage after the installation, you need to do two things:
First, make spack use a custom location for stage by adding the following to your `~/.spack/config.yaml`:
```
config:
  build_stage:
    - $spack/var/spack/stage
```
Second, prescribe an extra argument to `install` command to keep the stage after successful installation:
```
spack install --keep-stage dealii@develop
```

### MKL and Licensed software
Spack supports installation of [licensed software](http://spack.readthedocs.io/en/latest/packaging_guide.html#license). For example in order to install MKL on Linux:

1. add the `license.lic` file to `${SPACK_ROOT}/etc/spack/licenses/intel/`.
2. manually download Intel MKL archive `l_mkl_11.3.2.181.tgz` (Spack can not do it for you due to the license of `intel-mkl`).
3. `cd` to the folder with archive and run `spack install intel-mkl@11.3.2.181`.

One can then run `spack install dealii ^intel-mkl@11.3.2.181`.


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