# Introduction

deal.II is somewhat difficult to install especially if you are new to it and
if you need additional dependencies for parallel computations (p4est,
Trilinos, PETsc, etc.). There are various different ways to acquire deal.II and
its (optional) dependencies and the number of choices can be overwhelming. This document tries to help you navigating the
different options.

Please follow the following guidelines:

1. Are you happy with latest release of deal.II? If yes, go to the next
   point. If you want to work on deal.II itself, you will need the latest
   development version. Go to #development.

2. Are you just trying things out and learning how to use deal.II? If yes, try
   #VM or if you are confortable using docker, try #docker. Otherwise go to
   the next point.

3. Is your machine running Windows? See #Windows. Are you running
   MacOS? You can use the prebuilt package, see #MacOS.

4. Are you running Ubuntu/Debian/Arch/Gentoo/..., consider installing a #Linux
   package.

5. If you need additional dependencies (MPI, Trilinos, p4est, PETSC, ...) it
   is easier to use a source-based installer like #candi or #spack. Otherwise,
   go to the next point.

6. Download the source code to deal.II (.tar.gz file from the latest 
     https://github.com/dealii/dealii/releases
   and go to "Installation from source".

7. None of this works or still lost? Contact us at https://www.dealii.org/mail.html

# Installation from source
  Download a .tar.gz with the source from
  https://github.com/dealii/dealii/releases or clone the git repository (``
  git clone https://github.com/dealii/dealii.git``) and proceed with
  installation as described at
  https://www.dealii.org/current/readme.html#installation

# VM
 Install VirtualBox on your Windows/MacOS/Linux machine and download the
 virtual machine image from https://www.math.clemson.edu/~heister/dealvm/

# docker
  After you install docker on your machine, you can download and run
  precompiled docker images. See https://github.com/dealii/docker-files for
  more information.

# Development version
  - You can use the VM or windows WSL, but most of us run Linux (or MacOS) natively.
  - You will need to install deal.II from source. See "Development sources" at
    https://www.dealii.org/download.html
  - What dependencies you need depends on what you are trying to do. You can
    install dependencies in several different ways: manually, using the
    Debian/Ubuntu or other packages, using candi, or using spack

# Windows:
  Instructions: https://github.com/dealii/dealii/wiki/Windows

  If you are running Windows 10, we recommend using the WSL as this allows you
  to use the Debian/Ubuntu packages and run with MPI and other
  dependencies. Alternatively, you can compile deal.II natively under Visual
  Studio, but there is limited support for other dependencies (MPI, TBB,
  PETSc, Trilinos).  

  Note that WSL practically work like a Linux machine (with a few quirks such
  as graphical user interfaces), so you can install dependencies (and deal.II)
  from source or using candi or spack.

# MacOS
  We provide a binary package (.dmg) as part of each of the releases, see
  https://github.com/dealii/dealii/releases and download and install it.
  For more details see https://github.com/dealii/dealii/wiki/MacOSX

  Otherwise, the .dmg is also a convenient way to aquire common dependencies
  and installing from source is the easy, see "Installation from source".

# Linux packages

  We provide packages to several linux distributions as shown under 
  "Linux distributions" at https://www.dealii.org/download.html

  For example, if you are running 18.04 you can use the deal.II package using

```
export REPO=ppa:ginggs/deal.ii-9.2.0-backports

apt-get update && apt-get install -y software-properties-common && add-apt-repository $REPO \
&& apt-get update && apt-get install -y libdeal.ii-dev
```

If you are running 20.04 you can use the deal.II package:

```
export REPO=ppa:ginggs/deal.ii-9.2.0-backports

apt-get update && apt-get install -y software-properties-common && add-apt-repository $REPO \
&& apt-get update && apt-get install -y libdeal.ii-dev
```

# Candi
  Candi is a tool maintained by the deal.II community to install deal.II and
  many of the optional dependencies on a linux-like system from source and is
  quite useful to run on clusters where you need typically need to install
  packages manually.
  See https://github.com/dealii/candi

