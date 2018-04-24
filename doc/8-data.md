# Data Structures

Well, really the only data structure is the array in memory, but some constructions be common in many programs.

## Array with size

The array have 64kb of memory. Instead of having a number to indicate which place corresponds to the filling of the array, it is easier to have a place where it corresponds.

```
#array )( $ffff
#array> 'array

:,a | v --
	array> !+ 'array> ! ;
```

## Stack

See here a stack definition, not overflow test.

```
#stack )( $fff
#stack> 'stack

:push | v --
	stack> !+ 'stack> ! ;
:pop | -- v
	stack> 4 - dup 'stack> ! @ ;
```

## Particle System

A more complex example, We make a particle system:

```
^r4/lib/gui.txt
^r4/lib/vesprite.txt
^./draws.vsp

:p.create | size 'fx --
	here over 4+ !
	swap 6 << 'here +! | 16 values
:p.clear | 'fx --
	dup 4+ @ swap ! ;

:p.cnt | 'fx -- cnt
	@+ swap @ - 6 >> ;

:p!+ | 'fx -- adr
	dup @ 64 rot +! ;

:p.map | vec 'fx --
	dup @+ swap @
	( over <? )( pick3 exec 64 + ) 4drop ;
```

Every particle in this memory has 16 values, 6 << is 64 bytes. first the variable need 2 values, current mem and initial mem, see how store first free memory in the second value and increment here for reserve all the memory used, the define clear word, copy the initial mem in current mem, the p.create word not have have ;, then clear the particle at the end.

p.cnt return the actual cnt of particles, p!+ store new particle, return adress for fill with values!
p.map traverse the array with the correct adress, the draw or test or whatever.

```
#particles 0 0

:r0.01
	rand 0.01 mod ;

:addp
	'particles p!+ >a
	64 a!+
	0 a!+ 0 a!+ | x y
	r0.01 a!+ r0.01 a!+ | vx vy
	;

:show&move | adr -- adr
	dup >a
	a@+ qdim
	a@+ a@+ fpos
	a@+ a> 12 - +!
	a@+ a> 12 - +!
	'd1 vesprite
	;

:main
	mark
	1024 'particles p.create	| 1024 particles

	show clrscr
		'show&move 'particles p.map
		'addp <f1>
		'exit >esc<
	;

: main ;
```

hit F1 for add a particles!!
