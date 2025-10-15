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

## 18.Explain the role of MMU in memory management.
- The Memory Management Unit is a hardware component responisble for all handling memory and address translations between virtual addresses used by programs and physical adress in RAM.Its key role inclue:
- Translating virtual addresses to physical addresses.
- enforcing memory protection and access permissions
- Supporting virtual memory by enabling paging and segmentation.
- Handling page faults and triggering interrupts when needed.

## 19.Describe the translation lookaside buffer (TLB).
- A TLB is a small, fast cache inside the MMU that stores recent virtual-to-physival address translations.It seepds up memory access by avoiding the need to traverse the page table for every memory reference.

## 20.What is TLB Miss? How Is It Handled?
- A TLB miss occurs when the requested vitual address is present in TLB.
## Handling:
- The MMU searches the page table for the address translation.
- If found,the translation is loaded into the TLB for future use.
- If the page is not in memory, a page fault occurs and the operating system loads the page from the secondary storage.

## 21.Discuss the working principle of MMU.
The MMU works as follows:
- The CPU generates a virtual address.
- MMU check the TLB for the translation
    - If found->physical address is used directly.
    - If not found->page table is consulted.
- The MMU enforces access permissions (read/write/execute).
- If a page is missing->page fauly is triggered and OS handles it.

## 22.Address translation in MMU?
Address translation is the process of converting a programs's virtual address to a physical address using:
- Segmentation:Divides memory into segments(code, data, stack).
- Paging:Divided memory into fixed-size pages mapped to frames.
- TLB:Speeds up translation by caching recent mappings.

## 23.How does MMU support virtual memory?
The MMU supports virtual memory by:
- Allowing programs to use addresses larger than physical memory.
- Using paging or segmentation to map virtual addresses to physical memory.
- Handling page faults when pages are not in RAM and swapping them in from    disk.

## 24.Describe the process of page table traversal in MMU.
When a virtual address is not in the TLB:
- MMU consults the page table stored in memory.
- The virtual page number is used as an index.
- The corresponding physical frame number is retrieved.
- The full physical address is formed using the frame number+page offset.

## 25.What is page fault handling in MMU?
A page fault occurs when the requested page is not in RAM.
Steps for handling:
- CPU triggers a page fault interrupt.
- OS identifies the required page on disk.
- OS loads the page into a free frame (or replaces a page using a replacement algorithm).
- Page table is updates and execution resumes.

## 26.Explain the page replacement algorithms used in MMU.
When memory is full and a new page needs to be loaded, the MMU uses page replacement algorithms to decide which page to evict.Common Algorithms:
- FIFO (Fisrt-In-First-Out)
- LRU(Least Recently Used)
- Optimal Page Replacement
- Clock Algorithm

## 27.Define page replacement algorithms.
- Page replacement algorithms determine which page to remove from memory when a page fault occurs, aiming to minimize future page faults and optimize memory usage.

## 28.Describe the FIFO page replacement algorithm.
FIFO(First-In-First-Out):
- Evicts the oldest page in memory when a new page is needed.
- Simple to implemet using a queue
- Limitation:May remove frequently used pages leading to Bleady's anomaly.

## 29.Explain the LRU (Least Recently Used) page replacement algorithm.
The Least Recently Used (LRU) page replacement algorithm replaces the page that has not been used for the longest time.
- Assumption: Pages used recently will likely be used again soon, and those not used for a long time are less likely to be accessed.
- Implementation methods:
     - Counter-based: keep a timestamp for each page and replace the one                           with oldest timestamp.
     - Stack-based: Keep pages in a stack; When accessed, move the page to the top,the bottom page is replaced.

## 30.Explain Clock page replacement algorithm
The clock algorithm is an efficient approximation of LRU.
- Pages are arranges in a circular list(like a clock).
- Each page has a reference bit(0 or 1).
- A "clock hand" pointer moves around the circle:
      - If the page's reference bit is 0,it is replaced.
      - If the reference bit is 1,it is cleared to 0 and the hand moves forward.
- This continues unitll a page with reference bit=0 is found.

