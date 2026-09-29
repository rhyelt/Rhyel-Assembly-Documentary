# NASM (x86-64) - Netwide Assembler

NASM is an assembler for x86 and x86-64 processors. It uses Intel-style syntax. This documentary is just simplifying the stuff for it.

## Moving Data:

mov - This copies data from one location to another. Use is mov <register>, <data>

movzx - Same concept as 'mov' but with zero extension. Use is movzx <register>, <data>

movsx - Same concept as 'mov' but with sign extension. Use is movsx <register>, <data>

push - This takes the value from a register to the top of the stack. Use is push <register>

pop - This is the opposite, it takes the value from the stack and to the register. Use is pop <register>

lea - This calculates an address via register + data, before moving it to a specified register. Use is lea <register>, <[register + data]>

xchg - This swaps the values between two operands (register or memory location). It just swaps them in short. Use is xchg <operand1>, <operand2>

movabs - This moves a full 64-bit value into a 64-bit register. Use is movabs <register>, <0x123456789ABCDEF0>

## Extending:

Zero extension - This fills the extra bits with 0.

For example:

`movzx rax, al` - If 'al' contains 1111 1010 (250 as an unsigned number). After zero extension is 00000000 00000000 ... 1111 1010, still 250.

Sign extension - This fills the extra bits with copies of the number's sign bit.

For example:

`movsx rax, al` - If 'al' contain 1111 1010 (-6 as a signed 8-bit number). After sign extension is 11111111 11111111 ... 1111 1010, still -6.

## Registers:

Registers are small and fast storage locations used to hold data, addresses and results. These stuff can only hold a specific size or amount of data or integer value, for example 64-bit is 2 to the 64th power of unsigned values (18,446,744,073,709,551,616), 32-bit is 2 to the 32nd power of unsigned values (4,294,967,296), 16-bit is 2 to the 16th power of unsigned values (65,636), 8-bit is 2 to the 8th power of unsigned values (256).

These are the 64-bit registers.

RAX, RBX, RCX, RDX, RSI, RDI, RBP, RSP, and R8 up to R15

And these are the sub-registers which is you know, the parts of a register (For example, RAX).

EAX (32 bits), AX (16 bits), AL (8 bits), and AH (8-15 bits)

## Stack:

The Stack is a part of RAM or Memory used for temporary data. Compared to the Registers, this is

RSP - The pointer which always points to the top of the stack.

The stack's size is only limited to available memory given.

## Comments:

A Comment is a text in the code ignored by the assembler, which is useful for explaining and documenting the code. It uses the semicolon.

`;This is a comment using the semicolon.`

## Mathematics:

add - This adds two values. Use is add <operand>, <value>

sub - This subtracts two values. Use is sub <operand>, <value>

inc - This increases a value by one. Use is inc <operand>

dec  - This decreases a value by one. Use is dec <operand>

imul - This multiples signed integers. Use is imul <operand>, <value>

mul - This multiples unsigned integers. Use is mul <operand>, <value>

idiv - This divides signed integers. Use is idiv <operand>, <value>

div - This divides unsigned integers. Use is div <operand>, <value>

neg - This turns a value to a negative. Use is neg <operand>

## Bit Operations:

and - This performs a bit-by-bit AND operation. AND outputs 1 only when the bits in the same position in both operands are 1. Use is and <operand1>, <operand2>

or - This performs a bit-by-bit OR operation. OR outputs 1 if atleast one of the bits in the same position of the two operands is 1. Use is or <operand1>, <operand2>

xor - This performs a bit-by-bit XOR operation. XOR outputs 1 if the bits in the same position of the two operands has a difference. Use is xor <operand1>, <operand2>

not - This just flips the bits like a switch. 1 becomes 0 and 0 becomes 1. Use is not <operand>

shl - This shifts the bits to the left by a given amount. Use is shl <operand>, <amount>

shr - Same concept as shl, but it shifts right. Use is shr <operand>, <amount>

sar - This shifts all the bits to the right while preserving the left side of the bits. Use is sar <operand>, <amount>

## Comparisons and Conditions:

cmp - This compares two operands by subtracting them without storing the result. Use is cmp <operand1>, <operand2>

test - This performs a bit AND operation without storing the result, this is used to check whether a value is zero. Use is test <operand1>, <operand2>

