
# Dependencies
See [here](../deps.markdown)


# Building


    git clone https://gitlab.cosma.dur.ac.uk/swift/swiftsim.git
    pushd swiftsim
    git checkout 5f3649f7f92266c41d381ee9c8a3009a08d8da8a
    ./autogen.sh
    ./configure --enable-ipo --with-parmetis --with-tbbmalloc  # Can substitute --with-tbbmalloc with --with-tcmalloc or --with-jemalloc

    make -j 32 

# Downloading data


    pushd swiftsim/examples/PMillennium/PMillennium-768
    ./getIC.sh
    popd


# Running Single Node
Strong scaling can be tested by increasing `NUM_THREADS` (`-n` sets the number of timesteps to run for):


    pushd swiftsim/examples/PMillennium/PMillennium-768
    ../../swift --pin --cosmology --self-gravity -v 1 --threads=<NUM_THREADS> -n 10 \
        -P Restarts:enable:0 \
        p-mill-768.yml
    popd


# Running Multi Node
Strong scaling can be tested by increasing `NUM_MPI_PROCESSES` and `THREADS_PER_NODE` (`-n` sets the number of timesteps to run for):


    pushd swiftsim/examples/PMillennium/PMillennium-768
    mpirun -np <NUM_MPI_PROCESSES> ../../swift_mpi --pin --cosmology --self-gravity -v 1 --threads=<THREADS_PER_NODE> -n 10 \
        -P Restarts:enable:0 \
        p-mill-768.yml
    popd

