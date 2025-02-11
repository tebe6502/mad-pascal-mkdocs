#

## [VAR](https://www.freepascal.org/docs-html/ref/refse22.html#x53-730004.2)

Słowo `VAR` rozpoczyna sekcję deklaracji zmiennych.

```delphi
var
    label: type;
    label: type = value;
```

```delphi
var
    a: word;
    b: byte = 1;
    c: Boolean = true;

    s: string = 'Atari';

    tb: array [0..3] of byte = (0,1,2,3);
```

### [VOLATILE](https://pl.wikipedia.org/wiki/Zmienna_ulotna)

Modyfikator `VOLATILE` oznacza zmienną jako tzw. ulotną. Oznaczenie jako `VOLATILE` powoduje wyłączenie optymalizacji kodu wynikowego dla tej zmiennej,
jest to przydatne w przypadku rejestrów sprzętowych których wartości mogą ulegać zmianie przy każdym kolejnym odczycie.


```delphi
var
  [volatile] joy   : byte absolute $ff08;
  pio              : byte absolute $fd30;

begin
  repeat
    pio := $ff;
    joy := 2;
    if (joy xor $ff) = 1 then writeln('UP');
  until false;
end.
```

### [STRIPED]()

Modyfikator `STRIPED` pozwala dla tablic nie przekraczających maksymalnej liczby elementów `256`, typu prostego (`WORD`, `SMALLINT`, `CARDINAL`, `INTEGER`,`SHORTREAL`, `REAL`, `POINTER`) zmienić sposób alokacji danych.

```delphi
var
  [striped] tab: array [0..255] of cardinal;

  adr.tab
  adr.tab+$100
  adr.tab+$200
  adr.tab+$300
```

## [ABSOLUTE]()

Modyfikator `ABSOLUTE` pozwala ustalić adres w pamięci dla zmiennych `VAR`.

```delphi
var
   a: byte absolute $0600;
   tb: array [0..255] of byte absolute $a000;

   tab: array [0..3] of byte;
   v: integer absolute tab;

   procedure test(var buf);
   var ptr: PByte absolute buf;
```

## [REGISTER]()

Modyfikator `REGISTER` ustala adres pamięci dla zmiennych `VAR` na stronie zerowej (maksymalnie można przydzielić 16 bajtów).

```delphi
var
   a: byte register;
   c: integer register;
```

> **UWAGA:**  
> _Z tego samego obszaru 16 bajtów strony zerowej korzysta kompilator alokując tam swoje programowe rejestry `EDX` `ECX` `EAX` dlatego użycie modyfikatora `REGISTER` nie jest możliwe kiedy procedura lub funkcja też używa `REGISTER`._


```delphi
procedure test(a,b,c: integer); register;
```