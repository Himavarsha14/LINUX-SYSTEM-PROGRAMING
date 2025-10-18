## 1.Implement a program that uses pipes for communication between a parent and child process. Show how data can be passed between processes using pipes.
```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
int main()
{
        int fd[2];
        pipe(fd);
        int pid=fork();
        if(pid==0)
        {
                char buf[20];
                read(fd[0],buf,sizeof(buf));
                printf("Child got:%s",buf);
                printf("\n");
        }
        else
        {
                char text[]="Hello from parent";
                write(fd[1],text,strlen(text)+1);
        }
        return 0;
}
```
## 2.Create a program where two processes communicate synchronously using pipes. Ensure that one process waits for the other to finish before proceeding.
```c
#include<stdio.h>
#include<unistd.h>
#include<string.h>
int main()
{
        int p1[2],p2[2];
        pipe(p1);
        pipe(p2);
        if(fork()==0)
        {
                char buf[50];
                read(p1[0],buf,sizeof(buf));
                printf("child got:%s\n",buf);
                write(p2[1],"ACK",4);
        }
        else
        {
                write(p1[1],"Hello",6);
                char ack[10];
                read(p2[0],ack,sizeof(ack));
                printf("parent got:%s\n",ack);
        }
        return 0;
}
```
## 3.Implement a program that uses Named pipes for communication between two processes.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<sys/stat.h>
#include<string.h>
int main()
{
        char *fifo="/tmp/myfifo";
        mkfifo(fifo,0666);
        if(fork()==0)
        {
                int fd=open(fifo,O_WRONLY);
                write(fd,"Hello FIFO",11);
                close(fd);
        }
        else
        {
                char buf[50];
                int fd=open(fifo,O_RDONLY);
                read(fd,buf,sizeof(buf));
                printf("Reader got:%s\n",buf);
                close(fd);
        }
        return 0;
}
```
## 4.Write a C program to create a message queue using the msgget system call. Ensure that the program checks for errors during the creation process.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,IPC_CREAT|0666);
        if(id==-1)
                perror("msgget");
        else
                printf("Message Queue created with ID:%d\n",id);
        return 0;
}
```
