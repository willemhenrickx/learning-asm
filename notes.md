## low level learning video notes

### 3 different operations

#### 3.1 moving the value `8` into the x64 register `rdi`

```nasm
mov rdi, 8
```

#### 3.2 moving the value of the x64 register `rsi` into the x64 register `rdi`

```nasm
mov rdi, rsi
```

#### 3.3 memory operations, moving into `rdi`  the quadword pointer of `rsi`

```nasm
mov rdi, qwordptr[rsi]
```
This means, telling the CPU to threat the value inside of `rsi` as a pointer and remove 8 bytes (`qword`) and put into the `rdi` register. This is a load operation, as we are reading from memory a value and putting it into a register.


we can do the same operation in the opposite direction too.
This is a store operation, as we are storing the value of `rdi` into a quadword pointer `rsi` as we are storing a register value into memory.

```nasm
mov qwordptr[rsi], rdi
```

### memory allocation into the vm

## Creel: 16 bit register notes

![16-bit registers](./img/16-bit-8086-registers.png)


AX, BX, CX en DX = general purpose registers

The only thing weird about the BX register is that you to push and pop the register before and after an operation, as the CPU expects an unchanged value.

BP = base Point, SP = stack pointer

IP = instruction pointer (PC) -> happens automatically, we could change this but better to leave it be.

Segementation is not really anymore something is being used in modern asm, as memory is no longer segmented but flat memory instead.



## Creel: 32 bit register notes

![32 bit registers 80386](./img/32-bit-80386-registers.png)

same as 16-bit registers, but just prepended an `E` before the registers, = extended

### Pentium IV and x64

![Pentium IV and x64](./img/PentiumIV-and-x64.png)

- 8 more registers, ranging from `R8` TO `R15`
- R prepended for the x64 counterpart of the already existing registers
- Can access lower bytes of `SI`, `DI`, `SP` AND `BP` now, this was previously not possible
- New registers naming convention:
  - `R8` -> `R8D` D for DoubleWord -> `R8W` W for Word -> `R8B` B for byte


### using registers within x64 architecture

- if we were to use an 32 bit register, `EAX` inside an x64 architecture, this will result in the first 32 bits equal to `0`'s.
**THATS ONLY FOR 32bit registers, not for 16 or 8 bit ones**


### general notes

if we would
```nasm
mov rax, -1
```
this would mean that all the 64 bits in the `rax` register would be set to `1` meaning the value would result in `FFFFFFFFFFFFFF`

but if we then would do the following instructions

```nasm
mov rax, -1 ;FFFFFFFFFFFFFFFFFFFFFF
mov al, 5   ;FFFFFFFFFFFFFFFFFFFF05
mov ax, 17  ;FFFFFFFFFFFFFFFFFF0011
mov eax, 1  ;0000000000000000000001
```

where the `eax` completely wipes the upper bits.
**This happens for ALL operations with 32 registers**
### debugging with `gdb`

```bash
# assembling the assembly code
nasm -f elf64 -o reset.o reset.asm
# as reset.s -o reset.o also works
# linking it
ld -o reset reset.o
# gcc works too
# gcc -g -o reset reset.o -nostdlib -static

# running the executable with gdb
gdb ./reset
(gdb) break _start # setting breakpoint at the _start function
(gdb) run # running the program
(gdb) si # Step Into (execute next line)
(gdb) info registers rax # Checking the value of rax register
(gdb) si # Step Into (execute next line)
(gdb) info registers rax # Checking the value of rax register
(gdb) si # Step Into (execute next line)
(gdb) info registers rax # Checking the value of rax register
(gdb) si # Step Into (execute next line)
(gdb) info registers rax # Checking the value of rax register
```

