# Graphics User Interface

## Immediate Mode User Interface

After several versions of building a gui for :r4 I found the one type of GUI called IMGUI, this fits perfectly with the concept of forth. The idea here is to draw the interface while the interaction is calculated, that is, there is no way to save but the behavior is being executed while drawing.

The implentatios is simple, every gui element have an id, in the case of button, if the mouse enter in this element, the id is store and if the mouse unclick this id, the vector of execution is called.

The zones are managed with the gc library: xc,yc and w,h shared by sprites and others.

The basic things are in lib/gui.txt and in lib/btn.txt there are diferent buttons.

A simple usageof `tbnt`, the stack usage is `'vector "string"`.

```
^r4/lib/btn.txt

#value

:main
	show clrscr
		$ffffff ink
		value "value:%d" print cr cr
		$ff0000 ink
		'exit dup >esc< "Exit" btnt cr cr
		cr
		$ff ink
		0 ( 10 <? )( sp
			[ dup 'value ! ; ] over "hit %d" mprint btnt cr cr
			1+ ) drop
		cminiflecha ;

: main ;
```

See the power of IMGUI, we can define a dynamic quantity of button, the dynamic label can be made with `mprint`, a memory version of `print`, leave in stack a memory adress with the formated string, and the `dup` in the vector of button use the top of stack of loop for set the var `value`.


