## Overview

Based on your version of Ubuntu and Debian, the default packaged version of deal.II varies. We also have backports that bring newer version to older distributions. Here is the overview:

- Ubuntu 24.04: default: 9.5.1, supported backports: 9.6.0
- Ubuntu 22.04: default: 9.3.2, supported backports: 9.6.0, 9.5.1, 9.4.0
- Ubuntu 20.04: default: 9.1.1, supported backports: 9.5.1, 9.4.0, 9.3.2, 9.2.0
- Ubuntu 18.04: default: 8.5.1, supported backports: 9.2.0
- Debian 13 (trixie): 9.5.1
- Debian 12 (bookworm): 9.4.1
- Debian 11 (bullseye): 9.2.0

## Installing the default version packaged with your system:
```
apt-get install libdeal.ii-dev
```


## Installing a backport (replace XXX with the version listed above):

```
sudo -i

export REPO=ppa:ginggs/deal.ii-XXX-backports

apt-get update && apt-get install -y software-properties-common
add-apt-repository $REPO
apt-get update
apt-get install libdeal.ii-dev
apt-get install build-essential cmake ninja-build gdb git-core
```

## Installing the tutorial steps

To install the tutorial steps, proceed with an installation of the documentation package:

```
apt-get install libdeal.ii-doc
```

The tutorial steps are located in the `/usr/share/doc/libdeal.ii-doc` directory. To copy a particular tutorial step into the current folder and run it, proceed with the following commands (for example, step-1):

```
$ cp -r /usr/share/doc/libdeal.ii-doc/examples/step-1 .
$ cd step-1
$ cmake .
$ make run
```