## 31.Write a C program to allocate memory for a linked list node dynamically.
```c
#include<stdio.h>
#include<stdlib.h>
struct node
{
        int data;
        struct node *next;
};
int main()
{
        struct node *head=(struct node*)malloc(sizeof(struct node));
        if(!head)
        {
                printf("Memory allocation failed\n");
                return 1;
        }
        head->data=10;
        head->next=NULL;
        printf("Node created with data=%d\n",head->data);
        free(head);
        return 0;
}
```
## 32.Write a C program to simulate memory allocation using the first-fit algorithm.
```c
#include<stdio.h>
int main()
{
        int blocks[5]={100,500,200,300,600};
        int process[3]={212,417,112};
        int allocation[3]={-1,-1,-1};
        for(int i=0;i<3;i++)
        {
                for(int j=0;j<5;j++)
                {
                        if(blocks[j]>=process[i])
                        {
                                allocation[i]=j;
                                blocks[j]-=process[i];
                                break;
                        }
                }
        }
        for(int i=0;i<3;i++)
        {
                if(allocation[i]!=-1)
                {
                        printf("P%d allocated to block %d\n",i+1,allocation[i]+1);
                }
                else
                {
                        printf("P%d Not allocated\n",i+1);
                }
        }
}
```
## 33.Write a C program to simulate memory allocation using the best-fit algorithm.
```c
#include<stdio.h>
int main()
{
        int blocks[5]={100,500,200,300,600};
        int process[3]={212,417,112};
        int allocation[3]={-1,-1,-1};
        for(int i=0;i<3;i++)
        {
                for(int j=0;j<5;j++)
                {
                        if(blocks[j]>=process[i])
                        {
                                allocation[i]=j;
                                blocks[j]-=process[i];
                                break;
                        }
                }
        }
        for(int i=0;i<3;i++)
        {
                if(allocation[i]!=-1)
                {
                        printf("P%d allocated to block %d\n",i+1,allocation[i]+1);
                }
                else
                {
                        printf("P%d Not allocated\n",i+1);
                }
        }
}
```
## 34.Write a c program to simulate memory allocation using the worst-fit algorithm.
```c
#include<stdio.h>
int main()
{
        int blocks[5]={100,500,200,300,600};
        int process[3]={212,417,112};
        int allocation[3]={-1,-1,-1};
        for(int i=0;i<3;i++)
        {
                int worst=-1;
                for(int j=0;j<5;j++)
                {
                        if(blocks[j]>=process[i])
                        {
                                if(worst==-1 || blocks[j]>blocks[worst])
                                        worst=j;
                        }
                }
                if(worst!=-1)
                {
                        allocation[i]=worst;
                        blocks[worst]-=process[i];
                }
        }
        for(int i=0;i<3;i++)
        {
                if(allocation[i]!=-1)
                        printf("p%d allocated to block %d\n",i+1,allocation[i]+1);
                else
                        printf("p%d not allocated\n",i+1);
        }
}
```
## 35.Write a c program to simulate memory allocation using the next-fit algorithm.
```c
#include<stdio.h>
int main()
{
        int blocksize[10],processsize[10],blockcount,processcount;
        int allocation[10],lastpos=0;
        printf("Enter number of block:");
        scanf("%d",&blockcount);
        printf("Enter block sizes:\n");
        for(int i=0;i<blockcount;i++)
        {
                scanf("%d",&blocksize[i]);
        }
        printf("Enter number of processes:");
        scanf("%d",&processcount);
        printf("Enter process sizes:\n");
        for(int i=0;i<processcount;i++)
        {
                scanf("%d",&processsize[i]);
                allocation[i]=-1;
        }
        for(int i=0;i<processcount;i++)
        {
                int start=lastpos;
                int j=start;
                do
                {
                        if(blocksize[j]>=processsize[i])
                        {
                                allocation[i]=j;
                                blocksize[j]-=processsize[i];
                                lastpos=j;
                                break;
                        }
                        j=(j+1)%blockcount;
                }
                while(j!=start);
        }
        printf("\nprocess no.\t process size\t block no.\n");
        for(int i=0;i<processcount;i++)
        {
                printf("%d\t\t%d\t\t",i+1,processsize[i]);
                if(allocation[i]!=-1)
                        printf("%d\n",allocation[i]+1);
                else
                        printf("Not allocated\n");
        }
        return 0;
}
```
## 36.Write a c program to implement a simple memory allocator using the buddy system.
```c
#include<stdio.h>
#include<math.h>
int main()
{
        int total=1024;
        int req;
        printf("Enter memory request:");
        scanf("%d",&req);
        int size=1;
        while(size<req)
                size*=2;
        if(size<=total)
        {
                printf("Allocated %d bytes using budddy block of size %d\n",req,size);
                total-=size;
        }
        else
        {
                printf("Not enough memory\n");
        }
        return 0;
}
```
## 37.Develop a C program to implement a memory allocator using custom memory management algorithm.
```c
#include<stdio.h>
#include<stdlib.h>
#define MEM_SIZE 100
char memory[MEM_SIZE];
int used=0;
void *myalloc(int size)
{
        if(used+size>MEM_SIZE)
                return NULL;
        void* ptr=&memory[used];
        used+=size;
        return ptr;
}
int main()
{
        char *p=(char*)myalloc(20);
        if(p)
                printf("Allocated 20 bytes\n");
        else
                printf("Allocation failed\n");
        return 0;
}
```

