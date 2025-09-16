## 1.Write a C program to create a thread that prints Hello world.
```c
#include <stdio.h>
#include <pthread.h>
void *helloworld(void *args);
int main(void)
{
    pthread_t t1;
    pthread_create(&t1, NULL, helloworld, NULL);
    pthread_join(t1, NULL);
    return 0;
}
void *helloworld(void *args)
{
    printf("Hello world\n");
    return NULL;
}
```
## 2.Modify the above program to create multiple threads, each printing its own message.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
void *thread1(void *args)
{
        printf("Hello\n");
        sleep(1);
        return NULL;
}
void *thread2(void *args)
{
        printf("I am learning linux\n");
        sleep(1);
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        //to create threads
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## Program to create two threads that prints numbers from 1 to 10 concurrently.
```c
#include<stdio.h>
#include<unistd.h>
#include<stdlib.h>
#include<pthread.h>
void *thread1(void *args)
{
        int i=1;
        for(i=1;i<=10;i++)
        {
                printf("Thread1:%d\n",i);
                sleep(1);
        }
        return NULL;
}
void *thread2(void *args)
{
        int i=1;
        for(i=1;i<=10;i++)
        {
                printf("Thread2:%d\n",i);
                sleep(1);
        }
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 4.Implement a thread that calculates the factorial of a number
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
void *factorial(void *args)
{
        int n= *(int *)args;
        long long int fact=1;
        for(int i=1;i<=n;i++)
        {
                fact=fact*i;
        }
        long long *result=(long long *)malloc(sizeof(long long));
        *result=fact;
        return result;
}
int main()
{
        pthread_t ti;
        int num;
        printf("enter a number to find factorial:\n");
        scanf("%d",&num);
        void *res;

        pthread_create(&ti,NULL,factorial,&num);
        pthread_join(ti,&res);

        long long *factresult=(long long*)res;

        printf("factorial of %d is %lld:\n",num,*factresult);
        free(factresult);
        return 0;
}
```
## 5.Write a program to create two threads that print their thread ids.
```c
#include<stdio.h>
#include<unistd.h>
#include<pthread.h>
void *thread1(void *args)
{
        printf("hello\n");
        return NULL;
}
void *thread2(void *args)
{
        printf("world\n");
        return NULL;
}
int main()
{
        pthread_t t1,t2;

        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);

        printf("thread1 identifier:%lu\n",t1);
        printf("thread2 identifier:%lu\n",t2);

        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 6.Write a program to create a thread that prints sum of two numbers.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
struct numbers
{
        int a;
        int b;
};
void *add(void *args)
{
        struct numbers *nums=(struct numbers *)args;
        int sum=nums->a+nums->b;
        printf("sum of %d and %d is: %d\n",nums->a,nums->b,sum);
        return NULL;
}
int main()
{
        pthread_t t1;
        struct numbers *n=malloc(sizeof(struct numbers));
        n->a=10;
        n->b=20;
        pthread_create(&t1,NULL,add,n);
        pthread_join(t1,NULL);
        free(n);
}
```
## 7.Implement a C programs that creates a thread that prints square of a number.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

void *square(void *args) {
    int num = *(int *)args;
    int sq = num * num;

    int *result = (int *)malloc(sizeof(int));
    *result = sq;

    return result;
}

int main() {
    pthread_t t1;
    int n;

    printf("Enter a number:\n");
    scanf("%d", &n);

    void *res;
    pthread_create(&t1, NULL, square, &n);
    pthread_join(t1, &res);

    int *squareresult = (int *)res;
    printf("Square of %d is %d\n", n, *squareresult);

    free(squareresult);
    return 0;
}
```
## 8.Write a c program that creates a thread and  prints current date and time.
```c
#include<stdio.h>
#include<stdlib.h>
#include<pthread.h>
#include<time.h>
void *printDateTime(void *args)
{
        time_t t;
        struct tm *current_time;
        time(&t);
        current_time=localtime(&t);
        printf("Current date and time:%s",asctime(current_time));
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,printDateTime,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 9.Write a c program that creates a thread that calculates given number is prime or not.
```c
#include<stdio.h>
#include<stdlib.h>
#include<unistd.h>
#include<pthread.h>
void *prime(void *args)
{
        int num=*(int *)args;
        int isprime=1;
        for(int i=2;i<num;i++)
        {
                if(num%i==0)
                {
                        isprime=0;
                        break;
                }
        }
        if(isprime)
        {
                printf("The given number is a prime number\n");
        }
        else
        {
                printf("The given number is not a prime number\n");
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        int n;
        scanf("%d",&n);
        pthread_create(&t1,NULL,prime,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 10.Implement a C program that checks whether a string is a palindrome or not using threads.
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>

void *checkPalindrome(void *args) {
    char *str = (char *)args;
    int len = strlen(str);
    int *result = (int *)malloc(sizeof(int));

    *result = 1;

    for (int i = 0; i < len / 2; i++) {
        if (str[i] != str[len - i - 1]) {
            *result = 0;
            break;
        }
    }

    return result;
}

int main() {
    pthread_t t1;
    char str[100];

    printf("Enter a string: ");
    scanf("%s", str);  // input string (no spaces)

    void *res;
    pthread_create(&t1, NULL, checkPalindrome, str);
    pthread_join(t1, &res);

    int *isPalindrome = (int *)res;
    if (*isPalindrome)
        printf("The string '%s' is a palindrome.\n", str);
    else
        printf("The string '%s' is not a palindrome.\n", str);

    free(isPalindrome);
    return 0;
}
```
## 11.Write a C program to create a thread that prints Hello world with thread synchronisation.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
void *thread(void *arg)
{
        pthread_mutex_lock(&mtx);
        printf("Hello world\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
}
```
## 12.Develop a C program that creates two threads that prints their IDs and synchronize their outputs.
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
void *thread1(void *args)
{
        pthread_mutex_lock (&mtx);
        printf("Hello\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void *thread2(void *args)
{
        pthread_mutex_lock(&mtx);
        printf("World\n");
        pthread_mutex_unlock(&mtx);
        return NULL;
}
void main()
{
        pthread_t t1,t2;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);
        printf("First thread identifier: %lu\n",t1);
        printf("Second thread identifier: %lu\n",t2);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&mtx);
}
```
## 13.Implement a C program to create a thread that generates random numbers and synchronizes access to a shared buffer.
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
#include<time.h>
#define SIZE 10
int buff[SIZE];
int k=0;
pthread_mutex_t mtx;
void *thread(void *arg)
{
        srand(time(NULL));
        for(int i=0;i<SIZE;i++)
        {
                pthread_mutex_lock(&mtx);
                int num=rand()%100;
                buff[k++]=num;
                printf("%d\n",num);
                pthread_mutex_unlock(&mtx);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 14.Write a C program to create a thread that performs addition of two numbers with mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
#include<unistd.h>
#include<stdlib.h>
struct numbers
{
        int a;
        int b;
};
pthread_mutex_t mtx;
void *add(void *args)
{
        struct numbers *nums=(struct numbers *)args;
        pthread_mutex_lock(&mtx);
        int sum=nums->a+nums->b;
        printf("sum of %d and %d is: %d\n",nums->a,nums->b,sum);
        pthread_mutex_unlock(&mtx);
        return NULL;
}
int main()
{
        pthread_t t1;
        struct numbers *n=malloc(sizeof(struct numbers));
        n->a=10;
        n->b=20;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,add,n);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        free(n);
}
```
## 15.Implement a C program to create two threads that increment and decrement a shared variable,respectively,using mutex locks?
```c
#include<stdio.h>
#include<pthread.h>
int sh_var=0;
pthread_mutex_t mtx;
void *thread1(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var++;
                printf("the shared variable in incrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
void *thread2(void *args)
{
        int i=0;
        for(i=0;i<5;i++)
        {
                pthread_mutex_lock(&mtx);
                sh_var--;
                printf("The shared variable in decrementing thread:%d\n",sh_var);
                pthread_mutex_unlock(&mtx);
        }
}
int main()
{
        pthread_t t1,t2;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,thread1,NULL);
        pthread_create(&t2,NULL,thread2,NULL);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 16.Develop a C program to create two threads that reads input from the user and synchronizes access to shared resources?
```c
#include<stdio.h>
#include<pthread.h>
pthread_mutex_t mtx;
char str[100];
void *userinput(void *args)
{
        pthread_mutex_lock(&mtx);
        printf("Enter input:\n");
        fgets(str,100,stdin);
        printf("user input:%s",str);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,userinput,NULL);
        pthread_join(t1,NULL);
        printf("User given input in mainthread:%s",str);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```
## 17.Implement a C program to create a thread that prints prime numbers up to a given limit with mutex locks
```c
#include<stdio.h>
#include<pthread.h>
#include<math.h>
pthread_mutex_t mtx;
void *prime(void *args)
{
        printf("The prime numbers in the range are:\n");
        for(int n=2;n<10;n++)
        {
                int count=0;
                for(int i=2;i<=n/2;i++)
                {
                        if(n%i==0)
                        {
                                count++;
                                break;
                        }
                }
                if(count==0)
                {
                        pthread_mutex_lock(&mtx);
                        printf("%d\n",n);
                        pthread_mutex_unlock(&mtx);
                }
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_mutex_init(&mtx,NULL);
        pthread_create(&t1,NULL,prime,NULL);
        pthread_join(t1,NULL);
        pthread_mutex_destroy(&mtx);
        return 0;
}
```

