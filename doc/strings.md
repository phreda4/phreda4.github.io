# Strings

The way in which the string is used in :r4 is not defined in the base of the language, this management can be rewritten in another way but the current development has the following characteristics.

Strings are array of characters ending in 0, that is, the null character marks the end of the string, there is a discussion about which representation is better and originally the FORTH prefers the other representation where the length of the string is indicated at the beginning , this, in my opinion, has 3 cons.

First, to save its length you have to define how many bits are to be used for that value, if it is saved in 8 bits, the maximum length of a string will be 255 and so, with the representation used by 0 this does not happen, the length can be as far as you can memory have.

Second, the operation with string usually involves traversing it in some way, character to character, if you have a length and a direction I will need a record more than if I use only the address, each character I will have to read it anyway and this simplify this loop.

Third, it is the form used in all the operating systems of today, so it is not necessary to convert anything when a communication is made with an api.

When discussing this topic, it is generally stated as an argument in favor that the aggregate operation at the end of the string is more optimal in the representation with a tongue, this can be avoided by keeping this last position.

In fact, in the `mem.txt` library we have the word `::strcpyl` that exactly what it does is to return the last place of the copy where you can chain the following characters to add, at no cost to go through again.

One of the most confusing ideas in dynamic languages is the definition of a string that can change in size, this generates a false image of what really happens inside the computer, that is, there is no memory that changes in size, the memory is always the same, it is quiet and has the same address (avoid talking about page and segmentation). In fact this functionality that is added to the language produces the dreaded garbage collector, and we can already know that if there is a garbage collector it is because someone is generating it, US!.

To avoid this way of thinking it is convenient to presuppose the opposite, every string is fixed and is in a unique place, if we need that this will vary we will have to build the buffer or take a part of free memory with a maximum size that produces this operation, Contrary to what one might think that this exposure of internal mechanisms will complicate the problem, the opposite occurs. Thanks to this exhibition, we have available mechanisms that, with few modifications, may find some additional characteristic that is desired, when access to this level of detail is not available, many times it is necessary to build on this mechanism, another even more. complicated.

