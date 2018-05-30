**Disclaimer:** This page is written by a FEM novice, I hope it will finish up with a set of working examples for a code gallery. This way, it can contain some obvious knowledge or simply wrong statements. Feel free to improve the page or e-mail me directly to k.ladutenko@metalab.ifmo.ru

# Rationale

Information about solving Maxwell equation using FEM (and Deal.II) is dispersed, this is an attempt to provide the whole picture in a single place.

# Introduction

There are two distinctive features of Maxwell`s equations, that should be taken into account in the scope of finite element method. 

1. They are vector by nature.
2. They can be discontinuous by definition.
 
For some problems, it can be OK to solve it like a continuous scalar value. However, this can (and for many cases will) lead to spurious solutions or, on the other side, to completely losing the phenomena. Even in a simple case of Helmholtz equation, which is valid both for acoustics (let's consider only pressure waves) and light propagation (time-harmonic Maxwell case) there is a difference, that shouldn't be neglected in general. For the incidence of the acoustic wave to the interface of two media, you can evaluate the reflected and transmitted power just from material parameters and angle of incidence. For the electromagnetic wave, you will additionally need to know the polarization to give the answer: e.g. for Brewster's angle p-polarized wave has no reflection, while the s-polarized wave will be split to the reflected and transmitted parts. This result directly follows from two mentioned features of the electric (and magnetic) field: 1)It is a vector, so it always has some direction. Moreover, the phenomenon may (or may not) depend on this direction. 2) The normal (it is a direction!) vector component has to be discontinuous at a material interface (for most cases), it is a straightforward consequence of Maxwell`s equations. 

To fit this properties of Maxwell`s equations along with Galerkin approach special "vector" types of finite elements were proposed, namely Nedelec (aka Whitney 1st form aka Bossavit aka edge aka H(curl)-conforming) and Raviart-Thomas (aka Whitney 2nd form aka Nedelec 2nd form aka Rao-Wilton-Glisson aka face aka H(div)-conforming) elements. You must use them for simulation of Maxwell`s equations.   
 
# Frequency domain
Time-harmonic Maxwell equation turns to be a Helmholtz equation. It is mostly applicable for free space propagation, cavity problems, scattering on ideal conductors, antennas, and some others, where you can use zero boundary condition on objects surface and the polarization is fixed. This way the problem is naturally reduced to a scalar one (no need for vector finite elements?). To describe more realistic cases you should solve inhomogeneous Helmholtz equation, so you can take into account the surface currents (real or displacement) impressed with the incident field. 

### Available codes

