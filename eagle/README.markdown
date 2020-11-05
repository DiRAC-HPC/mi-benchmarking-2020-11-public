
# Dependencies
See [here](../deps.markdown)


# Building


    git clone https://gitlab.cosma.dur.ac.uk/swift/swiftsim.git
    pushd swiftsim
    git checkout 5f3649f7f92266c41d381ee9c8a3009a08d8da8a
    ./autogen.sh
    # Can substitute --with-tbbmalloc with --with-tcmalloc or --with-jemalloc
    ./configure --with-subgrid=EAGLE --with-hydro=sphenix --with-kernel=wendland-C2 --enable-ipo --with-parmetis --with-tbbmalloc
    make -j

# Downloading Data
Note that the input is 3.6GB in size.


    pushd examples/EAGLE_low_z/EAGLE_25
    ./getIC.sh
    ../../EAGLE_ICs/getEagleCoolingTable.sh
    ../../EAGLE_ICs/getEagleYieldTable.sh
    popd


# Running Single Node
Strong scaling can be tested by increasing `NUM_THREADS` (`-n` sets the number of timesteps to run for):


    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=<NUM_THREADS> -v 1 -n 2000 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd


# Strong Scaling Multi Node
Strong scaling can be tested by increasing `NUM_MPI_PROCESSES` and `THREADS_PER_NODE` (`-n` sets the number of timesteps to run for):


    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np <NUM_MPI_PROCESSES> ../../swift_mpi --eagle --cosmology --threads=<THREADS_PER_NODE> -v 1 -n 2000 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd


# Weak Scaling Test
Weak scaling can be tested by increasing the problem size with the number of threads.
We increase the number of threads cubically as the replicate parameter increases the problem size on all 3 axes.
Feel free to change the `--threads` argument to suit the optimal number of threads per node for your system.

This is shown below:



    # Scale 1
     pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=28 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:8 \
        -P Gravity:mesh_side_length:32 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
     popd


    # Scale 2
    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np 8 ../../swift_mpi --eagle --cosmology --threads=28 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:16 \
        -P Gravity:mesh_side_length:64 \
        -P InitialConditions:replicate:2 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd

    # Scale 3
    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np 27 ../../swift_mpi --eagle --cosmology --threads=28 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:24 \
        -P Gravity:mesh_side_length:96 \
        -P InitialConditions:replicate:3 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd

    # Scale 4
    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np 64 ../../swift_mpi --eagle --cosmology --threads=28 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:32 \
        -P Gravity:mesh_side_length:128 \
        -P InitialConditions:replicate:4 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd

    # Scale 5
    pushd examples/EAGLE_low_z/EAGLE_25
    # May require MPI
    mpirun -np 125 ../../swift_mpi --eagle --cosmology --threads=28 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:40 \
        -P Gravity:mesh_side_length:160 \
        -P InitialConditions:replicate:5 \
        -P Restarts:enable:0 \
        -P Snapshots:output_list_on:1 \
        -P Snapshots:output_list:../../EAGLE_ICs/EAGLE_25/output_list.txt \
        eagle_25.yml
    popd

