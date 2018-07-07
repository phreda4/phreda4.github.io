# Text Editor

The text editor is in `IDE/edit-code.txt`. This program can edit source code and call the compiler, the debuger or run the application.

The idea is very simple, only insert or modify the string in memory, load, save to disk and make some call to aditionary editors, there are syntax coloring, handle mouse and selection, leak undo and multiple files, for see the includes.

When the text is load, `:loadtxt` convert the end of line 10 13 to 13 if exist.
`:ram` build the memory map, and `:editor` is the show word, make the interactive behavior.

In lines from 44 to 69 is the basic words for modify the text.

```
:lins  | c --
	fuente> dup 1- $fuente over - 1+ cmove>
	1 '$fuente +!
:lover | c --
	fuente> c!+ dup 'fuente> !
	$fuente >? ( dup '$fuente ! ) drop
:0lin | --
	0 $fuente c! ;

#modo 'lins

:back
	fuente> fuente <=? ( drop ; )
	dup 1- c@ undobuffer> c!+ 'undobuffer> !
	dup 1- swap $fuente over - 1+ cmove
	-1 '$fuente +!
	-1 'fuente> +! ;

:del
	fuente>	$fuente >=? ( drop ; )
    1+ fuente <=? ( drop ; )
	9 over 1- c@ undobuffer> c!+ c!+ 'undobuffer> !
	dup 1- swap $fuente over - 1+ cmove
	-1 '$fuente +! ;
```

The art in the program is include in the source code, for this, try to use editor called from the code, this is done with Control-E keys. The idea is put the name in the source, put the cursor in this line and press Ctrl-E.

in line 244 `:controle' word call the apropiate editor for vectors graphics, bitmap and icons. Before call the editor, fill the info of file. See how store in file this info and how call the editor.

```
	dup "mem/inc-%w.mem" mprint savemem
	empty
	"r4/system/inc-%w.txt" mprint run ;
```

All the data are store in `mem/inc-*.mem` and all the editor are in `r4/system/` with the name `inc-*.txt`, the * is fill with the extension of file: bmr, vsp, inc