jz - Jumps to a specified label if the previous comparison was zero. Use is jz <label>

jnz - Jumps to a specified label if the previous comparison was zero. Use is jnz <label>

jg - Jumps to a specified label if the previous comparison's first operand was greater than the second operand. Use is jg <label>

jl - Jumps to a specified label if the previous comparison's first operand was lesser than the second operand. Use is jl <label>

je - Jumps to a specified label if the previous comparions's first operand is equal to the second operand. Use is je <label>

jne - Jumps to a specified label if the previous comparions's first operand is not equal to the second operand. Use is jne <label>

## Extra Jumps:

ja - Jumps if the first value is above the second value using unsigned numbers. Use is ja <label>

jb - Jumps if the first value is below the second value using unsigned numbers. Use is jb <label>

jae - Jumps if the first value is above or equal to the second value using unsigned numbers. Use is jae <label>

jbe - Jumps if the first value is below or equal to the second value using unsigned numbers. Use is jbe <label>

jge - Jumps if the first value is greater than or equal to the second value using signed numbers. Use is jge <label>

jle - Jumps if the first value is less than or equal to the second value using signed numbers. Use is jle <label>

## Labels (Functions):

jmp - Jumps to the specified label. Use is jmp <label>

call - Calls the specified label like a function. Use is call <label>

There are two types of labels. Global and Local. The global labels can either start with letters, underscores (_), or question marks (?) and end with a colon (:). The local labels start with a dot and also ends with a colon. "_start:" is a global label that Linux looks for in x86-64 Assembly that is where the very first instruction of the code lives. But the linker doesn't know so you need to tell the linker that it exists and public using 'global'. You dont need to use tabs or any whitespaces but it's recommended to use them for readability:

For example:

section .text
    global start

_start:
    mov eax, 60
    mov edi, 0
    syscall

There are different types of starting global label in each operating system _start for Linux and _main for both Windows and Mac

## Sections:

A section is a named, division of a program that tells the system how to load, organize, and protect different types of data and code in physical memory. Stated using section <section>. These are the different types of sections.

.text - The actual executable CPU instructions containing stuff like mov, add, syscall, etc. The system marks this as read-only so that malicious code cannot modify it.

.data - Containing the static variables that the user has given with a starting value. This is a read-write.

.bss - Reserved for variables that don't have a starting value when the program starts, for example... an empty buffer meant to hold user input later. This section takes up zero space inside the actual compiled file on your disk.The system simply allocates the requested amount of RAM and zeroes it out when the program starts.

.rodata - Similar to .data but its read-only meaning constants.

## Syscall:

Syscall is used to ask the kernel to perform something because the code is locked to Ring 3 (The Kernel is in Ring 0 which has full control over the hardware.) The CPU pauses the execution for the .text code, it saves the current pointer location, switching to Ring 0. The kernel looks at the RAX register to see what number is in it, if it is 1, it jumps to its own internal sys_write code and if it's 60, it jumps to its sys_exit code. It reads the arguments from RDI, RSI, and RDX (It may point to the .data or .bss sections) and physically drives the hardware (For example, sending characters to the terminal.) It returns the result to RAX register then flips the CPU back to Ring 3. You can find different syscalls in the internet, its very OS-based.

## Memory Addressing:

Memory addresses or operands are surrounded by brackets,

These are the examples.

`mov rax [rbx]`

`mov rax [rbx + 8]`

`mov rax, [rbx + rcx*4 + 16]`

## Data Sizes:

Data sizes are how much data an instruction operates on or stores.

8-bit = 1 byte
16-bit = 2 bytes
32-bit = 4 bytes
64-bit = 8 bytes

They can also be called,

byte = 8-bit
word = 16-bit
dword = 32-bit
qword = 64-bit

For example:

mov al, 10 -> 8-bit
mov ax, 10 -> 16-bit
mov eax, 10 -> 32-bit
mov rax, 10 -> 64-bit

It matters especially with memory operands. mov byte [address], 10 stores 1 byte, while mov qword [address], 10 stores 8 bytes.

## Strings:

A string is a group of characters stored next to each other in memory. Assembly does not have a built-in string type, so strings are stored as bytes.

For example:

`message db "Hello, World!", 10`

The db means define byte. Each character takes one byte. The 10 adds a newline.

