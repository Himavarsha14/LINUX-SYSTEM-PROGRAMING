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
#include<stdio.h>
#include<signal.h>
#include<unistd.h>
void term_handler(int sig)
{
        printf("Caught SITERM signal\n");
        _exit(0);
}
int main()
{
        signal(SIGTERM,term_handler);
        printf("PID:%d.Use 'kill -15 %d' to send SIGTERM.\n",getpid(),getpid());
        while(1)
                pause;
        return 0;
}
```

## 7.Write a C program to install a custom signal handler for SIGILL.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGILL (Illegal Instruction)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGILL,&act,NULL);
    raise(SIGILL);
    while(1) pause();
}
```

## 8.Write a C program to install a custom signal handler for SIGABRT.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>
#include <stdlib.h>

void handler(int signo){
    printf("Caught SIGABRT (Abort)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGABRT,&act,NULL);
    abort();
    while(1) pause();
}
```

## 9.Write a C program to install a custom signal handler for SIGQUIT
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGQUIT\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGQUIT,&act,NULL);
    while(1) pause();
}
```
## 10.Write a C program to install a custom signal handler for SIGTSTP.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGTSTP\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGTSTP,&act,NULL);
    while(1) pause();
}
```
## 11.Write a C program to install a custom signal handler for SIGVTALRM.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>
#include <sys/time.h>

void handler(int signo){
    printf("Caught SIGVTALRM\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGVTALRM,&act,NULL);

    struct itimerval timer={{1,0},{1,0}};
    setitimer(ITIMER_VIRTUAL,&timer,NULL);

    while(1);
}
```

## 12.Write a C program to install a custom signal handler for SIGWINCH.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGWINCH (Window size changed)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGWINCH,&act,NULL);
    while(1) pause();
}
```

## 13.Write a C program to install a custom signal handler for SIGXFSZ.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGXFSZ (File size limit exceeded)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGXFSZ,&act,NULL);
    raise(SIGXFSZ);
    while(1) pause();
}
```
## 14.Write a C program to install a custom signal handler for SIGPWR.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGPWR (Power failure restart)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGPWR,&act,NULL);
    raise(SIGPWR);
    while(1) pause();
}
```
## 15.Write a C program to install a custom signal handler for SIGSYS.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGSYS (Bad system call)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGSYS,&act,NULL);
    raise(SIGSYS);
    while(1) pause();
}
```
## 16.Write a C program to install a custom signal handler for SIGIO.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGIO (I/O possible)\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGIO,&act,NULL);
    raise(SIGIO);
    while(1) pause();
}
```
## 17.Write a C program to install a custom signal handler for SIGUSR2.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught signal %d (USR1 or USR2)\n",signo);
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGUSR1,&act,NULL);
    sigaction(SIGUSR2,&act,NULL);
    while(1) pause();
}
```
## 18.Write a C program to install a custom signal handler for SIGALRM.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGALRM\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGALRM,&act,NULL);
    alarm(3);
    while(1) pause();
}
```
## 19.Write a C program to install a custom signal handler for SIGTRAP.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo){
    printf("Caught SIGTRAP\n");
}

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler=handler;
    sigaction(SIGTRAP,&act,NULL);
    raise(SIGTRAP);
    while(1) pause();
}
```
## 20.Write a C program to install a custom signal handler for SIGTTIN.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo) {
    printf("Caught SIGTTIN (background read from terminal)\n");
    fflush(stdout);
}

int main() {
    struct sigaction act;
    memset(&act, 0, sizeof(act));
    act.sa_handler = handler;
    sigaction(SIGTTIN, &act, NULL);

    printf("PID: %d\n", getpid());
    printf("Run in background and try to read input to trigger SIGTTIN\n");

    while (1) pause();
}
```
## 21.Write a C program to install a custom signal handler for SIGCONT.
```c
#include <stdio.h>
#include <signal.h>
#include <string.h>
#include <unistd.h>

void handler(int signo) {
    printf("Caught SIGCONT (process continued)\n");
    fflush(stdout);
}

int main() {
    struct sigaction act;
    memset(&act, 0, sizeof(act));
    act.sa_handler = handler;
    sigaction(SIGCONT, &act, NULL);

    printf("PID: %d\n", getpid());
    printf("Press Ctrl+Z to stop, then 'fg' to continue\n");

    while (1) pause();
}
```
## 22.Write a c program on watch dog timer and explain its working.
```c
/* file: sigaction_demo.c */
#include <stdio.h>
#include <string.h>
#include <signal.h>
#include <unistd.h>

void handler(int s){ printf("Handler for %d\n", s); fflush(stdout); }

