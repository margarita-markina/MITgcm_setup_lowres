This configuration compiles and runs successully on cyclone (2025-04-30)

to compile it we applied changes to packages.conf and SIZE.h (marginal changes, probably involving some removal of END-OF_LINE)
the steps to successfully compile the model on cyclone were as follows:

# copied packages.conf from 
# the high-resolution code directory to 
# the low -resolution code directory

# changed SIZE.h to 90x75, nSx = 1, nSy = 1, nPx = 4, nPy = 1

# go to build directory (/Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150)
# delete everythingin there if there is something inside: 
# !!!! make sure to be in the right directory !!!!
rm *
module load OpenMPI/4.0.3-GCC-9.3.0
#### do not: module load netCDF-Fortran/4.4.4-foss-2018b
# compile the model while still being in the buil directory
/Data/gfi/users/kih012/MITgcm_c67/MITgcm/tools/genmake2 -mods /Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/code_TEST_EXP_NA_180x150 -optfile /Data/gfi/users/kih012/MITgcm_c67/MITgcm/tools/build_options/linux_ia32_gfortran+mpi_fc_lam -rootdir /Data/gfi/users/kih012/MITgcm_c67/MITgcm/ -mpi
make depend
make
# you may check whether mitgcmuv has been produced, check for errors during each step of the compilation
# go to run directory (/Data/gfi/work/kih012/MITgcm_c67/TEST_EXP_NA_180x150/run_TEST_EXP_NA_180x150)
cd $WORK/TEST_EXP_NA_180x150/run_TEST_EXP_NA_180x150/
# run directory should contain all the relevant data (data* and eedata) files, symbolic links ot the input_binary files
# if output is directed to a specific directory, create this directory (specified in data.diagnostics, here directory diags required)
mkdir diags
# copy mitgcmuv from build directory to run directly
cp /Data/gfi/users/kih012/MITgcm_c67/SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150/mitgcmuv .
# OR:
cp $MITHOME/                         SetupsRita/TEST_EXP_NA_180x150/build_TEST_EXP_NA_180x150/mitgcmuv .
# run the model:
mpirun -np 4 ./mitgcmuv



