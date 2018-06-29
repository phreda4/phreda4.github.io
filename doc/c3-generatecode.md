# :r4 Compiler

## Generate code for x86

The generation of code from the plain.txt is make in.

```
compiler/r4-asmcode86-0.txt
```

There are a basic code generator, very simple. Every basic word can be translate to assembly and simple concatenate this instruction the code is ready.

This compilers make a unoptimized code, very dumb but work.

The compiler generate code for add to a simple framework, this create a graphic windows andd keep track about event in the windows OS, the keyboard, the mouse and some inteface to API calls.

The data stack is simulated, EAX is the value of top os stack, the stack is in memory in DATASTK definition, EBP point to next to data stack.

The return stack is the normal ESP return stack.

ESI and EDI are the A and B registers.

The compile generate two files, code.asm and data.asm and put in the r4asm/ folder, here the framework called r4asm.asm include this files for make an executable.

The assembly is done with the FASM compiler [FlatAssembler](https://flatassembler.net/)

<img src="../gif/compile.gif">

## More optimized code for x86

The key concept is simulate the stack with registers, is some cases the stack operation disappears.

## Cell information

The first stage is the same in the unoptimiced code, but change when generate code for every word.

The first analisys is the cell analisis in `compiler/r4-cellana.txt` code. This word fill some arrays with information of execution.

Cell information, every cell used in word in:

```
#:cells )( 1024
#:cellt )( 1024
#:cellv )( 1024
```

Stack information, what cell used in every word of definition in:

```
#:stki )( $fff		| stack of tokens
#memstk )( $ffff	| stack memory
#memstk> 'memstk
```

The main idea is collect information of cells, who is static (only read), who is used in calculus (arithmetic or logic operations), or in memory (access to memory for read or write), cell live segment.

Example of sqrt code with coments, actual development:
```
; vreg:4
; IN: eax
; ---- blocks ----
; ;IF  (2:4) d:1 v:1
; WHI (12:14:36) d:0 v:5
; IFE (20:29:32) d:0 v:4
; ---- cells ----
; 0 R:3 W:2 >W W> noC (0:41) 1
; 1 R:2 W:3 W> noC (5:39) 2
; 2 R:4 W:2 noC (6:37) 3
; 3 R:1 W:2 CPY U C (7:11) 4
; 4	 CTE R:1 U $FE (9:10) 0
; 5 R:1 W:1 CPY U (16:19) 4
; 6	 CTE R:1 CPY U 2 (17:18) 0
; 7 R:1 W:1 CPY (21:24) 4
; 8	 CTE R:1 CPY U 2 (22:23) 0
; 9	 CTE R:1 CPY U 2 (27:28) 0
; 10	 CTE R:1 U C $2 (34:35) 0
	; 0 | :sqrt_46	| 0 | eax
; sqrt_46 | a--b | ;m | len:40 | calls:1
w15:
	; 1 | 0?	| 0 | eax
	; 2 | (	| 0 | eax
or eax,eax
jnz _1
	; 3 | ;	| 0 | eax
ret
	; 4 | )	| |
_1: 	; 5 | $0	| 0 | eax
xor ebx,ebx
	; 6 | $40000000	| 0 1 | eax ebx
mov edx,$40000000
	; 7 | PICK2	| 0 1 2 | eax ebx edx
mov ecx,eax
	; 8 | CLZ	| 0 1 2 3 | eax ebx edx ecx
bsr ecx,ecx
xor ecx,31
	; 9 | $FE	| 0 1 2 3 | eax ebx edx ecx
	; 10 | AND	| 0 1 2 3 4 | eax ebx edx ecx $FE
and ecx,$FE
	; 11 | >>	| 0 1 2 3 | eax ebx edx ecx
sar edx,cl
	; 12 | (	| 0 1 2 | eax ebx edx
_2: 	; 13 | 1?	| 0 1 2 | eax ebx edx
	; 14 | )(	| 0 1 2 | eax ebx edx
or edx,edx
jz _3
	; 15 | ROT	| 0 1 2 | eax ebx edx
	; 16 | PICK2	| 1 2 0 | ebx edx eax
mov ecx,ebx
	; 17 | PICK2	| 1 2 0 5 | ebx edx eax ecx
	; 18 | +	| 1 2 0 5 6 | ebx edx eax ecx edx
add ecx,edx
	; 19 | >=?	| 1 2 0 5 | ebx edx eax ecx
	; 20 | (	| 1 2 0 | ebx edx eax
cmp eax,ecx
jl _4
	; 21 | PICK2	| 1 2 0 | ebx edx eax
mov ecx,ebx
	; 22 | PICK2	| 1 2 0 7 | ebx edx eax ecx
	; 23 | +	| 1 2 0 7 8 | ebx edx eax ecx edx
add ecx,edx
	; 24 | -	| 1 2 0 7 | ebx edx eax ecx
sub eax,ecx
	; 25 | ROT	| 1 2 0 | ebx edx eax
	; 26 | 2/	| 2 0 1 | edx eax ebx
sar ebx,1
	; 27 | PICK2	| 2 0 1 | edx eax ebx
	; 28 | +	| 2 0 1 9 | edx eax ebx edx
add ebx,edx
	; 29 | )(	| 2 0 1 | edx eax ebx
jmp _5
_4: ; cpy: 1 >>
	; 30 | ROT	| 1 2 0 | ebx edx eax
	; 31 | 2/	| 2 0 1 | edx eax ebx
sar ebx,1
	; 32 | )	| 2 0 1 | edx eax ebx
_5: 	; 33 | ROT	| 2 0 1 | edx eax ebx
	; 34 | $2	| 0 1 2 | eax ebx edx
	; 35 | >>	| 0 1 2 10 | eax ebx edx $2
sar edx,$2
	; 36 | )	| 0 1 2 | eax ebx edx
jmp _2
_3: 	; 37 | DROP	| 0 1 2 | eax ebx edx
	; 38 | NIP	| 0 1 | eax ebx
	; 39 | ;	| 1 | ebx
lea eax,[ebx]
ret
```

IN is the registers in stack from input, the eax is the container. Next a list of blocks of code with the numbers of intrucctions. Next a list of cell used, with the quantity of reads (R) and writes (W), if is a constant a CTE, some other infor, the life (from:to) and the virtual register. In this word, only with 3 register you can compile.

Then the list of words with the current stack precalculate in previous analysis, and the stack in real values.

The lines without ; is the code generated, the labels are global.

