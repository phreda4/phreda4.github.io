# Landscape

Simplicity is not a starting point, it requires analysis, testing and understanding. This language is made to find a simple solution to a problem, but this is not simple, try all ideas, usually the first ones are correct but, sometimes, you need a deeper knowledge or you can not see a certain part of it.

:r4 is brutally simple and therefore can be seen as something primitive, in fact someone can compare it with BF and maybe have a point of contact, but, although it seems otherwise, the idea is to simplify the development.

If something teaches forth is to distrust the preconceptions, anyone who learns FORTH would try to avoid the terrible gymnastics of controlling the data stack, many dialects immediately add the premises, only to save it's sport. Do not do it! The careful order that can be found from the data traveling from one word to the other is sometimes the key to everything, putting the name of the word is as important as finding the order of the parameters.

If you are in a hurry you can start by putting variables for any data and then find the way that these variables are combined in a place on the stack, sometimes I have put to translate code from C to :r4 and, although it is possible to do this translation almost without understanding what he is doing, as one sees that he is doing and manages to find a better way, the code begins to shrink.

It is preferable to recode that patching, but, rightly, FORTH has the ability to modify things at almost any level without requiring a big change, the absence of a legal language that defines what can be done and what does not, instead of complicating the development simplifies it.

Each system is composed of several levels, some closer to the machine and others more abstract, but beware! It is easy to define levels that are not necessary, overcomplicate and try to solve problems that do not exist are the worst mistakes one can make, Charles Mooro and Jeff Fox are clear about this, I strongly recommend reading everything they find about them.

When you think about :r4 I think that you are not aware of the real code that it generates, but I do, since my goal is to improve the language. I'm not sure if this can be seen for someone who does not think how is generating the code and for this I do explicit here: The most important abstraction is the address, the name of a direction can point to anything, a data, a code , a more complex structure, a drawing, etc ... this is the point of separation of language levels, where words are defined that build a level, for example at the level of bits, and then words that build above this level, for example a graphic icon.

## Words Names.

The words are case insensitive, since it does not make sense to give different meanings to words that are upper-case or low-case.

Each definition hides the previously defined name, so the search for the words in the compiler is done from the end to the beginning. The other form of hidding is the declaration of local or exported, in the source code each inclusion only makes visible the words defined with :: or #:, the other are invisible.

I also think that the problem of name pollination is overdimensioned, in fact when you define auxiliary names for a main word, you prefer to put short names and save the meaningful names to the keywords of the program.

It is a headache to redefine the basic word, today this is not proven but desirable an indication of the duplication of the name.

## Memory

Another problematic topic is the use of memory, here in the operating system there is an indication of how to handle it, of course, if you need to run several programs at the same time (for sure?), The focus on: r4 is to return to the piece of memory flat, I found after doing many programs that it is possible to do everything or almost everything using a stack to mark the memory blocks used, if designed carefully, it is only necessary to dynamically use one structure at a time, the last one.

There are two ways to reserve a memory space, first fixedly

```
#buffer )( 1024
```

and then dynamically

```
#buffer

here 'buffer!
1024 'here +!
```

It is very important to find the amount of memory that I will use for a structure, if it is given by a fixed factor it is the easiest, if it depends on a variable factor, we must find what value is approximate.

There is a type of programming problem that consists of declaring that you do not know how much a list of numbers can grow and you are invited to design a linked list, this approach has several problems: first, memory is limited, then, design a structure that It will take our requirement twice (since now I need to save the number and the pointer to the next one) and it is also unreal, it will never be able to handle more than half of the memory that could be used with a simple array.

You always have to define how much I will use for each thing, if I need more I will redefine this memory and if I find that I do not need so much I can reduce it, but maintaining the value of these limits is part of the program, not knowing is having an unknown in more the problem.