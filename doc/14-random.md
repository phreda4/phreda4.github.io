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
