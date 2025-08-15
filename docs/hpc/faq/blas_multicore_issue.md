# Blas code not running on multi cores

Blas (Basic Linear Algebra Subprograms) and Lapack (Linear Algebra PACKage) are libraries that provide routines for performing linear algebra operations. The basic (or reference) **Un-optimised** versions are provided by [Netlib](https://github.com/Reference-LAPACK/lapack).

!!! warning

    Users are strongly advised Not to use the basic un-optimised versions of these libraries. Instead they should use the alternative mentioned below.

There are several **optimised** implementations of these libraries, such as:-

1. [OpenBLAS](https://github.com/OpenMathLib/OpenBLAS),
2. [FlexiBLAS](https://github.com/mpimd-csc/flexiblas),
3. [Intel MKL](https://www.intel.com/content/www/us/en/developer/tools/oneapi/onemkl.html) (Math Kernel Library), and

When running code that uses these libraries on a multi-core system, you may encounter issues where the code does not utilize multiple cores effectively.

## How to check if the code is using multiple cores

While there is no straightforward way to check if the code is actually running on multiple cores, an easy approach is to monitor the CPU usage of the process while it is running.

You can run a small example of your choice that can run on multiple cores. You can submit a job and then [SSH to the compute node](./ssh-compute-node.md). Once on the compute node, you can use commands like `top` or `htop` to monitor the CPU usage of your process. If you see that only one core is being utilized, then the code is not running on multiple cores.

## How to resolve the issue

The resolution to this issues is pretty simple and depends on the library that you are using. In general, you need to export some variables before running your code.

We provide the details below for each the above mentioned libraries.

### OpenBLAS

Please export the following environment variable before running your code:

```bash
export OPENBLAS_NUM_THREADS=Number_of_threads_you_want_to_use
export OMP_NUM_THREADS=Number_of_threads_you_want_to_use
```

### Intel MKL

The following variables should be exported before running your code:

``` bash
export MKL_NUM_THREADS=Number_of_threads_you_want_to_use
export OMP_NUM_THREADS=Number_of_threads_you_want_to_use
```

### FlexiBLAS

Since `FlexiBLAS` is a wrapper library which allows you to choose `OpenBLAS`, `Intel MKL` or other implementation as the backend, you can use the same environment variables as above depending on the backend you are using.
