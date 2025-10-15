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



