# Vector Graphics

:r4 born for and experiment, I make a animation program for wince called redamation, some complication apear and try to remediate the problem separating the layer of drawing of the layer of program, I start to learn forth and try to add a programable unit in this animation program in forth. This days have good opinion of flash and then try to use vector graphics for all.

First to all, :r4 not have console, only a framebuffer, the words for use are

```
SW 		| -- w		Screen Width
SH 		| -- h		Screen height
FRAMEV	| -- m		framebuffer memory
REDRAW  | --		copy framebuffer to read screen
```

But in the original design I add words for make vector graphics, later, when make the compiler, only use this words and redoing this vector graphics word in :r4 itself.

This redefinitios is in system/asmbase/asmbase.txt, with previous definition only for see the evolution of this complex words.

All the vector graphics words are antialiased and have fill options, not really used today but remember this used in the flash era.

# Usage

If you try to use the framebuffer you can define put or get a pixel:

```
:putpixel | color x y -
	sw * + 2 << vframe + ! ;

:getpixel | x y -- color
	sw * + 2 << vframe + @ ;
```

All the bitmat words are buildon top of interpreter, drawing bitmap font, or drawing bitmap sprites.

But the first thing to I do for learn is make an vector font and a vector sprite, this is a brief description of how work the last incarnation.

in lib/vesprite.txt you can see this definition

```
::vesprite | 'rf --
	$80000000 'xp !
	( @+ 1? )( dup $f and 2 << 'jves + @ exec ) 2drop ;
```

This is really simple, initialize a xp with a flag for close the poligons in a automatic way, this is, not need to draw the last line. And iterate reading mem until a 0 is reached, 0 in a memory with a vector sprite is the end of the sprite.
For every integer read, I extrat the lower 4 bits ($f and) and shift 2 to left (a 0 is a 0 a 1 is a 4 and so on), add a address of var with a list of vector, extract the vector and jump.

```
#jves a0 a1 a2 a3 a4 a5 a6 a7 a8 a9 aa ab ac ad ae af
```

See the var of vector, a detail in the lang, when you define a variable, always the actions are adress, this simplify the compiler of bytecodes (and the language), then, in a var difinition is the same `'var` or `var`.

Prior to this the definitions or a0,a1,etc.

```
:a0 drop ;
:a1 8 >> ink ;
:a2 swap >b gc>xy b@+ gc>xy
	>r neg pick2 + r> neg pick2 +
	1.0 pick2 dup * pick2 dup * +
	1 max / >r 		| xc yc xm ym d
	swap neg r@ 16 *>> swap r> 16 *>>
	fmat fcen b> ;
:a3 gc>xy fcen @+ gc>xy fmat ;
:a4 xp $80000000 <>? ( yp pline )( drop )
	gc>xy 2dup 'yp !+ ! op ;  | punto
:a5 gc>xy pline ; | linea
:a6 swap >b gc>xy b@+ gc>xy pcurve b> ;  | curva
:a7 swap >b gc>xy b@+ gc>xy b@+ gc>xy pcurve3 b> ; | curva3

.
.
.
```

you can see, for a0 is a nothing word, a1 is the set of color in the upper 24 bit of the integer read from memory.
a2 and a3 is a complex word for define 2d matrix for shader.
a4 is the op, then decode the 2 coordinate from the 32 bit integers and use OP for set the origin point of draw. the decode word gc>xy is define in lib/convert.txt, you can see

| 14 - 14 - 4
| 0000 0000 0000 00 | 00 0000 0000 0000 | 0000
| x					y			      control

this is in bit how store the info of every point. You see too this word use vars for scale (w and h) and center(xc,yc) the draw.
You see too how trace the last PLINE with a xp,yp saved for close the polygons.
a5 make a pline, a6 make a curve, etc.
then a files who store this sprites are .vsp, some others definitions are used, the old ones, like .spr

The editor call the actual editr for this variables when you press CTRL-E in the line of include, type a line

```
^./test.vsp
```
and hit CRTL-E in the editor and see what happen
