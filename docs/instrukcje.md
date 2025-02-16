#

## Warunkowe

### [case of else](https://www.freepascal.org/docs-html/ref/refsu55.html#x164-18600013.2.2)

Obecnie **Mad Pascal** akceptuje dla zmiennej `CASE` typy tylko o długości 1 bajta: `SHORTINT` `BYTE` `CHAR` `BOOLEAN`.

```delphi
case a of               // dla zmiennej A typu CHAR
  'A'..'Z': begin end;
  '0'..'9': begin end;
  '+','*': begin end;
end;
```

### [if then else](https://www.freepascal.org/docs-html/ref/refsu56.html#x165-18700013.2.3)

Instrukcje warunkowe `IF` mogą być zagnieżdżane. Wykorzystywane jest to przy budowie bardziej złożonych warunków.

## Iteracyjne

### [for to downto do](https://www.freepascal.org/docs-html/ref/refsu57.html)

```delphi
    FOR zmienna := { wartość początkowa } TO { wartość końcowa } DO { instrukcje do wykonania }
    FOR zmienna := { wartość końcowa } DOWNTO { wartość początkowa } DO { instrukcje do wykonania }
```

Instrukcja `FOR` służy do organizacji obliczeń, które będą wykonywane z góry określoną liczbą razy.

Zmienna sterująca musi być identyfikatorem typu porządkowego, a oba wyrażenia powinny być zgodne w sensie przypisania z typem zmiennej sterującej.

W czasie realizacji pętli `TO` zmiennej sterującej przypisywana jest następna wartość w danym typie, w pętli `DOWNTO` poprzednia.

Zabroniona jest *ręczna* zmiana wartości zmiennej sterującej. W przypadku takiej próby **Mad-Pascal** zasygnalizuje błąd **Illegal assignment to for-loop variable**.

Kompilator dba o to aby nie wystąpiła pętla bez końca, dlatego w przeciwieństwie do pętli `repeat` i `while` można bez obaw stosować taką pętlę:

```delphi
    for i:=0 to 255 do writeln(i);    // dla zmiennej I typu BYTE
```

Po zakończeniu działania pętli `FOR` jej licznik będzie miał wartość **+1** większą niż wskazuje zakres jaki podaliśmy, czyli dla:

```delphi
    for i:=0 to 10 do;
```

Wartość zmiennej `I` jaką odczytamy to `I = 11`, w przypadku `FPC` będzie to `I = 10`.


### [for in do](https://www.freepascal.org/docs-html/ref/refsu59.html)

```delphi
    FOR zmienna IN array DO {instrukcje do wykonania}
    FOR char IN 'string literal' DO {instrukcje do wykonania}
```

Konstrukcja `FOR IN DO`	pozwala na szybsze odczytywanie elementów tablic lub stałych tekstowych.

```delphi
    var
    days : array [0..6] of string =
    ('poniedzialek', 'wtorek', 'sroda' ,'czwartek', 'piatek', 'sobota', 'niedziela');
    
    a: string;

    begin
      for a in days do writeln(a);
```

### [while do](https://www.freepascal.org/docs-html/ref/refsu60.html#x169-19100013.2.7)

```delphi
    while { warunek } do { instrukcja do wykonania }
```

Konstrukcja ta służy do organizacji obliczeń, które będą wykonywane tak długo jak wyrażenie znajdujące się po słowie `WHILE` jest prawdą.

W przypadku pętli `WHILE` warunek jest sprawdzany bezpośrednio przed jej wykonaniem, co w rezultacie może spowodować, że taka pętla nigdy niezostanie wykonana.

```delphi
    while BlitterBusy do;   // oczekiwanie na zakończenie działania blittera VBXE
```

Ograniczenia dla instrukcji `WHILE`:

```delphi
    while i<=255 do inc(i); // pętla bez końca gdy zmienna I typu BYTE
```

### [repeat until](https://www.freepascal.org/docs-html/ref/refsu59.html#x168-19000013.2.6)

```delphi
    repeat
      { instrukcje do wykonania }
    until { warunek zakończenia }
```

Instrukcja ta wykonuje cyklicznie inne instrukcje zawarte pomiędzy słowami `REPEAT` i `UNTIL` do momentu gdy wyrażenie znajdujące się za słowem `UNTIL` nie przyjmie wartości `TRUE`.

Efekt zastosowania pętli `REPEAT` jest bardzo podobny do działania pętli `WHILE`. Jedyna różnica polega na tym, że w pętli `REPEAT` warunek zakończenia sprawdzany jest dopiero po wykonaniu instrukcji.

Oznacza to, że pętla `REPEAT` zawsze będzie wykonana co najmniej raz. Dopiero po pierwszej iteracji program sprawdzi, czy można zakończyć działanie pętli.

```delphi
    i:=0;
    repeat
      inc(i);
    until i=0;      // pętla wykona się 256 razy
```