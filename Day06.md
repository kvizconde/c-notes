# C Notes - Day 06

----



[TOC]



---



## 3 indicators associated with a stream

- end-of-file indicator : tested by feof
- error indicator : tested by ferror
- file position indicator

> clearerr clears both indicators



```c
if (!fgets(line, LINESIZE, fp)) {
  if (ferror(fp)) {
    /* read error */
  } else {
    /* end-of-file */
  }
  clearerr(fp); /* you can put clearerr here */ 
}
```

> if we don't need to go over the function again then we don't need clearerr



---



## Finding our current location in a stream



```c
/* basically returns the number of bytes from the beginning of the stream */
long ftell(FILE *);
```

> returns -1 on failure



##### Not all streams support ftell.

> Example:
>
> It fails when applied to stdin associated with keyboard input



#### Example:

```c
long pos = ftell(fp);

if (pos == -1) {
  perror("ftell");
  /* additional error-handling if needed */
}
```



---



## Seeking within a stream



```c
void rewind(FILE *); /* go the beginning of the file */

int fseek(FILE*, long, int);

fseek(fp, pos, whence);
```

> **fp** : stream
>
> **offset** : 
>
> **whence** : **3** possible values
>
> 1. **SEEK_SET** : relative to the beginning
> 2. **SEEK_CUR** : relative to the curent position
> 3. **SEEK_END** : relative to the end



#### Example:

```c
if (fseek(fp, -5, SEEK_CUR) != 0) {
  perror("fseek");
  /* additional error-handling if needed */
}
```



##### Not all stream are "seekable"

> Example:
>
> stdin when associated with keyboard input.

```c
fseek(fp, 0, SEEK_SET); /* go to beginning of file */
fseek(fp, 0, SEEK_END); /* go to end of file */
fseek(fp, -5, SEEK_CUR); /* go back 5 bytes */
```



**Can we seek past the end of a file?**

> Depends on OS. But UNIX supports this.

⭐️ But generally, we cannot seek past the **beginning of a file**.



In ANSI C, whenever we switch between reading and writing a stream, we need to do a fseek.

> *stream is open for both read & write.*



#### Example:

Changing a file to all uppercase character by character.

```c
int c;

while((c = fgetc(fp)) != EOF) {
  fseek(fp, -1, SEEK_CUR);
	fputc(toupper(c), fp);
	fseek(fp, 0, SEEK_CUR);
}
```



---



## Pointers ➡



### Definition:

> A pointer is a variable that stores an Address.



⭐️ **p points to x if p contains the address of x**



**We need to be able to :**

1. create a pointer
2. find addresses
3. get to the destination specified by an address or pointer



### Sources of Addresses



1. & : address-of operator

   > int n;
   >
   > &n

   

2. array names : they can be used as their starting address

   > int a[10];
   >
   > a — starting address of array

   

### Getting to the Destination

> *Specified by an Address or Pointer*



If **x** is an address or pointer, then ***x** is the destination

*- go to the destination (dereference operator)

- *we are dereferencing 'x' to get to the destination*



#### Example:

```c
int a[] = { 3, 2, 7 };

printf("%d", *a);	/* 3 */
*a = 4; /* changes a[0] to 4 */

printf("%d", a == &a[0]); /* TRUE : 1 */
```

> The starting address of array 'a' is the same as the first element of the array.
>
> &a[0] is equivalent to &(a[0])



### Declaring a Pointer



#### Examples:

```c
/* p is a pointer to an int, where int is the destination type */
int *p; /* int <-- p */

int n = 123;
p = &n /* p points to n */
*p = 666; /* changes n to 666 */
```



```c
double x = 1.25;
double *q = &x;

printf("%f", *q); /* 1.25 */
```



#### 2 ways of looking at int *p;

```c
/* 1 */
int* p; /* p is an int* */

/* 2 */
int *p; /* *p is an int */
```



```c
/* 1 */
int** q; /* q is an int ** */

/* 2 */
int* *q; /* *q is an in int * */

/* 3 */
int **q; /* **q is an int */
```



***P → N[123]**



```c
int n = 123;
int *p = &n;
int **q = &p;

**q = 666; /* changes n to 666 */

/* prints a pointer */
/* need to cast (void *)q to avoid warning */
printf("%p", q);
```

