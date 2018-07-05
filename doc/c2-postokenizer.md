# :r4 Compiler

## The Post Tokenizer

When have the dictionary and the token string fill then you can start to make more analysis to the programa to compile/evaluate/convert.

There are two word for make this, `::tokenpostall` analize all the word and `::tokenpostusa` only analyze the words used by the program, if you convert or compile the program, not need all the words analysed but when you use this information for and editor, for example, you need analize all, for show whats happen in all words.

The way in which the word analysis works is as follows: The list of tokens is calculated by calculating the movement of the stacks, of return and of data and marking different uses of the words , like access to memory, use of records, etc.

The calculation of movement of the data stack is made using two indicators, the use and the difference, the use maintains that cells are used and the difference is how much is removed or put on the stack.

## Compile to plain code

The next stage is convert the code making some optimization

```
^r4/Compiler/r4-makeplain.txt
```

`::makeplain` is the main word, this generate a file in "r4/compiler/plain.txt" with some optimizations from the tokenized source code.

Make some transformation to source code without modify behavior.

* Make one file source code, without includes, remove all the ^ prefixes. For make this I need decorate the names of words with the number for avoid the duplicate names in private/exported definition, without the decorate one word not exported in one include can be called wrong.

* Remove all the anonymous definition words. Convert all the [ ] words with a new word with generate names, this avoid the jump to end of definition:

```
[ 1 'v +! ; ] --> :_a1 1 'v +! ;  '_a1
```

* Remove dead code and dead data, when traslate code, if the word not have calls then not save in generated code.

* When a Variable not have adress references, then no one make a modification of this value, is a constant, then is converted to constant number and remove the variable. Some problem with the `!+ !` operator is resolve but not for large sequences `!+ !+ !+ ..`, need more research on this.

```
#var 1
.. var + ..   -->    .. 1 + ..
```

* When generate code calculate when can do a constant operation then make a constant folding. Need more research for change order of opertion for simplify more, but the basic work ok.

```
3 4 + -->  7
```

* Make the transformation of division to multiply inverse. Division operation is very expensive, when the divisor is a constant can be converted to a multiplication of inverse or a shift. The last operation is for sign correct result.

```
13 / --> $4ec4ec4f 34 *>> dup 31 >> -
4 / --> 2 >> dup 31 >> -
```

* Make simple multiplication to power of 2 transformation, some advance convertion need more reserch, many compilers transform multiplication by constant to sequences of shift and adds.

```
8 * --> 3 >> dup 31 >> -
```

When have a plain.txt generate, I reset the tokenizer and tokenize again with this version for generate code.

This convertions are transformation from code in :r4 to code in :r4, this is in part, because the low level words for manipulate integer, `*>>` `/<<` and `*/` are very clever, the last one is clasical in forth, but the first two born when need this behavior and not have access to asm, this decision is the key.

## Bytecode compile

You can compile to bytecode and use the interprete avoid the compile phase, if yoou generate first the plain code, the tokens result are more eficient, more compact and more fast!, the plain button in the editor is for this.
