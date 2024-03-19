# What is GDB?

[GDB](https://sourceware.org/gdb/), or GNU Debugger, is a powerful command-line tool used for debugging programs written in various programming languages, primarily C and C++. It allows developers to inspect and manipulate the execution of their programs, aiding in the identification and resolution of bugs and errors. GDB provides a wide range of features for debugging, including:

1. **Setting Breakpoints**: Developers can set breakpoints at specific lines of code, function calls, or memory addresses, allowing them to pause program execution at desired points and inspect the program's state.

2. **Examining Variables**: GDB enables developers to examine the values of variables, both scalar and complex data structures, during program execution. This feature is invaluable for understanding how data is manipulated and identifying potential issues.

3. **Stepping Through Code**: Developers can step through the program's execution line by line, allowing them to trace the flow of control and understand the sequence of operations. GDB offers commands for stepping into functions, stepping over lines, and stepping out of functions.

4. **Backtracing**: When an error occurs, GDB can provide a backtrace of function calls leading up to the error, helping developers pinpoint the root cause of the issue.

5. **Memory Inspection**: GDB allows developers to inspect memory contents, including stack frames, heap allocations, and register values, aiding in the detection of memory-related errors such as buffer overflows or memory leaks.

6. **Conditional Breakpoints**: Developers can set breakpoints that trigger only when certain conditions are met, allowing for more precise debugging in complex scenarios.

7. **Core File Analysis**: GDB can analyze core dump files generated when a program crashes, providing insight into the state of the program at the time of the crash.

Again, we do not aim to teach the basics of GDB in this page and we assume that the user is familiar with the basic usage. You can keep the [GDB cheat sheet](https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf) open which will help you to learn its usage faster and it will also be useful in this tutorial.

## Debugging MPI Programs with GDB

The process of debugging MPI programs with GDB is slightly more complex than debugging serial programs due to the parallel nature of MPI applications. When debugging an MPI program, you are dealing with multiple processes running concurrently, each with its own memory space and execution context. GDB provides features to debug MPI programs, but it requires additional steps and considerations to effectively debug parallel applications.

Consider that you have an MPI program as shown below. The sample program given below is simple MPI program that performs addition and multiplication on each rank and prints the result. The first argument to the multiplication functions is the rank of the process and the second argument is the size of the communicator (this is actually the bug  which is there in the code below and our task is to use gdb on such programs to find the issue). We will create a simple  program using functions so that you can step inside while debugging. Let us assume that the file is named `mpi_debug.cpp`.
 
```c++
#include <iostream>
#include <mpi.h>

// Function for multiplication
int multiplication(int a, int b, int rank, const char* hostname) {
    std::cout << "Rank " << rank << " on host " << hostname << " performing multiplication with arguments: " << a << " and " << b << std::endl;
    int result = a * b;
    std::cout << "Result of multiplication: " << result << std::endl;
    return result;
}

int main(int argc, char** argv) {
    MPI_Init(&argc, &argv);
    
    int rank, size;
    MPI_Comm_rank(MPI_COMM_WORLD, &rank);
    MPI_Comm_size(MPI_COMM_WORLD, &size);

    int num1 = rank;
    int num2 = rank;

    // Calling multiplication function
    int product = multiplication(num1, num2, rank, hostname);
    std::cout << std::endl;

    MPI_Finalize();
    return 0;
}
```

## Steps to Debug the above MPI program using GDB

1) Since we will be debugging interactively, we need to ask for interactive session as shown below.

```bash
qsub -I -l select=2:ncpus=3:mem=20gb -l walltime=03:00:00
```

2) Once your job gets the resources allocated, we load the appropriate modules by

```bash
module load tools/prod
module load OpenMPI/4.1.4-GCC-12.2.0
```

3) Next, we compile the code with debug flag as shown below.

```bash
mpic++ -g -o mpi_debug mpi_debug.cpp
```

4) Now, we run the program using `mpirun` as shown below.

```bash 
mpirun -np 2 ./mpi_debug
```

5) On running the above, we get the following output.
    
```bash
Rank 0 on host cx1-100-11-2.cx1.hpc.ic.ac.uk performing multiplication with arguments: 0 and 0
Result of multiplication: 0

Rank 1 on host cx1-100-11-2.cx1.hpc.ic.ac.uk performing multiplication with arguments: 1 and 1
Result of multiplication: 1
```

6) From the output, we can see that the output of rank `0` is right as multiplication of rank `0` and communicator size `2` is `0`. However, the output of rank `1` is wrong as multiplication of rank `1` and communicator size `2` is `2` and not `1`. This is the bug in the code and we need to debug it using GDB. We must emphasize that the bug is trivial to fix by seeing the code but the aim of this tutorial is to show how to use GDB on MPI programs. 

