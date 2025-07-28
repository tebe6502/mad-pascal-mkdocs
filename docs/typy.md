#

## [Podstawowe](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-250003.1.1)

|Type               |Range                    |Size in bytes|
|:------------------|:-----------------------:|:-----------:|
|BYTE               |0 .. 255                 |1            |
|SHORTINT           |-128 .. 127              |1            |
|WORD               |0 .. 65535               |2            |
|SMALLINT           |-32768 .. 32767          |2            |
|CARDINAL           |0 .. 4294967295          |4            |
|LONGWORD           |0 .. 4294967295          |4            |
|DWORD              |0 .. 4294967295          |4            |
|UINT32             |0 .. 4294967295          |4            |
|INTEGER            |-2147483648 .. 2147483647|4            |
|LONGINT            |-2147483648 .. 2147483647|4            |

<br/>
## [Logiczne](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-250003.1.1)

|Type               |Ord(True)  |Ord(False)   |Size in bytes|
|:------------------|:---------:|:-----------:|:-----------:|
|BOOLEAN            |1 +.. 255   |0            |1            |

Typ `BOOLEAN` może przechowywać wartości `1 .. 255` dla `TRUE`, wartość `0` dla `FALSE`.

```delphi
 bol:=Boolean(140);

 writeln(ord(bol),',',bol);      // 140,TRUE
```

<br/>
## [Wyliczeniowe](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-280003.1.1)

Typ wyliczeniowy w **Mad-Pascal** został zaimplementowany w podstawowej postaci, tzn.:

```delphi
Type
  Days = (monday,tuesday,wednesday,thursday,friday,
          saturday,sunday);

  Joy = (right_down = 5, right_up, right, left_down = 9, left_up, left, down = 13, up, none);
```
Typ wyliczeniowy przechowywany jest tylko w pamięci kompilatora **Mad-Pascal**, do pliku wynikowego nie zostaną zapisane jakiekolwiek informacje dotyczące pól typu wyliczeniowego. Dopuszczalne jest użycie komendy `ORD`, `SIZEOF` oraz rzutowania dla typu wyliczeniowego.

```delphi
var
   d: Days;

   d:=friday;
   writeln(ord(d));
   writeln(ord(sunday));
   writeln(sizeof(days));
   writeln(sizeof(monday));

   d:=days(20);

   case d of
    sunday: writeln('sunday');
   end;
```

Aktualnie kompilator **Mad-Pascal** nie sprawdzi poprawności typów wyliczeniowych dla operacji `IF ELSE`.

## [Rzeczywiste](https://www.freepascal.org/docs-html/ref/refsu5.html#x27-300003.1.2)

|Type               |Range                    |Size in bytes|
|:------------------|:-----------------------:|:-----------:|
|SHORTREAL (Q8.8)   |-128..127                |2            |
|REAL (Q24.8)       |-8388608..8388607        |4            |
|SINGLE (IEEE-754)  |1.5E-45 .. 3.4E38        |4            |
|FLOAT (IEEE-754)   |1.5E-45 .. 3.4E38        |4            |
|FLOAT16 (IEEE-754) |65504 .. -65504          |2            |

<br/>
Konwersja typu `FLOAT`, `SINGLE` do liczby całkowitej dostępna jest tylko w zakresie `INTEGER`. Typ `INTEGER` nie pozwoli zaprezentować maksymalnej wartości `3.4E38` typu `FLOAT` `SINGLE`.

## [Znakowe](https://www.freepascal.org/docs-html/ref/refsu6.html#x29-320003.2.1)

|Type               |Range                    |Size in bytes|
|:------------------|:-----------------------:|:-----------:|
|CHAR               |ATASCII (0 .. 255)       |1            |
|STRING             |1 .. 255                 |256          |
|PCHAR              |0 .. 65535               |2            |

<br/>
Ciąg znaków `STRING` reprezentowany jest jako tablica o możliwym maksymalnym rozmiarze `[0..255]` (w **FPC** ten typ to `SHORTSTRING`). Pierwszym bajtem takiej tablicy `[0]` jest długość ciągu z zakresu `0..255`. Od bajtu `[1..]` zaczyna się właściwy ciąg znaków.

