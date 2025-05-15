# Debugging an Application

Debugging is an essential process in software development aimed at identifying and resolving errors, glitches, or unexpected behaviors in a program's code. It involves systematically investigating issues to pinpoint their root causes and rectify them, ensuring that the software operates as intended. Debugging can help you to identify various issues such as:-

1. **Runtime errors**: These are errors that occur while the program is running. They are often caused by invalid input, incorrect usage of library functions, or memory corruption.

2. **Memory leaks**: These occur when a program allocates memory but fails to release it, leading to a gradual depletion of system resources.

3. **Deadlocks**: These occur when two or more processes are unable to proceed because each is waiting for the other to release a resource.

4. **Infinite loops**: These occur when a program gets stuck in a loop and fails to exit.

5. **Concurrency issues**: These occur when multiple threads or processes access shared resources in an uncoordinated manner, leading to race conditions and data corruption.

There are many tools available in the market that can help you to debug your applications. Two of the most common and widely used commercial debuggers are:-

1. **TotalView** ([https://totalview.io/](https://totalview.io/)): TotalView is a powerful and intuitive parallel debugger for C, C++, and Fortran applications. It provides a comprehensive set of debugging tools for serial and parallel applications, including memory debugging, reverse debugging, and memory leak detection.

2. **Linaro DDT** ([https://www.linaroforge.com/linaroDdt](https://www.linaroforge.com/linaroDdt)): Linaro DDT (fomerly Allinea DDT) is a parallel debugger for C, C++, and Fortran applications. It provides a range of debugging tools, including memory debugging, reverse debugging, and memory leak detection.

Both these tools can help you to fix issues in both the serial program and parallel program. However, we do not have license to run these tools on our cluster and therefore, you will need to use open source tools like GDB to debug your applications.
