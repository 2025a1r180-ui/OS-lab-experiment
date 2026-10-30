# Experiment 10(a): Process Creation and Synchronization Using `fork()` and `wait()`

## Aim

To write and execute a C program that creates a child process using
`fork()` and makes the parent process wait until the child process
terminates using `wait()`.

## Learning Objectives

-   Understand the concept of process creation in Linux.
-   Learn how the `fork()` system call creates a child process.
-   Differentiate between a parent process and a child process.
-   Use `getpid()` to obtain the process ID of the running process.
-   Understand how `sleep()` pauses a process for a specified time.
-   Use `wait()` to synchronize the parent process with its child.

## Software/Requirements

-   Linux operating system (or a Linux virtual machine)
-   GCC compiler
-   Text editor such as GNU Nano
-   Terminal

## Theory

A **process** is a program that is currently executing.

-   `fork()` creates a new process. The newly created process is called
    the **child process**, while the original process is called the
    **parent process**.
-   `fork()` returns `0` in the child process, the child's process ID in
    the parent process, and `-1` if process creation fails.
-   `getpid()` returns the process ID (PID) of the calling process.
-   `sleep(2)` pauses the child process for two seconds.
-   `wait(NULL)` makes the parent process wait until one of its child
    processes terminates. On success, it returns the PID of the
    terminated child; on failure, it returns `-1`.

## C Program

``` c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(void)
{
    pid_t p = fork();

    if (p < 0) {
        perror("fork failed");
        return 1;
    }
    else if (p == 0) {
        printf("Child running with PID: %d\n", getpid());
        sleep(2);
        printf("Child exiting...\n");
        exit(0);
    }
    else {
        printf("Parent waiting for child to terminate...\n");

        pid_t terminated_pid = wait(NULL);

        if (terminated_pid == -1) {
            perror("wait failed");
        }
        else {
            printf("Parent detected terminated child PID: %d\n",
                   terminated_pid);
        }
    }

    return 0;
}
```

## Algorithm

1.  Start the program.
2.  Call `fork()` to create a child process.
3.  Check the return value of `fork()`.
4.  If the return value is `0`, execute the child-process block:
    -   Display the child's PID using `getpid()`.
    -   Pause for two seconds using `sleep(2)`.
    -   Display the child termination message and exit.
5.  If the return value is positive, execute the parent-process block:
    -   Display that the parent is waiting.
    -   Call `wait(NULL)` so the parent waits for the child to
        terminate.
    -   Display the PID returned by `wait()`.
6.  End the program.

## How to Compile and Run

Save the program as `exp10a.c`, then enter these commands in the
terminal:

``` bash
gcc exp10a.c -o exp10a
./exp10a
```

## Sample Output

The process IDs will be different each time you run the program. A
typical output is:

``` text
Parent waiting for child to terminate...
Child running with PID: 13805
Child exiting...
Parent detected terminated child PID: 13805
```

The exact order of the first two messages can vary because the parent
and child are scheduled independently. However, the parent prints the
message after `wait()` only after the child has terminated.

## What We Learned

1.  We learned how to create a child process using the `fork()` system
    call.
2.  We learned that the parent and child are separate processes and have
    different process IDs.
3.  We learned to identify the child process using the return value `0`
    from `fork()`.
4.  We learned to obtain a process ID using `getpid()`.
5.  We learned that `sleep()` can pause a process for a specified
    duration.
6.  We learned that `wait()` provides process synchronization by making
    the parent wait for its child to finish.
7.  We understood that `wait()` returns the terminated child's PID on
    success and `-1` if an error occurs.

## Result

The C program was successfully used to demonstrate child-process
creation using `fork()` and parent-child synchronization using `wait()`.
The parent process waits until the child process terminates before
continuing.
