# GCC Compiler Module

This page is a small guide on what type of modules to be used for compiling your programs with the GCC compiler. We are creating this page because of the way our module are built.

If you use the incorrect module, you may get strange errors. Please see the example below which illustrates the same.

Let us consider that we have the following `HelloWorld.cpp` program:

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello" << std::endl;
    return 0;
}
```

To compile the program, we need to load the GCC compilers first. To load them, we first load the `tool/prod` as shown below:

```console
[user@login ~]$ module load tools/prod

# Then search for all GCC modules by
[user@login ~]$ module spider GCC
```

The above command will give you output like below:

```text
-------------------------------------------------------------------------------------------------------------------------------    
  GCC:
-------------------------------------------------------------------------------------------------------------------------------    
    Description:
      The GNU Compiler Collection includes front ends for C, C++, Objective-C, Fortran, Java, and Ada, as well as libraries        
      for these languages (libstdc++, libgcj,...).

     Versions:
        GCC/7.3.0-2.30
        GCC/9.3.0
        GCC/10.2.0
        GCC/10.3.0
        GCC/11.2.0
        GCC/11.3.0
        GCC/12.2.0
        GCC/12.3.0
        GCC/13.2.0
        GCC/13.3.0
```

Let's use `GCC/12.3.0` for example:

```console
# First unload the current modules (if any)
[user@login ~]$ module purge

# Then load the tools/prod and necessary GCC module
[user@login ~]$ module load tools/prod
[user@login ~]$ module load GCC/12.3.0

# Compile the program
[user@login ~]$ g++ -o HelloWorld HelloWorld.cpp
```

The program should compile without any errors.

## A note about GCCcore
!!! info
	Generally, it's best if you avoid using `GCCcore` alone and instead use one of the `GCC` modules. Below you will see an explanation of why.

One may decide to use modules like `GCCcore/12.3.0` i.e. the one which has the word "core" in it. Let us go ahead and test compiling our program.

```bash
# First unload the current modules (if any)
[user@login ~]$ module purge

# Load the module
[user@login ~]$ module load GCCcore/12.3.0

# Compile the program
[user@login ~]$ g++ -o HelloWorld HelloWorld.cpp
```

You will see errors like:

```console
/usr/bin/ld: /rds/easybuild/zen2/apps/software/GCCcore/12.3.0/bin/../lib/gcc/x86_64-pc-linux-gnu/12.3.0/libgcc.a(_muldi3.o): unable to initialize decompress status for section .debug_info
```

This is because the "core" modules do not load the `binutils` module which is required for the GCC compiler to work correctly. You can see this by typing the following:

```console
# See all loaded modules
[user@login ~]$ module list

# You will see the following output, note that "binutils" is missing
Currently Loaded Modulefiles:
 1) tools/prod   2) GCCcore/12.3.0 
```

If we were to load `binutils` now, by doing:
```console
[user@login ~]$ module load binutils/2.40-GCCcore-12.3.0

[user@login ~]$ module list
Currently Loaded Modulefiles:
 1) tools/prod            3) zlib/1.2.13-GCCcore-12.3.0 <aL>    
 2) GCCcore/12.3.0 <aL>   4) binutils/2.40-GCCcore-12.3.0 <aL>
```

We can see the `binutils` module is loaded which is required for the GCC compiler to work correctly.

You can try re-compiling again and it should work successfully.
