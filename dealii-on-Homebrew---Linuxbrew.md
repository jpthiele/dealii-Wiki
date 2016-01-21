# Using deal.II on Mac OS X and Linux via Homebrew/Linuxbrew

---
**The most up-to-date instructions can be found at the [homebrew-dealiisuite tap](https://github.com/davydden/homebrew-dealiisuite).**

---


The deal.II suite is also available on Homebrew ( OS-X) and Linuxbrew (https://github.com/Homebrew/linuxbrew Linux).

## Installing Homebrew/Linuxbrew

### Homebrew
```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
### Linuxbrew
On Debian/Ubuntu on needs extra packages:
```
sudo apt-get install build-essential curl git m4 ruby texinfo libbz2-dev libcurl4-openssl-dev libexpat-dev libncurses-dev zlib1g-dev csh subversion
```

If you will be installing `opencascade`, you also need
```
sudo apt-get install libfontconfig1-dev libfreetype6-dev libx11-dev libxcursor-dev libxext-dev libxfixes-dev libxft-dev libxft-dev libxi-dev libxrandr-dev libxrender-dev
```
to be able to `brew install qt` and 
```
sudo apt-get install libgl2ps-dev tcl8.6-dev tk8.6-dev libgl1-mesa-dev libglu1-mesa-dev libxmu-dev libxext-dev
```
to build `opencascade` itself.

Then

```
git clone https://github.com/Homebrew/linuxbrew.git ~/.linuxbrew
```

Add to your `.bashrc` or `.zshrc`:
```
export PATH="$HOME/.linuxbrew/bin:$PATH"
export MANPATH="$HOME/.linuxbrew/share/man:$MANPATH"
export INFOPATH="$HOME/.linuxbrew/share/info:$INFOPATH"
```
## Installing deal.II suite
In order to install deal.II suite, one needs to add Homebrew Science https://github.com/Homebrew/homebrew-science - additional repository 
of scientific software - by running 
```
brew tap homebrew/science
```
Before trying the instructions below make sure that your Homebrew/Linuxbrew is up-to-date 
by running `brew update`.
### OS-X
Note that this uses the system BLAS/LAPACK libraries.
```
brew install cmake
brew install openmpi --c++11
brew install boost --c++11
brew install gsl
brew install scalapack
brew install mumps
brew install metis
brew install parmetis
brew install hypre
brew install superlu43
brew install superlu_dist
brew install arpack --with-mpi --without-check
brew install hdf5 --with-mpi --c++11
brew install netcdf --with-fortran --with-cxx-compat
brew install suite-sparse
brew install hwloc
brew install sundials --with-mpi
brew install fftw --with-mpi --with-fortran
brew install petsc
brew install slepc
brew install p4est
brew install adol-c
brew install cppunit
brew install doxygen --with-graphviz
brew install glpk
brew install glm
brew install trilinos
brew install dealii
```

### Linux
On Linux some of the packages do not currently compile, therefore they have to be skipped (thus `--without-XYZ` arguments). Otherwise, the steps are pretty much equivalent to those listed above.

Note that this configuration uses OpenBLAS as opposed to the system BLAS/LAPACK libraries (such an option is currently not supported).
```
brew install pkg-config
brew install cmake
brew install openmpi --c++11
brew install boost --with-mpi --without-single
brew install gsl
brew install openblas
brew install scalapack --with-openblas --without-check
brew install mumps --with-openblas
brew install metis
brew install parmetis
brew install hypre --with-openblas
brew install superlu43 --with-openblas
brew install superlu_dist  --with-openblas
brew install arpack --with-mpi --with-openblas
brew install hdf5 --with-mpi --c++11
brew install netcdf --with-fortran --with-cxx-compat
brew install suite-sparse --with-openblas
brew install hwloc
brew install sundials --with-mpi
brew install fftw --with-mpi --with-fortran
brew install petsc --with-openblas
brew install slepc --without-check
brew install p4est --with-openblas
brew install adol-c
brew install cppunit
brew install doxygen --with-graphviz
brew install glpk
brew install glm
brew install trilinos --with-openblas --without-scotch --without-x11
brew install dealii --without-opencascade --with-openblas
```

Possible failures:

If `netcdf` fails, you may need to install it via something like `LD_LIBRARY_PATH=~/.linuxbrew/Cellar/netcdf/4.3.3.1/lib brew install netcdf --with-fortran --with-cxx-compat`. 
See https://github.com/Homebrew/homebrew-science/issues/2521#issuecomment-122032005 for a discussion.