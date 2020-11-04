# Performance tuning via compilation options

Our configuration script makes educated guesses for the compiler flags
to be used. More can be added via the `CFLAGS` keyword. A clean-sheet
can be obtained by using the `--disable-optimization` parameter.

Some configuration options can be used to tune the code performance:

 - `--enable-ipo`: Activate the compiler's inter-procedural optimizer
 - `--with-tbbmalloc`: Links-in TBB's malloc library in lieu of glibc's (if found in the $PATH)
 - `--with-tcmalloc`: Links-in tcmalloc library in lieu of glibc's (if found in the $PATH)
 - `--with-jemalloc`: Links-in the jemalloc library in lieu of glibc's (if found in the $PATH)

The malloc libraries usually provide better performance over the system's one. SWIFT
does a lot of small allocations from multiple threads.

Note that when compiling on a machine that support AVX-512 and the Intel compiler, the additional flag
`-qopt-zmm-usage=high` is added to the list of flags for the file `src/runner_doiact_grav.c` which
contains all the heavy math operations related to the gravity calculation. From our experience,
the rest of the code does *not* benefit from this flag.

# Performance tuning at runtime

If the code was compiled with libNUMA then the internal mechanism for thread pinning can
be used to pin the threads to their cores. This is activated by adding `--pin` to the
runtime parameters.

Note that with more recent linux kernel versions the benefit from this flag has become
quite marginal. 

