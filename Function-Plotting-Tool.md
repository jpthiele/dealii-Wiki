# A program to convert an analytically known function into a visualization

<!-- No auto-Table of Contents support! -->

## Introduction

This is a little program that reads the formula of a function from an input file (along with other information) and generates a graphical representation of this function in one of the formats supported by the deal.II DataOutBase class for plotting in one of the widely used visualization programs.
To use this program, you have to cut and paste the `plot.cc` and `Makefile` to a directory of your choice. 

Change the Makefile to reflect your installation of deal.II. Then say
```
make
```
Run the executable (default is `plot`). This will create a file `parameters.par` that you can edit to change the function, change the output format, etc.


If you call the executable with a command line option, e.g.
```
test@somewhere.com> ./plot myparameters.par

0.0689890:DEAL:SetUpLogging::Log file output.log
0.0709890:DEAL::Finite Element: FE_Q<2>(1)
0.0709890:DEAL::Generated a 1 valued FunctionParser.
0.0969850:DEAL:CreateMesh::Active Cells: 256
0.0969850:DEAL:CreateMesh::Distributing DOFS
0.263959:DEAL:cg::Starting value 0.0818334
0.269958:DEAL:cg::Convergence step 18 value 3.86807e-17
0.269958:DEAL:OutputSolution::Writing system solution: output.gmv
```
the program will read `myparameters.par` and write on whatever you specified in this file. 



## The source file

