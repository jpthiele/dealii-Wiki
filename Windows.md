This wiki page contains information about how to use deal.II on the
 - [Windows subsystem for Linux](#using-dealii-with-the-windows-subsystem-for-windows)
 - [Using deal.II on native Windows](#using-dealii-on-native-windows)
For an overview of different ways to use deal.II on Windows have a look at
the corresponding [FAQ entry](https://github.com/dealii/dealii/wiki/Frequently-Asked-Questions#can-i-use-dealii-on-a-windows-platform).

# Using deal.II with the Windows Subsystem for Linux

**Warning: please be aware that the following is experimental and you will
likely encounter bugs in compilers and deal.II itself. Only continue if you
are willing to experiment.**

Windows 10 has gained a compatibility layer for running Linux binaries
natively on Windows. You can find more information on the
[Wikipedia page](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux).

In the following section a detailed HowTo is given to install the subsystem
and a Linux distribution on top of it. Our choice at hand is [Debian
GNU/Linux](https://www.microsoft.com/en-us/store/p/debian-gnu-linux/9msvkqc78pk6)
because it already contains the latest deal.II release in binary form.
(<b>Note:</b> The same is true for the Ubuntu distribution.)

## (Required) Installing the subsystem and Debian GNU/Linux

Have a look at the excellent documentation about the Linux subsystem on the
[Windows help pages](https://docs.microsoft.com/en-us/windows/wsl/install-win10)

1. (As described in detail on the Windows help pages, we first have to
   install the subsystem. For this, locate the Windows PowerShell in your
   Start menu (`Start` -> `Windows PowerShell`), right click on `Windows PowerShell`
  -> `More` -> `Run as administrator`

2. Install the subsystem by using the following command
   ```console
   Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
   ```
   and restart

3. Open the Windows Store and search for "Debian", and install "Get Debian
   GNU/Linux". When finished start the application. You will be prompted to
   enter a user name and password.

4. Switch to the "root" account by running
   ```console
   user@computer% sudo -s
   ```
   Enter the password that you used in step 3.

5. Edit the package manager configuration by using nano (or an editor of
   your choice):
   ```console
   root@computer# nano /etc/apt/sources.list
   ```
   You should see three lines. If they contain the release name `stretch`,
   remove all three lines and replace them with a single line:
   ```
   deb http://deb.debian.org/debian buster main contrib non-free
   ```

6. Now update/upgrade the system by running
   ```console
   root@computer# apt update
   [...]

   root@computer# apt dist-upgrade
   [...]
   Do you want to continue? [Y/n] <Enter>
   [...]

   root@computer# apt autoremove
   [...]
   Do you want to continue? [Y/n] <Enter>
   [...]
   ```

## (Required) Installing the deal.II library and tools

We continue the installation process by installing the deal.II library with
development headers and documentation. The packages in Debian (or Ubuntu)
are called `libdeal.ii-dev` and `libdeal.ii-doc`:

1. As root user (see above) run:
   ```console
   root@computer# apt install libdeal.ii-dev libdeal.ii-doc
   [... long list ...]
   0 upgraded, 443 newly installed, 0 to remove and 0 not upgraded.
   Need to get 441 MB of archives.
   After this operation, 2,016 MB of additional disk space will be used.
   Do you want to continue? [Y/n] <Enter>
   [...]
   ```

   At this point, let us install a number of useful, additional tools:
   ```console
   root@computer# apt install build-essential cmake ninja-build gdb clang clang-format
   [...]
   Do you want to continue? [Y/n] <Enter>
   [...]
   ```

   If you plan to use graphical tools, a number of useful programs are:
   ```console
   root@computer# apt install xterm gnuplot
   [...]
   Do you want to continue? [Y/n] <Enter>
   [...]
   ```

   If you plan to use MSVC, you will also need to install ssh, zip and
   unzip:
   ```console
   root@computer# apt install ssh zip unzip
   [...]
   Do you want to continue? [Y/n] <Enter>
   [...]
   ```
   You will also need to install git:
   ```console
   root@computer# apt-get install git-core
   [...]
   Do you want to contiue? [Y/n] <Enter>
   [...]
   ```
   Now, exit the root account:
   ```console
   root@computer# exit
   user@computer$
   ```

2. Do a quick "smoke test" whether everything installed fine by compiling
   and running the first example step:
   ```console
   user@computer$ cd
   user@computer$ cp -r /usr/share/doc/libdeal.ii-doc/examples/step-55 .
   user@computer$ cd step-55
   user@computer$ cmake .
   user@computer$ make release
   user@computer$ make run
   [...]
   [100%] Built target run
   ```

## (Recommended) Installing an X server

In order to run graphical applications from within the Linux Subsystem a
so-called X server has to be installed. This step is in particular
necessary, if you plan to install
[Eclipse](https://github.com/dealii/dealii/wiki/Eclipse), or
[KDevelop](https://github.com/dealii/dealii/wiki/KDevelop) via the Linux
subsystem.

1. Download and install [xming](https://sourceforge.net/projects/xming/).

2. Start xming. A styliced X should appear in the task bar.

3. Open a Linux terminal and try to run xterm:
   ```console
   user@computer$ export DISPLAY=:0
   user@computer$ xterm
   ```
   This should spawn a new window with a shell. Simply close the shell
   again.

4. In order to avoid to have to export `DISPLAY=:0` every single time, it
   is convenient to append
   ```
   export DISPLAY=:0
   ```
   to the end of the `.bashrc` file.

You should now be able to proceed and run all graphical and command lines
tools that are mentioned in the documentation and video lectures about
deal.II.


## (Optional) Installing Microsoft Visual Studio Community Edition

This step is optional and only needed if you intent to use MSVC for code
development. (Great alternatives are
[Eclipse](https://github.com/dealii/dealii/wiki/Eclipse), or
[KDevelop](https://github.com/dealii/dealii/wiki/KDevelop).)

1. Go to the [Microsoft website](https://www.visualstudio.com/downloads)
   and download Microsoft Visual Studio Community Edition

2. Launch the web installer. Make sure to select "Linux development with C++"

3. Restart.

### Create MSVC project

Next, let us create a small example project with MSVC. First, you have to
decide where the MSVC project shall be located. For this example we will
use the directory `workspace` in the (Windows) documents directory of the
current user located on driver C.  The corresponding path to access this
directory from Linux is `/mnt/c/Users/<user>/Documents/workspace`. (Substitute
`<user>` with your Windows username in the following console listings!)

1. Copy an example step to the Windows user directory. For this, start the Linux
   terminal again and `cd` to the user directory and copy and example step:
   ```console
   user@computer$ mkdir -p /mnt/c/Users/<user>/Documents/workspace
   user@computer$ cd /mnt/c/Users/<user>/Documents/workspace
   user@computer$ cp -r /usr/share/doc/libdeal.ii-doc/examples/step-6 .
   user@computer$ cd step-6
   user@computer$ cmake .
   ```

2. (Only once) Download a script for generating Visual C++ Linux project files:
   ```console
   user@computer$ cd /mnt/c/Users/<user>/Documents/workspace
   user@computer$ git clone https://github.com/robotdad/vclinux
   ```

3. Generate the Visual C++ Linux project file:
   ```console
   user@computer$ cd /mnt/c/Users/<user>/Documents/workspace/step-6
   user@computer$ ../vclinux/bash/genvcxproj.sh . step-6.vcxproj
   ```

4. Start the sshd server:
   ```console
   root@computer$ sudo service ssh start
   ```
   Make sure to keep the terminal open and the sshd server running while working in Visual Studio

5. Open Visual Studio and navigate to `Tools` -> `Options` -> `Cross Platform` -> `Connection Manager`
    * The Host name should be 'localhost'
    * The Port should be 22
    * The User Name and Password are the ones you used to set up Debian
    * Hit okay and wait for Visual Studio to run through setting up the connection

6. Configure the project in Visual Studio
    * Open the project file `c:\Users\<user>\Documents\workspace\step-6.vcxproj` in Visual Studio.
    * In the `Solution Explorer` right-click on the project and select `Properties`
    * Go to the `Debugging` page and set `Program` to `/mnt/c/Users/<user>/Documents/workspace/step-6/step-6`.
    * Go to the `Remote Build` page and set `Build Command Line` to `cd /mnt/c/Users/<user>/Documents/workspace/step-6/; cmake .; make`.
    * Go to `General` and set `Remote Build Root Directory` to `/mnt/c/Users/<user>/Documents/workspace` and `Remote Build Project Directory` to `/mnt/c/Users/<user>/Documents/workspace/step-6/`
    * Warning! Windows is very accepting of paths that include space characters, but you will run into errors at this step if you use a path with a space character. At this time, escape characters in the above steps will not work. Thus, you must find the short path name for your file location. I recommend using [these](https://superuser.com/a/728792) steps to find the short path name.

7. Run the executable via `Debug` -> `Start Debugging` (or press `F5`) and celebrate!

# Using deal.II on native Windows

**Warning: please be aware that the following is experimental and you will
likely encounter bugs in compilers and deal.II itself. Only continue if you
are willing to experiment.**

## Visual Studio

Since deal.II 8.4.0 we have experimental support for the newer Visual
Studio C++ compilers (2017 or newer), but this is still work in progress.
You can check the current development status
[here](https://github.com/dealii/dealii/issues/1921)

Installation instructions:

1. Download and install Visual Studio 2017:
   https://www.visualstudio.com/vs/ and make sure you select the C++
   compiler
2. Install cmake from https://cmake.org/download/ (pick the windows
   installer)
3. Extract deal.II to a folder, for example c:\dealii (or clone the git
   development version)
4. Configure using cmake by opening the 64bit command line shortcut and
   run:

   ```
   set PreferredToolArchitecture=x64
   cd c:\dealii
   mkdir build
   cd c:\dealii\build
   cmake -G "Visual Studio 15 2017 Win64" ..
   ```
   <b>Note:</b> Setting the tool architecture to 64 bit works around problems of
   the compiler or linker running out of memory and leads to much quicker
   compile times.
   <b>Note:</b> Use generator ``"Visual Studio 15 2017 Win64"`` for Visual Studio
   2017.

6. Compile and install the library by opening ``deal.II.sln`` in
   c:\dealii\build, pick the install target and compile. Note: you need to
   either compile in the same terminal as above (using ``cmake --build .``)
   or open ``devenv.exe`` from the same terminal, to use the 64 bit tool
   architecture.

7. in cmd go to one of the examples in c:\dealii\examples\step-xy:
   ```
   cmake -D DEAL_II_DIR=c:\dealii\build -G "Visual Studio 15 2017 Win64" .
   ```

8. Open the newly created solution (step-xy.sln) in that directory and
   compile/run/debug.

## Running build tests on Windows:

Install git and mingw (for perl etc). Then create a .bat file:
```
git pull origin master
rmdir /Q /S buildtest17
mkdir buildtest17
cd buildtest17
ctest -C Debug -DMAKEOPTS="/m:1" -DCTEST_CMAKE_GENERATOR="Visual Studio 15 2017" -S ../tests/run_buildtest.cmake -V
cd ..
```

## Cygwin / MingGW

Cygwin and forks such as MinGW and MinGW-64 are unsupported due to multiple
unresolved miscompilation issues.

## Other Windows compilers

We haven't had much success with any other compiler on Windows (Intel,
Borland, ...).
