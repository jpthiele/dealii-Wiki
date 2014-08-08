# Rationale

Most of the local objects in dealii have a tensor product structure. Quadrature formulas are tensor products of one-dimensional formulas. Shape functions are tensor products of polynomials in different coordinate directions. Even spaces like Nédéléc and Raviart-Thomas have such structures in each component. Katharina Kormann and Martin Kronbichler have shown that exploiting these structures can boost performance of local integrals considerably. Therefore, adapting finite element and quadrature classes to their concepts is a valuable task.