```
//----------------------------  plot.cc  ---------------------------
//
//    Copyright (C) 2005 by Luca Heltai
//
//    This file is subject to QPL and may not be  distributed
//    without copyright and license information. Please refer
//    to the file deal.II/doc/license.html for the  text  and
//    further information on this license.
//
//----------------------------  plot.cc  ---------------------------

// Base libraries
#include <base/function_parser.h>
#include <base/parameter_handler.h>
#include <base/logstream.h>
#include <base/quadrature_lib.h>

// Grid libraries
#include <grid/tria.h>
#include <grid/grid_in.h>
#include <grid/grid_refinement.h>
#include <grid/grid_generator.h>

// Dofs libraries
#include <dofs/dof_handler.h>
#include <dofs/dof_constraints.h>

// Finite Element libraries.
#include <fe/fe_system.h>
#include <fe/mapping_q.h>
#include <fe/fe_tools.h>

// Linear Algebra Class libraries.
#include <lac/vector.h>

// Numerics libraries
#include <numerics/vectors.h>
#include <numerics/data_out.h>

// C++ libraries
#include <fstream>


#ifdef HAVE_STD_STRINGSTREAM
#  include <sstream>
#  define MY_STR str().c_str()
#  define MY_OSTRSTREAM std::ostringstream
#else
#  include <strstream>
#  define MY_STR str()
#  define MY_OSTRSTREAM  std::ostrstream
#endif
 
class Parameters : public ParameterHandler
{
 public:
  Parameters(int, char**);
  ~Parameters();
  
  /** Reads the parameters. This method reads the input file
      "parameters.par" and set all the variable of this class to
      be used later in the problem. */
  void set_parameters();
  
  /** Log file name. Used by the deallog class. */
  std::string	log_file;
  /** Verbosity of console logging. 0 disables it. */
  unsigned int	log_console_level;
  /** Verbosity of file logging. 0 disables it. */
  unsigned int	log_file_level;
  /** Write time information. Decides wether to write time information
      in the log file. */
  bool		log_record_time;
  /** Use differential time when loggin.*/
  bool		log_differential_time;  
  
  /** Read the domain mesh from a file. If set to false, the domain is
      the hypercube [bool read_mesh_from_file;
  
  /** The global initial refinement.*/
  unsigned int	grid_refinement;

  /** The Finite Element to use.*/
  std::string	  finite_element;
  
  /** The file to read. If there is a command line argument, this file
      is read, if it is there, else it is filled with default
      values.*/
  std::string     file_parameters;

  /** File of Solution.*/
  std::string     file_solution;

  /** Format of the solution. Supported formats:
      dx|ucd|gmv|gpl|eps|pov|tec|tecbin|vtk*/
  std::string     file_solution_format;

  /** Input file for the domain mesh. Supported format: gmsh.*/
  std::string	  file_domain_mesh;

  /** Variables to use in our function. This is the string that
      contains the name of the variables used in the function to
      plot. */
  std::string   variables;
  /** Expression of the function to plot. */
  std::string   expression;
  
  /** Number of Quadrature points used to project. It has to be
      enough.*/
  unsigned int n_q_points;
};
  


/** 
   Output Object. This class is used for all output related things.
*/
template <int dim,
	  typename VECTOR=Vector<double> >
class OutputObject
{
  public:
  
  OutputObject (Parameters * some_par) {
    par = some_par;
  };
 
  /** Output system Solution. The output format is defined in
      according to values set in "parameters.par". \see Parameters
      for available options. */
  void output_solution (const DoFHandler<dim> &dof_handler,
			const VECTOR &solution,
			unsigned int n_components);
  
  /** Setup Logging.*/
  void set_log_file ();
  
  private:
  /** 
      Holds the problem parameters.
  */
  Parameters * par;
  
  /** Output File. */
  std::ofstream log_file;
};


/**
 - This class implements a drawer. We make use of a few classes from
 - the deal.II library, to show how to output a given function
 - interpolated on a given finite element space.
 - @author Luca Heltai, 2005
 */
template <int dim, typename VECTOR=Vector<double> >
class FunctionDraw {
  public:
  FunctionDraw(int, char**); 
  ~FunctionDraw(); 
    
  void run();
  
  private:
  void initialize();
  void create_mesh();
  void output_function();
  
  Parameters par;

  Triangulation<dim> triangulation;
   
  DoFHandler<dim> dof_handler;
  
  OutputObject<dim> out;
  
  VECTOR solution;

  SmartPointer<FiniteElement<dim> > fe;
  
  SmartPointer<FunctionParser<dim> > function;
  
};
    
/** The initialization of the parameters follows the general structure of the
    \class ParameterHandler, with structured input.
*/
Parameters::Parameters(int argc, char ** argv) 
{
  enter_subsection ("Logging");
  
  declare_entry("Log file", "output.log", Patterns::Anything());
  declare_entry("Log console level", "5", Patterns::Integer());
  declare_entry("Log file level", "3", Patterns::Integer());
  declare_entry("Write time information", "true", Patterns::Bool());
  declare_entry("Use differential time", "false", Patterns::Bool());
  
  leave_subsection();
  
  enter_subsection ("I/O Options");

  declare_entry("Write parameter file", "true", Patterns::Bool());
  declare_entry("Parameter file", "parameters.par", Patterns::Anything());

  declare_entry("Solution file", "output",  Patterns::Anything());
  declare_entry("Solution format", "gmv",  
		Patterns::Selection("dx|ucd|gmv|gpl|eps|pov|tec|tecbin|vtk"));
  declare_entry("Read domain mesh from file", "false", Patterns::Bool());
  declare_entry("Domain coarse mesh gmsh file", "square.msh",
		Patterns::Anything()); 

  leave_subsection();
  
  enter_subsection("Numerical Parameters");
  declare_entry("Refinement", "4", Patterns::Integer());
  declare_entry ("Finite element", "FE_Q(1)", Patterns::Anything());
  declare_entry ("Number of quadrature points", "5", Patterns::Integer());
  leave_subsection();
 
  enter_subsection ("Problem Data");
  declare_entry ("Variables", "x,y", Patterns::Anything());
  declare_entry ("Function to plot", "0", Patterns::Anything());
  leave_subsection ();
  
  if(argc > 1) 
    file_parameters = argv[1](0,1]^dim.*/);
  else 
    file_parameters = "parameters.par";
}

Parameters::~Parameters() 
{}

void Parameters::set_parameters ()
{
  read_input(file_parameters);
  
  enter_subsection ("Logging");
  
  log_file		= get("Log file");
  log_console_level	= get_integer("Log console level");
  log_file_level	= get_integer("Log file level");
  log_record_time	= get_bool("Write time information");
  log_differential_time	= get_bool("Use differential time");
  
  leave_subsection();
  
  enter_subsection ("I/O Options");
  

  file_parameters	= get ("Parameter file");

  file_solution		= get ("Solution file");
  file_solution_format	= get ("Solution format");

  read_mesh_from_file	= get_bool("Read domain mesh from file");
  file_domain_mesh	= get ("Domain coarse mesh gmsh file");

  leave_subsection();
  
  enter_subsection ("Numerical Parameters");
  grid_refinement	= get_integer ("Refinement");  
  finite_element	= get("Finite element");
  n_q_points		= get_integer("Number of quadrature points");
  
  leave_subsection();
 
  
  enter_subsection ("Problem Data");
  variables = get("Variables");
  expression = get("Function to plot");
  leave_subsection ();
  
  std::ofstream out (file_parameters.c_str());
  print_parameters(out, Text);
  
}

template <int dim, typename VECTOR>
void OutputObject<dim,VECTOR>::set_log_file () {
   // Initialize Logging to output file.
  log_file.open(par->log_file.c_str());
  deallog.attach(log_file);
  deallog.depth_console(par->log_console_level);
  deallog.depth_file(par->log_file_level);
  deallog.log_execution_time (par->log_record_time);
  deallog.log_time_differences (par->log_differential_time);
  deallog.push("SetUpLogging");
  deallog << "Log file " 
	  << par->log_file << std::endl;
  deallog.pop();
}

template <int dim, typename VECTOR>
void OutputObject<dim,VECTOR>::output_solution(const DoFHandler<dim> &dof_handler,
					       const VECTOR &solution,
					       unsigned int n_components)
{
  deallog.push("OutputSolution");
  
  char   filename[sprintf(filename,"%s.%s", par->file_solution.c_str(),
	  par->file_solution_format.c_str());
    
  deallog << "Writing system solution: " << filename 
	  << std::endl;
    
    
  std::ofstream output (filename );
    
  DataOut<dim> data_out;
    
  std::vector<std::string> solution_names; 
  
  for (unsigned int i=1; i<= n_components; ++i) {
    MY_OSTRSTREAM tmp;
    tmp << "v_" << i;
    solution_names.push_back( tmp.MY_STR );
  }								
  
  data_out.attach_dof_handler (dof_handler);
  data_out.add_data_vector (solution, solution_names);
    
  data_out.build_patches (1);
  if (par->file_solution_format == "dx") data_out.write_dx (output);
  else if (par->file_solution_format == "ucd" ) data_out.write_ucd (output);
  else if (par->file_solution_format == "gpl" ) data_out.write_gnuplot (output);
  else if (par->file_solution_format == "eps" ) data_out.write_eps (output);
  else if (par->file_solution_format == "pov" ) data_out.write_povray (output);
  else if (par->file_solution_format == "tec" ) data_out.write_tecplot (output);
  else if (par->file_solution_format == "tecbin" ) data_out.write_tecplot_binary (output);
  else if (par->file_solution_format == "vtk" ) data_out.write_vtk (output);
  else if (par->file_solution_format == "gmv" ) data_out.write_gmv (output);
  else {
    Assert(false, ExcInternalError());
  }
  deallog.pop();
}

template <int dim, typename VECTOR>
FunctionDraw<dim,VECTOR>::FunctionDraw(int argc, char ** argv) :
  par(argc, argv),
  dof_handler(triangulation),
  out(&par)
{}

template <int dim, typename VECTOR>
FunctionDraw<dim,VECTOR>::~FunctionDraw() 
{
  dof_handler.clear();

  FunctionParser<dim> * function_real = function;
  function = 0;
  delete function_real;
  
  FiniteElement<dim> * fe_real = fe;
  fe = 0;
  delete fe_real;
}


template <int dim, typename VECTOR>
void FunctionDraw<dim,VECTOR>::initialize()
{
  par.set_parameters();
  out.set_log_file();
  
  // Creates a new pointer to a FESystem
  FiniteElement<dim> *fe_real = 
    FETools::get_fe_from_name<dim>(par.finite_element);
  // Associates a smart pointer to the newly created pointer.
  fe = fe_real;
  
  deallog << "Finite Element: " << fe->get_name() << std::endl;
 
  // generate a new function parser
  FunctionParser<dim> * function_real = 
    new FunctionParser<dim>(fe->n_components());
  // assign it to our function
  function = function_real;
  
  deallog << "Generated a " << fe->n_components() 
	  << " valued FunctionParser." << std::endl;
    
  
  // Constants we want to pass to the function parser
  std::map<std::string, double> constants;
  constants["pi"](100];) = M_PI;
  
  function->initialize(par.variables,
		       par.expression,
		       constants);
  
}

template <int dim, typename VECTOR>
void FunctionDraw<dim,VECTOR>::run()
{
  initialize();
  
  create_mesh();

  output_function();
}

template <int dim, typename VECTOR>
void FunctionDraw<dim,VECTOR>::create_mesh()
{
  deallog.push("CreateMesh");
  
  if(par.read_mesh_from_file) {
    GridIn<dim> grid_in;
    grid_in.attach_triangulation (triangulation);
    std::ifstream input_file(par.file_domain_mesh.c_str()); 
    grid_in.read_msh(input_file); 
  } else {
    GridGenerator::hyper_cube (triangulation, 0, 1);
  }
  
  triangulation.refine_global (par.grid_refinement);
  
  deallog << "Active Cells: "
	  << triangulation.n_active_cells()
	  << std::endl;
  
  deallog << "Distributing DOFS" << std::endl;
  
  dof_handler.distribute_dofs (*fe);
  solution.reinit(dof_handler.n_dofs());
  
  deallog.pop();
}

template <int dim, typename VECTOR>
void FunctionDraw<dim,VECTOR>::output_function()
{
  QGauss< dim > quadrature(par.n_q_points);
  ConstraintMatrix constraints;
  constraints.close();
  MappingQ<dim> mapping(1);
  
  VectorTools::project(mapping,
		       dof_handler,
		       constraints,
		       quadrature,
		       *function,
		       solution);

  out.output_solution(dof_handler,
		      solution,
		      fe->n_components() );
  
}

int main (int argc, char ** argv)
{
  try
    {  
      FunctionDraw<2> draw(argc, argv);
      draw.run();
    }
  catch (std::exception &exc)
    {
      std::cerr << std::endl << std::endl
                << "----------------------------------------------------"
                << std::endl;
      std::cerr << "Exception on processing: " << std::endl
                << exc.what() << std::endl
                << "Aborting!" << std::endl
                << "----------------------------------------------------"
                << std::endl;
 
      return 1;
    }
  catch (...)
    {
      std::cerr << std::endl << std::endl
                << "----------------------------------------------------"
                << std::endl;
      std::cerr << "Unknown exception!" << std::endl
		<< "Aborting!" << std::endl
                << "----------------------------------------------------"
                << std::endl;
      return 1;
    };
 
  return 0;
}                                
```


