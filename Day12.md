# C Notes - Day 12

----



[TOC]



------



## Structures



#### Example:

```c
#define IDSIZE 10
#define NAMESIZE 20

struct name {
  char last[NAMESIZE];
  char first[NAMESIZE];
},

struct grade {
  char id[IDSIZE];
  struct name name;
  int score;
},

struct grade g, *p;
strcpy(g.id, "a12345678");
strcpy(g.name last, "simpson");
strcpy(g.name first, "homer");
g.score = 49;

p = &g;
printf("%s %s\n", p->name.first, p->name.last);
```

> a -> b = (*a).b



## typedef

> Used to give a type another name



#### Example:

- Give the type "unsigned long" the name "ulong"

```c
unsigned long n; /* n is a variable of type unsigned long */

/* vs */

typedef unsigned long n; /* n is the type unsigned long */
n m; /* m is a variable of type unsigned long */

/* THIS IS WHAT WE WANT */
typedef unsigned long ulong;
```



#### Example:

```c
typedef struct grade grade;
grade g, *p;
```



> Give the type array of 25 ints the name set

```c
int a[25];
typedef int set [25];
set a, b, c, d;
```





### ⭐️ We can combine typedef with structure declaration



#### Example:

```c
typedef struct name { /* name here becomes optional */
  char first[LINESIZE];
  char last[LINESIZE];
} name; /* if you choose to put name here */

typedef struct {
  char id[IDSIZE];
  name name;
  int score;
} grade;
```

> typedef struct {
>
> ​	// something here
>
> } name;



### We can declare a structure and create variables of that type at the same time.

```c
struct grade {
  /**/
} g, *p, a[10]; /* etc. */
```



---



## Void Pointers



Example: void *p;



1. A pointer is compatible with any type of pointers to objects.

   > ```c
   > int *q;
   > p = q; /* OK, compatible */
   > q = p; /* OK, also compatible */
   > ```



2. A void pointer cannot be directly dereferenced.

   > *p - invalid
   >
   > Example:
   >
   > ```c
   > int n = 123;
   > int *q = &n;
   > void *p = q;
   > *p = 456; /* invalid, p is a void pointer */
   > 
   > /* we must cast the pointer */
   > *(int *)p = 456; /* OK, changes n to 456 */
   > 
   > 
   > /* NOTE: THIS DOES NOT WORK! */
   > (int)*p = 456; /* NO!! */
   > ```



---



## Lifetimes



1. The Lifetime of a local object is the duration of execution of the block in which it is created.



2. The Lifetime of an external (global) object is duration of execution of the program.

   > Dynamic memory gives us more control over the lifetime of objects.



---



## Dynamic Memory



**4 functions.**

```c
#include <stdlib.h>
```

1. malloc
2. calloc
3. realloc
4. free



##### Used to allocate dynamic memory:

> - malloc
> - calloc



##### Typically used to resize dynamic memory:

> - realloc



##### Used to deallocate dynamic memory:

> - free



```c
void *malloc(size_t nbytes); /* returns the starting address of the allocated memory or the null pointer on failure */
```

> a (void) * can be assigned to any type.



#### Example:

> Allocating an array of 100 ints

```c
int *p; size_t i;
p = malloc(100 *sizeof(int));

if (p == 0) {
  fprintf(stderr, "unable to allocate memory \n");
  exit(1);
}

/* proceed to use the dynamic array; an example */
for (i = 0; i < 100; i++) {
  p[i] = 5 * i + 1;
}

/* free the array when done */
free(p);
```



| 1    | 6    | 11       | /// you should not access this part anymore | ///  |
| ---- | ---- | -------- | ------------------------------------------- | ---- |
| *p   |      | free(p); | xxx                                         | xx   |

> **Common Bugs :** 
>
> 1. use after free (don't try to access anymore after you call free)
> 2. double free (you don't need to call it more than once)



```c
void *realloc (void *oldaddr, size_t newsize);
```



What if the dynamic array is not large enough?

```c
int *p, *tmp;
p = malloc(100 *sizeof(int));
if (p == 0) {
  ....
  exit(1);
}

/* use p & we need to resize to 200 ints */
tmp = realloc(p, 200 * sizeof(int));
if (tmp == 0) {
  fprintf(stderr, "realloc failed\n");
  exit(1); /* or continue to use original 100 ints */
}

/* case: realloc succeeded */
p = tmp;
/* continue to use p that now points to 200 ints */
....

/* deallocate memory when done */
free(p);
```

> make sure you have succeeded before you assign it back to the original when using realloc.





















