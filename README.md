# Voronoi-vector
LAMMPS compute (based on lammps compute voronoi/atom) to obtain the per-atom vector from atom to centroid of Voronoi cell.

## INSTALL
Put the *.cpp *.h into directory lammps/src/, and compile the LAMMPS with what you need for compiling compute voronoi/atom 
(i.e. compile LAMMPS with voro++ libraray). 
See https://lammps.sandia.gov/doc/compute_voronoi_atom.html for the help on compiling.

## USAGE
```
compute v1 all voro_vec/atom
dump    d1 all custom 1 dump.voro id type x y z c_v1[1] c_v1[2] c_v1[3]
run  0
```
c_v1[1-3] is the per-atom vector from one atom to the corresponding centroid of Voronoi cell.
