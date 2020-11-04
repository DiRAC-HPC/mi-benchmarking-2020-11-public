# General aspects
 - SWIFT is an MPI+X code solving the equations of gravity and hydrodynamics using particle-based methods.
 - Webpage and documentation: www.swiftsim.com
 - Code repository: https://gitlab.cosma.dur.ac.uk/swift/swiftsim
 - Licence: LGPL v3.0
 - Is is written in C using gnu99 extensions and posix for the threading (*not* OpenMP).
 - The code exploits task-based parallelism and uses asynchronous MPI communications. We rely on the MPI 3.1 standard and the MPI_THREAD_MULTIPLE option.
 - We use many 1000s of small messages to help with the fine-grained tasking design of the software. This is problematic with some MPI implementations that only marginally obey the MPI 3.1 standard.
 - The gravity code relies heavily on the compiler's auto-vectorizer to produce an efficient binary.
 - The hydro code is largely bound by memory bandwidth.

# Benchmarks

There are two separate benchmarks designed to test the performance of the different code sections.

The first test is focused on the performance of the gravity code in large-scale runs. This exercises
the math library, raw FP-performance at full-load when there is a lot of operations to perform for
every particle in the system. This test comes in different sizes as the goal is to fill the memory
of the nodes as much as possible. In decreasing size (factors of 8):

 - [PMillenium 3072](./pmillenium_3072/README.markdown)
 - [PMillenium 1536](./pmillenium_1536/README.markdown)
 - [PMillenium 768](./pmillenium_768/README.markdown)

The second test exercises mainly the hydrodynamics and additional physics modules. This is less demanding
on the raw FP performance but tests the memory bandwidth, numa effects and scalability. 

 - [EAGLE](./eagle/README.markdown)


# Initial conditions (ICs)
The ICs sizes are:

 - PMill-384   - 1.1 GB
 - PMill-768   - 8.3 GB
 - PMill-1536  - 163 GB
 - PMill-3072  - 1.3 TB

# Outputs
Please save files named `timesteps_<NUM_THREADS>.txt` as these contain information about performance.
