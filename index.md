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

For make a program you need define yours words.

A program have two types of words, actions and data, time and space. For define an action :r4 use the prefix : and to define data the prefix #

```
:thiswordisanaction
	1 2 3 + * ;

#thisisdata 33
```

When you define a word, only you can use a word previous defined, or current definition for recursive words.

When start a program, the diccionary have basic definitions:

[Main Diccionary](doc/main-dicc.md)

The main diccionary has a basic interaction with the OS, 12 words, very basic. This aproach has some advantages over recreate all the API calls.

* Not deal with deprecate and diferent behavior in versions.
* Is easy port to other OS or embebed in a boot image. No OS !
* The compiler only need code this 12 words for comunicate with the OS.

The cons is not have full access to hard, for now!.

Words for SOUND, PRINTER, JOYSTICK and WEB has a definition for particular nedded,but the idea is avoid this and in some point, recreate in :r4