#

## [PROGRAM](https://www.freepascal.org/docs-html/ref/refse111.html#x232-25600016.1)

Nagłówek `program` nie jest wymagany, jest dostarczany tylko w celu zapewnienia kompatybilności wstecznej z `Turbo Pascal`.

```
   program name [(parameters, ...)] [: address];
```

Możliwe jest określenie adresu kompilacji po znaku dwukropka `:`, jest to odpowiednik przełącznika z linii komend `-code address`.

```delphi
program test;

uses // List of unit dependencies goes here...
     
// Implementation of procedures, and functions goes here...

end.
```

## [UNIT](https://www.freepascal.org/docs-html/ref/refse111.html#x224-24600016.2)

> **UWAGA:**  
> _Nazwa pliku z modułem musi być zgodna co do wielkości liter z nazwą jaką wpisujemy po słowie UNIT._

Moduły `UNIT` występują tylko w postaci źródłowych plików `.pas`, nie można ich skompilować oddzielnie.

Moduły `UNIT` składają się z sekcji:

- `INTERFACE` **wymagana**
- `IMPLEMENTATION` **wymagana**
- `INITIALIZATION` *opcjonalna*.

```delphi
{
  Example UNIT
}
unit Unit1;       // -> filename 'Unit1.pas'   !!! keep the same case !!!

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
unit test;        // -> nazwa pliku 'test.pas'

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


## [LIBRARY](https://www.freepascal.org/docs-html/ref/refse117.html#x242-26600016.7)

Nagłówek `library` jest wymagany.

```
   library name [: address];
```

Możliwe jest określenie adresu kompilacji po znaku dwukropka `:`, jest to odpowiednik przełącznika z linii komend `-code address`.

Budowa biblioteki jest podobna do modułu `unit`, programu `program`.


```delphi
{
  Example LIBRARY
}
library lib1;

uses // List of unit dependencies goes here...
     
// Implementation of procedures, and functions goes here...


// exported subroutine(s), variable(s)

exports

idents1, idents2, idents3  ... ;

// optional library initialization code goes here...

begin

end.
```

Domyślnie funkcje i procedury zadeklarowane i zaimplementowane w bibliotece nie są dostępne dla programisty, który chce korzystać z tej biblioteki.

Aby udostępnić funkcje lub procedury z biblioteki, należy je wyeksportować w klauzuli `exports`.

Funkcje, procedury i inne identyfikatory są eksportowane z dokładnymi nazwami określonymi w klauzuli `exports`.

Aby móc korzystać z bibliotek w modułach `UNIT` lub programie `PROGRAM`, należy je najpierw skompilować i zasemblować, przełącznik **Mad Assembler-a** `-hm` musi być aktywny.

```DELPHI
mp.exe library.pas -ipath:<Mad_Pascal_path>\lib 
mads.exe library.a65 -hm -xi:<Mad_Pascal_path>\base
```

Plików `.pas` z kodem źródłowym bibliotek nie możemy umieszczać w klauzuli `uses`.

Aby skorzystać z identyfikatorów wyeksportowanych w `LIBRARY` korzystamy z modyfikatora `EXTERNAL`.


### [EXPORTS](https://wiki.freepascal.org/Exports)

Klauzula `exports` eksportuje identyfikatory z modułu `library`.

Moduł `library` może mieć maksymalnie jedną klauzulę `exports`. Lista identyfikatorów wyszczególniona w klauzuli rozdzielona jest znakiem przecinka, znak średnika oznacza koniec listy.



### [EXTERNAL](https://www.freepascal.org/docs-html/ref/refse98.html)

Modyfikator `external` pozwala wskazać procedury/funkcje, zmienne jako zewnętrzne, dostępne z innego modułu, biblioteki lub dołączonego kodu relokowalnego.

```delphi
   function dodaj(a,b: integer): integer; external 'TEST_LIB';
   procedure prc(a,b,c: integer); external;  
```


## [USES](https://wiki.freepascal.org/Uses)

Klauzula `uses` importuje identyfikatory z modułów `unit`.

Każda jednostka **Mad-Pascal** – `program`, `unit`, lub `library` – może mieć maksymalnie jedną klauzulę `uses` na sekcję, która musi pojawić się zaraz po nagłówkach sekcji. 

Nagłówki sekcji to `interface`, `implementation` w modułach `unit`. W `program` i `library` nie ma żadnych wyraźnych nagłówków sekcji, dlatego klauzula `uses` pojawia się bezpośrednio po nagłówku `program`, `library`.

```delphi
    uses crt, sysutils, atari;
```

Moduł `SYSTEM` nie może znajdować się na tej liście, ponieważ jest on domyślnie zawsze ładowany przez kompilator.

Kolejność, w jakiej pojawiają się moduły, jest istotna, ponieważ określa, w jakiej kolejności są one inicjowane. 
Moduły są inicjowane w tej samej kolejności, w jakiej pojawiają się w klauzuli `uses`. 

Identyfikatory są wyszukiwane w odwrotnej kolejności, tzn. gdy kompilator szuka identyfikatora, szuka go najpierw w ostatnim module w klauzuli `uses`, następnie w przedostatniej itd.
Jest to ważne w przypadku, gdy dwa lub więcej modułów w klauzuli `uses` deklaruje ten sam identyfikator. 

    uses graph, vbxe;

W obu modułach `GRAPH` i `VBXE` występuje procedura `SetColor` oraz `Line`, dla w/w przykładu odwołania do tych procedur będą realizowane przez moduł `VBXE`,

    uses vbxe, graph;

odwołania do tych procedur będą realizowane przez moduł `GRAPH`.

Możemy też bezpośrednio odwołać się do identyfikatora z konkretnego modułu, np.:

```delphi
    vbxe.Line
    graph.Line
    vbxe.SetColor
    graph.SetColor
```

Kompilator będzie szukał wersji źródłowych wszystkich modułów wyszczególnionych w klauzuli `uses` na podstawie ścieżki z której został uruchomiony kompilator `mp.exe\lib\`.

Przy pomocy słowa kluczowego `IN` możemy zastąpić mechanizm automatycznego wyszukiwania modułu.

```delphi
    uses unita in '..\unita.pas';
```

Moduł `unita` jest wyszukiwany w katalogu nadrzędnym bieżącego katalogu roboczego kompilatora. 
Można dodać dyrektywę `{$UNITPATH ..}`, aby upewnić się, że moduł zostanie znaleziony bez względu na to, gdzie znajduje się bieżący katalog roboczy kompilatora.

Gdy kompilator szuka plików modułów, dodaje rozszerzenie `.pas` do nazwy modułu.


### [$LIBRARYPATH](https://www.freepascal.org/docs-html/prog/progsu99.html)

```delphi
{$LIBRARYPATH path1; path2; ...}
```
Dyrektywa `$LIBRARYPATH` pozwala wskazać dodatkowe ścieżki poszukiwań modułów `UNIT` zadeklarowanych przez `uses`.


### [$UNITPATH](https://www.freepascal.org/docs-html/prog/progsu119.html)

```delphi
{$UNITPATH path1; path2; ...}
```
Dyrektywa `$UNITPATH` pozwala wskazać dodatkowe ścieżki poszukiwań modułów `UNIT` zadeklarowanych przez `uses`.
