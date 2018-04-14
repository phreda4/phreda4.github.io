# Memory

The use of memory in: r4 is very different from the rest of the languages, the management is basic and easy, the real challenge is to build applications that are designed to use the complicated memory system in this easier.

The first criticism of memory management is that if there is a garbage collection in memory management, that is, if there is a collector, then there is a garbage generator and that is the program that is running!

The idea here is to use the memory directly, I have a memory address where the free zone begins and I will use this memory leaving a mark of use, when I want to return back to this mark.

This usage record is in a stack, so here is the first clarification, we will use memory but hierarchically, so if we are not going to use a part, this part will always be the last, and therefore, easily running the address of free memory we release this part.
Here the criticism can be: well, of course, if I do the same in another language it will be the same and if, in fact, it is better to think of forth as a way of thinking and not a language in particular, the language will help to use this way of thinking.
Another clarification in this regard can be exemplified with the ordering algorithms, to the question, which is the fastest ordering algorithm the answer is: none, not ordering is faster than ordering, it may seem silly but if one is careful with the choice of the algorithm that is developed and avoids the need to order with some change this can be extremely beneficial. It never hurts to think!

All the aplication you see in the developer package are made with this schema, not use a garbage collector at all, My friend Manuel Cornes implement Dynamic Memory Management but until now, is not used.

