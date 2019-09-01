# C Notes - Day 07

---



[TOC]



---



## Pointers Continued ➡️



### Visual Guide

| Address | 8 Bytes Memory |
| ------- | -------------- |
| 3276880 | n = 123        |
| 3276872 | p = 3276880    |
| 3276864 | q = 3276872    |
| 3276856 | r = 3276864    |



#### Examples:

> Printing pointer addressess

```c
#include <stdio.h>

int main (void)
{
  long n = 123;
  long *p = &n;
  long **q = &p;
  
  
  printf("%p\n", (void *) &n);
  
  printf("%p\n", (void *) p);
  printf("%p\n", (void *) &p);
  
  printf("%p\n", (void *) q);
  printf("%p\n", (void *) &q);
  
  return 0;
}
```



---



## Simulating pass-by-reference



### ⭐️ Follow these 3 steps:

1. Add a star (*) in front of the name of the parameter the function wants to modify.
2. Within the function, dereference the parameter to get to the original.
3. Pass in the address of the object you want changed into the function.



#### Examples:

> Tripling an integer

```c
void triple (int *n)
{
  *n *= 3;
}

int main (void)
{
  int x = 1;
  
  /* n is bound to x : it contains the address of x */
  triple(&x);
}
```



> Swapping 2 integers

```c
void swap (int *x, int *y) {
  int tmp = *x;
  *x = *y;
  *y = tmp;
}

int main (void)
{
  int a = 5;
  int b = 32;
  
  swap(&a, &b);
}
```



> Swapping 2 pointers to int

```c
void swap(int **x, int **y)
{
  int* tmp = *x;
  
  /* a = b */
  *x = *y;
  
  /* b = tmp */
  *y = tmp;
}

int main (void)
{
  int m = 5;
  int n = 32;
  
  int *a = &m;
  int *b = &n;
  
  swap(&a, &b);
}
```



### ⭐️ Assignment of Pointers

> p and q: 2 pointers of the same type
>
> p = q makes p point to the same thing as q



#### Examples:

```c
int a[] = {3, 2, 7};

int *p = a;

printf("%d", *p); /* 3 */

/* example: p[1] is 2 */
p[0], p[1], p[2]
```

> It turns out we can index pointers; actually we have been indexing pointers from the 1st class.



> Let's look at arr_sum:

```C
/* a[] can be written as const int *a */
int arr_sum(const int a[], size_t n)
{
  size_t i;
  int sum = 0;
  
  for (i = 0; i < n; i++)
  {
    sum += a[i]; /* a[i] indexing a pointer */
  }
  return sum;
  
}

int main (void)
{
  int a[] = {3, 2, 7};
  
  /* a is the starting address of array */
  arr_sum(a, 3);
}
```



### As a formal parameter of a function

```c
/* Where T is some Type */
T a[] == T *a; /* these are equivalent */
```

**⭐️ They are not equivalent for all other situations!**

> Example:
>
> **char *p = "hello";**
>
> is **NOT** equivalent to:
>
> **char p[] = "hello";**



```c
/* same thing */
size_t strlen(const char s[]);
size_t strlen(const char *s);
```



