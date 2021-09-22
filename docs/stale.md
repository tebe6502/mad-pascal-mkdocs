#

## [CONST](https://www.freepascal.org/docs-html/ref/refse9.html)

Słowo `CONST` rozpoczyna sekcję deklaracji stałych.

Do deklaracji stałych `CONST` służy znak `=`. Dopuszczalne jest użycie operatorów, niektórych funkcji i stałych:

`+` `-` `*` `/` `not` `and` `or` `div` `mod` `ord` `chr` `sizeof` `pi`


```delphi
const
  e = 2.7182818;       { Real type constant }
  f : single = 3.14;   { Single type constant }
  a = 2;               { Ordinal BYTE type constant }
  c = '4';             { Character type constant }
  s = 'atari';         { String type constant }
  sc = chr(32);
  ls = SizeOf(cardinal);
```

Kompilator na podstawie wartości dobiera typ dla deklarowanych stałych. Dla wartości *0..255* będzie to typ `BYTE`, *256..65535* typ `WORD`, *-128..127* typ `SHORTINT` itd.


Istnieje możliwość określenia typu przez programistę,

```delphi
const
  x: word = 5;         { wymuszenie typu stałej }
  b: byte = -11;
```