# Simple Shooter

Now, make a simple space shooter.

I use the `gui` for generic words, `vesprite` for vector graphics, `part16` for particle system, `penner` for penner animation (tranform inteval 0..1 in diferent times) and draws.vsp for make graphics, in this case i download a svg sheet of space graphics and import with the editor the selected ones.

```
^r4/lib/gui.txt
^r4/lib/vesprite.txt
^r4/lib/part16.txt
^r4/lib/penner.txt
^./draws.vsp
```

lets define 3 particle systems, for aliens, for bullets and for fx, I use deferent collection because fx not collide with others, bullets collide with aliens.

```
#aliens 0 0
#shoot 0 0
#fx 0 0
```

The players is one, I define Y position, X position, X velocity and X aceletarion

```
#yp -0.8
#xp
#xv
#xa
```

Try to make modification to acceleration with keys and use the velocity for angle of ship too.

From botton to up.
Define memory for particles, a paper color and call main.

```
:memory
	$cccccc 'paper !
	mark
	200 'aliens p.ini
	200 'shoot p.ini
	200 'fx p.ini
	;

: memory main ;
```

## The main word

For debuging put a 33 in the stack, and print the top of stack in the show  loop, if any word produce or consume more the this number change and is the alert for found a bug.

I draw the aliens, shoot and fx in this order, then the player an then control the new aliens, I add exit with escape for now.

```
:main
	33
	show clrscr
		negro
		dup "%d" print

		'aliens p.draw
		'shoot p.draw
		'fx p.draw

		player
		alien

		'exit >esc<
		;
```

## The Player

I define 80 pixels for the player, in xp and yp fpos, this word scale the real pixel width and height in the -1.0 .. 1.0 interval, this is for work in any screen resolution, the qdim need calculate in the same but for now set in 80.

Draw 'nave with velocity multiply by 8 ( 3 << ) like rotate, this make a nice effect.

Now calculate the position add velocity beetwen -0.9 and 0.9 and set to position.
The add aceleration to velocity between -0.006 to 0.006 ans set to velocity.

Then test the keyboard, <le>ft key and <ri>gth key for set the aceleration variable and set to 0 when release any of two keys, with space shoot.

```
:player
	80 qdim
	xp yp fpos
	'nave xv 3 << rvesprite
	xv xp + -0.9 max 0.9 min 'xp !
	xv xa +
	-0.006 max 0.006 min 'xv !
	[ -0.0004 'xa ! ; ] <le>
	[ 0.0004 'xa ! ; ] <ri>
	[ 0 'xa ! ; ] dup >le< >ri<
	'+disparo <spc>
	;
```

