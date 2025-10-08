## 1.Why we are using pthread_create instead of clone() for creating threads?
- The arguments of clone() are complex because its a low level system call that requires you to explicitly provide things like the function pointer,child stack,flags, and optional TLS/pid/cgroup info.
- Thats why we usually use pthread_create(), which hides this complexity and provides a simpler,standardized interface.

## 2.Why a stack grows?
A stack grows because each function call needs its own space to keep track of execution.
- When a function is called, the system pushes a stack frame onto the stack, storing local variables,return address, and function parameters.
- When the function returns, its stack frame is popped, and control goes back to the caller.

## 3.Can you fetch the thread entry point return value in your main thread?
Yes,In POSIX threads,you can fetch the return value of a thread in the main thread using pthread_join().
- The thread's entry function returns a void*.
- When you call pthread_join(thread_id, &retval);,the pointer retval will hold the return value.
  
- Example:
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void* thread_func(void* arg) {
    int *result = malloc(sizeof(int));  
    *result = 10 + 20;                 
    return result;                     
}

int main() {
    pthread_t tid;
    void *retval;

    pthread_create(&tid, NULL, thread_func, NULL);
    pthread_join(tid, &retval);

    printf("Thread returned: %d\n", *(int*)retval);

    free(retval); // free the allocated memory
    return 0;
}
```
## 4.What segments are shared by multiple threads within a process?
- 1.Code/Text Segment:Instructions of the program.
- 2.Data Segment(global & statc variables).
- 3.Heap Segment-dynamically allocated memory(malloc).
- 4.File descriptors and other OS resource.

## 5.what happens when main function is invoked?
- When a C program starts,the OS loader loads it into memory,stes up the stack, heap  and data segments, and passes command-line arguments and environment variables.The C runtime(_start) initializes the program and then calls main().
- After main() finishes,its return value is sent to the OS as the programs's exit status,and cleanup is performed.

## 6.What happens when cpu stops executing.
- When the CPU stops executing instructions, it either enters a halt/idle state waiting for interrupts (program or system idle) or completely stops on shutdown, while the OS handles cleanup of processes and resources.

## 7.During the context switch, which instruction will be usedto copy the contents of cpu register to your context area of PCB?
- During a context switch, the CPU registers are saved to the PCB using PUSH instructions, and restored later using POP instructions.

## 8.How do you create separate process?
- A new process is created using the fork() system call, which duplicates the parent process; the child process often uses exec() to run a different program.

## 9.How do server create seperate process?
- A server typically uses the fork() system call after accepting a client connection. The child process handles the client request, while the parent process continues listening for new connections.

## 10.Advantage of Thread over process
- Lightweight:Threads share the same address space, so less overhead than processes.
- Faster communication:Threads can easily share data without IPC.
- Less resource usage:Threads do not require separate memory allocation like processes.
- Better performance:Thread creation, switching, and termination are faster than processes.
- Parallelism:Multiple threads can perform tasks simultaneously, improving CPU utilization.

## 11.Advantage of process over thread
- Isolation:Each process has its own address space, so a crash is one process doesn't affect others.
- Security:Processses can't easily corrupt each other's data.
- Stabilty:Bugs in one process dont bring down the whole application.
- Better fault tolerance:Easier to manage failures in separate processes.\

## 12.How to overcome synchronization issues with global variables when multiple threads access them?
- Use synchronization mechanisms:
  - Mutex locks- to allow only one thread at a time.
  - Semaphores- for controlling access count.
  - Spinlocks/Condition variables- for more advanced synchronization.
  - Atomic operations-for simple shared data.
- Protect the critical section properly to avoid race conditions.