int main(){
    struct sigaction act;
    memset(&act,0,sizeof(act));
    act.sa_handler = handler;
    act.sa_flags = 0;
    sigaction(SIGINT, &act, NULL);   /* Ctrl+C */
    sigaction(SIGTERM, &act, NULL);

    printf("sigaction handlers installed (SIGINT, SIGTERM). PID=%d\n", getpid());
    while(1) pause();
    return 0;
}
```
## 23.Writte a C program to demonstrate hoe to block signals using sigprocmask.
```c
#include<stdio.h>
#include<signal.h>
#include<string.h>
#include<unistd.h>
int main()
{
        sigset_t set;
        sigemptyset(&set);
        sigaddset(&set,SIGINT);
        printf("Blocking SIGINT for 5 seconds.\n");
        sigprocmask(SIG_BLOCK,&set,NULL);
        sleep(5);
        printf("Unblocking SIGINT.\n");
        while(1);
        return 0;
}
```
##  24.Write a C program to handle Signal handling in multithreaded environment.
```c
#include<stdio.h>
#include<string.h>
#include<signal.h>
#include<pthread.h>
#include<unistd.h>
void *thread(void *arg)
{
        printf("thread started, sleeping....\n");
        sleep(10);
        printf("Thread done\n");
        return NULL;
}
void handler(int s)
{
        printf("Main thread caught signal %d\n",s);
        fflush(stdout);
}
int main()
{
        pthread_t t;
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_handler=handler;
        sigaction(SIGINT,&act,NULL);
        pthread_create(&t,NULL,thread,NULL);
        printf("Send Ctrl+c to see which thread handles it\n");
        pthread_join(t,NULL);
        return 0;
}
```

## 25.Write a C program to handler a real-time signal using sigqueue.
```c
#include<stdio.h>
#include<string.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int s,siginfo_t *info, void *u)
{
        printf("RT signal %d received, value=%d\n",s,info->si_value.sival_int);
        fflush(stdout);
}
int main()
{
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_sigaction=sighandler;
        act.sa_flags=SA_SIGINFO;
        sigaction(SIGRTMIN,&act,NULL);

        union sigval val;
        val.sival_int=123;
        sigqueue(getpid(), SIGRTMIN,val);
        while(1)
                pause();
        return 0;
}
```
## 26.Write a c program to handle SIGTSTP and suspend the process.
```c
#include<stdio.h>
#include<string.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int signo)
{
        printf("SIGTSTP caught\n");
        raise(SIGTSTP);
}
int main()
{
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_handler=sighandler;
        sigaction(SIGTSTP,&act,NULL);
        printf("Press Ctrl+z to trigger SIGTSTP\n");
        while(1)
                pause;
        return 0;
}
```
## 27.Write a C program to handle multiple signals using sigaction().
```c
#include<stdio.h>
#include<string.h>
#include<signal.h>
#include<unistd.h>
void sighandler(int signo)
{
        printf("Executing signal handler\n");
}
int main()
{
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_handler=sighandler;
        sigaction(SIGINT,&act,NULL);
        sigaction(SIGTERM,&act,NULL);
        sigaction(SIGUSR1,&act,NULL);
        printf("Handlers are for SIGINT<SIGTERM<SIGUSR1 installed.PID=%d\n",getpid());
        while(1)
                pause();
}
```
## 28.Write a C program to demonstrate IPC using signals.
```c
#include<stdio.h>
#include<string.h>
#include<signal.h>
#include<unistd.h>
#include<sys/wait.h>
volatile sig_atomic_t got=0;
void sighandler(int signo)
{
        got=1;
}
int main()
{
        pid_t pid;
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_handler=sighandler;
        sigaction(SIGUSR1,&act,NULL);
        sigaction(SIGUSR2,&act,NULL);
        pid=fork();
        if(pid==0)
        {
                printf("Child sleeping 1s then sending SIGUSR1 to parent (%d)\n",getppid());
                sleep(1);
                kill(getppid(),SIGUSR1);
                while(!got)
                        pause();
                printf("Child:got reply from parent,exiting\n");
                return 0;
        }
        else
        {
                printf("Parent:Waiting for SIGUSR1, sending SIUSSR2 to child (%d)\n",pid);
                kill(pid,SIGUSR2);
                wait(NULL);
        }
        return 0;
}
```
## 29.Write a C program to demonstrate error handling using signals in system calls.
```c
#include<stdio.h>
#include<signal.h>
#include<string.h>
#include<unistd.h>
#include<errno.h>
void sighandler(int signo)
{
        printf("Signal %d receieved\n",signo);
}
int main()
{
        struct sigaction act;
        char buf[10];
        memset(&act,0,sizeof(act));
        act.sa_handler=sighandler;
        sigaction(SIGALRM,&act,NULL);
        alarm(2);
        ssize_t n;
retry:
        n=read(0,buf,10);
        if(n<0)
        {
                if(errno==EINTR)
                {
                        printf("read interrupted by signal,retrying\n");
                        goto retry;
                }
                perror("read");
                return 1;
        }
        printf("read returned %zd bytes\n",n);
        return 0;
}
```
## 40.Write a program to block and unblock using sigprocmask()
```c
#include<stdio.h>
#include<signal.h>
#include<string.h>
#include<unistd.h>
int main()
{
        sigset_t set;
        sigemptyset(&set);
        sigaddset(&set,SIGINT);
        printf("Blocking SIGINT for 5s.Try ctrl+c\n");
        sigprocmask(SIG_BLOCK,&set,NULL);
        sleep(5);
        printf("Unblocking SIGINT\n");
        sigprocmask(SIG_UNBLOCK,&set,NULL);
        while(1)
                pause();
}
```
## 41.Write a C programming to handle SIGBUS in system programming context.
```c
#include<stdio.h>
#include<signal.h>
#include<string.h>
#include<unistd.h>
void sighandler(int signo)
{
        printf("caught SIGBUS %d\n",signo);
}
int main()
{
        struct sigaction act;
        memset(&act,0,sizeof(act));
        act.sa_handler=sighandler;
        sigaction(SIGBUS,&act,NULL);
        raise(SIGBUS);
        while(1)
                pause();
}
```
