## 1.Program to read and append contents of a file
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main()
{
        int fd,ret;
        char buf[1000];
        //Open file for reading only
        fd=open("file.txt",O_RDONLY);
        if(fd<0)
        {
                printf("Failed to open the file\n");
                return 0;
        }
        //read the file
        ret=read(fd,buf,50);
        if(ret<0)
        {
                printf("Failed to read the file\n");
                close(fd);
                return 1;
        }
        buf[ret]='\0';
        //String to append
        char bufa[]="e When a file is created,it is stored in hard disk.\n";
        //open file to append
        int fd1;
        fd1=open("file.txt",O_APPEND|O_WRONLY);
        if(fd1<0)
        {
                printf("Failer to open\n");
                return 0;
        }
        printf("Opened successfully\n");
        //appending to file
        int ret1;
        ret1=write(fd1,bufa,strlen(bufa));
        if(ret1<0)
        {
                printf("Failed to append\n");
                close(fd1);
                return 1;
        }
        printf("Written %d bytes to file\n",ret1);
        close(fd1);
        printf("Read content: %s",buf);
}
```
## 2.Develop a C program to open an existing text file and display its contents
```c
#include<stdio.h>
#include<sys/types.h>
#include<unistd.h>
#include<fcntl.h>
void main()
{
        int fd,ret;
        char buf[65];
        fd=open("file.txt",O_RDONLY);
        if(fd<0)
        {
                printf("Failed to open the file\n");
                return;
        }
        ret=read(fd,buf,64);
        if(ret<0)
        {
                printf("Failed to read the file\n");
                close(fd);
                return;
        }
        buf[ret]='\0';
        printf("%s\n",buf);
        close(fd);
        return;
}
```

