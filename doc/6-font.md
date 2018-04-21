# Fonts

## First Vector Font

One of the first things I did in :r4 is a font, as I wanted it to be scalable it had to be vector, so I encoded each letter that composed this font by hand. The font in question is lib/font.txt, start with the definition of shapes, then a array with the direccion of every char in line 136, in line 145 another array with the width of every char.
The Basic working of this font is here

```
:p0 ;
:p1 xa ya op ;
:p2 xa ya pline ;
:p3 fc 0? ( xa 'xcc ! ya 'ycc ! )( xa ya xcc ycc pcurve ) not 'fc ! ;

#fp8acc p0 p1 p2 p3

::v8draw | addr --
	0 dup tox 'xa ! dup toy 'ya ! 'fc !
	( c@+ 1? )(
		dup $1 and? ( 3 >> tox 'xa )( 3 >> toy 'ya ) !
		$6 and 2* 'fp8acc + @ exec ) 2drop
	poli ;
```

Every char is a string of byte ending with 0, every byte have a control in the 3 lower bits, and data in the 5 bits (-16 to 15 range). Bit 1 is X/Y indicator, Bits 2 and 3:
```
00x No operation (only load the X or Y coord)
01x OP, set the origin point.
10x PLINE, trace a line for polygon
11x PCURVE, first set the control point and then trace the curve.
```

in lib/gc.txt, line 42:
```
::tox w 5 *>> xc + ;
::toy h 5 *>> yc + ;
```

the words tox and toy calculate the screen coordinate from the 5 bits of control, w and h vars are the width and height of char, xc and yc are the center of this draw. The last word draw the polygon.
The font are draw in a grid of 31x31 lines, very ugly for some details!

## Bitmap font

After seeing how ugly these characters were, I had to build a bitmap font, take now if the characters 8x12 of the pc I had used in DOS and build the next font. the file is lib/fonti.txt. This is the default font now.

In this source the obtaining of the bitmaps was automatic, much more simple to draw, the use of a pointer to the video memory to draw the pixels is the reason of the special registers of the language, A and B.

Now have a cursor position of chars in ccx ans ccy, remember sw is the screen width:

```
::char8i | c --
	ccx ccy setxy
	3 << dup 2/ + 'rom8x12 +
	sw 8 - 2 << swap
	12 ( 1? )( 1-
		swap c@+
		$80 ( 1? )(
			over and? ( ink@ a!+ )( 4 a+ )
			2/ ) 2drop
		pick2 a+
		swap )
	3drop ;
```

first, set the register A with the real direcction of the framebuffer for the position ccx,ccy. Then calculate the position of bitmap font for char C, this is, 12 byte for char, multiply for C, and added the start of romfont.
For speed reasons, I calculate the increment of line in a stack, this is sw minus 8 pixels with in dwords (2 <<).

Ready for count the 12 lines, and inner, the 8 bits, but last counter I use the mask for extract the bit, if the bit is on put the pixel ink@ a!+ or skip the pixel 4 a+, remeber all the adress are in bytes!. when 1 line is complete down the line with the precalculate value.

See I not use limits comprobations, I try to avoid this or make this checks in upper levels, but be warning, if you draw a text with this font ouside the screen... crash!

## More Fonts

There are many other fonts, like lib/fontj.txt, the bitmap font of sinclair microcomputers. Some variable width bitmap fonts, like Arial, Verdana, Lucida, Times and DejaVu is some sizes, all in bitmap form.

The last incarnations are the antialias bitmap monospace font, lib/fontm.txt for draw and you need include the separate romfont, for example:

```
^r4/lib/fontm.txt
^inc/fntm/droidsans13.txt

:test
	'fontdroidsans13 fontm
	"Hola" print
	;
```

And the vector exported form svg font, lib/rfont.txt, with the separate romfont too. Here you can specify size of font!.

```
^r4/lib/rfont.txt
^inc/rft/gooddog.rft

:test
	gooddog 0.2 %s rfont!
	"Hello Dog!" printc
	;
```

Here, the `0.2 %s` is 20% of screen heigth, and then the size is a rotio of screen size. A constant number can be defined.

## Print

The connection with print is in lib/print.txt, filling vector and vars for draw every char with EMIT or iterate over a string for PRINT, some cursor and selection words are defined here.
You can change the font and print when you like. Remenber, the vector fonts are scalable, the bitmap font not!.

## Usage

Demo in demos/test-font.txt,
