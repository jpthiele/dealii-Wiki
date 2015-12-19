# The deal.II FAQ
Over the course of the project deal.II has accumulated a large number of answers
to frequently asked questions, which are collected here.

## Table of Contents
  * [The deal.II FAQ](#the-dealii-faq)
    * [Table of Contents](#table-of-contents)
    * [General questions on deal.II](#general-questions-on-dealii)
      * [Can I use/implement triangles/tetrahedra in deal.II?](#can-i-useimplement-trianglestetrahedra-in-dealii)
      * [I'm stuck!](#im-stuck)
      * [I'm not sure the mailing list is the right place to ask ...](#im-not-sure-the-mailing-list-is-the-right-place-to-ask-)
      * [How fast is deal.II?](#how-fast-is-dealii)
      * [deal.II programs behave differently in 1d than in 2/3d](#dealii-programs-behave-differently-in-1d-than-in-23d)
      * [I want to use deal.II for work in my company. Do I need a special license?](#i-want-to-use-dealii-for-work-in-my-company-do-i-need-a-special-license)
    * [Supported System Architectures](#supported-system-architectures)
      * [Can I use deal.II on a Windows platform?](#can-i-use-dealii-on-a-windows-platform)
        * [Run deal.II through a virtual box](#run-dealii-through-a-virtual-box)
        * [Dual-boot your machine with Ubuntu](#dual-boot-your-machine-with-ubuntu)
        * [Run deal.II natively on Windows](#run-dealii-natively-on-windows)
        * [Are any of the native Windows compilers supported by deal.II?](#are-any-of-the-native-windows-compilers-supported-by-dealii)
      * [Can I use deal.II on an Apple Macintosh?](#can-i-use-dealii-on-an-apple-macintosh)
      * [Does deal.II support shared memory parallel computing?](#does-dealii-support-shared-memory-parallel-computing)
      * [Does deal.II support parallel computing with message passing?](#does-dealii-support-parallel-computing-with-message-passing)
      * [How does deal.II support multi-threading?](#how-does-dealii-support-multi-threading)
      * [My deal.II installation links with the Threading Building Blocks (TBB) but doesn't appear to use multiple threads!](#my-dealii-installation-links-with-the-threading-building-blocks-tbb-but-doesnt-appear-to-use-multiple-threads)
    * [Configuration and Compiling](#configuration-and-compiling)
      * [Where do I start?](#where-do-i-start)
      * [I tried to install deal.II on system X and it does not work](#i-tried-to-install-dealii-on-system-x-and-it-does-not-work)
      * [How do I change the compiler?](#how-do-i-change-the-compiler)
      * [I get warnings during linking when compiling the library. What's wrong?](#i-get-warnings-during-linking-when-compiling-the-library-whats-wrong)
      * [I can't seem to link/run with PETSc](#i-cant-seem-to-linkrun-with-petsc)
        * [Is there a sure-fire way to compile deal.II with PETSc?](#is-there-a-sure-fire-way-to-compile-dealii-with-petsc)
        * [I want to use HYPRE through PETSc](#i-want-to-use-hypre-through-petsc)
        * [Is there a sure-fire way to compile dealii with SLEPc?](#is-there-a-sure-fire-way-to-compile-dealii-with-slepc)
      * [Trilinos detection fails with an error in the file Sacado.hpp or <code>Sacado_cmath.hpp</code> ](#trilinos-detection-fails-with-an-error-in-the-file-sacadohpp-or-sacado_cmathhpp)
      * [My program links with some template parameters but not with others.](#my-program-links-with-some-template-parameters-but-not-with-others)
      * [When trying to run my program on Mac OS X, I get image errors.](#when-trying-to-run-my-program-on-mac-os-x-i-get-image-errors)
    * [C++ questions](#c-questions)
      * [What integrated development environment (IDE) works well with deal.II?](#what-integrated-development-environment-ide-works-well-with-dealii)
      * [Is there a good introduction to C++?](#is-there-a-good-introduction-to-c)
      * [Are there features of C++ that you avoid in deal.II?](#are-there-features-of-c-that-you-avoid-in-dealii)
      * [Why use templates for the space dimension?](#why-use-templates-for-the-space-dimension)
      * [Doesn't it take forever to compile templates?](#doesnt-it-take-forever-to-compile-templates)
      * [Why do I need to use typename in all these templates?](#why-do-i-need-to-use-typename-in-all-these-templates)
      * [Why do I need to use this-&gt; in all these templates?](#why-do-i-need-to-use-this--in-all-these-templates)
      * [Does deal.II use features of C++11 (formerly known as C++0x or C++1x)?](#does-dealii-use-features-of-c11-formerly-known-as-c0x-or-c1x)
      * [Can I convert Triangulation cell iterators to DoFHandler cell iterators?](#can-i-convert-triangulation-cell-iterators-to-dofhandler-cell-iterators)
    * [Questions about specific behavior of parts of deal.II](#questions-about-specific-behavior-of-parts-of-dealii)
      * [How do I create the mesh for my problem?](#how-do-i-create-the-mesh-for-my-problem)
      * [How do I describe complex boundaries?](#how-do-i-describe-complex-boundaries)
      * [I am using discontinuous Lagrange elements (FE_DGQ) but they don't seem to have vertex degrees of freedom!?](#i-am-using-discontinuous-lagrange-elements-fe_dgq-but-they-dont-seem-to-have-vertex-degrees-of-freedom)
      * [How do I access values of discontinuous elements at vertices?](#how-do-i-access-values-of-discontinuous-elements-at-vertices)
      * [Does deal.II support anisotropic finite element shape functions?](#does-dealii-support-anisotropic-finite-element-shape-functions)
      * [The graphical output files don't make sense to me -- they seem to have too many degrees of freedom!](#the-graphical-output-files-dont-make-sense-to-me----they-seem-to-have-too-many-degrees-of-freedom)
      * [In my graphical output, the solution appears discontinuous at hanging nodes](#in-my-graphical-output-the-solution-appears-discontinuous-at-hanging-nodes)
      * [When I run the tutorial programs, I get slightly different results](#when-i-run-the-tutorial-programs-i-get-slightly-different-results)
      * [How do I access the whole vector in a parallel MPI computation?](#how-do-i-access-the-whole-vector-in-a-parallel-mpi-computation)
      * [How to get the (mapped) position of support points of my element?](#how-to-get-the-mapped-position-of-support-points-of-my-element)
    * [Debugging deal.II applications](#debugging-dealii-applications)
      * [I don't have a whole lot of experience programming large-scale software. Any recommendations?](#i-dont-have-a-whole-lot-of-experience-programming-large-scale-software-any-recommendations)
      * [Are there strategies to avoid bugs in the first place?](#are-there-strategies-to-avoid-bugs-in-the-first-place)
      * [How can deal.II help me find bugs?](#how-can-dealii-help-me-find-bugs)
      * [Should I use a debugger?](#should-i-use-a-debugger)
      * [deal.II aborts my program with an error message](#dealii-aborts-my-program-with-an-error-message)
      * [The program aborts saying that an exception was thrown, but I can't find out where](#the-program-aborts-saying-that-an-exception-was-thrown-but-i-cant-find-out-where)
      * [I get an exception in virtual dealii::Subscriptor::~Subscriptor() that makes no sense to me!](#i-get-an-exception-in-virtual-dealiisubscriptorsubscriptor-that-makes-no-sense-to-me)
      * [I get an error that the solver doesn't converge. But which solver?](#i-get-an-error-that-the-solver-doesnt-converge-but-which-solver)
      * [How do I know whether my finite element solution is correct? (Or: What is the "Method of Manufactured Solutions"?)](#how-do-i-know-whether-my-finite-element-solution-is-correct-or-what-is-the-method-of-manufactured-solutions)
      * [My program doesn't produce the expected output!](#my-program-doesnt-produce-the-expected-output)
      * [The solution converges initially, but the error doesn't go down below 10<sup>-8</sup>!](#the-solution-converges-initially-but-the-error-doesnt-go-down-below-10-8)
      * [My time dependent solver does not produce the correct answer!](#my-time-dependent-solver-does-not-produce-the-correct-answer)
      * [My Newton method for a nonlinear problem does not converge (or converges too slowly)!](#my-newton-method-for-a-nonlinear-problem-does-not-converge-or-converges-too-slowly)
      * [Printing deal.II data types in debuggers is barely readable!](#printing-dealii-data-types-in-debuggers-is-barely-readable)
      * [My program is slow!](#my-program-is-slow)
      * [How do I debug MPI programs?](#how-do-i-debug-mpi-programs)
      * [I have an MPI program that hangs](#i-have-an-mpi-program-that-hangs)
      * [One statement/block/function in my MPI program takes a long time](#one-statementblockfunction-in-my-mpi-program-takes-a-long-time)
    * [I have a special kind of equation!](#i-have-a-special-kind-of-equation)
      * [Where do I start?](#where-do-i-start-1)
      * [Can I solve my particular problem?](#can-i-solve-my-particular-problem)
      * [Why use deal.II instead of writing my application from scratch?](#why-use-dealii-instead-of-writing-my-application-from-scratch)
      * [Can I solve problems over complex numbers?](#can-i-solve-problems-over-complex-numbers)
      * [How can I solve a problem with a system of PDEs instead of a single equation?](#how-can-i-solve-a-problem-with-a-system-of-pdes-instead-of-a-single-equation)
      * [Is it possible to use different models/equations on different parts of the domain?](#is-it-possible-to-use-different-modelsequations-on-different-parts-of-the-domain)
      * [Where do I start to implement a new Finite Element Class?](#where-do-i-start-to-implement-a-new-finite-element-class)
    * [General finite element questions](#general-finite-element-questions)
      * [How do I compute the error](#how-do-i-compute-the-error)
      * [How to plot the error as a pointwise function](#how-to-plot-the-error-as-a-pointwise-function)
      * [I'm trying to plot the right hand side vector but it doesn't seem to make sense!](#im-trying-to-plot-the-right-hand-side-vector-but-it-doesnt-seem-to-make-sense)
      * [What does XXX mean?](#what-does-xxx-mean)
    * [I want to contribute to the development of deal.II!](#i-want-to-contribute-to-the-development-of-dealii)
    * [I found a typo or a bug and fixed it on my machine. How do I get it included in deal.II?](#i-found-a-typo-or-a-bug-and-fixed-it-on-my-machine-how-do-i-get-it-included-in-dealii)
    * [I'm fluent in deal.II, are there jobs for me?](#im-fluent-in-dealii-are-there-jobs-for-me)

## General questions on deal.II

### Can I use/implement triangles/tetrahedra in deal.II?

This is truly one of the most frequently asked questions. The short answer
is: No, you can't. deal.II's basic data structures are too much tailored to
quadrilaterals and hexahedra to make this trivially possible. Implementing
other reference cells such as triangles and tetrahedra amounts to
re-implementing nearly all grid and DoF classes from scratch, along with
the finite element shape functions, mappings, quadratures and a whole host
of other things. Make triangles and tetrahedra work would certainly involve
having to write several ten thousand lines of code, and to make it usable
in all the rest of the library would require auditing a very significant
fraction of the 600,000 lines of code that make up deal.II today.

That said, the current specialization on quadrilaterals and hexahedra has
two very positive aspects: First, quadrilaterals and hexahedra typically
provide a significantly better approximation quality than triangular meshes
with the same number of degrees of freedom; you therefore get more accurate
solutions for the same amount of work. Secondly, because the shape of cells
are known, we can make a lot of things known to the compiler (such as the
number of iterations of a loop over all vertices of a cell) which avoid a
large number of run-time computations and makes the library as fast as it
is. A simple example is that in deal.II we know that a loop over all
vertices of a cell has exactly `GeometryInfo<dim>::vertices_per_cell`
iterations, a number that is known to the compiler at compile-time. If we
allowed both triangles and quadrilaterals, the loop would have
`cell->n_vertices()` iterations, but this would in general not be known at
compile time and consequently not allow the compiler to optimize on.

If you do need to work with a geometry for which all you have is a
triangular or tetrahedral mesh, then you can convert this mesh into one
that consists of quadrilaterals and hexahedra using the `tethex` program,
see http://code.google.com/p/tethex/wiki/Tethex .

### I'm stuck!

Further down below on this page (in the debugging section) we list a number
of strategies on how to find errors in your program. If your question is
how to implement something new for which you don't know where to start,
have you taken a look at the set of tutorial programs and checked whether
one or the other already has something that's close to what you want?

That said, there will be situations where documentation doesn't help and
where you need other someone else's opinion. That's what the [deal.II
mailing lists](http://dealii.org/mail.html) are there for: Feel free to
ask! You may also wish to subscribe to the users' list -- not so much
because someone else might ask the same question you have, but because
reading the list gives you background information on things others are
working on that may help you when you want to do something similar.

When asking for help on the mailing list, be specific. We frequently get mail of the following kind:
<pre>
  I'm trying to do X. This works fine but it fails when I try to transfer
  the data to my MyClass::Estimator object. I tried to use something
  similar to what's done in a couple of tutorial programs but it doesn't
  work. I'm new at C++ and I just can't seem to get the syntax right.
</pre>

This message doesn't contain nearly enough information for anyone to really
help you: we don't know what `MyClass::Estimator` is, we don't know how you
try to transfer data, we haven't seen your code, and we haven't seen the
compiler's error messages. (For more examples of how not to write help
requests, see [Section 3.2 of this
document](http://faculty.washington.edu/dchinn/how-not-to-code.pdf).) We
could poke in the dark, but it would probably be more productive if you
gave us a bit more detail explaining what doesn't work: show us the code
you implemented, show us the compiler's error message, or be specific in
some other way in describing what the problem is!

### I'm not sure the mailing list is the right place to ask ...

Yes, it probably is. Please direct your questions to the mailing list and
not to individual developers. There are many reasons:

  1. Others might have similar questions in the future and can search the
     archives.
  1. There are many active users on the mailing list that are happy to
     help. There probably is someone who did something very similar before.
  1. Imagine everyone would stop using the mailing list and email us
     directly. We would spend most of our time answering the same questions
     over and over.
  1. Many users are reading the mailing list and are interested in deal.II
     in general and are learning by skimming emails. Give them a chance.
  1. As a consequence of all this, we typically prioritize questions on
     mailing lists over emails sent directly to us asking for help.
  1. Don't be afraid. There are no stupid questions (only off-topic ones).
     Everyone started out at some point. Asking the questions in the open
     helps us improve the library and documentation.

That said, if there is something you can not discuss in the open, feel free
to contact us!


### How fast is deal.II?

The answer to this question really depends on your metric. If you had to
write, say, a Stokes solver with a particular linear solver, a particular
time stepping scheme, on a piecewise polygonal domain, and Q2/Q1 elements,
you can write a code that is 20% or 30% faster than what you would get when
using deal.II because you know the building blocks, shape functions,
mappings, etc. But it'll take you 6 months to do so, and 20,000 lines of
code. On the other hand, when using deal.II, you can do it in 2 weeks and
204 lines (that's the number of semicolons in step-22).

In other words, if by "fast" you mean the absolute maximal efficiency in
terms of CPU time deal.II is more than likely to lose against a
hand-written Fortran77 code. But for most of us, the real question of
"fast" also includes the time it takes to get the code running and
verified, and in that case deal.II is most likely the fastest library out
there simply by virtue of the fact that it is by far the largest and most
comprehensive finite element library available as Open Source.

This all, by the way, does not mean that we don't care about speed: We
spend a lot of effort profiling the library and working on the hot spots to
make codes fast. The discussion of this issue in the introduction of
step-22 is a good example. There are also some guidelines below on how to
profile your code in the debugging section of this FAQ.

### deal.II programs behave differently in 1d than in 2/3d

In deal.II, you can write programs that look exactly the same in 2d and 3d,
but there are cases where 1d is slightly different. That said, this is an
area that we have significantly rewritten, and starting with deal.II 7.1,
most cases should work in 1d in just the same way as they do in 2d/3d. If
you find something that doesn't work, please report it to the mailing list.

Historically, the differences primarily resulted from the fact that in
deal.II, we represent vertices differently from lines and quads; whereas
the latter can store information (for example boundary indicators, user
flags, etc) vertices don't. As a consequence, the boundary indicator of a
boundary part in 1d (i.e. either the left or right vertex) were determined
by convention, rather than by setting it explicitly: the left boundary of a
1d domain always had boundary indicator zero, the right boundary always
boundary indicator one. This was different from the 2d/3d case where by
default (unless you explicitly set things differently) all boundaries have
indicator zero. This left-boundary-has-id-0, right-boundary-has-id-1 is
still the default today, but at least you can set the boundary indicators
of these end-points to something different today.

A second difference is that vertices have no extent, and so you can't apply
quadrature to them. As a consequence, the FEFaceValues class wasn't usable
in 1d. Again, this should work these days: every quadrature formula that
has a single quadrature point is a valid one for points as well.

### I want to use deal.II for work in my company. Do I need a special license?

Before going into any more details, you **need** to carefully read the
license deal.II is under. In particular, the explanations below are not meant
to be legal advice and does not override the provisions in the Open Source
license.

However, before this, let us provide our overarching philosophy: It is our
intention to have constructive relationships with those who want to use our
work commercially, and we encourage commercial use. After having used a
more restrictive license until 2013, we have come to the conclusion that
these licenses serve neither side particularly well: it made commercial use
difficult, and the lack of commercial use deprived us of critical feedback,
potential contributions from professional users, and our users of potential
employment opportunities. Everyone is better off with the LGPL license we
are using now, and we hope that deal.II also finds use in commercial
settings.

Now for the smaller print: Generally, the LGPL is a fairly liberal license.
In particular, if you *develop a code based on deal.II*, then there is no
requirement that you also open source your own code: you can keep it closed
source, under a proprietary license, and you don't need to give it to
anyone (neither your customers nor to us).

The LGPL is only restrictive in that the *changes you make to deal.II
itself* must also be licensed under the LGPL. There is not frequently a
need to change the library itself, and in many of these cases you will
probably be interested to get them into the upstream development sources
anyway (e.g., in cases of bugs) rather than having to forward port them
indefinitely. Of course, we are interested in this as well. However, there
is no such requirement that you upstream these changes: the only people you
have to make these modifications to deal.II available to are your
customers.

As mentioned above, the preceding paragraphs are not a legal
interpretation. For definite interpretations of the LGPL, you may want to
consult lawyers familiar with the topic or search the web for more detailed
interpretations.


## Supported System Architectures

### Can I use deal.II on a Windows platform?

deal.II has been developed with a Unix-like environment in mind and it
shows in a number of places regarding the build system and compilers
supported. That said, there are multiple methods to get deal.II running if
you have a Windows machine.

#### Run deal.II through a virtual box

The simplest way to try out deal.II is to run it in a premade virtual
machine. You can download the virtual machine for VirtualBox from
http://www.math.clemson.edu/~heister/dealvm/ and run it inside windows.

Note that your experience depends on how powerful your machine is. More
than 4GB RAM are recommended. A native installation of Linux is preferable
(see below).

#### Dual-boot your machine with Ubuntu

The simplest way to install Linux as a Windows user is to dual-boot.
Dual-boot means that you simply install a second operating system on your
computer and you choose which one to start when you boot the machine. Most
versions of Linux support installing themselves as a second operating
system. One example is using the Ubuntu installer for Windows. This
installer will automatically dual-boot your system for you in a safe and
fully reversible manner. Simply follow the instructions on
http://www.ubuntu.com/download/desktop/install-ubuntu-with-windows

If at some point in the future you wish to remove Ubuntu from your system,
from the Windows program manager (add-remove programs in older versions and
programs and features in newer versions) you can simply uninstall Ubuntu as
you would any other program.

*Note:* The actual install file is linked through the text "Windows
installer" in the first gray box.  You will be prompted to donate to
Ubuntu, which is entirely optional. You will also be prompted to use a
different version of Ubuntu if you use Windows 8.

#### Run deal.II natively on Windows

Native Windows support for deal.II is currently experimental and not
officially supported. See the separate page on [[Windows]] for more
details.

#### Are any of the native Windows compilers supported by deal.II?

If you are thinking of compilers like Microsoft Visual C++ or Borland C++,
then the answer is: No, not at the moment. The basic problem is that none
of the main developers use Windows and as in any other volunteer project,
things happen if people with the necessary expertise step forward to make
it work.

### Can I use deal.II on an Apple Macintosh?

Yes, at least on the more modern OS X operating systems this works just
fine. deal.II supports native compilers shipping with XCode as well as gcc
from Mac Ports.

The only issue we are currently aware of is that if deal.II is configured
to interface with PETSc, then PETSc needs to be configured with the
<code>--with-x=0</code> flag to prevent linking in the X11 libraries (you
probably won't need them anyway). Installing with PETSc has a myriad of
other problems, though we believe that we have a way to stably interface
it. You may want to read through the PETSc-related entries further down,
however.

### Does deal.II support shared memory parallel computing?

Yes. deal.II supports multithreading with the help of the
[http://www.threadingbuildingblocks.org Threading Building Blocks (TBB)
library](c967ec2ff74d85bd4327f9f773a93af3]). It is enabled by default and
can be controlled via the `DEAL_II_WITH_THREADS` configuration toggle
passed to `cmake` (see the deal.II readme file).

### Does deal.II support parallel computing with message passing?

Yes, and in fact it has been shown to scale very nicely to at least 16,384
processor cores in a paper by Bangerth, Burstedde, Heister and Kronbichler.
You should take a look at the documentation modules discussing parallel
computing, as well as the step-40 tutorial program.


### How does deal.II support multi-threading?

deal.II will use multi-threading using several approaches:
1. some BLAS routines might be multi-threaded (typically using OpenMP).
   This can be controlled from the command line using OMP_NUM_THREADS (also
   see the entry in the FAQ below)
2. Many places in the library are parallelized using the Threading Building
   Blocks (TBB) library.

MPI_InitFinalize() has an optional third argument that specifies the number
of threads to use for the TBB. The default is 1. This gets send to the TBB
via a call to  MultithreadInfo::set_thread_limit(). If you pass
numbers::invalid_unsigned_int into MPI_InitFinalize (or if you don't use
that class, call set_thread_limit directly) then TBB will use the maximum
number of threads that makes sense (and you can limit it using
DEAL_II_NUM_THREADS from the command line).

Also note that while our Trilinos wrappers support multi-threading, the
PETSc wrappers do not support this at this time, so you need to run with
one thread per process.

### My deal.II installation links with the Threading Building Blocks (TBB) but doesn't appear to use multiple threads!

This may be a quirky interaction with the [GOTO
BLAS](http://www.tacc.utexas.edu/tacc-projects/gotoblas2/) :-( If you use
Trilinos or PETSc, both of these require a BLAS library from your system,
and the deal.II cmake configuration will make sure that it is linked with.
The problem stems from the fact that by default, the GOTO BLAS will simply
grab all cores of the system for its own use, and -- before your `main()`
function even starts, allow the main thread to use only a single core. (For
the technically interested: it sets the processor scheduling affinity mask,
using `set_sched_affinity` to a single bit.)

When the TBB initialization runs, still before `main()` starts, it will
find that it can only run on a single core and will consequently not be
able to work on multiple tasks in parallel.

The solution to this problem is to forbit the GOTO BLAS to grab all
processors for itself, since we spend very little time in BLAS anyway. This
can be done by setting either the `OMP_NUM_THREADS` or `GOTO_NUM_THREADS`
environment variables to 1, see
http://www.tacc.utexas.edu/tacc-software/gotoblas2/faq .


## Configuration and Compiling

### Where do I start?

Have a look at the  [ReadMe instructions](http://www.dealii.org/developer/readme.html) for details on how to configure and install the library with `cmake`.

### I tried to install deal.II on system X and it does not work

That does occasionally (though relatively rarely) happen, in particular if
you work on an operating system or with a compiler that the primary
developers don't have access to. In a case like this, you should ask for
help on the mailing list. However, remember: If your question only contains
the text "I tried to install deal.II on system X and it does not work" then
that's not quite enough to figure out what is happening. Even though the
people developing this software belong to the most able programmers in the
universe (and a decent number of parallel universes), all of us need data
to find errors. So, whatever went wrong, paste the error message into your
email. If the error is from the `cmake` invocation, show us the error
message that was printed on screen.
If the error happens after configuring and during compiling, add lines from
screen output showing the error to the mail.


### How do I change the compiler?

deal.II can be compiled by a number of compilers without problems (see the
section [prerequisites](http://www.dealii.org/readme.html#prerequisites) in
the readme file). If `cmake` does not pick the right one, selecting another
is simple, and described in a
[section](http://www.dealii.org/developer/development/cmake.html#compiler)
in the [cmake
documentation](http://www.dealii.org/developer/development/cmake.html).


### I get warnings during linking when compiling the library. What's wrong?

On some linux distributions with particular versions of the system
compiler, one can get warnings like these during the linking stage of
compiling the library:
```
`.L3019' referenced in section `.rodata' of
/home/bangerth/deal.II/lib/lac/sparse_matrix.float.g.o: defined in discarded section
`.gnu.linkonce.t._ZN15SparsityPattern21optimized_lower_boundEPKjS1_RS0_'
of /home/bangerth/deal.II/lib/lac/sparse_matrix.float.g.o
```

While annoying, these warnings do not actually seem to indicate anything
particularly harmful. Apparently, the compiler generates the same code
multiple times in exactly the same form, and the linker is only warning
that it is throwing away all but one of the copies. There doesn't seem to
be way to avoid these warnings, but they can be safely ignored.

### I can't seem to link/run with PETSc

Recent deal.II releases support PETSc 3.0 and later. This works, but there
are a number of things that can go wrong and that result in compilation or
linker errors, as explained below. If your program links properly with
PETSc support, it will very likely also produce the correct results.

If you get errors like this when trying to run step-17 of the tutorials,
even though linking seems to have succeeded just fine:
```
   [make run
   ============================ Running step-17
   ./step-17: error while loading shared libraries: libpetsc.so: cannot open
              shared object file: No such file or directory
   make: *** [run](step-17]) Error 127
```

this means is that while linking, the compiler could find the libpetsc.so
library, but the executable can't find it when running. The reason is that
we can tell the linker where to look, but the executable apparently did not
remember this (this is the standard Unix behavior). What you have to do is
to set the LD_LIBRARY_PATH to include the path to the PETSc libraries. For
example, under `bash` you would have to do this:
```
   export LD_LIBRARY_PATH=/path/to/petsc/libraries:$LD_LIBRARY_PATH
```

If you do so, the Unix loader can query the environment variable for where
to find this particular library when trying to run the executable, and
running the program should succeed.

Similarly, if you get errors of the kind during linking
```
/home/xxx/deal.II/lib/libdeal_II.g.so: undefined reference to
`KSPSetInitialGuessNonzero(_p_KSP*, PetscTruth)'
/home/xxx/deal.II/lib/libdeal_II.g.so: undefined reference to
`VecAXPY(_p_Vec*, double, _p_Vec*)'
...
```

then the compiler can't seem to find the PETSc libraries. The solution is
as above: specify the path to those libraries via `LD_LIBRARY_PATH`.


#### Is there a sure-fire way to compile deal.II with PETSc?

Short answer is "No". The slightly longer answer is, "PETSc has too many
knobs, switches, dials, and a kitchen sink too many for its own damned
good. There is not a sure-fire way to compile deal.II with PETSc!". It
turns out that PETSc is a very versatile machine and, as such, there is no
shortage of things that can go wrong in trying to configure PETSc to work
seamlessly with deal.II on a first attempt. We have all struggled with
this, although it has become a lot better in recent years.

You can find instructions on how to install PETSc linked to from the
deal.II ReadMe file, or going directly to
http://www.dealii.org/developer/external-libs/petsc.html .

#### I want to use HYPRE through PETSc

Hypre implements algebraic multigrid methods (AMG) as preconditioners, for
example the BoomerAMG method. AMGs are among the most efficient
preconditioners available and they have also been shown to be scalable to
thousands of processors. deal.II allows the use of Hypre through the
PETScWrappers::PreconditionBoomerAMG class; it is used in `step-40`. Hypre
can be installed as a sub-package of PETSc and deal.II can access it
through the PETSc interfaces.

To use the Hypre interfaces through PETSc, you need to configure PETSc as
discussed in http://www.dealii.org/developer/external-libs/petsc.html  ,
and add the following switch to the command line: `--download-hypre=1`.

#### Is there a sure-fire way to compile dealii with SLEPc?

Happily, the answer to this question is a definite yes; that is, <b>if you
have successfully compiled and linked PETSc already</b>.

The real trick here is that during configuration SLEPc will pull out
PETSc's configuration and just does whatever that tells it to do. Detailed
steps are discussed in
http://www.dealii.org/developer/external-libs/slepc.html .

Once deal.II is compiled, it is worth to start by looking at the step-36
tutorial program to see how to get started using the interface with SLEPc.

<i>
Note: To use the solvers and other algorithms SLEPc provides it is
absolutely essential to have your PETSc installation working correctly
since they share the same vector-matrix (and other) data structures.
</i>


### Trilinos detection fails with an error in the file `Sacado.hpp` or `Sacado_cmath.hpp`

This is a complicated one (and it should also be fixed in more recent
Trilinos versions). In the Trilinos file `Sacado_cmath.hpp`, there is some
code of the form
```cpp
  namespace std {
    inline float acosh(float x) {
      return std::log(x + std::sqrt(x*x - float(1.0))); }
    ...
  }
```

In other words, Sacado is putting things into namespace `std`. The
functions it is putting there are functions that have been defined by the
C99 standard but that didn't make it into the C++98 standard before; some
of them are widely used. The problem is that these functions were later
added to the C++0x (now C++11) standard and so if your compiler is new
enough (e.g. GCC 4.5 and later) then the compiler's C++ standard library
already contains these functions. Adding them again in this file then
yields errors of the kind
```
/home/.../trilinos-10.4.2/include/Sacado_cmath.hpp: In function 'float std::acosh(float)':
/home/.../trilinos-10.4.2/include/Sacado_cmath.hpp:41:16: error: redefinition of 'float std::acosh(float)'
/usr/include/c++/4.5/tr1_impl/cmath:321:3: error: 'float std::acosh(float)' previously defined here
```

The only useful way to avoid this error is to edit the Trilinos header
file. To do this, find and open the file `include/Sacado_cmath.hpp` in the
directory in which Trilinos was installed. Then change the block enclosed
in
```cpp
  namespace std {
    ...
  }
```

to read
```cpp
#ifndef _GLIBCXX_USE_C99_MATH
  namespace std {
    ...
  }
#endif
```

What this will do is make sure that the new members of namespace `std` are
only added if the compiler has not already done so itself.




### My program links with some template parameters but not with others.

deall.II has many types for whose initialization you need to provide a
template parameter, e.g. `SparseMatrix<double>`. The implementation of
these classes can typically be found in files ending `.templates.h`, e.g.
`sparse_matrix.templates.h`. The corresponding `.cc` files, e.g.
`sparse_matrix.cc`, essentially only provide the explicit instantiations of
these classes for the most commonly used template parameters. Sometimes
this is done by including a corresponding `.inst` file, e.g.
`sparse_matrix.inst`.

If you want to use a data type with a template parameter for which there is
an explicit instantiation, you only need to include the respective `.h`
header file, e.g. `sparse_matrix.h`. If, however, you want to use a
template parameter for which there is no explicit instantiation in the
corresponding `.cc` file, you have to include the respective `.templates.h`
file in order for your program to link successfully.

The reason for all of this is essentially a matter of reducing compilation
time. As long as you use data types with template parameters for which
there is an explicit instantiation - and this should be the case most of
the time - you do not need to compile the respective (lengthy) .templates.h
file every time you compile your code. If, however, you need to use an
instance of e.g. `SparseMatrix<bool>`, you have to include the respective
`.templates.h` file and you have to compile it along with the remaining
files of your program every time.

### When trying to run my program on Mac OS X, I get image errors.

You may encounter an error of the form

```
dyld: Library not loaded: libdeal_II.g.7.0.0.dylib. Reason: image not found
```

on OS X. This goes hand in hand with the following message you should have
gotten at the end of the output of `./configure`:

```
     Please add the line
        export DYLD_LIBRARY_PATH=\$DYLD_LIBRARY_PATH:$DEAL2_DIR/lib
     to your .bash_profile file so that OSX will be
     able to find the deal.II shared libraries when
     executing your programs.
```

What happens is this: when you say "make all", all the deal.II files are
compiled and linked into a library (called libdeal_II.g.7.0.0) which on Macs
have the file ending .dylib. Then you go to examples/step-1 and compile your
program, which uses all the functions and classes that have previously been
put into this library.

Now the following happens: On most operating systems, the actual executable
program (i.e. the file step-1 in your directory that resulted from compiling)
does not contain any information that would indicate where the various
libraries that it uses can be found. For example, the step-1 program does not
know where the libdeal_II.g.7.0.0.dylib file is. This is just how most
operating systems function. But when you want to execute the program, somehow
the program has to know where the library it needs is located. On most
unix-like operating systems, this is done by setting an "environment
variable" -- on linux this would the variable "LD_LIBRARY_PATH", on Mac OS X
it is "DYLD_LIBRARY_PATH".

So to let the operating system know where the library is located, you could
type
  export DYLD_LIBRARY_PATH=$DYLD_LIBRARY_PATH:/Users/renjun/deal.ii/lib
every time before you want to execute the program. That would be cumbersome. A
simpler way would be if this export command is executed every time when you
open a new shell window. This can be achieved by putting this command in a
file that is executed every time you open a shell window. Depending on what
shell you use, these files are alternatively called
  .cshrc
  .bashrc
  .bash_profile
or similar. I'm not quite sure which file is relevant for you, but you can try
them one after the other by putting the text in there, closing the window,
opening it again, and then trying to execute
  ./step-1
(or saying "make run" in this directory) and seeing whether that works.

## C++ questions

### What integrated development environment (IDE) works well with deal.II?

The short answer is probably: whatever works best for you. deal.II uses
standard unix-style Makefiles, which most IDEs should support. In the past,
many of the main developers have used emacs (or even vi), but there are
much better tools around today, such as [eclipse](http://www.eclipse.org/),
[KDevelop](http://www.kdevelop.org),
[Xcode](http://developer.apple.com/technologies/tools/),
[QtCreator](http://qt.nokia.com/products/developer-tools/), all of which
have been used by people using deal.II.

We have gathered some notes on using the following IDEs for deal.II:
  - [[Eclipse]]
  - [[KDevelop]]
  - [[emacs]]: While we don't recommend using emacs any more, this link provides a couple of notes on formatting styles used within deal.II.

When thinking about what IDE to use, keep this in mind: Many of us have
used emacs (or, worse, vi) for years and feel very comfortable with it.
But, emacs and vi were both started in 1976, at a time when computers had
little memory, virtually no CPU power, and only text-based interfaces.
While they have of course become a lot better over time, the design
limitations this involved are still very much part of the code base:
fundamentally, they are both still text-based and file-oriented. What IDEs
can provide are multiple views of the same project in graphical and textual
form and, more importantly, can integrate entire projects spanning hundreds
of files in multiple directories: they know where a variable is declared
(even if it's in a different file), what it's type is, and the properties
of this type. Neither emacs nor vi nor any other older editor can provide
anything that comes even close to what kdevelop or eclipse can offer in
this regard.

What all this implies is that you should consider using one of the more
modern tools, even if you're well acquainted with an existing, older one.
Of course it takes a while to get used to a new application but my
(Wolfgang's) experience with switching from emacs to kdevelop was that I
have become '''so''' much more productive by using modern tools that the
time invested in learning it was amortized very quickly. I found this
experience a real eye-opener!

### Is there a good introduction to C++?

There are of course many good books and online resources that explain C++.
As far as websites are concerned,
[www.cplusplus.com](http://www.cplusplus.com) has both [reference material
for individual classes of the C++ standard
library](http://cplusplus.com/reference/) as well as a [a tutorial on parts
of the C++ language](http://cplusplus.com/tutorial) if you want to brush up
on the correct syntax of things.

### Are there features of C++ that you avoid in deal.II?

There are few things that we avoid <i>as a matter of principle.</i> C++ is,
by and large, a pretty well designed language in the sense that its
features are there because they have been found to be useful by a lot of
people. As an example, people have found that it is easier to write and
debug code that throws exceptions in error cases rather than encoding error
situations by special return values (e.g. by returning -1). There are of
course ways to avoid exceptions (or templates, or certain parts of the C++
standard libraries, or any number of other things people have found
objectionable in C++) and some software projects have chosen to restrict
the use of C++ (for example Mozilla) or to emulate only those parts of C++
they like in C (e.g. the GNOME desktop environment, which leads to awkward
to understand code
[as described here](http://developer.gnome.org/gobject/stable/howto-gobject-methods.html)).

But ultimately, it is our belief that these approaches shoot their
inventors in the foot: they avoid features of C++ that were really intended
to make programming life simpler. It may be simpler for novice programmers
to read code without templates; ultimately, however, learning to read and
use templates will make you a much more productive programmer since you
don't write the same code multiple times. As a consequence, the use of C++
is driven by the question of what is best suited to write a particular
algorithm, not by abstract considerations. This fits into the realization
that deal.II is a large piece of software -- not a small research project
-- that requires professional software management practices and for which
long term development can no longer be driven by an individual programmer's
preferences of style.

### Why use templates for the space dimension?

The fundamental motivation for this is to use dimension-independent
programming, i.e. you want to write code in such a way that it looks
exactly the same in 2d as in 3d (or 1d, for that matter). There are of
course many ways to do this (and libraries have done this for a long time
before deal.II has). The three most popular ones are to use a preprocessor
`#define` that sets the space dimension globally, to use a global variable
that does this, and to have each object have a member variable that denotes
the space dimension it is supposed to live in (in much the same way as the
template argument does in deal.II). Neither of these approaches is optimal
(nor is our own approach to use templates), however. In particular, using a
preprocessor symbol or a global variable will not allow you to mix and
match objects of different dimensionality. There are situations when you
want to do that; for example deal.II internally builds higher dimensional
quadrature formulas as tensor products of lower dimensional ones, and in
application codes you may wish to discretize both volume models (e.g.
simulating 3d models of plate tectonics and mountain belt formation) with
surface models (e.g. erosion processes on the 2d earth surface).

This leaves the option to have a member variable denoting the space
dimension in each object, a choice most other finite element libraries have
followed. But this isn't optimal either, for two reasons. For example,
consider this code that describes the equivalent of the `Point<dim>` class
for points in dim-dimensional space and its `norm()` member function:

```cpp
class Point
{
public:
  Point (const unsigned int dimension)
    : dim(dimension),
      coordinates (new double[dim])
  {}

  ~Point() { delete[] coordinates; }

  double norm () const;
  ...
private:
  unsigned int dim;
  double *coordinates;
};

double Point::norm () const
{
  double s = 0;
  for (unsigned int d=0; d<dim; ++d)
    s += coordinates[d] * coordinates[d];
  return std::sqrt(s);
}
```

This is going to lead to rather slow code, for multiple reasons:

 - The constructor and destructor have to allocate and deallocate memory on
   the heap, both expensive processes.

 - When accessing any element of the `coordinates` array, two pointers have
   to be dereferenced. For example, the access to `coordinates[d]` really
   expands to `*(this->coordinates + d)`.

 - The compiler can not optimize the loop since the upper bound `dim` of
   the loop variable is unknown at compile time.


Compare this to the way deal.II (approximately) implements this class:

```cpp
  template <int dim>
  class Point
  {
    public:
      Point () {}
      ~Point() {}

      double norm () const;
      ...
    private:
      double coordinates[dim];
  };

  template <int dim>
  double Point<dim>::norm () const
  {
    double s = 0;
    for (unsigned int d=0; d<dim; ++d)
      s += coordinates[d] * coordinates[d];
    return std::sqrt(s);
  }
```

Here, the following holds:

 - Constructor and destructor do not have to allocate and deallocate memory
   on the heap; rather, since the size of the `coordinates` array is known
   at compile time (i.e. whenever you instantiate the template for a
   particular dimension), the array lives on the stack. It is also much
   smaller than before: the dimension is encoded in the type and doesn't
   need a memory location, we don't need to store a pointer to an array,
   and we don't incur the memory overhead of having to manage an object on
   the heap.

 - When accessing any element of the `coordinates` array, only one pointer
   has to be dereferenced. For example, the access to `coordinates` really
   expands to `*(this + d)`.

 - The compiler can optimize the loop since the upper bound `dim` of the
   loop variable is known at compile time. In particular, for a point in
   2d, the code the compiler will produce is likely to look more like this
   because the loop can be unrolled and the loop counter can be optimized
   away:
```cpp
double Point<2>::norm () const
{
  return std::sqrt(coordinates[0] * coordinates[0] + coordinates[1] * coordinates[1]);
}
```
Obviously, for a 3d point, the code will look differently, but the compiler
can do this since it knows what the dimension of the point is at compile
time.

There is another reason for the deal.II way: type safety. In short, a 2d
point is not the same as a 3d point. If you assign one to the other, then
this may be on purpose and the executable should simply change the value of
the `dim` member variable from 2 to 3. But it may also be a legitimate
error -- for example, you shouldn't be able to use 2d points to initialize
the 3d quadrature points needed to integrate on a 3d cell. This can of
course be caught by run-time checks, but the reason for strongly typed
languages such as C++ has always been that it is much more efficient if the
compiler can already catch this sort of error at compile time. Using
templates for the space dimension avoids these sort of mistakes up front by
forcing the programmer to explicitly specify her intent, rather than
encoding intent in assertions.

Of course there are also downsides to using templates. Most notably, error
messages that involve templates are notoriously unreadable, and that
compiling template heavy code is slow: for example, we have to compile the
`Point` class three times (for dim=1, dim=2 and dim=3) rather than only
once. Nevertheless, we believe that these valid objections do not outweigh
the benefits of templates.

### Doesn't it take forever to compile templates?

Yes, in general it does. The reason is that while for non-templates it is
enough to put the ''declaration'' of a function into the header file and
the ''definition'' into the `.cc` file, for templates that doesn't work.
Let's say you have something like
```cpp
  template <typename T> T square (const T & t);
```

in your header file and you put the definition
```cpp
  template <typename T> T square (const T & t) { return t*t; }
```

into the `.cc` file, then the compiler will say "Yes, I saw this template,
and if I see a use of this function later on I will generate a function
from it by replacing `T` by whatever type you use in the call". But if
there is no call later on in the same `.cc` file, then the compiler won't
do anything. If, at the same time, in a different `.cc` file that includes
the header file, you use the function with `T=double` the compiler will say
"Yes, I saw the declaration, but there is no definition; I assume the
function has been compiled in a different `.cc` file with `T=double` and
I'll simply record a call to this instantiation in the object file". The
call will then be resolved at link time if indeed another object file
contains an instantiation of the template for `T=double`. However, if no
other object file contains such a definition, a linker error will result.

In general, for functions like the above, it is difficult to foresee what
kinds of template arguments the function may be instantiated for, and so
there is no real practical way to put the definition into a `.cc` file.
Rather, one puts it into a header file, and so all `.cc` files that may use
this function see its definition (i.e. its body) and the compiler can
instantiate it in each source file for whatever template argument is
necessary. This makes sure that you never get linker errors, but at the
same time it makes compiling slow since every header file now not only has
to parse the function's declaration, but also its definition -- and in the
case of deal.II the definitions of all template functions add up to tens or
hundreds of thousands of lines of code. This is one of the reason why many
C++ programs compile relatively slowly: because they use a significant part
of the C++ standard library, most of which consists of templates.

deal.II can avoid much of this overhead. The trick is to recognize that in
the example above we don't really know what types `T` user code may
possibly want to use for this template. But in the case of using the space
dimension as a template parameter, we know pretty exactly all the possibly
values: `Triangulation<dim>` may really only be instantiated for `dim=1, 2,
3` and for nothing else. Consequently, we can do the following: Put all the
definitions of the member functions of deal.II into the `.cc` file and at
the bottom of the file instruct the compiler to please instantiate all of
these templates for `dim=1, 2, 3`. Similar things can be done for many
other template functions in deal.II; for example, there are a good number
of functions that require vector types as template arguments, of which
deal.II provides a good number, yet this list is finite and enumerable.
Consequently, we can simply, at the bottom of the `.cc` file, tell the
compiler to instantiate all of these template functions for every single
vector type deal.II supports, and then don't have to put thousands of lines
of template definitions into header files.

In many cases, enumerating all possible template arguments is tedious; it
is also difficult to extend this list when a new vector type is added, for
example. To simplify this task, deal.II uses a preprocessor: for many files
that want to instantiate a function or class for multiple template
arguments, we have a file `.inst.in` that has the equivalent of a
`for`-loop over all possible values or types for a template argument; the
file is processed by the `common/scripts/expand_instantiations` program to
produce a `.inst` file that can then be included into the `.cc` file.

### Why do I need to use `typename` in all these templates?

This is indeed a frequent question. To answer it, it is necessary to
understand how a compiler deals with templates, which will take a bit of
space here. Let's take for example this case:

```cpp
  void f(int);
  void g(double d) {
    f(d);
  }
  void f(double);
```

Here, in the function `void g(double)`, we call `f` with a double as an
argument. Because at that point the compiler has only seen the declaration
of the first overload of `f`, it will convert the double `d` to an integer
and call this first overload. The fact that a second overload was declared
later does not change this situation, since it wasn't visible at the time
the compiler parsed `g`.

Templates are designed to work essentially the same, but there are slight
complications. Take this example:

```cpp
  void f(int);
  void f(char);
  template <typename T> void g(T t) {
    f(1.1);
    f(t);
  }
  void f(double);
```

In the first line of `g`, the same thing happens as before: the argument is
cast to `int` and the first of the two overloads of `f` is called. But when
the compiler sees the template, it doesn't know yet what type `T` actually
represents, so there is no way to settle on one of the two functions `f`
the compiler has seen before when deciding about the second line. In fact,
the C++ standard says that because the type of the argument `t` in the call
depends on the template type, determining what function to actually call
should only happen <i>at the time and place when the template is
instantiated</i> (this is called <i>argument dependent name lookup</i> or
<i>ADL</i>). In other words, if below the code above we had this:
```cpp
  void h() {
     g(1.1);
  }
```

then in the instantiation of `g` the first call would be to `f(int)`
(because the argument 1.1 does not depend on the type given in the template
argument, and consequently only functions are considered that were seen
<i>before the definition of</i> `g(T)`) whereas the second call to `f`
would be to `f(double)` -- even though `f(double)` wasn't even declared at
the place the compiler saw the call in the template (though it is available
at the place where we instantiate `g<double>`) -- because the function call
argument `t` has type `T` and therefore depends on the template argument.

Argument dependent lookup allows you to use function templates like `g`
with your own data types. For example, you could have your own library that
does
```cpp
  #include <f.h>

  struct X { /* something */ };

  void f (const X & x) { /* do something with the X */ }

  void my_function() {
    X x;
    g(x);
  }
```

Presumably the writer of the `g` function did not know about your own type
`X` yet, but her code still works because you provided a suitable overload
of `f` in your own code.

So ADL is clever and allows you to use templates in ways the author of the
template did not anticipate. But it has a dark side: for every statement in
your code, the compiler has to figure out whether it depends on the
template types or not, and it needs in fact to know quite a lot about it.
Take this example:

```cpp
  int p;
  template <typename T> void g(T t) {
    T::something * p;
    f(p);
  }
```

Here, is the call to `f` dependent because `p` depends on the type `T`? If
`f` is called with an argument of type `X` that is declared like this

```cpp
  struct X {
    typedef int something;
  };
```

then `T::something * p;` would declare a local variable called `p` that is
of type pointer-to-int. On the other hand, if we had

```cpp
  struct X {
    static double something;
  };
```

then `T::something * p;` multiplies the variable `X::something` by the
global variable `p` and ignores the result of the multiplication. The
following call to `f` would then be non-dependent because the type of the
(global) variable `p` does not depend on the template argument.

The example shows that the compiler can't know whether a call is dependent
or not in a template it is just seeing unless we tell it that
`T::something` is supposed to be a type or a variable or function name. To
avoid this situation, C++ says: if a compiler sees `T::something` then this
is a variable or function name unless it is prefixed by the keyword
`typename` in which case it is supposed to be a type. In other words, the
call to `f` here is going to be non-dependent:
```cpp
  int p;
  template <typename T> void g(T t) {
    T::something * p;
    f(p);
  }
```

and instantiating `g` with the first example for `X` is going to lead to
errors because `T::something` didn't turn out to be a variable. On the
other hand, if we had
```cpp
  int p;
  template <typename T> void g(T t) {
    typename T::something * p;
    f(p);
  }
```

then the call is dependent and will be deferred until the compiler knows
the type of `T`.

### Why do I need to use `this->` in all these templates?

This is a consequence of the same rule in the C++ standard as discussed in
the previous question, Argument Dependent Lookup of names (ADL). Consider
this piece of code:
```cpp
  template <typename T> class Base {
    public:
      void f();
  };

  template <typename T> class Derived : public Base<T> {
    public:
      void g();
  };

  template <typename T> void Derived<T>::g() {
    f();
  }
```

By the rules, when the compiler <i>parses</i> the function `Derived::g`
(note that parsing happens before and independently of <i>instantiating</i>
the function for a particular argument type `T`), it sees that the call to
`f()` does not depend on the template type and so it looks for a
declaration of such a function somewhere. In the example above, it doesn't
find one (we'll come to this in a second), which will yield an error. On
the other hand, in this code,
```cpp
  void f(); // global function

  template <typename T> class Base {
    public:
      void f();
  };

  template <typename T> class Derived : public Base<T> {
    public:
      void g();
  };

  template <typename T> void Derived<T>::g() {
    f();
  }
```

it would find the global function and so when instantiating the function
for, say, `T=int`, you'd get a function `Derived<int>::g` that would call
the global function `::f`. This may or may not be what you had in mind.

The question of course is why the compiler didn't record a call to
`Base<T>::f` in `Derived<int>::g`? After all, the compiler knows that
`Derived` is derived from `Base`. This has a lot to do with the fact that
at the time of <i>parsing</i> the template, the compiler doesn't know for
which template arguments the template will later be instantiated, and with
explicit or partial specializations.  Consider for example this code:
```cpp
  template <typename T> class Base {
    public:
      void f();
  };

  class X {
    public:
      void f();
  };

  template <> class Base<int> : public X {};

  template <typename T> class Derived : public Base<T> {
    public:
      void g();
  };

  template <typename T> void Derived<T>::g() {
    f();
  }
```

Here, if you look at `Derived<T>::g`, the call to `f()` will be resolved to
`Base<T>::f` for all possible types `T`, unless `T=int` in which case the
call will be to `X::f`. The point is that at the time the compiler sees
(parses) the template, it simply doesn't know yet what `T` is, and so ADL
says: if the call is not dependent, find a non-dependent function to record
(e.g. a global function) rather than trying to find a call in scopes you
can't yet know will be relevant (e.g. `Base` or `X`). Likewise, in this
code,
```cpp
  template <typename T> class Base {
    public:
      void f();
  };

  template <> class Base<int> {
    public:
      struct f {};
  };

  template <typename T> class Derived : public Base<T> {
    public:
      void g();
  };

  template <typename T> void Derived<T>::g() {
    f();
  }
```

the meaning of `f()` changes depending on the template type: if `T=int`, it
creates an object of type `Base<int>>::f` and then throws the object away
again immediately. For all other template arguments `T`, it calls
`Base::f`.

Given this longish description of how compilers look up names under the ADL
rule, let's get back to the original question: If you have this code,
```cpp
  template <typename T> class Base {
    public:
      void f();
  };

  template <typename T> class Derived : public Base<T> {
    public:
      void g();
  };

  template <typename T> void Derived<T>::g() {
    f();
  }
```

how do you achieve that the call in `Derived::g` goes to `Base::f`? The
answer is: Tell the compiler to defer the decision of what the call is
supposed to do till the time when it knows what `T` actually is. And we've
already seen how to do that: we need to make the call <i>dependent</i> on
`T`! The way to do that is this:
```cpp
  template <typename T> void Derived<T>::g() {
    this->f();
  }
```

Here, `this` is a pointer to an object of type `Derived<T>`, which is of
course dependent. So the resolution of what the statement is supposed to
represent is deferred until instantiation time; at that time, however, the
compiler knows what the base class is (for example it knows if there are
explicit specializations) and so it knows which base classes to look into
in an attempt to find a function with the name `f`.

### Does deal.II use features of C++11 (formerly known as C++0x or C++1x)?

We strive to keep deal.II compatible with the previous C++ standard, C++98,
to make sure that deal.II can be built by all widely available compilers on
current and recent operating systems. This typically prevents the use of
new language features. That said, we occasionally use things from C++11 for which we have backup solutions for compilers that do not provide them.

The deal.II documentation has a page dedicated to the issue of what parts of C++11 we use and how this works: at http://dealii.org/developer/doxygen/deal.II/group__CPP11.html .

### Can I convert Triangulation cell iterators to DoFHandler cell iterators?

Yes. You can also convert between iterators belonging to different
DoFHandlers as long as the are based on the identical Triangulation:

```cpp
Triangulation<2>::active_cell_iterator it = triangulation.begin_active();

DoFHandler<2>::active_cell_iterator it2 (&triangulation, it->level(), it->index(), &dof_handler);
```

## Questions about specific behavior of parts of deal.II

### How do I create the mesh for my problem?

Before answering the immediate question, one remark: When you use adaptive
mesh refinement, you definitely want the initial mesh to be as coarse as
possible. The reason is that you can make it as fine as you want using
adaptive refinement as long as you have memory and CPU time available.
However, this requires that you don't waste mesh cells in parts of the
domain where they don't pay off. As a consequence, you don't want to start
with a mesh that is too fine to start with, because that takes up a good
part of your cell budget already, and because you can't coarsen away cells
that are in the initial mesh.

That said, there are essentially three ways to generate a mesh, all of
which are discussed in significantly more detail in the step-49 tutorial
program:
 - For many standard geometries (square, cube, circle, sphere, ...) there
   are functions in namespace `GridGenerator` that can generate coarse
   meshes.
 - If `GridGenerator` does not offer a mesh for the geometry you have, but
   if the geometry is simple, then you can often create one "by hand". Take
   a look, for example, at how we create the mesh in step-14 using the
   `Triangulation::create_triangulation` function. All you need to do is
   take a piece of paper, draw the geometry and a number of coarse cells
   that form quadrilaterals, identify the locations of vertices and the
   connectivity from cells to vertices, and pass the corresponding lists to
   the Triangulation. Something similar can be done for simple 3d
   geometries.
 - If your geometry is truly complicated enough so that you can't draw a
   mesh by hand any more (i.e. if it requires more than, for example, 20-30
   coarse mesh cells), then you'll need a mesh generator. For
   quadrilaterals and hexahedra, there aren't all that many mesh
   generators. [http://geuz.org/gmsh/ gmsh](1]);),
   [lagrit](https://lagrit.lanl.gov/) and [cubit](http://cubit.sandia.gov/)
   come to mind. The primary problem is that most mesh generators' output
   meshes aren't particularly coarse by default, so you may want to pay
   particular attention to this point when running the mesh generator.
   (This is relevant since deal.II is particularly good about creating
   adaptively refined meshes, but if your coarse mesh is already very large
   then you will likely not have a lot of resources left to adaptively
   refine it some more.) Once you have a mesh from a mesh generator, you
   would read it using the `GridIn` class, as demonstrated, for example, in
   step-5.

### How do I describe complex boundaries?

You need to define classes derived from the `Boundary` base class and
attach these to particular parts of the boundary of the triangulation. The
`Triangulation` class will then query your boundary object whenever it
needs a new point on the boundary after mesh refinement.

In deal.II releases after 8.1, the way geometry is described has been made
much more flexible. In particular, it is no longer only possible to
describe the boundary, but it is also possible to describe where points in
the interior lie. The step-53 tutorial program explains how this is done
for a realistic example.


### I am using discontinuous Lagrange elements (`FE_DGQ`) but they don't seem to have vertex degrees of freedom!?

Indeed. And here's the reason: a vertex is an entity that is shared between
different cells, i.e. it doesn't belong to one cell or another. If you have
a shape function that is associated with it, then its support will extend
to all of the cells that are adjacent to the vertex since no cell is
different than any other cell. This is what happens, for example, with the
`FE_Q(1)` element. The same is true, by the way, for degrees of freedom
(and associated shape functions) that correspond to edges and faces between
cells.

But that doesn't answer the question of discontinuous elements. There, you
have functions that are interpolation polynomials whose <i>support
point</i> happens to be located at the same position as the vertex, but the
actual support of the shape function is restricted to a single cell. In
other words, '''logically''' these shape functions belong to a cell, not a
vertex or edge or face, since the latter are all shared between adjacent
cells. What this leads to is that, for example for the `FE_DGQ(1)` element,
you have
 - `fe.dofs_per_vertex` is zero
 - `fe.dofs_per_line` is zero
 - `fe.dofs_per_face` is zero
 - `fe.dofs_per_cell` is 4 in 2d and 8 in 3d.
In other words, all shape functions are associated with the cell interior.

If this answer isn't quite satisfactory (because, after all, the shape
functions <i>are</i> defined by interpolation at the location of the
vertices), one could turn the question around: If you ask me for the degree
of freedom associated with vertex 13, then I should ask you in return
<i>which one</i> you have in mind since if there, say, four cells that meet
at this vertex, then there will be 4 degrees of freedom defined there.
Likewise, if you ask me for the value of the degree of freedom associated
with vertex 13, then I should ask you in return <i>which one</i> as the
function is discontinuous there and will have multiple values at the
location of the vertex.

### How do I access values of discontinuous elements at vertices?

The previous question answered why DG elements aren't defined at the
vertices of the mesh. Consequently, functions like `cell->vertex_dof_index`
aren't going to provide anything useful. Nevertheless, there are occasions
where one would like to recover values of a discontinuous field at the
location of the vertices, for example to average the values one gets from
all adjacent cells in recovery estimators.

So how does one do that? The answer is: Getting the values at the vertices
of a cell works just like getting the values at any other point of a cell.
You have to set up a quadrature formula that has quadrature points at the
vertices and then use an FEValues object with it. If you then use
FEValues::get_function_values, you will get the values at all quadrature
points (i.e. vertices) at once.

Setting up this quadrature formula can be done in two different ways: (i)
You can create an object of type `Quadrature` from a vector of points that
you can initialize with the reference coordinates of the 2<sup>dim</sup>
vertices of a cell; or (ii) you can use the `QTrapez` class that has its
quadrature points in the vertices. In the latter case, however, you need to
verify that the order of quadrature points is indeed the same order as the
vertices of a cell and, if that is not the case, translate between the two
numbering systems.

### Does deal.II support anisotropic finite element shape functions?

There is currently no easy-to-use support for this. It's not going to work
for continuous elements because we assume that `fe.dofs_per_face` is the
same for all faces of a cell.

It may be possible to make this work for discontinuous elements, though.
What you would have to do is define a bunch of different elements with
anisotropic shape functions and select which element to use on which cells,
using the `hp::DoFHandler` to deal with using different elements on
different cells. The part that's missing is to implement elements with
anisotropic shape functions. I imagine that this wouldn't be too
complicated to do since the element is discontinuous, but someone would
have to implement it.

That said, you can do anisotropic <i>refinement</i>, which of course also
introduces a kind of anisotropic approximation of your finite element
space.

### The graphical output files don't make sense to me -- they seem to have too many degrees of freedom!

Let's assume you have a 2x2 mesh and a Q<sub>1</sub> element then you would
assume that output files (e.g. in VTK format) just have 9 vertex locations
and 9 values, one for each of the 9 nodes of the mesh. However, the file
actually shows 16 vertices and 16 such values.

The reason is that frequently output quantities in deal.II are
discontinuous: it may be that the finite element in use is discontinuous to
begin with; or that the quantity we want to output is defined on a
cell-by-cell basis (e.g. error indicators) and therefore discontinuous; or
that it is a quantity computed from a DataPostprocessor object that could
be discontinuous. In order to not make things more complicated than
necessary, deal.II <i>always</i> assumes that quantities are discontinuous,
even if some of them may in fact be continuous. The problem is that all
graphical formats want to see one value for each output field per vertex.
But discontinuous fields have more than one value at the location of a
vertex of the mesh. The solution to the problem is then to simply output
each vertex multiple times -- with different vertex numbers but at exactly
the same location, once for each cell it is adjacent to. In other words, in
2d, each cell has four unique vertices. The 2x2 mesh in the example
therefore has 16 vertices (4 vertices for each of the 4 cells) and we
output 16 values. Several of these vertices will have the same location and
if the field is indeed continuous, several of the values will also be the
same.

### In my graphical output, the solution appears discontinuous at hanging nodes

Let me guess -- you are using higher order elements? If that's the
case, then the solution only looks discontinuous but isn't
really. What's happening is that the solution is, in fact, a higher
order polynomial (e.g., a quadratic polynomial) along each edge of a
cell but because all visualization file formats only support writing
data as bilinear elements we need to write data in a way that shows
only a linear interpolation of this higher order polynomial along each
edge. This is no problem if the two neighboring elements share the
entire edge because then the linear interpolations from both sides
coincide. However, if we have a hanging node, then the value at the
hanging node appears to float above or below the linear interpolation
from the longer side, like here (in the left picture, see the gap at
the bottom in the blue green area, and around the top left in the
greenesh area; pictures by Kevin Dugan):

<img width="400px" src="http://www.dealii.org/images/wiki/gap-in-q2-1.png" align="center" />
<img width="400px" src="http://www.dealii.org/images/wiki/gap-in-q2-2.png" align="center" />

From this description you can already guess what the solution is: the
solution is internally in fact continuous: even though we only show a
linear interpolation on the long edge, the true solution actually goes
through the "floating" node. All this is, consequently, just an
artifact of the way visualization programs show data.

If this bothers you or it simply looks bad in your graphics, you can
lessen the problem by not plotting just a linear interpolation on each
cell but outputting the solution as a linear interpolation on a larger
number of "patches" per cell (e.g., plotting 5x5 patches per
cell). This can be done by using the `DataOut::build_patches` function
with an argument larger than one -- see its documentation.

This all said, if you are in fact using a Q1 element and you see such
gaps in the solution, then something is genuinely wrong. One
possibility is that you forget to call `ConstraintMatrix::distribute`
after solving the linear system, or you do not set up these
constraints correctly. In either case, it's a bug if this happens with
Q1 elements.

### When I run the tutorial programs, I get slightly different results

This is sometimes unavoidable. deal.II uses a number of iterative
algorithms (e.g. in solving linear systems, but the adaptive mesh
refinement loop is also an iteration if you think about it) where certain
criteria are specified by comparing floating point numbers. For example,
the CG method terminates the iteration whenever the residual drops below a
certain threshold; similarly, we refine as many cells as are necessary to
take care of a fraction of the total error. In both cases, the quantities
that are compared are floating point numbers which are subject to floating
point round off. The problem is that floating point round off depends on
the processor (sometimes), compiler flags or randomness (if parallelization
is involved) and consequently an a solver may terminate one iteration
earlier or later, depending on your environment, than the one from which we
produced our results. With a different solution typically come different
refinement indicators and different meshes downstream.

In other words, this is something that simply happens. What should worry
you, however, is if you run the same program twice and you get slightly
different output. This hints at non-deterministic effects that one should
investigate.

### How do I access the whole vector in a parallel MPI computation?

Note that this causes a bottleneck for large scale computations and you
should try to use a parallel vector with ghost entries instead. If you
really need to do this, create a TrilinosWrappers::Vector (or a
PETScWrappers::Vector) and assign your parallel vector to it (or use a copy
constructor). You can find this being done in step-17 if you search for
"localized_solution".

### How to get the (mapped) position of support points of my element?

Option 1: The support points on the unit cell can be accessed using
FiniteElement::get_unit_support_point(s) and mapped to real coordinates
using Mapping::transform_unit_to_real_cell()

Option 2: DoFTools::map_dofs_to_support_points() maps all the support
points at once.

Option 3: You can create a FEValues object using the support points as a
Quadrature:
<pre>
Quadrature<dim> q(fe.get_unit_support_points());
FEValues<dim> fe_values (..., q, update_q_points);
...
fe_values.get_quadrature_points();
</pre>


## Debugging deal.II applications

### I don't have a whole lot of experience programming large-scale software. Any recommendations?

Yes. First, the questions of this FAQ already give you a number of good
pointers for example on debugging. Also, a good resource for some of the
questions mathematicians, scientists and engineers (who may have taken a
programming course, but know little of the bigger world of software
engineering) typically have, is the [Software
Carpentry](http://software-carpentry.org/) page. That site is specifically
targeted at people who may want to use scientific computing to solve
particular applications, but have little or no formal training in dealing
with large software. In other words, it is specifically written for people
for an audience like the users of deal.II.


### Are there strategies to avoid bugs in the first place?

Why yes, good you ask. There are indeed techniques that help you avoid
writing code that has bugs. By and large, these techniques go by the name
<i>defensive programming</i>, and the idea is to get yourself into a
mindset while programming that anticipates that you will make mistakes,
rather than expecting that your code is correct and then reacting to the
situation when it turns out that this isn't true. The point is that even
the most experienced programmers do introduce a lot of bugs into their
code; what makes them good is that they have strategies to find them
quickly and systematically.

Below we show one of the most important lessons learned. A more complete
list can be found in [our code conventions
page](http://dealii.org/developer/doxygen/deal.II/CodingConventions.html)
which has a collection of best practices including code snippets to show
how they are used.

The single most successful strategy to avoid bugs is to <i>make assumptions explicit</i>. For example, assume for a second that you have a class that denotes a point in 3d space:
```cpp
  class Point3d
  {
  public:
    double coordinate (const unsigned int i) const;
    // ...more here...
  private:
    double coordinates[3];
  };

  double
  Point3d::coordinate (const unsigned int i) const
  {
    return coordinates[i];
  }
```

Here, when we wrote the `coordinate()` function, we worked under the
assumption that the index `i` is between zero and two. As long as that
assumption is satisfied, everything is fine. The problems start when
someone calls this function with an index greater than two -- the function
will in that case simply return garbage, but that may not be immediately
obvious and may only much later lead to weird results in your program.
Inexperienced programmers will say "Why would I do that, it doesn't make
any sense!". Defensive programming starts from the premise that this is
something that simply <i>will happen</i> at one point in time, whether you
want to or not. It's actually not very difficult to do, since all of us
have probably written code like this:
```cpp
  Point3d point;
  // ... do something with it
  double norm = 0;
  for (unsigned int i=0; i<=3; ++i)
    norm += point.coordinate(i) ** point.coordinate(i);
  norm = std::sqrt(norm);
```

Note that we have accidentally used `<=` instead of `<` in the loop.

If we accept that bugs will happen, we should make it as simple as possible
to find them. In the spirit of making assumptions explicit, let's write
above function like this:
```cpp
  double
  Point3d::coordinate (const unsigned int i) const
  {
    if (i >= 3)
      {
        std::cout << "Error: function called with invalid argument!" << std::endl;
        std::abort ();
      }
    return coordinates[i]
  }
```

This has the advantage that an error message is produced whenever the
function is called with invalid arguments, and for good measure we also
abort the program to make sure the error message can really not be missed
in the rest of the output of the program. The disadvantage is that this
check will always be performed whenever the program runs, even if it is
well tested and we are fairly certain that in all places where the function
is called, indices are valid. To avoid this drawback, the C programming
language has the `assert` macro, which expands to the code above by
default, but that can be disabled using a compiler flag. deal.II provides
an improved version of this macro that is used as follows:
```cpp
  double
  Point3d::coordinate (const unsigned int i) const
  {
    Assert (i<3, ExcMessage ("Function called with invalid argument!"));
    return coordinates[i];
  }
```

The macro expands to nothing in optimized mode (see below), and if it is
triggered in debug mode it doesn't only abort the program, but also prints
an error message and shows how we got to this point in the program.

Using assertions in your program is the single most efficient way to make
assumptions explicit and help find bugs in your program as early as
possible. If you are looking for some more background, check out the
wikipedia articles on
[assertions](http://en.wikipedia.org/wiki/Assertion_(computing)),
[preconditions](http://en.wikipedia.org/wiki/Precondition) and
[postconditions](http://en.wikipedia.org/wiki/Postcondition), and generally
the [design by contract
methodology](http://en.wikipedia.org/wiki/Design_by_contract).

### How can deal.II help me find bugs?

In addition to using the `Assert` macro introduced above, the deal.II
libraries come in two flavors: debug mode and optimized mode. The
difference is that the debug mode libraries contain a lot of assertions
that verify the validity of parameters you may pass when calling library
functions and classes; the optimized libraries don't contain these and are
compiled with flags that instruct the compiler to optimize. This makes
executables linked against the optimized libraries between 4 and 10 times
faster. On the other hand, you will find that you will find 90% or more of
your bugs by using the debug libraries because most bugs simply pass data
to other functions that they don't expect or that don't make sense. The
consequence is that you should always use debug mode when you are still
developing your code. Only when it runs without bugs -- and under no
circumstances any earlier -- should you switch to optimized mode to do
production runs. One of the silliest things you can do is switch to
optimized mode because you otherwise get an error you can't make sense of
and that you don't know how to fix; certainly, if the library complains
about something and you ignore it, nothing good can come out of the
remainder of the run of your program.

To switch between debug and optimized mode, take a look at the top of the
deal.II-provided Makefiles: there's a flag you can set that switches
between the two modes.

### Should I use a debugger?

This question has an emphatic, unambiguous answer: Yes! You may get by for
a while by just putting debug output into your program, compiling it, and
running it, but ultimately finding bugs with a debugger is much faster,
much more convenient, and more reliable because you don't have to recompile
the program all the time and because you can inspect the values of
variables and how they change. Learn how to use a debugger as soon as
possible. It is time well invested.

Debuggers come in a variety of ways. On Linux and other Unix-like operating
systems, they are almost all based in one way or other on the [GNU Debugger
(GDB)](http://www.gnu.org/s/gdb/). GDB itself is a tool that is driven by
interactively typing commands; if you know your way around with it, it is
quite useable but it is rather austere and unless you are already familiar
with this style of debugging, don't learn it. Rather, you should either use
a graphical front-end or, even better, a front-end to GDB that is
integrated into an Integrated Development Environment (IDE). An example of
the stand-alone graphical front-ends to GDB are
[DDD](http://www.gnu.org/software/ddd/), a program that was the first of
its kind on Linux for many years but whose development has pretty much
ceased in the early 2000s; it is still quite a good program, though.
Another example is [KDbg](http://kdbg.org/), a GDB front-end for the KDE
desktop environment.

As mentioned, a better choice is to use a debugger front-end that is
integrated into the IDE. Every decent IDE has an integrated debugger, so
you have your choice. A list of IDEs and how they work with deal.II is
given in the C++ section of this FAQ.

### deal.II aborts my program with an error message

You are likely seeing something like the following:
```
--------------------------------------------------------
An error occurred in line <1302> of file </u/bangerth/p/deal.II/1/deal.II/include/deal.II/lac/vector.h> in function
    Number& dealii::Vector<Number>::operator()(unsigned int) [Number = double](with)
The violated condition was:
    i<vec_size
The name and call sequence of the exception was:
    ExcIndexRange(i,0,vec_size)
Additional Information:
Index 10 is not in [Stacktrace:
-----------
#0  ./step-1: dealii::Vector<double>::operator()(unsigned int)
#1  ./step-1: foo()
#2  ./step-1: main
--------------------------------------------------------
```

This error is generated by the following program:
```cpp
#include <lac/vector.h>
using namespace dealii;

void foo ()
{
  Vector<double> x(10);
  for (unsigned int i=0; i<=x.size(); ++i)
    x(i) = i;
}


int main ()
{
  foo ();
}
```

So what to do in a case like this? The first step is to carefully read what
the error message actually says as it contains pretty much all the
information you need. So let's take the error message apart:

 - The first two lines tell you where the problem happened: in the current
   case, in line 1302 of file
   `/u/bangerth/p/deal.II/1/deal.II/include/deal.II/lac/vector.h` in
   function `Number& dealii::Vector<Number>::operator()(unsigned int) [with
   Number = double](0,10[)`. This is a function in the library, so you
   likely don't know what exactly it does and what to do with it, but there
   is more information to come.

 - The second part is the condition that should have been true but wasn't,
   leading to the error: `i<vec_size`. The variables involved in this
   condition (`i,vec_size`) are local variables of the function, or member
   variables of the class, so again you may not be entirely familiar with
   them. But you can already gather some of the information: `i` likely is
   an index, which should have been less than the variable `vec_size`
   (which sounds a lot like the length of a vector); the assertion says
   that it <i>should</i> have been smaller, but that it wasn't actually.

 - There is more information: The exception generated is of kind
   `ExcIndexRange(i,0,vec_size)` and the additional information says `Index
   10 is not in [In other words, the variable `i` has value 10, and
   `vec_size` is also ten. This should already give you a fairly good idea
   what is happening: the vector has size ten, and following C array
   convention, that means that only indices zero through nine are value,
   but ten is not.

 - The final part of the error message -- the stack trace -- tells you how
   you got to this place: reading from the bottom, `main()` called `foo()`
   which called the function that generated the error.

Taken together, this information should allow you figure out in 80% of
cases what was going on, and fix the problem. Here, it is that we used the
condition `i<=x.size()` in the loop, rather than the correct condition
`i<x.size()`. In the remaining 20% of cases, things might be more
difficult. For example, `foo()` might be a large and difficult function,
and you would need to know in which part of the function did we access an
invalid index of the vector. Or `i` was an index computed from other
variables and you'd need to find out why it got the invalid value. In these
cases, you'll have to learn how to use a debugger such as gdb, and in
particular how to move up and down in the call stack and to inspect local
variables in your source code.

### The program aborts saying that an exception was thrown, but I can't find out where

deal.II creates two kinds of exceptions (in deal.II language): ones where we
simply abort the program because you are doing something that can't be right
(such as accessing element 11 of a 10-element vector; this results in what has
been discussed in the previous question) and ones that use the C++ construct
`throw` to raise an exception. The latter construct is used for things that
can't be statically checked in debug mode because they may depend on values
read from input files or on a status that may simply change from one run of
the program to the next; consequently, they <i>always</i> need to be verified,
not only in debug mode, and there is sometimes a way to work around it in a
program. The typical case is trying to write to a file that can't be opened
(e.g. because the directory/file you specified in a parameter file doesn't
exist or because the file system has run out of disk space).

Most of the time, the exceptions deal.II throws are annotated with the
location and function where this exception was raised, and if you use a
`main()` function such as the one used starting in step-6, this information
will be printed. However, there are also cases where this kind of information
is not available and then it is often difficult to establish where exactly the
problem is coming from: all you know is that an exception was thrown, but not
where or why.

To debug such problems, two approaches have proven useful:

 - Run your program in a debugger (see the question about debuggers above,
   as well as these videos showing how to use the debugger in
   22c8e221823811aa1178b450171824af:
   http://www.math.tamu.edu/~bangerth/videos.676.8.html,
   http://www.math.tamu.edu/~bangerth/videos.676.25.html). You need to
   instruct the debugger to stop whenever an exception is thrown. If you
   work with gdb on the command line, then issue the command `catch throw`
   before starting the program and it will stop everytime the code executes
   a `throw` statement. Integrated development environments typically also
   have ways of switching this on. Note that not every exception that is
   thrown actually indicates an error -- sometimes, there are legitimate
   reasons to throw an exception and catch it in the calling function, so
   you may have to continue (resume) a number of times before finding the
   place where this happens.

 - Debugging by subtraction: Starting at the end of your program, remove
   one function/code block after the other until your program runs through
   without aborting. For example, if your program looked like step-6, see
   if it runs through if you don't create graphical output in `run()`. If
   it does, then you know that the exception must have been thrown in the
   block of code you just removed. If the program continues to abort, then
   reduce the number of mesh refinement cycles to find out withhin which
   cycle the problem happens. If it happens in the very first cycle, then
   remove calling the linear solver. If the program now runs through, then
   the problem happened in the solver. If it still aborts, then it must
   have happened before the solver, for example in the assembly. Repeating
   this, you will be able to narrow down which statement caused the
   problem, and knowing where a problem happens is already more than half
   of what you need to know to fix it.


### I get an exception in `virtual dealii::Subscriptor::~Subscriptor()` that makes no sense to me!

The full text of the error message probably looks something like this (the
stack trace at the bottom is of course different in your code):
```
An error occurred in line <103> of file </.../deal.II/source/base/subscriptor.cc> in function
    virtual dealii::Subscriptor::~Subscriptor()
The violated condition was:
    counter == 0
The name and call sequence of the exception was:
    ExcInUse (counter, object_info->name(), infostring)
Additional Information:
Object of class N6dealii15SparsityPatternE is still used by 5 other objects.
  from Subscriber SparseMatrix

Stacktrace:
-----------
#0  /.../deal.II/lib/libdeal_II.g.so.7.0.0: dealii::Subscriptor::~Subscriptor()
#1  /.../deal.II/lib/libdeal_II.g.so.7.0.0: dealii::SparsityPattern::~SparsityPattern()
#2  /.../deal.II/lib/libdeal_II.g.so.7.0.0: dealii::BlockSparsityPatternBase<dealii::SparsityPattern>::reinit(unsigned int, unsigned int)
#3  /.../deal.II/lib/libdeal_II.g.so.7.0.0: dealii::BlockSparsityPattern::reinit(unsigned int, unsigned int)
#4  /.../deal.II/lib/libdeal_II.g.so.7.0.0: dealii::BlockSparsityPattern::copy_from(dealii::BlockCompressedSimpleSparsityPattern const&)
#5  ./step-6: NavierStokesProjectionIB<2>::setup_system()
#6  ./step-6: NavierStokesProjectionIB<2>::run(bool, unsigned int)
#7  ./step-6: main
```

What is happening is this: deal.II derives a bunch of classes from the
`Subscriptor` base class and then uses the `SmartPointer` class to point to
such objects. `SmartPointer` is actually a fairly simple class: when given
a pointer, it increases a counter in the `Subscriptor` base of the object
pointed to by one, and when the pointer is reset to another object or goes
out of scope, it decreases the counter again. (It can also records
<i>who</i> points to this object.) If someone tries to delete the object
pointed to, then the destructor `dealii::Subscriptor::~Subscriptor()` is
run and checks that in fact the counter in this object is zero, i.e. that
nobody is pointing to the object any more -- because if some pointer was
still pointing to it, it would be a poor decision to delete the object as
then the pointer would point to invalid memory. If the counter is nonzero,
you get the error above: you are trying to delete an object that is still
pointed to. In the case above, you try to delete a `SparsityPattern` object
(that is, from the stack trace, a part of a block sparsity pattern) even
though there is still a `SparseMatrix` pointing to it (we get this from the
"Additional Information" field).

The solution in cases like these is to make sure that at the time you
delete the object, no other objects still have pointers that point to it.

There is one rather frequent case that results in an error like the above
and that is often difficult to understand: if an exception is thrown in
some function and not caught, all local objects are destroyed in the
opposite order of their declaration; if it isn't caught in the function
that called the place where the exception was generated, its local
variables are also destroyed, and so on. This automatic destruction of
objects typically bypasses all the clean-up code you may have at the end of
a function and can then lead to errors like the above. For example, take
this code:
```cpp
void f() {
  SparseMatrix s;
  SparsityPattern sp;
  // initialize sp somehow
  s.reinit (sp);
  Vector v;
  // build a linear system

  solve_linear_system (s, v);

  s.reinit ();
}
```

If the code executes normally, at the bottom of the function, the local
variables `s,sp,v` will be destroyed in reverse order. Since we have called
`s.reinit()`, the object no longer stores a pointer to `sp` and so
destruction of `sp` before `s` incurs no harm. But if the function
`solve_linear_system` throws an exception, for example because the linear
system is singular, the call to `s.reinit()` isn't executed any more, and
you will get an error like the one shown at the top.

In cases like these, the challenge becomes finding where the exception was
thrown. The easiest way is to run your program in a debugger and let the
debugger tell you whenever an exception is generated. In `gdb`, you can do
that by saying `catch throw` before running the program; essentially, the
command puts a breakpoint on all places where exceptions are thrown.
Remember, however, that not every place where an exception is thrown is a
candidate for the problem above: it may also be an exception that is caught
in the function above and that never propagates to a point where it
produces trouble. Consequently, it may well happen that you have to
continue several times after seeing an exception thrown until you finally
find the place where the offending exception happens.

### I get an error that the solver doesn't converge. But which solver?

Solvers are often deeply nested -- take a look for example at step-20 or
step-22, where there is an outer solver for a Schur complement matrix, but
both in the implementation of the Schur complement as well as in the
implementation of the preconditioner we solve other linear problems which
themselves may have to be preconditioned, etc. So if you get an exception
that the solver didn't converge, which one is it?

The way to find out is to not wait till the exception propagates all the
way to `main()` and display the error code there. Rather, you probably
don't have a Plan B anyway if a solver fails, so you may want to abort the
program if that happens. To do this, wrap the call to the solver in a
try-catch block like this:
```cpp
  try {
    cg.solve (system_matrix, solution, system_rhs, preconditioner);
  } catch (...) {
    std::cerr << "*** Failure in Schur complement solver! ***" << std::endl;
    std::abort ();
  }
```

Of course, if this is in the Schur preconditioner, you may want to use a
different error message. In any case, what this code does is catch the
exceptions thrown by the solver here, or by the system matrix's `vmult`
function (if not already caught there) or by the preconditioner (if not
already caught there). If you had already caught exceptions in the `vmult`
function and in the preconditioner, then you now know that any exception
you get at this location must have been because the CG solver failed, not
the preconditioner, etc. The upshot is that you need to wrap <i>every</i>
call to a solver with such a try-catch block.

### How do I know whether my finite element solution is correct? (Or: What is the "Method of Manufactured Solutions"?)

This is not always trivial, but there is an "industry-standard" way of
verifying that your code works as intended, called the '''method of
manufactured solutions'''. Before we describe the method, let us point this
out: '''A code that has not been verified (i.e. for which correctness has
not been established) is worthless. You do not want to have results in your
thesis or a publication that may later turn out to be incorrect because
your code does not converge to the correct solution!'''

The idea to verify a code is that you need a problem for which you know the
exact solution. Unless you solve the very simplest possible partial
differential equations, it is typically not possible to choose a right hand
side and boundary values and then find the corresponding solution to the
PDE analytically, on a piece of paper. But you can turn this around: Let's
say your equation is <i>Lu=f</i>, then choose some function <i>u</i> and
compute <i>f=Lu</i>. Note that the solution <i>u</i> does not necessarily
have to be something that looks like a useful or physically reasonable
solution to the equation, all that is necessary is that it is a function
you know. Because <i>L</i> is a differential operator, computing <i>f</i>
only involves computing the derivatives of the known function; this may
yield lengthy expressions if you have nonlinearities or spatially variable
coefficients in the equation, but should not be too complicated and can
also be done using computer algebra programs such as Maple or Mathematica.

If you now put this particular right hand side <i>f</i> into your program
(along with boundary values that correspond to the values of the function
<i>u</i> you have chosen) you will get a numerical solution
<i>u<sub>h</sub></i> that we would hope converges against the exact
solution <i>u</i> at a particular rate, say <i>O(h<sup>2</sup>)</i> in the
<i>L<sub>2</sub></i> norm. But since you know the exact solution (you have
chosen it before), you can compute the error between numerical solution and
exact solution, and verify not only that your code converges, but also that
it shows the convergence rate you expect.

The method of manufactured solutions is shown in the step-7 tutorial
program.

### My program doesn't produce the expected output!

There are of course many possible causes for this, and you need to find out
which of these causes might be the reason. Possible places to start are:

 - Are matrix and right hand side assembled correctly? For most reasonably
   simple problems, you can compute the local contributions to these
   matrices by hand, and then compare those with the ones you compute on
   every cell of your program (remember that you can print the contents of
   the local matrix and right hand side to screen). A good strategy is also
   to reduce your problem to a 1x1 or 2x2 mesh and then print out the
   entire system matrix for comparison.

 - Do you compute the matrix you need, or its transpose? The mathematical
   literature often multiplies the equation from the right with a test
   function but that is awkward because the matrix you get this way is the
   transpose from the one you need. The deal.II documentation goes to
   lengths in multiplying test functions from the left to avoid this sort
   of error; do the same in your derivations.

 - Your constraints or boundary values may be wrong. While the
   ConstraintMatrix and functions like
   VectorTools::interpolate_boundary_values are well enough tested that
   they are unlikely candidates for problems, you may have computed
   constraints wrongly if you collect them by hand (for example if you deal
   with periodic boundary conditions or similar) or you may have specified
   the wrong boundary indicator for a Dirichlet boundary condition. Again,
   the solution is to reduce the problem to the simplest one you can find
   (e.g. on the 1x1 or 2x2 mesh talked about above) and to ask the
   ConstraintMatrix to print its contents so that you can compare it by
   hand with your expectations.

 - Your discretization might be wrong. Some equations require you to use
   particular (combination of) finite elements; for example, for the Stokes
   equations and many other saddle point problems, you need to satisfy an
   LBB or Babuska-Brezzi condition. For other equations, you need to add
   stabilization terms to the bilinear form; for example, advection or
   transport dominated problems require stabilization terms such as
   artificial diffusion, streamlinear diffusion, or SUPG.

 - The solver might be wrong. This can reasonably easily happen if you have
   a complex solver such as, for example, the one used in step-22. In such
   cases it has proven useful to simply replace the entire solver by the
   sparse direct UMFPACK solver (see step-29). UMFPACK is not the fastest
   solver around, but it never fails: if the linear system has a solution,
   UMFPACK will find it. If the output of your program is essentially the
   same as before, then the solver wasn't your problem.

 - Your assumptions may be wrong. Double check that you had the correct
   right hand side to compute the numerical solution you compare against
   your analytical one. Also remember that the numerical solution is
   usually only an approximation of the true one.

In general, if your program is not computing the output you expect, here
are a few strategies that have often worked in finding the problem:
 - Take a good look at the output you get. For example, a close look can
   already tell you if (i) the boundary conditions are correct, (ii) the
   solution is continuous at hanging nodes, (iii) the solution follows the
   characteristics of the right hand side. This may already help you narrow
   down which part of the program may be the culprit. A common mistake is
   also to have a solution that by some accident is too large by a certain
   factor; consequently, the error will not converge to zero but to some
   constant value. This, again, is easily visible from a graphical
   representation of the solution and/or the error. Plotting the error is
   discussed in the section below entitled "How to plot the error as a
   pointwise function".
 - If you have a time dependent problem, is the first time step right?
   There is no point in running the program for 1000 time steps and trying
   to find our why it is wrong, if already the first time step is wrong.
 - If you still can't find what's going on, make the program as small as
   possible. Copy it to another directory and start stripping off parts
   that you don't need. For example, if it is a time dependent program for
   which you have previously already found out that the first time step is
   wrong, then remove the time loop. If you have tried whether you have the
   same problem when the mesh is uniformly refined, then throw out all the
   code that deals with adaptive refinement, constraints and hanging nodes.
   In this process, every time you simplify the program, verify that the
   problem is still there. If the problem disappears, you know that it must
   have been in the last simplification step. If the problem remains, it
   must be in the code that is now one step smaller. Ultimately, the code
   should be small enough so that you can just go through it and find the
   error by inspection.
 - Learn to use a debugger. You will find that using a debugger is so much
   more convenient than trying to put screen output statements into your
   code, recompiling, and hoping that they reveal the problem. Modern
   integrated development environment as the ones discussed elsewhere in
   this FAQ have the debugger built-in, allowing you to use it seamlessly
   in your editing environment.

### The solution converges initially, but the error doesn't go down below 10<sup>-8</sup>!

First: if the error converges to zero, then you are basically doing
something right already. Congratulations!

As for why the error does not converge any further, there are two typical
cases what could be the reason:

 - While the discretization error should converge to zero, the error of
   your numerical solution is composed of both the discretization error and
   the error of your linear or nonlinear solver. If, for example, you solve
   the linear system to an accuracy of 10<sup>-5</sup>, then there will be
   a point where the discretization error will get smaller than that by
   using finer and finer meshes but the solver error will not become
   smaller any more. To continue observing the correct convergence order,
   you will also have to solve the linear system with more accuracy.

 - If you compute the error through an external program, for example by
   writing out the solution to a file and reading it from another program
   that knows about the exact solution, then you need to make sure you
   write the solution with sufficient accuracy. The default setting of C++
   writes floating point numbers with approximately 8 digits, so if you
   want to make sure that your solution is correct to 10<sup>-10</sup>, for
   example, you'll have to write out the solution with more than 10 digits.

### My time dependent solver does not produce the correct answer!

For time dependent problems, there are a number of other things you can try
over the discussion already given in the previous answer. In particular:

 - If you have a time iteration and the solution at the final time (where
   you may evaluate the error) is wrong, then it was likely already wrong
   at the first time step. Try to run your program only for a single time
   step and make sure the solution there is correct. For example, it could
   be that you set the boundary values wrongly; this would be quite
   apparent if you looked at the first time step because the effect would
   be largest close to the boundary, but it may no longer be visible if you
   ran your program for a couple hundred time steps.

 - Are your initial values correct? Output the initial values using DataOut
   just like you output the solution and inspect it for correctness.

 - If you have a multi-stage time stepping scheme, are *all* the initial
   values correct?

 - Finally, you can test your scheme by setting the time step to zero. In
   that case, the solution at time step zero should of course be equal to
   the solution at time step zero. If it isn't, you already know better
   where to look.

### My Newton method for a nonlinear problem does not converge (or converges too slowly)!

Newton methods are tricky to get right. In particular, they sometimes
converge (if slowly) even though the implementation has a bug because all
that is required for convergence is that the search direction is a
direction of descent; consequently, if for example you have the wrong
matrix, you may compute something that is a direction of descent, but not
the full Newton direction, and so converges but not at quadratic order.

Here are a few considerations for implementing Newton's method for
nonlinear PDEs:

 - Try it with a linear program by removing all the nonlinearities in your
   problem. Your Newton iteration must converge in a single step, i.e. the
   Newton residual must be zero at the beginning of the second iteration.
   If that's not the case, something is wrong in your implementation.

 - Newton's iteration will converge with optimal order for the problem
   `F(u)=0` if you <i>consistently</i> compute the Newton residual
   `(F(u<sup>k</sup>), &phi;<sup>i</sup>)` and the Newton (Jacobian) matrix
   `(F'(u<sup>k</sup>) &phi;<sup>j</sup>, &phi;<sup>i</sup>)`. If you have
   a bug in either of the two, your method may converge, but typically at a
   (much) lower rate and with consequently many more iterations.
   Consequently, one way to debug Newton's methods is to verify that Newton
   matrix and Newton residual are matching in their code. However, if you
   have a matching bug in <i>both</i> of the matrix and right hand side
   assembly, then your Newton method will converge with correct order but
   against the wrong solution.

 - If you have nonzero boundary values for your problem, set the correct
   boundary values for the initial guess and use zero boundary values for
   all following updates. This way, the updated `u<sup>k+1</sup> =
   u<sup>k</sup> + &delta; u<sup>k+1</sup>` already has the right boundary
   values for all following iterations.

 - If your problem is strongly nonlinear, you may need to employ a line
   search where you compute `u<sup>k+1</sup> = u<sup>k</sup> + &alpha;
   &delta; u<sup>k+1</sup>` and successively try &alpha;=1, &alpha;=1/2,
   &alpha;=1/4, etc until the residual computed for `u<sup>k+1</sup>` for
   this &alpha; is smaller than the residual for `u<sup>k</sup>`.

 - A rule of thumb is that if your problem is strongly nonlinear, you may
   need 5 or 10 iterations with a step length &alpha; less than one, and
   all following steps use the full step length &alpha;=1.

 - For most reasonably behaved problems, once your iteration reaches the
   point where it takes full steps, it usually converges in 5 or 10 more
   iterations to very high accuracy. If you need significantly more than 10
   iterations, something is likely wrong.

### Printing deal.II data types in debuggers is barely readable!

Indeed. For example, plain gdb prints this for cell iterators:
```
$2 = {<dealii::TriaIterator<dealii::DoFCellAccessor<dealii::DoFHandler<2, 3> > >> = {<dealii::TriaRawIterator<dealii::DoFCellAccessor<dealii::DoFHandler<2, 3> > >> = {<std::iterator<std::bidirectional_iterator_tag, dealii::DoFCellAccessor<dealii::DoFHandler<2, 3> >, long, dealii::DoFCellAccessor<dealii::DoFHandler<2, 3> >*, dealii::DoFCellAccessor<dealii::DoFHandler<2, 3> >&>> = {<No data fields>},
      accessor = {<dealii::DoFAccessor<2, dealii::DoFHandler<2, 3> >> = {<dealii::CellAccessor<2, 3>> = {<dealii::TriaAccessor<2, 2, 3>> = {<dealii::TriaAccessorBase<2, 2, 3>> = {
                static space_dimension = <optimized out>, static dimension = <optimized out>,
                static structure_dimension = <optimized out>, present_level = -9856,
                present_index = 32767, tria = 0x4a1556}, <No data fields>}, <No data fields>},
          static dimension = 2, static space_dimension = 3, dof_handler = 0x7fffffffdac8},
        static dim = <optimized out>,
        static spacedim = <optimized out>}}, <No data fields>}, <No data fields>}
```

Fortunately, this can be simplified to this:
```cpp
$3 = {
  triangulation = 0x4a1556,
  dof_handler = 0x7fffffffdac8,
  level = 2,
  index = 52
}
```

All you need is (i) gdb version 7.1 or later, or a graphical frontend for
it (e.g. [http://www.gnu.org/software/ddd/ DDD] or
[kdevelop](http://www.kdevelop.org)), (ii) some code that goes into your
$HOME/.gdbinit file. Instructions for setting up this file, which implements
pretty printers for `Point`, `Tensor`, `Vector`, and the various iterator
classes for triangulations and DoFHandlers, is posted
[here](Debugging-with-GDB).

gdb can also pretty print many of the `std::XXX` classes, but not all linux
distributions have it configured this way. To enable this, follow the
instructions [from this
website](http://sourceware.org/gdb/wiki/STLSupport). The little python
snippet can be placed as a separate python block into `.gdbinit`.

### My program is slow!

This is a problem that is true for a lot of us. The question is which part
of your program is causing it. Before going into more detail, there are,
however, some general observations:

 - Running deal.II programs in debug mode will take, depending on the
   program, between 4 and 10 times as long as in optimized mode. If you are
   using the deal.II Makefiles, you can switch between debug and optimized
   mode somewhere at the top of the Makefile.

 - A typical finite element program will spend around one third of its time
   in assembling linear systems, around one half in solving these linear
   systems, and the rest of the time on other things. If your program's
   percentages significant deviate from this rule of thumb, you know where
   to start.

 - There is a rule that says that even the best programmers are unable to
   point out where in the program the most CPU time is spent without some
   form of profiling. This is definitely true also for the primary
   developers of deal.II, so it is likely true for you as well. A corollary
   to this rule is that if you start optimizing parts of your code without
   first profiling it, you are more than likely just going to make things
   more complicated without significant gains because you pick the simplest
   places to optimize, not the ones with the biggest impact.

So how can you find out which parts of the program are slow? There are two
tools that we've really come to like, both from the
[valgrind](http://www.valgrind.org/) project: callgrind and cachegrind.
Valgrind essentially emulates what your CPU would do with your program and
in the process collects all sorts of information. In particular, if you run
your program as in
```
  valgrind --tool=callgrind ./myprogram
```

(this will take around 10 times longer than when you just call
`./myprogram` because of the emulation) then the result will be a file in
this directory that contains information about where your program spent its
time. There are a number of graphical frontends that can visualize this
data; my favorite is `kcachegrind` (a misnomer -- it is, despite its name,
actually a frontend from callgrind, not cachegrind). Pictures of how this
output looks can be found in the introduction of step-22. It typically
shows how much time was spent in each function and a call graph of which
functions where called from where.

Using valgrind's cachegrind can give you a more detailed look at much of
the same kind of information. In particular, it can show you source line
for source line how many instructions were executed there, and how many
memory accesses (as well as cache hits and misses) were generated there.
See the valgrind manual for more information.

Lastly, since you are probably most interested in the performance of the
optimized version of your code (which you will probably use for long
expensive runs), you should run valgrind on the optimized executable.

### How do I debug MPI programs?

This is clearly an awkward topic for which there are few good options:
debugging parallel programs using MPI has always been a pain and it is
frustrating even to experienced programmers. That said, there are parallel
debuggers that can deal with MPI, for example
[TotalView](http://www.roguewave.com/products/totalview-family/totalview.aspx)
that can make this process at least somewhat simpler.

Whether you have or don't have TotalView, here are a few guidelines of
strategies that have helped us in the past:

 - Try to reduce the problem to the smallest one you can find: The smallest
   mesh, the smallest number of processors. Reducing the number of
   processors needed to demonstrate the bug must be your highest priority.

 - One of the biggest problems you typically have is that the processes
   that communicate via MPI typically run on different machines. If you can
   manage to reduce the problem to a small enough number of processors, you
   can run them all locally on a single workstation, rather than a cluster
   of computers. Ideally, you would reduce the problem to 2 or 4 processors
   and then just start the program using `mpirun -np 4 ./myexecutable` on
   the headnode of the cluster, a workstation, or even a laptop.

 - Try to figure out which MPI process (the MPI rank) has the problem, for
   example by printing the output of
   `Utilities::System::get_this_mpi_process(MPI_COMM_WORLD)` at various
   points in your program.

 - If you know which MPI process has the problem and if this is
   reproducible, let each process print out its MPI rank and its process id
   (PID) using the system function `getpid` at the very beginning of the
   program. The PID is going to be different every time you run the
   program, but if you know the connection between MPI rank and PID and you
   know which rank will produce the problem, then you can predict which PID
   will have the problem. The point of this is that you can attach a
   debugger to this PID; for example, `gdb` has the command `attach <pid>`
   with which you can attach the debugger to a running program, rather than
   running the program from the start within the debugger. Attaching a
   program to the debugger will stop it (and, after a while, will typically
   also stop all the other MPI processes once they come to a place where
   they are waiting for a communication from the stopped process). You can
   then look at variables, continue running to breakpoints, or do whatever
   else you want to do with the process you attached the debugger to. In
   particular, if for example you attached the debugger to the process that
   you know will segfault or run onto a failing assertion, you can just
   type `continue` in the debugger to let the program continue till it
   aborts. You can then inspect the state of the program at the point of
   the problem inside the debugger you attached.

 - The above process relies on the fact that you have time to attach a
   debugger between starting the program, reading the mapping from MPI
   process rank to PID, and attaching a debugger. If the program produces
   the error very quickly, it is often useful to insert a call to
   `sleep(60);` (and including the appropriate header file) just after
   outputting MPI rank and PID. This gives you 60 seconds to attach the
   debugger before the program will continue.

 - If finding out which MPI process has the problem turns out to be too
   complicated, or if it isn't predictable which process will produce an
   error, then there is a fallback option: attach a debugger to
   <i>every</i> MPI process. This is awkward to do by hand, but there is a
   shortcut: at least under linux (or any other unix system) you can run
   the program as in
```
  mpirun -np 4 xterm -e gdb ./my_executable
```

In this example, we start 4 MPI processes; in each of these 4 processes, we
open an `xterm` window in which we start an instance of `gdb` with the
executable. You'd then `run` the executable in each of the 4 windows, and
debug it as you usually would. This might be tedious but as mentioned
above, debugging MPI programs often is tedious indeed. To find out which
gdb window belongs to which MPI rank, you can type the command
```
  !export | grep RANK
```
into the gdb window (this works with openmpi at least).

### I have an MPI program that hangs

Apart from programs that segfault or that run onto a failing assertion
(both cases that are relatively easy to debug using the techniques above),
programs that just hang are the most common problem in parallel
programming. The typical cause for this is that there is a point in your
program where all or some MPI processes expect to get a message from a
process X (e.g. in a global communication, say MPI_Reduce, MPI_Barrier, or
directly in point-to-point communications) but process X is not where it
should be -- for example, because it is in an endless loop, or -- more
likely -- because process X didn't think that it should participate in this
communication. In either case, the other processes will wait forever for
process X's message and deadlock the program. An example for this case
would go like this:
```cpp
  void assemble_system () {
              // optimization in case there is nothing to do; we won't
              // have to initialize FEValues and other local objects in
              // that case
    if (tria.n_locally_owned_active_cells() == 0)
      return;

    ...
    for (cell = ...)
      if (!cell->is_ghost() && !cell->is_artificial())
        ...do the assembly on the locally owned cells...

    system_rhs.compress();
  }
```

Here, the call to `compress()` at the end involves communication between
MPI processes. In particular, say, it implies that process Y will wait for
some data from process X. Now what happens if process X realizes that it
doesn't have any locally owned cells? In that case, process X will quit the
function at the very top, and will never call `compress()`. In other words,
process Y will wait forever, possibly making process Z wait further down
the program etc. In the end, the program will be deadlocked.

The goal of debugging the program must be to find where individual
processes are stopped in order to determine which incoming communication
they are waiting for. If you attached a debugger to the program above,
you'd find for example that all but one process is stopped in the call to
`compress()`, and the one remaining process is stopped in some other MPI
call, then you already have a good idea what may be going on.

### One statement/block/function in my MPI program takes a long time

Let's say you have a block of code that you suspect takes a long time and
you want to time it like this:
```cpp
  Timer t;
  t.start();
  my_function();
  t.stop();
  if (my MPI rank == 0)
    std::cout << "Calling my_function() took " << timer() << " seconds." << std::endl;
```

The output is large, i.e. you think that the function you called is taking
a long time to execute and that you should focus your efforts on optimizing
it. But in an MPI program, this isn't quite always true. Imagine, for
example, that the function looked like this:
```cpp
  void my_function () {
    double val = compute_something_locally();
    double global_sum = 0;
    MPI_Reduce (&val, &global_sum, MPI_DOUBLE, 1, 0, MPI_COMM_WORLD);
    if (my MPI rank == 0)
      std::cout << "Global sum = " << global_sum << std::endl;
  }
```

In the call to `MPI_Reduce`, all processors have to send something to
processor zero. Processor zero will have to wait till everyone sends stuff
to this processor. But what if processor X is still busy doing something
else (stuff above the call to `my_function`) for a while? The processor
zero will wait for quite a while, not because the operations in
`my_function` are particularly expensive (either on processor zero or
processor X) but because processor X was still busy doing something else.
In other words: you need to direct your efforts in making the "something
else on processor X" faster, not making `my_function` faster.

To find out whether this is really the problem, here is a simple way to see
what the "real" cost of `my_function` is:
```cpp
  Timer t;
  MPI_Barrier (MPI_COMM_WORLD);
  t.start();
  my_function();
  MPI_Barrier (MPI_COMM_WORLD);
  t.stop();
  if (my MPI rank == 0)
    std::cout << "Calling my_function() took " << timer() << " seconds." << std::endl;
```

This way, you really only measure the time spent between when all
processors have finished doing what they were doing before, and when they
are all finished doing what they needed to do for `my_function`.

Another way to find some answers is to use the capabilities of the `Timer`
class which can provide more detailed information when deal.II is
configured to support MPI.

## I have a special kind of equation!

### Where do I start?

The deal.II tutorial has a number of programs that deal with particular
kinds of equations, such as vector-valued problems, mixed discretizations,
nonlinear and time-dependent problems, etc. The best way to start is to
take a look at the existing tutorial programs and see if there is one that
is already close to what you want to do. Then take that, try to understand
its structure, and find a way to modify it to solve your problem as well.
Most applications written based on deal.II are not written entirely from
scratch, but have started out as modified tutorial programs.

### Can I solve my particular problem?

The simple answer is: if it can be written as a PDE, then this is possible
as evidenced by the many publications in widely disparate fields obtained
with the help of deal.II. The more complicated answer is: deal.II is not a
problem-solving environment, it is a toolbox that supports you in solving a
PDE by the method of finite elements. You will have to implement assembling
matrices and right hand side vectors yourself, as well as nonlinear outer
iterations, etc. However, you will not need to care about programming a
triangulation class that can handle locally refined grids in one, two, and
three dimensions, linear algebra classes, linear solvers, different finite
element classes, etc.

To give only a very brief overview of what is possible, here is a list of
the nontrivial problems that were treated by the programs that the main
authors alone wrote to date:

 - Time-dependent acoustic and elastic wave equation, including nonlocal
   absorbing boundary conditions;
 - Stokes flow discretized with the discontinuous Galerkin finite element
   method;
 - General hyperbolic problems including Euler flow, using the
   discontinuous Galerkin finite element method;
 - Distributed parameter estimation problems;
 - Mixed finite element discretization of a mortar multiblock formulation
   of the Laplace equation;
 - Large-deformation elasto-plasticity in the simulation of plate
   tectonics.

To illustrate the complexity of the programs mentioned above we note that
most of them include adaptive mesh refinement tailored to the efficient
computation of specific quantities of physical interest and error
estimation measured in terms of these quantities. This includes the
solution of a so-called dual problems, that means e.g. for the wave
equation the solution of a wave equation solved backward in time.

Problems other users of deal.II have solved include:
 - Elastoplasticity;
 - Porous media flow;
 - Crystal growth simulations;
 - Fuel cell simulations and optimization;
 - Fluid-structure interaction problems;
 - Time dependent large deformation problems for metal forming;
 - Contact problems;
 - Viscoelastic deformation of continental plates;
 - Glacial ice flows;
 - Thermoelastoplastic metal forming;
 - Eulerian coordinates problems in biomechanical modeling.

Some images from these applications can be found on the deal.II Wiki's
[page. A good overview of the sort of problems that are being solved with
the help of deal.II can also be obtained by looking at the large number of
[http://www.dealii.org/developer/publications/toc.html publications written with the help of deal.II]([[Gallery]]]).

Probably, many other problem types are solved by the many users which we do
not know of directly. If someone would like to have his project added to
this page, just contact us.


### Why use deal.II instead of writing my application from scratch?

You can usually get the initial version of a code for any given problem
done relatively quickly when you write it yourself, since the learning
curve is not as steep as if you had to learn a new library; it's also true
that it's easy to make this code twice as fast as if you had to use a
library. In other words, this sounds like you should write finite element
codes for your problem yourself.

However, you also need to keep in mind that it is the things you want to do
after the first 3 months that will take you forever if you want to write it
yourself, and where you will never be able to catch up with existing,
established libraries: higher order elements; complicated, unstructured 3d
meshes; parallelization; producing output in a format that's easily
visualizable in 3d; adding an advected field for a tracer quantity; etc.
Viewed this way, it's worth remembering that the primary commodity that's
in short supply is not CPU time but your own programming time, and that's
where you will be '''orders magnitude faster''' when using what others have
already done, even if maybe your program ends up twice as slow as if you
had written it from scratch with a particular application in mind.

### Can I solve problems over complex numbers?

Yes, you can, and it has been done numerous times with deal.II. However, we
have a standard recommendation: consider such problems as systems of
partial differential equations, where the individual components of the
solution are the real and imaginary part of your unknown. The reason for
this is that for complex-valued problems, the product `<u,v>` of two vectors
is not the same as `<v,u>`, and it is very easy to get this wrong in many
places. If you want to avoid these common traps, then the easiest way
around is to split up you equation into two equations of real and imaginary
part first, and then treat the resulting system as a system of real
variables. This also makes the type of linear system clearer that you get
after discretization, and tells you something about which solver may be
adequate for it.

The step-29 tutorial program shows how this is done for a complex-valued
Helmholtz problem.

### How can I solve a problem with a system of PDEs instead of a single equation?

The easiest way to do this is setting up a system finite element after you
chose your base element, e.g.,
```cpp
FE_Q<dim> base_element(2);
FESystem<dim> system_element(base_element, 3);
```

will produce a biquadratic element for a system of 3 equations. With this
finite element, all the functions that you always called for a scalar
finite element should just work for this vector-valued element as well.

Refer to the step-8 and in particular to the step-20 tutorial programs for
a lot more information on this topic. Several of the other tutorial
programs beyond step-20 also use vector-valued elements and there is a
whole module in the documentation on vector-valued problems that is worth
reading.

### Is it possible to use different models/equations on different parts of the domain?

Yes. The step-46 tutorial program shows how to do this: It solves a problem
in which we solve Stokes flow in one part of the domain, and elasticity in
the rest of the domain, and couple them on the interface. Similar
techniques can be used if you want to exclude part of the domain from
consideration, for example when considering voids in a body in which the
governing equations do not make sense because there is no medium.

### Where do I start to implement a new Finite Element Class?

If you really need an element that isn't already implemented in deal.II,
then you'll have to understand the interplay between FEValues, the finite
element, the mapping, and quadrature objects. A good place to start would
be to read the deal.II paper (Bangerth, Hartmann, Kanschat, ACM Trans.
Math. Softw., 2007).

The actual implementation would most conveniently start from the `FE_Poly`
class. You first implement the necessary polynomial space in the base
library, then you derive `FE_Your_FE_Name` from `FE_Poly` (using your new
polynomial class as a template) and add the connectivity information.

You'll probably need more specific help at various points -- this is what
the mailing list is there for!

## General finite element questions

### How do I compute the error

If your goal is to compute the error in the form `||`u-u<sub>h</sub>`||` in
some kind of norm, then you should use the function
{{{VectorTools::integrate_difference}}} which can compute the norm above in
any number of norms (such as the L2, H1, etc., norms). Take a look at
step-7.

On the other hand, if your goal is to *estimate* the error, then the one
class that can do this is {{{KellyErrorEstimator}}}. This class is used in
most of the tutorial programs that use adaptively refined meshes, starting
with step-6.

### How to plot the error as a pointwise function

The functions mentioned in the previous question compute the error as a
cellwise value. As a consequence, the values computed also include a factor
that results from the size of the cell. If you're interested in the pointwise
error as something that can be visualized, for example because you want to
find a pattern in why the solution is not as you expect it to be, what you
should do is this:
 - Interpolate the exact solution
 - Subtract the interpolated exact solution from the computed solution
 - Put the resulting vector into a {{{DataOut}}} object. This will plot the
   nodal values of the errors u-u<sub>h</sub> on the current mesh.

As an example, the following code shows how to do this in principle:
```cpp
  template <int dim>
  class ExactSolution : public Function<dim>
  {
  public:
    ExactSolution () : Function<dim>(dim+1) {}

    virtual double value (const Point<dim>   &p,
                          const unsigned int  component) const
    {
      return ...exact solution as a function of p...
    }
  };


  template <int dim>
  void MyProblem<dim>::plot_error () const
  {
    Vector<double> interpolated_exact_solution (dof_handler.n_dofs());
    VectorTools::interpolate (dof_handler,
                              ExactSolution<dim>(),
                              interpolated_exact_solution);
    interpolated_exact_solution -= solution;

    DataOut<dim> data_out;

    data_out.attach_dof_handler (dof_handler);
    data_out.add_data_vector (solution, "solution");
    data_out.add_data_vector (interpolated_exact_solution, "pointwise_error");
  }
```


### I'm trying to plot the right hand side vector but it doesn't seem to make sense!

In particular, what you probably see is that the plot shows values that are
smaller by a factor of two along the boundary than in the inside, and by a
factor of four in the corners (in 2d) or eight (in 3d). Similarly, on
adaptively refined cells, the values appear to scale with the cell size.
The reason is that trying to plot a right hand side vector doesn't make
sense.

While you plot the vector as if it is function (by connecting dots with
straight lines in 1d, or plotting surfaces in 2d), the thing you right hand
side vector is in fact an element of the dual space. To wit:
 - A vector in primal space is a vector of nodal values so that `sum_i  U_i
   phi_i(x)` is a reasonable function of `x`. Solution vectors are examples
   of elements of primal space.
 - A vector in dual space is a vector W formed from the integration of an
   object in primal space against the shape functions, e.g. `W_i  = int
   f(x) phi_i(x)`. Examples of dual vectors are right hand side vectors.

For vectors in dual space, it doesn't make sense to plot them as functions
of the form
     `sum_i W_i phi_i(x)`.
The reason is that the values of the coefficients W_i are not of
**amplitude** kind. Rather, the W_i are of kind amplitude (e.g. f(x)) times
integration volume (the integral `*` dx over the support of shape functions
phi_i). In other words, the sizes of cells comes into play for W_i, as does
whether a shape function lies in the interior or at the boundary. In your
case, the area of the integral when you integrate against shape functions
at the boundary happens to be half the size of the integration area for
shape functions in the interior.


### What does XXX mean?

The documentation of deal.II uses many finite element specific terms that
may not always be entirely clear to someone not familiar with this
language. In addition, we have certainly also invented our shares of
deal.II specific terminology. If you encounter something you are not
familiar with, take a look at the [deal.II glossary
page](http://www.dealii.org/developer/doxygen/deal.II/DEALGlossary.html)
that explains many of them.

## I want to contribute to the development of deal.II!

deal.II is Open Source -- this not only implies that you as everyone else has
access to the source codes, it also implies a certain development model:
whoever would like to contribute to the further development is invited to do
so: If you have changes or ideas, please send them to the
[deal.II mailing list](http://www.dealii.org/mail.html)!

This model follows a small number of simple rules. The first and basic one
is that if you have something that might be of interest to others as well,
you are invited to send it to the list for possible inclusion into the
library and use by others as well. Such additions useful to others are, for
example:
 - new backends for output in a new graphical format;
 - input filters for some kind of data;
 - tool classes that do something that might be interesting to use in other
   programs as well.

A few simple projects can also be found in the
[list of open issues](https://code.google.com/p/dealii/issues/list), where they
are generally marked as "Enhancements".

If you consider providing some code for inclusing into the library, these
are the simple rules of gaining reputation in the Open Source community:
 - your reputation grows with the number and complexity of your
   contributions;
 - your reputation with the maintainers of the library also grows with the
   degree of conformance of your proposed additions with the administrative
   rules stated below;
 - originators of code are credited full authorship.

In order to allow that a library remains a consistent piece of software,
there are a number of administrative rules:
 - there are a number of maintainers that decide what goes into the
   library;
 - maintainers are benevolent, i.e. in general they want your addition to
   become part of the library;
 - however, they have to evaluate additions with respect to some criteria,
   among which are value for others;
 - whether it fits into the general framework (meaning that if your
   contribution requires the installation of some obscure other library
   that people do not usually have, then that must be discussed;
   alternatively, a way must be provided to disable your contribution on
   machines that do not have this lib);
 - completeness and amount of documentation;
 - existence and completeness of error checking through assertions.

However, again: the basic rule is that if you think your addition is
interesting to others, there most probably is a way to get it into the
library!


## I found a typo or a bug and fixed it on my machine. How do I get it included in deal.II?

First: thank you for wanting to do this! This software project is kept alive
by people like you contributing to it. We like to include any improvement,
even if it is just a single typo that you fixed.

If you have only a small change, or if this is your first time submitting
changes, the easiest way to get them to us is by just emailing the
[deal.II mailing lists](http://dealii.org/mail.html) and we will make sure they
get incorporated. If you continue submitting patches (which we hope you will!)
and become more experienced, we will start to ask you to use
[git](http://en.wikipedia.org/wiki/Git_%28software%29) as the version control
system and base your patches off of the
[deal.II github repository](https://github.com/dealii/dealii).

The process for this is essentially the following (if you don't quite
understand the terminology below, take a look at the manuals at the
[github web site](https://github.com/), read
[this online tutorial](https://www.atlassian.com/git/tutorial), or ask on the
mailing list):
  - Create a github account
  - Fork the deal.II github repository, using the button at the top right
    of https://github.com/dealii/dealii
  - Clone the repository onto your local file system
  - Create a branch for your changes
  - Make your changes
  - Push your changes to your github repository
  - Create a pull request for your changes by going to your github
    account's deal.II tab where, after the previous step, there should be a
    button that allows you to create a pull request.

This list may sound intimidating at first, but in reality it's a fairly
straightforward process that takes no more than 2 minutes after the first
couple of times. But, as said, we'll be happy to hold your hand the first few
times around and help you with the process!

If you've submitted patches several times and know your way around git by now,
please also consider to
  - make sure you base your patch off the most recent revision of the
    repository
  - you rewrite the history of your patch so that it contains a relatively
    small number of commits that are each internally consistent and could
    also be applied independently (see, for example, [the discussion
    towards the bottom of this
    page](https://github.com/dealii/dealii/pull/87)).



## I'm fluent in deal.II, are there jobs for me?

Certainly. People with numerical skills are a sought commodity, both in
academia and in businesses. In the US, the National Labs are also hiring
lots of people in this field.
