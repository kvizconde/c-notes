# C Notes - Day 09

------



[TOC]



-----



#### Examples:

> int is_palindrome

```c
int is_palindrome(const char *s) {
  size_t i, j;
  
  for (i = 0, j = strlen(s); i < j; i++, j--) {
    if (s[i] != s[j - 1]) {
      return 0;
    }
  }
	return 1;
}
```



----



## Bit Manipulation



```c
~, >>, <<, &, |, ^ and their variants
/* Example */
>>=, <<=, &=, |=, ^=
```



### x = 0011 1010 0110 1101 (use this as example)



### ~	: complement (0 <--> 1)

> Example:
>
> unsigned short x = 0x3A6D;
>
> Find ^x
>
> x = 0011 1010 0110 1101
>
> (flip the bits) ~x = 1100 0101 1001 0010 —> convert to hexadecimal
>
> **hex = 0xC592**



### <<	: left shift operator (always shift in 0)

> Example:
>
> Find x << 5 for above x
>
> x = 0011 1010 0110 1101 00000
>
> x << 5 = 0x4DA0



### >>	: right shift

> for repetitive values in a signed type, may shift in 1's or 0's; Otherwise, shifts in 0's)
>
> Example:
>
> Find x >> 5 for above x
>
> x = 0011 1010 0110 1101
>
> x >> 5 = 0x01D3



#### Examples of shifts

```c
/*0001*/	0x1 << 1 = 2;
/*0010*/	0x2 << 1 = 4;
/*0100*/	0x4 << 1 = 8;
/*0011*/	0x3 << 1 = 6;

x << 1 <--> x * 2;

0x4 >> 1 = 2;
0x8 >> 1 = 4;
x >> 1 <--> x/2;
```

> SAR or SHR



Under 2's-complement, a negative value has 1 as its most significant bit.



---



1. & - bitwise and

   > 1 if both bits are 1; otherwise 0

   

2. | - bitwise or

   > 0 if both bits are 0; otherwise 1



1. ^ - bitwise xor

   > 0 if both bits are the same; otherwise 1



#### Examples:

> unsigned short x = 0X3A6D, y = 0XDEAD;

```c
x = 0011 1010 0110 1101;
y = 1101 1110 1010 1101;
------------------------

x & y = 0001 1010 0010 1101;
x | y = 1111 1110 1110 1101;
x ^ y = 1110 0100 1100 0000;

------------------------
/* Convert to Hexadecimal */
  
x & y = 0x1A2D;
x | y = 0xFEED;
x ^ y = 0xEAC0;
```



> Find last 5 bits of m

```c
/* Find last 5 bits of m */
m & 0 .... 011111; /* 0x1F */
```



> Find last n bits of m

```c
m & 0 .... 01 .... 1;

1....1....0
1....1 << n
~0 << n
```



> m& ~(~0<<n)

```c
0....01....1
/* 01....1 is n */
```