## 38.Write a C program to demonstrate memory mapping using mmap().
```c
#include<stdio.h>
#include<sys/mman.h>
#include<unistd.h>
#include<string.h>
int main()
{
        char *ptr=mmap(NULL,4096,PROT_READ|PROT_WRITE,MAP_PRIVATE|MAP_ANONYMOUS,-1,0);
        if(ptr==MAP_FAILED)
        {
                perror("mmap");
                return 1;
        }
        strcpy(ptr,"HELLO from mmap");
        printf("%s\n",ptr);
        munmap(ptr,4096);
        return 0;
}
```
## 31.Implement a C program to read from and write to a memory-mapped file.
```c
#include<stdio.h>
#include<sys/mman.h>
#include<fcntl.h>
#include<unistd.h>
#include<string.h>
int main()
{
        int fd=open("test.txt",O_RDWR|O_CREAT,0666);
        ftruncate(fd,100);
        char *ptr=mmap(NULL,100,PROT_READ|PROT_WRITE,MAP_SHARED,fd,0);
        strcpy(ptr,"Memory mapped file content");
        msync(ptr,100,MS_SYNC);
        printf("Read:%s\n",ptr);
        munmap(ptr,100);
        close(fd);
        return 0;
}
```
## 32.Develop a C program to demonstrate shared memory segment and synchronize access using shmget and shmat().
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
#include<string.h>
int main()
{
        key_t key=1234;
        int shmid=shmget(key,1024,0666|IPC_CREAT);
        char *str=(char *)shmat(shmid,NULL,0);
        strcpy(str,"HELLO shared memory");
        printf("Data written\n");
        shmdt(str);
        return 0;
}
```
## Reader:
```c
#include<stdio.h>
#include<sys/ipc.h>
#include<sys/shm.h>
int main()
{
        key_t key=1234;
        int shmid=shmget(key,1024,0666);
        char *str=(char*)shmat(shmid,NULL,0);
        printf("Read:%s\n",str);
        shmdt(str);
        shmctl(shmid,IPC_RMID,NULL);
        return 0;
}
```
## 33.Implement a C program to simulate page replacement algorithm FIFO.
```c
#include<stdio.h>
int main()
{
        int pages[5]={1,2,3,4,1};
        int frame[3]={-1,-1,-1},n=5,f=3,front=0,faults=0;
        for(int i=0;i<n;i++)
        {
                int found=0;
                for(int j=0;j<f;j++)
                {
                        if(frame[j]==pages[i])
                        {
                                found=1;
                                break;
                        }
                }
                if(!found)
                        {
                                frame[front]=pages[i];
                                front=(front+1)%f;
                                faults++;
                        }
                        printf("Frames:");
                        for(int j=0;j<f;j++)
                                printf("%d ",frame[j]);
                        printf("\n");
                }
                printf("Page faults:%d\n",faults);
                return 0;
        }
```




