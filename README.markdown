# General aspects
 - SWIFT is an MPI+X code solving the equations of gravity and hydrodynamics using particle-based methods.
 - Is is written in C using gnu99 extensions and posix for the threading (*not* OpenMP).
 - The code exploits task-based parallelism and uses asynchronous MPI communications. We rely on the MPI 3.1 standard and the MPI_THREAD_MULTIPLE option.
 - We use many 1000s of small messages to help with the fine-grained tasking design of the software. This is problematic with some MPI implementations that only marginally obey the MPI 3.1 standard.
 - The gravity code relies heavily on the compiler's auto-vectorizer to produce an efficient binary.
 - The hydro code is largely bound by memory bandwidth.

# Benchmarks
 - [EAGLE](./eagle/README.markdown)
 - [PMillenium 1536](./pmillenium_1536/README.markdown)
 - [PMillenium 768](./pmillenium_768/README.markdown)

# Outputs
Please save files named `timesteps_<NUM_THREADS>.txt` as these contain information about performance.
