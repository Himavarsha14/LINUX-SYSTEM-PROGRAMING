## 1.C program to demonstrate dynamic memory allocation using malloc
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
        int n;
        scanf("%d",&n);
        int *arr;
        arr=(int *)malloc(n*sizeof(int));
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        for(int i=0;i<n;i++)
        {
                printf("%d ",arr[i]);
        }
        free(arr);
        return 0;
}
```
## 2.Program to demonstrate dynamic memory allocation using calloc
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
        int n;
        scanf("%d",&n);
        int *arr;
        arr=calloc(n,sizeof(int));
        //calloc initializes memory with zeroes
        printf("Contents of memory before giving the input:\n");
        for(int i=0;i<n;i++)
        {
                printf("%d ",arr[i]);
        }
        printf("\n");
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        printf("Contents of memory after giving input:\n");
        for(int i=0;i<n;i++)
        {
                printf("%d ",arr[i]);
        }
        free(arr);
        return 0;
}
```
## 3.Program to demonstrate resize dynamically allocated memory
```c
#include<stdio.h>
#include<stdlib.h>
int main()
{
        int n;
        scanf("%d",&n);
        int *arr;
        arr=(int *)malloc(n*sizeof(int));
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        for(int i=0;i<n;i++)
        {
                printf("%d ",arr[i]);
        }
        printf("\n");
        int newsize;
        scanf("%d",&newsize);
        arr=(int *) realloc(arr,newsize*sizeof(int));
        if(newsize>n)
        {
        printf("Enter the elements:\n");
        for(int i=n;i<newsize;i++)
        {
                scanf("%d",&arr[i]);
        }
        }
        printf("Elements after resizing the array:\n");
        for(int i=0;i<newsize;i++)
        {
                printf("%d ",arr[i]);
        }
        free(arr);
        return 0;
}
```
## 4.What is memory management in system programming?
- Memory management is the process of efficiently handling the computer’s memory resources. In system programming, it involves allocating memory to processes, keeping track of which parts of memory are in use, and freeing memory when it is no longer needed. It ensures that multiple programs can run smoothly without interfering with each other, preventing memory leaks and crashes.


