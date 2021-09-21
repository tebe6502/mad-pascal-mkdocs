#

## FOR

Wartość licznika po zakończeniu działania instrukcji iteracyjnej `FOR` w `FPC` będzie równa wartości jaka została określona dla maksymalnej wartości licznika. W przypadku `MP` wartość będzie większa o +1.

Przykład:
```delphi
for i:=0 to 10 do;
```

`FPC` po zakończeniu petli **i = 10**, `MP` **i = 11**


## PARAMETRY FUNKCJI, PROCEDURY

Dla poniższego przykładu `FPC` wygeneruje inny wynik niż `MP`

```delphi
var i: byte;

function ran(a: smallint): byte;
begin

 ran := i;

 inc(i, 3+a);

end;

procedure kefrens(a,b: byte);
begin

 writeln(a);
 writeln(b);

end;


begin

Kefrens(ran(5)+3, ran(3)+5);
```

`FPC`, `Delphi` oblicza parametry procedury/funkcji od prawej strony do lewej (wynik dla w/w przykładu 9,5).

`MP` oblicza parametry procedury/funkcji od lewej do prawej (wynik dla w/w przykładu 3, 13)


## STRINGI W PAMIĘCI

```delphi
 writeln('ala','ma',kota');
 writeln('ala','ma','psa');
```

Kompilator tylko raz odłoży w pamięci ciągi znakowe 'ala', 'ma'. Dzięki rozbiciu dłuższego stringa na mniejsze elementy możemy oszczędzić pamięć.


## ZNAKI W PAMIĘCI

```delphi
writeln(#69,#82,#82,#32, a);	//
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

Aby pozbyć się takiej sytuacja należy ustawic na sztywno DATAORIGIN (-data:ADDRESS)

## [USES](../moduly/#uses)

Kolejność modułów na liście USES może mieć znaczenie.
