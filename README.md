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
# emperical radius from https://www.webelements.com
variable radius1 equal 1.55
variable radius2 equal 1.35
variable radius3 equal 1.25
variable max_edge_to_count equal 7 # >= 7
variable edge_threshold equal 0.5
variable face_threshold equal 0.001
# For trilinic box, better to set a large cutoff to handle ghost atom correctly.
variable maximum_cutoff equal 10.0 # maximum communication cutoff

comm_modify cutoff ${maximum_cutoff}
variable r atom (type==1)*${radius1}+(type==2)*${radius2}+(type==3)*${radius3}

compute voro all voro_vec/atom edge_histo ${max_edge_to_count} edge_threshold ${edge_threshold} face_threshold ${face_threshold} radius v_r
thermo_style custom c_voro[*]
#                                                 dx       dy     dz     l
#dump    d1 all custom 1 dump.voro id type x y z c_voro[1] c_voro[2] c_voro[3] c_voro[4]
dump    d1 all custom 1 dump.voro id type x y z c_voro[*]
run  0
```

c_voro[1-3] is the per-atom vector from one atom to the corresponding centroid of Voronoi cell.
c_voro[4] is the the length of the vector.
c_voro[5-8] is the number of 3-edge, 4-dege, 5-edge and 6-edge faces.
c_voro[9] is the Voronoi volume, and c_v1[10] is the Voronoi coordination number.


In my experience, it's better to set a large cutoff to handle ghost atoms rightly for a triclinc box with a
large tilt factor. 
