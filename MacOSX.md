# Using deal.II on Mac OS X

<!-- No auto-Table of Contents support! -->

## Installing the prepacked image file

The easiest way to install dealII on Mac OS X is by downloading the prepackaged `.dmg` file from the download page. Simply download and drag the `deal.II.app` to the `/Applications/` folder.

The application is only a launcher for the `Terminal.app` application, which sets all required variables for deal.II to work correctly on your system.

The examples are in the folder

	/Applications/deal.II.app/Contents/Resources/examples/

This application is always built with the latest version of the MacOS X operating system, and with the latest version of XCode. See instructions below to make sure your system is compatible with the binary application.

## Detailed instructions:

1. Install xcode from the app store. You might need to install the command line tools. Open a terminal and make sure "clang --version" does not report "command not found". You might need to run

        xcode-select --install

    to install the tools.
2. Install cmake if you don't have it (check by typing "cmake" in terminal). To do so got to http://www.cmake.org/download/ and download the Mac OSX binary .dmg, then open the .dmg and drag cmake into "applications".
3. Download the latest .dmg of deal.II from https://www.dealii.org/download.html and drag it into "applications".
4. You should now have cmake and deal.II in your "applications" in finder. Opening the deal.II app will give you a terminal you can use to compile deal.II applications. The library is installed under ```/Applications/deal.II.app/Contents/Resources```
NOTE: If you can not run the app because it is from an "unidentified developer", go to "Applications" in "Finder", ctrl+click on the deal.II app and select "Open".
5. Set up your bash profile. Open a terminal and type "touch .profile;open .profile". In the editor add the following line to the file:

        export PATH=/Applications/CMake.app/Contents/bin:$PATH

6. done.


# Instructions for Mac 10.9 Users, Deal.II ver. 8.0 or later (current trunk)

From Mac OS 10.9 onwards, Apple changed quite a few things in their developer tools. You should install XCode 5 **and** its command line tools. If you only do this, however, the installer  will not populate /usr/lib and /usr/include as it used to do, since from Mac OS 10.9, they decided that everything should be self contained in specific SDK directories. If you want to have the standard locations populated (to have, among others, zlib.h, etc.) you should also run the following command:
  
```sh
xcode-select --install
```

which will download and install a lot of useful things for us.

If you decide to compile everything yourself, you should follow the instructions below. Otherwise we provide a useful package containing all precompiled libraries for the full optional deal.II under the Downloads section of this site.

# Simple instructions for Mac 10.8 Users, Deal.II ver. 8.1

Here are instructions for a minimal deal.II installation (without other packages):
  1. Install XCode
  1. Install XCode "command line tools" (you can find them under preferences->Downloads->Components).
  1. Install cmake (download Mac OSX 64/32-bit Universal .dmg file, click to install, hit "install links into /local/bin")
  1. Test that cmake works by running "cmake --version" in a terminal
  1. Now proceed as explained in the deal.II readme (download the .tar.gz, extract using "tar xf deal.II-8.1.0.tar.gz", create build directory, cd into it)
  1. You may need to overwrite the cxx compiler by calling:
      cmake -D CMAKE_CXX_COMPILER=clang++ ..
  1. Now proceed with "make install" and "make test"


# Instructions for Mac 10.8 Users, Deal.II ver. 8.0 or later (current trunk)

Recent Mac Versions come with Apple own version of gcc and clang compilers. They are both based on LLVM backend, but they behave slightly differently. 

**If you want to use deal.II in conjunction with Trilinos, then you should install the macports compiler gcc47**.

