# writing code

## a multitude of assembly languages

- each type of processor (architecture) has different assembly languages
- popular ones:
  - X86: most "useful"(?) and most computers, complicated to write
  - ARM: used by smartphones, tablets, raspberry pi, and apple silicon
  - 6502: used by older systems like nes, apple ii, and commodore 64; easier syntax for human programmers
  - Z80: used by TI-8X calculators... for some reason
  - RISC-V: simpler assembly language, made for education and research; reduced instruction set compiler
- assembly can optimize code from a compiler like in game engines

## data

- bit (0|1), nybble (4 bits), byte (8 bits) -> KB -> MB -> GB
- bytes & ascii
  - a byte for an ascii char

## registers

- x86-64 registers being used...
  - rax: accumulator (general purpose, but can be auto-read/auto-set)
  - rbx: base
  - rcx: counter
  - ...
  - r8-r15: general purpose
  - rbp: stack base pointer (auto-set)
  - ...
  - etc.
- all registers have convention set for how to use?
- specialized registers like **instruction pointer** or **stack pointer**

### register data

- registers store: numbers, letters (chars), or memory addresses
- size of register is based on the processor (32-bit / 64-bit usually)
  - processor bit refers to the size of the registers!!!
  - Usually 32-bit registers start with `E`, and 64-bit registers start with `R`
- **word** is 2 bytes, double word is 4 bytes, quadruple word is 8 bytes
  - **qword** is the size of most modern processor registers
  - but words are not always 2 bytes??? depends on manual

## i like the way you mov

```asm
mov rax, 5;
```

- move (mov) value into register
- variables and registers are quite similar... but
  - registers are limited in number 
  - can't be named
  - limited in size kinda like a variable

```asm
mov rax, 3;
mov rbx, rax;
```

## math

```asm
mov rax, 3;
mov rbx, 1;
add rax, rbx; addition

mov rax, 3;
mov rbx, 1;
sub rax, rbx; subtraction

mul rax, rbx; multiplication
```

## jumps

- **instruction pointer** (rip) / program counter
  - stores memory address of current line in program (updates itself automatically)

```asm
mov rcx, 10
jmp .addNumbers
sub rcx, 11 ; line will be skipped

.addNUmbers ; label get assembled to memory addresses
  mov rax, 3
  mov rbx, 1
  add rax, rbx
```

- memory addresses are in hex
- instruction pointer is moved to that instruction

## conditionals

- conditional jumps instead of if statements

```asm
mov rax, 3
mov rbx, 1

cmp rax, rbx
jl .jumpToHere
add rbx, 1 ; We only get to this line if the jump is not executed

.jumpToHere
  ; do some code
```

- the result of `cmp` is stored into `eflags` register
- `jl` means jump if less than! so if rax < rbx

### flags

- 5 flags on x86-64 processor in the `eflags` register
- `ZF` / Zero Flag
  - if the result is 0 of operation is 0: set to 1
  - else: set to 0

## loops

```asm
; X86-64 Intel Syntax Assembly
mov rax, 8 ; our exponent
mov rbx, 2 ; our base
mov rcx, 1 ; our result. since our counter is starting at 0, we start our result at 2^0, which is 1
mov rdx, 0 ; our counter

.calculatePower
  mul rcx, rbx       ; multiply our result by our base, save into rcx
  inc rdx            ; increment counter
  cmp rdx, rax       ; compare our counter with our exponent
  jl .calculatePower ; jump to the beginning of the loop if rdx < rax, since we still have more iterations to go

; once we've made it here, rcx contains our result (rbx^rax)!
```

## functions

```asm
; X86-64 Intel Syntax Assembly
call .addNumbers
call .doSomethingElse

.addNumbers
  mov rax, 3
  add rax, 1
  ret

.doSomethingElse
  add rax, 1
  ret
```

- class pushes instruction onto stack
- updates instruction pointer
- `ret` pops memory address off the stack into the instruction pointer

## stack

- stack is memory that can be pushed and popped to

```asm
; X86-64 Intel Syntax Assembly
push rax
push rbx ; rbx is above rax
push rcx ; rcx is above rbx and rax
push rdx ; rdx is above rcx, rbx, and rax

; Pop them off in reverse
pop rdx ; rdx is at the top of the stack, pop it off first
pop rcx
pop rbx
pop rax
```

- values popped back and "restored" to their respective registers
- stack has special **stack pointer** (`%rsp`)
- we could put these values into random spots in memory (heap-like), but would take extra instructions
- stack is sectioned off at the beginning of the program so should work fine

## conventions

> not actually enforced, just good practice and is what the C compiler does,
> but it could be faster if you avoid convention...!

- arguments to be passed into a function get pushed on the stack
- arguments get popped off the stack into registers inside functions
- return values get saved in `rax`
- general purpose registered can be "callee-owned" or "caller-owned"

```asm
; X86-64 Intel Syntax Assembly
push 3 ; m
push 5 ; x
push 2 ; b

; The stack is now (top) b, x, m (bottom)

call .getSlopeIntercept ; after this call, rax will contain our return value (17)

.getSlopeIntercept:
  ; arguments are popped in reverse order they were pushed on
  pop r8 ; b
  pop r9 ; x
  pop r10 ; m
  ; registers r8-r10 are "callee-owned"

  mul r10, r9
  add r10, r8

  ; return values go into rax
  mov rax, r10

  ret
```

### Caller/Callee-owned

- categories for general purpose registers
  - callee can be modified by callees and callers
  - `rax` is an example register that is modified
- caller used `call` command to invoke function
  - caller is responsible for saving the value onto the stack if necessary
  - caller-owned can be modified however the caller wants and value won't be lost