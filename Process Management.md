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
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<string.h>
#include<sys/wait.h>
void main()
{
        int fd[2];
        pid_t pid;
        char write_msg[]="Hello from parent via pipe!";
        char read_msg[100];
        if(pipe(fd)==-1)
        {
                perror("pipe");
                exit(1);
        }
        pid=fork();
        if(pid<0)
        {
                perror("fork");
                exit(1);
        }
        else if(pid==0)
        {
                close(fd[1]);
                read(fd[0],read_msg,sizeof(read_msg));
                printf("Child received: %s\n",read_msg);
                close(fd[0]);
        }
        else
        {
                close(fd[0]);
                write(fd[1],write_msg,strlen(write_msg)+1);
                close(fd[1]);
        }
        return 0;
}
```
## 49.Explain the concept of process scheduling policies such as FIFO,Round robin, and priority scheduling.
### 1.FIFO(First In, First OUT/FCFs)
- Processes are schedules in the order they arrive.
- Simple, but can cause convoy effect(long job delays short jobs).

### 2.Round Robbin(RR)
- Each process gets a fixed time slice(Quantum).
- After Quantum expires, the process is preempted and moved to the back of the queue.
- Good for time-sharing systems, ensures fairness.

### 3.Priority Scheduling
- Each process has a priority value.
- Higher-priority processes are executed first.
- Can cause starvation for low-priority tasks (solved by aging).

## 50.Discuss the significance of the execve() function in process creation and execution.
- execve() is a system call in Linux/Unix used to execute a new program within the context of an existing process.
- When a process calls execv(),its current memory image is replaces by a new program (code,data,stack).
### Key points:
- Does not create a new process->it just replaces the current process image.
- File descriptors(open files, sockets,pipes) remain open unless marked close-on-exec.
- Often used after fork():parent calls fork() to create a new process, child calls execve() to run a new program.
- Example Use: Shells use fork() + execve() when you run commands.

## 51.Write a C program using nice() to adjust process priority
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/time.h>
#include <sys/resource.h>

int main() {
    int priority;
    printf("Default priority: %d\n", getpriority(PRIO_PROCESS, 0));
    nice(5); // Increase niceness (lower priority)
    priority = getpriority(PRIO_PROCESS, 0);
    printf("New priority after nice(5): %d\n", priority);
    while(1); // Keep process running
    return 0;
}
```
- nice() changes niceness->higher value=lower priority(less CPU time).

## 52.Explain the concept of process swapping and its role in memory management.
- Definition: Swapping is moving an entire process from main memory to disk(swap space) and bringing it back later.
### Role:
- Frees memory for other processes.
- Allows more processes to run than the available RAM.

### Drawback:
Expensive (disk I>O is slow), can cause thrashing if excessive swapping occurs.

## 53.Discuss the difference between the fork() and pthread_create() functions in terms of process/thread creation.
| Feature     | `fork()`                                   | `pthread_create()`                     |
| ----------- | ------------------------------------------ | -------------------------------------- |
| Creates     | New **process**                            | New **thread**                         |
| Memory      | Independent memory space                   | Shared memory within same process      |
| PID/TID     | Child gets new PID                         | Thread has same PID, different TID     |
| Performance | Heavier (duplicate PCB, memory structures) | Lighter (just a new execution context) |
| Use case    | Multiprocessing                            | Multithreading                         |
## 54.Describe the purpose of the prctl() system call in process management.
- prctl() = process control.
- Used to control specific characteristics of processes.
- Examples:
       - Set process name ( PR_SET_NAME).
       - Get process name ( PR_GET_NAME).
       - Enable/disable features (like core dumps, death signal).
 - Useful in debugging and fine-tuning process behaviour.

## 55.Write a C program to demonstrate the use of the clone() system call to create a thread.
```c
#define _GNU_SOURCE
#include <sched.h>
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int thread_func(void *arg) {
    printf("Hello from clone! PID=%d\n", getpid());
    return 0;
}

int main() {
    char *stack = malloc(1024*1024); // Allocate stack for new thread
    if (!stack) {
        perror("malloc");
        exit(1);
    }

    clone(thread_func, stack + (1024*1024), CLONE_VM | CLONE_FS | CLONE_FILES | CLONE_SIGHAND | SIGCHLD, NULL);
    sleep(1); // Give time for thread to run
    printf("Back in main process.\n");
    free(stack);
    return 0;
}
```
## 56.Explain the concept of process preemption and its impact on system responsiveness.
- Preemption = Forcibly suspending a running process so the CPU can switch to another.
- Ensures:
     - Fairness->no single process hogs CPU.
     - Responisveness->interactive tasks(keyboard,GUI)get timely CPU.
