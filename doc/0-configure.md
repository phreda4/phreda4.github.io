# Virtual Machine

:r4 has a VM for execute the programs, this program is made in C and has a compile to bytecode, a bytecode executer and graphics rutines.

![Main](../img/0-main.png)

When you run r4.exe the system, you can configure with line arguments or the file r4.ini.
The options for configuration file, are:

```
w<SCRW> windows width
h<SCRH>	windows height
b remove border in window

f fullscreen

s no start the screen (silent mode)

p<PRINTER NAME> set printer to name

c<FILECODE> compile code
i<IMAGEN> build bytecodes
x<IMAGEN> execute bytecodes

? help (line argument)
```

The basic configuration is screen size, you can choose a windows wiht `w1024 h600` or a fullscreen with `f`.

For compile to bytecodes you can call r4 from a console or a bat file and make the image. The file `compiledebug.bat` is:

```
r4 cr4/ide/edit-code.txt idebuga.r4x
```

This code make `debuga.r4x` with the bytecodes from compile `r4/ide/edit-code.txt`.

There are some special files in the interpreter: if main.r4x exist then load and execute (not need compile), if not, main.txt is load, compile and execute. if there are an error the vm call to debug.r4x program to see the error.

Every `run` in the code stack the file executed and call the name to compile and execute, when finish this code reload the previous one.