Step by step instructions (Working on Mac OS 10.8.8 and XCode 4.6.2): 

  1. Install XCode
  1. Install XCode "command line Tools" (you can find them under preferences->Downloads->Components).
  1. (**only if you want to use also Trilinos**) Install macports (http://www.macports.org/)
  1. Install gcc47
```
sudo port install gcc47
```
  1. **Optional**: install all additional packages you might want to use with `deal.II`. For example:
```
sudo port install arpack +gcc47 
sudo port install tbb +gcc47
sudo port install netcdf +gcc47
sudo port install boost +gcc47
sudo port install suitesparse +gcc47
sudo port install metis +gcc47
sudo port install qt4-mac +gcc47
```

The openmpi package that comes with macports will not work properly with Trilinos. You should install it from scratch if you want mpi support.

I have customised my profile file in order to allow me to switch from one compiler to another in a relatively easy way, maintaining a reasonable structure in the libraries that I install, without the need to recompile everything when I change my mind. I try to use the same compiler for all the libraries I use, and to keep copies around of the compiled libraries in order to have a "backup plan" when things go wrong with my own deal.II experiments...

I usually keep all source files under the same directory (`$SRC`), build in subdirectories (`$SRC/library-name/$TYPE`) of the source directories, and install in separate "destination" directories (`$DST/library-name-$TYPE`). Of  course this is my personal taste. You can easily change this below....

At the end of your `~/.profile` file, add the following lines:
```
# Available compilers: native, gcc, openmpi
. ~/.profile-libs gcc
```

Create the file `~/.profile-libs`, which will accept a command line argument specifying the compiler to use:
```sh
# TYPE is the first argument 
TYPE=$1

# Versions of the external libraries we want to compile/install
TRILINOS_VER=11.0.3
P4EST_VER=0.3.4

# Where source directories and install destination should be.
SRC=~/libs_src
DST=~/libs

# Change the following to suite your own system
case "$TYPE" in
native)
    export CC=/usr/bin/clang
    export CXX=/usr/bin/clang++
    export USE_MPI=OFF
    ;;
gcc)
    export CC=/opt/local/bin/gcc-mp-4.7 
    export CXX=/opt/local/bin/g++-mp-4.7 
    export USE_MPI=OFF
    ;;
openmpi)
    export CC=/usr/local/bin/mpicc
    export CXX=/usr/local/bin/mpic++
    export USE_MPI=ON
    ;;
esac

export DEAL_II_SRC=$SRC/deal.II
export P4EST_SRC=$SRC/p4est-$P4EST_VER
export TRILINOS_SRC=$SRC/trilinos-$TRILINOS_VER-Source

export DEAL_II_BUILD=$DEAL_II_SRC/$TYPE
export TRILINOS_BUILD=$TRILINOS_SRC/$TYPE
export P4EST_BUILD=$P4EST_SRC/$TYPE

export DEAL_II_DIR=$DST/deal.II-$TYPE
export P4EST_DIR=$DST/p4est-$P4EST_VER-$TYPE
export TRILINOS_DIR=$DST/trilinos-$TRILINOS_VER-$TYPE

export DYLD_LIBRARY_PATH=$TRILINOS_DIR/lib

```

This assumes you have downloaded all your packages under `$SRC`, and that you are installing them under `$DST`. Change these to your liking. 

## Trilinos 11.0.3

Once the above environment variables are set, you can install Trilinos using the following script:

### trilinos.sh

```sh
EXTRA_ARGS=$@

if [ ! -d "$TRILINOS_BUILD" ]; then
	mkdir $TRILINOS_BUILD
fi
cd  $TRILINOS_BUILD

cmake \
-D TrilinosFramework_ENABLE_MPI:BOOL=$USE_MPI \
-D CMAKE_INSTALL_PREFIX:PATH=$TRILINOS_DIR \
-D TPL_ENABLE_MPI:BOOL=$USE_MPI \
-D BUILD_SHARED_LIBS:BOOL=ON \
-D CMAKE_BUILD_TYPE:STRING=RELEASE \
-D Trilinos_ENABLE_Fortran:BOOL=OFF \
-D Trilinos_WARNINGS_AS_ERRORS_FLAGS:STRING="" \
-D CMAKE_VERBOSE_MAKEFILE:BOOL=TRUE \
-D Trilinos_ENABLE_TESTS:BOOL=OFF \
-D Trilinos_ENABLE_ALL_PACKAGES:BOOL=OFF \
-D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES:BOOL=ON \
-D Trilinos_ENABLE_Epetra:BOOL=ON \
-D Trilinos_ENABLE_EpetraExt:BOOL=ON \
-D Trilinos_ENABLE_Tpetra:BOOL=ON \
-D Trilinos_ENABLE_Jpetra:BOOL=ON \
-D Trilinos_ENABLE_Kokkos:BOOL=ON \
-D Trilinos_ENABLE_Sacado:BOOL=ON \
-D Trilinos_ENABLE_Amesos:BOOL=ON \
-D Trilinos_ENABLE_AztecOO:BOOL=ON \
-D Trilinos_ENABLE_Ifpack:BOOL=ON \
-D Trilinos_ENABLE_Teuchos:BOOL=ON \
-D Trilinos_ENABLE_Rythmos:BOOL=ON \
-D Trilinos_ENABLE_Piro:BOOL=ON \
-D Trilinos_ENABLE_MOOCHO:BOOL=ON \
-D Trilinos_ENABLE_ML:BOOL=ON \
-D Trilinos_ENABLE_Thyra:BOOL=ON \
-D Trilinos_ENABLE_TrilinosCouplings:BOOL=ON \
$EXTRA_ARGS \
../

make -j2 install
```

## p4est

The following script will install p4est. It has been modified from the file `doc/p4est-setup.sh` in the p4est distribution, to use the variables which where set in the environment.

### p4est.sh

```sh
#! /bin/bash

# This program comes with ABSOLUTELY NO WARRANTY.

UNPACK=$P4EST_SRC

# choose names for fast and debug compilation directories
BUILD_DIR="$P4EST_BUILD"
BUILD_FAST="$BUILD_DIR/FAST"
BUILD_DEBUG="$BUILD_DIR/DEBUG"

function busage() {
        echo "Usage: `basename $0` <p4est_tar.gz_file> [location>](<install)"
}
function bdie () {
        echo "Error: $@"
        exit 1
}


if test -z "$CFLAGS" -a -z "$P4EST_CFLAGS_FAST" ; then
        export CFLAGS_FAST="-O2"
else
        export CFLAGS_FAST="$CFLAGS $P4EST_CFLAGS_FAST"
fi
echo "CFLAGS_FAST: $CFLAGS_FAST"
if test -z "$CFLAGS" -a -z "$P4EST_CFLAGS_DEBUG" ; then
        export CFLAGS_DEBUG="-O0 -g"
else
        export CFLAGS_DEBUG="$CFLAGS $P4EST_CFLAGS_DEBUG"
fi
echo "CFLAGS_DEBUG: $CFLAGS_DEBUG"

# choose names for fast and debug installation directories
INSTALL_DIR="$P4EST_DIR"
INSTALL_FAST="$INSTALL_DIR/FAST"
INSTALL_DEBUG="$INSTALL_DIR/DEBUG"

echo
echo "This script tries to unpack, configure and build the p4est library."
echo "Build FAST: $BUILD_FAST"
echo "Build DEBUG: $BUILD_DEBUG"
echo "Install FAST: $INSTALL_FAST"
echo "Install DEBUG: $INSTALL_DEBUG"
echo "Checking environment: CFLAGS P4EST_CFLAGS_FAST P4EST_CFLAGS_DEBUG"

# remove old versions
if test -d "$BUILD_DIR" ; then
        rm -rf "$BUILD_DIR"
fi

test -f "$UNPACK/src/p4est.h" || bdie "Main header file missing"
test -f "$UNPACK/configure" || bdie "Configure script missing"

echo "See output in files .../config.output and .../make.output"
echo
echo "Build FAST version in $BUILD_FAST"
mkdir -p "$BUILD_FAST"
cd "$BUILD_FAST"

if [ "$USE_MPI" == "ON" ]; then
    ENABLE_MPI=--enable-mpi
else
    ENABLE_MPI=""
fi

"$UNPACK/configure" $ENABLE_MPI --enable-shared \
        --disable-vtk-binary --with-blas \
        --prefix="$INSTALL_FAST" CFLAGS="$CFLAGS_FAST" \
        CPPFLAGS="-DSC_LOG_PRIORITY=SC_LP_ESSENTIAL" \
        "$@" > config.output || bdie "Error in configure"
make -C sc -j 8 > make.output || bdie "Error in make sc"
make -j 8 >> make.output || bdie "Error in make p4est"
make install >> make.output || bdie "Error in make install"
echo "FAST version installed in $INSTALL_FAST"

echo
echo "Build DEBUG version in $BUILD_DEBUG"
mkdir -p "$BUILD_DEBUG"
cd "$BUILD_DEBUG"
"$UNPACK/configure" --enable-debug $ENABLE_MPI --enable-shared \
        --disable-vtk-binary --without-blas \
        --prefix="$INSTALL_DEBUG" CFLAGS="$CFLAGS_DEBUG" \
        CPPFLAGS="-DSC_LOG_PRIORITY=SC_LP_ESSENTIAL" \
        "$@" > config.output || bdie "Error in configure"
make -C sc -j 8 > make.output || bdie "Error in make sc"
make -j 8 >> make.output || bdie "Error in make p4est"
make install >> make.output || bdie "Error in make install"
echo "DEBUG version installed in $INSTALL_DEBUG"
echo

```

## Deal.II

And finally, the deal.II library. Please consider using this script, since it will automatically 
feed the *build tests* page with the outcome of your compilation.

### deal.sh

```sh
if [ ! -d "$DEAL_II_BUILD" ]; then
	mkdir $DEAL_II_BUILD
fi

NAME=`basename $DEAL_II_BUILD`
cp deal.conf $DEAL_II_BUILD/$NAME

cd $DEAL_II_SRC
svn update

LOGFILE=/usr/local/src/conf-scripts/deal.log

umask 0006

$DEAL_II_SRC/contrib/utilities/build_test -j4 \
	CONFIGFILE=$DEAL_II_BUILD/$NAME \
	SOURCE_DIR=$DEAL_II_SRC \
	builddir=$DEAL_II_BUILD \
	installdir=$DEAL_II_DIR \
	LOGFILE=$LOGFILE \
	MAKEOPTS=-j2 \
	CLEAN_TMPDIR=false

grep SUCCESSFUL $LOGFILE && mail  dealii.build.tests@gmail.com < $LOGFILE 

```

## Using deal.II with Xcode IDE

Setting up Xcode for use of the deal.II libraries requires lots of project specifications that can automatically be generated by CMake. The following procedure covers the installation of necessary tools, the compilation of the deal.II libraries and the Xcode project generation. It was tested with Mac OS X 10.8.4, Xcode 4.6.3, CMake 2.8.10.2 and deal.II 8.0pre.

First, install Xcode (via the app store) and MacPorts (available on http://www.macports.org). Then install CMake with the terminal command
```
sudo port install cmake
```
Create a new folder on your machine somewhere and change to it in terminal. Then download the current deal.II version by typing
```
svn checkout https://svn.dealii.org/trunk/deal.II
```
Change to the deal.II subdirectory
```
cd deal.II
```
Create new build directory and change to it
```
mkdir build
cd build
```
Run cmake to prepare compilation
```
cmake -DCMAKE_INSTALL_PREFIX=/Users/Username/libs/deal_8.0.pre ..
```
where the path is the directory where the deal.II library will be installed, change it as you like.
Compile the library
```
make -j8
make install
```
8 is the number of cores your machine has, change this if necessary. The first command might take a while (up to one hour). After compilation, the whole deal.II directory can be deleted, as the library is now installed in the path specified above (INSTALL_PREFIX). Go to the specified directory and change to the step-1 example folder. Run cmake to prepare example to be used with Xcode
```
cmake -G Xcode -DDEAL_II_DIR=/Users/Username/libs/deal_8.0.pre .
```
The DDEAL_II_DIR is the directory where the library is to be found, like specified before (INSTALL_PREFIX). The file step-1.xcodeproj is now generated and can be opened with Xcode. A click on RUN should compile the code into subdirectory "Debug", but not execute it. For build and run change Product -> Scheme to 'step-1'. Code completion, compiling and debugging should be working now. If you add a new class, activate target membership step-1 so that the class is automatically compiled when clicking on run.


## Getting rid of pop-ups when running executables compiled with deal.II

When running an deal.II executable that uses MPI on a mac with the firewall or parental control enabled, a pop-up appears that asks 
"Do you want the application xxx to accept incoming network connections?". It pops up as many MPI precesses you asked for which becomes
annoying. The way to prevent the pop-up is to code sign your executable. If you have a code signing certificate, configure you project 
using the OSX_CERTIFICATE_NAME feature:
```
cmake -DOSX_CERTIFICATE_NAME="put your code signing certificate name here" .
```
The executable will be signed using the certificate every time it is recompiled.

If you don't have a certificate or you are not sure, then follow these instructions:

  1. Open Keychain Access.
  1. Choose Keychain Access->Certificate Assistant->Create a certificate.
  1. Choose a name for the certificate (Something like "deal.II developer").
  1. Choose Certificate Type "Code Signing".
  1. Check "Let me override defaults".
  1. Press Continue.
  1. Write any serial number you want (the name and serial number must be unique in the system).
  1. Select how many days you want the certificate to be valid (2000 is just above 5 years).
  1. Press Continue then put your personal data (nobody will see that but you so you can have fun with it).
  1. Press Continue to get Key Pair Information. Keep as is.
  1. Press Continue to get Key Usage Extension. Keep as is.
  1. Press Continue to get Extended Key Usage Extension. Keep as is.
  1. Press Continue to get Basic Constraints Extension. Keep as is.
  1. Press Continue to get Subject Alternate Name Extension. Keep as is.
  1. Press Continue and keep the Keychain "login" selected.
  1. Press Continue then Done.
  1. Right Click on your newly generated certificate.
  1. Select "Get Info"
  1. Select the triangle next to "Trust"
  1. Scroll down to "Code Signing" and change it "Always Trust"
  1. The system will ask for the administrator password to allow the changes. Enter it and press OK.

Now you have a code signing certificate that you can use with deal.II. Configure the project with the new certificate with a command line like this:
```
cmake -DOSX_CERTIFICATE_NAME="deal.II developer" .
```



---- 
# Older instructions

---- 

## Installation without PETSc and Trilinos

The installation of deal.II without support for Petsc and Trilinos is standard.
The gcc that comes with the latest version of Xcode works without problems.
At the time of writing this was gcc version 4.2.1 (Apple Inc. build 5646) (dot 1).
It is suggested that you use the latest version of Xcode available from
[## Dynamic libraries not found

If you get a message of the kind

```
dyld: Library not loaded: libdeal_II_2d.g.6.2.1.dylib
  Referenced from: /Users/.../deal.II/examples/step-1/./step-1
  Reason: image not found
make: *** [run](http://developer.apple.com/technologies/tools/xcode.html]) Trace/BPT trap
```

you have to set the path for dynamical libraries. Using <tt>csh</tt> or <tt>tcsh</tt>, this reads
```
setenv DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Users/.../deal.II/lib
```

while <tt>bash</tt> users use
```
export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Users/.../deal.II/lib
```



## Compiling Trilinos without Fortran support

One of the major hassles with using deal.II on a mac was compiling Trilinos which required fortran support.
It now seems possible to compile Trilinos 10.4.1  with all the features needed for deal.ii without fortran support. This is very nice as you can then simply use the mac gcc compilers and not worry about all the compatibility issues.

Here is my configure script:

```sh
EXTRA_ARGS=$@
TRILINOS_HOME=/Users/andrewmcbride/lib/trilinos-10.4.1-Source

 cmake \
 -D CMAKE_INSTALL_PREFIX:PATH=/Users/andrewmcbride/lib/trilinos-10.4.1-Source/MAC_SL \
 -D CMAKE_BUILD_TYPE:STRING=RELEASE \
 -D BUILD_SHARED_LIBS:BOOL=ON \
 -D TPL_ENABLE_MPI:BOOL=OFF \
 -DCMAKE_C_FLAGS:STRING="-fPIC" \
 -DCMAKE_CXX_FLAGS:STRING="-fPIC" \
 -D Trilinos_ENABLE_ALL_PACKAGES:BOOL=OFF \
 -D Trilinos_ENABLE_Stratimikos:BOOL=ON \
 -D Trilinos_ENABLE_Sacado:BOOL=ON \
 -D Trilinos_ENABLE_NOX-Epetra:BOOL=OFF \
 -D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES:BOOL=ON \
 -D Trilinos_ENABLE_Fortran:BOOL=OFF \
 -D CMAKE_CXX_COMPILER:FILEPATH=/usr/bin/g++ \
 -D CMAKE_C_COMPILER:FILEPATH=/usr/bin/gcc \
 $EXTRA_ARGS \
 ${TRILINOS_HOME}
```

## Fortran support

Xcode does not provide support for Fortran.
This is needed (at present but see comments above) to compile Trilinos and PETSc with reasonable features.
To get the Fortan compilers on your mac you can follow one of several routes.

 1. Install the precompiled high performance gcc libraries with Fortan support avaialble from  [A known problem with the latest release of these gcc libraries is said to be fixed (see [http://gcc.gnu.org/bugzilla/show_bug.cgi?id=43648](http://sourceforge.net/projects/hpc/]))

 1. Build gcc with fortran support from source available from [http://gcc.gnu.org/]

 1. Install gcc with fortran support using macports or similar.
This has never worked for me.

You will then need to install deal.II using these libraries: for example on my
mac the configure looks like this (no trilinos or petsc)
```
  $ ./configure --with-umfpack --with-blas --with-lapack
  CC=/Users/andrewmcbride/lib/gcc_snwleo/usr/local/bin/gcc
  CXX=/Users/andrewmcbride/lib/gcc_snwleo/usr/local/bin/g++
  F77=/Users/andrewmcbride/lib/gcc_snwleo/usr/local/bin/gfortran
  --with-doxygen=/Applications/Doxygen.app/Contents/Resources/doxygen
```

## Dynamic_cast bug

Due to inconsistencies between the Xcode and standard gcc releases a bug arose that caused deal.II to "crash" when performing certain dynamic casts.
A work around this problem was implemented in December 2009.
It is suggested that you install the svn version of deal.II to ensure this problem does not arise


## Installation with Trilinos 10.2 and CMake

The latest versions of Trilinos require one to use the CMake build system, which requires some system configuration up front.  These steps were followed to obtain a working installation of Trilinos with MPI and deal.II.

On a fully updated 10.6 (Snow Leopard) machine, you should be able to install mac ports, and then install their versions of gcc45 and cmake.  To get a Fortran MPI compiler, you can download the latest version of OpenMPI, and compile it with the MacPorts gcc45, and install it in some directory (NOT the system one!!), making sure that the new /bin directory is in your PATH first.

Next, download and untar the Trilinos source, then create a directory where you want to build Trilinos.  Inside of that directory, copy the following into a text file:

```sh
 #!/bin/sh

 EXTRA_ARGS=$@

 rm -f CMakeCache.txt

 cmake \
 -D TrilinosFramework_ENABLE_MPI:BOOL=ON \
 -D CMAKE_INSTALL_PREFIX=/path_to/trilinos_install_dir \
 -D TPL_ENABLE_MPI:BOOL=ON \
 -D MPI_BASE_DIR:PATH=/path_to_custom_MPI_install/mpi \
 -D BUILD_SHARED_LIBS:BOOL=ON \
 -D CMAKE_BUILD_TYPE:STRING=DEBUG \
 -D CMAKE_CXX_COMPILER:FILEPATH=mpicxx \
 -D CMAKE_C_COMPILER:FILEPATH=mpicc \
 -D CMAKE_Fortran_COMPILER:FILEPATH=mpif90 \
 -D Trilinos_WARNINGS_AS_ERRORS_FLAGS:STRING="" \
 -D CMAKE_VERBOSE_MAKEFILE:BOOL=TRUE \
 -D Trilinos_ENABLE_TESTS:BOOL=OFF \
 -D Trilinos_ENABLE_ALL_PACKAGES:BOOL=TRUE \
 -D Trilinos_ENABLE_ALL_OPTIONAL_PACKAGES:BOOL=ON \
 -D Trilinos_ENABLE_Epetra:BOOL=ON \
 -D Trilinos_ENABLE_EpetraExt:BOOL=ON \
 -D Trilinos_ENABLE_Tpetra:BOOL=ON \
 -D Trilinos_ENABLE_Jpetra:BOOL=ON \
 -D Trilinos_ENABLE_Kokkos:BOOL=ON \
 -D Trilinos_ENABLE_Sacado:BOOL=ON \
 -D Trilinos_ENABLE_Amesos:BOOL=ON \
 -D Trilinos_ENABLE_AztecOO:BOOL=ON \
 -D Trilinos_ENABLE_Ifpack:BOOL=ON \
 -D Trilinos_ENABLE_Teuchos:BOOL=ON \
 -D Trilinos_ENABLE_ML:BOOL=ON \
 -D Trilinos_ENABLE_Thyra:BOOL=ON \
 -D Trilinos_ENABLE_TrilinosCouplings:BOOL=ON \
 $EXTRA_ARGS \
 /path_to/trilinos-10.2.2-Source
```

Note that you will have to specify the paths for where to install Trilinos, where to find the source, and where to find your newly compiled MPI.  Next, you should give this text file executable permissions, and run it; Trilinos will configure.  You should then be able to compile it using 'make all -jX', where X is the number of cores you have, and then 'make install'.

To configure deal.II, you should set the environment variable 'TRILINOS_DIR' to your install directory for Trilinos, then export the /lib subdirectory of that location to the DYLD_LIBRARY_PATH.  You should then be able to configure deal.II and have it see the installation.

Note: additional packages can be added by simply adding another line like {{{-D Trilinos_ENABLE_AztecOO:BOOL=ON}}}  Also, there is no claim that this list of options is complete/optimal, but it seems to work well for the author of this note.



## Teuchos Error (Trilinos 10.4.1) when deal.II is Configured with Mumps, Blacs, Petsc and Trilinos 10.4.1

When deal.ii was configured to use Mumps and Blacs like so:

```
        ./configure --enable-shared --disable-threads \
                    --with-cpu=native --with-umfpack  \
                    --with-mumps=PATH-TO-MUMPS \
                    --with-scalapack=PATH-TO-SCALAPACK \
                    --with-blacs=PATH-TO-BLACS  --with-lapack=lapack \
                    --with-metis-libs=/opt/local/lib \
                    --with-trilinos=PATH-TO-TRILINOS
```

the following error occurred during compilation:

```
        ============================ Compiling expand_instantiations
        ============================ Compiling report_features
        Undefined symbols:
        "Teuchos::PrintActiveRCPNodes::PrintActiveRCPNodes()", referenced from:
         global constructors keyed to report_features.cc in ccnoQB3F.o
        "Teuchos::PrintActiveRCPNodes::~PrintActiveRCPNodes()", referenced from:
        global constructors keyed to report_features.cc in ccnoQB3F.o
        ld: symbol(s) not found
```

Removing {{{--with-mumps and --with-blacs}}} allowed successful compilation.

The compilers used above were OpenMPI's MPI wrappers (including the Fortran MPI compiler), which were compiled with Macports' gcc45.
