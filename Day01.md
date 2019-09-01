# C Notes - Day 01

---



[TOC]



  * [hello, world](#hello-world)
  * [Compiling](#compiling)
  * [2nd Program](#2nd-program)
    + [definition vs declaration](#definition-vs-declaration)
      - [A definition can act as a declaration.](#a-definition-can-act-as-a-declaration)
  * [Arrays []](#arrays-)
    + [Standard Idiom to process an Array](#standard-idiom-to-process-an-array)
    + [Pass By Value vs Pass By Reference](#pass-by-value-vs-pass-by-reference)
      - [Testings:](#testings)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>



---



creator: Dennis Ritchie

procedural language



we'll be using ANSI C/C90 of the gcc compiler.



## hello, world

```c
/* hello.c */ <-- this is the only comment style in C

/* int is the return type */
/* main is the name of the function */

int main (void) {
  printf("hello, world\n");
  return 0;
}

```



## Compiling

````c
$ gcc -ansi -W -Wall -Werror -pedantic hello.c -o hello
/* gcc-8 (for mac users) */
````

> **$** is the prompt

$ ./hello <— executes program

- execution starts at the function named <u>**main**</u>

- **<u>main</u>** must return an integer

- **Convention:** **<u>main</u>** should return 0 if there are no errors;
  it should return a positive integer if there are errors.

- the return value of **<u>main</u>** is passed to the shell that started the program.

  > To display this return value: $ echo $?
  >
  > **the shell does not accept negative value**
  >
  > echo **%errorlevel%**



## 2nd Program

```c
/* square.c */
#include <stdio.h>

/* function prototype: used to declare square */
int square(int);

int main (void) {
  int x = 12;
  printf("%d\n", square(x));
  return 0;
}

int square(int x) { /* the dfn of square */
  return x*x;
}
```

> **int** : type
>
> **x** : name of the variable
>
> **12** : declare variable x
>
> **%d\n** : for printing integers in base 10



### definition vs declaration

> **definition** : contains the actual code for function
>
> **declaration** : tells compiler info about the function



#### A definition can act as a declaration.

- A **function prototype** allows the compiler to check whether we are calling the function correctly.

- **stdio.h** contains the function prototypes of functions related to I/O in the standard C library.

  

---



## Arrays []

**Examples:**

1. int a [5]; /* array of 5 ints,
   elements are: a[0], a[1], a[2], a[3], and a[4];
   of this in a local array, it contains unknown values */
2.  int b[4] = {3, 2, 7, 6}; <— initializers
3. int c[4] = {3 , 2}; /* rest default to 0s */
4. int d[4] = {3, 2, 7, 6 , 8}; /* error */
5. int e[] = {3, 2, 7}; /* array of 3 ints */



### Standard Idiom to process an Array



**<u>T a[N];</u>**

> **T** : some type
>
> **N** : some positive integer

```c
size_t i;

for (i = 0; i < N; i++)
		/* process a[i] */
```

> **i = 0** : initizalization
>
> **i < n** : test, condition to continue inside loop
>
> **i++** : update after executing body for the loop



**<u>Examples</u>**

1. **Summing an array of integers**

   ```c
   int main (void) {
   	int a[] = {3, 2, 7, 6, 8};
   	size_t i; int sum = 0;
   	
   	for (i = 0; i < 5; i++) {
   			sum += a[i];
     }
     printf("%d\n", sum);;
     return 0;
   	}
   }
   ```

   - In ANSI C, all variables must be declared at the start of a block.
   - In C, uninitialized local variables have unknown/garbage values.

2. Encapsulate the summing of the array in a function

   ```c
   int arr_sum(const int a[], size_t n) {
     int sum = 0; size_t i;
     for (i = 0; i < n; i++)
       	sum += a[i];
     return sum;
   }
   ```



### Pass By Value vs Pass By Reference



- For passing an "object" into a function
- If it is passed by values, the function gets a copy of the object
- If it is passed by reference, the function gets the original object



***In C, basically everything is passed by value, ie: function gets a copy.***

**However**, it is impossible to pass a whole array **<u>by itself</u>** into a function. The array name is used as the starting address of the array

> **size_t** : type to store sizes (an unsigned integral type)



#### Testings:

1. 

  ```c
  int a[] = {3, 2, 7, 6, 8};

  /*printing the return value of the function*/
  printf("%d\n", arr_sum(a, 5));
  ```



2. print whether the function returns the correct answer.

   ```c
   printf("%d\n", arr_sum(a, 5) == 26);
   ```

   ANSI C does not have a boolean type

   0 (& its equivalents) is true, everything else is **FALSE!!**



3. Finding the minimum value of an array of integers

   ```c
   int arr_min(int a[], size_t n) { /*precondition: n > 0*/
     int min = a[0];
     size_t i;
   	for (i = 0; i < n; i++)
       if (a[i] < min)
         min = a[i]
     return min;
   }
   ```

   
