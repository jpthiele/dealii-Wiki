Welcome to the deal.II wiki! This wiki collects explanations and advice for many topics, in particular installation support, guidelines for contributions, FAQs, etc.

At several places, the wiki refers to pages on https://www.dealii.org/.

In case you have questions, contact  the [deal.II mailing list](https://www.dealii.org/mail.html).

## Overview of main topics spanned by the deal.II wiki:

### Installation:

* [Download](https://www.dealii.org/download.html)
* [README and installation instructions](https://www.dealii.org/developer/readme.html)
* [Getting deal.II](https://github.com/dealii/dealii/wiki/Getting-deal.II)
* Apple/MacOS: see wiki pages [MacOSX](https://github.com/dealii/dealii/wiki/MacOSX) and [Apple ARM M1 OSX](https://github.com/dealii/dealii/wiki/Apple-ARM-M1-OSX)
* Homebrew/Linuxbrew on MacOSX and Linux: see [deal.II on Homebrew Linuxbrew](https://github.com/dealii/dealii/wiki/deal.II-on-Homebrew---Linuxbrew)
* Windows: see wiki page [Windows](https://github.com/dealii/dealii/wiki/Windows#using-dealii-on-native-windows)
* Spack: see [deal.II in Spack](https://github.com/dealii/dealii/wiki/deal.II-in-Spack)
* Docker: see [Docker Images](https://github.com/dealii/dealii/wiki/Docker-Images)
* [Power architecture](https://github.com/dealii/dealii/wiki/Power-architecture)

### Getting used to deal.II

* [deal.II manual](https://www.dealii.org/developer/doxygen/deal.II/index.html)
* [deal.II tutorials](https://www.dealii.org/developer/doxygen/deal.II/Tutorial.html)

### FAQs:

* [Frequently Asked Questions](https://github.com/dealii/dealii/wiki/Frequently-Asked-Questions)

### Developing/contributing:

* [Contributing](https://github.com/dealii/dealii/wiki/Contributing)
* [Coding conventions](https://www.dealii.org/developer/doxygen/deal.II/CodingConventions.html)
* [Indentation](https://github.com/dealii/dealii/wiki/Indentation)
* [Commit Authorship](https://github.com/dealii/dealii/wiki/Commit-authorship)
* [Debugging with GDB](https://github.com/dealii/dealii/wiki/Debugging-with-GDB)
* [Testing infrastructure (Continuous Integration)](https://github.com/dealii/dealii/wiki/Testing-Infrastructure)
* [The deal.II Testsuite (ctest)](https://www.dealii.org/developer/developers/testsuite.html)
* Working with Integrated Development Environments: [Eclipse](https://github.com/dealii/dealii/wiki/Eclipse), [KDevelop](https://github.com/dealii/dealii/wiki/KDevelop), [QtCreator](https://github.com/dealii/dealii/wiki/QtCreator), and for the nostalgics: [Emacs](https://github.com/dealii/dealii/wiki/Emacs)

### Publishing and literature:

* [Code DOI best practices](https://github.com/dealii/dealii/wiki/Code-DOI-best-practices)
* [Publications on and using deal.II](https://www.dealii.org/publications.html)
* [Literature](https://github.com/dealii/dealii/wiki/Literature)

### Gallery:

* Picture/video [Gallery](https://github.com/dealii/dealii/wiki/Gallery)
* [Code gallery](https://dealii.org/developer/doxygen/deal.II/CodeGallery.html)

### Scripts and Tips & Tricks:

[Cubit](https://github.com/dealii/dealii/wiki/Mesh-Input-And-Output), [High-order visualization with Paraview](https://github.com/dealii/dealii/wiki/Mesh-Input-And-Output), [Matlab](https://github.com/dealii/dealii/wiki/Mesh-Input-And-Output), [Intel Trace Analyzer and Collector](https://github.com/dealii/dealii/wiki/Mesh-Input-And-Output), [Function Plotting Tool](https://github.com/dealii/dealii/wiki/Function-Plotting-Tool)

### Miscellaneous:

[DoF Handler](https://github.com/dealii/dealii/wiki/DoF-Handler), [Electromagnetic problem](https://github.com/dealii/dealii/wiki/Electromagnetic-problem), [Tensor product structures for polynomials and quadrature](https://github.com/dealii/dealii/wiki/Tensor-product-structures-for-polynomials-and-quadrature), [White papers](https://github.com/dealii/dealii/wiki/White-papers)

## A word of caution for contributors: 

due to limitations with Github's wiki implementation, the table of contents for each individual page is manually generated. In particular, the shell script at

https://github.com/ekalinin/github-markdown-toc

(with a little postprocessing) generated the FAQ table of contents. If you add new entries, then please also update the table of contents accordingly.

Step by step on how to update the TOC:

```
git clone https://github.com/tjhei/github-markdown-toc
cd github-markdown-toc
./gh-md-toc https://github.com/dealii/dealii/wiki/Frequently-Asked-Question
```

and replace the TOC with the output.
