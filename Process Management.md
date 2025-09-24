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
   char *args[]={"ls","-l",NULL};
   execvp("ls",args);
   printf("This print never appears\n");
}
```
## 6.How does the vfork() system call differ from frok()
### fork()
- Creates a new child process that is an exact copy of the parent process.Both parent and child run independently with seperates address spaces.
### vfork()
- Similar to fork(), but the child shares the parents's address space uniti; it calls exec() or _exit().It is more efficient but dangerous if the child modfifies variables before exec() or _exit().

## 7.What is the significance of getpid() and getppid() system calls
### getpid()
- returns process ID(PID) of the current process.
- getppid() returns parent process ID(PPID).

## 8.Explain the concept of process termination in UNIX-like operating systems.
A process can terminate in several ways:
- Normal termination->calls exit() or return from main().
- Abnormal termination->caused by signals like SIGKILL,SIGSEGV
- killed by parent->parent process can terminate a child.
- Orphaned process termination->reparented to init/systemd before ending.
  When terminated, the process enters a zombie states until the parent collects its exit status using wait() or waitpid().

## 9.Write a C program to create a child process using fork() and print its PID.
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        perror("fork failed");
        return 1;
    }
    else if (pid == 0) {
        // Child process
        printf("Child process created with PID: %d\n", getpid());
    }
    else {
        // Parent process
        printf("Parent process PID: %d, Child PID: %d\n", getpid(), pid);
    }

    return 0;
}
```
## 10.Describe the process hierarchy in UNIX-like operating system.
- All processes form a tree structure.
- Root is init (PID 1) or systemd.
- Every process has a parent,except init.
- If a parent dies,the child is reparented to init/systemd.
- Hierarchy can be viewed using ps-ef or pstree.

## 11.What is the purpose of the exit() function in C programming.
- Used to terminate a process.
- Performs cleanup:
     - Closes open files.
     - Flushes buffers.
     - Notifies parent about exit status.
- Syntax: void exit(int status);

## 12.Explain how the execve() system call works and provide a code example.
- Replaces the current process image with a new proram.
- Does not create a new process;PID remains the same.
- Syntax:int execve(const char *pathname,char *const argv[], char *const envp[]);
```c
#include <stdio.h>
#include <unistd.h>
int main() {
    char *args[] = {"/bin/ls", "-l", NULL};
    execve("/bin/ls", args, NULL);
    perror("execve failed");
    return 1;
}
```
## 13.Discuss the role of fork() system call in implementing multitasking.
- Allows multiple processes to exist simultaneously.
- Parent can continue running while child executes another task.
- Basis for parallel execution in UNIX-like Os.
- Often used with exec() to run new programs concurrently.

## 14.Write a C program to create multiple child processes using fork() and display their PIDs.
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
int main() {
    int i;
    for (i = 0; i < 3; i++) {
        pid_t pid = fork();
        if (pid == 0)
       {
            printf("Child %d created with PID: %d\n", i+1, getpid());
            return 0;
        }
    }
    printf("Parent PID: %d\n", getpid());
    return 0;
}
```
## 15.How does the exec() system call replace the current process image with a new one?
- Loads a new program into the current process memory.
- Old program is replaced,new code starts executing from main().
- PID remains unchanged.
- Example:
      - fork() creates a child.
      - child calls exec()->replaces itself with a new program.

## 16.Explain the process of process scheduling in operating system.
- Determines which process runs at a given time.
- Ensures efficient CPU utilization,fairness,responsiveness.
### Types of scheduling
- 1.FCFS(First Come First Serve)-Simple queue.
- 2.SJF(Shortest Job First)-optimal but hard to predict.
- 3.Round Robin-each process gets a fixed time quantum.
- 4.priority Scheduling- Processes with higher priority run first.
- 5.Multilevel Queue-seperate processes into categories(interactive vs batch).

## 17.Describe the role of clone() system call in process management.
- clone() is a Linux-specific system call used to create new processes/threads.
- Unlike fork(), which always creates a seperate process, clone() allows fine-grained control over which resources (memory,file descriptors,signal handlers) are shared between parent and child.
- Basis for Linux threads implementation(pthread_create() internally uses clone()).

## 18.Write a C program to create a zombie process.
- A zombie process occurs when a child terminates but the parent doesn't collect its status via wait().
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        printf("Child process (PID: %d) exiting...\n", getpid());
        exit(0); 
    }
    else {
        printf("Parent process (PID: %d) sleeping...\n", getpid());
        sleep(10); 
    }
    return 0;
}
```
## 19.Discuss the significance of setuid() and setgid() system calls in process man.
### setuid(uid_t uid)
- Sets user ID of the process.
### setgid(gid_t gid)
- sets group ID.

