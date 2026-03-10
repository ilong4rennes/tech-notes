- Overflow
- **Integer arithmetic** representation is limited to the number of bits and might cause overflow. But it is always consistent. 
- **Floating-point arithmetic** is not associative, thus finite precision. 
# 2.1 Information Storage 

- Most computers use blocks of eight bits, or bytes, as the smallest addressable unit of memory.
- A machinelevel program views memory as a very large array of bytes, referred to as **virtual memory**. Every byte of memory is identified by a unique number, known as its **address**, and the set of all possible addresses is known as the **virtual address space**.

## Hexadecimal Notation

- begins with `0x`
- When $x = 2^n$ for some nonnegative integer n, we can readily write x in hexadecimal form by remembering that the binary representation of x is simply 1 followed by n zeros.

## Words

- Every computer has a word size, indicating the nominal size of integer and pointer data.
- For a machine with a w-bit word size, the virtual addresses can range from 0 to $2^{w − 1}$, giving the program access to at most $2^w$ bytes.
	- Most personal computers today have a 32-bit word size. This limits the virtual address space to 4 GB.

## Data Sizes
![[Screen Shot 2024-05-18 at 17.13.51.png]]
## Addressing and Byte Ordering

- For program objects that span multiple bytes: In virtually all machines, a multi-byte object is stored as a contiguous sequence of bytes, with **the address of the object given by the smallest address of the bytes used**.
	- E.g. suppose a variable x of type int has address 0x100, that is, the value of the address expression &x is 0x100. Then the 4 bytes of x would be stored in memory locations 0x100, 0x101, 0x102, and 0x103.
- For ordering the bytes representing an object: 
	- Little endian: the least significant byte comes first (first = smallest address)
	- Big endian: the most significant byte comes first
	- E.g. suppose the variable x of type `int` and at address `0x100` has a hexadecimal value of `0x01234567`![[Screen Shot 2024-05-18 at 17.31.59.png]]

## Representing Strings

- A string in C is encoded by an array of characters terminated by the null character `\0`.
	- the terminating byte has the hex representation `0x00`

## Representing Code

- Different machine types use different and incompatible instructions and encodings, thus not binary compatible. 
- A fundamental concept of computer systems is that a program, from the perspective of the machine, is simply a sequence of bytes.

## Bit-level Operations in C

- AND: `&`
- OR: `|`
- XOR: `^`
- NOT: `~`
- One common use of bit-level operations is to implement **masking** operations

## Logical Operations in C

- AND: `&&`
- OR: `||`
- NOT: `!`

## Shift Operations in C

- `x << y` <ins>Left Shift</ins>: Fill 0 on the right.
- `x >> y`<ins>Right Shift (Logical)</ins>: Fill 0 on the left.
- `x >> y`<ins>Right Shift (Arithmetic)</ins>: Fill the most significant bit on the left.

signed data: almost all use arithmetic right shifts
unsigned data: must use logical rights shifts

# 2.2 Integer Representations 

## Integral Data Types
![[Screen Shot 2024-05-18 at 18.37.01.png]]
- Both C and C++ support signed (the default) and unsigned numbers. Java supports only signed numbers.

## Unsigned Encodings

Binary to unsigned, length w: $$B2U_w(\vec{x}) \mathrel{\dot{=}} \sum_{i=0}^{w-1} x_i 2^i$$![[Screen Shot 2024-05-18 at 18.43.43.png]]
## Two’s-Complement Encodings

Binary to two's-complement, length w: $$B2T_w(\vec{x}) \mathrel{\dot{=}} -x_{w-1} 2^{w-1} + \sum_{i=0}^{w-2} x_i 2^i$$![[Screen Shot 2024-05-18 at 18.47.35.png]]
![[Screen Shot 2024-05-18 at 18.48.44.png]]
- `<limits.h>` defines a set of constants delimiting the ranges of the different integer data types for the particular machine on which the compiler is running. (INT_MAX, INT_ MIN, and UINT_MAX) 
- `<stdint.h>` defines a set of data types with declarations of the form intN_t and uintN_t, specifying N-bit signed and unsigned integers
## Conversions Between Signed and Unsigned

