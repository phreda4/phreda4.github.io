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


