# CSAPP Notes

## Representing and Manipulating Information
### Bits, Bytes, and Integer
each bit is 0 or 1  
exclusive-or(Xor): A ^ B = 1 when either A = 1 or B = 1, but not A = B = 1  
representing & manipulating sets  
p && \*p (avoids null pointer access)(char*)  
Shift options: logical shift, arithmetic shift  

unsigned: $B2U(X)=\sum_{i=0}^{w-1} x_i⋅2^i$  
UMin=0, UMax=$2^w-1$  
Two's complement: $B2T(X)=-x_{w-1}⋅2^{w-1}+\sum_{i=0}^{w-2} x_i⋅2^i$  
TMin=$-2^{w-1}$, TMax=$2^{w-1}-1$  
observations: $|TMin|=TMax+1, UMax=2TMax+1$  

signed and unsigned evaluation: signed value implictly cast to unsigned
```
// print -128
char a = -128;
a=-a;
printf("%d\n", a);
```
sign extension: $X'=x_{w-1},...,x_{w-1},x_{w-1},x_{w-2},...,x_{0}$  

truncating: 11000(24) =>(mod $2^4$) 1000(8)  
unsigned addition: $UAdd_w(u,v)=u+v\ mod\ 2^w$  
unsigned multiplication: $UMult_w(u,v)=u⋅v\ mod\ 2^w$  
multiply with shift: `u << k` gives u * $2^k$  
for unsigned: `u >> k` gives ⌊u / $2^k$⌋  
for minus signed: ⌊u / $2^k$⌋ gives `u >> k` if u % 2 == 0, else `(u + 1) >> k`  
signed x, -x = ~x + 1  
data type `size_t` defined as unsigned value with length = word size
```
size_t i;
for (i = cnt - 2; i < cnt; i--)
    a[i] += a[i + 1];
```
byte ordering: big endian, small endian(x86)  
### floating point
rational number: $\sum_{k=-j}^ib^k\ 2^k$, only can represent $\frac{x}{2^k}$  
floating point: $(-1)^sM2^E$  
rounding: towards zero, round up, round down, nearest even(default)  
FP multiplication: $(-1)^{s1}M1\ 2^{E1}$ * $(-1)^{s2}M2\ 2^{E2}$ => $(-1)^sM2^E$  
s = s1 ^ s2, M = M1 * M2, E = E1 + E2  
associative(x)  

## Machine-Level Representation of Programs
complex instruction set computer(CISC)  
reduced instruction set computer(RISC)  
gcc -Og -S main.c  
data type:  
'integer' data of 1, 2, 4, or 8 bytes: data values, addresses  
floating data of 4, 8, or 10 bytes  
objdump gdb  
%r(64 bits), %e(32 bits)  
%rsp(stack pointer)  
movq source, dest, but movq mem mem is wrong  
memory addressing: D(Rb, Ri, S) => Mem[Reg[Rb] + S * Reg[Ri] + D]  
leaq src, dest(src is address mode expression)  
uses: `p = &x[i]`, `x + k * y`

$rip(instruction pointer)  
status of recent tests(CF, ZF, SF, OF)  
CF: carry flag(for unsigned)  
SF: sign flag(for signed)  
ZF: zero flag  
OF: overflow flag(for signed)  

cmpq src2,src1 like computing src1 - src2 without setting destination, and setting the flag  
testq src2,src1 like computing src1 & src2 without setting destination, and setting the flag  
movzbl(clear high order bits)  

(trick)if `x > 6 && x < 0`, `(unsigned)x > 6`  
switch: create jump table, or transform to if-else  

ABI(application binary interface)  
call: push + jmp  
ret: pop + jmp  

for integers and pointers:  
argument: %rdi %rsi %rdx %rcx %r8 %r9, other: stack  
instruction pointer: %rip  
return value: %rax  

caller frame: save local variable, arguments for callee frame and return address  
callee frame: save old %rbp(optional), registers states and local variable  

caller save: %rax, %rdi,...,%r9, %r10, %r11  
callee save: %rbx, %r12, %r13, %r14, %rbp, %rsp  

`A[i]` address `A + i * (C * K)`  
`A[i][j]` address `A + i * (C * K) + j * K`  
alignment  
for largest alignment requirement K, overall structure must be multiple of K  
saving space: put large data types first  

XMM registers: 128 bits  
addss, addps, addsd  
return value: %xmm0

x86 linux memory layout:  
- stack
- heap
- data
- text/shared libraries

memory allocation in heap of two side, low and high address

buffer overflow attack:  
- code injection attacks
- return-oriented programming attacks(ROP)

buffer overflow attacks solutions:  
- avoid overflow
- system-level protection
    - randomized stack offsets
    - nonexecutable code segments
- stack canaries: place special value beyond the buffer; gcc: -fstack-protector(now default) %fs:...

union
- allocate according to largest element
- can only use one field at a time

optimize:  
reduce frequency  
replace costly operation with simpler one  
reuse portions of expression  
procedure calls  
removing aliasing  

cycles per element(CPE)  
superscalar processor pipeline  
for integer, loop unrolling with
- reassociated computation
- separate accumulators
for float, YMM registers  
branch prediction through loop







reflexion:  
dont use signed data types overflow  

