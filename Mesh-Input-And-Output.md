# A little script to illustrate exchanging data with Cubit

### Cubit

The following cubit journal file (actually a python script) exports the current mesh and 
boundary conditions from cubit to a file "output.ucd" in the current directory.
The [output file format](http://www.csit.fsu.edu/~burkardt/data/ucd/ucd.html) is [`AVS UCD`](http://vis.lbl.gov/NERSC/Software/express/help6.2/help/reference/dvmac/UCD_Form.htm).

This is a modification of the original script that takes into account boundary ids via
sidesets ids. If you want to save the boundary faces as well, you just need to 
add the relevant surfaces in 3d or curves in 2d to a sideset, and the id will be the one of the
sideset. 

```python
#!python
# This script will output whatever mesh you have currently in CUBIT
# in the AVS UCD format. (http://www.csit.fsu.edu/~burkardt/data/ucd/ucd.html)
# You may need to tweak it to get exactly what you want out of
# it.  Your mileage may vary.

# set the filename -- you may need the entire path
outucdfile = "output.ucd"

outfile = open(outucdfile,"w")

cubit.cmd("body all rotate -90 about y")
cubit.cmd("body all reflect x")

# ============================================================
# Collect all the nodes
# ============================================================
group_id = cubit.get_id_from_name("temp_bc_curves")
if group_id != 0:
  cubit.cmd("delete group " + str(group_id))
cubit.cmd("group 'temp_nodes' add node all")
group_id = cubit.get_id_from_name("temp_nodes")
node_list = cubit.get_group_nodes(group_id)
cubit.cmd("delete group " + str(group_id))
n_nodes = len(node_list)

# ============================================================
# Collect all the hex
# ============================================================
group_id = cubit.get_id_from_name("temp_hexes")
if group_id != 0:
  cubit.cmd("delete group " + str(group_id))
cubit.cmd("group 'temp_hexes' add hex all")
group_id = cubit.get_id_from_name("temp_hexes")
hex_list = cubit.get_group_hexes(group_id)
cubit.cmd("delete group " + str(group_id))
n_hex_cells = len(hex_list)


# ============================================================ 
# Now the boundary conditions in 3d
# ============================================================
bc_surfaces = {}
n_bc_quads = 0

bc_ids = cubit.get_sideset_id_list()
for bc_id in bc_ids :
  bc_surfaces[= cubit.get_sideset_surfaces(bc_id)
  for bc_surface in  bc_surfaces[bc_id](bc_id]):
    bc_quads = cubit.get_surface_quads(bc_surface)
    n_bc_quads += len(bc_quads)


# ============================================================ 
# Collect all the surfaces. Notice that the surfaces that make up a
# volume are not grouped here. This is only for 2d objects, i.e.,
# when the number n_hex_cells is zero.
# ============================================================
surface_list = ()
quad_cell_list = {}
n_quad_cells = 0
if n_hex_cells == 0:
  group_id = cubit.get_id_from_name("temp_surfs")
  if group_id != 0:
    cubit.cmd("delete group " + str(group_id))
  cubit.cmd("group 'temp_surfs' add surf all")
  group_id = cubit.get_id_from_name("temp_surfs")
  surface_list = cubit.get_group_surfaces(group_id)
  cubit.cmd("delete group " + str(group_id))
  for surface_id in surface_list:
    quad_cell_list[= cubit.get_surface_quads(surface_id)
    n_quad_cells +=  len(quad_cell_list[surface_id](surface_id]))

# ============================================================ 
# Now the boundary conditions in 2d
# ============================================================
bc_curves = {}
bc_edges = {}
n_bc_edges = 0
if n_hex_cells == 0:
  bc_ids = cubit.get_sideset_id_list()
  for bc_id in bc_ids :
    bc_curves[= cubit.get_sideset_curves(bc_id)
    group_id = cubit.get_id_from_name("temp_bc_curves")
    if group_id != 0:
      cubit.cmd("delete group " + str(group_id))
    for bc_curve in bc_curves[bc_id](bc_id]):
      cubit.cmd("group 'temp_bc_curves' add edge all in curve " + str(bc_curve))
    group_id = cubit.get_id_from_name("temp_bc_curves")
    bc_edges[= cubit.get_group_edges(group_id)
    cubit.cmd("delete group " + str(group_id))
    n_bc_edges +=  len(bc_edges[bc_id](bc_id]))

print 'Edges: ' + str(n_bc_edges)

# ============================================================
# Now we write the header.
# ============================================================
n_elements = n_hex_cells + n_bc_quads + n_quad_cells + n_bc_edges
outfile.write(str(n_nodes) + " " + str(n_elements) + " 0 0 0\n")


# ============================================================
# The node list.
# ============================================================
for node_num in node_list:
   outfile.write(str(node_num ))
   outfile.write("\t")
   node_coord = cubit.get_nodal_coordinates(node_num)
   if abs(node_coord[outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[2](2])<1e-15:)) + " ")
   if abs(node_coord[outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[1](1])<1e-15:)) + " ")
   if abs(node_coord[outfile.write("0")
   else	:
       outfile.write(str(node_coord[0](0])<1e-15:)))
   outfile.write("\n")


# ============================================================
# The hex list. 3d
# ============================================================
k = 1
for hex_num in hex_list:
   outfile.write(str(k) + " 0 " + " hex  ")
   k += 1
   hex_nodes = cubit.get_connectivity("Hex", hex_num)
   i = 0
   while i < 8:
      outfile.write(str(hex_nodes[+ " ")
      i += 1
   outfile.write("\n")


# ============================================================
# The quads on the boundaries. 3d
# Note that the boundary id is given by the sideset id.
# ============================================================
if n_hex_cells != 0:
  k=1
  for bc_id in bc_ids:
    for bc_surface in  bc_surfaces[bc_id](i])):
      bc_quads = cubit.get_surface_quads(bc_surface)
      for quad_num in bc_quads:
        outfile.write(str(k))
        k += 1
        outfile.write(" " + str(bc_id) + " quad ")
        quad_nodes = cubit.get_connectivity("Quad", quad_num)
        outfile.write(str(quad_nodes[+ " ")
        outfile.write(str(quad_nodes[3](0]))) + " ")
        outfile.write(str(quad_nodes[+ " ")
        outfile.write(str(quad_nodes[1](2]))))
        outfile.write("\n")

# ============================================================
# The quads on the surfaces. 2d
# ============================================================
if n_hex_cells == 0:
  k = 1
  for surface_id in surface_list:
    for quad_num in quad_cell_list[outfile.write(str(k))
      k += 1
      outfile.write(" " + str(surface_id) + " quad ")
      quad_nodes = cubit.get_connectivity("Quad", quad_num)
      outfile.write(str(quad_nodes[0](surface_id]:)) + " ")
      outfile.write(str(quad_nodes[+ " ")
      outfile.write(str(quad_nodes[2](3]))) + " ")
      outfile.write(str(quad_nodes[outfile.write("\n")

# ============================================================
# The edges on the curves. 2d
# Boundary id = sideset_id
# ============================================================
if n_hex_cells == 0:
  k=1
  for bc_id in bc_ids:
    for edge_num in bc_edges[bc_id](1]))):
      outfile.write(str(k))
      k += 1
      outfile.write(" " + str(bc_id) + " line ")
      edge_nodes = cubit.get_connectivity("Edge", edge_num)
      outfile.write(str(edge_nodes[+ " ")
      outfile.write(str(edge_nodes[1](0]))))
      outfile.write("\n")

outfile.close()

cubit.cmd("body all reflect x")
cubit.cmd("body all rotate 90 about y")

print str(n_nodes) + " nodes\n"
print str(n_hex_cells) + " hexes\n"
print str(n_quad_cells) + " quads\n"
print str(n_bc_quads) + " face_quads\n"
print str(n_bc_edges) + " edges\n"

```

# Alternate versions of this script

### Cubit 15.2

The following script, which also outputs material IDs (as identified by the `block id` to which each element belongs), has been [verified to work](https://groups.google.com/forum/#!topic/dealii/ZnU3XIK_ipk) with `Cubit v15.2`. 

```python
#!python
# This script will output whatever mesh you have currently in CUBIT
# in the AVS UCD format. (http://www.csit.fsu.edu/~burkardt/data/ucd/ucd.html)
# You may need to tweak it to get exactly what you want out of
# it.  Your mileage may vary.

# set the filename -- you may need the entire path
outucdfile = "output.ucd"

outfile = open(outucdfile,"w")

cubit.cmd("body all rotate -90 about y")
cubit.cmd("body all reflect x")

# ============================================================
# Collect all the nodes
# ============================================================
group_id = cubit.get_id_from_name("temp_bc_curves")
if group_id != 0:
  cubit.cmd("delete group " + str(group_id))
cubit.cmd("group 'temp_nodes' add node all")
group_id = cubit.get_id_from_name("temp_nodes")
node_list = cubit.get_group_nodes(group_id)
cubit.cmd("delete group " + str(group_id))
n_nodes = len(node_list)

# ============================================================
# Collect all the hex
# ============================================================
hex_cell_list = {}
n_hex_cells = 0

block_ids = cubit.get_block_id_list()
for block_id in block_ids:
    hex_cell_list[block_id] = cubit.get_block_hexes(block_id)
    n_hex_cells += len(hex_cell_list[block_id])

# ============================================================
# Now the boundary conditions in 3d
# ============================================================
bc_surfaces = {}
n_bc_quads = 0

bc_ids = cubit.get_sideset_id_list()
for bc_id in bc_ids :
  bc_surfaces[bc_id] = cubit.get_sideset_surfaces(bc_id)
  for bc_surface in  bc_surfaces[bc_id]:
    bc_quads = cubit.get_surface_quads(bc_surface)
    n_bc_quads += len(bc_quads)


# ============================================================
# Collect all the surfaces. Notice that the surfaces that make up a
# volume are not grouped here. This is only for 2d objects, i.e.,
# when the number n_hex_cells is zero.
# ============================================================
surface_list = ()
quad_cell_list = {}
n_quad_cells = 0
if n_hex_cells == 0:
  group_id = cubit.get_id_from_name("temp_surfs")
  if group_id != 0:
    cubit.cmd("delete group " + str(group_id))
  cubit.cmd("group 'temp_surfs' add surf all")
  group_id = cubit.get_id_from_name("temp_surfs")
  surface_list = cubit.get_group_surfaces(group_id)
  cubit.cmd("delete group " + str(group_id))
  for surface_id in surface_list:
    quad_cell_list[surface_id] = cubit.get_surface_quads(surface_id)
    n_quad_cells +=  len(quad_cell_list[surface_id])

# ============================================================
# Now the boundary conditions in 2d
# ============================================================
bc_curves = {}
bc_edges = {}
n_bc_edges = 0
if n_hex_cells == 0:
  bc_ids = cubit.get_sideset_id_list()
  for bc_id in bc_ids :
    bc_curves[bc_id]    = cubit.get_sideset_curves(bc_id)
    group_id = cubit.get_id_from_name("temp_bc_curves")
    if group_id != 0:
      cubit.cmd("delete group " + str(group_id))
    for bc_curve in bc_curves[bc_id]:
      cubit.cmd("group 'temp_bc_curves' add edge all in curve " + str(bc_curve))
    group_id = cubit.get_id_from_name("temp_bc_curves")
    bc_edges[bc_id] = cubit.get_group_edges(group_id)
    cubit.cmd("delete group " + str(group_id))
    n_bc_edges +=  len(bc_edges[bc_id])

print 'Edges: ' + str(n_bc_edges)

# ============================================================
# Now we write the header.
# ============================================================
n_elements = n_hex_cells + n_bc_quads + n_quad_cells + n_bc_edges
outfile.write(str(n_nodes) + " " + str(n_elements) + " 0 0 0\n")


# ============================================================
# The node list.
# ============================================================
for node_num in node_list:
   outfile.write(str(node_num ))
   outfile.write("\t")
   node_coord = cubit.get_nodal_coordinates(node_num)
   if abs(node_coord[2])<1e-15:
       outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[2]) + " ")
   if abs(node_coord[1])<1e-15:
       outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[1]) + " ")
   if abs(node_coord[0])<1e-15:
       outfile.write("0")
   else	:
       outfile.write(str(node_coord[0]))
   outfile.write("\n")


# ============================================================
# The hex list. 3d
# ============================================================
k = 1
for block_id in block_ids:
    for hex_num in hex_cell_list[block_id]:
        print str(k) + " " + str(block_id) + "  hex  "
        outfile.write(str(k) + " " + str(block_id) + "  hex  ")
        k += 1
        hex_nodes = cubit.get_connectivity("Hex", hex_num)
        i = 0
        while i < 8:
            outfile.write(str(hex_nodes[i]) + " ")
            i += 1
        outfile.write("\n")

# ============================================================
# The quads on the boundaries. 3d
# Note that the boundary id is given by the sideset id.
# ============================================================
if n_hex_cells != 0:
  k=1
  for bc_id in bc_ids:
    for bc_surface in  bc_surfaces[bc_id]:
      bc_quads = cubit.get_surface_quads(bc_surface)
      for quad_num in bc_quads:
        outfile.write(str(k))
        k += 1
        outfile.write(" " + str(bc_id) + " quad ")
        quad_nodes = cubit.get_connectivity("Quad", quad_num)
        outfile.write(str(quad_nodes[0]) + " ")
        outfile.write(str(quad_nodes[3]) + " ")
        outfile.write(str(quad_nodes[2]) + " ")
        outfile.write(str(quad_nodes[1]))
        outfile.write("\n")

# ============================================================
# The quads on the surfaces. 2d
# ============================================================
if n_hex_cells == 0:
  k = 1
  for surface_id in surface_list:
    for quad_num in quad_cell_list[surface_id]:
      outfile.write(str(k))
      k += 1
      outfile.write(" " + str(surface_id) + " quad ")
      quad_nodes = cubit.get_connectivity("Quad", quad_num)
      outfile.write(str(quad_nodes[0]) + " ")
      outfile.write(str(quad_nodes[3]) + " ")
      outfile.write(str(quad_nodes[2]) + " ")
      outfile.write(str(quad_nodes[1]))
      outfile.write("\n")

# ============================================================
# The edges on the curves. 2d
# Boundary id = sideset_id
# ============================================================
if n_hex_cells == 0:
  k=1
  for bc_id in bc_ids:
    for edge_num in bc_edges[bc_id]:
      outfile.write(str(k))
      k += 1
      outfile.write(" " + str(bc_id) + " line ")
      edge_nodes = cubit.get_connectivity("Edge", edge_num)
      outfile.write(str(edge_nodes[0]) + " ")
      outfile.write(str(edge_nodes[1]))
      outfile.write("\n")

outfile.close()

cubit.cmd("body all reflect x")
cubit.cmd("body all rotate 90 about y")

print str(n_nodes) + " nodes\n"
print str(n_hex_cells) + " hexes\n"
print str(n_quad_cells) + " quads\n"
print str(n_bc_quads) + " face_quads\n"
print str(n_bc_edges) + " edges\n"
```

### Cubit 13.2
In case it is of any use, one of the initial versions of this script is also provided below. This has been verified to work on `Cubit v13.2`, but unfortunately does not output material IDs for reasons documented in the paragraphs below.

```python
#!python
# This script will output whatever mesh you have currently in CUBIT
# in the AVS UCD format. (http://www.csit.fsu.edu/~burkardt/data/ucd/ucd.html)
# You may need to tweak it to get exactly what you want out of
# it.  Your mileage may vary.

# set the filename -- you may need the entire path
outucdfile = "output.ucd"

outfile = open(outucdfile,"w")

cubit.cmd("body all rotate -90 about y")
cubit.cmd("body all reflect x")

# ============================================================
# Collect all the nodes
# ============================================================
group_id = cubit.get_id_from_name("temp_bc_curves")
if group_id != 0:
  cubit.cmd("delete group " + str(group_id))
cubit.cmd("group 'temp_nodes' add node all")
group_id = cubit.get_id_from_name("temp_nodes")
node_list = cubit.get_group_nodes(group_id)
cubit.cmd("delete group " + str(group_id))
n_nodes = len(node_list)

# ============================================================
# Collect all the hex
# ============================================================
group_id = cubit.get_id_from_name("temp_hexes")
if group_id != 0:
  cubit.cmd("delete group " + str(group_id))
cubit.cmd("group 'temp_hexes' add hex all")
group_id = cubit.get_id_from_name("temp_hexes")
hex_list = cubit.get_group_hexes(group_id)
cubit.cmd("delete group " + str(group_id))
n_hex_cells = len(hex_list)


# ============================================================
# Now the boundary conditions in 3d
# ============================================================
bc_surfaces = {}
n_bc_quads = 0

bc_ids = cubit.get_sideset_id_list()
for bc_id in bc_ids :
  bc_surfaces[bc_id]    = cubit.get_sideset_surfaces(bc_id)
  for bc_surface in  bc_surfaces[bc_id]:
    bc_quads = cubit.get_surface_quads(bc_surface)
    n_bc_quads += len(bc_quads)


# ============================================================
# Collect all the surfaces. Notice that the surfaces that make up a
# volume are not grouped here. This is only for 2d objects, i.e.,
# when the number n_hex_cells is zero.
# ============================================================
surface_list = ()
quad_cell_list = {}
n_quad_cells = 0
if n_hex_cells == 0:
  group_id = cubit.get_id_from_name("temp_surfs")
  if group_id != 0:
    cubit.cmd("delete group " + str(group_id))
  cubit.cmd("group 'temp_surfs' add surf all")
  group_id = cubit.get_id_from_name("temp_surfs")
  surface_list = cubit.get_group_surfaces(group_id)
  cubit.cmd("delete group " + str(group_id))
  for surface_id in surface_list:
    quad_cell_list[surface_id] = cubit.get_surface_quads(surface_id)
    n_quad_cells +=  len(quad_cell_list[surface_id])

# ============================================================
# Now the boundary conditions in 2d
# ============================================================
bc_curves = {}
bc_edges = {}
n_bc_edges = 0
if n_hex_cells == 0:
  bc_ids = cubit.get_sideset_id_list()
  for bc_id in bc_ids :
    bc_curves[bc_id]    = cubit.get_sideset_curves(bc_id)
    group_id = cubit.get_id_from_name("temp_bc_curves")
    if group_id != 0:
      cubit.cmd("delete group " + str(group_id))
    for bc_curve in bc_curves[bc_id]:
      cubit.cmd("group 'temp_bc_curves' add edge all in curve " + str(bc_curve))
    group_id = cubit.get_id_from_name("temp_bc_curves")
    bc_edges[bc_id] = cubit.get_group_edges(group_id)
    cubit.cmd("delete group " + str(group_id))
    n_bc_edges +=  len(bc_edges[bc_id])

print 'Edges: ' + str(n_bc_edges)

# ============================================================
# Now we write the header.
# ============================================================
n_elements = n_hex_cells + n_bc_quads + n_quad_cells + n_bc_edges
outfile.write(str(n_nodes) + " " + str(n_elements) + " 0 0 0\n")


# ============================================================
# The node list.
# ============================================================
for node_num in node_list:
   outfile.write(str(node_num ))
   outfile.write("\t")
   node_coord = cubit.get_nodal_coordinates(node_num)
   if abs(node_coord[2])<1e-15:
       outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[2]) + " ")
   if abs(node_coord[1])<1e-15:
       outfile.write("0 ")
   else	:
       outfile.write(str(node_coord[1]) + " ")
   if abs(node_coord[0])<1e-15:
       outfile.write("0")
   else	:
       outfile.write(str(node_coord[0]))
   outfile.write("\n")


# ============================================================
# The hex list. 3d
# ============================================================
k = 1
for hex_num in hex_list:
   outfile.write(str(k) + " 0 " + " hex  ")
   k += 1
   hex_nodes = cubit.get_connectivity("Hex", hex_num)
   i = 0
   while i < 8:
      outfile.write(str(hex_nodes[i]) + " ")
      i += 1
   outfile.write("\n")

# ============================================================
# The quads on the boundaries. 3d
# Note that the boundary id is given by the sideset id.
# ============================================================
if n_hex_cells != 0:
  k=1
  for bc_id in bc_ids:
    for bc_surface in  bc_surfaces[bc_id]:
      bc_quads = cubit.get_surface_quads(bc_surface)
      for quad_num in bc_quads:
        outfile.write(str(k))
        k += 1
        outfile.write(" " + str(bc_id) + " quad ")
        quad_nodes = cubit.get_connectivity("Quad", quad_num)
        outfile.write(str(quad_nodes[0]) + " ")
        outfile.write(str(quad_nodes[3]) + " ")
        outfile.write(str(quad_nodes[2]) + " ")
        outfile.write(str(quad_nodes[1]))
        outfile.write("\n")

# ============================================================
# The quads on the surfaces. 2d
# ============================================================
if n_hex_cells == 0:
  k = 1
  for surface_id in surface_list:
    for quad_num in quad_cell_list[surface_id]:
      outfile.write(str(k))
      k += 1
      outfile.write(" " + str(surface_id) + " quad ")
      quad_nodes = cubit.get_connectivity("Quad", quad_num)
      outfile.write(str(quad_nodes[0]) + " ")
      outfile.write(str(quad_nodes[3]) + " ")
      outfile.write(str(quad_nodes[2]) + " ")
      outfile.write(str(quad_nodes[1]))
      outfile.write("\n")

# ============================================================
# The edges on the curves. 2d
# Boundary id = sideset_id
# ============================================================
if n_hex_cells == 0:
  k=1
  for bc_id in bc_ids:
    for edge_num in bc_edges[bc_id]:
      outfile.write(str(k))
      k += 1
      outfile.write(" " + str(bc_id) + " line ")
      edge_nodes = cubit.get_connectivity("Edge", edge_num)
      outfile.write(str(edge_nodes[0]) + " ")
      outfile.write(str(edge_nodes[1]))
      outfile.write("\n")

outfile.close()

cubit.cmd("body all reflect x")
cubit.cmd("body all rotate 90 about y")

print str(n_nodes) + " nodes\n"
print str(n_hex_cells) + " hexes\n"
print str(n_quad_cells) + " quads\n"
print str(n_bc_quads) + " face_quads\n"
print str(n_bc_edges) + " edges\n"
```

### A note on the (non-) output of material ID's
For the first and third versions of this script, the material IDs are not output (they are always set by default to zero).
This is because there appears to be a deficiency/bug in the implementation of `cubit.get_block_hexes(block_id)` (in `Cubit v13.2`, at least). 
In theory one should be able to collect the cells in the following manner, assuming that the `block_id` represents the material id (this mirrors what is done already for `sideset_id`s and boundary ids):

```python
# ============================================================
# Collect all the hex
# ============================================================
hex_cell_list = {}
n_hex_cells = 0

block_ids = cubit.get_block_id_list()
for block_id in block_ids:
    hex_cell_list[block_id] = cubit.get_block_hexes(block_id)
    n_hex_cells += len(hex_cell_list[block_id])
```

and one could then write out the elements to file with a set of commands along the lines of

```python
# ============================================================
# The hex list. 3d
# ============================================================
k = 1
for block_id in block_ids:
    for hex_num in hex_cell_list[block_id]:
        print str(k) + " " + str(block_id) + "  hex  "
        outfile.write(str(k) + " " + str(block_id) + "  hex  ")
        k += 1
        hex_nodes = cubit.get_connectivity("Hex", hex_num)
        i = 0
        while i < 8:
            outfile.write(str(hex_nodes[i]) + " ")
            i += 1
        outfile.write("\n")
```

However, there `cubit.get_block_hexes(block_id)` does not return anything other than an empty set when these sets of commands were last tested (`Cubit v13.2`).
By contrast the script verified to work with `Cubit v15.2` includes this logic as the bug has been corrected in later versions of the software. 

Another version of the above script is available that produces a VTK file. You can find it here:

https://gist.github.com/luca-heltai/b127a69b86b4b140c87a01110053f751

You can add a shortcut in a Custom Toolbar to load the script into memory. Then when you are
ready to export, you can type the following command in the script tab:

	saveVTK("filename.vtk")