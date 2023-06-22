### Motivation

As of version 9.1, deal.II allows one to generate _vtk_ and _vtu_ output using high-order [Lagrange VTK cells](https://blog.kitware.com/modeling-arbitrary-order-lagrange-finite-elements-in-the-visualization-toolkit/). As their name suggests, these cells are described by a set of Lagrange points with numerical quantities attached to them. This becomes useful when you are working with high-order elements and/or high-order mappings, since these objects can be represented more accurately. 

This page provides instructions on how the high-order VTK meshes can be visualized using ParaView (**note**: this feature was implemented in the version 5.5, older versions will not be able to visualize these meshes).

### Creating high-order output

First, we produced 2D and 3D _vtu_ output of a spherical shell mesh using the program provided below. Specifically, the mesh was generated using the `GridGenerator::hyper_shell` function, which attaches a `SphericalManifold` manifold to all cells. Subsequently, the mesh was attached to a `DoFHandler` object with a `FE_Q` element of the order four. A trigonometric function was interpolated to the underlying discrete high-order finite-element space using the `MappingQGeneric` mapping of order four. 

Finally, we write the mesh along with the finite-element representation of the function to a _vtu_ file using `DataOut::write_vtu` with high-order Lagrange cells of 4-th order. Note that the cell order in this context relates to both the description of a cell shape and all quantities (e.g., vector and scalar fields) attached to it. Since we used the 4-th order finite element in this example to represent a function on our mesh, writing the result using cells of orders higher than four will not really give us much in terms of representing the function more accurately (unless your basis functions behave very differently compared to Lagrange polynomials used by VTK, which is not the case here since we used `FE_Q`). However, our mesh represents a spherical shell, which, strictly speaking, cannot be described by a polynomial precisely. Therefore, increasing the cell order will allow one to store and visualize the shape more accurately. Note that for this to produce an effect, you will have to increase the order of the `MappingQGeneric` mapping accordingly. All this, of course, comes at an additional computational cost and memory. Thus, it requires finding a compromise for a problem you are solving. 

### Visualization

The produced _vtu_ files were loaded in ParaView v5.7. (As of 2023, Visit 3.3.3 is still not able to read these files correctly, and so ParaView is your only choice in visualizing this kind of data for the moment.) Even though the mesh is high-order, ParaView uses (bi-/tri-)linear interpolation between the Lagrange points by default. To enable the high-order interpolation, you need to locate the property "Nonlinear Subdivision Level" in the object properties (the easiest way to do this is to search for this field by its name) and set the value to a number larger than one, according to your needs:

![Nonlinear Subdivision Level](https://github.com/agrayver/dealii_wiki_imgs/blob/master/subdivision_option.png)

For our example, the 2D result looks like this:

![2D shell](https://github.com/agrayver/dealii_wiki_imgs/blob/master/shell_2d.png)

Accordingly, here is the 3D mesh:

![3D shell](https://github.com/agrayver/dealii_wiki_imgs/blob/master/shell_3d.png)

You can see now that cells have curved shapes and the quantity within each cell varies as a high-order (specifically, 4-th order) function.  

### Extra details

For more information on the deal.II's implementation of this feature, see the documentation of the [DataOut::build_patches](https://www.dealii.org/current/doxygen/deal.II/classDataOut.html#a5eb51872b8736849bb7e8d2007fae086) method. Additionally, step-53 and step-65 provide some more examples and details about high-order mappings, their internal representation and output. 

### Code

The images shown above were generated using the _vtu_ files produced by the program below:

```c++
#include <deal.II/grid/tria.h>
#include <deal.II/grid/grid_generator.h>
#include <deal.II/numerics/data_out.h>
#include <deal.II/dofs/dof_handler.h>
#include <deal.II/numerics/vector_tools.h>
#include <deal.II/fe/mapping_q_generic.h>

#include <iostream>
#include <fstream>
#include <sstream>
#include <cmath>

using namespace dealii;

template<int dim>
class TestFunction: public Function<dim>
{
public:
  double value(const Point<dim> &p, const unsigned int component = 0) const
  {
    double v = 0;
    for(int d = 0; d < dim; ++d)
    {
      v += cos(2 * numbers::PI * p[d]);
    }

    return v;
  }
};

template<int dim>
void shell_grid(unsigned fe_order,
                unsigned mapping_order,
                unsigned output_mesh_order)
{
  FE_Q<dim> fe(fe_order);
  MappingQGeneric<dim> mapping(mapping_order);

  Triangulation<dim> triangulation;
  GridGenerator::hyper_shell(triangulation, Point<dim>(), 0.5, 1., 0, true);

  for(unsigned n = 0; n < 2; ++n)
  {
    for(auto cell: triangulation.active_cell_iterators())
    {
      for(unsigned face = 0; face < GeometryInfo<dim>::faces_per_cell; ++face)
        if(cell->face(face)->at_boundary() &&
           cell->face(face)->boundary_id() == 1)
          cell->set_refine_flag();
    }
    triangulation.execute_coarsening_and_refinement();
  }

  DoFHandler<dim> dof_handler(triangulation);
  dof_handler.distribute_dofs(fe);

  TestFunction<dim> function;
  Vector<double> vec(dof_handler.n_dofs());
  VectorTools::interpolate(mapping, dof_handler, function, vec);

  DataOutBase::VtkFlags flags;
  flags.write_higher_order_cells = true;

  std::stringstream ss;
  ss << "shell_dim=" << dim
     << "_p=" << fe_order
     << "_mapping=" << mapping_order
     << "_n=" << output_mesh_order
     << ".vtu";

  std::ofstream out(ss.str());
  DataOut<dim> data_out;
  data_out.set_flags(flags);
  data_out.attach_dof_handler(dof_handler);
  data_out.add_data_vector(vec, "vec");
  data_out.build_patches(mapping, output_mesh_order, DataOut<dim>::curved_inner_cells);
  data_out.write_vtu(out);
}

int main()
{
  const unsigned fe_order = 4;
  const unsigned mapping_order = 4;
  const unsigned output_mesh_order = 4;

  shell_grid<2>(fe_order, mapping_order, output_mesh_order);
  shell_grid<3>(fe_order, mapping_order, output_mesh_order);
}
```