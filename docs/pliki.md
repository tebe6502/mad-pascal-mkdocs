# Operacje na plikach

## [FILE](../typy/#plikowe-binarne)

```delphi
var f: file;
```

## [TEXT](../typy/#plikowe-tekstowe)

```delphi
var f: text;            // zamiennie TEXT
    g: textfile;        //       lub TEXTFILE
```


## [ASSIGN](../biblioteki-podstawowe/#assign)


Przykład otwarcia kanału dla urządzenia **S:** (ekran) w celu wyprowadzania znaków w trybie `GRAPHICS 1`, `GRAPHICS 2`
```delphi
var scr: text;
    s: string = 'color COLOR ' + #155 + 'COLOR '* + 'color '*;

begin

 assign(scr, 'S:');      // koniecznie przed InitGraph
 rewrite(scr);           // koniecznie przed InitGraph

 InitGraph(2);           // Graphics 2
 
 GotoXY(1,6);
 
 write(scr, s);

 close(scr);

end.
```

## [RESET](../biblioteki-podstawowe/#reset)

```delphi
var t: text;

 reset(t);          // dla odczytu pliku tekstowego, brak dodatkowego parametru z rozmiarem rekordu
```

```delphi
var f: file;

 reset(t, 1);       // dla odczytu pliku binarnego (rekord = 1)
```

W przypadku braku podania długości rekordu dla pliku binarnego `FILE` zostanie przyjęta wartość domyślna =128

## [REWRITE](../biblioteki-podstawowe/#rewrite)

```delphi
var t: text;

 rewrite(t);        // dla zapisu pliku tekstowego, brak dodatkowego parametru z rozmiarem rekordu
```

```delphi
var f: file;

 rewrite(f, 1);     // dla zapisu pliku binarnego (rekord = 1)
```

W przypadku braku podania długości rekordu dla pliku binarnego `FILE` zostanie przyjęta wartość domyślna =128


## [APPEND](../biblioteki-podstawowe/#append)

```delphi
var
 t: text;

begin

 assign(t, 'D:TEXT.TXT');
 append(t);

 writeln(t, 'ATARI');

 writeln(t, 'C64');

 write(t, 'Amstrad');

 close(t);
```


## [BLOCKREAD](../biblioteki-podstawowe/#blockread)

```delphi
var f: file;
    pnt: pointer;

begin

 pnt:=pointer(dpeek(88));

 assign(f, 'D:FILENAME');
 reset(f, 1);
 blockread(f, pnt^, 8);
 close(f);

end.
```


```delphi
var f: file;
    buf: array [0..0] of byte;

begin

 buf:=pointer(dpeek(88));

 assign(f, 'D:FILENAME');
 reset(f, 1);
 
 blockread(f, buf, 8);
 close(f);

end.
```

## [BLOCKWRITE](../biblioteki-podstawowe/#blockwrite)

```delphi
var f: file;
    buf: array [0..0] of byte ABSOLUTE $bc40;

begin

 assign(f, 'D:FILENAME');
 rewrite(f, 1);
 
 blockwrite(f, buf, 40*24);
 close(f);

end.
```

## [CLOSE](../biblioteki-podstawowe/#close)

```delphi
begin

 assign(f, 'D:FILENAME');
 reset(f, 1);
 
 close(f);

end.
```
