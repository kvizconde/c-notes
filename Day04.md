# C Notes - Day 04

----



[TOC]



---



## Reading a Line

fgets (**<u>buffer</u>**, **<u>buffsize</u>**, **<u>stream</u>**)

- keeps extracting characters from <u>stream</u> & storing than in <u>buffer</u> until

  1. end-of-file becomes true
  2. a newline character has been extracted from <u>stream</u> & stored in <u>buffer</u>
  3. ( <u>buffsize</u> - 1 ) characters have been extracted & stored into <u>buffer</u>

  > *In all cases, a null character is appended at the end.*

  

- fgets returns the null pointer on read error or on end-of-file with no characters read; otherwise it returns <u>buffer</u>



#### Examples:

char line[10];

line

|      |      |      |      |      |      |      |      |      |      |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|  ?   |  ?   |  ?   |  ?   |  ?   |  ?   |  ?   |  ?   |  ?   |  ?   |

<center>⬇️</center>
fgets (line, 10, stdin);

|  H   |  E   |  L   |  L   |  O   |  W   |  \0  |  ?   |  ?   |  ?   |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: | :--: |
|      |      |      |      |      |      |      |      |      |      |

<center>⬇️</center>
fgets(…);



> *does not wait for user input!!*
>
> **USE A LARGE BUFFER !!**



### Standard Idiom to process stdin line by line

```c
#define LINESIZE 1024 /* choose a large size */
char line[LINESIZE];

/* as long as we can read a line */
while (fgets(line, LINESIZE, stdin) != 0) {
  /* process line */
}
```

> Note: The null pointer can be denoted by 0 or null.
>
> *Null character is FALSE*



---



## Reading a data item



#### scanf / fscanf / sscanf

> - **scanf** : scan from stdin
> - **fscanf** : scan from a file specified by the 1st argument
> - **sscanf** : scan from a string specified by the 1st argument



#### Example:

**Reading an Integer**

```c
int n;
scanf("%d", &n); /* We need to check the return values */
```

> The ampersand '&' is the address of the operator, in the above example, the operator is 'n'
>
> "%d" - we ask scanf to assign an integer to n

⭐️ scanf & its variants return the number of assignments



#### Examples:

```c
int n = 10, m = 11, k = 12;
double x = 1.23, y = 3.45;
char word[1000];

/* 1 (with white spaces) */
k = sscanf(" 67 89abc", "%d%d", &m, &n);
/* 67 is assigned to m, and 89 is assigned to n and k will be 2*/


/* 2 */
k = sscanf(" 67 ab89", "%d%d", &m, &n);
/* m = 67, n = unchanged, k = 1 */


/* 3 */
k = sscanf(" 12.345 3.14", "%d%lf", &n, &x);
/* n = 12, x = 0.345, k = 2 */


/* 4 */
k = sscanf(" abc123", "&s%d", word, &n);
/* word = abc123, n = unchanged, k = 1 */


/* 5 */
k = sscanf("hello", "%d", &n);
/* n = unchanged, k = 0 */


/* 6 */
k = sscanf("   ", "&d", &n);
/* n = unchanged, k = EOF */
```

> 1. m = 67
>
>       n = 89
>
>       k = 2
>
>       Note: sscanf will skip spaces and scan the next character, in the above example sscanf will stop before 'a' because 'a' is not an INTEGER (%d).
>
> 
>
> 2. m = 67
>
>       n = 89
>
>       k = 1
>
>       Note: 'n' will not get the new assignment because sscanf will stop once it sees 'ab' which comes before '89'.
>
> 
>
> 3. n = 12
>
>       x = 0.345
>
>       k = 2
>
>    
>
> 4. word = abc123
>
>        n = unchanged
>
>       k = 1
>
>       Note: 'abc123' is a string // "%s" = word
>
>    
>
> 5. n = unchanged
>
>       k = 0 
>
>    
>
> 6. n = unchanged
>
>       k = EOF



#### Example:

**Summing integers obtained from user interactively**

```c
#define LINESIZE 1024
char line[LINESIZE]
int n, sum = 0;

while(1) {
  printf("Enter an Integer: ");
  
  if (!fgets(line, LINESIZE, stdin)) {
    /* we need to clear error before restarting */
    clearerr(stdin);
    break;
  }
  /* this checks if user DID enter an integer */
  if (sscanf(line, "%d", &n) == 1) {
    sum += n;
  }
}

printf("%d\n", sum);

```



---



#### Stdout

> when associated with terminal is line-buffered



#### Stderr

> is unbuffered
>
> fprintf(stderr, "hello");		/* will show even without /n new line */



---



## File I/O



### 3 Steps:

1. Open the File
2. Perform I/O
3. Close the File



**Opening a File**

- This associates a stream with the file



### Standard Idiom to Open a File

```c
FILE * fp;

if ((fp = fopen(filename, openmode)) == 0) {
  perror("fopen");
  /* additional error-handling if needed */
  /* ex: exit(1); <-- #include <stdlib.h> */
}
```

> perror is used to print a system error message;
>
> It can only be used if a system function fails & it sets **<u>errno</u>**
>
> perror looks at **<u>errno</u>** & prints a corresponding error message (to standard error)
>
> 
>
> Note: which function fails is not hardcoded into the error message so you need to list them.



#### errno ⭐️

> is basically a global integer variable into which certain system functions store an error number