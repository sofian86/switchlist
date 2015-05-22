# Introduction #

SwitchList has two ways to store its data: .swl files (used up through version 1.3) and .switchlist files (used after 1.3).  .swl files are XML - text and mostly-human readable.  Using XML has limitations - iOS apps can't read them, so we could never write an iPad version of SwitchList with the old file format.

.switchlist files are actually little databases using the SQLite database.  These are still readable, but require using the "sqlite3" tool to examine the database.

Here are some cookbook solutions for examining a .switchlist file, or converting it to another form.

This page is only needed by folks writing the SwitchList program itself, or who want to copy the information from SwitchList to another program or another form.

# Details #

To view a database, you use the sqlite3 tool.  Run
```
sqlite3 path-to-switchlist-file
```

".tables" shows the database tables inside the SwitchList file; - there's one for each major thing in switchlist.  Freight cars (ZFREIGHTCAR), cargos (ZCARGO), places (ZPLACE), etc.  Each major object has its own database, always prefixed with Z.

".schema" shows the structure behind the database, and names all the different "columns" of information.  (It's in cryptic form, but it'll give a clue.)  ZFREIGHTCAR has columns for reporting marks (ZREPORTINGMARKS), current industry location (ZCURRENTLOCATION), etc.  You can print the information in a schema using the SELECT command.  Dumping all information about freight cars can be done with:
```
select * from ZFREIGHTCAR;
```
To get a list of all freight car reporting marks, you can specify the column:
```
select ZREPORTINGMARKS, ZCARTYPEREL from ZFREIGHTCAR order by ZREPORTINGMARKS;
```

See also the 

&lt;A href="http://www.sqlite.org/lang\_select.html"&gt;

SQLite Query Language

Unknown end tag for &lt;/a&gt;

 docs.