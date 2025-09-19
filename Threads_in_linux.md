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
## 18.Implement a C program to create a thread that calculates the sum of Fibonacci series up to a given limit using dynamic programming with mutex locks.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

pthread_mutex_t mtx;
int *fibs;
size_t count = 0;
size_t capacity = 10;
int sum = 0;
int limit = 10;

void *range_fib(void *args)
{
    int a = 0, b = 1, c;
    fibs = malloc(capacity * sizeof(int));
    if (!fibs) {
        printf("Malloc error\n");
        return NULL;
    }

    while (a <= limit) {
        if (count == capacity) {
            capacity *= 2;
            fibs = realloc(fibs, capacity * sizeof(int));
            if (!fibs) {
                printf("Realloc error\n");
                return NULL;
            }
        }
        pthread_mutex_lock(&mtx);
        fibs[count++] = a;
        sum += a;
        pthread_mutex_unlock(&mtx);

        c = a + b;
        a = b;
        b = c;
    }
    return NULL;
}

int main()
{
    pthread_t t1;
    pthread_mutex_init(&mtx, NULL);

    pthread_create(&t1, NULL, range_fib, NULL);
    pthread_join(t1, NULL);

    printf("Fibonacci numbers up to %d: ", limit);
    for (size_t i = 0; i < count; i++) {
        printf("%d ", fibs[i]);
    }
    printf("\nFinal sum = %d\n", sum);

    pthread_mutex_destroy(&mtx);
    free(fibs);
    return 0;
}
```
## 19.Write a C program to create a thread that checks if a given year is a leap year using dynamic programming with mutex locks.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
pthread_mutex_t mtx;
void *leap_year(void *args)
{
    int *year = malloc(sizeof(int));
    if (!year) {
        printf("Malloc error\n");
        return NULL;
    }
    *year = *(int *)args;
    int is_leap = (*year % 400 == 0) || ((*year % 100 != 0) && (*year % 4 == 0));
    pthread_mutex_lock(&mtx);
    printf("%d is %s leap year.\n", *year, is_leap ? "a" : "not a");
    pthread_mutex_unlock(&mtx);
    free(year);
    return NULL;
}

int main()
{
    pthread_t t1;
    int in_year;
    printf("Enter a year: ");
    scanf("%d", &in_year);
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t1, NULL, leap_year, &in_year);
    pthread_join(t1, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}
```
## 20.Write a C program to create a thread that checks if a string is palindrome using dynamic programming with mutex locks?
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <pthread.h>
pthread_mutex_t mtx;
void *check_palindrome(void *args) {
    char *str = malloc(100 * sizeof(char));
    if (!str) {
        printf("Malloc error\n");
        return NULL;
    }
    printf("Enter a string: ");
    scanf("%s", str);

    int n = strlen(str);
    int is_palindrome = 1;

    for (int i = 0; i < n / 2; i++) {
        if (str[i] != str[n - i - 1]) {
            is_palindrome = 0;
            break;
        }
    }
    pthread_mutex_lock(&mtx);
    if (is_palindrome)
        printf("\"%s\" is a palindrome.\n", str);
    else
        printf("\"%s\" is not a palindrome.\n", str);
    pthread_mutex_unlock(&mtx);

    free(str);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_mutex_init(&mtx, NULL);
    pthread_create(&t1, NULL, check_palindrome, NULL);
    pthread_join(t1, NULL);

    pthread_mutex_destroy(&mtx);
    return 0;
}
```
## 21.Implement a C program to create a thread that performs selection sort on an array of integers.
```c
#include<stdio.h>
#include<pthread.h>
void *sort(void *args)
{
        printf("enter number of elements:\n");
        int size;
        int i,j;
        scanf("%d",&size);
        int arr[size];
        printf("Enter elements of an array:\n");
        for(i=0;i<size;i++)
        {
                scanf("%d",&arr[i]);
        }
        for(i=0;i<size;i++)
        {
                int min=i;
                for(j=i+1;j<size;j++)
                {
                        if(arr[j]<arr[min])
                        {
                                min=j;
                        }
                }
                int temp=arr[i];
                arr[i]=arr[min];
                arr[min]=temp;
        }
        printf("Sorted array:");
        for(int i=0;i<size;i++)
        {
                printf("%d ",arr[i]);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,sort,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 22.Develop a C to create a thread that calculates area of a triangle.
```c
#include<stdio.h>
#include<pthread.h>
void *area_of_triangle(void *args)
{
        double height,breadth;
        printf("Enter height and breadth of a triangle:\n");
        scanf("%lf %lf",&height,&breadth);
        double area=0.5*height*breadth;
        printf("The area of the triangle is:%.2lf\n",area);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,area_of_triangle,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 23.Write a C program to create a thread that calculates the sum of squares of numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>
void *sum_of_squares(void *args) {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i * i;
    }
    printf("Sum of squares from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, sum_of_squares, NULL);
    pthread_join(t1, NULL);
    return 0;
}
```
## 24.Write a C program to create a thread that generated a random array of numbers.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>

void *generate_random_array(void *args) {
    int n;
    printf("Enter the size of the array: ");
    scanf("%d", &n);
    int arr[n];
    srand(time(NULL));

    printf("Random array: ");
    for (int i = 0; i < n; i++) {
        arr[i] = rand() % 100; // random number between 0-99
        printf("%d ", arr[i]);
    }
    printf("\n");
    return NULL;
}
int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, generate_random_array, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 25.Implement a C program to create a thread that performs bubble sortt on an array of integers.
```c
oid *bubble_sort(void *args) {
    int n;
    printf("Enter the size of the array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements: ", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
            }
        }
    }
    printf("Sorted array: ");
    for (int i = 0; i < n; i++)
        printf("%d ", arr[i]);
    printf("\n");

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, bubble_sort, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 26.Develop a C program to create a thread that calculates greatest common divisor(GCD) of two numbers?
```c
#include <stdio.h>
#include <pthread.h>
void *calculate_gcd(void *args) {
    int a, b;
    printf("Enter two numbers: ");
    scanf("%d %d", &a, &b);

    int x = a, y = b;
    while (y != 0) {
        int temp = y;
        y = x % y;
        x = temp;
    }

    printf("GCD of %d and %d is: %d\n", a, b, x);
    return NULL;
}

int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_gcd, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 27.Implement a C program that calculates sum of even numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>

void *sum_of_even(void *args) {
    int sum = 0;
    for (int i = 2; i <= 100; i += 2) {
        sum += i;
    }

    printf("Sum of even numbers from 1 to 100 is: %d\n", sum);
    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, sum_of_even, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 28.Implement a C program that performs multiplication of two matrices.
```c
#include <stdio.h>
#include <pthread.h>

void *matrix_multiply(void *args) {
    int r1, c1, r2, c2;

    printf("Enter rows and columns of first matrix: ");
    scanf("%d %d", &r1, &c1);
    printf("Enter rows and columns of second matrix: ");
    scanf("%d %d", &r2, &c2);

    if (c1 != r2) {
        printf("Matrix multiplication not possible! Columns of first must equal rows of second.\n");
        return NULL;
    }

    int mat1[r1][c1], mat2[r2][c2], result[r1][c2];

    printf("Enter elements of first matrix:\n");
    for (int i = 0; i < r1; i++)
        for (int j = 0; j < c1; j++)
            scanf("%d", &mat1[i][j]);

    printf("Enter elements of second matrix:\n");
    for (int i = 0; i < r2; i++)
        for (int j = 0; j < c2; j++)
            scanf("%d", &mat2[i][j]);

    // Initialize result matrix to 0
    for (int i = 0; i < r1; i++)
        for (int j = 0; j < c2; j++)
            result[i][j] = 0;

    // Matrix multiplication
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++) {
            for (int k = 0; k < c1; k++) {
                result[i][j] += mat1[i][k] * mat2[k][j];
            }
        }
    }

    // Print result
    printf("Resultant matrix:\n");
    for (int i = 0; i < r1; i++) {
        for (int j = 0; j < c2; j++)
            printf("%d ", result[i][j]);
        printf("\n");
    }

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, matrix_multiply, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 29.Develop a C program that calculates the average of numbers from 1 to 100.
```c
#include <stdio.h>
#include <pthread.h>

void *calculate_average(void *args) {
    int sum = 0;
    for (int i = 1; i <= 100; i++) {
        sum += i;
    }

    double average = sum / 100.0;  // divide by total count
    printf("Average of numbers from 1 to 100 is: %.2lf\n", average);

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, calculate_average, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 30.Write a C program to create a thread that generates a random string.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>
#include <string.h>

void *generate_random_string(void *args) {
    int length;
    printf("Enter the length of the random string: ");
    scanf("%d", &length);

    char str[length + 1];
    const char charset[] = "abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

    srand(time(NULL));

    for (int i = 0; i < length; i++) {
        int key = rand() % (int)(sizeof(charset) - 1);
        str[i] = charset[key];
    }
    str[length] = '\0';

    printf("Random string: %s\n", str);

    return NULL;
}

int main() {
    pthread_t t1;

    pthread_create(&t1, NULL, generate_random_string, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 31.Write a C program to create a thread that checks if a given number is a perfect square.
```c
#include<stdio.h>
#include<pthread.h>
void *perfect_square(void *args)
{
        int n;
        scanf("%d",&n);
        int is_perfect=0;
        for(int i=1;i*i<=n;i++)
        {
                if(i*i==n)
                {
                        is_perfect=1;
                        break;
                }
        }
        if(is_perfect)
        {
                printf("The given number is a perfect square\n");
        }
        else
        {
                printf("The given number is not a perfect square\n");
        }
        return 0;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,perfect_square,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```
## 32.Write a C program to calculate the sum of digits of a given number.
```c
#include<stdio.h>
#include<pthread.h>
void *sum_of_digits(void *args)
{
        int num=*(int *)args;
        int sum=0;
        while(num)
        {
                int rem=num%10;
                sum=sum+rem;
                num=num/10;
        }
        printf("The sum of digits are:%d\n",sum);
        return 0;
}
int main()
{
        pthread_t t1;
        int n;
        printf("Enter a number:\n");
        scanf("%d",&n);
        pthread_create(&t1,NULL,sum_of_digits,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 33.Implement a C program to create a thread that calculates the factorial of a given number using recursion
```c
#include <stdio.h>
#include <pthread.h>

int max_value;
int n;
void *find_max(void *args)
{
    int *arr = (int *)args;
    max_value = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > max_value)
            max_value = arr[i];
    }
    return NULL;
}
int main()
{
    pthread_t t1;

    printf("Enter size of array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_create(&t1, NULL, find_max, arr);
    pthread_join(t1, NULL);

    printf("Maximum element = %d\n", max_value);
    return 0;
}
```
## 34.Develop a C program to create a thread that finds the maximum element in an array.
```c
#include <stdio.h>
#include <pthread.h>

int max_value;
int n;
void *find_max(void *args)
{
    int *arr = (int *)args;
    max_value = arr[0];

    for (int i = 1; i < n; i++)
    {
        if (arr[i] > max_value)
            max_value = arr[i];
    }
    return NULL;
}
int main()
{
    pthread_t t1;

    printf("Enter size of array: ");
    scanf("%d", &n);

    int arr[n];
    printf("Enter %d elements:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%d", &arr[i]);

    pthread_create(&t1, NULL, find_max, arr);
    pthread_join(t1, NULL);

    printf("Maximum element = %d\n", max_value);
    return 0;
}
```
## 35.Write a C program That sorts an array of strings.
```c
#include<stdio.h>
#include<pthread.h>
#include <stdio.h>
#include <string.h>

int main()
{
    int n;
    printf("Enter number of strings: ");
    scanf("%d", &n);
    char arr[n][100];
    printf("Enter %d strings:\n", n);
    for (int i = 0; i < n; i++)
    {
        scanf("%s", arr[i]);
    }
    for (int i = 0; i < n - 1; i++)
    {
        for (int j = i + 1; j < n; j++)
        {
            if (strcmp(arr[i], arr[j]) > 0)
            {
                char temp[100];
                strcpy(temp, arr[i]);
                strcpy(arr[i], arr[j]);
                strcpy(arr[j], temp);
            }
        }
    }

    printf("\nSorted strings:\n");
    for (int i = 0; i < n; i++)
    {
        printf("%s\n", arr[i]);
    }

    return 0;
}
```
## 36.Write a C program to create a thread that checks if a number is even or odd.
```c
#include<stdio.h>
#include<pthread.h>
void *even_odd(void *args)
{
        int num=*(int *)args;
        if(num%2==0)
        {
                printf("%d is an even number\n",num);
        }
        else
        {
                printf("%d is an odd number\n",num);
        }
        return NULL;
}
int main()
{
        pthread_t t1;
        printf("Enter a number:\n");
        int n;
        scanf("%d",&n);
        pthread_create(&t1,NULL,even_odd,&n);
        pthread_join(t1,NULL);
        return 0;
}
```
## 37.Implement a C program to create a thread that calculates the average of elements in an array.
```c
#include <stdio.h>
#include <pthread.h>

double sum = 0;
int n;

void *calculate_sum(void *args)
{
    double *arr = (double *)args;
    sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];
    double average=sum/n;
    printf("Average=%.2lf\n",average);
    return NULL;
}

int main()
{
    printf("Enter the number of elements: ");
    scanf("%d", &n);
    double arr[n];
    printf("Enter %d numbers:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%lf", &arr[i]);
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_sum, arr);
    pthread_join(t1, NULL);
    return 0;
}
```
## 38.Write a C program to create a thread that generates a random password.
```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <time.h>
#include <string.h>

#define PASSWORD_LENGTH 12  // length of password

char password[PASSWORD_LENGTH + 1];  // global variable to store password

void *generate_password(void *args)
{
    const char charset[] = "abcdefghijklmnopqrstuvwxyz"
                           "ABCDEFGHIJKLMNOPQRSTUVWXYZ"
                           "0123456789"
                           "!@#$%^&*()_+-=<>?";
    int n = sizeof(charset) - 1;  // exclude null character

    for (int i = 0; i < PASSWORD_LENGTH; i++)
    {
        int index = rand() % n;   // pick random index
        password[i] = charset[index];
    }
    password[PASSWORD_LENGTH] = '\0';  // null terminate

    return NULL;
}

int main()
{
    srand(time(0));  // seed random number generator

    pthread_t t1;
    pthread_create(&t1, NULL, generate_password, NULL);
    pthread_join(t1, NULL);

    printf("Generated Password: %s\n", password);

    return 0;
}
```
## 39.Implement a C program to create a thread that calculates the area of a rectangle.
```c
include <stdio.h>
#include <pthread.h>

void *calculate_area(void *args)
{
    double *arr = (double *)args;
    double area = arr[0] * arr[1];
    printf("Area of rectangle = %.2lf\n", area);
    return NULL;
}

int main()
{
    pthread_t t1;
    double arr[2];

    printf("Enter length of rectangle: ");
    scanf("%lf", &arr[0]);

    printf("Enter width of rectangle: ");
    scanf("%lf", &arr[1]);

    pthread_create(&t1, NULL, calculate_area, arr);
    pthread_join(t1, NULL);

    return 0;
}
```
## 40.Develop a c program that creates a thread that calculates the average of numbers in an array.
```c
#include <stdio.h>
#include <pthread.h>

double sum = 0;
int n;

void *calculate_sum(void *args)
{
    double *arr = (double *)args;
    sum = 0;
    for (int i = 0; i < n; i++)
        sum += arr[i];
    double average=sum/n;
    printf("Average=%.2lf\n",average);
    return NULL;
}

int main()
{
    printf("Enter the number of elements: ");
    scanf("%d", &n);
    double arr[n];
    printf("Enter %d numbers:\n", n);
    for (int i = 0; i < n; i++)
        scanf("%lf", &arr[i]);
    pthread_t t1;
    pthread_create(&t1, NULL, calculate_sum, arr);
    pthread_join(t1, NULL);
    return 0;
}
```
## 41.Develop a C program to create a thread that calculates the sum of elements in an array.
```c
#include<stdio.h>
#include<pthread.h>
int sum=0;
int n;
void *calculate_sum(void *args)
{
        int *arr=(int *)args;
        sum=0;
        for(int i=0;i<n;i++)
        {
                sum+=arr[i];
        }
        printf("The sum of elements of array is:%d\n",sum);
        return NULL;
}
int main()
{
        printf("Enter number of elements:\n");
        scanf("%d",&n);
        int arr[n];
        printf("Enter elements of the array:\n");
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        pthread_t t1;
        pthread_create(&t1,NULL,calculate_sum,arr);
        pthread_join(t1,NULL);
        return 0;
}
```
## 42.Write a c program to create a thread that calculates the factorial of numbers from 1 to 10.
```c
#include<stdio.h>
#include<pthread.h>
void *fact(void *args)
{
        int num=*(int *)args;
        int fact;
        for(int n=1;n<=num;n++)
        {
                printf("The factorial of");
                fact=1;
                for(int i=1;i<=n;i++)
                {
                        fact=fact*i;
                }
                printf(" %d is: %d\n",n,fact);
        }
        return NULL;
}
int main()
{
        int num;
        printf("Range of number to calculate:\n");
        scanf("%d",&num);
        pthread_t t1;
        pthread_create(&t1,NULL,fact,&num);
        pthread_join(t1,NULL);
        return 0;
}
```
## 43.Write a C program to create a thread that searches for a given number in an array.
```c
#include<stdio.h>
#include<pthread.h>
int key;
int n;
void *search(void *args)
{
        int *arr=(int *)args;
        int is_found=0;
        printf("Enter element to search:\n");
        scanf("%d",&key);
        for(int i=0;i<n;i++)
        {
                if(arr[i]==key)
                {
                        is_found=1;
                }
        }
        if(is_found==1)
        {
                printf("The %d is present in the array.\n",key);
        }
        else
        {
                printf("The %d is not present in the array.\n",key);
        }
        return NULL;
}
int main()
{
        printf("Enter number of elements in an array:\n");
        scanf("%d",&n);
        printf("Enter elements of array:\n");
        int arr[n];
        for(int i=0;i<n;i++)
        {
                scanf("%d",&arr[i]);
        }
        pthread_t t1;
        pthread_create(&t1,NULL,search,&arr);
        pthread_join(t1,NULL);
        return 0;
}
```
## 44.Develop a C program that reverses a given string.
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
void *revstr(void *args)
{
        char *str=(char *)args;
        int n=strlen(str);
        for(int i=n-1;i>=0;i--)
        {
                printf("%c",str[i]);
        }
        return NULL;
}
int main()
{
        char str[100];
        printf("Enter a string:\n");
        fgets(str,100,stdin);
        str[strcspn(str,"\n")]='\0';

        pthread_t t1;
        pthread_create(&t1,NULL,revstr,str);
        pthread_join(t1,NULL);
        return 0;
}
```
## 45.Develop a C program to create a thread that reads input from the user.
```c
#include <stdio.h>
#include <pthread.h>
void *read_input(void *args) {
    char str[100];
    printf("Thread: Enter a string:\n");
    fgets(str, sizeof(str), stdin);
    str[strcspn(str, "\n")] = '\0';

    printf("Thread: You entered -> %s\n", str);
    return NULL;
}
int main() {
    pthread_t t1;
    pthread_create(&t1, NULL, read_input, NULL);
    pthread_join(t1, NULL);
    printf("Main: Thread finished execution.\n");
    return 0;
}
```
## 46.Write a C program to create a thread that performs addition of two matrices.
```c
#include <stdio.h>
#include <pthread.h>

#define MAX 10

int rows, cols;
int A[MAX][MAX], B[MAX][MAX], C[MAX][MAX];

void *add_matrices(void *args) {
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            C[i][j] = A[i][j] + B[i][j];
        }
    }
    printf("Resultant matrix after addition:\n");
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            printf("%d ", C[i][j]);
        }
        printf("\n");
    }
    return NULL;
}

int main() {
    printf("Enter number of rows and columns:\n");
    scanf("%d %d", &rows, &cols);

    printf("Enter elements of first matrix:\n");
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            scanf("%d", &A[i][j]);

    printf("Enter elements of second matrix:\n");
    for (int i = 0; i < rows; i++)
        for (int j = 0; j < cols; j++)
            scanf("%d", &B[i][j]);

    pthread_t t1;
    pthread_create(&t1, NULL, add_matrices, NULL);
    pthread_join(t1, NULL);

    return 0;
}
```
## 47.Write a C program to create two threads using pthreads library.Each thread should print "Hello word" along with its thread ID.
```c
#include<stdio.h>
#include<pthread.h>
void *thread1(void *args)
{
        printf("Hello world\n");
        return NULL;
}
void *thread2(void *args)
{
        printf("HEllo world\n");
        return NULL;
}
int main()
{
        pthread_t t1,t2;
        pthread_create(&t1,NULL,thread1,NULL);
        printf("thread1 identifier:%lu\n",(unsigned long)t1);
        pthread_create(&t2,NULL,thread2,NULL);
        printf("Thread2 identifier:%lu\n",(unsigned long)t2);
        pthread_join(t1,NULL);
        pthread_join(t2,NULL);
        return 0;
}
```
## 48.Develop a C program to create a thread that calculates the length of a given string.
```c
#include<stdio.h>
#include<string.h>
#include<pthread.h>
int len=0;
void *str_len(void *args)
{
        char str[100];
        fgets(str,100,stdin);
        str[strcspn(str,"\n")]='\0';
        for(int i=0;str[i]!='\0';i++)
        {
                len++;
        }
        printf("The length of the string is:%d\n",len);
        return NULL;
}
int main()
{
        pthread_t t1;
        pthread_create(&t1,NULL,str_len,NULL);
        pthread_join(t1,NULL);
        return 0;
}
```










