# Vector Editor

There are many version of vector sprites, the last is the `.vsp` files, previous are `.spr`
The last editor is `system/edit-ves.txt`. Some capabilities are missing, and there are some errors but you can import drawings from `.svg` files, not full compatible.

When the editor is called with a .vsp file what is executed is, first, the system of denomination of variables, the copy, the deletion, the import of svg and just when a single drawing is edited the editor itself is called.

<img src="../gif/vesedit.gif">

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
	1 max / >r
	swap neg r@ 16 *>> swap r> 16 *>>
	fmat fcen b> ;
:a3 gc>xy fcen @+ gc>xy fmat ;
:a4 xp $80000000 <>? ( yp pline )( drop )
	gc>xy 2dup 'yp !+ ! op ;
:a5 gc>xy pline ; | linea
:a6 swap >b gc>xy b@+ gc>xy pcurve b> ;
:a7 swap >b gc>xy b@+ gc>xy b@+ gc>xy pcurve3 b> ;
:a8 gc>xy op ;
:a9 gc>xy line ;
:aa swap >b gc>xy b@+ gc>xy curve b> ;
:ab swap >b gc>xy b@+ gc>xy b@+ gc>xy curve3 b> ;
:ac 8 >> ink sfill finpoli ;
:ad 8 >> ink@ fcol lfill finpoli ;
:ae 8 >> ink@ fcol rfill finpoli ;
:af 8 >> 2 << paltex + @ tfill finpoli ;
```

you can see, for a0 is a nothing word, a1 is the set of color in the upper 24 bit of the integer read from memory.
a2 and a3 is a complex word for define 2d matrix for shader.
a4 is the op, then decode the 2 coordinate from the 32 bit integers and use OP for set the origin point of draw. the decode word gc>xy is define in lib/convert.txt, you can see

```
| 14 - 14 - 4
| 0000 0000 0000 00 | 00 0000 0000 0000 | 0000
| x                   y                   control
```

this is 14 bits for x coord, 14 bits for y coord and 4 bits for control, the jump table. this is how store the info of every point.
You see too how trace the last `PLINE` with a xp,yp saved for close the polygons.
a5 make a pline, a6 make a curve, etc.
then a files who store this sprites are .vsp, some others definitions are used, the old ones, like .spr

The editor call the actual editr for this variables when you press CTRL-E in the line of include, type a line

```
^./test.vsp
```

and hit CRTL-E in the editor and see what happen.