Two's complement to unsigned: $$T2U_w(x) = 
\begin{cases} 
x + 2^w, & x < 0 \\
x, & x \geq 0 
\end{cases}$$
Unsigned to two's complement: $$U2T_w(u) = 
\begin{cases} 
u, & u < 2^{w-1} \\
u - 2^w, & u \geq 2^{w-1} 
\end{cases}$$
## Expanding the Bit Representation of a Number

- To convert an **unsigned number** to a larger data type, dadd leading zeros to the representation - **zero extension**
- To convert a **two's complement number** to a larger data type, add copies of the most significant bit to the representation - **sign extension**

## Truncating Numbers

The effect of truncation for unsigned numbers is: $$B2U_k([x_{k-1}, x_{k-2}, \ldots, x_0]) = B2U_w([x_{w-1}, x_{w-2}, \ldots, x_0]) \mod 2^k$$
The effect for two’s-complement numbers is: $$B2T_k([x_{k-1}, x_{k-2}, \ldots, x_0]) = U2T_k(B2U_w([x_{w-1}, x_{w-2}, \ldots, x_0]) \mod 2^k)$$
# 2.3 Integer Arithmetic 
## Unsigned Addition

Unsigned Addition with length $w$, such that $0 \leq x, y < 2^w$: $$x \mathbin{{}^{u}_{w}\mathop{+}} y = \begin{cases} 
x + y, & x + y < 2^w \\ 
x + y - 2^w, & 2^w \le x + y < 2^{w+1} 
\end{cases}$$
Test overflow: $\text{sum} < x$
## Two's-Complement Addition

Two's-Complement Addition applied to x and y in range $-2^{w-1} \le x, y \le 2^{w-1} - 1$: $$x \mathbin{{}^{t}_{w}\mathop{+}} y = \begin{cases} 
x + y - 2^w, & 2^{w-1} \le x + y & \text{Positive overflow} \\ 
x + y, & -2^{w-1} \le x + y < 2^{w-1} & \text{Normal} \\ 
x + y + 2^w, & x + y < -2^{w-1} & \text{Negative overflow} 
\end{cases} $$
## Two's-Complement Negation

Two's-Complement negation for x in the range $-2^{w-1} \leq x \le 2^{w-1} - 1$: $$- \mathbin{{}^{t}_{w}\mathop{}} x = \begin{cases} 
-2^{w-1}, & x = -2^{w-1} \\ 
-x, & x > -2^{w-1} 
\end{cases}$$
## Unsigned Multiplication

w-bit unsigned multiplication: $$x \mathbin{{}^{u}_{w}\mathop{*}} y = (x \cdot y) \bmod 2^w$$
## Two’s-Complement Multiplication

w-bit two's-complement multiplication: $$x \mathbin{{}^{t}_{w}\mathop{*}} y = U2T_w((x \cdot y) \bmod 2^w)$$
## Multiplying by Constants

- integer multiply instruction is fairly slow, requiring 10 or more clock cycles, whereas other integer operations—such as addition, subtraction, bit-level operations, and shifting—require only 1 clock cycle.
- optimization used by compilers: replace multiplications by constant factors with combinations of shift and addition operations. 
- 左移 k 位等于乘以 $2^k$, 即 k zeros have been added to the right
	- 如 x * 14 = (x<<3)+(x<<2)+(x<<1) = (x<<4)-(x<<2)
	- 判断如何移动：14 的位级表示为 1110，所以分别左移 3，2，1

## Dividing by Powers of Two

- integer division is slower than integer multiplication
- optimization: replace divisions using right shift
- integer division always rounds to zero
- x >> k gives $\left\lfloor \frac{x}{2^k} \right\rfloor$ (round down)
- round to zero: 
```c
(x<0 ? x+(1<<k)-1 : x) >> k;
```