## 20.Explain the concept of Groups and their significance in UNIX like OS.
- A process group is a collection of related processes.
- Identified by a PGI (Process Group ID).
- Used for job control in shells (foreground/background processes).
- Signals like CTRL+C are delivered to the whole process group.
- Example:ps -o pid, pgid, comm shows PGID

## 21.Write a C program using waitpid() for process synchronization.
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        printf("Child process (PID: %d) running...\n", getpid());
        sleep(2);
        printf("Child process exiting.\n");
        exit(0);
    }
    else {
        int status;
        waitpid(pid, &status, 0); 
        printf("Parent collected child (PID: %d) with status %d\n", pid, WEXITSTATUS(status));
    }
    return 0;
}
```

## 22.Discuss the role of the execle() function in the exec() family of calls.
- part of exec() family.
- Load a new program into the current process.
- Unlike execl(),it allows apecyfying a custom environment.

## 23.Discuss the purpose of nice() in process debugging.
- nice(int inc) adjusts the priority (niceness) of process.
- Range: -20(highest priority) to +19(lowest).
- Default:0.
- Lower value:more CPU share.

## 24.Write a C program to create a Demon process.
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>

int main() {
    pid_t pid = fork();
    if (pid < 0) exit(1);
    if (pid > 0) exit(0); // Parent exits

    // Child becomes session leader
    setsid();
    chdir("/");           
    umask(0);             
    close(STDIN_FILENO);
    close(STDOUT_FILENO);
    close(STDERR_FILENO);

    while (1) {
        sleep(10);
    }
    return 0;
}
```

## 25.Difference between fork() and clone()
- fork()->creates a new process with seperate address space.
- clone()->allows fine-grained control over resource sharing(memory,file descriptors,signal handlers).
- clone() is more flexible ->basis for linux threads(pthread).

## 26.Write a C program using system() to execute shell commands
```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    printf("Listing files using system()...\n");
    int ret = system("ls -l");
    if (ret == -1) {
        perror("system");
    }
    return 0;
}
```
## 27.Explain Process states in UNIX-like operating systems
A process goes through different states:
- R (Running/Runnable)->executing or ready for CPU.
- S (Sleeping-interruptible)->waiting for event(I/O).
- D (Uninterruptible sleep)->waiting on kernal(e.g.,disk I/O).
- T (Stopped/Traced)->paused by signal(e.g., SIGSTOP,debugging).
- Z (Zombie)->finished execution, parent hasn't colleceted status.
- X(Dead)-> internal state,not usually seen.
These states are visible with ps-l under the STAT column.

## 28.What is the purpose of chroot() system call
- changes the root directory (/) of the calling process.
- Provides file system isolation-> Process can only see files inside the new root.
- Common in jails,containers and chroot environments.
- Example:
```c
#include <unistd.h>
#include <stdio.h>

int main() {
    if (chroot("/tmp") == 0) {
        printf("Root directory changed to /tmp\n");
        chdir("/");
        system("ls");
    } else {
        perror("chroot failed");
    }
    return 0;
}
```
## 29.What is the role of execv()
- part of exec() family.
- Loads a new program into the calling process.
- Unlike exeve(), it doesn't need environment explicitly ->inherits current environment.

