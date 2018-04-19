# Start

EVERYTHING, inside the computer, is a number.
Although the computer works only with numbers,
There are ways to convert these numbers in:

letters, using the ASCII code

```
65 -> "A"
66 -> "B"
etc.
```

images, dividing the image into points (pixels) and matching a number to a color intensity

```
255,  0,  0 red color
0  ,255,  0 green color
0  ,  0,  0 black color
255,255,255 white color
etc.
```

sounds, by recording the sound waveform
It is definitely a sequence of numbers

The computer does two things with these numbers

You can Store and Remember these numbers in your Memory:
The most accurate analogy is a drawer where I keep a number that can be read at another time

You can perform calculations with numbers:
It refers to perform mathematical calculations, addition, subtraction, etc. .. with these numbers.
These calculations are in sequence, one behind the other and this sequence can be repeated or skipped according to conditions.

These actions will be performed or executed by the computer when we give you the ORDERS to do them.
These orders or actions in sequence will be the code of our program.

in summary, we will define

#DATA and :ACTIONS

that constitute the code to be executed by the computer

The main objective of the language is to facilitate programming.
This language will be translated into the real code that the computer uses to function.

We will use WORDS as the basis of language.
we define a word as a set of characters separated by space
example
```
POSITION TO RUN
```

and also
```
= GREETINGS? +
```

but
```
23 4.3
```

although they are words, they are also valid numbers.

The language has a DICTIONARY, where words are defined with specific actions,
for example

the word `+` will add two numbers.

the word `@` will get a number that is in the memory.

Words that are numbers are not in the dictionary but have a defined action:
they are stacked in a memory called DATA STACK

We will also use PREFIXES to indicate different actions.
for example the prefix $ defines number in base 16, so $ff is number 255 in base 10, therefore it will be stacked.

The words that will define the program will be executed in order
for example
```
2 2 +
```
stack the 2 then stack another 2 and then add these numbers.

Every word that is in the code and not a number will be searched in the dictionary,
if it is not found it means that the program is badly written.

EVERY word must be defined before being used.

we can define words with the two most important prefixes

```
#FACT
```
defines the word DATA as a place of memory, also called variable DATA

```
:ACTION
```
defines the word ACCION as a set of actions to execute

The prefix ":" only defines the starting point of the program, here the execution begins:
The word ; defines the end of a definition or the end of the program.
Then the complete example is left

```
: 2 2+ ;
```

The program will be a set of definitions of data and actions and at the end will be the starting point of the program

```
#points

:add 1 'points +! ;

add add ;
```

First we define the variable `points`, initially it will contain a 0.

Then we define the word `add`, what you will add is to execute the following words until you find a `;`
In this case it will be 1 that will stack the number 1 `'points` using the prefix "'" stacks the address of the variable `points`
`+!` add 1 to the variable points
`;` end of definition and word

At the end of the program is the starting point, which here repeats the word add twice and then ends the program.
The idea of this code is to add the number 1 to the variable points twice, at the end of the execution `points` will contain the number 2.

# The Data Stack

The actions will be operations on numbers, taking and leaving numbers from memory.
The data stack can be viewed as a short-term memory, to calculate or to pass this data between words.

Part of the job of making a program is to plan what happens in this stack.

We have words to reorder the stack, to add numbers and to remove.

We already saw that a number is stacked when it is executed.

We can also add values to the data stack
doubling the top with `DUP`
or the second with `OVER`
or the third, fourth, fifth
with `PICK2`, `PICK3`, `PICK4`
Here we stop, in case you need to copy a deeper value in the stack,
it is an indication that our code is unnecessarily complicated, surely there is an easier way to do the same.
There are many more indicators of this but the order of the stack is the main one ..

Note that we are copying the value .. it's like saying ..
we will do something with this number but we need the original value too ..

We can delete items from the stack with `DROP` or `NIP`.

We can change the order of the elements with `SWAP` or `ROT`.

Sort the stack so that the calculations flow without many words of stack.

All the arithmetic and logical operations will be performed on the numbers that are in the stack
the word
`+`, `-`, `*`, `/`, `MOD`, `AND`, `OR`, `XOR`
take the two numbers at the top of the stack and stack the result
We also have operations like
`NOT`, `NEG`, `ABS`, `SQRT` that modify only the top of the stack

# Control structures

They are the parts of the code that are executed in a certain condition or that are repeated.
These parts of code will be called blocks and we will delimit them with parentheses
Using also the word) (to indicate the end of one block and the beginning of another.

The conditional words are going to check the top of the stack or the first two numbers
completing the operation of these control structures

The simple conditionals are
```
0?
1?
+?
-?
```

The compound conditionals are
```
<?
>?
<=?
>=?
<>?
AND?
```

These conditionals must be stuck to the blocks

## IF, IF ELSE
Execute the block if the condition is met
Execute block A if the condition is met or block B if it is not met
less than 5 or greater than 8 or none

```
5 <? ( "less than 5" print )( 8 >? ( "Greater than 8" print )( "none" print ) )
```

use the word cut to finish the execution

```
5 <? ("less than 5" print ; )
8 >? ("greater than 8" print ; )
"none" print
```

## WHILE
repeat the block while the condition is met
count from 0 to 9
```
0 ( 10 <? )( 1+ ) drop
```

count from 10 to 1
```
10 ( 1? )( 1- ) drop
```

traverse a string ending in 0
```
( c@+ 1? )( emit ) drop
```

## UNTIL
Repeat the block until the condition is met
Although less frequent this
```
10 ( 1- 0? )
```

## REPEAT
Repeat forever
```
( loop )
```

# Variables and Memory

As we saw, the program is composed of DATA + ACTION

About the data
We can define memory to save data in 3 different ways.
- Defining variables
```
#position 3
#list 1 2 3 4 0
```

- Defining memory blocks
```
#drawing )( $ffff
```

- Using free memory
```
Mem 'memory !
```

The main words to use the memory are:
fetch
```
@ | address -- value
```

takes from the stack a direction in the memory, obtains the value that is there and stacks it
and store
```
! | address value --
```

saves in the direction the value that is in the stack