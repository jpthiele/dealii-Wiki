## Overview

Based on your version of Ubuntu and Debian, the default packaged version of deal.II varies. We also have backports that bring newer version to older distributions. Here is the overview:

- Ubuntu 22.04: default: 9.3.2, supported backports: 9.4.0
- Ubuntu 20.04: default: 9.1.1, supported backports: 9.4.0, 9.3.2, 9.2.0
- Ubuntu 18.04: default: 8.5.1, supported backports: 9.2.0

- Debian 12 (bookworm, testing): 9.4.0
- Debian 11 (bullseye, stable): 9.2.0

## installing the default version packaged with your system:
```
apt-get install libdeal.ii-dev
```


## installing a backport (replace XXX with the version listed above):

```
sudo -i

export REPO=ppa:ginggs/deal.ii-XXX-backports

apt-get update && apt-get install -y software-properties-common
add-apt-repository $REPO
apt-get update
apt-get install libdeal.ii-dev
apt-get install build-essential cmake ninja-build gdb git-core
```