## The Makefile

```

target = plot

debug-mode = on


D = /usr/local/deal.II


clean-up-files = *gmv *gnuplot *gpl *eps **pov




include $D/common/Make.global_options


libs.g   = $(lib-deal2-2d.g) \
           $(lib-lac.g)      \
           $(lib-base.g)
libs.o   = $(lib-deal2-2d.o) \
           $(lib-lac.o)      \
           $(lib-base.o)


ifeq ($(debug-mode),on)
  libraries = $(target).g.$(OBJEXT) $(libs.g)
else
  libraries = $(target).$(OBJEXT) $(libs.o)
endif


$(target) : $(libraries)
        @echo ============================ Linking $@
        @$(CXX) -o $@$(EXEEXT) $^ $(LIBS) $(LDFLAGS)


run: $(target)
        @echo ============================ Running $<
        @./$(target)$(EXEEXT)


clean:
        -rm -f **.$(OBJEXT) **~ Makefile.dep $(target)$(EXEEXT) $(clean-up-files)


./%.g.$(OBJEXT) :
        @echo ==============debug========= $(<F)
        @$(CXX) $(CXXFLAGS.g) -c $< -o $@
./%.$(OBJEXT) :
        @echo ==============optimized===== $(<F)
        @$(CXX) $(CXXFLAGS.o) -c $< -o $@


.PHONY: run clean


Makefile.dep: $(target).cc Makefile \
              $(shell echo $D/**/include/**/**.h)
        @echo ============================ Remaking $@
        @$D/common/scripts/make_dependencies  $(INCLUDE) -B. $(target).cc \
                > Makefile.dep
        @if test -s $@ ; then : else rm $@ ; fi


include Makefile.dep

```


