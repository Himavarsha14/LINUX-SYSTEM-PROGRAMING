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
## 8.C program to move a file from one directory to another
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main()
{
        if(rename("hello.txt","/home/Himavarsha/linux/file.txt")==0)
                printf("File moved successfully.\n");
        else
                printf("Error moving file.\n");
        return 0;
}
```
## 9.Program to list all files in the current directory
```c
#include<stdio.h>
#include<sys/types.h>
void main()
{
        printf("This is a program to implement list all files in current directory.\n");
        execl("/bin/ls","ls",NULL);
        printf("This print never appears\n");
}
```
## 10.Program to get the size of the file
```c
#include<stdio.h>
#include<sys/stat.h>
int main()
{
        struct stat st;
        if(stat("file.txt",&st)==0)
        {
                printf("size of file.txt=%ld bytes\n",st.st_size);
        }
        else
        {
                printf("Error getting file size.\n");
        }
        return 0;
}
```
## 11.Program to check if a directory "Test" exists
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    struct stat st;
    if (stat("Test", &st) == 0 && S_ISDIR(st.st_mode))
        printf("Directory 'Test' exists\n");
    else
        printf("Directory 'Test' does not exist\n");
    return 0;
}
```
## 12.Program to create a new directory "Backup" in parent directory
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    if (mkdir("../Backup", 0755) == 0)
        printf("Directory 'Backup' created in parent\n");
    else
        perror("mkdir");
    return 0;
}
```
## 13.Program to recursively list all files and directories in a given directory
```c
#include <stdio.h>
#include <dirent.h>
#include <string.h>
#include <sys/stat.h>

void listDir(const char *path) {
    DIR *dir = opendir(path);
    if (!dir) return;

    struct dirent *e;
    while ((e = readdir(dir)) != NULL) {
        if (strcmp(e->d_name, ".") == 0 || strcmp(e->d_name, "..") == 0)
            continue;

        char full[1024];
        snprintf(full, sizeof(full), "%s/%s", path, e->d_name);
        printf("%s\n", full);

        struct stat st;
        if (stat(full, &st) == 0 && S_ISDIR(st.st_mode))
            listDir(full);
    }
    closedir(dir);
}

int main() {
    listDir(".");
    return 0;
}
```
## 14.Program to delete all files in the directory temp
```c
#include <stdio.h>
#include <dirent.h>
#include <unistd.h>
#include <string.h>


int main() {
DIR *dir = opendir("Temp");
struct dirent *dp;
char filepath[1024];


if (!dir) {
perror("opendir");
return 1;
}


while ((dp = readdir(dir)) != NULL) {
if (strcmp(dp->d_name, ".") == 0 || strcmp(dp->d_name, "..") == 0)
continue;


snprintf(filepath, sizeof(filepath), "Temp/%s", dp->d_name);
if (unlink(filepath) == 0)
printf("Deleted: %s\n", filepath);
else
perror("unlink");
}
closedir(dir);
return 0;
}
```
## 15.Program to Count number of lines in a file data.txt
```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>

int main() {
    int fd = open("file.c", O_RDONLY);
    if (fd < 0)
    {
            printf("Error opening file.\n");
            return 1;
    }
    char buf[1024];
    int n, lines = 0;
    while ((n = read(fd, buf, sizeof(buf))) > 0) {
        for (int i = 0; i < n; i++)
        {
            if (buf[i] == '\n')
            {
                    lines++;
            }
        }
    }
    close(fd);
    printf("Lines: %d\n", lines);
    return 0;
}
```
## 16.Program to Append "Goodbye!" to "message.txt"
```c
#include <stdio.h>
#include <fcntl.h>
#include <unistd.h>
#include <string.h>

int main() {
    int fd = open("helloworld.txt", O_WRONLY | O_APPEND | O_CREAT,0666);
    if (fd < 0) { perror("open"); return 1; }

    write(fd, "Goodbye!\n", strlen("Goodbye!\n"));
    close(fd);
    return 0;
}
```
## 17.Program to change file permissions to read only mode
```c
#include <stdio.h>
#include <sys/stat.h>

int main() {
    if (chmod("file.txt", 0444) == 0)
        printf("Permissions changed to read-only\n");
    else
        perror("chmod");
    return 0;
}
```
## 18.Program to Change ownership of the file
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>
#include <pwd.h>

int main() {
    struct passwd *pw = getpwnam("user1");
    if (!pw) { printf("User not found\n"); return 1; }

    if (chown("file.txt", pw->pw_uid, pw->pw_gid) == 0)
        printf("Owner changed\n");
    else
        perror("chown");
    return 0;
}
```
## 19.Program to get last modified time stamp
```c
#include <stdio.h>
#include <sys/stat.h>
#include <time.h>

int main() {
    struct stat st;
    if (stat("file.txt", &st) == 0) {
        printf("Last modified: %s", ctime(&st.st_mtime));
    } else {
        perror("stat");
    }
    return 0;
}
```
## 20.Program to create temporary file and write data
```c
#include <stdio.h>
#include <unistd.h>
#include <string.h>

int main() {
    char tempName[] = "/tmp/tempfileXXXXXX";   // template for mkstemp
    int fd = mkstemp(tempName);                // create unique temp file

    if (fd < 0) {
        perror("Failed to create temp file");
        return 1;
    }

    char data[] = "This is temporary data\n";
    write(fd, data, strlen(data));             // write data to file

    printf("Temporary file created: %s\n", tempName);

    close(fd);                                 // close file
    return 0;
}
```
## 21.C program to create new file and write hello world! to it.
```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>
#include<string.h>
int main()
{
        int fd;
        fd=open("filefile.txt",O_RDONLY|O_WRONLY|O_CREAT,0666);
        if(fd<0)
        {
                printf("Failed to open file\n");
                return 1;
        }
        write(fd,"Hello world!",strlen("Hello world!"));
        close(fd);
        return 0;
}
```




