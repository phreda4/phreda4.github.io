# Math

All in :r4 is an integer, the floating point aproach to modeling system is a bad decision, when write a system you can take the minimal step and make this the unity or use fixed point number, controling the limits of quantity of bit used, in :r4 use 16.16 for convert from source code, but you can make any representation or mixed.

## Fixed Point Math

The addition and subtraction of numbers in fixed point is equal to the addition and subtraction of the integers, the difference is in the multiplication and division.

In Source code you can write 1.0 and the compiler traslate to $1000, the 16.16 format, 2 words of main diccionary make the calculations easy:

```
*>>		| a b c -- d	d=(a*b)>>c	;64 bits
<</		| a b c -- d	d(a<<c)/b	;64 bits
```

These words have an immediate translation to asm, in the case of * >> what it does is multiply two numbers and then do a right shift without losing the upper bits, the result of the multiplication is saved in 64 bits. in << / the number is transformed into 64 bits by moving it to the left and then dividing so as not to lose the lower bits.

These words enabled an optimization in the generation of code that is normally done in low level, which is to convert the divisions by constants into multiplications with shift, I am not sure of this but I think it is the only language that can do this.

in lib/math.txt I define some word for help work with fixed point numbers. See the definition of *. and /.

```
::*.	| a b -- c
	16 *>> dup 31 >> - ;

::/.	| a b -- c
	16 <</ ;
```

in *. include and adjust for negative multiply, if you use only positive not need this, but if you use negative, the rigth shift of -1 is not 0 !, for this need this correction.

Also here are the words for sin, cos, tan, originally taken from Charles Moore's code and converted to use spins instead of radians, this simplification avoids the use of MOD, an expensive division, and is also much simpler to think, 0.5 is half a turn, etc.

There are a definition of atan2, sqrt. , ln. and exp. and some words with bit-tricks for integer like clamps or min, max.