## An example parameter file

```
# Listing of Parameters
# ---------------------
subsection I/O Options
  set Domain coarse mesh gmsh file = square.msh
  set Parameter file               = parameters.par
  set Read domain mesh from file   = false
  set Solution file                = output
  set Solution format              = gmv
  set Write parameter file         = true
end


subsection Logging
  set Log console level      = 5
  set Log file               = output.log
  set Log file level         = 3
  set Use differential time  = false
  set Write time information = true
end


subsection Numerical Parameters
  set Finite element              = FE_Q(1)
  set Number of quadrature points = 5
  set Refinement                  = 4
end


subsection Problem Data
  set Function to plot = exp(x*y) # default: 0
  set Variables        = x,y
end
</pre>

## Another example parameter file

<pre>
# Listing of Parameters
# ---------------------
subsection I/O Options
  set Domain coarse mesh gmsh file = square.msh
  set Parameter file               = parameters.par
  set Read domain mesh from file   = false
  set Solution file                = output
  set Solution format              = gmv
  set Write parameter file         = true
end


subsection Logging
  set Log console level      = 5
  set Log file               = output.log
  set Log file level         = 3
  set Use differential time  = false
  set Write time information = true
end


subsection Numerical Parameters
  set Finite element              = FESystem[set Number of quadrature points = 5
  set Refinement                  = 4
end


subsection Problem Data
  set Function to plot = sin(pi*y);sin(pi*x) # default: 0
  set Variables        = x,y
end
```

--[[User:Luca|Luca]](FE_Q(1)^2]) 08:39, 5 Apr 2005 (CEST)