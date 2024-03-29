#

## [UNIT](https://www.freepascal.org/docs-html/ref/refse111.html#x224-24600016.2)

Moduły `UNIT` w **MP** składają się z sekcji `INTERFACE` **wymagana**, `IMPLEMENTATION` **wymagana**, `INITIALIZATION` *opcjonalna*.


```delphi
{
  Example UNIT
}
unit Unit1;

interface

uses // List of unit dependencies goes here...
     
// Interface section goes here

implementation

uses // List of unit dependencies goes here...

// Implementation of procedures, and functions goes here...

initialization

// Unit initialization code goes here...

end.
```

Przykład:
```delphi
unit test;

interface

type  TUInt24 =
  record
    byte0: byte;
    byte1: byte;
    byte2: byte;
  end;

const
  LoRes = 1;
  MedRes = 2;
  HiRes = 3;

  procedure Print(a: string);

implementation

uses test2;

procedure Print(a: string);
begin

  writeln(a);

end;

end.
```

## [USES](https://wiki.freepascal.org/Uses)

Do deklaracji wybranego modułu służy instrukcja `USES` np.:

    uses atari in 'd:\units\atari.pas';

    uses crt, sysutils, atari;

Moduły są odczytywane od końca listy `USES` (od prawej do lewej strony), dla w/w przykładu najpierw domyślnie `SYSTEM` następnie `ATARI` `SYSUTILS` `CRT`

Kolejność modułów na liście `USES` może mieć znaczenie w przypadku używania procedur/funkcji o tych samych nazwach, kolejne moduły przykrywają poprzednie np.:

    uses graph, vbxe;

W obu modułach `GRAPH` i `VBXE` występuje procedura `SetColor` oraz `Line`, dla w/w przykładu odwołania do tych procedur będą realizowane przez moduł `GRAPH`

W przypadku

    uses vbxe, graph;

odwołania do procedur będą realizowane przez moduł VBXE.

Możemy też bezpośrednio wywołać procedurę/funkcję z konkretnego modułu, np.:

    vbxe.Line
    graph.Line
    vbxe.SetColor
    graph.SetColor


## [$LIBRARYPATH](https://www.freepascal.org/docs-html/prog/progsu99.html)

```delphi
{$LIBRARYPATH path1; path2; ...}
```
Dyrektywa `$LIBRARYPATH` pozwala wskazać dodatkowe ścieżki poszukiwań modułów `UNIT` zadeklarowanych przez `USES`.


## [$UNITPATH](https://www.freepascal.org/docs-html/prog/progsu119.html)

```delphi
{$UNITPATH path1; path2; ...}
```
Dyrektywa `$UNITPATH` pozwala wskazać dodatkowe ścieżki poszukiwań modułów `UNIT` zadeklarowanych przez `USES`.
