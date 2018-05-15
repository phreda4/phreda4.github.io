# Memory Data Base

Library for memory database with char separators, variable length registers.

`^r4/lib/db2.txt`

Library for make form for edit, include db2.

`^r4/lib/formkit.txt`

## Database Definition

The database is a collection of records separated by the character `~`, each field is separated by the character `|` and within the field you can separate value with the character `_`, this definition is arbitrary, and you could have chosen non-printable characters but you tried to simplify to edit the files with a common text editor

See the demo in apps/database agenda.

```
#dbagenda 0 "db/agenda/agenda.db"

#fkagenda 0 "db/agenda/agenda.fk"

#gragenda 0 0
$400 $201 $202 $203 $304 0
"Nombre|Móvil|Particular|Trabajo|e-mail"
```

`dbagenda` define the file for store the db, `fkagenda` define the form for entry the datas and `gragenda` define a grid for view the db.
The file `db/agenda/agenda.fk` define the field with type, for edit, and size. Remember the fields are positional, not for name, the name is decorative.

```
Agenda~
Nombre|A80~
Móvil|A20|9999999999~ Particular|A20|9999999999~ Trabajo|A20|9999999999~
Email|A40~ Dirección|A40~
```

load the db and the form:

```
	mark
	'fkagenda fkload
	'dbagenda dbload
```

## Log Version

There are words for make a db with a log file, every change in the db is append to a log file id disk and you can reconstruct the entire db or make a replicate with a timestamp.
