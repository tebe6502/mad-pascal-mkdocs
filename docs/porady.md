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

Jeśli zamienimy ciąg znaków na ich kody, wówczas kompilator nie będzie ich przechowywał w obszarze stałych.

```delphi
 writeln('Atari');
 writeln(#.#.#.#.#.#.#.#.);
```


## ZNAKI W PAMIĘCI

```delphi
writeln(#69,#82,#82,#32, a);
```
Kody znaków oddzielone przecinkiem nie zostaną potraktowane jako ciągi znakowe które kompilator zapisuje do stałych.


## OPTYMALIZACJA WYRAŻEŃ

Należy unikać łączenia wyrażeń z funkcjami, w takich przypadkach nie zadziała optymalizacja kodu wynikowego, albo będzie ona tylko częściowa, np:

```delphi
function add(a,b: byte): byte;
begin
 Result := a + b;
end;

function mul(a,b: byte): byte;
begin
 Result := a * b;
end;

var x: byte;

x := add(7,9) + mul(8,5);
```

Najlepiej zastosować dodatkowe zmienne, w których zostanie zapisana wartość funkcji, np:

```delphi
var hlp1, hlp2: byte;

hlp1 := add(7,9);
hlp2 := mul(8,5);

x := hlp1 + hlp2;
```

## WARUNKI

```delphi
 var i: byte;
 var k: byte;
 var bol: Boolean;
 
 i:=0;
 k:=6;

 bol := i > 15 - (k * 3);
```

Dla w/w warunku wyrażenie po prawej stronie zostanie przekształcone do typu ze znakiem, następnie lewa strona wyrażenia zostanie sprowadzona do odpowiedniego typu ze znakiem.

Wynikiem porównania będzie `TRUE` ( `0 > -3` ).

Jeśli zależy nam aby wartościowanie wyrażenia było przeprowadzone na typie `BYTE` musimy wymusić to rzutowaniem, np:

```delphi
 i:=0;
 k:=6;
 
 bol := i > byte(15 - (k * 3));
```

Teraz wynikiem będzie `FALSE` ( `0 > 253` ).


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


## DISPLAY LIST

jako CONST, bo wtedy jest najbardziej na początku pamięci programu (~$2000)