W **FPC** `STRING` nie jest równoznaczny z `SHORTSTRING`, inaczej ustalamy adresy dla **FPC** `STRING`, inaczej dla `SHORTSTRING`.
```delphi
    var P: PChar;
        s: string;
    
    P:=pointer(s);
    P:=pointer(@s);
```

```delphi
    var P: PChar;
        s: string[255];   // shortstring

    P:=pointer(@s);       // s[0]
    P:=pointer(@s[1]);
```

Ciąg znaków `PCHAR` reprezentowany jest przez wskaźnik do typu `CHAR`. Znakiem końca ciągu `PCHAR` jest znak `#0`.

Dopuszczalne jest użycie dodatkowych znaków po końcowym apostrofie, takich jak `*`, `~`.

Znak `*` oznacza ciąg w inwersie, tylda `~` ciąg w kodach **ANTIC-a**.

Innym sposobem modyfikacji wyprowadzanych znaków jest użycie systemowej zmiennej `TextAttr`, każdy znak wyprowadzany na ekran jest poddawany operacji `ORA TextAttr` (domyślnie `TextAttr = 0`).

```delphi
a: string = 'Atari'*;         // ciąg znaków w inwersie
b: string = 'Spectrum'~;      // ciąg znaków w kodach ANTIC-a
c: char = 'X'~*;              // znak w inwersie, kodach ANTIC-a
```

## [Wskaźnikowe](https://www.freepascal.org/docs-html/ref/refse15.html)

|Type               |Range                    |Size in bytes|
|:------------------|:-----------------------:|:-----------:|
|POINTER            |0 .. 65535               |2            |

<br/>
Wskaźniki w **Mad-Pascal** mogą być typowane i bez określonego typu, np.:

```delphi
 a: ^word;         // wskaźnik typowany na słowo
 b: pointer;       // wskaźnik bez typu
```

Niezaincjowany wskaźnik najczęściej będzie miał adres `$0000`, należy zadbać aby przed jego wykorzystaniem zaincjować go adresem odpowiedniej zmiennej, np.:

    a := @tmp;         // wskaźnikowi A zostaje przypisany adres zmiennej TMP

Jeśli tego nie zrobimy to w przypadku uruchomienia takiego programu na **PC** spowodujemy błąd ochrony pamięci **Access Violation**.

Zwiększanie wskaźnika przez `INC` zwiększy go o rozmiar typu na jaki wskazuje. Zmniejszenie wskaźnika przez `DEC` zmniejszy go o rozmiar typu na jaki wskazuje. Jeśli typ jest nieokreślony, wówczas domyślną wartością zwiększania/zmniejszanie będzie `1`.

Dla wskaźników dopuszczalne są operacje relacji `=`, `<>`, `<`, `<=`, `>`, `>=`, oraz operacje arytmetyczne `+` oraz `-`.

Przy pomocy wskaźnika możemy dokonać rzutowania zmiennej na inny typ:

```delphi
var
   s: single;
   d: cardinal;

begin

 s := 3.14;

 d:=PCardinal(@s)^;	// d = $4048F5C3

end;
```

## [Tablice statyczne](https://www.freepascal.org/docs-html/ref/refsu14.html#x38-500003.3.1)

Tablice w **Mad-Pascal** są tylko statyczne, jednowymiarowe lub dwuwymiarowe z początkowym indeksem równym `0`, np.:

```delphi
var tb: array [0..100] of word;
var tb2: array [0..15, 0..31] of Boolean;
var tab: array [0..7] of array [0..31] of byte;
var tab256: array [byte] of word;

var [striped] tb: array [0..99] of cardinal;
```

W przypadku początkowego indeksu innego niż zero zostanie wygenerowany błąd **Error: Array lower bound is not zero**.

