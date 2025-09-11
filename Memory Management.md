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
  
## 5.Define Virtual Memory
- Virtual memory is a memory management technique that gives an application the illusion of having a large, continuous memory space, while physically it may be smaller and fragmented. It uses a combination of RAM and disk storage.
  
## 6.Differentiate between Physical Memory and Virtual Memory
- Physical Memory: Actual RAM installed in the system. Limited and directly accessed by the CPU.
- Virtual Memory: Logical abstraction provided by the OS, combining RAM and disk to extend usable memory.

## 7.Role of an Operating System in Memory Management
The OS manages memory by:
- Allocating memory to processes.
- Keeping track of memory usage.
- Ensuring isolation and protection between processes.
- Handling swapping between RAM and disk.
- Managing fragmentation using techniques like paging and segmentation.
  
## 8.Purpose of Memory Allocation
- Memory allocation ensures that processes get the required memory for execution, enabling efficient CPU utilization and avoiding conflicts between processes.
  
## 9.Significance of Memory Deallocation
- Deallocation frees memory that is no longer needed, preventing memory leaks and ensuring efficient reuse of system resources.

## 10.Define Fragmentation in Memory Management
- Fragmentation occurs when memory is allocated and freed in such a way that available memory is split into small, unusable pieces.

## 11.Types of Fragmentation
- Internal Fragmentation
- External Fragmentation
- Internal Fragmentation
     Happens when allocated memory is slightly larger than requested, leaving unused space inside a block.
- External Fragmentation
     Occurs when free memory is divided into small scattered blocks, preventing allocation of large continuous memory even    though total free space is sufficient.
## 12.How Fragmentation is Managed in Memory Allocation
- Compaction: Rearranging memory to create larger contiguous free blocks.
- Paging: Divides memory into fixed-size pages to avoid external fragmentation.
- Segmentation with Paging: Reduces both internal and external fragmentation.

## 13.Concept of Paging
Paging divides memory into fixed-size blocks:
Pages (logical units of a process)
Frames (physical units in memory)
The OS maps pages to frames using a page table, avoiding external fragmentation.

## 14.Segmentation
- Segmentation divides memory into variable-sized segments based on logical divisions like code, stack, heap, data, etc. Each segment has its own base and limit address.

## 15.Difference between Paging and Segmentation
- Paging: Fixed-size blocks, removes external fragmentation.
- Segmentation: Variable-sized blocks, matches program’s logical structure.

## 16.Define Page Table
- A page table is a data structure used by the OS to map virtual addresses (pages) to physical addresses (frames) in memory.

## 17.Define Memory Management
- Memory management is the process of controlling and coordinating computer memory, including allocation, deallocation, protection, and efficient utilization of both physical and virtual memory.