- Controlled by scheduler (e.g., Round Robbin, priority scheduling).

## 57.Discuss the role of the exec functions in handling file descriptors during process execution.
- On exex(), file descriptors remain open by default unless marked with FD_CLOEXCEC.
- This allows the new program to reuse parent's open files(e.g., I/O redirection in shells).
- Example:
- ls > out.txt->shell opens out.txt,forks,child exec()s ls,and ls writes into out.txt.

## 58.Write a C program to create a child process using fork() and communicate between parent and child using pipes.
```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd[2];
    pid_t pid;
    char msg[] = "Hello from parent!";
    char buffer[50];

    pipe(fd);
    pid = fork();

    if (pid == 0) { // Child
        close(fd[1]); // Close write end
        read(fd[0], buffer, sizeof(buffer));
        printf("Child received: %s\n", buffer);
    } else { // Parent
        close(fd[0]); // Close read end
        write(fd[1], msg, strlen(msg)+1);
    }
    return 0;
}
```

## 59.Explain the significance of process priorities and how they affect scheduling decisions.
- Priorities decide which process gets CPU first.
- High-priority =more CPU time, low-priority=less CPU time.
- Helps in:
      - Ensuring critical tasks (real-time,kernal) run quickly.
      - Balancing background vs interactive workloads.

## 60.Describe the process of process termination and the various ways it can occur.
A process ends when:
- Normal Exit->exit() call.
- Error Exit->abnormal condition.
- Killed by signal->eg., kill -9.
- Parent killed->with SIGKILL or job control.
On termination:
- Resources are freed (memory, files).
- Exit status is sent to the parent (Can be checked with wait()).

## 61.Discuss the role of the exit status in process termination and how it can be retrieved by the parent process.
- Every process return an exit status (an integer) when it terminates.
- Purpose:
     - Indicates success (0) or error (non-zero).
     - Parent uses it to decide what happened to the child.
- ### How parent retrives it:
- By calling wait() or waitpid()->exit status is stored in the status variable.
- Use macros WIFEXITED(status) to WEEXITSTATUS(status) to interpret.

```c
int status;
pid_t pid = wait(&status);
if (WIFEXITED(status)) {
    printf("Child exited with status %d\n", WEXITSTATUS(status));
}
```

## 62.Write a C program to demonstrate process synchronization using the fork() and wait() system calls.
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) { // Child
        printf("Child: Doing work...\n");
        sleep(2);
        printf("Child: Done!\n");
    } else { // Parent
        wait(NULL); // Wait for child
        printf("Parent: Child finished, now continuing.\n");
    }
    return 0;
}
```

## 63.Explain the concept of process suspension and resumption using signals.
- Processes can be paused (suspended) or resumed using signals:
- SIGSTOP->suspends a process.
- SIGCONT->resumes a process.
- Example:
- Kill -STOP <pid> suspends.
- Kill -CONT <pid> resumes.
- Useful in job control(shells: Ctrl+Z->suspend, fg/bg->resume).

## 64.Discuss the role of process scheduling algorithms in determining the order of execution among processes. 
- Decides which process runs next on CPU.
- Goals:Fairness, Efficiency, Responsiveness.
- Common algorithms:
- FIFO/FCFS->Simple queue, non-preemptive.
- Round Robin(RR)->preemptive,time slices,good for multitasking.
- Priority Scheduling->Higher priority processes run first.
- Multilevel Queue->Seperate queues for foreground/background.
- Scheduling policy directly affects throughput, latency and user experience.

## 65.Write a C program to demonstrate process synchronization using the fork() and wait() system calls
```c
#define _GNU_SOURCE
#include <stdio.h>
#include <unistd.h>
#include <sched.h>
#include <string.h>

