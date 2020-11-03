
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
        eagle_25.yml
    popd


# Running Multi Node
Strong scaling can be tested by increasing `NUM_MPI_PROCESSES` and `THREADS_PER_NODE` (`-n` sets the number of timesteps to run for):


    pushd examples/EAGLE_low_z/EAGLE_25
    mpirun -np <NUM_MPI_PROCESSES> ../../swift_mpi --eagle --cosmology --threads=<THREADS_PER_NODE> -v 1 -n 2000 \
        -P Restarts:enable:0 \
        eagle_25.yml
    popd

