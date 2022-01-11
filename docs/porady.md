#

## FOR

Wartość licznika po zakończeniu działania instrukcji iteracyjnej `FOR` w **FPC** będzie równa wartości jaka została określona dla maksymalnej wartości licznika. W przypadku **MP** wartość będzie większa o `+1`.

Przykład:
```delphi
for i:=0 to 10 do;
```

**FPC** po zakończeniu petli `i = 10`, **MP** `i = 11`

## SHL

**FPC** podaje inne wyniki dla SHL które przekracza rozmiar danego typu, np.:

```delphi
i: byte;
c: cardinal;

i:=1;
c:=i shl 33;
```

Dla w/w przykładu **FPC** zwróci wartość `c = 2`, **MP** zwróci `c = 0`.

Natomiast dla:
```delphi
c:=1 shl 33;
```

**FPC** zwróci `c = 0`, tak samo jak **MP*.


## STRINGI W PAMIĘCI

```delphi
 writeln('ala','ma','kota');
 writeln('ala','ma','psa');
```

Kompilator tylko raz odłoży w pamięci ciągi znakowe `ala`, `ma`. Dzięki rozbiciu dłuższego stringa na mniejsze elementy możemy oszczędzić pamięć.


## ZNAKI W PAMIĘCI

```delphi
writeln(#69,#82,#82,#32, a);
```
Kody znaków oddzielone przecinkiem nie zostaną potraktowane jako ciągi znakowe które kompilator zapisuje do stałych.


## KRÓTSZE WARUNKI BOOLEAN

```delphi
 if spanbelow = true then ;
```

można zastąpić

```delphi
 if spanbelow then ;
```

## INFINITE LOOP

Gdy assemblacja pliku *.a65 powoduje 'Infinite loop', plik OBX zostaje zapisany ale jest uszkodzony.

Aby pozbyć się takiej sytuacja należy ustawic stały adres dla `DATAORIGIN` (-data:ADDRESS)

## [USES](../moduly/#uses)

Kolejność modułów na liście `USES` może mieć znaczenie.
