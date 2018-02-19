Table of Contents
=================

   * [Using deal.II on Mac OS X and Linux via Spack](#using-dealii-on-mac-os-x-and-linux-via-spack)
      * [Quick installation on the desktop](#quick-installation-on-the-desktop)
      * [Installation example on a Centos7 cluster (Emmy cluster of RRZE, Erlangen, Germany)](#installation-example-on-a-centos7-cluster-emmy-cluster-of-rrze-erlangen-germany)
      * [Enabling CUDA](#enabling-cuda)
      * [Environment Modules](#environment-modules)
      * [System provided packages](#system-provided-packages)
      * [Installing GCC](#installing-gcc)
      * [Best practices using Spack:](#best-practices-using-spack)
         * [Info:](#info)
         * [Extra options:](#extra-options)
         * [Compiler flags](#compiler-flags)
         * [Different versions coexisting:](#different-versions-coexisting)
         * [Filesystem Views:](#filesystem-views)
         * [Check before build:](#check-before-build)
         * [Develop using Spack](#develop-using-spack)
         * [Keep the stage to run unit tests](#keep-the-stage-to-run-unit-tests)
         * [MKL and Licensed software](#mkl-and-licensed-software)
         * [Freeze package versions](#freeze-package-versions)

# Using deal.II on Mac OS X and Linux via Spack

The deal.II suite is also available on Spack (https://github.com/LLNL/spack) -- a flexible package manager developed with High-Performance-Computing in mind. It is intended to let you build for many combinations of compiler, architectures, dependency libraries, and build configurations, all with a friendly, intuitive user interface.

For a quick overview of Spack's features, we recommend this short presentation https://tgamblin.github.io/files/Gamblin-Spack-SC15-Talk.pdf
or the following videos from lead developers:
[Massimiliano Culpo - SPACK: A Package Manager for Supercomputers, Linux and MacOS](https://www.youtube.com/watch?v=Qok-nXfIWfg) and 
[Todd Gamblin (LLNL) - Managing HPC Software Complexity with Spack](https://www.youtube.com/watch?v=Ie1cZTR09kk).

[Spack tutorial 101](http://spack.readthedocs.io/en/latest/tutorial.html) is good place to start as well.

Note: Spack is in active development and is in alpha state, thereby below we recommend using the specific snapshot of the code (hash), that was tested on macOS / Ubuntu / CentOS7 with deal.II and is being used to run the complete testsuite on `Unbutu16.04` with `openmpi` and `openblas`. See [Experimental section of CDash](https://cdash.kyomu.43-1.org/index.php?project=deal.II).

## Quick installation on the desktop

Add the following to `~/.bashrc` (or equivalent)
```
export SPACK_ROOT=/path/to/spack
export PATH="$SPACK_ROOT/bin:$PATH"
```
`SPACK_ROOT` is the destination where you want Spack to be installed (i.e. `$HOME/spack`).

Now clone Spack
```
cd $SPACK_ROOT
git clone https://github.com/llnl/spack.git .
git checkout develop
git reset --hard d8c105a7b1f882f6a25f6c590d31e94a891220e0
```

**Make sure C/C++/Fortran compilers are in path** (on Ubuntu you need to `sudo apt-get install gfortran`, on macOS you can compile `gcc` with spack, see [below](#installing-gcc), and you have **curl** (`sudo apt-get install curl`) to download packages. Then install the complete deal.II suite
```
spack install dealii@8.5.1
```
**DONE**! No extra (preliminary) configuration steps are needed on most Linux distributions. **IMPORTANT:** If you compile deal.II on a cluster, see the next section on how to use externally provided MPI implementation instead.

Before configuring your project you need to set `DEAL_II_DIR` by 
```
export DEAL_II_DIR=$(spack location -i dealii)
```
You may jump ahead and read [best practices using spack](#best-practices-using-spack). Also a good starting point is [Getting Started Guide](http://spack.readthedocs.io/en/latest/getting_started.html).


## Installation example on a Centos7 cluster (Emmy cluster of RRZE, Erlangen, Germany)
In order to use Spack on a cluster, there are two options: (1) you are a sysadmin and you know hardware details and can use Spack to properly configure and build MPI providers (e.g. `openmpi`); (2) you are a user and you need to make Spack work with MPI provided on your cluster. Below we consider the latter case.

Here is a brief step-by-step instruction to install deal.II on [Emmy cluster](https://www.rrze.fau.de/dienste/arbeiten-rechnen/hpc/systeme/emmy-cluster.shtml#access) of RRZE, Erlangen, Germany:

(1) Download spack
```
module load git
mkdir $WOODYHOME/spack
cd $WOODYHOME/spack
git clone https://github.com/llnl/spack.git $WOODYHOME/spack
git reset --hard d8c105a7b1f882f6a25f6c590d31e94a891220e0
export PATH=$WOODYHOME/spack/bin:$PATH
```
(2) Load `openmpi` and let Spack find GCC compiler which is also loaded as a dependency:
```
module load openmpi/2.0.2-gcc
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
  openmpi:
    version: [2.0.2]
    paths:
      openmpi@2.0.2%gcc@4.8.5: /apps/OpenMPI/2.0.2-gcc/
    buildable: False
  dealii:
    variants: +optflags~python
```
Those paths are the location where external packages can be found (i.e. `<prefix>` instead of `<prefix>/bin` or `<prefix>/lib`). `providers` section essentially tells Spack which packages to use to satisfy virtual dependencies such as `MPI`, `BLAS`, `LAPACK`, `ScaLAPACK`, etc.

(4) Now install deal.II:  `spack install dealii@8.5.1`.

## Enabling CUDA
You can build the current development version of `dealii` with CUDA. A possible configuration of `packages.yaml` is
```
packages:
 dealii:
  version: ['develop']
  variants: +cuda cuda_arch=52
 cuda:
  version: ['8.0.61']
```
where we specified explicitly CUDA architecture as well as version of `cuda` to be used.

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
CMakePackage:   dealii

Description:
    C++ software library providing well-documented tools to build finite
    element codes for a broad variety of PDEs.

Homepage: https://www.dealii.org

Preferred version:  
    8.5.1      https://github.com/dealii/dealii/releases/download/v8.5.1/dealii-8.5.1.tar.gz

Safe versions:  
    develop    [git] https://github.com/dealii/dealii.git
    8.5.1      https://github.com/dealii/dealii/releases/download/v8.5.1/dealii-8.5.1.tar.gz
    8.5.0      https://github.com/dealii/dealii/releases/download/v8.5.0/dealii-8.5.0.tar.gz
    8.4.2      https://github.com/dealii/dealii/releases/download/v8.4.2/dealii-8.4.2.tar.gz
    8.4.1      https://github.com/dealii/dealii/releases/download/v8.4.1/dealii-8.4.1.tar.gz
    8.4.0      https://github.com/dealii/dealii/releases/download/v8.4.0/dealii-8.4.0.tar.gz
    8.3.0      https://github.com/dealii/dealii/releases/download/v8.3.0/dealii-8.3.0.tar.gz
    8.2.1      https://github.com/dealii/dealii/releases/download/v8.2.1/dealii-8.2.1.tar.gz
    8.1.0      https://github.com/dealii/dealii/releases/download/v8.1.0/dealii-8.1.0.tar.gz

Variants:
    Name [Default]               Allowed values          Description


    adol-c [off]                 True, False             Compile with Adol-c
    arpack [on]                  True, False             Compile with Arpack and
                                                         PArpack (only with MPI)
    build_type [DebugRelease]    Debug, Release,         The build type to build
                                 DebugRelease            
    doc [off]                    True, False             Compile with documentation
    gsl [on]                     True, False             Compile with GSL
    hdf5 [on]                    True, False             Compile with HDF5 (only with
                                                         MPI)
    int64 [off]                  True, False             Compile with 64 bit indices
                                                         support
    metis [on]                   True, False             Compile with Metis
    mpi [on]                     True, False             Compile with MPI
    nanoflann [off]              True, False             Compile with Nanoflann
    netcdf [on]                  True, False             Compile with Netcdf (only with
                                                         MPI)
    oce [on]                     True, False             Compile with OCE
    optflags [off]               True, False             Compile using additional
                                                         optimization flags
    p4est [on]                   True, False             Compile with P4est (only with
                                                         MPI)
    petsc [on]                   True, False             Compile with Petsc (only with
                                                         MPI)
    python [on]                  True, False             Compile with Python bindings
    slepc [on]                   True, False             Compile with Slepc (only with
                                                         Petsc and MPI)
    sundials [off]               True, False             Compile with Sundials
    trilinos [on]                True, False             Compile with Trilinos (only
                                                         with MPI)

Installation Phases:
    cmake    build    install

Build Dependencies:
    adol-c     cmake     hdf5     muparser    oce     slepc         trilinos
    arpack-ng  doxygen   lapack   nanoflann   p4set   suite-sparse  zlib
    blas       graphivz  metis    netcdf      petsc   sundials      
    boost      gsl       mpi      netcdf-cxx  python  tbb

Link Dependencies:
    adol-c     doxygen   lapack   nanoflann   p4est   suite-sparse  zlib
    arpack-ng  graphviz  metis    netcdf      petsc   sundials      
    blas       graphviz  mpi      netcdf-cxx  python  tbb
    boost      hdf5      muparser oce         slepc   trilinos

Run Dependencies:
    None

Virtual Packages: 
    None
```

A lot of `spack` commands have help, for example
```
$ spack install -h
usage: spack install [-h] [--only {package,dependencies}] [-j JOBS]
                     [--keep-prefix] [--keep-stage] [--restage] [-n] [-v]
                     [--fake] [-f] [--clean | --dirty] [--run-tests]
                     [--log-format {junit}] [--log-file LOG_FILE]
                     ...

build and install packages

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
  --restage             if a partial install is detected, delete prior state
  -n, --no-checksum     do not check packages against checksum
  -v, --verbose         display verbose build output while installing
  --fake                fake install. just remove prefix and create a fake
                        file
  -f, --file            install from file. Read specs to install from .yaml
                        files
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

The `spec` command has a useful flag `-I` which will show install status of dependencies in the graph.

### Develop using Spack
Probably the easiest way to use Spack while contributing patches to the deal.II is the following. 
Install `deal.II` via Spack, go to its installation prefix and copy-paste the CMake command from `.spack/build.out`, which looks like
```
'cmake' '/path/to/spack/var/spack/stage/dealii-develop-tkowxhk55kpi7facfh3ufipofyolt6h7/dealii' '-DCMAKE_INSTALL_PREFIX:PATH=path/to/spack/opt/spack/darwin-sierra-x86_64/clang-8.1.0-apple/dealii-develop-tkowxhk55kpi7facfh3ufipofyolt6h7' '-DCMAKE_BUILD_TYPE:STRING=DebugRelease' <more options>
```
You would need to adjust the path to the `dea.II` source folder (first path) and should remove the `DCMAKE_INSTALL_PREFIX` as you probably don't want to accidentally override the version installed by Spack. 

Assuming that your build folder is within the `dealii` sources (tha'ts what `..\/` is for below), you can get this substitution done by
```
$ cat $(spack location -i dealii)/.spack/build.out | grep "==> 'cmake'" | sed -e "s/[^ ]*[^ ]/'..\/'/3" | cut -d " " -f2-
```

Before running `cmake` from the build folder, you may want to run 
```
spack env dealii@develop+mpi^openmpi^openblas bash
```
to make sure your environment (paths, variables, etc) is set exactly the same way as when compiling `deal.II` from Spack
(adjust the spec `dealii@develop+mpi^openmpi^openblas` to be what you use when you installed `deal.II`).

An alternative is to create a [Filesystem view](#filesystem-views) for an already installed library and then compile patched version of deal.II manually by providing path to the view for each dependency. 

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
Spack supports installation of [licensed software](http://spack.readthedocs.io/en/latest/packaging_guide.html#license) as well as usage of [Licensed compilers](http://spack.readthedocs.io/en/latest/getting_started.html#licensed-compilers). For example in order to install MKL on Linux:

1. add `Intel` license file as `license.lic` file to `${SPACK_ROOT}/etc/spack/licenses/intel/`.
2. run `spack install dealii ^intel-mkl@11.3.2.210`

In order to configure Intel compilers see [this page](http://spack.readthedocs.io/en/latest/getting_started.html#vendor-specific-compiler-configuration).


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