int main() {
    struct sched_param param;
    param.sched_priority = 10;

    if (sched_setscheduler(0, SCHED_FIFO, &param) == -1) {
        perror("sched_setscheduler");
    } else {
        printf("Changed scheduling policy to SCHED_FIFO with priority 10\n");
    }

    while(1); // Keep running
    return 0;
}
```
## 66.Explain the concept of process migration and its relevance in distributed systems.
- Definition:Moving a process from one machine to another during execution.
- Relevance:
      - Load balancing(Distribute work evenly).
      - Fault tolerance(recover from failures).
      - Resource optimization (move proces closer to required resources).
- Used in cloud computing,cluster computing.

## 67.Describe the role of process identifiers (PIDs) in process management and their uniqueness within the system.
- PID = unique numbers assigned by OS to each process.
- Used to:
- Track/manage processes.
- Send signals (kill,kill -9).
- retrieve status(waitpid).
- Uniqueness:At a given time, each process has a unique PID.

## 68.Write a C program to create a child process using fork() and demonstrate inter-process communication (IPC) using shared memory.
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/shm.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main() {
    int shmid;
    char *shm;
    key_t key = 1234;

    shmid = shmget(key, 1024, IPC_CREAT | 0666);
    if (shmid < 0) { perror("shmget"); return 1; }

    if (fork() == 0) { // Child
        shm = shmat(shmid, NULL, 0);
        printf("Child writing to shared memory...\n");
        strcpy(shm, "Hello from child!");
    } else { // Parent
        wait(NULL);
        shm = shmat(shmid, NULL, 0);
        printf("Parent read: %s\n", shm);
    }
    return 0;
}
```
## 69.Explain the concept of process tracing and its importance in debugging and monitoring.
- Defintion:Mechanism to monitor and control execution of processes.
- Implemented via ptrace() system call.
- Importance:
      - Debuggers(like gdb) use it.
      - Allows setting breakpoints, inspecting memory, register.
      - Useful for security monitoring.

## 70.Discuss the role of the fork() system call in creating copy-on-write (COW) processes and its impact on memory usage.
- Normally, fork() would copy entire parent memory->expenisve.
- with COW:
     - Parent & child share same memory pages initially (read-only).
     - When either modifies, OS creates a private copy.
- Benifits:
     - Faster fork().
     - Saves memory.
- Example:Linux fork() + exec() combo is efficient due to COW.

## 71.Describe the concept of process control blocks (PCBs) and their role in process management.
- PCB = data structure maintained by OS for each process.
- Contains:
     - Process state(running,waiting,ready).
     - Program counter(next instruction).
     - CPU registers.
     - Memory info.
     - Scheduling info (priority, time slice).
     - Open files list.
  - Role:
      - Stores all info to suspend & resume processes.
      - Essential for context switching.

## 72.Write a C program to demonstrate the use of the prctl() system call to change process attributes.
```c
// prctl_example.c
#define _GNU_SOURCE
#include <stdio.h>
#include <sys/prctl.h>
#include <unistd.h>
#include <signal.h>
#include <string.h>

int main() {
    // Set process name (shows up in ps)
    if (prctl(PR_SET_NAME, (unsigned long)"my_prctl_proc", 0, 0, 0) == -1) {
        perror("prctl(PR_SET_NAME)");
    }

    // Set parent-death signal: get SIGTERM if parent dies
    if (prctl(PR_SET_PDEATHSIG, SIGTERM) == -1) {
        perror("prctl(PR_SET_PDEATHSIG)");
    }

    printf("PID=%d, name set to 'my_prctl_proc'. Parent PID=%d\n", getpid(), getppid());
    printf("Sleeping for 60s — try killing parent to see PDEATHSIG behavior.\n");
    for (int i=0;i<60;i++) sleep(1);
    return 0;
}
```

## 73.Explain the concept of process scheduling classes and their significance in real-time operating systems.
- Scheduling Classes group policies with similar behaviour.
- Examples in Linux:
      - Real-time class:SCHED_FIFO, SCHED_RR.Tasks are scheduled before ordinary tasks and can preempt them.
  Necessart for systems with strict timing(Audio processing, Control loops).
