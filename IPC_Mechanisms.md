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

## 5.Develop two separate C programs, one for sending messages and the other for receiving messages through a created message queue.
### Sender:
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
#include<string.h>
struct msg
{
        long type;
        char text[50];
};
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,IPC_CREAT|0666);
        struct msg m={1,"Hello from sender"};
        msgsnd(id,&m,sizeof(m.text),0);
        return 0;
}
```
### Receiver:
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
struct msg
{
        long type;
        char text[50];
};
int main()
{
        key_t key=ftok(".",'A');
        int id=msgget(key,0666);
        struct msg m;
        msgrcv(id,&m,sizeof(m.text),1,0);
        printf("Receieved:%s\n",m.text);
        return 0;
}
```
## 6.Create a program to remove an existing message queue using the msgctl system call.Ensure that the program prompts the user for confirmation before deleting the message queue.
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/msg.h>
int main()
{
        key_t key;
        int msqid;
        char choice;

        key=ftok(".",'A');
        if(key==-1)
        {
                perror("ftok");
                return 1;
        }

        msqid=msgget(key,0666);
        if(msqid==-1)
        {
                perror("msgget");
                return 1;
        }

        printf("Message Queue ID:%d\n",msqid);
        printf("Do you really want to delete this message queue?(y/n): ");
        scanf(" %c",&choice);

        if(choice=='y'||choice=='Y')
        {
                if(msgctl(msqid,IPC_RMID,NULL)==-1)
                {
                        perror("msgctl");
                        return 1;
                }
        printf("Message queue deleted successfully.\n");
        }
        else
        {
                printf("Deletion cancelled.\n");
        }
        return 0;
}
```
## 7.Design a multithreaded program where threads communicate through named pipes.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<fcntl.h>
#include<unistd.h>
#include<sys/stat.h>
#include<string.h>

#define FIFO_NAME "mkfifo"

void* writer_thread(void* arg)
{
        int fd=open(FIFO_NAME,O_WRONLY);
        char msg[]="Hello from writer";

        write(fd, msg, strlen(msg)+1);
        printf("Writer:sent message\n");

        close(fd);
        return NULL;
}

void *reader_thread(void* arg)
{
        int fd=open(FIFO_NAME, O_RDONLY);
        char buffer[100];

        read(fd,buffer,sizeof(buffer));
        printf("Reader:received message:%s\n",buffer);

        close(fd);
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        mkfifo(FIFO_NAME,0666);
        pthread_create(&t1,NULL,writer_thread,NULL);
        sleep(1);
        pthread_create(&t2,NULL,reader_thread,NULL);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        unlink(FIFO_NAME);
        return 0;
}
```
## 8.Write a C program where two processes communicate using message queues.Implement sending and receiving messages between the processes using msgget,msgsnd, and msgrcv.
### sender:
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
    key_t key;
    int msgid;

    key = ftok(".", 'A');
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    msgid = msgget(key, IPC_CREAT | 0666);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    struct msg_buffer message;
    message.msg_type = 1;
    strcpy(message.msg_text, "Hello from sender process!");

    if (msgsnd(msgid, &message, sizeof(message.msg_text), 0) == -1) {
        perror("msgsnd");
        return 1;
    }

    printf("Sender: Message sent successfully.\n");
    return 0;
}
```
### receiver:
```c
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

struct msg_buffer {
    long msg_type;
    char msg_text[100];
};

int main() {
    key_t key;
    int msgid;

    key = ftok(".", 'A');
    if (key == -1) {
        perror("ftok");
        return 1;
    }

    msgid = msgget(key, 0666);
    if (msgid == -1) {
        perror("msgget");
        return 1;
    }

    struct msg_buffer message;

    if (msgrcv(msgid, &message, sizeof(message.msg_text), 1, 0) == -1) {
        perror("msgrcv");
        return 1;
    }

    printf("Receiver: Message received: %s\n", message.msg_text);
    msgctl(msgid, IPC_RMID, NULL);

    return 0;
}
```

