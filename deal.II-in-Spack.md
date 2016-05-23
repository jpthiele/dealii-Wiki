# Using deal.II on Mac OS X and Linux via Spack

The deal.II suite is also available on Spack (https://github.com/LLNL/spack).

## Install and configure Spack

Add the following to `~/.bash_profile` (or equivalent)
```
SPACK_ROOT=/path/to/spack
export PATH=$SPACK_ROOT/bin:$PATH
```
`SPACK_ROOT` is destination where you want Spack to be installed (i.e. `~/spack`).

Now clone Spack
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
Now `DEAL_II_DIR` environment variable should be set appropriately and `cmake` executable will be available in path:
```
$echo $DEAL_II_DIR
$which cmake
```

## Install GCC
OSX by default does not provide any Fortran compiler.
One can get it from GCC

```
spack install gcc
```

Load `gcc` and let Spack find the newly installed compiler:
```
spack load gcc
spack compiler find
```
Now you can install deal.II with `gcc`
```
spack install dealii%gcc
```

## Mixing GCC and Clang on OSX

At the time of writing this page, Spack does not provide a native way to mix C/C++ and Fortran compilers from different families. However, that can be done by manually changing few lines of `python` code as described below:

(i) Edit `~/.spack/compilers.yaml` to provide path to gfortran compiler within the `clang@x.y.z-apple` entry:
```
compilers:
  darwin-x86_64:
    clang@7.3.0-apple:
      cc: /usr/bin/clang
      cxx: /usr/bin/clang++
      f77: /path/to/bin/gfortran
      fc: /path/to/bin/gfortran
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