You can get the address of a string using lea.

`lea rsi, [message]`


## Arrays:

An array is a group of values stored next to each other in memory. Assembly does not have a built-in array type.

For example:

numbers dd 10, 20, 30, 40

This creates an array containing four 32-bit numbers.

You can access an element using an index.

`mov eax, [numbers + rcx*4]`

The 4 is used because each number is 4 bytes.


## Data Definition:

Data definition instructions are used to put values directly into memory.

db - Defines 8-bit values. Use is db <value>

dw - Defines 16-bit values. Use is dw <value>

dd - Defines 32-bit values. Use is dd <value>

dq - Defines 64-bit values. Use is dq <value>

For example:

db 10
dw 1000
dd 100000
dq 1000000000


## Null Terminator:

A null terminator is a byte with the value 0 placed at the end of a string. It tells programs where the string ends.

For example:

`message db "Hello", 0`


## Times:

Times repeats a data definition a certain number of times.

For example.

`buffer times 64 db 0`

This creates 64 bytes filled with 0.


## Reserved Memory:

resb - Reserves bytes.

resw - Reserves words.

resd - Reserves dwords.

resq - Reserves qwords.

For example:

`buffer resb 64`

This reserves 64 bytes of memory.


## Flags:

Flags are small pieces of information stored by the CPU about the result of an operation.

ZF - Zero Flag. Set when the result is zero.

CF - Carry Flag. Set when an unsigned operation produces a carry or borrow.

SF - Sign Flag. Set when the result is negative.

OF - Overflow Flag. Set when a signed operation produces a result that is too large or too small.

For example:

cmp rax, rbx
je equal

If RAX and RBX are equal, cmp sets the Zero Flag and je jumps to equal.


## Loops:

Assembly does not have built-in for or while loops. Loops are made using labels and jumps.

For example:

mov rcx, 5

loop_start:
    dec rcx
    jnz loop_start

This keeps decreasing RCX until it becomes zero.

loop - Decreases RCX by one and jumps to a label if RCX is not zero. Use is loop <label>

## Label (Functions) 2:

A function is a section of code that can be called from another section of code.

For example.

my_function:
    mov rax, 123
    ret

You can call it using:

`call my_function`

call saves the address of the next instruction on the stack and jumps to the function.

ret takes that saved address from the stack and jumps back to it.


## Function Arguments:

Arguments are values given to a function.

On Linux x86-64, the first six integer or pointer arguments normally go into:

RDI - First argument

RSI - Second argument

RDX - Third argument

RCX - Fourth argument

R8 - Fifth argument

R9 - Sixth argument

The return value normally goes into RAX.


## Calling Conventions:

A calling convention is a set of rules that tells functions how to communicate with each other.

It determines where arguments go, where return values go, which registers can be changed, and how the stack is used.

Linux and Windows use different calling conventions.


## Pointers:

A pointer is a value that contains a memory address.

For example:

`lea rax, [message]`

RAX now contains the address of message.

You can use that address to access the data.

`mov bl, [rax]`

This reads the byte stored at the address in RAX.

A pointer contains an address, not the actual data.


## Address Math:

Addresses can be calculated using registers, constants, and indexes.

For example.

`mov eax, [rbx + rcx*4 + 8]`

The CPU calculates the address as:

`RBX + RCX * 4 + 8`

This is useful for accessing arrays.

The scale can normally be 1, 2, 4, or 8.


## Structures:

A structure is a group of different values stored together in memory.

For example, a structure could have a 32-bit value at offset 0 and another 32-bit value at offset 4.

You can access a value using its offset.

For example:

`mov eax, [rbx + 4]`

This accesses the value 4 bytes after the address in RBX.


## Heap:

The heap is memory used for data that is allocated while the program is running.

Assembly does not have a universal malloc or free instruction. The program normally asks the operating system or a library to allocate and free heap memory.

The exact method depends on the operating system.


## Program Flow:

The CPU normally executes instructions in order.

Jumps, calls, returns, and conditional jumps can change this order.

For example.

mov rax, 1
cmp rax, 1
je equal

mov rbx, 0
jmp done

equal:
    mov rbx, 1

done:

The CPU checks the comparison and chooses where to continue.

This is how things like if statements, loops, and functions are created using assembly.

DOCUMENTATION CREATED BY RHYEL.