- Normal class:SCHED_OTHER(CFS in Linux) for general-purpose fairness.
- Batch/Idle: SCHED_BATCH, SCHED_IDLE for CPU-intensive or background tasks.
### Significance in RTOS/real-time OS:
- Guarantees for latency/jitter.
- Deterministic dispatching:high priority real-time tasks run predictably.
- Prevents starvation for critical tasks(though must be carefully designed to avoid starving lower-priority ones).

## 74.Discuss the role of the vfork() system call in process creation and its differences from fork().
### vfork():
- It is used to create a new process(child) that shares the address space of the parent process temporarily until it calls exec() or _exit().It is optimized for cases where the child immediately executes a new program

### Differences from fork():
| Feature       | `fork()`                                        | `vfork()`                                                       |
| ------------- | ----------------------------------------------- | --------------------------------------------------------------- |
| Address space | Child gets a **copy** of parent’s address space | Child **shares** parent’s address space                         |
| Execution     | Parent and child execute **independently**      | Parent is **suspended** until child calls `exec()` or `_exit()` |
| Performance   | Slower due to copying                           | Faster as no copy is made                                       |
| Safety        | Safe to modify memory                           | Unsafe to modify shared memory before `exec()`                  |

## 75.Describe the purpose of the sigaction() system call in handling signals in processes.
- sigaction() is used to define how a process handles specific signals (like SIGINT, SIGTERM).
- int sigaction(int signum,const struct sigaction *act, struct sigaction *oldact);
- signum- signal number
- act- specifies new action
- oldact- retrives previous action
## It allows:
- Setting custom signal handlers
- Ignoring signals
- Restoring default behaviour
- Using flags like SA_RESTART and SA_SIGINFO for more control.

## 76.Explain the concept of process migration and its implications in distributed computing environments.
- Process migration refers to moving a running process from one machine to another in a distributed system.
### Implications:
- Improves load balancing.
- Enhances resource utilization.
- Enables fault tolerance.
- Requires maintaining process state(memory, open files, etc).
- Complex due to network latency and consistency issues.

## 77.Write a C program to create a child process using fork() and demonstrate process communication using message queues.
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>
#include <string.h>

struct msg_buffer {
    long msg_type;
    char msg_text[100];
};

int main() {
    key_t key = ftok("progfile", 65);
    int msgid = msgget(key, 0666 | IPC_CREAT);

    struct msg_buffer message;
    message.msg_type = 1;
    strcpy(message.msg_text, "Hello from Parent");
    msgsnd(msgid, &message, sizeof(message), 0);
    printf("Message sent: %s\n", message.msg_text);

    msgrcv(msgid, &message, sizeof(message), 1, 0);
    printf("Message received: %s\n", message.msg_text);

    msgctl(msgid, IPC_RMID, NULL);
    return 0;
}
```

## 78.Discuss the role of the sigprocmask() system call in managing signal masks for processes.
- sigprocmask() is used to block, unblock, or check signals in a process's signal mask.
### usage:
int sigprocmask(int how,const sigset_t *set, sigset_t *oldset);
- how:SIG_BLOCK, SIG_UNBLOCK, SIG_SETMASK
- It ensures signals are not handled while executing critical sections.

## 79.Describe the purpose of the setrlimit() system call in setting resource limits for processes.
setrlimit() sets resource limits for a process (like CPU time,file size, memory).
```c
#include <sys/resource.h>

struct rlimit limit;
limit.rlim_cur = 10;
limit.rlim_max = 20;
setrlimit(RLIMIT_CPU, &limit);
```
## 80.Discuss the concept of process groups and their importance in job control and signal propagation.
- A process group is a collection of related processes (like a shell pipeline).
### Importance:
- Allows group signal control(killpg()).
- Enables job control(fore ground/back groud jobs).
- Helps the shell manage processes efficiently.

## 81.Explain the role of the prlimit() system call in setting resource limits for processes.
- prlimit() combines the functionality of getrlimit() and sterlimit()-it sets or retrieves resources limits for a given process.
### syntax:
```c
int prlimit(pid_t pid, int resource,
            const struct rlimit *new_limit,
            struct rlimit *old_limit);
