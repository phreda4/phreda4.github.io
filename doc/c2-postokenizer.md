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

Make one file without includes, remove all the ^ prefixes!

Remove all the anonymous definition words. Convert all the [ ] words with a new word with generate names, this avoid the jump to end of definition:

```
[ 1 'v +! ; ] --> :_a1 1 'v +! ;  '_a1
```

Remove dead code and dead data, when traslate code, if the word not have calls then not save in generated.

When a Variable not have adress references, then no one make a poke, then is a constant, then is converted to constant and remove the variable.

```
#var 1
.. var + ..   -->    .. 1 + ..
```

When generate code calculate when can do a constant operation then make a constant folding.

```
3 4 + -->  7
```

Make the transformation of division to multiply inverse.

```
13 / --> $4ec4ec4f 34 *>> dup 31 >> -
```

Make simple power of 2 transformation, some advance convertion is not doing now!.

```
8 * --> 3 >> dup 31 >> -
```

When have a plain.txt generate, I reset the tokenizer and tokenize again with this version for generate code.











