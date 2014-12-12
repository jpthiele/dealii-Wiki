# Discussion of possible modifications to DoFHandler

<!-- No auto-Table of Contents support! -->
# Overview

## Features desired from the application point-of-view

  1. Loops over the following subsets of the hierachy of a Triangulation or a DoFHandler have to be efficient:
    - active cells
    - cells on a single level
    - cells on all levels
  1. Each cell accessor must be able to access efficiently:
    - neighbors,
    - children,
    - subobjects of higher codimension.
  1. As much information as possible should be generated algorithmically to avoid memory bandwith
  1. More flexible mesh and dof enumeration structures to improve optimization options

# Proposed Changes

## Interface between meshes and `DoFHandler` objects

### Geometry versus topology

As a first proposal, the mesh geometry should be separated from the mesh topology, see [issue 1](https://github.com/dealii/dealii/issues#issue/1).

<b>Note:</b> Some applications could make use of multi-leafed meshes, where one cell would have several neighbors at the same face. 

### Detangling

Currently, the DoFHandler copies the Triangulation structure to a large extend, in particular the fact, that cells are stored as a hierarchy, while objects with positive codimension are not. If a DoFHandler could be defined by an iterator range, then the current `DoFHandler` would be defined by the active iterators between begin_active() and end(), while `MGDoFHandler` would consist of a set of `DoFHandler`s defined buy the level ranges.

**Warning!** We have to be able to exchange between levels as well as between an "active DoFHandler" and a "level DoFHandler"!
It should be noted, that this technique wastes some memory. For instance, for the "active" DoFHandler, space is allocated for all cells in the hierarchy, even inactive and unused cells. This is a limited amount, which is probably not critical. Still, new interfaces can produce the same waste without impacting the library negatively (reducing waste being a bonus).

Mesh accessors defining a DoFHandler as above must provide a unique identifier (integer) for each object (cell, codim1, codim2 to codimdim). This is needed to access the degrees of freedom of neighboring cells or of the positive codimension objects shared by a cell. It would be ideal if these identifiers were consecutive, but we might settle for the existing holes.

Another important question to be settled is, whether we actually want to iterate over objects of positive codimension. I would assume that a lot simplifies if we do not. Currently, we cannot create FEValues without having a cell.

Here a minimal mesh interface as it might be seen by the DoFHandler

```
template <int dim, int codim>
class MeshAccessor
{
  public:
  types::mesh_index index() const;

  template<int diffdim>
  MeshAccessor<dim,codim+diffdim>& sub_accessor(unsigned int number) const;
  MeshAccessor<dim,codim>& child(unsigned int number) const;
  // This one only if codim==0
  MeshAccessor<dim,codim>& neighbor(unsigned int number) const;
};
```

A few more functions may be needed to identify coarser neighbors.

### First step

In order to simplify DoFHandler implementations, we might decide to just implement the index function in the current triangulation accessors, which can be done comaprably cheaply.

## Functionality of DoFHandler

Currently, most of the actual implementation of DoFHandler is determined by DoFObjects (and its cousin hp::DoFObjects). This is also, where more flexible features can be implemented. Including the current ones, we have

  1. `DoFObjects`, saving the same amount of dofs for each object of same type
  1. `hp::DoFObjects`, saving different dofs and possibly several sets on each type
  1. DoFObjects that only have to save the first dof on each cell etc. Here, we save a lot of memory access, but we do not allow renumbering inside a geometric object
  1. DoFObjects for DG methods, which can be simpler than others

A marriage between the current DoFObjects and hp::DoFObjects involves nothing more than a simple initialization of the latter with equally spaced blocks. This can be done in an afternoon. As for the other options, they extend the functionality and should be discussed for possible later implementation.

### Access to level dofs and active dofs

Currently, MGDoFAccessor (which is going to be reitred and superceded by DoFAccessor) has two functions to access degrees of freedom, get_dof_indices() and get_mg_dof_indices(), which makes writing generic loops a pain. Therefore, it is suggested to have a single function return either or. On a branch, we have such a function, together with a flag in DoFAccessor, which decides when to use which option.

Alternatively, we continue having a separate Accessor/Iterator pair for level access, such that the begin functions would look like

```
class DoFHandler
{
  cell_iterator begin(active);
  level_iterator begin(level);
  level_iterator begin();
};
```

As far as I can see, the regular dofs can only be accessed on active cells, so whenever we iterate through a single or all levels, it must be level dofs. Right?

# Current obstacles

## Closely entangled Trangulation and DoFHandler objects

The structures in DoFHandler, namely DoFLevel and DoFFaces currently mirror the same structures in the Triangulation. This is a questionable practice, since none of the DoFHandler objects has the same hierarchical structure. DoFHandler and hp::DoFHandler only live on the finest level. MGDoFHandler has degrees of freedom on each level and thus duplicates them in vertices.
On the other hand, the Triangulation structures are complicated and require a lot of specializations, which makes maintaining DoFHandler objects difficult.

## Features which may not be beneficial

DoFHandlers have a lot of iterators which may rarely be used, if at all. Raw iterators should not be used in applications and should not be public.
Iterating over quads, lines, contradicts our paradigm of dimension independent programming.Therefore, the corresponding begin and send functions would be considered deprecated. This would reduce the amount of duplicated code considerably.