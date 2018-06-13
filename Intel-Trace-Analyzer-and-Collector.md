# Using Intel Trace Analyzer and Collector (work-in-progress)

![itac_example](https://user-images.githubusercontent.com/8023934/41157662-df8546e2-6b26-11e8-8fd2-1ad69ea56605.png)

##  links

- https://software.intel.com/en-us/intel-trace-analyzer/documentation 
- https://software.intel.com/en-us/itc-user-and-reference-guide-user-guide 
- http://www.prace-ri.eu/IMG/pdf/WP237.pdf 
- https://software.intel.com/en-us/intel-trace-analyzer-support/training

similar tools
- http://ipm-hpc.sourceforge.net
- http://hpctoolkit.org

## Usage Procedure

Below we consider the case when a user/developer would like to manually measure non-MPI timing of certain regions of the deal.II library and/or user code. This necessitates extra include and link flags.

### 1.a) Build deal.II

```
module load intel64/18.0up02 itac/2018up02 cmake git
cmake ../ -DCMAKE_CXX_FLAGS:STRING="-g -trace -O2 -march=native" -DDEAL_II_LINKER_FLAGS="-trace" -DDEAL_II_INCLUDE_DIRS="/apps/intel/ComposerXE2018/itac/2018.2.020/include" -DDEAL_II_DEFINITIONS="USE_VT"
```
Notes:
* Avoid specifying MPI libraries manually, this ruins link sequence set up by compiler flag `–trace`.
* adjust `intel` modules to fit your setup.

Now you can add timers to required parts of the library
```
#ifdef USE_VT
#include <VT.h>
#endif
...
#ifdef USE_VT
  VT_Region region("my_name", “my_group", __FILE__, __LINE__);
#endif
<some-function>
#ifdef USE_VT
  region.end();
#endif
```

Finally build the library
```
make all -j20
```

### 1.b) Build downstream library/project

Build the user project/library as usual
```
cmake ../ -DDEAL_II_DIR=/path/to/dealii
make all -j20
```
Similar to the above, you can manually add timers to certain parts of the project.

### 2. Prepare a configuration file  `trace.conf`
```
# Log file
LOGFILE-NAME my_trace.stf
LOGFILE-FORMAT STF
# disable all MPI activity
ACTIVITY MPI OFF
# enable all bcasts, recvs and sends
SYMBOL MPI_WAITALL ON
SYMBOL MPI_IRECV ON
SYMBOL MPI_ISEND ON
SYMBOL MPI_BARRIER ON
SYMBOL MPI_ALLREDUCE ON
# enable all activities in the Application class 
ACTIVITY Application ON
# 
STATE Application:* UNFOLD
STATE lib*:* FOLD
STATE libdeal*:* UNFOLD
```

### 3. Run the application (usually by submitting an interactive job to the queue)

```
qsub -l nodes=1:ppn=40,walltime=01:00:00 -I
module load intel64/18.0up02 itac/2018up02
export VT_CONFIG=/path/to/trace.conf
mpirun -trace -np 20 my_executable
```

You should see at the end: 
```
[0] Intel(R) Trace Collector INFO: Writing tracefile….
```

### 4. GUI to analyze:
```
$ module load itac/2018up02
$ traceanalyzer my_trace.stf
```