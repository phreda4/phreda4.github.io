The first program is, of couse, hello world

```
0 ^r4/lib/gui.txt
1
2 :hello
3	show clrscr
4	"Hello,World !" print
5	'exit >esc< ;
6
7 : hello ;
```

brief explanation:

line 0 has a word with the prefix ^, this is a include, the file gui.txt in r4/lib/ folder is append and compile, this grow the dicctionary with new words defined in this file or in includes from this files.
A note of this word, the space break the word, then not space alowed in the name of file or folder, this can be avoided but is not so important.
then, this line include word for make a gui in :r4

line 2 has a word with de prefix :, this define an action word called hello, the definition end with the word ; in the line 5.

line 3, first word, SHOW, a complex word, derived from ColorForth, this meaning: from here to ; this secuences repeat and execute UPDATE, who actualize the keyboard and mouse vars and REDRAW, who refresh the screen, in brief, generate a interactive loop, this loop exit with the word EXIT.
second word, CLRSCR, erase the framebuffer (the screen) and initialize some GUI vars.

line 4, first a word with prefix ", this prefix is special and allow space to end with another ", the string stack the adress of this string.
The second word PRINT,take the adress of a string and print in screen.

line 5, first word with prefix ', this prefix is adress-of, the word EXIT is not execute but the adress of EXIT is push on the data stack, the next word >ESC< execute this adress if the key scape is release, the third word is the end of definition ;

line 7, first word is : alone, this is a start of program, here star the execution, the call HELLO define previously and then end the program.
