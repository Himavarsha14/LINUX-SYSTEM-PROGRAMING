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
## 3.Implement a C program to create a new directory named "Test" int the current directory.
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
        char *args[]={"mkdir","Test",NULL};
        execv("/bin/mkdir",args);
        perror("execv failed\n");
        return 0;
}
```
## 4.Write a C program to check if a file named "sample.txt" exists in the current directory.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
        int fd;
        fd=open("sample.txt",O_RDONLY);
        if(fd==-1)
        {
                printf("File 'sample.txt' does not exist.\n");
        }
        else
        {
                printf("File 'sample.txt' exists.\n");
                close(fd);
        }
        return 0;
}
```
## 5.Write a C program to rename file.
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
        if(rename("hello.c","helloworld.c")==0)
        {
                printf("File renamed successfully.\n");
        }
        else
        {
                printf("Error renaming file.");


        }
        return 0;
}
```
## 6.Implement a C program to delete a file.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
int main()
{
        if(unlink("file.txt")==0)
        {
                printf("File deleted successfully.\n");
        }
        else
        {
                printf("File deletion failed.\n");
        }
        return 0;
}
```
## 7.Implement a C program to copy the contents of one file to another.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<stdlib.h>
int main()
{
        int src,dest;
        char buf[24];
        int n;

        src=open("hello.txt",O_RDONLY);
        if(src<0)
        {
                printf("Failed to open.\n");
        }
        dest=open("file.txt",O_WRONLY | O_CREAT |O_TRUNC,0644);
        if(dest==-1)
        {
                printf("Error opening destination file.\n");
                close(src);
        }
        while((n=read(src,buf,sizeof(buf)))>0)
                {
                        write(dest,buf,n);
                }
        printf("File copied successfuly\n");
        close(src);
        close(dest);
        return 0;
}
```






