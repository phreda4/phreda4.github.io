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

The shoot particle systema work wiht two words, add a shoot (+disparo) add a particle with behavior disparo and in xp,yp position, calc angle with velocity 0.25 + for horizontal to vertical translation and scale 5 >> for adjust the velocity.

The word disparo call by particle drawing with the adress of current particle, if you leave a 0 in the stack, then the particle is erased from list.

See how get x and y , if escape from screen delete ( 1.0 >? ) and if hit with any alien (p.in2d?) the remove the alien and add the explotion.

Calculate the new position with the velocity and finally draw the shoot.

```
:disparo | adr --
	>b
	20 qdim
	b@+ b@+
	1.0 >? ( 2drop 0 ; )
	2dup 'aliens 0.06 p.in2d? 1? ( 'aliens p.del +explosion 0 ; )( drop )
	fpos
	b@+ b> 12 - +!
	b@+ b> 12 - +!
	'bala b@+ rvesprite
	;

:+disparo
	'disparo 'shoot p!+ >a
	xp a!+ yp a!+
	xv 3 << neg 0.25 + sincos
	5 >> a!+ 5 >> a!+
	0 a!+
	;
```

The alien word add an alien with a random number.

The addalien add to particle list aliens the x randomized and upper in screen with velocity random in x and -0.01 in vy, choose a draw too.

The dalien1 word execute in every particle alien, leave 0 in stack when need remove, this is when alien is outside of the screen or is hit with the player, for now call to exit but this you can lose one live and so on.

Here calculate the positions with velocity andfinaly draw the sprite.

```
:dalien1 | adr --
	>b
	80 qdim
	b@+
	-1.0 <? ( drop 0 ; ) 1.0 >? ( drop 0 ; )
	b@+
	-1.0 <? ( 2drop 0 ; )
	over xp - over yp - distfast 0.1 <? ( 3drop exit ; ) drop
	fpos
	b@+ b> 12 - +!
	b@+ b> 12 - +!
	b@+ b@+ rvesprite
	;

:addalien
	'dalien1 'aliens p!+ >a
	r1 a!+ 1.0 a!+
	r.001 a!+ -0.01  a!+
	rand %10000 and? ( 'alien1 )( 'alien2 ) a!+ drop
	0 a!+
	;

:alien
	rand
	$1f00 nand? ( addalien ) drop
	;
```
