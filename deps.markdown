
# Compilers
 - Intel Compiler 2017 or newer
 - GCC 8.1 or newer
 - clang 9.x or newer

# MPI library
MPI that implements 3.1 standard (See [here](https://gitlab.cosma.dur.ac.uk/swift/swiftsim/-/blob/master/README-mpi-hints.md) for more info).
We have had some success with:
 - Intel MPI 2018
 - Intel MPI 2020 with or without UCX v1.8.1 (sometimes problematic)
 - OpenMPI 4.x

# Dependencies
 - Parallel HDF5 v1.10.2 or newer
 - GSL v2.x
 - FFTW v3.3.x (Threaded-version recommended, MPI-version not used)
 - Parmetis v4.0.3 (only for runs over MPI)
