# Voronoi-vector
LAMMPS compute (based on lammps compute voronoi/atom) to obtain the per-atom vector from atom to centroid of Voronoi cell.

## INSTALL
Put the *.cpp *.h into directory lammps/src/, and compile the LAMMPS with what you need for compiling compute voronoi/atom 
(i.e. compile LAMMPS with voro++ libraray). 
See https://lammps.sandia.gov/doc/compute_voronoi_atom.html for the help on compiling.

How to link with voro++ in lammps?
E.g. Change the following lines in LAMMPS MAKE/Makefile.mpi

```
FFT_INC =-I/path/to/voro/head/voro++   # directory where voro++.h locates
FFT_LIB = -L/path/voro/lib/ -lvoro++   # directory where libvoro++.a locates
```


## USAGE
See example directory for an example lammps script.

```
comm_modify cutoff 10.0
compute v1 all voro_vec/atom
dump    d1 all custom 1 dump.voro id type x y z c_v1[1] c_v1[2] c_v1[3] c_v1[4]
run  0
```

c_v1[1-3] is the per-atom vector from one atom to the corresponding centroid of Voronoi cell.
c_v1[4] is the the length of the vector.

BTW, c_v1[5] is the Voronoi volume, and c_v1[6] is the Voronoi coordination number.


In my experience, it's better to set a large cutoff to handle ghost atoms rightly for a triclinc box with a
large tilt factor. 
