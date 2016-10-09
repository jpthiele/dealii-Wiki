**Disclaimer:** This page is written by a FEM novice, I hope it will finish up with a set of working examples for a code gallery. However, it can contain some obvious knowledge or simply wrong statements. Feel free to improve the page or e-mail me directly to k.ladutenko@metalab.ifmo.ru

# Rationale

Information about solving Maxwell equation using FEM (and Deal.II) is dispersed, this is an attempt to provide the whole picture in a single place.

# Frequency domain
Time harmonic Maxwell equation turns to be a Helmholtz equation.

### Available codes

* [step-7](http://dealii.org/developer/doxygen/deal.II/step_7.html) Non-homogeneous Neumann boundary conditions for the Helmholtz equation. Check the convergence with Method of Manufactured Solutions.
* [step-29](http://dealii.org/developer/doxygen/deal.II/step_29.html) Split complex valued functions into their real and imaginary parts. Uses absorbing boundary, however, the angle, when it stops working properly is not very big. See the reflection from "non-reflecting" boundary for the source located close to it.

 ![step 29 mod](https://docs.google.com/uc?authuser=0&id=0B7jg2ikAVgGLRXRRREZSX2F1MTg&export=download)
* Ross Kynch done some work in this direction. There are a [testBC.cc (google group link)](https://groups.google.com/d/msg/dealii/ZJqmZgObysw/RfyFkbY0D9AJ) and [dealii/tests/fe/nedelec_non_rect_2d.cc](https://github.com/dealii/dealii/blob/master/tests/fe/nedelec_non_rect_2d.cc) solutions using Nedelec elements. 
An improvement (WIP) [PR #2240](https://github.com/dealii/dealii/pull/2240) "New Nedelec finite element: FE_NedelecSZ". [There is a topic](https://groups.google.com/d/msg/dealii/1g3YSUdPSGY/0oW3upegbqMJ) on the problem with face orientation of Nedelec elements (BTW, is it solved now in the current master of deal.ii?). He had also [published](https://github.com/rosskynch/MIT_Forward) a Deal.II solver for Eddy current. [One more topic](https://groups.google.com/d/msg/dealii/odXjp7U3y0s/qXjYyWAefhIJ) on assembly and preconditioning of Nedelec elements.
* [There is a topic](https://groups.google.com/d/msg/dealii/8SbZ04qLwdQ/UReeEYmUFsAJ) by Ce Qin with his source to solve time-harmonic Maxwell equation with complex coefficients using Nedelec elements on a rectangular mesh.
* Related topic to [Computing the curl of a solution vector field obtained from Nedelec elements](https://groups.google.com/d/msg/dealii/iWrNRAH8b6o/GHmCs2oLmtUJ) by David Fernández 
* [Topic by](https://groups.google.com/d/msg/dealii/xpW2-h326Bs/5Nhj9TzHKlgJ) Simon Schernthanner with some useful references.
* There is a  detailed report on [Nedelec elements](http://www.dealii.org/reports/nedelec/nedelec.pdf) (by Anna Schneebeli, University of Basel, Switzerland).   

### Possible extensions

* Use Raviart–Thomas elements to resolve high k cases. See https://arxiv.org/pdf/1111.0671.pdf for details. This paper uses the same boundary condition as step-29, so it should suffer from the same problems within the near-field region and for small incident angles.
* Use Nedelec elements with PML (perfectly matched layer) condition, see section 5 of the paper "Electromagnetic Scattering by Unbounded Rough Surfaces" by P. Li et al. [pdf](https://www.math.purdue.edu/~lipeijun/paper/2011/Li_Wu_Zheng_SIMA_2011.pdf). Conformal PML (CPML aka CIFS-PML) has proved to be close to ideal for near-field in FDTD method can deal this problem, for FEM application see the paper "Locally-Conformal Perfectly Matched Layer Implementation for Finite Element Mesh Truncation" by O. Ozgun et al., the follow-up by the same authors [pdf](http://journals.tubitak.gov.tr/elektrik/issues/elk-08-16-1/elk-16-1-6-0802-3.pdf) As soon as the reflection from the PML layer usually (at least for FDTD) comes from discretization of polynomial by a step function high-order (3 should be enough) elements in PML should be a game changer.

### Applied problems 

These problems are well known and can be used as starting point for many other problems. 
          
* Mie scattering on the sphere has a well known analytic solution. It is still of great interest our days with hundreds of publications every year. It is an open boundary problem, so it needs PML (see Ozgun papers above). A typical problem set can include a dielectric particle, plasmonic nanoparticle, core-shell case. Due to the curved boundary of the particle non-linear mapping of finite elements to the mesh is beneficial here to have a good accuracy on a coarse mesh.
* Cavity problem. A nice example can be to find several whispering modes in a ring resonator.
* Find a band-gap structure for a photonic crystal. Needs Bloch-periodic boundary condition.
* Simulate a wave Y-splitter in the photonic crystal waveguide.


# Time domain

## Wave equation
For a free space the problem can be solved as a [wave equation](https://en.wikipedia.org/wiki/Electromagnetic_wave_equation) 

### Available codes
* [step-23](http://dealii.org/developer/doxygen/deal.II/step_23.html) Wave in the box (all boundaries are reflecting)
* [step-24](http://dealii.org/developer/doxygen/deal.II/step_24.html) Extend step-23 with simple first order approximation to absorbing boundary condition (the reflection from the boundary is visible in linear scale, which is not acceptable for typical EM application with usual PML reflection in the range from -30dB to -110dB). 

![The example](https://docs.google.com/uc?authuser=0&id=0B7jg2ikAVgGLYldpN1VKVzdwcTA&export=download)

* [step-25](http://dealii.org/developer/doxygen/deal.II/step_25.html) The sine-Gordon soliton equation, which is a nonlinear variant of the time dependent wave equation covered in step-23 and step-24.
* [step-48](http://dealii.org/developer/doxygen/deal.II/step_48.html) Explicit time stepping for the Sine–Gordon equation based on a diagonal mass matrix. Efficient implementation of (nonlinear) finite element operators.

### Applied problems

* Surface plasmon-polaritons wave, e.g. like in fig 2 it this [paper](http://iopscience.iop.org/article/10.1088/1367-2630/10/3/033035)
* It would be nice to have some example of non-trivial materials in time-domain, e.g. with dispersion, anisotropy, nonlinearity, etc...
* Solving Mie problem in time domain is a good task to verify the method (it will give the spectral response, which should be post processed with discrete Fourie transform).