## 30.What is the significance of process identifiers
- Unique integer assigned to each process.
- Used to manage processes:kill(pid,SIGTERM),waitpid(pid,...).
- PID 1=init/systemd.
- special PIDs:
      - 0->scheduler
      - 1->init/systemd
      - -1->all processes

## 31.Discuss the concept of orphan processes and how they are handled in UNIX-like operating systems.
### Orphan Process:
- An orphan process is a process whose parent has terminated while the child is still running.
- In UNIX-like systems,when a processs becomes orphaned, it is adopted by init (PID 1) or systemd (in modern linux).
- This ensures the orphan is waited on properly to prevent zombies.
### Handling:
- Kernal automatically reassigns orphan processes to init.
- init periodically calls wait() to clean them up.

## 32.Write a program in C to demostrate process synchronization using semaphores.
```c
#include <stdio.h>
#include <pthread.h>
#include <semaphore.h>
#include <unistd.h>
sem_t sem;
void* task(void* arg) {
    sem_wait(&sem);   
    printf("Thread %ld entering critical section\n", (long)arg);
    sleep(1);
    printf("Thread %ld leaving critical section\n", (long)arg);
    sem_post(&sem);   
    return NULL;
}
int main() {
    pthread_t t1, t2;
    sem_init(&sem, 0, 1); 
    pthread_create(&t1, NULL, task, (void*)1);
    pthread_create(&t2, NULL, task, (void*)2);
    pthread_join(t1, NULL);
    pthread_join(t2, NULL);
    sem_destroy(&sem);
    return 0;
}
```
## 33.Describe the concept of process priority and how it is managed in operating system.
- Each process in an OS has a priority that determines scheduling order.
- Higher-priority processes are scheduled before lower-priority ones.
- Linux uses nice values(-20 to +19):
     - Lower nice =higher priority
- Managed by scheduler algorithm like CFS( Completely Fair Scheduler).

## 34.Explain the purpose of the fork() system call in creating copy-on-write(COW) processes.
- fork() creates a new process by duplicating the parent’s memory.
- Instead of immediately copying memory, modern OSes use
- ### Copy-on-Write (COW):
- Both processes share the same memory pages as read-only.
- When one process writes, a separate copy of that page is made.
- This makes fork() efficient.

## 35.Discuss the role of execvp() function in searching for executable files.
- execvp() replaces the current process with a new executable.
- Unlike execv(), it searches the PATH environment variable to find the executable.
- Example: execvp("ls", args) will find /bin/ls.

## 36.Write a C program to demonstrate the use of the execvpe() function.
```c
#define _GNU_SOURCE
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
int main() {
    char *args[] = {"ls", "-l", NULL};
    char *envp[] = {"MYVAR=HelloWorld", NULL};
    printf("Before execvpe\n");
    if (execvpe("ls", args, envp) == -1) {
        perror("execvpe failed");
    }
    printf("This will not print if execvpe succeeds\n");
    return 0;
}
```
## 37.Explain the concept of process context switching and its impact on system performance.
- Context switch = switching CPU from one process/thread to another.
- Involves saving the state (registers, program counter, stack pointer) of one process and loading another.
- ### Impact:
- Necessary for multitasking.
- But frequent switches increase overhead (cache misses, scheduler latency).