```
## 82.Discuss the concept of process scheduling policies in multi-core systems and their implications.
Common policies:
- SCHED_OTHER-default time-sharing.
- SCHED_FIFO- real time, first-in-first-out.
- SCHED_RR- round robin
- SCHED_DEADLINE- deadline-based scheduling
### Implications:
- Affects CPU utilization.
- Impacts latency and throughput
- Important for real-time and parallel systems.

## 83.Describe the role of the setpriority() system call in adjusting process priorities.
- setpriority() sets the nice value(priority) of a process.
- int setpriority(int which, id_t who, int prio);
- which:PRIO_PROCESS, PRIO_PGRP, PRIO_USER
- prio:--20(highest) to +19(lowest).

## 84.Discuss the role of the prlimit64() system call in setting resource limits for processes with 64-bit address space.
### Purpose:
- The prlimit64() system call sets or retrieves resource limits (like CPU time, memory usage, file size) for a process.
### Syntax:
```c
int prlimit64(pid_t pid, int resource,
              const struct rlimit64 *new_limit,
              struct rlimit64 *old_limit);
```
### Key points:
- Works with 64-bit address space, allowing very large limits (beyond 4GB).
- Combines functionality of setrlimit() and getrlimit() in one call.
- pid=0->affects the calling process.
- Example resource limits:RLIMIT_CPU, RLIMIT_AS, RLIMIT_NOFILE.
### Use:
- To control how much CPU, memory, or file descriptors a process can consume.

## 85.Describe the purpose of the sched_getaffinity() system call in querying the CPU affinity of a process.
### purpose:
- Retrieves the CPU affinity mask of a process --- i.e., which CPUs the process is allowed to run on.

### Syntax:
```c
int sched_getaffinity(pid_t pid, size_t cpusetsize, cpu_set_t *mask);
```
### Explanation:
- mask stores CPUs on which the process can execute.
- pid=0->refers to the calling process.
### Use:
- Helps optimize performance by checking which cores are available for a process.

## 86.Discuss the concept of process re-parenting and its implications in process management.
### Definition:
- Process re-parenting is the mechanism in which a child process is assigned a new parent process when its original parent terminates or exits before the child.

### How it happens:
- Every process in Linux has a parent process ID(PPID).
- When a parent process dies before its child:
      - The child becomes an orphan process.
      - The init process(PID 1) or systemd automatically adopts (re-parents) the orphaned child.
     - This enusres that every process in the system always has a valid parent.

## 87.Discuss the concept of process checkpointing and its relevance in fault tolerance and process migration.
### Definition:
- Process checkpointing is the technique of saving the current state of a running process (CPU registers, memory, stack, open files, etc.) to a file ---called a checkppoint ---so that it can be resumed later from the same point.
### Relevance:
### 1.Fault Tolerance:
- If a system crashes, the process can be restarted from the last checkpoint instead of from the beginning.
### 2.Process Migration:
- A checkpoint file can be transferred to another machine and resumed there (Useful in distributed systems and load balancing).
### 3.Long-running tasks:
- Scientific computations and simulations use checkpoints to avoid losing progress.
Tools:DMTCP(Distributed Multithreaded Checkpointing), BLCR, CRIU.

## 88.Explain the significance of the /proc filesystem in providing information about processes in Linux.
### Definition:
- /proc is a virtual filesystem that provides a window into the kernal and process information in Linux.
### Key Points:
- Contains directories named after process IDs(PIDs)->/proc/[pid]/
- Common files:
- /proc/[pid]/status->process state, memory info
- /proc/[pid]/cmdline->command-line arguments
- /proc/cpuinfo, /proc/meminfo->hardware info
### Use:
- Helps in monitoring, debugging, and gathering runtime system info.

## 89.what is process and what is the difference between process and program?

| **Aspect** | **Program**                         | **Process**                      |
| ---------- | ----------------------------------- | -------------------------------- |
| Definition | Set of instructions stored on disk. | Running instance of a program.   |
| Nature     | Passive                             | Active                           |
| Example    | `/bin/ls` file                      | `ls` command running in terminal |

## 90.what type of infomation pcb contains ?
Contains all information about a process:
- process ID(PID).
- Process state.
- Program counter.
- CPU registers.
- Memory info.
- Accounting info.
- I/O status and open files.
->Acts as the process's identify record inside the kernal.

## 91.Explain about memory segements ?
- Text Segment: Program code.
- Data Segment: Initialized global/static variables.
- BSS Segment: Uninitilized global/static variables.
- Heap: Dynamically allocated memory( malloc,calloc ).
- Stack: Function calls, local variables.

## 92.when fork invoked what are things will happen ?
- Duplicates the parent process.
- Creates a child with a new PID.
- Both processes start from the next instruction.
- Memory and file descriptors are copied(Copy-on-Write).
- Return values differ:
      - Parent gets child PID.
      - Child gets 0.

## 93.what is the difference between ps and top?
| **Command** | **Description**                                      |
| ----------- | ---------------------------------------------------- |
| `ps`        | Shows a snapshot of running processes at one moment. |
| `top`       | Continuously updates live process information.       |

## 94.what are the types of processes we have explain each process breifly?
- Foreground:Runs interactively in terminal.
- Background:Runs without terminal control.
- Daemon:System service process(e.g.,sshd).
- Zombie:Finished but not yet removed from process table.
- Orphan:Parent exited before child.

## 95.What is the PPID of Orphan Process.
- PID 1(init/systemd) becomes the new parent.

## 96.what is the difference between exit vs return?
| **return**                              | **exit()**                     |
| --------------------------------------- | ------------------------------ |
| Exits current function.                 | Terminates the entire process. |
| Used in functions.                      | Used anywhere in code.         |
| In `main()`, `return` = `exit(status)`. |                                |

## 97.In parent process can you access to the exit code of return value of child process (or) in parent process how do you block to get the id of child?
- Parent can wait for child using wait() or waitpid():
```c
int status;
waitpid(child_pid, &status, 0);
if (WIFEXITED(status))
    printf("Exit code: %d\n", WEXITSTATUS(status));
