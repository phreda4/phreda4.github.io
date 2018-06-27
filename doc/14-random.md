# Random words

In the library

`^r4/lib/rand.txt`

There are several words that generate randomness, all of them have a variable for seed and a word who generate the number and advance the seed.

```
#:seed8 12345
::rand8 | -- r8

#:seed 495090497
::rand | -- r32
::rerand | -- ; randomize with time

::mseed | seed --
::mrand | -- r32

::random | -- r32

::rndseed	| seed --
::rnd | -- r32

::rnd128 | -- r32
```

Diferents implementation have speed over randomness issues.

The first implementation rand8, only give a 8bits number, the others are full 32 bits ramdoms, with limits.

For make random fixed point numbers you can use a mask or a mod. For example: if you need a random number between 0.0 and 1.0 you can use:

```
:rand01 rand $ffff and ;
```

Or if you can need a random between 0 and 0.1 you can use:

```
:rand0.1 rand 0.1 mod abs ;
```

The last abs if for avoid negative numbers. If you like generate random numbers for -1.0 to 1.0 then:

```
:r1 rand 1.0 mod ;
```

