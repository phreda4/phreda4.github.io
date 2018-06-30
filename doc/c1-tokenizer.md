# :r4 Compiler

## Dictionary

The main data estructure is the dictionary. This definition and the memory map for the tokenizar is in `compiler/r4-dicc.txt`.

First the dictionary is in variables

```
|---- diccionario
| str|token|info|mov  	16 bytes for word
| 0   +4    +8   +12

#:indicepal )( $3ffff	| 16k words
#:indicepal> 'indicepal
#:indicepal< 'indicepal
```

Every entry has a 4  places, the fist is the adress of source code, the second is the adress in tokens, the 3 is information of words and the last is the information of movements of stack and the lenght of definition.
The variable `#:indicepal>` is the last entry created and `#:indicepal<` is the last include file, for see who words are exported.

In information place store data in the bits:

```
 bit  meaning
 $1	0 action 1 data
 $2	0 local 1 export
 $4	1 is used with adress
 $8	1 r stak is unbalanced
 $10	0 one ; 1 many ;
 $20	1 recursive word
 $40	1 has anonymous definitions
 $80	1 end without ;
 $100	1 inline
 $200	1 change memory
 $400	1 change A register
 $800	1 change B register
 $40000000	1 set A register
 $80000000	1 set B register
```

And store the count of call in mask $00fff000 (12 bits) and the level of word in the mask $3f000000 (6 bits) The level is, for word with only basics words is level 0, when call a word of level 0 then is level 1, and so on.

When the word is a variable I use the bits ($1c0) for store the description.

```
 0 value
 1 adress
 2 adress code
 3 string
 4 value list
 5 adress list
 6 adress code list
 7 string list
 8 complex structure
```

The last value called mov, store

```
 1		dD (-128..127)
 2     dU (-128..127)
 3/    dR ( -8..7 )
 /4	token lenght (12 bits)
```

And the memory map for this tokenizer is made of this vars

```
#:inisrc
#:data
#:data>
#:code
#:code>
#:<<boot

| memory Map
|-----------------	inisrc
| source code...
|-----------------	here
| includes...
|-----------------	data
| data constant...	data>
|-----------------	code
| token in code		<<boot
| 					code>
```

## The Tokenizer

The generated language is compiled in several stages, each stage is independent and can be replaced or reused for other propopsitos, in fact, many stages in the debugger, the compiler, the profiler are shared.

The first stage is tokenization, in this stage the written text code is converted into chain of tokens and the dictionary is built.

In `compiler/r4-token.txt`:

`::tokenfile` load a file a return error or 0 and fill the dicctionary and convert all the code to tokens.

`::tokenasm` Add the line `^r4/system/asmbase/asmbase.txt` to the beginning and then upload the file below, set the main dicctionary to special for remove some words defined in asmbase and call the tokenizer.


The definition of main dicctionary is in `lib/macrosr4.txt`, here you cas see the words eliminated when you compile to assembly.

The main tokenization word is `tokeniza`, this initialize the dicctionary to empty and first load all includes, with a stack for scan all the files in deep first order, only load the includes one time and keep the call order, define first word when needed, remember you can't use a word without define first.

Then, when all the files load, this memory is called source, mark the start of tokens (`code` and `code>`) and constant (`data` and `data>`) then now start tokenizer the source.

`:code2token` is the main iterator for convert every word to token. start for the first include file `:compilatoken` take the adress of string code and remove the space, check for prefix, check for number, search in main dicc and search in normal dicc. There are two states, in data definition state or in code definition state. When found the end of file in the string get the next direction of include and continue until the end of include files.

There are controls for blocks and anonymous definition and words with multiple definition (without `;`).

When all the source code is converted, there are a call to `tokenpost`, this is in `compiler/r4-post.txt`, this make a tree of call and calculate the numbers of call start of main definition word `:`, when this information I can detect the unused variables and unused code.


## Interative interprete

When have the tokens, you can execute the code or some part. The next task is assign real memory to variables for execute, this is the old version in `system/r4code.txt`, see the definition of `c/var'.













