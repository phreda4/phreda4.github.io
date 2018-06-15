# :r4 Compiler

## The Tokenizer

The generated language is compiled in several stages, each stage is independent and can be replaced or reused for other propopsitos, in fact, many stages in the debugger, the compiler, the profiler are shared.

The first stage is tokenization, in this stage the written text code is converted into chain of tokens and the dictionary is built.

in compiler/r4-token.txt

::tokenfile load a file a return error or 0 and fill the dicctionary and convert all the code to tokens.

::tokenasm Add the line ^r4/system/asmbase/asmbase.txt to the beginning and then upload the file below, set the main dicctionary to special for remove some words defined in asmbase and call the tokenizer.

The definition of dicctionary and the memory map for the tokenizar is in compiler/r4-dicc.txt.

The definition of main dicctionary is in lib/macrosr4.txt.

The main tokenization word is tokeniza, this initialize the dicctionary to empty and first load all includes, with a stack for scan all the files in deep first order, only load the includes one time and keep the call order, define first word when needed, remember you can't use a word without define first.

Then, when all the files load, this memory is called source, mark the start of tokens (code and code>) and constant (data and data>) then now start tokenizer the source.

:code2token is the main iterator for convert every word to token. start for the first include file :compilatoken take the adress of string code and remove the space, check for prefix, check for number, search in main dicc and search in normal dicc. There are two states, in data definition state or in code definition state. When found a 0 in the string get the next direction of include and continue until the end of include files.

There are controls for blocks and anonymous definition and words with multiple definition (without ;).

When all the source code is converted, there are a call to tokenpost, this is in compiler/r4-post.txt, this make a tree of call and calculate the numbers of call start of main definition word :
This analisys add some info to dicctionary








