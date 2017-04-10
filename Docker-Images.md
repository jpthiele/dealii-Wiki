 Several docker images with full installations of deal.II and (almost) all its dependencies are
available on https://hub.docker.com/r/dealii/dealii/.

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