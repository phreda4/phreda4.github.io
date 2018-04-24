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

See here a stack definition, not limit control.

```
#stack )( $fff
#stack> 'stack

:push | v --
	stack> !+ 'stack> ! ;
:pop | -- v
	stack> 4 - dup 'stack> ! @ ;
```

