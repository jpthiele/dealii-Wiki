 Several docker images with full installations of deal.II and (almost) all its dependencies are
available on https://hub.docker.com/r/dealii/dealii/.

Under the section `tags`, you'll find all available configurations. 

All libraries and dealii are installed under `/home/dealii/libs`. The default user in the image is `dealii`.

These images can be used, for example, in the following way:
~~~
# Pull desired tag
TAG=dealii/dealii:dealii:v8.5.0-gcc-mpi-fulldepsmanual-debugrelease
docker pull $TAG
docker tag $TAG dealii

# Now run inside the image, sharing the current directory
docker run --rm -t -i -v `pwd`:/home/dealii/app dealii
dealii@3c1bb6ff22b4:~$
~~~
the prompt `dealii@3c1bb6ff22b4:~$` is inside the docker image. For example, if you type
~~~
dealii@3c1bb6ff22b4:~$ export  | grep DIR
declare -x ARPACK_DIR="/home/dealii/libs/arpack-3.4.0"
declare -x DEAL_II_DIR="~/dealii-v8.5.pre.4"
declare -x HDF5_DIR="/home/dealii/libs/hdf5-1.10.0-patch1"
declare -x METIS_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x MUMPS_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x OCE_DIR="/home/dealii/libs/oce-0.17.2"
declare -x OPENCASCADE_DIR="/home/dealii/libs/oce-0.17.2"
declare -x P4EST_DIR="/home/dealii/libs/p4est-1.1"
declare -x PARMETIS_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x PETSC_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x SCALAPACK_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x SLEPC_DIR="/home/dealii/libs/slepc-3.7.3"
declare -x SUPERLU_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x SUPERLU_DIST_DIR="/home/dealii/libs/petsc-3.7.4"
declare -x TRILINOS_DIR="/home/dealii/libs/trilinos-12-8-1"
dealii@3c1bb6ff22b4:~$ 
~~~
you will see what was installed where in the image.

These images are guaranteed to work identically on Mac OS, Linux, Windows, on Travis CI, and on gitlab CI. 

Here you can find an example `.travis.yml` that uses one of the available images to test a user application using a controlled environment:

~~~
sudo: required

env:
  - BUILD_TYPE=Release
  - BUILD_TYPE=Debug

services:
  - docker

notifications:
  email: false

language: C++

before_install:
- docker pull dealii/dealii:v8.5.0-gcc-mpi-fulldepsmanual-debugrelease

script:
- export DOCKER_RUN="docker run -P -v `pwd`:/home/dealii/app:rw dealii/dealii:v8.5.0-gcc-mpi-fulldepsmanual-debugrelease  /bin/sh -c"
- $DOCKER_RUN "test -d app/build-travis && rm -rf app/build-travis; mkdir app/build-travis; cd app/build-travis; cmake -GNinja ../; ninja"
- $DOCKER_RUN "cd app/build-travis; ctest -N; ctest -V"
~~~

Gitlab works differently, allowing you to directly run the tester inside your custom image. An example `.gitlab-ci.yml` to use with your own applications is here:

~~~
image: dealii/dealii:v8.5.0-gcc-mpi-fulldepsmanual-debugrelease

before_script:
  - ./scripts/check_indentation.sh

debug:
  script:
   - test -d build_linux_debug && rm -rf build_linux_debug
   - mkdir build_linux_debug; cd build_linux_debug; cmake .. -GNinja -DCMAKE_BUILD_TYPE=Debug; ninja
   - ctest -N; ctest -V; cd ..

release:
  script:
   - test -d build_linux_release && rm -rf build_linux_release
   - mkdir build_linux_release; cd build_linux_release; cmake .. -GNinja -DCMAKE_BUILD_TYPE=Release; ninja
   - ctest -N; ctest -V; cd ..
~~~

The above will create two `pipelines`, in gitlab terminology, and run a `Debug` and `Release` ctest.