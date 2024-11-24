# assembleee

some assembly required notes!

- can compile c into assembly with -S flag in the compiler
- steps of compilation for c:
  - clang -S file.c => assembly file in assembly code (preprocessor/compiler step)
  - as file.s => object file in object binary/machine code (assembler step)
  - ld file.o => link object code into executable binary/machine code (linker step)
