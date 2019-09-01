# C Notes - Day 11

---



[TOC]



---



# #undef

```c
#define LINESIZE 1024

#undef LINESIZE /* undefine LINESIZE */
```



## Conditional Compilation

```c
#if / #elif / #else / #endif
#ifdef / #endif
#ifndef / #endif
```





**Quick way to comment out of a block of code:**

```c
#if 0
/* not passed to the compiler */
	------------/*...*/ 
  ------------/*...*/
  ------------/*...*/
#endif
/* Comments don't nest in C. */
```

> Example (ignore ' '):
>
> '#'ifdef DEBUG /* if the DEBUG macro is defined */
>
> ​		…..
>
> '#'endif



We can define DEBUG within the source code(not flexible) or when we are compiling the program using gcc:

gcc	-DDEBUG….

> -DD means 'define'



```c
#ifndef ABC /* if the ABC macro is not defined */
----
#endif
#ifndef is used to implement include guards.
```

> Including the same header file more than once may lead to errors. (Usually this is due indirect inclusion)



### ⭐️ Include guards are used to ensure a header file is not read in more than once.



```c
/* myheader.h */
#ifndef MYHEADER_H
#define MYHEADER_H
----
----
----
#endif /* you need this at the end */
```



#### Examples:

```c
#include "myheader.h"

/* second time, it is already defined, so it skips the entire file */
#include "myheader.h"
```



```c
#if DEBUGLEVEL == 1
-----
#elif DEBUGLEVEL == 2
-----
#else
-----
#endif
```



```c
#define UNIX 1
#define DOS 2

int main(void)
{
  #if OS == UNIX
  /*do something & execute clear*/
  #elif OS == DOS
  /*do something & execute cls*/
  #else
  /*print some \n s*/
  #endif
}
```

> gcc **-D**OS=UNIX….
>
> **-D : define**



## '#' string size operator



**Example:**

```c
#define CHECK(X) printf("%s...%s\n", (X)? "passed" : "FAILED", #x)

CHECK(strlen(s) == 5); --> printf("%s...%s\n", (strlen(s) == 5)? "passed" : "FAILED", "strlen(s) == 5");
```



---



## Structures



Allow us to package data of different types together.



#### Example:

```c
struct grade { /* structure declaration */
  /* members in the structure */
  char id[10];
  int score;
}; /* don't forget semicolon! */

struct grade g, a[10], *p;
g.score = 65;
strcopy(g.id, "a12345678");
printf("%s: %d\n", g.id, g.score); /* a12345678: 65 */

/* accessing the array */
a[0].score == 89;
strcopy(a[0].id, "a666666666");

/* assignment of structures */
a[i] = g;

/* with pointers */
p = &g;
printf("%s: %d\n", (*p).id);

*a.b = *(a.b);

/* a better way with pointers; use --> notation */
a->b = (*a).b;

```

> **struct** : keyword
>
> **grade** : tag



What is the size of a structure?

Size of structure >= sum of sizes of its members

sizeof(struct grade) >= 14

The size can be bigger because of alignment requirements.



**possible layout of struct grade:**

| id       | ////////          | score   |
| -------- | ----------------- | ------- |
| 10 bytes | padding (2 bytes) | 4 bytes |