## 38.Write a C program to create a process group and change its process group ID (PGID).
```c
B#include <stdio.h>
#include <unistd.h>
int main() {
    pid_t pid = fork();
    if (pid == 0) {
        printf("Child PID: %d, PGID before: %d\n", getpid(), getpgid(0));
        setpgid(0, 0);
        printf("Child PID: %d, PGID after: %d\n", getpid(), getpgid(0));
    } else {
        printf("Parent PID: %d, PGID: %d\n", getpid(), getpgid(0));
    }
    return 0;
}
```
## 39.Explain the difference between process creation using fork() and pthread_create().
| Aspect       | fork()                                 | pthread\_create()                   |
| ------------ | -------------------------------------- | ----------------------------------- |
| Memory Space | Creates new process (separate memory). | Creates new thread (shared memory). |
| IPC          | Needs pipes, shared memory, sockets.   | Directly share variables.           |
| Scheduling   | Independent processes.                 | Lighter threads in same process.    |
| Overhead     | Higher (process creation).             | Lower (thread creation).            |

## 40.Discuss the significance of the execvp() function in searching for executables in the PATH environment variable.
- execvp() looks for the program in the PATH environment variable.
- If you run execvp("ls", args), it checks /bin/ls, /usr/bin/ls, etc.
- Saves programmer effort — no need to provide full path.

## 41.Write a program in C to demonstrate inter-process communication (IPC) using shared memory.
```c
#include <stdio.h>
#include <stdlib.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <string.h>

#define SHM_SIZE 1024
int main() {
    int shmid;
    char *shm
    shmid = shmget(1234, SHM_SIZE, IPC_CREAT | 0666);
    if (shmid < 0) {
        perror("shmget");
        exit(1);
    }
    shm = (char *)shmat(shmid, NULL, 0);
    if (shm == (char *)-1) {
        perror("shmat");
        exit(1);
    }
    if (fork() == 0) {
        strcpy(shm, "Hello from child using shared memory!");
    } else {
        sleep(1); // wait for child
        printf("Parent read: %s\n", shm);
    }
    shmdt(shm);
    shmctl(shmid, IPC_RMID, NULL);
    return 0;
}
```
## 42.Describe the role of the fork() system call in implementing the shell's job control
- The shell uses fork() to create a child process when executing a command.
- The parent (shell) can manage jobs:
- Foreground (wait for child).
- Background (&, parent doesn’t wait).
- Enables features like job suspension (Ctrl+Z) and resuming (fg, bg).

## 43.Explain the purpose of the execlp() function and provide an example
- execlp() executes a file, searching PATH for the program.
- Syntax:
- int execlp(const char *file, const char *arg, ..., NULL);

## 44.Discuss the significance of the setpgid() system call in managing process groups.
- setpgid(pid, pgid) changes the process group of a process.
- Used by shells for job control → allows grouping of processes (like pipelines) so signals (Ctrl+C, Ctrl+Z) can be sent to the whole group.

## 45.Write a C program to create a child process using vfork() and demonstrate its usage
```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>

int main() {
    pid_t pid = vfork();
    if (pid == 0) {
        printf("Child: Running with vfork(), PID=%d\n", getpid());
        _exit(0);
    } else {
        printf("Parent: PID=%d, child PID=%d\n", getpid(), pid);
    }
    return 0;
}
```
## 46.Explain the concept of process priority inheritance and its importance in real-time systems.
- In real-time systems, priority inversion can occur:
- A high-priority task waits on a resource locked by a low-priority task.
- If a medium-priority task runs, the high-priority task is blocked longer.
### Priority inheritance fixes this:
- Temporarily raises the priority of the low-priority task holding the resource.
- Ensures faster release and prevents deadline misses.

## 47.Describe the role of the fork() system call in copy-on-write(COW) mechanism and its benefits.
- fork() creates a new child process by duplicating the parent process.
- In traditional implementation, this would copy the entire memory space, which is expensive.
### With Copy-on-Write (COW):
- Parent and child initially share the same physical memory pages (marked as read-only).
- If either process modifies a page, a private copy of that page is created (write triggers the copy).
### Benefits:
- Reduces memory usage.
- Improves performance since actual copying happens only when necessary.
- Makes fork() faster and efficient.

## 48.Write a C program to create a pipeline between two processes using the pipe() system call.
