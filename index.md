# :r4 programing languaje

:r4 is a language derived from ColorForth created in 2005.
As all Forth is Minimalist, it does not need complex mechanisms of abstraction, in fact the only means of abstraction is the adress.
Forth is not popular these days but curiously does not die, every programmer of forth knows why.

The idea is simple, 
Any number in source code go to the data stack.
Any adress of word is stack too.
Any word (characters separate by spaces) search in dicctionary,
	if is found execute the word.
	else is an error, stop compilation!!

Unlike ColorForth, the meaning of words is defined by prefixes.
the most importan is ' (adressof) then *word* execute a word, and *'word* is the adress of word.

[main diccionary](doc/main-dicc.md)
