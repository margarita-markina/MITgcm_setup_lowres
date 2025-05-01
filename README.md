# MITgcm Low-Resolution North Atlantic (180x150) Setup on Cyclone 

This configuration compiles and runs successfully on Cyclone at the University of Bergen (as of 2025-04-30).
At present, the model is run using mpirun, as SLURM is not installed on the system.

To compile it, we applied minor changes to packages.conf and SIZE.h (e.g., removal of stray END-OF_LINE markers).

## Steps to successfully compile the model on Cyclone:

- Copy packages.conf from the high-resolution code directory to the low-resolution code directory.

- Change SIZE.h to: 
nx = 90
ny = 75
nSx = 1
nSy = 1
nPx = 4
nPy = 1

- Go to the build directory:
/Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150

- Delete everything in the build directory (make sure you're in the correct path):
rm *

- Load the required module:
module load OpenMPI/4.0.3-GCC-9.3.0

Do NOT load:
netCDF-Fortran/4.4.4-foss-2018b

- Compile the model using:
/Data/gfi/users/kih012/MITgcm_c67/MITgcm/tools/genmake2 -mods /Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/code_TEST_EXP_NA_180x150 -optfile /Data/gfi/users/kih012/MITgcm_c67/MITgcm/tools/build_options/linux_ia32_gfortran+mpi_fc_lam -rootdir /Data/gfi/users/kih012/MITgcm_c67/MITgcm/ -mpi

make depend
make

- Confirm that mitgcmuv was created and compilation finished without errors.


## To run the model:

- Go to the run directory:
/Data/gfi/work/kih012/MITgcm_c67/TEST_EXP_NA_180x150/run_TEST_EXP_NA_180x150

- Ensure the run directory contains all relevant data* and eedata files, and symbolic links to the input_binary files.

- Create output directory (if specified in data.diagnostics, e.g., diags):
mkdir diags

- Copy mitgcmuv from build directory:
cp /Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150/mitgcmuv .

Alternatively:
cp $MITHOME/SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150/mitgcmuv .

- Run the model:
mpirun -np 4 ./mitgcmuv