```
- Parent blocks untill child terminates.

## 98.what is the difference between Zombie and Orphan process?
| **Type**   | **Definition**                                       | **Cause**                    |
| ---------- | ---------------------------------------------------- | ---------------------------- |
| **Zombie** | Process terminated but entry still in process table. | Parent didn’t call `wait()`. |
| **Orphan** | Parent terminated before child.                      | Adopted by `init`.           |

## 99.what is the use of fork() ?
- Creates a child process.
- Enables parallel execution
- Basis for multitasking and IPC in Linux.

## 100.define system call name some blocking system calls?
### Definition:
- A system call is an interface between a user process and the kernal, allowing a program to request services (like file operations, process creation, or I/O) from the operating system.
### Examples of system Calls:
read(), Write(), open(), close(), fork(), exec(), wait(), exit().
### Blocking system calls:
- These calls suspend (block) the calling process untill an event occurs.
Examples:
- read()->blocks untill data is available.
- accept()->Waits for a network connection.
- wait()->waits for a child process to finish.
- recv()->waits for incoming data.

## 101.Why can’t we run a program directly from hard disk? Why copy to RAM for execution?
- The CPU cannot access data directly from hard disk(too slow and non-volatile).
- RAM (main memeory) is much faster and directly accessible by the CPU.
- Hence, before execution, the program (binary) is loaded into RAM, then the CPU fetches instructions from there.

## 102.How do you copy data from RAM to CPU registers?
- The CPU uses the ḍata and control signals to fetch data from a specific memory address in RAM.
- The address of the data is placed on the address bus.
- The Memory Read(RD) signal is asserted.
- Data is transferred through the data bus into the CPU register.

## 103.How do you copy data from CPU registers to RAM?
- The address of the destination memory is placed on the address bus.
- Data from the register is put on the data bus.
- The Memory Write(WR) signal is asserted.
- The RAM controller writes data to that memory location.

## 104.Explain about paging technique.
### Definition:
- Paging is a memory management technique that divides logical memory into fixed-size pages and physical memory into frames.
### Key Points:
- Each process has its own oage table to map virtual pages to physical frames.
- Eliminates external fragmentation.
- Page size is typically 4KB.
- CPU generates virtual address->translated to physical address using page table.

## 105.Why is the number of pages not fixed?
- The number of pages depends on the process size and page size.
-                     No.of pages=process size/page size
- As process sizes vary, the number of required pages also varies.
- Different processes->different memory requirements->different page counts.

## 106.Explain about shared memory. Why do we require shared memory?
### Definition: 
- Shared memory is an interprocess communication (IPC) method that allows multiple processes to access a common region of memory.
### Why required:
- Fastest IPC method (no need to copy data through the kernal).
- Enables efficient data sharing between related processes (e.g., client-server).
- Useful for real-time or large data exchange scenarios.

## 107.How can multiple processes share data?
Multiple processes can share data using:
- 1.Shared memory(most efficient).
- 2.Pipes/FIFOs.
- Message queues.
- Sockets(for networked communication).
- Files(Persistent data exchange).

## 108.explain the scenario when pages of process-1 and pages of process-2 are copied into same frames (or) explain the scenario where pages of multiple process are sharing the same physical frames(or) Can the page table of multiple process point to same physical frames?
- Yes, page tables of multiple processes can point to the same physical frame.
### Example:
- When two processes use a shared library or shared memory segment, both their page tables point to the same physical pages.
- This allows data sharing without duplication.

### Use cases:
- Shared code segments (like libc).
- IPC via shared memory.
- Copy-on-Write (COW) mechanism after fork().

## 109.How does the child process use the memory segment of the parent process?
- After fork(), the child inherits the parent's address space(text, data, heap, stack).
- Initially, they share the same physical pages (Copy-on-Write).
- When either process writes to a shared page, a new copy is created for that process.

## 110.How do parent and child processes keep track of which instruction to execute?
- Each process has its own program counter(pc).
- After fork(), both parent and child have identical PC values, but seperate copies.
- The OS scheduler independently increments and maintain each PC as they execute.

## 111.How do parent and child processes execute different blocks of code after fork()?
- fork() returns different values:
     - 0 to child.
     - Child PID to parent.
- Based on this, you can write conditional code:
```c
pid_t pid = fork();
if (pid == 0)
    printf("Child code\n");
