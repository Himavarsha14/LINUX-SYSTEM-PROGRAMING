## 1.Write a C program to catch and handle the SIGINT signal
```c
#include<stdio.h>
#include<unistd.h>
#include<signal.h>
void signal_handler(int signo)
{
        printf("\n");
        printf("This is my signal handler\n");
        _exit(0);
}
int main()
{
        signal(SIGINT,signal_handler);
        printf("This is main function.\n");
        while(1)
        {
                sleep(1);
        }
        return 0;
}
```
## 2.Implement a C program to send a custom signal to another process.
### Sender program(sends signal)
```c
include<stdio.h>
#include<unistd.h>
#include<signal.h>
#include<stdlib.h>
int main(int argc, char *argv[])
{
        if(argc!=2)
        {
                printf("Usage:%s <PID>\n",argv[0]);
                return 1;
        }
pid_t pid=atoi(argv[1]);
kill(pid,SIGUSR1);
printf("Sent SIGUSR1 to process:%d\n",pid);
return 0;
```
### Receiver program(receives signal)
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void handler(int signum)
{
        printf("Received SIGUSSR! signal!\n");
        _exit(0);
}
int main()
{
        signal(SIGUSR1,handler);
        printf("Waiting for signal.... PID=%d\n",getpid());
        while(1)
                pause();
        return 0;
}
```
## 3.Create a C program to ignore the SIGCHILD signal temporarily
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
#include<sys/wait.h>
int main()
{
        signal(SIGCHLD, SIG_IGN);
        pid_t pid=fork();
        if(pid==0)
        {
                printf("child process exiting...\n");
                _exit(0);
        }
        else
        {
                printf("Parent ignoring SIGCHILD...\n");
                sleep(3);
                printf("Parent resumed\n");
        }
        return 0;
}
```
## 4.Write a program to block the SIGTERM signal using sigprocmask().
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int sig)
{
        printf("SIGTERM received!\n");
}
int main()
{
        sigset_t set;
        signal(SIGTERM,sighandler);
        sigemptyset(&set);
        sigaddset(&set,SIGTERM);
        sigprocmask(SIG_BLOCK,&set,NULL);
        printf("SIGTERM blocked for 10 seconds\n");
        sleep(10);

        sigprocmask(SIG_UNBLOCK,&set,NULL);
        printf("SIGTERM unblocked.\n");
        return 0;
}
```
## 5.Implement a C program to handle the SIGALRM signal using sigaction().
```c
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void alarm_handler(int sig)
{
        printf("Alarm signal caught\n");
}
int main()
{
        struct sigaction sa;
        sa.sa_handler=alarm_handler;
        sa.sa_flags=0;
        sigemptyset(&sa.sa_mask);
        sigaction(SIGALRM,&sa,NULL);
        alarm(3);
        printf("Waiting for alarm....\n");
        pause();
        return 0;
}
```
## 6.Write a C program to install a custom signal handler for SIGTERM.
```c
```

## 
