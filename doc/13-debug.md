# Debug in :r4

One of the characteristics of the forth is the interactivity, this produces a gradual development word by word,: r4 has no interactivity, although it is possible to add it and at some point it will have an interactive ide, so when a program is executed and this fails it is a little more difficult to find the error.
The first word to debug a program is `trace`, when the corresponding library is included it is easy to stop the program and see some higher cells of the data stack. Many times this is enough to find where it is failing

## Trace library

`^r4/lib/trace.txt`

The trace library adds some words to stop the execution and inspect the stack or a portion of memory.

```
::memmap	| inimem --
::trace	| --
::=trace	| nro --
::<trace	| nro --
::>trace	| nro --
::log	| "" --
```

`Memmap` show a dump of memory and conditional trace stop when reach some value in stack, `log` make a file with a log for the purpose of debugging.

## Step by Step Debugger



