# 3d Math

:R4 has a 3d matrix stack for calculate points in 3d, in the similar way of opengl, you can make operation with this stack and then calculate point in 3d space.

The lib for make this is r4/llib/3dmat.txt

the var mati is not exported, is 4x4 matrix with the identity and continue in var mats with room for 20 levels.

```
#mati | matrix id
1.0 0 0 0		| 1.0 = $10000
0 1.0 0 0
0 0 1.0 0
0 0 0 1.0
#mats )( 1280 | 20 matrices
#:mat> 'mats

::matini
	'mats dup 'mat> ! 'mati 16 move ;

::mpush | --
	mat> dup 64 + dup 'mat> ! swap 16 move ;

::mpop | --
	mat> |'mats =? ( drop ; )
	64 - 'mat> ! ;

::nmpop | n --
	6 << mat> swap - |'mats <? ( 'mats nip )
	'mat> ! ;
```


