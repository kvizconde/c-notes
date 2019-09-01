# C Notes - Day 10

------



[TOC]



---



## Arrays vs. Pointers



#### Example:

```c
char a[] = "hello"; /* a is an array */
char *p = "hello"; /* p is a pointer */

printf("%s", a); /* hello */
printf("%s", p); /* hello */

p = a; /* OK */
a = p; /* doesn't compile */
```

> Think of an array name as the starting address of the array.



### Pointer Vs. Address

- A pointer can store an address.
- An address is a constant and can't be assigned to.



> int n = 123;
>
> int *p = 123; <— NO you can't do this
>
> You can assign 123 to n but you cannot assign 123 to a pointer.



```c
char a[] = "hello";
/* p can only store an address */
char *p = "world"; /* THIS does not store "world" */

p = a; /* OK */
printf("%s", p); /* hello */

a = p; /* does not compile */
a[1] = p[0]; /* OK */
printf("%s", a);

/* CANNOT chance String constant! */
p[0] = a[1]; /* INVALID */
printf("%s", p);
```

> "world" is stored in read-only memory (String constant)
>
> Arrays and Pointers are totally different objects!!



---



## Array of Pointers



#### Examples:

```c
int *a[5]; /* array of 5 pointers to int */
int b[5] = {3, 2, 7, 6, 8};

a[0] = &b[0];
a[1] = &b[1];
....
a[4] = &b[4];

int **c[5];
c[0] = &a[0];
```



|  b   |  3   |  2   |  7   |  6   |  8   |
| :--: | :--: | :--: | :--: | :--: | :--: |
|      |  ↑   |  ↑   |  ↑   |  ↑   |  ↑   |
|  a   |  *   |  *   |  *   |  *   |  *   |
|      |  ↑   |  ↑   |  ↑   |  ↑   |  ↑   |
|  c   |  **  |  **  |  **  |  **  |  **  |

> ```C
> /* char **argv is the same as *argv */
> int main(argc, char *argv[]) {
>   /* argc: 3 */
> }
> ```



| argv** → |     * →      |  "./a"  |
| :------: | :----------: | :-----: |
|          |     * →      | "hello" |
|          |     * →      | "world" |
|          | null pointer |         |

> printf("%s", argv[1]); /* hello */



----



#### Midterm Pointer Examples:

1. 

   ```c
   int a = 12, b = 34;
   int *p = &a, *q = &b;
   *q = *p;
   a++, b--;
   
   /* 13, 12 */
   printf("%d %d", a, b);
   ```

   > a = 13, b = 12;



2. 

   ```c
   int a = 12, b = 34;
   int *p = &a, *q = &b;
   q = p; /* q points to what p is pointing to */
   *q = *q + 1;
   *p = *p + 2;
   
   /* 15, 34 */
   printf("%d %d", a, b);
   ```

   > a = 15, b = 34;



---



## The C Preprosssor



C Source File → ***Preprocessor*** → Preprocessed File → ***Compiler*** → Assembly Output   → ***Assembler*** → Object File (Machine Code) → ***Linker*** → Executable File



The preprocessor handles preprocessor directives



### Some preprocessor directives



1. -#include

   ```c
   #include
   2 versions: 
   #include <...> /* for system header files */
   
   /* look for the file relative 
   to our current directory */
   #include "..." /* for our own / header files */
     
   
   #include <stdio.h>
   #include "a2.h"
   #include "headers/a2.h"
   ```



2. -#define

   ```c
   #define /* used to define macros */
   
   /* EXAMPLE */
   #define LINESIZE 1024;
   int main(void) {
     /* macro expression (works by text replacement) */
     char line[LINESIZE]; /* char line[1024] */
     ...
   }
   ```

   > LINESIZE is 1024 - not expanded



---



## Function-like Macros 



#### Examples:



1. 

```c
#define SQUARE(x) x*x
printf("%d", SQUARE(2)); --> printf("%d", 2*2);
printf("%d", SQUARE(1+1)); --> printf("%d", 1+1*1+1);

```



2. 

```c
#define SQUAREv2(x) (x)*(x)
printf("%d", SQUAREv2(1+1)); --> printf("%d", 1+1*1+1);
printf("%d", 25/SQUAREv2(5)); --> printf("%d", 25/(5)*(5));


```



3. 

```c
#define SQUAREv3(x) ((x) * (x))
printf("%d", 25/SQUAREv3(x));
  ---> printf("%d", 25/((5)*(5)));

int n = 1;
/* result is unspecified */
SQUAREv3(n++) --> ((n++)*(n++))

```

