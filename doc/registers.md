# Registers A and B

## A little history

In the beginning I wanted to work with the video memory directly, for this I defined among the basic words a pointer that can be addressed and that the pixels can be read and written in the memory.

In the virtual machine this register was really a register in assembly, the compiler also treated it the same. This occupies one of the few register of the processor.

This treatment was necessary for the work with pixels to be fast enough and to use a variable that would add access to this variable to memory access. And, perhaps most important, is that the value should be maintained in the flow of the program between calls of words, otherwise a place in the data stack would be sufficient, and in fact, in many cases this is better.

The greenarray chips have two registers but what convinced me to take this path is the presentation of Chuck Moore in 2017 forth day that directly uses the stack next to the registers, although it breaks the orthogonality that the value of these records are kept outside the stack and if one does not know that a word uses a certain record can generate an unwanted behavior, the advantages of having this possibility simplifies many things.

In one of the revisions of the language remove the words of access to the pixels and add the use of registration, in addition to removing the ability to use the stack of addresses as pointer to memory. Now with these records it is possible to use this way of working with a memory pointer between calls to words for any task, not just memory management.

## adress registers A and B

The stack with this words are:

```
>A		| v --		A=v ; A is a register for use like pointer
A>		| -- v      v=A
A+		| v --		A=A+v
A@		| -- v		v=[A]
A!		| v --		[A]=v
A@+		| -- v		v=[A] A=A+4
A!+		| v --		[A]=v A=A+4

>B		| v --		B=v ; B is a register for use like pointer
B>		| -- v		v=B
B+		| v --		B=B+v
B@		| -- v		v=[B]
B!		| v --		[B]=v
B@+		| -- v		v=[B] B=B+4
B!+		| v --		[B]=v B=B+4
```

A and B registers not are stacks but load with >A and >B and retrieve the value (not clear) with A> and B>, need this because the @ and the ! are store and load from memory. A word for increment the pointer with A+ and B+ (can be negative to back), the peek and poke values A@ B@ A! B!, peek and increment A@+ B@+, poke and increment A!+ B!+.

I don't use A or B for get the value because I still reserve for definition, the modification of R to R@ is past revision is for this pourpose, define #r is ok now!!


