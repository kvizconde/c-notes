# **C** Notes - Day 02

----



[TOC]



---



## How to test your functions:

```c
/* %s for the (PRED)? ... second %s for the #PRED */

# define CHECK(PRED) printf("%s...%s\n", (PRED) ? "passed" : "FAILED", #PRED)

/* output: passed...arr_sum(a, 5) == 26
	 output: FAILED...arr_sum(a, 5) == 26 */
```



#### Examples:

> Looking for an integer in an integer array:

```c
size_t arr_find(const int a[], size_t n, int x)	{
  size_t i;
  for (i = 0; i < n; i++) {
    if (a[i] == x) {
      return i;
    }
  }
  return -1
}

/* test your function */
CHECK(arr_find(a, 5) == (size_t) - 1);
```



> Squaring the numbers of an integer array:

```c
void arr_square(int a[], size_t n) {
  size_t i;
  for (i = 0; i < n; i++) {
    a[i] *= a[i];
  }
}
```



---



> **pre-condition**
>
> **post-condition**

## Design By Contract (DBC) 🤝



> **A function is a contract between its implementor of its users.**

- A precondition of a function is the condition that must hold before the function will work correctly.
- A poscondition of a function is the condition that must be true when the function returns.



> *By making the precondition & postcondition clear, we can reduce bugs.* 🐛

- **Failure to meet precondition is a bug.** 🐞

- The assert macro is used for <u>debugging</u> precondition violations.



```c
/* precondition n >= 0 */
int arr_sum(const int a[], size_t n) {
  size_t i;
  assert(n >= 0); /* assert macro */
  
  /* same code as before */
  
}
```

> We can use turn off 'assert' after we have finished debugging.

#### Example:

- gcc -DNDEBUG -ansi -pedantic -W -Wall -Werror file.c

> DN (means define)
>
> DEBUG (turns off assertions)



---



## Strings

There is no seperate string type in C.

A String is simply an array of characters terminated by the null character.

The null character can be denoted by '\0' or simply 0.



#### Example:

1. "hello" : a string constant; any attempt to modify it leads to undefined behaviour.

   > Note: there is a hidden null character at the end already —> ex: ("hello\0").
   >
   > We say it has length 5 but we need an array of at least 6 characters to store it.

   

2. char a[6] = {'h', 'e', 'l', 'l', 'o', '\0'};

   > Single quotes ' ' is for characters
   >
   > Double quotes " " is for strings

   

3. char b[6] = "hello";

   > A String is an array of characters terminated with the NULL character —> '\0'

   

4. char c[] = "hello";  /* compiler counts the characters */





### Standard Idiom to process a String

```c
/* Let s be the string */

size_t i;

for (i = 0; s[i] != '\0'; i++) {
  /* process s[i] */
}

/* Note: this does not process the null character */
```



#### Examples:

1. Length of a String

   ```c
   size_t str_length (const char s[]) {
     size_t i;
     
     for (i = 0; s[i] != '\0', i++) {
       return i;
     }
   }
   ```



2. Changing a String to all UPPERCASE

   ```c
   /* #include <ctype.h> to use toupper */
   #include <ctype.h>
   
   void str_uppercase(char s[]) {
     size_t i;
     for (i = 0; s[i] != 0; i++) {
       s[i] = toupper(s[i]);
     }
   }
   
   /* BAD: trying to change a string constant */
   str_uppercase("hello");
   
   /* OK */
   char s[] = "hello";
   str_uppercase(s)
   ```

   > toupper('a') —> 'A'
   >
   > toupper('A') —> 'A'
   >
   > toupper('#') —> '#'

   

3. Looking for a character in a String

   ```c
   size_t str_find(const char s[], int x) {
     size_t i;
     
     for (i = 0; s[i] != '\0'; i++) {
       if (s[i] == x) {
         return i;
       }
     }
     return -1;
   }
   ```

   > **In C, 'a' has type int.**



---



## String functions in the standard C Library



**'#include <string.h>'**

1. size_t strlen(const char s[]); /*  length of a string */

2. comparing strings:

   > int strcmp(const char s[], const char t[]);
   >
   > strcmp("hell", "hell")  //returns 0;
   >
   > - returns 0 if strings are the same —> returns a non-zero value otherwise
   >
   > strcmp("hell", "hello")  //returns a negative value
   >
   > strcmp("hello", "hell")  //returns a positive value



### ⭐️ Use this method for TESTING:

```c
/* the function to test */
str_replace_all

char s[] = "hello";

/* TESTING */
CHECK(str_replace_all(s, 'l', 't') == 2);
CHECK(strcmp(s, "hexxo") == 0);
```





