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
## 22.Write a c program to check whether the given path is file or not.
```c
#include<stdio.h>
#include<sys/stat.h>
int main()
{
        char path[256];
        struct stat path_stat;
        printf("Enter the path:");
        scanf("%s",path);
        if(stat(path,&path_stat)!=0)
        {
                perror("stat");
                return 1;
        }
        if(S_ISDIR(path_stat.st_mode))
        {
                printf("The path refers to a DIRECTORY.\n");
        }
        else if(S_ISREG(path_stat.st_mode))
        {
                printf("The path refers to a FILE.\n");
        }
        else
        {
                printf("The path refers to another type.\n");
        }
        return 0;
}
```
## 23.Develop a C program to create a hard link named "hardlink.txt" to a file named "source.txt".
```c
#include<stdio.h>
#include<unistd.h>
int main()
{
        const char *source="source.txt";
        const char *linkname="hardlink.txt";
        if(link(source,linkname)==0)
        {
                printf("Hard link created successfully.\n");
        }
        else
        {
                perror("link");
        }
        return 0;
}
```
## 24.Implement a C program to read and display the contents of a CSV file named "data.csv"?
```c
#include<stdio.h>
#include<stdlib.h>
#include<fcntl.h>
#include<unistd.h>
#include<errno.h>
int main()
{
        int fd;
        fd=open("data.csv",O_RDONLY|O_CREAT,0666);
        if(fd<0)
        {
                perror("oprn system call failed\n");
                return 1;
        }
        char buffer[1024];
        int n;
        while((n=read(fd,buffer,sizeof(buffer)-1))>0)
        {
                buffer[n]='\0';
                printf("%s",buffer);
        }
        close(fd);
        return 0;
}
```
## 25.Write a C program to get the absolute path of the current working directory.
```c
#include<stdio.h>
#include<unistd.h>
#include<limits.h>
int main()
{
        char cwd[PATH_MAX];
        if(getcwd(cwd,sizeof(cwd))!=NULL)
        {
                printf("Current working directory:%s\n",cwd);
        }
        else
        {
                perror("getcwd");
        }
        return 0;
}
```
## 26.Develop a C program to get the size of a directory.
```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<dirent.h>
#include<sys/stat.h>
long get_dir_size(const char *path)
{
        DIR *dir;
        struct dirent *entry;
        struct stat info;
        char fullpath[1024];
        long total_size=0;
        dir=opendir(path);
        if(!dir)
                return 0;
        while((entry=readdir(dir))!=NULL)
        {
                if(strcmp(entry->d_name,".")==0 || strcmp(entry->d_name, "..")==0)
                        continue;
                snprintf(fullpath,sizeof(fullpath),"%s/%s",path,entry->d_name);
                if(stat(fullpath,&info)==0)
                {
                        if(S_ISDIR(info.st_mode))
                                total_size+=get_dir_size(fullpath);
                        else
                                total_size+=info.st_size;
                }
        }
        closedir(dir);
        return total_size;
}
int main()
{
        const char *dir="Docunents";
        long size=get_dir_size(dir);
        printf("Total size of directory %s: %ld bytes\n",dir,size);
        return 0;
}
```

## 27.Implement a C program to recursively copy all files and directories from on directory to another.
```c
#include<stdlib.h>
#include<string.h>
#include<sys/stat.h>
#include<dirent.h>
#include<unistd.h>
void copy_file(const char *src,const char *dest)
{
        FILE *fs=fopen(src,"rb");
        FILE *fd=fopen(dest,"wb");
        if(!fs||!fd)
                return;
        char buf[1024];
        size_t n;
        while((n=fread(buf,1,sizeof(buf),fs))>0)
                fwrite(buf,1,n,fd);
        fclose(fs);
        fclose(fd);
}
void copy_dir(const char *src,const char *dest)
{
        mkdir(dest,0755);
        DIR *dir=opendir(src);
        if(!dir)
                return;
        struct dirent *entry;
        char src_path[1024],dest_path[1024];
        while((entry=readdir(dir)))
        {
                if(!strcmp(entry->d_name,".")||!strcmp(entry->d_name,".."))
                        continue;
                snprintf(src_path,sizeof(src_path),"%s/%s",src,entry->d_name);
                snprintf(dest_path,sizeof(dest_path),"%s/%s",dest,entry->d_name);

                struct stat st;
                stat(src_path,&st);
                if(S_ISDIR(st.st_mode))
                {
                        copy_dir(src_path,dest_path);
                }
                else
                {
                        copy_file(src_path,dest_path);
                }
        }
        closedir(dir);
}
int main()
{
        const char *src="source_dir";
        const char *dest="dest_dir";
        copy_dir(src,dest);
        printf("Directory copied successfully.\n");
        return 0;
}
```
## 28.Write a C program to get the number of files in a directory
```c
#include<stdio.h>
#include<dirent.h>
#include<string.h>
int main()
{
        DIR *dir=opendir(".");
        if(!dir)
        {
                perror("opendir");
                return 1;
        }
        struct dirent *entry;
        int count=0;
        while((entry=readdir(dir))!=NULL)
        {
                if(entry->d_type==DT_REG)
                        count++;
        }
        closedir(dir);
        printf("Number of files in directory are:%d\n",count);
        return 0;
}
```
## 29.Implement a C program to read data from a FIFO named "myfifo".
```c
#include<stdio.h>
#include<sys/stat.h>
int main(void)
{
        const char *fifo="myfifo";
        if(mkfifo(fifo,0666)==0)
        {
                printf("FIFo '%s' created successfully.\n",fifo);
        }
        else
        {
                perror("mkfifo");
                return 1;
        }
        return 0;
}
```
## 30.Write a C program to truncate a file named "file.txt" to a specified length?
### Reader:
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
int main(void)
{
        const char *fifo="myfifo";
        int fd=open(fifo,O_RDONLY|O_CREAT,0666);
        if(fd==-1)
        {
                perror("open");
                return 1;
        }
char buf[1024];
ssize_t n;
while((n=read(fd,buf,sizeof(buf)-1))>0)
{
        buf[n]='\0';
        printf("%s",buf);
}
if(n==-1)
        perror("read");
        close(fd);
        return 0;
}
```
### Writer:
```c
#include<stdio.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int main(void)
{
        const char *fifo="myfifo";
        int fd=open(fifo,O_WRONLY);
        if(fd==-1)
        {
                perror("open");
                return 1;
        }
        const char *msg="This is my message from eriter.\n";
        write(fd,msg,strlen(msg));
        close(fd);
        return 0;
}
```
## 31.Develop a C program to search for a specific string in a file named "data.txt" and display the line number(s) where it occurs?
```c
#include<stdio.h>
#include<string.h>
int main(void)
{
        const char *fname="hardlink.txt";
        const char *pattern="search";
         FILE *fp=fopen(fname,"r");
         if(!fp)
         {
                 perror("fopen");
                 return 1;
         }
         char line[4096];
         int lineno=0;
         int found=0;
         while(fgets(line,sizeof(line),fp))
         {
                 lineno++;
                 if(strstr(line,pattern))
                 {
                         printf("Found on line %d:%s",lineno,line);
                         found=1;
                 }
         }
         if(!found)
                 printf("'%s' not found in %s\n",pattern,fname);
         fclose(fp);
         return 0;
}
```
## 32.Implement a C program to get the file type (regular file, directory, symbolic link, etc.) of a given path?
```c

