# MISISSAE
Marine Ice Sheet Idealised Simulations for various Stokes Aproximations (Stokes, Blatter-Pattyn, SSA) in Elmer/Ice

# Setup
elmerf90 GeometryFunctions.F90 -o GeometryFunctions.so


## Mesh

The mesh is provided. If you wish to modify the mesh you can edit the geo file and re run the following:

gmsh mesh.geo -1 -2
ElmerGrid 14 2 mesh.msh 

The provided mesh is not yet partitioned for parallel. You can use ElmerGrid:
ElmerGrid 2 2 mesh -partition 1 3 0 2
(this creates 3 partitions, to run on a laptop; you may want more)

The mesh is a 2D mesh of triangular elements. For the HO and Stokes cases it will be extruded vertically.


# Running

This .sif runs the model forward using the SSA solver:
SSA.sif

This .sif takes the outputs from the SSA solver and remeshes onto the extruded 3D mesh:
remesh_to_3D.sif
This means that the HO and Stokes solvers can use the SSA solution as an initial guess for velocity.

It can also be a starting point for using optimised parameters from an SSA inversion (via vertical extrusion) in an HO or Stokes simulation. 

This runs the model forward using the HO (Blatter-Pattyn) solver:
HO.sif

This runs the model forward using the Stokes solver:
Stokes.sif


# Numerical choices: linear solve and preconditioner

The above examples use iterative solvers for SSA and HO and direct solve (MUMPS) for the Stokes.

In particular, the HO solver uses the block preconditioner, which should in principle also be a more efficient option for the Stokes solver (especially for large problems where MUMPS does not scale well), but it is difficult to get convergence.


*** Stokes vec or legacy? try block precond?

