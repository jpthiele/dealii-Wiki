# Rationale

Most of the local objects in dealii have a tensor product
structure. Quadrature formulas are tensor products of one-dimensional
formulas. Shape functions are tensor products of polynomials in
different coordinate directions. Even spaces like Nédéléc and
Raviart-Thomas have such structures in each component. Katharina
Kormann and Martin Kronbichler have shown that exploiting these
structures can boost performance of local integrals
considerably. Therefore, adapting finite element and quadrature
classes to their concepts is a valuable task.

An important ingredient required for the optimization of local
integrals is that the length of all loops is known at compile
time. Therefore, the number of quadrature points and the number of
shape functions must become template arguments.

The class `FEValues` was originally introduced to allow for
optimizations based on the internal structure of finite element shape
function spaces, but this was never exploited. Additionally, `FEValues`
stores a lot of data to avoid recomputation. Data, which is highly
redundant. At a time where computers had a single working thread and
computations were expensive, this was in part justified. Now, that
most algorithms become memory-bound, we must consider retiring
`FEValues`.

## Discussion

- Implementation of *hp* methods
- Efficient implementation of methods where we do not have tensor
  product structure
  - XFEM
  - Overlapping nonmatching grids
- Note: *hp* adaptivity is supported by the `FEEvaluation` class.

## Design and limitations of the `FEEvaluation` class

The matrix-free implementation uses the class `FEEvaluation` instead of `FEValues`. Different from the `FEValues` class, information about the degrees of freedom and the mapping from physical to unit cell is precomputed and stored. The degrees of freedom are numbered MPI-local and the constraints are incorporated into the DoF list in order to provide direct array access into vectors.
This enables e.g. fast evaluation of the shape functions at the quadrature points. On the other hand, only values, gradients, etc. that are precomputed can be accessed while the `FEValues` object enables on-the-fly computations of more general evaluations.
The main task would therefore be to merge the different functionalities of the two classes.


# Tensor product quadrature

It is suggested to introduce a new class template inheriting from
`Quadrature`, say `TensorProductQuadrature`. In a compatibility mode,
this class fills the fields in `Quadrature` (deferred execution
preferred). Quadrature points are stored in a `TensorProductPoints`,
where the access operator `[]` returns a `Point<dim>` created on the
fly. Additionally, it allows direct access to the one-dimensional
point sets. Similarly, we can consider a class `TensorProductWeights`.

Note that while we are currently not making use of this (**this is false**: 
there are classes to integrate singularities which are not based on tensor 
product quadrature formulas...),
`Quadrature` is not restricted to tensor products. Therefore,
`TensorProductQuadrature` is not going to be an equivalent
replacement.

## Discussion

* We will need anisotropic tensor products. Do we want an isotropic
  class as well?
* If anisotropic, are variadic templates the only option?
* The order of the quadrature point (do we make any assumptions yet?)
  - Currently: lexicographic, low to high abscissa
  - Low to high weight for numerical stability?
  - End points of intervals first to conform to finite elements?
* Not inheriting from `Quadrature` would on one hand mean that we have
  two independent quadrature sets, on the other hand we would not need
  to link to the library, which is important for off-loading.

## Draft of the classes

The convention for the indices shall be that `d=1,...,dim`, `di`
enumerates the coordinates in direction `d`, and `i` enumerates all
quadrature points.

```cpp
template <typename number, int dim, int...>
class TensorProductPoints
{
  public:
    template <typename number2>
    void set_coordinates (const unsigned int d, const std::vector<number2>&);
    
    number operator() (const unsigned int d, const unsigned int di) const;
    Point<dim> point(const unsigned int i) const;
};
```

The class `TensorProductWeights` is similar and
`TensorProductQuadrature` combines the two.

# Tensor product structure of polynomials

As Katharina and Martin point out, combining tensor product quadrature
with tensor product polynomials can generate very efficient code. This
has been known to the spectral element community for a while, but
widely ignored by us. It is implemented in the matrix free framework,
but only for certain elements and with limited functionality. One goal
of these new structures is making efficient matrix free computations
available throughout the library.

### Example: Lagrange polynomials

Given polynomials of degree `p-1` and `q` quadrature points in each
direction, the current `FEValues` in 3D computes and stores `p^3 q^3`
shape function values, three times as many derivatives and nine times
as many second derivatives. In (isotropic) tensor product
representation, we need `p q` values, derivatives, and second
derivatives. This means, that now even higher derivatives and 4D become
feasible. As for computation of these values, some polynomial spaces
allow for a recursive computation of the values of higher order
members. Thus, here is another point where we can reduce the
computational complexity by a factor up to `p`.

### Example: vector valued elements

`FE_Nedelec` as well as `FE_RaviartThomas` can be implemented such
that each component is an anisotropic tensor product.

### Example: `FE_DGP`

This example differs from the previous ones in that all multivariate
polynomials are still products of 1D ones, but that not all possible
combinations are admitted. This is true for serendipity elements as
well. For these we need truncated tensor products.

## Discussion

- The matrix free code currently does a renumbering to tensor product
  numbering of the shape functions, which is the inversion of a
  similar process when we generate the functions. Should we reconsider
  the ordering in `DoFHandler`?
- `FEEvaluation` currently knows the elements `FE_Q`, `FE_DGQ`, `FE_DGP` and `FE_Q_DG0`.