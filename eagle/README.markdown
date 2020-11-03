
# Dependencies
See [here](../deps.markdown)


# Building


    git clone https://gitlab.cosma.dur.ac.uk/swift/swiftsim.git
    pushd swiftsim
    git checkout 947ec89e8f9e1d8dbf0f870939bd361c327a470a
    ./autogen.sh
    ./configure --with-subgrid=EAGLE --with-hydro=sphenix --with-kernel=wendland-C2 --enable-ipo --with-parmetis --with-tbbmalloc  # Can substitute --with-tbbmalloc with --with-tcmalloc or --with-jemalloc
    make -j 32 

# Downloading data


    pushd examples/EAGLE_low_z/EAGLE_25
    ./getIC.sh
    ../EAGLE_ICs/getEagleCoolingTable.sh
    ../EAGLE_ICs/getEagleYieldTable.sh
    popd


# Running Single Node
Strong scaling can be tested by increasing `NUM_THREADS` (`-n` sets the number of timesteps to run for):


    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=<NUM_THREADS> -v 1 -n 2000 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd

# Weak scaling test
Weak scaling can be tested by increasing the problem size with the number of threads as shown below:


    # Scale 1
    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=1 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:8 \
        -P Gravity:mesh_side_length:32 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd

    # Scale 2
    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=8 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:16 \
        -P Gravity:mesh_side_length:64 \
        -P InitialConditions:replicate:2 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd

    # Scale 3
    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=27 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:24 \
        -P Gravity:mesh_side_length:96 \
        -P InitialConditions:replicate:3 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd

    # Scale 4
    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=64 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:32 \
        -P Gravity:mesh_side_length:128 \
        -P InitialConditions:replicate:4 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd

    # Scale 5
    pushd examples/EAGLE_low_z/EAGLE_25
    ../../swift --eagle --cosmology --threads=125 -v 1 -n 2000 \
        -P Scheduler:max_top_level_cells:40 \
        -P Gravity:mesh_side_length:160 \
        -P InitialConditions:replicate:5 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd


# Running Multi Node
Strong scaling can be tested by increasing `NUM_MPI_PROCESSES` and `THREADS_PER_NODE` (`-n` sets the number of timesteps to run for):


    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np <NUM_MPI_PROCESSES> ../../swift_mpi --eagle --cosmology --threads=<THREADS_PER_NODE> -v 1 -n 2000 \
        -P Restarts:enable:0 \
        -P Snapshots:scale_factor_first: 10000000000000.0 \
        -P Snapshots:time_first:10000000000000.0 \
        -P Snapshots:delta_time:10000000000000.0 \
        eagle_25.yml
    popd


