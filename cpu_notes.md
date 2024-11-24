# cpu

simple and can only do basic things:
- read values
- set values
- perform simple math -> add, subtract, comparison?

## communicating with cpu

- assembly is lowest "human-readable" abstraction
  - abstraction? assembly is an abstraction for "machine code" -> numbers
- assembly is translated from "text" to machine code with an **assembler**!

## the instruction cycle

3 stages: fetch data from memory, decode data, execute the instruction

fetch
- from memory/ram as electrical on/off switches?
- like a giant warehouse with boxes, but too big to move around easily
- cpu registers like a set of numbered "cubbyholes"
  - depending on processor, typically 16 general purpose registers
  - more registers used but only internally

decode
- stored data decoded into (possible representations):
  - instruction opcodes
  - numeric values
  - letters??
  - registers
  - memory addresses
- each cpu has an **instruction set** built into the chip
  - compares number to set instructions list

execute
- does instruction??
- arithmetic / logic processes use the ALU (arithmetic logic unit, duh)
  - returns value in a register??

## mapping an instruction to machine code

```asm
add r12, 4 ; Add the number 4 to the number saved in register 12
```

- add -> number in instruction set list
- param: r12 -> save destination, location of first value
- param: 4 -> second value to add

> pipelining (modern technique?): several instructions decoded at once, then executed at the "same time" so more efficient!

## electricity and the physical world

- binary numbers map to electrical circuits
  - multiple circuits can be arranged to represent binary numbers
- **buses** move the data around the cpu
  - "just groups of wires"
- **processor clock**
  - set everything on a timer, so everything happens at an organized pace
  - physical material that vibrates to produce a frequency, keeping cpu in sync
  - for every clock tick, one cpu instruction is read
  - modern cpus -> gigahertz (GHz) which is ~1 billion cycles per second