W pamięci tablica reprezentowana jest przez wskaźnik `POINTER`, wskaźnik jest adresem tablicy w pamięci `WORD`. Najszybszą metodą odwołania się do tablicy nie przekraczającej `256` bajtów z poziomu assemblera jest zastosowanie przedrostka `ADR.`, np.:

    asm
     lda adr.tb,y   ; bezpośrednie odwołanie do tablicy TB
     lda tb         ; odwołanie do wskaźnika tablicy TB
    end;

Kompilator generuje kod dla tablic zależnie od ich deklaracji:

* gdy nie przekracza 256 bajtów
```delphi
array [0..255] of byte;
array [0..127] of word;
array [0..63] of cardinal;
```

Gdy liczba bajtów zajmowanych przez tablicę nie przekracza 256 bajtów generowany jest najszybszy kod odwołujący się bezpośrednio do adresu tablicy (przedrostek `ADR.`) z pominięciem wskaźnika. Dla takiej tablicy nie ma możliwości zmiany adresu.

    ldy #118
    lda adr.tb,y

* gdy liczba elementów tablicy wynosi `1`
```delphi
array [0..0] of type;
```

Gdy liczba elementów tablicy wynosi `1` jest ona traktowana specjalnie. Generowany kod odwołuje się do tablicy poprzez wzkaźnik. Istnieje możliwość ustalenia nowego adresu takiej tablicy.

    lda TB
    add I
    tay
    lda TB+1
    adc #$00
    sta bp+1
    lda (bp),y

* gdy przekracza 256 bajtów
```delphi
array [0..255+1] of byte;
array [0..127+1] of word;
array [0..63+1] of cardinal;
```

Gdy liczba bajtów zajmowanych przez tablicę przekracza 256 bajtów generowany kod odwołuje się do tablicy poprzez wskaźnik. Istnieje możliwość ustalenia nowego adresu takiej tablicy.

    lda TB
    add I
    tay
    lda TB+1
    adc #$00
    sta bp+1
    lda (bp),y

### [Tablice STRIPED](../zmienne/#striped)

Dla poniższego przykłady kompilator musi pomnożyć indeks przez dwa, aby uzyskać dostęp do każdego elementu. 

```delphi
  array [0..7] of word;
```

Układ pamięci takiej tablicy będzie następujący:

    LHLHLHLHLHLHLHLH

Jeśli oznaczymy tablicę jako `STRIPED`

```delphi
  [striped] array [0..7] of word;
```

wygeneruje to następującą strukturę w pamięci:

    LLLLLLLLHHHHHHHH

Modyfikator `STRIPED` można stosować tylko dla tablic z typem prostym (`WORD`, `CARDINAL`, `SMALLINT`, `INTEGER`, `SHORTREAL`, `REAL`, `FLOAT16`, `POINTER`).

Modyfikator `STRIPED` nie zadziała dla tablic z indeksem większym niż `255`.

    
### Inicjalizacja tablic

Inicjalizacja tablicy `CONST` lub `VAR` przebiega w ten sam sposób. Po słowie określającym typ danych tablicy umieszczamy znak `=` i kolejne elementy tablicy między nawiasami okrągłymi `( val0, val1, ... )` :
```
const
   PBox : array [0..1] of word = (12,10);

var
   PBox : array [0..1] of word = (12,10);
```
W przypadku tablicy dwuwymiarowej:
```
   PBox : array [0..1, 0..1] of word = ( (12,10) , (1,6) );
```
Tablicę typu `CHAR` możemy zaincjować przez `STRING`:
```
   PBox : array [0..4] of char = 'Hello';
```
Możliwa jest inicjalizacja tablicy bez podawania jej rozmiaru, korzystamy wtedy z nawiasów kwadratowych `[ ]` :
```
   PBox : array of char = ['H', 'e', 'l', 'l', 'o'];
   PBox : array of word = [1,2,3,4,5];

   PBox : array of char = 'Hello';        // bez nawiasów [ ]
```

