# Bitmaps

Only with the access to the framebuffer was the handling of bitmaps built, to be used as sprites or for the management of images.

## Bitmaps Sprites

The handle of bitmap images are defined in lib/bmr.txt.


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


