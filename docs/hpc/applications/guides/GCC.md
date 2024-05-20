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

```bash
module load tools/prod

# Then search for all GCC modules by
module avail -i GCC
```

The above command will give you output like below:

```bash
------------------------------ /sw-eb/modules/all ------------------------------
GCC/8.3.0   GCC/11.2.0  GCC/13.2.0      GCCcore/10.3.0  GCCcore/12.3.0  
GCC/9.3.0   GCC/11.3.0  GCCcore/8.3.0   GCCcore/11.2.0  GCCcore/13.2.0  
GCC/10.2.0  GCC/12.2.0  GCCcore/9.3.0   GCCcore/11.3.0  gcccuda/2020a   
GCC/10.3.0  GCC/12.3.0  GCCcore/10.2.0  GCCcore/12.2.0  

----------------------- /apps/modules/4.7.1/modulefiles ------------------------
gcc-xml/2013-01-04  gcc/4.9.1                      gcc/6.2.0  gcc/10.2.0  
gcc/4.7.2           gcc/5.4.0(default)             gcc/8.2.0  gcc/11.2.0  
gcc/4.7.4           gcc/5.4.0-disable-linux-futex  gcc/9.3.0
```

For performance reasons, we recommend using the modules under the `/sw-eb/modules/all` directory. 

Someone may decide to use modules like `GCCcore/12.3.0` i.e. the one which has the word core in it. Let us go ahead and compile our program.

```bash
# Load the module
module load GCCcore/12.3.0

# Compile the program
g++ -o HelloWorld HelloWorld.cpp
```

You will get errors like

```bash
/usr/bin/ld: /rds/easybuild/zen2/apps/software/GCCcore/12.3.0/bin/../lib/gcc/x86_64-pc-linux-gnu/12.3.0/libgcc.a(_muldi3.o): unable to initialize decompress status for section .debug_info
```

This is because the core modules do not load the `binutils` module which is required for the GCC compiler to work correctly. To see this, if you type the following

```bash
# See all loaded modules
module list

# You will see the output
Currently Loaded Modulefiles:
 1) tools/prod   2) GCCcore/12.3.0 
```

Now, let us use the correct GCC module and compile the program.

```bash
# First unload the current modules
module purge

# Then load the tools/prod and necessary GCC module
module load tools/prod
module load GCC/12.3.0

# Compile the program
g++ -o HelloWorld HelloWorld.cpp
```

This time the program will compile without any errors.

If you see the list of modules loaded, you will see the following:

```bash
Currently Loaded Modulefiles:
 1) tools/prod            3) zlib/1.2.13-GCCcore-12.3.0 <aL>    
 2) GCCcore/12.3.0 <aL>   4) binutils/2.40-GCCcore-12.3.0 <aL>
```

As you can see from the output, the `binutils` module is loaded which is required for the GCC compiler to work correctly.
