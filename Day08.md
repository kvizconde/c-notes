# C Notes - Day 08

------



[TOC]



---



## Primitive Types

char / unsigned char / signed char

> char may be signed or unsigned



### Integral Types:

- short (signed short) / unsigned short

- int (signed int / signed) / unsigned int (unsigned)

- long (signed long) / unsigned long



**ANSI C does not specify how large each type must be.**



The sizeof operator can be used to determine the size of each type (as well as objects)

> By Definition, sizeof(char) is 1 byte.

int n = 1;

sizeof(n);



### Floating Point Types:

- float

- double

- long double



**Assume 8-bit char:**

> -128 → 127 signed char
>
> 0 → 255 unsigned char



### Suffixes:

| 1    | 1l / 1L | 1.0    | 1.0f  | 1.0l / 1.0L | 1u / 1U  |
| ---- | ------- | ------ | ----- | ----------- | -------- |
| int  | long    | double | float | long double | unsigned |

> **1ul / 1lu / 1UL / 1LU → unsigned long**



**What happens when we mix operands of different types in a binary operation?**

1 + 1.0

> 1 = int
>
> 1.0 = double

The int is converted to a double and addition of doubles is performed.



+int int

+double double

+float float

….

```c
/* subtraction of unsigned ints!! */
if (1 - 2U > 0) ....
```



long double

↑

double

↑

float

↑

unsigned long

↑

long

↑

unsigned int ← unsigned short

↑

int ← short / char, also could be unsigned short



> When we mix operands, the operand that type lower in this hierarchy is converted to the type  that is higher.



```c
/* strlen = size_t */
if (strlen(s) - strlen(t) > 0) ....
```



### ⭐️ It's dangerous to mix signed and unsigned types!!

```c
/* DANGEROUS! */
int i;

for (i = 0; i < strlen(s); i++) {
  ....
}
```



---



## Precedence and Assocciativity



```c
/* * has higher precedence than + */
a + b * c == a + (b * c)
```

> a - b - c → assocciates left to right

precedence and assocciativity determine how terms are grouped;

they do not determine the order of evaluation.



#### Examples:

```c
/* != has higher precedence than = */
c = getchar() != EOF
```



**Comma Operator:**

```c
tmp = a;
a = b;
b = c;

/* comma operator has low precedence */
tmp = a, a = b, b = c;
```



```c
/* equivalent from precedence table */
*p++ == *(p++);
```



---



## Order of Evaluation



```c
a + b + c == (a + b) + c;
```

> There are many cases where the order of evaluation of unspecified
>
> Example:
>
> order of evaluation of arguments to a function.

```c
f (n * n, m);
int n = 1;
/* don't do this; the result is unspecified */
f(++n, n++);
```



```c
int a = n++;

int b = ++n;
/* don't do this */
f(a, b);
```



### ⭐️ In general, you should not modify the same variable more than once in an expression.



---



## Some cases where order of evaluation is defined:



### Short Ciruit Operators:

&& - AND

|| - OR



#### Examples:

1. 

```c
int a = 1, b = 0, c = 2;

c = --a && ++b; /* not evaulated, b is unchanged */
printf("%d %d %d", a, b, c); /* 0, 0, 0 */
```



2. 

```c
int a = 1, b = 0, c = 2;

/* a is evaluated afterwards */
c = a-- && ++b; /* 0, 1, 1 */
```



3. 

```c
int a = 1, b = 0, c = 2;

c = --a || ++b;
/* 0, 1, 1 */
```



4. 

```c
int a = 1, b = 0, c = 2;

/* b is not evaluated because a is 1 at the start */
c = a-- || ++b;
/* 0, 0, 1 */
```



---



## Control Flow



1. if / else if / else

   > Example:

   ```c
   if (argc == 1) {
   /* do something */
   } else if (argc == 2) {
     /**/
   } else {
     /**/
   }
   ```



2. switch / case

   > Example:

   ```c
   /* choice must be some integral type */
   switch(choice) {
     case 'y': case 'Y':
       /**/
       break;
     case 'n': case 'N':
       /**/
       break;
     default:
       /**/
   }
   ```

   > case = label



3. for loop

   > Example:

   ```c
   for (<initialization>; <test>; <update>) {
     <body of loop>
   }
   ```



---



4. while loop

   > Example:

   ```c
   while (<condition>) {
     <body>
   }
   ```

   

   > **For Loop Equivalent:**

   ```c
   size_t i = 0, j = strlen(s);
   
   while (i < j) {
     if (s[i] != s[j]) {
       return 0;
     }
     i++, j--;
   }
   ```

   

#### Examples for while loop:

> A

```c
/* a */
int n = 2;
while (n < 6) { /* 4 times */
  printf("%d", n++);
}
/* 2 3 4 5 */
```



> B

```c
/* b */
int n = 2;
while (n < 6) { /* 4 times */
  printf("%d", ++n);
}
/* 3 4 5 6 */
```



> C

```c
/* c */
int n = 2;
while (++n < 6) { /* 3 times */
  printf("%d", n);
}
/* 3 4 5 */
```



> D

```c
/* d */
int n = 2;
while (n++ < 6) { /* 4 times */
  printf("%d", n);
}
/* 3 4 5 6 */
```



---



5. do/while loop

   > Example:

   ```c
   do {
     <body> /* evaluated at least once */
   } while (<condition>) {
     /* */
   }
   ```



6. goto

   > Can only use goto to jump to a location within the same function.
   >
   > Example:

   ```c
   void f(...) {
     if(...) {
       goto error;
     }
   error;
     ...
     ...
   	clearerr(stdin);
   }
   ```

   > *Most common use for 'goto' is error handling*

```c
if (fp != 0) {
  fclose(fp);
}
```



7. break / continue

   > Must be used in loops (except break is also used in switch statements)
   >
   > Example:

   ```c
   while (...) {
     if (...) {
       /* takes you to the bottom of the while body */
       continue;
     }
     if (...) {
       /* takes you out of the while loop */
       break;
     }
   }
   ```

   

   > ✋ **THIS IS DANGEROUS!! DON'T DO THIS!!** ⚠️

   ```c
   int i = 2;
   
   while (i < 10) {
     if (i == 5) {
       continue;
     }
     i++; /* i will be stuck at 5. INFINITE LOOP */
   /* continue will go here and ignore i++ */
   }
   ```

   > for loop is safer than while loop because the update happens on  top



---



### Standard Idiom to Process an Array Backwards

```c
/* T is some type, N is some positive integer value */
T a[N];

size_t i;
for (i = N; i > 0; i--) {
  /* process a[i-1] */
}
```



### Standard Idiom to Process a String Backwards

```c
/* Assume s is the string */

size_t i;
for (i = strlen(s); i > 0; i--) {
  /* process s[i-1] */
}
```