* [step-7](http://dealii.org/developer/doxygen/deal.II/step_7.html) Non-homogeneous Neumann boundary conditions for the Helmholtz equation. Check the convergence with Method of Manufactured Solutions.
* [step-29](http://dealii.org/developer/doxygen/deal.II/step_29.html) Split complex valued functions into their real and imaginary parts. Uses absorbing boundary, however, the angle, when it stops working properly is not very big. See the reflection from "non-reflecting" boundary for the source located close to it.

 ![step 29 mod](https://docs.google.com/uc?authuser=0&id=0B7jg2ikAVgGLRXRRREZSX2F1MTg&export=download)
* Ross Kynch ( @rosskynch ) done some work in this direction. There are a [testBC.cc (google group link)](https://groups.google.com/d/msg/dealii/ZJqmZgObysw/RfyFkbY0D9AJ) and [dealii/tests/fe/nedelec_non_rect_2d.cc](https://github.com/dealii/dealii/blob/master/tests/fe/nedelec_non_rect_2d.cc) solutions using Nedelec elements. 
An improvement (WIP) [PR #2240](https://github.com/dealii/dealii/pull/2240) "New Nedelec finite element: FE_NedelecSZ". [There is a topic](https://groups.google.com/d/msg/dealii/1g3YSUdPSGY/0oW3upegbqMJ) on the problem with face orientation of Nedelec elements (BTW, is it solved now in the current master of deal.ii?). He had also [published](https://github.com/rosskynch/MIT_Forward) a Deal.II solver for Eddy current. [One more topic](https://groups.google.com/d/msg/dealii/odXjp7U3y0s/qXjYyWAefhIJ) on assembly and preconditioning of Nedelec elements.
* [There is a topic](https://groups.google.com/d/msg/dealii/8SbZ04qLwdQ/UReeEYmUFsAJ) by Ce Qin with his source to solve time-harmonic Maxwell equation with complex coefficients using Nedelec elements on a rectangular mesh.
* Related topic to [Computing the curl of a solution vector field obtained from Nedelec elements](https://groups.google.com/d/msg/dealii/iWrNRAH8b6o/GHmCs2oLmtUJ) by David Fernández 
* [Topic by](https://groups.google.com/d/msg/dealii/xpW2-h326Bs/5Nhj9TzHKlgJ) Simon Schernthanner with some useful references.
* There is a  detailed report on [Nedelec elements](http://www.dealii.org/reports/nedelec/nedelec.pdf) (by Anna Schneebeli, University of Basel, Switzerland). 
* [It was reported](http://library.seg.org/doi/abs/10.1190/geo2015-0013.1) in 2015 by Grayver and Kolev to use deal.ii to implement a large-scale 3D geoelectromagnetic modeling using parallel adaptive high-order finite element method. They use Nedelec elements to solve time-harmonic Maxwell equation (so it is a Helmholtz equation), displacement currents were neglected because they are irrelevant at frequencies of interest. It is was shown that h-refinement with higher p-order (p=3) is beneficial to low-p-order for larger models as compared using (CPU time)/error ratio. 

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
For a free space, the problem can be solved as a [wave equation](https://en.wikipedia.org/wiki/Electromagnetic_wave_equation), which is OK for scalar continuous quantity. However, the problem is at it was stated above: electric (or magnetic) field is a vector, which has a natural discontinuity on the interface between two materials.

### Available codes
* [step-23](http://dealii.org/developer/doxygen/deal.II/step_23.html) Wave in the box (all boundaries are reflecting)
* [step-24](http://dealii.org/developer/doxygen/deal.II/step_24.html) Extend step-23 with simple first order approximation to absorbing boundary condition (the reflection from the boundary is visible in linear scale, which is not acceptable for typical EM application with usual PML reflection in the range from -30dB to -110dB). 

![The example](https://docs.google.com/uc?authuser=0&id=0B7jg2ikAVgGLYldpN1VKVzdwcTA&export=download)

* [step-25](http://dealii.org/developer/doxygen/deal.II/step_25.html) The sine-Gordon soliton equation, which is a nonlinear variant of the time dependent wave equation covered in step-23 and step-24.
* [step-48](http://dealii.org/developer/doxygen/deal.II/step_48.html) Explicit time stepping for the Sine–Gordon equation based on a diagonal mass matrix. Efficient implementation of (nonlinear) finite element operators.

### Possible extensions

There are many books and papers on different aspects of using FEM to solve Maxwell`s equations in time domain, the problem is that there are too many ideas, so is even difficult so select which approach to use. 

The one that looks to be done right is by Garry Rodrigue and Daniel White proposed in the paper "A Vector Finite Element Time-Domain Method for Solving Maxwell’s Equations on Unstructured Hexahedral Grids" published in [SIAM J. Sci. Comput.](http://epubs.siam.org/doi/abs/10.1137/S1064827598343826) (it seems to be also available with much more details in D. White [PhD thesis](http://www.osti.gov/scitech/servlets/purl/16341)). The proposed VFETD method uses vector "edge" (Nedelec) finite elements for the electric field and "face" (Raviart-Thomas) finite elements for magnetic flux. It was shown to be second-order accurate, to conserve energy and charge, stable to grid distortions. Similar to FDTD they use an explicit leapfrog time scheme with electric and magnetic field evaluations shifted by a half of the time-step. The mass matrix is solved with an incomplete Cholesky conjugate gradient. There are several examples in the paper, including PML usage (a very basic case with -31dB reflection).

In 2004 Rieben, Rodrigue, and White "Application of Novel High Order Time Domain Vector Finite Element Method to Photonic Band-Cap Waveguides" demonstrated a single-layered PML using a high-order approximation.

In 2005 Rieben, Rodrigue, and White published a paper titled "A high order mixed vector finite element method for solving the time dependent Maxwell equations on unstructured grids" ([preprint is available](https://e-reports-ext.llnl.gov/pdf/305732.pdf)), where they proposed to use any order elements (results for numerical simulations with up to 6-th order are provided), time-stepping up to 4-th order (using symplectic integration), and curvilinear mapping turning VFDTD method to the real state of art!

In 2007 Fisher, White, and  Rodrigue published "An efficient vector finite element method for nonlinear electromagnetic modeling", which is an extension of VFETM to a nonlinear case including many improvements to reduce CPU and memory usage.

In 2008 Donderici and Teixeira published "Conformal Perfectly Matched Layer for the Mixed Finite Element Time-Domain Method". Using second-order time discretization with VFDTD method they showed -68dB performance for a 24 layers of CPML. Note that CMPL is usually very good for arbitrary media simulation (dispersion, nonlinearity, etc) and deals well with near-fields (which original Berenger PML does not). I hope it can be converted to a high order formulation...




### Applied problems

* Surface plasmon-polaritons wave, e.g. like in fig 2 it this [paper](http://iopscience.iop.org/article/10.1088/1367-2630/10/3/033035)
* It would be nice to have some example of non-trivial materials in time-domain, e.g. with dispersion, anisotropy, nonlinearity, etc...
* Solving Mie problem in time domain is a good task to verify the method (it will give the spectral response, which should be post processed with discrete Fourie transform).

### Other improvments

* Rieben, Rodrigue, and White in "Improved Conditioning of Finite Element Matrices Using New High-Order Interpolatory Bases" showed an average 4x speedup on solving several problems due to the usage of a new basis for FE.