To use GDB on mpi program, we need to attach GDB to the running process and then step through the code manually to find issues. If you try to attach GDB directly to your running process, you will not know at which point the process is and you may not be able to debug it properly. Therefore, we will use the small code section below which will force all processes in the communicator to sleep unless we explicitly ask it to run. 

```c++
// Insert this function definition at the top of your code after the include directives.
// Function to retrieve process ID (PID)
pid_t get_process_id() {
    return getpid();
}

// Insert this code right after the  MPI initialization routines (though not a mandatory requirement 
// to add there only). Please make a judgement based on your code.
// Retrieve process ID and hostname
pid_t pid = get_process_id();

volatile int i = 0;
while (0 == i)
{
	std::cout << "My rank = " << rank << " PID = " << pid << " running on Host = " << hostname << " in sleep " << std::endl;
	sleep(5);
}
```

We used the header file `unistd.h` which defines the function `getpid()`.

Based on the above output, we can easily figure out the process id of each rank and its host. Depending on where the process is running, you may have to ssh to that host and use GDB there.

Please note that the above logic is forcing all ranks to sleep infinitely unless we attach GDB to it and ask it to run. You can also change the above logic to a specific rank only if you are sure that this is the rank causing issues.

7) After adding the above code, we compile again and run our code. We will redirect the standard output and input to a file called `output.log` and run the process in the background as shown below.

```bash
mpirun -np 2 ./mpi_debug > output.log 2>&1 &
```
where `2>&1` is used to redirect the standard error to the standard output and `&` is used to run the process in the background.

On seeing the output.log, we will see the following output.

```bash
My rank = 0 PID = 872257 running on Host = cx1-100-11-2.cx1.hpc.ic.ac.uk in sleep 
My rank = 1 PID = 872258 running on Host = cx1-100-11-2.cx1.hpc.ic.ac.uk in sleep 
```

You will see this output continuously with a gap of 5 seconds. Now we need to attach gdb to the problematic process which is the rank 1. 

8) Next we attach GDB to the rank 1 process by using the following commands. Since we are on the same node, we do not need an additional ssh step here.

The syntax to attach GDB to a running process is as follows.
```bash
gdb -p <PID>
```

where `<PID>` is the process id of the process you want to attach GDB to. In our case, the process id of rank 1 is `872258`. Therefore, we attach GDB to the process by using the following command.

```bash
gdb -p 872258
```

We get the following output.

```bash
GNU gdb (GDB) Red Hat Enterprise Linux 8.2-16.el8
Copyright (C) 2018 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
:
:
(gdb)
```

We have not shown all the output here but you will see a lot of output on the screen. The terminal is waiting for your input. 

9) Now we need to change  the value of the variable `i` to `1` (or any other value) so that the rank can come out of the infinite while loop. We do this by using the following command.

We would first need to find out where we are currently in the call stack. This will help us to jump to the main function and change the value there. We  use  the command `where` to find out the details.

```bash
(gdb) where
```

This will give us the following output.

```bash
#0  0x000014d5c665bd98 in nanosleep () from /lib64/libc.so.6
#1  0x000014d5c665bc9e in sleep () from /lib64/libc.so.6
#2  0x00000000004013d6 in main (argc=1, argv=0x7ffe1214e358) at 5b_problem_mpi.cpp:36
```

Thus we can see that we are two levels deep in the main function. We can move up two levels by using the up command twice or we can directly jump to the main function by using the following command.

```bash
(gdb) up 2

# This will give the output
#2  0x00000000004013d6 in main (argc=1, argv=0x7ffe1214e358) at 5b_problem_mpi.cpp:36
 sleep(5);
```

Thus we are now in main function and we can change the value of i here.

10) We change the value of `i` to `1` by using the following command.

```bash
(gdb) set var i = 1

# Now we print i to see its value.
```bash
(gdb) print i
$2 = 1
```

As we can see that the value has been changed.

11) Next, we set a breakpoint at the multiplication function and run the program. We do this by using the following commands.

```bash
(gdb) break multiplication
(gdb) continue
```

12) The program at rank 1 now runs till the breakpoint which is the start of the function definition in our case. Inside this function, we can now print the values of both the arguments to see what was the issue.

```bash
(gdb) print a
$3 = 1
(gdb) print b
$4 = 1
```

And, finally we can see that the issue is with the value of `b` which is `1` and not `2`. This is the bug in the code and we can fix it now by going back and setting num2 = size. Again this is a toy example but helps demonstrate the how to find a bug in a code base using a debugger.

Note:- If for some reason, you want to detach gdb from the process, you can do so by pressing `Ctrl + c` and then typing `quit` in the terminal.

We hope that you like this tutorial and will use it to debug your MPI programs. We agree that this is not the most convenient way to find bugs, but still it is an excellent way to track issues by using the full power of GDB on any of your ranks.
