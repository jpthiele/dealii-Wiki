# The Eclipse Integrated Development Environment and how to set up deal.II in it

# About Eclipse

[Eclipse](http://www.eclipse.org) is, today, the integrated development
environment (IDE) that is likely most widely used around the world. It was
originally developed for large-scale, industrial projects written in Java, but
it has since been extended by plugins for virtually any other programming
language, as well as many many modeling tools (for example, to name just one,
for UML diagrams). For the purpose of deal.II, the C++ plugin (the
[Eclipse CDT](http://www.eclipse.org/CDT)) is the basis for the steps described below. The advantages of using a modern IDE described at the top of the page on [are equally valid for Eclipse, or course.

Eclipse with the C++ plugin included can be downloaded from the download page
available via http://www.eclipse.org/ by choosing "Eclipse IDE for C/C++
Developers". You will have to specify the operating system from the "download links"
list to get the proper binaries for your operating system.
Alternatively, a generic Eclipse can be used, but the Eclipse CDT (C/C++
Development Tools, the C++ plugin for Eclipse) has then to be installed
separately.c Eclipse offers automatic `git` integration: just clone a git repository and add it as a project.

Using an integrated development environment such as Eclipse is difficult to
learn by just reading something about it. It's best learned by <i>doing</i> or
watching someone else do it. To this end, take a look at training videos 7, 8,
8.01, and 25 at https://www.math.colostate.edu/~bangerth/videos.html .

# A note upfront on using Subversion and Eclipse

If you will want to use subversion or some other version control system for the project you are about to set up, you first need to install the Eclipse plugin that can handle this -- installing the plugin later will not convert the project into one that uses subversion, even if the directories in which it is located have previously been created by subversion. See below about how to install the subversion plugin.

# General Eclipse settings

The settings file `eclipse.ini` is located right next to Eclipse executable. You might want to increase heap size by changing the following options:
* `-Xms256m`
* `-Xmx2048m`

The numbers indicate heap size in megabytes.

If you do not like the built-in dark theme of Eclipse, you can try installing plugins for a better experience: https://marketplace.eclipse.org/category/free-tagging/theme
- Darkest Dark Theme: https://marketplace.eclipse.org/content/darkest-dark-theme-devstyle
- Spectrum theme: https://marketplace.eclipse.org/content/eclipse-spectrum-dark-theme

After installing the plugin go to `Window`->`Preferences` and search for `Appearance`.


# Setting up a workspace in Eclipse using cmake4eclipse

The following instructions are based on this answer: https://stackoverflow.com/questions/9453851/how-to-configure-eclipse-cdt-for-cmake/38716337#38716337

From the Eclipse menu choose `Help`->`Eclipse Market place...` and search for `cmake4eclipse`, install it.
First, add deal.II itself as a project. This will force the Eclipse indexer to go through the entire library.  Go to `File`->`New`->`C/C++ Project`. Choose `Empty or Existing CMake Project`. Click `Next`, then uncheck `Use default location`, click `Browse` and navigate to the deal.II source directory (or just type the patch to deal.II source dir). Fill `Project name` and click `Finish`. After deal.II is added as a project indexer starts working, it may take a long time for it to finish. Fortunately, it is done only on newly added projects.

Next, add your project in the same way. After that, right-click on the new project, go to `Properties`, `Project references` and add deal.II as a reference. If references are not resolved automatically, go to `Project`->`C/C++ index`->`Rebuild`.

If you set up your project this way, DO NOT use CMake built-in generators, as it will modify the Eclipse project files in the source directory, likely resulting in conflicts and confusing the indexer. 

## Alternative way

After installing cmake4eclipse go to  `File`->`New`->`Makefile project with existing code`, enter the deal.II source location and select `CMake driven` from `Toolchain for indexer Settings`. After setting up the deal.II as a project you may want to add the location of STD headers at `Project`-> `Properties`-> `C/C++ General`->`Patch and Symbols` and click `Add`. You can figure out where the headers are following instructions from this thread: https://stackoverflow.com/questions/344317/where-does-gcc-look-for-c-and-c-header-files

Do the same thing with your deal.II-based project and add deal.II in `Project references`.

# Setting up a project depending on deal.II using CMake built-in generators


The easiest way to get Eclipse to understand your project is to let CMake generate an Eclipse project for you, assuming you are using CMake. To this end, go to the source directory (using the `step-22` example again) and say
```
  cmake -G"Eclipse CDT4 - Unix Makefiles" -DDEAL_II_DIR=/path/to/deal.II .
```
This generates not only a Unix Makefile, but also an Eclipse project file. You can get Eclipse to use it by selecting from the menu "File > Import...", then choosing "General > Existing Projects into Workspace". Click on "Next", and in the next dialog window select as the root directory the directory where you ran CMake above. After selecting this directory, Eclipse should show you the name of the project under "Projects". Simply click on "Finish" to let Eclipse choose it. It will then automatically know where to find header files, about build targets, etc.

To build the executable, go to the "Make target" tab (usually located in the right sub-window) and double-click on "all". In the "Console" tab (usually located in the bottom sub-window) you can view the commands being executed to build your program.

You may still have to set up, run and debug launches if you want to run or debug the executable so built, however.



# Setting up a project using deal.II by hand

After starting Eclipse for the first time, you will be asked to choose a workspace. Accept the default. Then, you
get a screen that provides you with a number of introductory options such as a
tutorial, the help pages, etc. Take a look at these or click on the icon to
get to the Workspace right away (where you will also land when calling Eclipse
for the second or later times). Then follow these steps, shown below taking the
step-22 tutorial as an example of any free-standing program that uses deal.II
(these steps are also demonstrated in video #7 linked to above):

  - Create the Eclipse project:
    - From the main menu bar select: "File > New > Makefile Project with Existing Code" (alternatively, you can use the right-click context menu in the "Project Explorer" sub-window on the left)
    - Browse for the directory which has the code of your project, `examples/step-22` in this example, select "Linux GCC" as the "Toolchain for Indexer Settings", then press Finish.
    The project now exists for Eclipse, and it should allow you to edit files,
    use auto-completion, etc. Go ahead by either opening by hand one of your
    project's `.cc` files or by hitting Shift-Ctrl-R and start typing the
    name of one of your files, for example in the current context
    `step-22.cc`. This should open the file and allow you to edit it.
    However, Eclipse will show swiggly lines under
    any deal.II symbol, the deal.II header files, etc. The next step is to
    teach Eclipse where to find the deal.II header files so that it knows
    about the deal.II classes, functions, etc. To this end, right-click on the
    newly created project in the "Project Explorer" subwindow on the left and
    choose "Properties" at the very bottom. Then:
    - Select "C/C++ General > Paths and Symbols" by expanding the tree view under "C/C++ General"
    - Select "GNU C++" and click on "Add..." on the right
    - Select "Add to all languages", just to be on the safe side
    - From the "File system..." select the include directory of an installed version of deal.II
    - Click "OK" as often as necessary to close all dialog windows.
    - If your project depends on other external libraries, you may wish to repeat this step for all include file directories of these projects.
    Eclipse should have removed the swirly lines from under all header files, but it will likely not recognize the deal.II classes and functions yet, as indicated by the lines under these names and the red marks at the right margin of the editor sub-window. To achieve this, just close the editor sub-window and open the file again. You can test that Eclipse has found everything by putting the cursor into the editor window where you opened step-22.cc and in one of the member functions start typing something like `triangulation.exec` and then hitting Ctrl-Space to see what auto-completions Eclipse proposes. It should, among others, at least propose `triangulation.execute_coarsening_and_refinement` based on its knowledge that the member variable `triangulation` is of type `Triangulation<dim>`.

*Note:* Eclipse will use a _very large amount_ of memory to index all of the
symbols deal.II provides, on the order of several gigabyte. If you do not have
that much memory, you can limit how much it uses by excluding the
`include/deal.II/bundled` directory from indexing, which may contain in
particular the BOOST header files that are exceedingly large. This can be done
with a _resource filer_, as described at
http://help.eclipse.org/kepler/index.jsp?topic=/org.eclipse.platform.doc.user/concepts/resourcefilters.htm .

The next step is to teach Eclipse how to compile your project. To this end, it needs a Makefile. If you are writing the Makefile yourself, all is well. If you are using CMake to build your project in the same directory as the source files are (that's the setup we use for the tutorial programs), then you need to run CMake first to produce a Makefile. These are the next steps then:

  - Configure the build:
    - As above, highlight the new "step-22" project, then go to "Project > Make target > Create" and enter "step-22" into the respective dialog field.
    - Alternatively, you can right-click on the "step-22" project name in the left sub-window and go to the "Make targets > Create" entry.
    - Afterwards select "Project > Properties" from the main menu bar and in the corresponding dialog select "C/C++ Build" from the left panel (i.e., the actual entry, not the down-arrow next to it). Then select the "Behaviour" tab to set the desired build target, here "step-22", at "Build (incremental Build)" text box. The correct entry for "Clean" is simply "clean" (this is used to remove all generated files in this directory).


  - Configuring the binary to run and debug ("launches"):
    - From the main menu bar select: "Run > Run configurations" to open the "Run Configuration" window
    - From the window, double click "C/C++ Application" on the left panel to create a new run configuration, or highlight "C/C++ Application" and click on the hardly-recognizable "New launch configuration" button at the top of the selection list on the left. Then, unless it is already there, type the executable name that is produced by the Makefile in the field "C/C++ Application", again "step-22" in this example.
    - If you have an application that requires an input file, then the name of this input file can be entered in the other tabs.
    - Close the dialog, and test whether the application runs as intended by clicking on the green right triangle button at the top of the primary window, roughly in the middle. The output of the program should again appear in the "Console" tab of the subwindow at the bottom.

At this point you should have a complete project you can edit, compile and run. The screen should look roughly like this after opening the step-22.cc file (my project names are prefixed with the name of the machine, thus the "ultra450." prefix to everything; also, when I created this snapshot, I had not set the include paths correctly and the red swirly lines under many symbols should not be there for you):

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-2.png" align="center" />'


# Debugging with Eclipse

Once you have set up a launch configuration for an application as described above, you can also debug this application. (One of the video lectures referenced above also shows you how to do this.) To this end, hit the small green "Bug" icon just to the left of the "Run" icon described above. This will build the executable (if it isn't up to date) and then move all of Eclipse into "Debug Perspective", as shown here:

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-3.png" align="center" />'

<br>

"Debug perspective" gives you an entirely different view on the program as the "Code perspective" you have seen before. (You can toggle between the two perspectives using the icons at the top right of the Eclipse main window.) In particular, what you primarily get to see is the call stack at the top left, a view of all local variables at the top right, the source at the center left, an outline of the current file at the center right, and the console output of your program at the bottom. All of these sub-windows can be resized, of course. Some of these sub-windows have other tabs as well that you can explore, and if you don't need one sub-window for a moment, then click on the triangle pointing down to minimize it -- it will end up as an icon at the left margin, right margin, or bottom right of the main window (depending on where the sub-window was located before) from where it can be restored.

Using the symbols in the "Debug" sub-window at the top left (or the menu entries and corresponding keyboard shortcuts listed in the "Run" menu"), you can now step through the program, inspect the current state of your program at the top right by observing the values of local variables, and set breakpoints by going to certain source locations and right-clicking onto the code line you want to set a breakpoint on.



# Setting up deal.II as a development project in itself

If you want to work on the deal.II sources themselves, rather than just use
them for your own project, then use the following steps:

  - Select menu item "File > New... > Makefile project with existing code" and in the dialog that opens give the project a name (e.g., "deal.II") and enter or select the path to the deal.II directory into which you have previously unpacked the library. In the "Toolchain for Indexer Settings" select "Linux GCC". Then click "Finish".

You will notice that an icon for the next project has appeared in the "Project Explorer" sub-window on the left side of the Eclipse main window. The icon can be expanded using the little down-arrow on its left to see the directory tree underneath and you can try to go down into it to open a file just to see how it works. You will also notice that a progress bar at the center bottom of the Eclipse window is indicating that Eclipse is reading and parsing all the files in this directory tree so that the editor can offer you auto-completion and similar things.

*Note 1:* The steps above only set up deal.II as a project inside Eclipse,
allowing you to edit files and telling Eclipse where to find symbols so that
things like auto-completion work. However, they do not allow you to build
and install deal.II from inside Eclipse. Eclipse does in principle allow you
to set up these things (see above, for how to do this on a project that
<i>uses</i> deal.II), but it is difficult to do when building in a directory
other than where the source files are located, as we suggest in the deal.II
installation instructions. Consequently, if you want to <i>develop deal.II,
not just use it</i>, then you still want to build and install from the command
line.

*Note 2:* The steps above allow you to use Eclipse to edit deal.II files and
most of it will work just as expected. However, there is one snag: some parts
of the code of deal.II is guarded by things like
```
  #ifdef DEAL_II_WITH_TRILINOS
  ...
  #endif
```
In particular, this is the case for all of the Trilinos and PETSc wrappers,
and parts of the MPI stuff. The problem is that whether this preprocessor
symbol is defined or not is something that can not be determined by looking at
the <i>source</i> directory we have taught Eclipse about: it is something that
is determined in a file `config.h` in the <i>build</i> directory (of which
there may in fact be
more than one per source directory). Consequently, by default, Eclipse doesn't
see the `#define DEAL_II_WITH_TRILINOS` anywhere and thus never offers you to
auto-complete any of the Trilinos, PETSc or MPI stuff. This is annoying. If
you want to develop stuff where this is an obstacle, you can provide Eclipse
with additional include paths to search, as explained above when setting up
external projects. You can do this to provide Eclipse with a path where it can
find a `deal.II/base/config.h` file (typically, the install directory where you
install the version you are currently developing) that has the requisite
`#define`s so that Eclipse also provides you with information about the
otherwise unavailable classes.

  - The next step is to set up deal.II as a project Eclipse knows how to compile. To this end, select (highlight) the deal.II project with the mouse in the "Project Explorer" sub-window. Then go to "Project > Make Target > Create" and enter a Makefile target name. You can get the existing target names by running *make* on the command line in the deal.II directory -- the two most common ones are "all" and "online-doc", along with "debug" and "optimized". You can repeat creating targets several times.

  - To see which targets now exist for Eclipse, note the "Make targets" tab in the right sub-window (the other tabs are "Outline" and "Task list", unless you have added more tabs to this sub-window using "Window > Show view").

  - You can let Eclipse build a particular target by double-clicking on a target. The corresponding output of the *make* command and the compilers it calls will then end up in the "Console" tab in the bottom sub-window.

Everything you've done so far is visible in the following screenshot:


'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-1.png" align="center" />'

<br>

*Note:* If a particular tab of a sub-window is not where you expect it, look around in the other sub-windows. They can be drag-and-dropped into the place where you want them to be. If they're not there altogether, take a look at the list of views in the "Window > Show view" menu entry for what's available!

A final, useful, modification if you have more than one processor in your system is to enable parallel builds. To this end, select the menu entry "Project > Properties...", then select "C/C++ Build" and in the "Builder Settings" tab uncheck the box that says "Use default build command". This enables editing the text field that follows and allows you to replace the default command *make* by *make -j8*, for example, if you want to compile with 8 parallel compile jobs.



# Some useful keyboard shortcuts

I found the following key combinations useful, beyond the usual ones to open or save files, etc:


<table border="1" cellspacing="0">
<tr>
<th colspan="2" style="background-color:#ffff99;"> Editing
</th></tr>
<tr>
<th colspan="2"> Jumping around in code
</th></tr>
<tr>
<td>       Ctrl-Shift-R
</td><td> Open resource: type the name of a file and get it opened. Note that the search window allows you to use wildcards such as `*` if you only want to type part of a name.
</td></tr>
<tr>
<td>       Ctrl-Shift-H
</td><td> Open type: type the name of a type/class and get it opened. Note that the search window allows you to use wildcards such as `*` if you only want to type part of a name.
</td></tr>
<tr>
<td>       Ctrl-Shift-T
</td><td> Open element: type anything (class name, function name) and get a list of anything that matches to select from
</td></tr>
<tr>
<td>       Ctrl-Alt-I
</td><td> Open the include browser: Show a list of all include files used in the current file. Right clicking on any of the shown include files allows opening them.
</td></tr>
<tr>
<td>	Ctrl-O
</td><td>Outline: Provide a list of all things that are happening in this file, e.g. class declarations and function definitions, include directives, etc; the dialog that appears allows you to start typing a name, reducing the list to all those that match.
</td></tr>
<tr>
<td>	F3
</td><td>Jump to declaration of a function or variable if the cursor is currently in a function definition
</td></tr>
<tr>
<td>	Ctrl-Shift-Down
</td><td>Jump to next function
</td></tr>
<tr>
<td>	Ctrl-Shift-Up
</td><td>Jump to previous function
</td></tr>
<tr>
<td>	Ctrl-L
</td><td>Goto line
</td></tr>
<tr>
<td>	Crtl-W
</td><td> Closes current "tab".
</td></tr>
<tr>
<td>	Alt-Left
</td><td>Jump to previous used "tab". This even opens a previously closed file.
</td></tr>
<tr>
<td>	Alt-Right
</td><td>Jump to next "tab"
</td></tr>
<tr>
<th colspan="2"> Searching and replacing
</th></tr>
<tr>
<td>	Ctrl-J
</td><td>Incremental find: start typing and see what fits so far
</td></tr>
<tr>
<td>	Ctrl-F
</td><td>Find and find/replace
</td></tr>
<tr>
<td>	Ctrl-K
</td><td>Find next
</td></tr>
<tr>
<th colspan="2"> Other things
</th></tr>
<tr>
<td>	Ctrl-Space
</td><td>Call auto-completion assistant
</td></tr>
<tr>
<td>	Alt-/
</td><td>Auto-complete. If multiple names match the already typed text, then you can cycle through them by hitting Alt-/ multiple times.
</td></tr>
<tr>
<td>	Ctrl-Shift-Space
</td><td>In an expression of the form "object.member()" or "function()" with the cursor in the parentheses, opens a context menu that shows the order and types of arguments that go there.
</td></tr>
<tr>
<td>	Ctrl-/
</td><td>Comment in/out selected text or current line
</td></tr>
<tr>
<td>	Ctrl-Shift-F
</td><td>Automatic indent highlighted lines of code.
</td></tr>
<tr>
<td>	Ctrl-_ (Ctrl-Shift-Minus)
</td><td>Split/unsplit editor view horizontally
</td></tr>
<tr>
<td>	Ctrl-{ (Ctrl-Shift-[)
</td><td>Split/unsplit editor view vertically
</td></tr>
<tr>
<td>	Ctrl-3
</td><td>Quick access: start typing and it shows all menus, editors, views, commands that match that substring
</td></tr>
<td>	Ctrl-Shift-L
</td><td>Show all key bindings, e.g,. for example to learn about more key shortcuts
</td></tr>
<tr>
<th colspan="2" style="background-color:#ffff99;"> Debugging
</th></tr>
<tr>
<td>	F6
</td><td> Step over (gdb's "next")
</td></tr>
<tr>
<td>	F5
</td><td> Step into (gdb's "step")
</td></tr>
<tr>
<td>	F7
</td><td> Step return (gdb's "finish")
</td></tr>
<tr>
<td>	F8
</td><td> Resume (gdb's "continue")
</td></tr>

<tr>
<th colspan="2" style="background-color:#ffff99;"> Running a program
</th></tr>
<tr>
<td>	Ctrl-B
</td><td> Build (call make)
</td></tr>
<tr>
<td> Ctrl-F11
</td><td> Run
</td></tr>
<tr>
<td> F11
</td><td> Run program in the debugger; you may want to set breakpoints beforehand, for example by right-clicking with the mouse on the left sub-window margin next to a particular line in the source code
</td></tr>
<tr>
<th colspan="2" style="background-color:#ffff99;"> Working with subversion (works both in file editors as well as the C/C++ project view)
</th></tr>
<tr>
<td> Ctrl-Alt-L
</td><td> Show diffs
</td></tr>
<tr>
<td> Ctrl-Alt-C
</td><td> Commit
</td></tr>
<tr>
<td> Ctrl-Alt-U
</td><td> Update from repository
</td></tr>
<tr>
<td> Ctrl-Alt-P
</td><td> Create a patch from local changes
</td></tr></table>

# Configuring Eclipse

Eclipse can be configured in a myriad different ways, to the point where one sometimes finds oneself wishing that there were less bells and whistles. Be that as it may, there are many useful things that may take a while to discover but that really make life easier. Below is a list of things that integrate working with big software projects like deal.II better into the existing workflows of version control, documentation, etc.


## Showing line numbers and dealing with tabs

Since deal.II assertions show which line of a file triggered an error, it is often useful to configure Eclipse to show line numbers next to each line of a file that's open in an editor. To do this, go to "Window > Preferences", then select the down-arrow of "General", in the sub-tree that then expands select the down-arrow next to "Editors" and click on "Text editor". In the dialog to the right of the window, click the checkbox called "Show line numbers".

'<img width="400px" src="http://www.dealii.org/images/wiki/Eclipse-line-numbers.png" align="center">'

<br>

In the same dialog, there is also a setting for the tab width. In the deal.II source code, we use a tab width of 8, but Eclipse by default uses 4, leading to an occasionally awkward display of indentation levels. Thus, set it to 8. Finally, since different editors have a habit of interpreting the tab size differently (some assume that a tab equals 4 spaces, whereas others assume 8), it is worthwhile to simply disable the use of tabs altogether when editing code, and letting the editor fill any space it encounters using spaces. For this, check "Insert spaces for tabs".


## Enabling code folding

Some people really like the editor to fold in code they aren't working on right now so that only that part that's relevant for the moment is visible (others just excessively use the "Outline" facility using the Ctrl-O shortcut). By default, the Eclipse folding settings are a bit awkward since it doesn't want to fold a whole lot, but this (like everything else) can be configured. To this end, go to "Window > Preferences" and select the C/C++ tab and under it the "Editor > Folding" sub-tab. You can select which regions of a code can be folded:

'<img width="400px" src="http://www.dealii.org/images/wiki/Eclipse-folding-assistant.png" align="center" />'

The buttons under "Initially fold these region types" allow you to set which regions should be folded in whenever you open a file the first time; I have chosen to see the entire file but you can obviously choose a different set.

Once selected as above, you can fold in or out certain parts of the code by clicking on the little "-" or "+" buttons in the gray area on the left side of an editor window, right next to line numbers if you have enabled them as shown above.


## Integrating Eclipse and Subversion

deal.II and many of its subprojects use [http://subversion.apache.org Subversion]([KDevelop]]) as the Version Control System of choice. Eclipse has a plugin that allows using Subversion for all the files one wants to work on; the plugin is called **Subversive** and you need to install it before creating any project (see above) you want to use this plugin for.

In order to install Subversive, go to menu entry "Help > Install new software" and choose the site in the "Work with" dialog at which the plugins for this version of Eclipse can be obtained. At the time of writing this text Indigo was the current Eclipse version,  and the site was "Indigo - http://download.eclipse.org/releases/indigo". It takes a while (a couple of minutes on my system) for Eclipse to figure out all the packages that are available but when it does you should be able to find the Subversive packages in the "Collaboration" tab that you can expand using the down arrow. Select them all and then hit the appropriate buttons to get them installed. Half-way through the process, Eclipse will ask you which "Subversive Connector" you want to install. These connectors are apparently specific for the particular subversion release you have (which you can find out by calling `svn --version` on the command line). Pick the one that matches your version best.

Once you have the Subversive plugin installed, create an Eclipse project as discussed at the top of this page. The project explorer panel should then look somewhat like this, displaying the revision number next to each file or directory:

'<img width="400px" src="http://www.dealii.org/images/wiki/Eclipse-subversive-1.png" align="center" />'

All the usual subversion operations (adding a file or directory, updating it from the repository, committing, diffing, annotating, or showing a history) can then be obtained through one of two ways:

  - If you are in an editor window, right-click into the window to get the context menu and from there go to the sub-menu called "Team".
  - Right-clicking on a file or directory in the "Project Explorer" panel on the left (i.e., the one shown above) and then again selecting the sub-menu "Team".

Maybe confusingly, however, the option to see the diffs of a file/directory against the one that's in the repository is not found in the "Team" sub-menu, but in the "Compare with" sub-menu.

## Integrating Eclipse and Doxygen

[Doxygen](http://www.doxygen.org/) is one of the most important tools for generating high quality documentation for C++ software, and it is <em>the</em> tool by which all of deal.II's documentation is generated. Thus, it would be nice if Eclipse could help write this documentation, and indeed it can. To this end, tell Eclipse that we want to be able to write Doxygen comments, by going to the "Window > Preference" menu entry and then selecting the  "C/C++" tab and "Editor" subtab as shown here:

'<img width="400px" src="http://www.dealii.org/images/wiki/Eclipse-doxygen.png" align="center" />'

Then select "Doxygen" as the "Documentation tool" in the bottom part of the panel on the right, as shown above.

With this setting so chosen, we can start editing and see how it can help us. To this end, let's assume we have written the following function declaration:

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-doxygen1.png" align="center" />'

We may want to document it using the usual Doxygen markup. So go to the line before the function, type `/**` and hit enter. Eclipse will then automatically expand the comment to the following:

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-doxygen2.png" align="center" />'

You can then start filling in the documentation for the function as a whole, the two function arguments, and the return type, using all the markup options Doxygen provides.

## Code style issues


Eclipse can be configured in more ways than one can count. One issue is configuring the indentation style. As a historical accident, deal.II is using an indentation style that doesn't match pretty much any other style that is around, but it can be approximated using Eclipse's settings.

Deal.II uses clang-format for indentation. This is not natively supported by Eclipse, but fortunately, a suitable add-on exists. From the Eclipse menu choose `Help`->`Eclipse Market place...` and search for CppStyle. This allows you to run clang-formant when Ctrl+Shift+F is pressed. It also has an option to run clang-format on save. It uses settings from the .clang-format file (custom for every project).  You can configure CppStyle globally from Window->preferences and then C/C++ -> CppStyle (or just type CppStyle in search bar). Project-wise settings are also possible. 

There is also a way to configure Eclipse without installing add-ons:
  - Highlight the deal.II project you have created above in the "Project Explorer" sub-window.
  - Go to "Project > Properties" from the main menu, or right-click on the project and choose "Properties" in the context menu.
  - Then go to expand the "C/C++ General" sub-tree by clicking on the down arrow next to it and select "Code style".
  - In the panel on the right, you can click on "Configure Workspace Settings..." to get a dialog in which you can edit an existing coding style or create a new one.
  - You can import the following style: [stylefile.xml](https://raw.githubusercontent.com/dealii/dealii/master/contrib/styles/eclipse-dealii-code-format.xml)
  - Alternatively, create a new one and choose the "GNU" style as the basis for this new style and follow the instructions below.

Modify the entries under the various tabs of this dialog as shown in the following set of pictures (click on the pictures to get larger versions):

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-3.png" align="left" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-4.png" align="right" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-5.png" align="left" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-6.png" align="right" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-7.png" align="left" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-8.png" align="right" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-9.png" align="left" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-10.png" align="right" />'

'<img width="800px" src="http://www.dealii.org/images/wiki/Eclipse-code-indentation-11.png" align="left" />'

# Setting up Eclipse with Parallel Tools Platform (PTP)

Section currently under development. Please be patient.
## What PTP is?

[PTP](http://www.eclipse.org/ptp/) is a set of tools for developing, debugging and profiling MPI applications. Instructions for installation into existing Eclipse can be found [here](http://www.eclipse.org/ptp/downloads.php). Make sure you're trying to install the correct PTP version for your Eclipse (7.0 for Kepler, 6.0 for Juno, 5.0 for Indigo). Most of the installation issues are described in PTP release notes [v.7.0](http://wiki.eclipse.org/PTP/release_notes/7.0). 
## Installing from package

These instructions have been tested under Ubuntu 12.04
If you were not able to install PTP into existing  Eclipse (or you can't find PTP perspectives in Window->Show Perspective -> Other  - it sometimes happens), maybe it would be easier to download a complete package from [Eclipse download site](http://www.eclipse.org/downloads/).  "Eclipse for Parallel Application Developers Eclipse for Parallel Application Developers" is the package that you have to download. Untar it wherever you want, open terminal, navigate to Eclipse folder and run ./eclipse (it is safe to do it this way - to make sure that Eclipse has some environmental variables as in terminal).
## SDM - Scalable debug manager

Debugging parallel programs is not easy - especially when the error occurs only in parallel executions. SDM allows debugging parallel programs in similar way that standard Eclipse debugger can help you improve  non-mpi applications. To use SDM you have to install it on target machine. [Here](http://wiki.eclipse.org/PTP/release_notes/7.0#Install_optional_PTP_debugger_component) are instructions how to do it. Compiling from source should work (tested on Ubuntu), but if you have some trouble with it, you should find compiled PTP in ptp-sdm-7.0.x.zip  package from here: [http://download.eclipse.org/tools/ptp/updates/kepler] (directory sdm/org.eclipse.ptp.OS_7.0.x.yyyymmddhhmm , where  OS is operating system and 7.0.x.yyyymmddhhmm is PTP release ).
## Setting up project with parallel run configuration

Set up you project in the same way as for standard deal.II programs. If you have workspace folder from other versions of Eclipse you can reuse it. Now you will only have to configure parallel runs.
### Local projects (only with Eclipse Kepler with PTP 7.x)

Go to Run->Run Configurations. Select Parallel Application and click on "New lauch configuration"(PICTURE). Specify your MPI version from drop-down list and select Local in connection type. You will be asked to confirm connection to local system, click Yes.(PICTURE) Now there will be some other option available: (PICTURE)
Go to applications and specify executable in "Application program". Click "Apply". Now you can test your configuration by pressing "Run", or just click "Close".  New run configuration will appear in drop-down menu near Run button at the top of a screen.

If you run configuration is working, you can set up debugger. Go to Run->Debug Configurations. In Parallel Applications will be your run configuration. Now you only have to set SDM executable path - go to Debugger tab.(PICTURE) If you have installed SDM with custom prefix, specify path to SDM. The number of MPI processes can be increased in the Resources tab.

## Debugging a parallel program

From the drop-down list near bug, icon selects your debug configuration. You will be asked to confirm opening Parallel Debug perspective, click yes. The following view will appear: (PICTURE)

The main difference between the default Eclipse debugger is "Parallel Debug" tab. The diamonds represent processes, green means that the process is running, yellow - suspended, red -terminated. You can select one process by double click. The black square indicates if process is selected, only selected processes are displayed in "Debug" tab. From Debug tab you can debug each process like serial one, you can also suspend a group of processes in "Parallel Debug" tab. 

More informations can be found in PTP help and tutorials.