# Using deal.II with the Linux subsystem (Windows 10 64bit only)

Windows 10 has gained a compatibility layer for running Linux binaries
natively on Windows. You can find more information on the
[Wikipedia page](https://en.wikipedia.org/wiki/Windows_Subsystem_for_Linux).

In the following section a detailed HowTo is given to install the subsystem
and a Linux distribution on top of it. Our choice at hand is [Debian
GNU/Linux](https://www.microsoft.com/en-us/store/p/debian-gnu-linux/9msvkqc78pk6)
because it already contains the latest deal.II release in binary form.
(<b>Note:</b> The same is true for the Ubuntu distribution.)

## Installing the subsystem and Debian GNU/Linux

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
   (<i>TODO: replace with `testing`</i>)

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

## Installing the deal.II library and tools

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
   ```

   Now, exit the root account:
   ```console
   root@computer# exit
   user@computer$
   ```

2. Do a quick "smoke test" whether everything installed fine by compiling
   and running the first example step:
   ```console
   ```

(<i>TODO: explain how to install tools </i>)

## Workflow

(<i>TODO: Explain workflow </i>)



# Using deal.II on native Windows

For an overview of different ways to use deal.II on Windows have a look at
the corresponding [FAQ entry](https://github.com/dealii/dealii/wiki/Frequently-Asked-Questions#can-i-use-dealii-on-a-windows-platform).

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
