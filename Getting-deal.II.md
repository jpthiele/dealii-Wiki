# Introduction

deal.II is somewhat difficult to install especially if you are new to it and
if you need additional dependencies for parallel computations (p4est,
Trilinos, PETsc, etc.). There are various different ways to acquire deal.II and
its (optional) dependencies and the number of choices can be overwhelming. This document tries to help you navigate the
different options.

Please follow the following guidelines:

1. Are you happy with the latest release of deal.II? If yes, go to the next
   point. If you want to work on deal.II itself, you will need the latest
   development version. Go to [Development version](https://github.com/dealii/dealii/wiki/Getting-deal.II#development-version).

2. Are you just trying things out and learning how to use deal.II? If yes, try
   [VM](https://github.com/dealii/dealii/wiki/Getting-deal.II#vm) or if you are comfortable using docker, try [docker](https://github.com/dealii/dealii/wiki/Getting-deal.II#docker). Otherwise go to
   the next point.

3. Is your machine running Windows? See [Windows](https://github.com/dealii/dealii/wiki/Getting-deal.II#windows). Are you running
   MacOS? You can use the prebuilt package. See [MacOS](https://github.com/dealii/dealii/wiki/Getting-deal.II#macos).

4. Are you running Ubuntu/Debian/Arch/Gentoo/..., consider installing a [Linux
   package](https://github.com/dealii/dealii/wiki/Getting-deal.II#linux-packages). If not, continue to the next point.

5. If you need additional dependencies (MPI, Trilinos, p4est, PETSC, ...) it
   is easier to use a source-based installer like [candi](https://github.com/dealii/dealii/wiki/Getting-deal.II#candi) or spack. Otherwise,
   go to the next point.

6. Get the source code to deal.II by downloading the .tar.gz file from the latest release at
     https://github.com/dealii/dealii/releases
   and go to [Installation from source](https://github.com/dealii/dealii/wiki/Getting-deal.II#installation-from-source).

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

# Windows
  Instructions: https://github.com/dealii/dealii/wiki/Windows

  If you are running Windows 10, we recommend using the WSL as this allows you
  to use the Debian/Ubuntu packages and run with MPI and other
  dependencies. Alternatively, you can compile deal.II natively under Visual
  Studio, but there is limited support for other dependencies (MPI, TBB,
  PETSc, Trilinos).  

  Note that the WSL practically works like a Linux machine (with a few quirks such
  as graphical user interfaces), so you can install dependencies (and deal.II)
  from source or using candi or spack.

# MacOS
  We provide a binary package (.dmg) as part of each of the releases, see
  https://github.com/dealii/dealii/releases and download and install it.
  For more details see https://github.com/dealii/dealii/wiki/MacOSX

  Otherwise, the .dmg is also a convenient way to acquire common dependencies
  and installing from source is easy, see "Installation from source".

# Linux packages

  We provide packages to several linux distributions as shown under 
  "Linux distributions" at https://www.dealii.org/download.html

  For example, if you are running Ubuntu 18.04 or 20.04 you can use the deal.II package using

```
export REPO=ppa:ginggs/deal.ii-9.2.0-backports

apt-get update && apt-get install -y software-properties-common && add-apt-repository $REPO \
&& apt-get update && apt-get install -y libdeal.ii-dev
```

# Candi
  Candi is a tool maintained by the deal.II community to install deal.II and
  many of the optional dependencies on a linux-like system from source and is
  quite useful to run on clusters where you typically need to install
  packages manually.
  See https://github.com/dealii/candi
