This documents the testing infrastructure and who it is administrating it.

# Continuous Integration

All pull-requests on github are tested with the following machines and their status is reported on the pull request itself:

1. **indentation** (name: ``continuous-integration/travis-ci/pr``): Checks the indentation by running ``./contrib/utilities/check_indentation.sh`` and checking that there are no changes detected. Runs on https://travis-ci.org/. Maintained by: deal.II developers. Configuration stored in ``/.travis.yml``.

2. **CI** (running on https://jenkins.tjhei.info/job/dealii/ and controlled by ``./Jenkinsfile``). This tests indentation and (currently) build with and without MPI and runs a selection of tests. See the Jenkinsfile for details and https://github.com/tjhei/candi/tree/docker#candi-docker-images for the Docker images used. Maintained by: [@tjhei](https://github.com/tjhei)

3. **OSX** (running on https://jenkins.tjhei.info/job/dealii-OSX/ and controlled by ``./contrib/ci/Jenkinsfile.osx``). Compiles under OSX. Maintained by: [@tjhei](https://github.com/tjhei)

4. **tidy** (running on https://jenkins.tjhei.info/job/dealii-tidy/ and controlled by ``./contrib/ci/Jenkinsfile.tidy``). Runs clang-tidy (by running ``./contrib/utilities/run_clang_tidy.sh``). Maintained by: [@tjhei](https://github.com/tjhei)

Note: PRs are not run automatically but need to be approved by one of the developers by adding the label "ready to test" to the PR. From now on, every push to this PR will cause a new build. To build the original build or to rebuild the last commit (maybe because the tester crashed), trigger a new build by leaving a comment containing the text ``/rebuild``.

# Regression testing

In addition to testing pull requests, we also continuously have machines execute the current master branch with different configurations. The results are reported at https://cdash.43-1.org and documentation can be found at https://www.dealii.org/developer/developers/testsuite.html#build_tests

We currently have the following machines running:

1. **tester**: The only machine in the "Regression tests" category on cdash. Many configurations. Located in Minneapolis and maintained by [@tamiko](https://github.com/tamiko)
2. **simserv04**: One of the machines in the "Continuous" category on cdash. Some older configurations. Located in Heidelberg and maintained by [@tamiko](https://github.com/tamiko)
3. **simserv02**: One of the machines in the "Continuous" category on cdash. Running the Intel compiler. Maintained by [@masterleinad](https://github.com/masterleinad)
4. **davyddenubuntu**: One of the machines in the "Continuous" category on cdash. A configuration with all dependencies but CUDA enabled and built via [Spack](https://github.com/dealii/dealii/wiki/deal.II-in-Spack) with and without optimization flags on Ubuntu 16.04 with GCC 5.4.0. Maintained by [@davydden](https://github.com/davydden)
5. **CUDA10** : One of the machines in the "Continuous" category on cdash. Configuration with CUDA 10. Maintained by [@Rombur](https://github.com/Rombur)
