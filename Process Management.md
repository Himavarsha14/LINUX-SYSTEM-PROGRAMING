## 1.Implement a simple fork() program to demonstrate parent and child process execution.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
        int pid;
        pid=fork();
        if(0==pid)
        {
                int i=0;
                for(i=0;i<5;i++)
                {
                        printf("This is child process\n");
                        sleep(1);
                }
        }
        else
        {
                int i=0;
                for(i=0;i<5;i++)
                {
                        printf("This is parent process\n");
                        sleep(1);
                }
        }
}
```
## 1.Explain the concept of process creation in operating systems.
- In an Operating System (OS), a process is simply a program in execution. Process creation happens when a new process is made by another process (called the parent process). The new process created is the child process.
- The very first process is init.the PID of init process is 1.
- Parent process strats from main() and child process starts from where the fork() gets invokes.
- fork() invokes two times once in parent process and once in child process.
- fork() returns 1 in parent process and 0 in child process.
## 2.Differentiate between the fork() and exec() system calls
### fork():
- fork() is a system call used to create a new child process.
- It is used to allow multitasking by running anew process alonside the parent.
- child process copies the memory segments of parent process and both child and parent runs independently.
- fork() returns twice,returns 0 in the child process and the child's PID in the parent process.
### exec():
- exec() family of calls are used to run a new program in a process.
- USed to replace the current process with a different program.
- It overwrites the process's memory with the new program.
- exec does not return on success; returns -1 only if there is an error.
## 3.What is the purpose of the wait() system call in process management?
The wait() system call in process management is used by a parent process to pause its execution untill child process terminates,Its main purposes are:
- To allow parent process to synchronize with the termination of child processes.
- To retrive the exit status of the terminated child.
- To prevent zombie process, which occur if a child terminates but the parent does not collect its exit status.
## 4.Describe the role of the exec() family of functions in process management.
The exec() family of functions in process management is used to replace the current process image with a new program.
- It allows a new process to run a different program without creating new process.
## 5.Write a C program to illustrate the use of the execvp() function.
```c
#include<stdio.h>
#include<sys/types.h>
void main()
{
   printf("This is an example of execvp\n");
   char *args={"ls","-l",NULL};
   execvp("ls",args);
   printf("This print never appears\n");
}
```
