# The Main Loop

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

line 0 has a word with the prefix `^`, this is a include, the file gui.txt in r4/lib/ folder is append and compile, this grow the dicctionary with new words defined in this file or in includes from this files.
A note of this word, the space break the word, then not space alowed in the name of file or folder, this can be avoided but is not so important.
then, this line include word for make a gui in :r4

line 2 has a word with de prefix `:`, this define an action word called hello, the definition end with the word `;` in the line 5.

line 3, first word, `SHOW`, a complex word, derived from ColorForth, this meaning: from here to ; this secuences repeat and execute `UPDATE`, who actualize the keyboard and mouse vars and `REDRAW`, who refresh the screen, in brief, generate a interactive loop, this loop exit with the word `EXIT`.
second word, `CLRSCR`, erase the framebuffer (the screen) and initialize some GUI vars.

line 4, first a word with prefix `"`, this prefix is special and allow space to end with another `"`, the string stack the adress of this string.
The second word `PRINT`,take the adress of a string and print in screen.

line 5, first word with prefix `'`, this prefix is adress-of, the word `EXIT` is not execute but the adress of EXIT is push on the data stack, the next word `>ESC<` execute this adress if the key scape is release, the third word is the end of definition ;

line 7, first word is `:` alone, this is a start of program, here star the execution, the call `HELLO` define previously and then end the program.

Note here, the cursor disapear, add a final `CMINIFLECHA` to draw a tiny arrow in the mouse position before the `;` because this program redraw the screen in every refresh of screen, when no dynamic content is needed, can be this:

```
0 ^r4/lib/gui.txt
1
2 :hello
3	clrscr
4	"Hello,World !" print
5	show 'exit >esc< ;
6
7 : hello ;
```

The clear screen and the print before the show prepare the framebuffer and show only wait for release ESC for exit, again, if you add `CMINIFLECHA`, the cursor now draw a arrow but not erase, you can paint with the mouse.

One more interesting example is add a Var called `XCOOR`

```
0 ^r4/lib/gui.txt
1
2 #xcoor
3
4 :main
5	show clrscr
6	"Hello,World 2!" print cr
7	xcoor "xc:%d" print
8
9	'exit >esc<
10	[ 1 'xcoor +! ; ] <up>
11	[ -1 'xcoor +! ; ] <dn>
12	cminiflacha ;
13
14 : main ;
```

Now we define a variable called `xcoor` in line 2, with not number inside, the now have 0!, all the words are case insensitive!
in Line 7 print this var in decimal in screen, here print have a similar aproach of fprintf function of C, this word consume the stack for fill the % control in string, %d is decima, %h is hex, %b is binary and so on.
In line 10 define an anonymous word, only I get de adress and use this for action when press the up key, this anonymous word add 1 to `xcoor` var and end, remember always add `;` in a `[ ]`.
In line 11 is the same for decrement in 1 when press the down key.

For concatenate words of interaction you can make this!
```
^r4/lib/gui.txt

...

...

: Hello main ;
```

How really work `SHOW` ? the basic definition is:

```
::show
	0 '.exit !
	( .exit 0? )( drop
		10 update drop
		r@ exec
		redraw ) drop
	r> drop ;
```

First, when define a word with `::` then the definition is global, when this file is include this entry in dicctionary remain, the simple definition `:` not can be called, unless if called by another word in the same file, this is very practical and fun, you export the external words and hide the internal words.

Then initialize a var `.EXIT` with 0, then ask in a loop until be diferent to 0 and call `UPDATE` with 10 (a call to OS for update the events wait for 10ms) and, this is the magic, get the adress of r stack a exec, this is, when you call a word the next direction is pushed in r stack, if you exec this  adress then exec the next word of show until end and return to this point.
last word in loop is redraw, this refresh the screen for see changes in the graphics.

last two words discard the adress in r stack for not exec more because you exit from this interactive screen.

In lib/show.txt lines 40 to 48 is the real definition, a case when return of a word with show is managed here ,more on this later.