#### $bin2csv
Możliwe jest zaincjowanie tablicy typu `BYTE` plikiem binarnym, używamy wtedy dyrektywy kompilatora [`{$bin2csv filename}`](../skladnia/#bin2csv) :
```
   tb: array of byte = [ {$bin2csv filename} ];
   tb: array [0..11] of byte = ( 1,2,3, {$bin2csv filename} );
```

#### $eval
Innym sposobem zaincjowania tablicy zadanego typu jest użycie dyrektywy [`{$eval par1[,par2],"expression"}`](../skladnia/#eval) :
```
   tb: array of cardinal = [ {$eval 100,":1*32"} ];
   tb: array of pointer = [ {$eval 24,"SCR_ADDRESS+:1*40"} ];

   tb: array [0..11] of byte = ( {$eval 3,4,":1*:2"} );
```


## [Rekordy](https://www.freepascal.org/docs-html/ref/refsu15.html#x39-550003.3.2)

W pamięci rekord reprezentowany jest przez wskaźnik `POINTER`.

    type
        TPoint = record x,y: byte end;
    var px: TPoint;

Domyślnie rekordy w **Mad-Pascal** są typu `PACKED`. Rozmiar całkowity pól rekordu ograniczony jest do 256 bajtów.
Jeśli zależy nam na zachowaniu kompatybilności z **FPC** należy dodatkowo poprzedzić słowo `RECORD` słowem `PACKED`.
Bez tego rozmiar pamięci jaki zajmuje rekord będzie mógł się różnić, będzie mniej zajmował pamięci na **6502**, potencjalnie więcej o kilka bajtów na **PC**.

type
    TPoint = packed record x,y: byte end;

    var px: TPoint;

Dostęp do pól rekordu z poziomu asm:

    mwa px bp2
    ldy #px.x-DATAORIGIN
    lda (bp2),y


### Tablica z rekordami

**Mad-Pascal** obsługuje tylko tablice wskaźników rekordów.

```Delphi
    type
        TPoint = record x,y: byte end;    

    var 
        tab: array [0..3] of ^TPoint;
```

Taka tablica musi zostać zaincjowana odpowiednimi adresami rekordów, domyślnie na początku wszystkie pola takiej tablicy są wyzerowane.

Pierwszy sposób zaincjowania tablicy wskaźników rekordów:
```Delphi
    var
       a1,a2,a3,a4: TPoint;       

    begin
     tab[0] := @a1;
     tab[1] := @a2;
     tab[2] := @a3;
     tab[3] := @a4;   
    end.
```
Drugi sposób:
```Delphi
    begin
     GetMem(tab[0], sizeof(TPoint));
     GetMem(tab[1], sizeof(TPoint));
     GetMem(tab[2], sizeof(TPoint));
     GetMem(tab[3], sizeof(TPoint));
    end.
```
Dostęp do pól rekordu z takiej tablicy:
```Delphi
  writeln(tab[1].x);
  writeln(tab[1].y);
```

## [Obiektowe](https://www.freepascal.org/docs-html/ref/refse28.html#x60-780005.1)

Obiekty to rekordy z dodatkowymi metodami. W pamięci obiekt reprezentowany jest przez wskaźnik `POINTER`.

```delphi
type
    TRMT = Object

    player: pointer;
    modul: pointer;

    procedure Init(a: byte); assembler;
    procedure Play; assembler;
    procedure Stop; assembler;

    end;
```

W obiektach możliwe jest użycie procedur `CONSTRUCTOR` oraz `DESTRUCTOR`. Wywołanie takich procedur odbywa się tylko ręcznie.


## [Proceduralne](https://www.freepascal.org/docs-html/ref/refse17.html)

W pamięci zmienne typu proceduralnego reprezentowane są przez wskaźnik `POINTER`.

```delphi
type
    tprc = procedure (a: byte; c: word);
    tfun = function (a:smallint; x: single): byte;
 
var
    fn: function (a,b,c: byte): word;
```

Dla typu proceduralnego procedury/funkcje z argumentami wymagają użycia modyfikatora `STDCALL`, co wymusi użycie stosu programowego.

```delphi
var
   fn: function (a,b: word): word;

function test(a,b,c,d: word): word; stdcall;
begin

end;

begin

fn := @test;

fn(1,2);

end;
```

Dla procedur z argumentami zamiast modyfikatora `STDCALL` dopuszczalny jest modyfikator `REGISTER`, pod warunkiem że będą to maksymalnie trzy argumenty.

```delphi
var
   prc: procedure (a,b: word);

procedure test(a,b,c: cardinal); register;
begin

// a -> EDX
// b -> ECX
// c -> EAX
 
end;

begin

prc := @test;

prc(3,6);

end;
```

W przypadku kiedy nie przekazujemy do procedury/funkcji argumentów użycie modyfikatora nie jest konieczne.


## [Plikowe binarne](https://www.freepascal.org/docs-html/ref/refsu17.html#x41-590003.3.4)

Typ `FILE` reprezentuje uchwyt do pliku oraz definiuje rozmiar rekordu.

```delphi
type
  ftype = array [0..63] of cardinal;

var
  f: file;               // rekord domyślny =128 bajtów
  f: file of byte;       // rekord 1 bajt
  f: file of ftype;      // rekord 256 bajtów (ftype = 64 * 4)
```

W pamięci **XE/XL** uchwyt `FILE` reprezentowany jest przez wskaźnik `POINTER` do tablicy o strukturze (rozmiar 12 bajtów):

```
.struct s@file
pfname   .word      ; pointer to string with filename
record   .word      ; record size
chanel   .byte      ; channel *$10
eof      .byte      ; EOF status
buffer   .word      ; load/write buffer
nrecord  .word      ; number of records for load/write
numread  .word      ; pointer to variable, length of loaded data
.ends
```

Do procedur, funkcji typ `FILE` może być przekazywany tylko jako zmienna.

Typy, procedury i funkcje związane z plikami binarnymi:
`Assign` - `Close` - `Reset` - `Rewrite` - `BlockRead` - `BlockWrite` - `FileExists` - `IOResult`

## [Plikowe tekstowe](https://www.freepascal.org/docs-html/ref/refsu17.html#x41-590003.3.4)

Typ `TEXT` reprezentuje uchwyt do pliku tekstowego.

```delphi
var
  t: text;
  f: textfile;
```

W pamięci **XE/XL** uchwyt `TEXT`reprezentowany jest przez wskaźnik `POINTER` do tablicy o strukturze (rozmiar 12 bajtów):

```
.struct s@file
pfname   .word      ; pointer to string with filename
record   .word      ; record size
chanel   .byte      ; channel *$10
eof      .byte      ; EOF status
buffer   .word      ; load/write buffer
nrecord  .word      ; number of records for load/write
numread  .word      ; pointer to variable, length of loaded data
.ends
```

Do procedur, funkcji typ `TEXT` może być przekazywany tylko jako zmienna.

Typy, procedury i funkcje związane z plikami tekstowymi:
`Assign` - `Close` - `Reset` - `Rewrite` - `Append` - `Readln` - `Writeln` - `FileExists` - `IOResult`

## [Nieoznaczone](https://www.freepascal.org/docs-html/ref/refsu70.html)

```Delphi
 procedure Something (var Data);
 procedure Something (const Data);
```

Brak podania typu parametru oznacza że do procedury/funkcji zostanie przekazany tylko adres parametru bez oznaczenia typu.

Jest to odpowiednikiem następującej deklaracji C/C++:

```Delphi
 void Something(void* Data);
```

Wewnątrz procedury/funkcji z nieoznaczonym parametrem, jeśli nieoznaczony parametr jest używany w wyrażeniu lub wartość musi być do niego przypisana, zawsze należy użyć rzutowania typu.


    var x: word;
 
    procedure test(var a);
    begin
 
      writeln(PWord(@a)^);  // = 95

      PWord(@a)^ := 11;
 
    end;
 
    begin
    
      x:=95;
 
      test(x);              // = 11
 
    end.
 