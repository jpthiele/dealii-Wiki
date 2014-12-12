# A simple script and directory structure for running multiple build tests

# Running build tests

Running build tests with many different configurations is a vital part of testing the deal.II library. In particular before new releases, running on as many platformas as possible is important to make sure the new release is not broken.

Therefore, we ask/encourage/beg everybody to check out the [build test page](http://www.dealii.org/cgi-bin/build.pl) and see if they have a configuration not listed there. In particular, operating systems and compilers are something the developers cannot provide in a wide range.

If you decided to participate in build tests regularly, we provide a simple structure below, which helps you setting up these texts on a Unix based system (e.g. Linux, Mac OS X, Cygwin)

# The code

## Directory structure

Create a directory, say `deal-test`, with the following entries:

```
deal-test
--> branches
--> configurations
--> log
```

### The `branches` subdirectory

In `branches` for instance do
```
svn checkout https://svn.dealii.org/trunk/deal.II trunk
```
or
```
svn checkout https://svn.dealii.org/branches/my_branch_name/deal.II my_branch
```

You can have as many branch checkouts there as you like.

Now, we have expanded the structure to
```
deal-test
--> branches
----> trunk
----> my_branch
--> configurations
--> log
```

### The `configurations` subdirectory

Here, you can store configuration options in files named `**.conf`, which will be processed for each branch in `branches`.

You can find an example configuration file [here](http://www.dealii.org/developer/development/Config.sample). It is not necessary to configure anything - an emtpy configuration file is equivalent to invoke `cmake` without any options.

Now, the directory structure is extended to
```
deal-test
--> branches
----> trunk
----> my_branch
--> configurations
----> disable-all.conf
----> fortran-libs.conf
----> fortran-mpi-multi.conf
--> log
```

What's missing is the
### Build test script

This script expects to be called inside your `deal-test` directory. After the directory structure has been set up as above, there is just one configuration option left, namely the directory where the libraries will be built and discarded again. This is set in the variable `TMPDIR` at the very top of the script.

```
#!/bin/bash
# set -e
# set -u

TMPDIR=/tmp

echo Starting script at `date`
dir=`pwd`

for branch in branches/**; do
  echo "Starting branch $branch"

  pushd $branch > /dev/null
  echo updating in `pwd`
  svn update
  popd > /dev/null

  for file in configurations/**; do
    pushd $branch > /dev/null
    echo "Run build_test with configuration $file `date`"
    ./contrib/utilities/build_test LOGDIR="$dir/log" CONFIGFILE="$dir/$file" MAKEOPTS="-j2"
    popd > /dev/null

    basename=$(basename $branch).$(basename $file)
    logfile=$(ls -t1 log/$basename** | head -n 1)
    echo "Mailing  " $branch $logfile `date`
    echo $branch $basename $logfile
    /usr/sbin/sendmail dealii.build.tests@gmail.com < $logfile
    1. or and equivalent..
    1. ssh mail mail dealii.build.tests@gmail.com < $logfile
  done
done
```

Copy this file into `deal-test/run-tests.sh` and everything is set up.

You can just call
```
bash run-tests.sh
```
or you can add a line of the form
```
22 22 ** ** * (cd ~/deal-test; /bin/bash ~/deal-test/run-tests.sh >> ~/deal-test/LOG 2>&1 )
```
to your `crontab`.

# Enjoy!
