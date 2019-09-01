# C Notes - Day 05

------



[TOC]



---



## Errors vs Bugs

- **<u>Errors</u>** are exceptional conditions that we expect can happen & we write code to handle them.

- <u>**Bugs**</u> are exceptional conditions that should never happen (ie: we don't expect to happen)

  > example: not meeting preconditions



### Assert

> For debugging not for error-handling



#### Example:

```c
if ((fp = fopen(filename, operands)) == 0) {
  perror("fopen");
  /*  */
}

```

> filename: a string eg: "data.txt"
>
> may need to be careful eg: "C:\newfile.txt"		"C:\\newfile.txt"
>
>  \newfile will be a new line because of '\n' so you need double '\' to avoid this



---



## Open Modes

> 12 different modes divided into **2 groups** of 6 modes
>
> **groups** : text vs binary



**6 Text Modes:**

1. "r" - open for reading, file must exist

2. "w" - create or <u>truncate</u> file for writing

   > truncate = delete content
   >
   > append = write to end
   >
   > update = read & write

3. "a" - create or open file for appending

4. "r+" - open for updating; file must exist

5. "w+" - create or truncate file for updating

6. "a+" - create or open file for updating, always writing at the end



**Corresponding Binary Modes:**

"rb", "wb", "ab", "r+b", "w+b", "a+b"

​								↓		   ↓	  	↓

​							 "rb+"   "wb+"  "ab+"



**Unix** : no difference between text & binary modes

**Windows/DOS** : special handling of newline characters in text



| Text Mode | User Input | C Program                |
| --------- | ---------- | ------------------------ |
|           | '\n'    ↔  | '\r' '\n'                |
|           | C       ↔  | c … all other characters |





## Closing a File

> This flushes the stream and disassociate a stream from a file



### Standard Idiom to Close a File

```c
/* fp is the stream */
if (fclose(fp) != 0) {
  perror("fclose");
  /* additional error-handling if needed */
}
```



---



## Stream I/O



- fgets / fscanf / fgetc
- fprintf



**getchar()				fgetc(fp)**



### Standard Idiom to process a stream character by character

```c
int c;
/* fp is FILE* */
while((c = fgetc(fp)) != EOF) {
  /* process c */
}
```



#### Examples:

```c
#include <stdio.h>

int main(void) {
  FILE *fp;
  int n, sum = 0;
  
  if ((fp == fopen("numbers.txt", "r")) == 0) {
    perror("fopen");
    return 1;
  }
  
  /* as long as we can read an int */
  while (fscanf(fp, "%d", &n) == 1) {
    sum += n;
  }
  printf("%d\n", sum);
  
  if (fclose(fp) != 0) {
    perror("fclose");
    return 2;
  }
  
  return 0;
}
```



> Another way to do int main(void) {} :
>
> int main(int argc, char *argv[]) {}

```c
#include <stdio.h>

int main(int argc, char *argv[] {
  FILE *fp;
  int n, sum = 0;
  
  if (argc != 2) {
    fprintf(stderr, "%s", "Message Here \n");
    return 1;
  }
  
  if ((fp == fopen(argv[1], "r")) == 0) {
    perror("fopen");
    return 2;
  }
  
  /* as long as we can read an int */
  while (fscanf(fp, "%d", &n) == 1) {
    sum += n;
  }
  printf("%d\n", sum);
  
  if (fclose(fp) != 0) {
    perror("fclose");
    return 2;
  }
  
  return 0;
}
```



---



## Command-Line Arguments



**$ gcc -ansi -pedantic a1.c**

> $ - the prompt
>
> **gcc, ansi, pedantic, a1.c :** are the command-line arguments aka :
>
> *arguments to the program*

**In the above example** :

> argc : 4
>
> argv[0] : "gcc"
>
> argv[1] : "-ansi"
>
> argv[2] : "pedantic"
>
> argv[3] : "a1.c"



Command line arguments are passed into our C program via main.



**2 forms of main:**

1. int main(void);

2. int main(int argc, char *argv[]);

   > **argc** : argument count
   >
   > **argv[]** : argument vector
   >
   > **char** * : think of this as a string for now



### Standard Idiom to process Command-Line arguments

```c
int main(int argc, char*argv[]) {
  int i;
  
  /* i=1 does not process program name */
  for (i = 1; i < argc; i++) {
    /* process argv[i] */
  }
  
}
```



#### Example:

$ echo hello world

hello world

$

```c
int main(int argc, char*argv[]) {
  int i;
  
  for (i = 1; i < argc; i++) {
    printf("%s ", argv[i]);
  }
  printf("\n");
  return 0;
}
```

> $ ./myecho hello world
>
> hello_world_		[  _  represents spaces ]



**Version 2:**

```c
int main(int argc, char*argv[]) {
  int i;
  
  for (i = 1; i < argc; i++) {
    if (i == argc-1) {
      printf("%s", argv[i]);
    } else {
      printf("%s ", argv[i]);
    }
  }
  printf("\n");
  return 0;
}
```



**Version 3:**

```c
for (...)
  if (i == argc-1) {
    printf("%s\n", argv[i]);
  } else {
    printf("%s ", argv[i]);
  }
```



**Version 4:**

```c
for (...)
  printf((i == argc-1) ? "%s\n" : "%s ", argv[i]);
```



**Version 5:**

```c
for (...)
  printf("%s%c", argv[1], (i == argc-1) ? '\n' : ' ');
```





#### Original:

```c
int is_valid(int score) {
  if (0 <= score && score <= 100) {
    return 1;
  } else {
    return 0;
  }
}
```



#### Simplify Your Code:

```c
int is_valid(int score) {
  return 0 <= score && <= 100 ? 1 : 0;
}
```



```c
int is_valid(int score) {
  /* this is a boolean condition */
  return 0 <= score && score <= 100;
}
```

