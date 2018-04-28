# Bitmaps

Only with the access to the framebuffer was the handling of bitmaps built, to be used as sprites or for the management of images.
In: r4 I am trying to use the memory without having to modify it, for this the bitmap is not compressed, it can be compressed in the disk but not in memory. This is why the memory for the bitmaps is uncompressed and have a minimal header, one dword!
The first 12 bits are the height of the image, the next 12 bits are the width, then there are 4 bits to different types of bitmaps, then the bit if there is a color palette or not.
Only the first 2 are full implemented (scale and rotation), The rest is partially implemented.

type 0: 32 bit/pixel
type 1: 32 bit/pixel with transparent channel
type 2: 16 color 565 schema
type 3: 16 bits color 4444 with alpha channel
type 4: 8 bit/pixel
type 5: 8 bit/pixel with transparent channel
type 6: 4 bit/pixel
type 7: 4 bit/pixel with transparent channel
type 8: 3 bit/pixel
type 9: 3 bit/pixel with transparent channel
type A: 2 bit/pixel
type B: 2 bit/pixel with transparent channel
type C: 1 bit/pixel
type D: 1 bit/pixel with transparent channel
types E,F: free

## Bitmaps Sprites

The handle of bitmap images are defined in lib/bmr.txt. The main words are:

```
::bmr.draw | x y 'bmr --
::bmr.drawscale | x y 'bmr wr hr --
::bmr.drawr | x y r 'bmr --
::bmr.save | "" --
::bmr.load | "" --
```



## Image Format

In lib/loadimg.txt we define the word for load diferent types of image file format:

```
::loadimg | filename -- img
	".jpg" =pos 1? ( drop loadjpg ; ) drop
	".png" =pos 1? ( drop loadpng ; ) drop
	".bmp" =pos 1? ( drop loadbmp ; ) drop
	".tga" =pos 1? ( drop loadtga ; )
	2drop 0 ;
```

The real code for load this file are in lib/loadbmp.txt, lib/loadpng.txt, lib/loadjpg.txt, lib/loadtga.txt. This word load in memory in the internal format of bmr this files, some options of format not implemented!


