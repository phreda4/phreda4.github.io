# Vector Graphics

:r4 born for an experiment, I make a animation program for wince called redamation, some complication apear and try to remediate the problem separating the layer of drawing of the layer of program, I start to learn forth and try to add a programable unit in this animation program in forth. This days have good opinion of flash and then try to use vector graphics for all.

First to all, :r4 not have console, only a framebuffer, the words for use are

```
SW 	| -- w		Screen Width
SH 	| -- h		Screen height
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

The Sprite is encoded in 32 bits numbers with 0 terminate, like a string. In every cell 4 have 4 bits operation and two 14 bits coordinates or 24 bits color.

The lib is `lib/vesprite.txt`, you can draw with pos/dim words:

```
::vesprite | 'adr --
```

with rotation:

```
::rvesprite | 'adr ang --
```

in 3d, with 3dmath stack:

```
::3dvesprite | 'adr --
```

and full interpolate:

```
::vespriteInter | n s1 s2 s3 --
```


## Where draw

The coordinates of the sprites are calculated with the center in xc,yc and w and h for width and height, this variables control the position and scale of the draw. There are many words for controling this like `POS` or `DIM` or `QDIM`, this only set this variables.

Another word of sprite drawing use the coordinates with a rotation and simply make turn the draw.
Another use 3d perspective calculation and then the sprite is in 3d too!

