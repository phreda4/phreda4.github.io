# Animation

When have Vector sprites the next thing is make some animation with this.
First, you need include words for

^r4/lib/gui.txt
^r4/lib/vesprite.txt
^./draws.vsp


In the .vsp file, with CNTR-E key enter in the editor and draw something, called d1, for example an exit to editor.

In the point the file draws.vsp are created and if you open with an editor, you see the definition of the draw

#:d1 $2133123 .. ..  .. 0

The prefix #: export de definition of variable, now in the program we have the draw.

start the program:

: main ;

and before this definition add

#xp #yp  | position
#xv #yv	 | velocity

:keyboard
	[ -0.05 'yv ! ; ] <up>
	[ 0.05 'yv ! ; ] <dn>
	[ 0 'yv ! ; ] dup >dn< >up<
	[ -0.05 'xv ! ; ] <le>
	[ 0.05 'xv ! ; ] <ri>
	[ 0 'xv ! ; ] dup >le< >ri<
	'exit >esc<
	;

:draws
	xp yp fpos
	64 qdim
	'd1 vesprite
	xv 'xp +! yv 'yp +!
	;

:main
	0 'xp ! 0 'yp !
	0 'xv ! 0 'yv !
	show clrscr
		keyboard
		draws
		;


Now we have a basic program for move a sprite with the keyboard. Some notes:

We use fixed point number, this be usefull here for the position and the velocity, here the key are the word FPOS, this make a calulation with the real screen size and convert the fixed point coordinate to integer position.

We modify with keys the velocity and then add to position, this are for smooth moves of sprite, you see how when press UP the velocity is set and when release UP the velocity are reset, this admit diagonal moves too.

When draw the sprite use qdim, the size of sprite are 64 pixels, change this for diferenct sizes.

The main loop call to keyboard test and the drawing words, nothing more!