else
    printf("Parent code\n");
```
## 112.Explain write operation to pages shared by parent and child (Copy-on-Write). Explain address translation.
- After ​fork(), both share same physical frames (read-only).
- When one writes, OS:
     - 1.Traps the write attempt.
     - 2.Creates a private copy of that page.
     - Updates that process's page table to point to the new frame.
     - This is copy-on-Wtite (COW).
### Adress Translation:
- Virtual address=(page number + offset).
- Using page table, page number->physical frame number->actual address.

## 113.What address does CPU place on the address bus?
- The CPU places the physical address on the address bus to access RAM.
- If the system uses virtual memory, virtual addresses are first translated into physical addresses before being placed on the bus.

## 114.When are virtual addresses generated?
- When a program executes and refers to memory (instruction fetch, data access).
- Virtual addresses are generated by the CPU for each process.

## 115.When are physical addresses generated from virtual addresses?
During address translation, which occurs:
- In MMU (Memory Management Unit) using page table.
- Just before the memory access to RAM.

## 116.What is the exec() family of calls? Why do we need them? What are they?
### Definition:
- The exec() family replaces the current process image with a new program -i.e., the process becomes the new program.
### Need:
- Used after fork() to load a different program in the child process.
### Variants:
- execl(path,arg0,arg1,.....,NULL);
- execv(path,argv[]);
- execlp(file,arg0, arg1, .....,NULL);
- execvp(file, argv[]);
- execve(path, argv[],envp[]);
l-list arguments, v-vector (array), p- search PATH, e-environment.

## 117.Why do most system calls return error?
- Because system resources (files, memory, permissions) may not always be available.
- On failure, most system calls return -1 and set errno (error number).
- Helps the program handle exceptions gracefully.

## 118.Difference between execl() and execv()
| **execl()**                                    | **execv()**                                                  |
| ---------------------------------------------- | ------------------------------------------------------------ |
| Takes **list** of arguments.                   | Takes **array (vector)** of arguments.                       |
| Example: `execl("/bin/ls", "ls", "-l", NULL);` | `char *args[] = {"ls", "-l", NULL}; execv("/bin/ls", args);` |

## 119.Difference between execlp() and execvp()
| **execlp()**                                               | **execvp()**                                             |
| ---------------------------------------------------------- | -------------------------------------------------------- |
| Takes **list** of arguments and searches file in **PATH**. | Takes **array** of arguments and searches in **PATH**.   |
| Example: `execlp("ls", "ls", "-l", NULL);`                 | `char *args[] = {"ls", "-l", NULL}; execvp("ls", args);` |


       

