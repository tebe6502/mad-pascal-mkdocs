#

## [Podstawowe](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-250003.1.1)

|Type    |Range                    |Size in bytes|
|:-------|:-----------------------:|:-----------:|
|BYTE    |0 .. 255                 |1            |
|SHORTINT|-128 .. 127              |1            |
|WORD    |0 .. 65535               |2            |
|SMALLINT|-32768 .. 32767          |2            |
|CARDINAL|0 .. 4294967295          |4            |
|LONGWORD|0 .. 4294967295          |4            |
|DWORD   |0 .. 4294967295          |4            |
|UINT32  |0 .. 4294967295          |4            |
|INTEGER |-2147483648 .. 2147483647|4            |
|LONGINT |-2147483648 .. 2147483647|4            |

<br/>
## [Logiczne](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-250003.1.1)

|Type    |Ord(True)                |Size in bytes|
|:-------|:-----------------------:|:-----------:|
|BYTE    |1                        |1            |

<br/>
## [Wyliczeniowe](https://www.freepascal.org/docs-html/ref/refsu4.html#x26-280003.1.1)

Typ wyliczeniowy w **MP** został zaimplementowany w podstawowej postaci, tzn.:

```delphi
Type
  Days = (monday,tuesday,wednesday,thursday,friday,
          saturday,sunday);

  Joy = (right_down = 5, right_up, right, left_down = 9, left_up, left, down = 13, up, none);
```
Typ wyliczeniowy przechowywany jest tylko w pamięci kompilatora **MP**, do pliku wynikowego nie zostaną zapisane jakiekolwiek informacje dotyczące pól typu wyliczeniowego. Dopuszczalne jest użycie komendy `ORD`, `SIZEOF` oraz rzutowania dla typu wyliczeniowego.

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

Aktualnie kompilator **MP** nie sprawdzi poprawności typów wyliczeniowych dla operacji `IF ELSE`.

## [Rzeczywiste](https://www.freepascal.org/docs-html/ref/refsu5.html#x27-300003.1.2)

|Type             |Range                   |Size in bytes|
|:----------------|:----------------------:|:-----------:|
|SHORTREAL (Q8.8) |-128..127               |2            |
|REAL (Q24.8)     |-8388607..8388608       |4            |
|SINGLE (IEEE-754)|1.5E-45 .. 3.4E38       |4            |
|FLOAT (IEEE-754) |1.5E-45 .. 3.4E38       |4            |

<br/>
Konwersja typu `FLOAT`, `SINGLE` do liczby całkowitej dostępna jest tylko w zakresie `INTEGER`. Typ `INTEGER` nie pozwoli zaprezentować maksymalnej wartości `3.4E38` typu `FLOAT` `SINGLE`.

## [Znakowe](https://www.freepascal.org/docs-html/ref/refsu6.html#x29-320003.2.1)

|Type    |Range                    |Size in bytes|
|:-------|:-----------------------:|:-----------:|
|CHAR    |ATASCII (0 .. 255)       |1            |
|STRING  |1 .. 255                 |256          |
|PCHAR   |0 .. 65535               |2            |

<br/>
Ciąg znaków `STRING` reprezentowany jest jako tablica o możliwym maksymalnym rozmiarze `[0..255]`. Pierwszym bajtem takiej tablicy `[0]` jest długość ciągu z zakresu `0..255`. Od bajtu `[1..]` zaczyna się właściwy ciąg znaków.

Ciąg znaków `PCHAR` reprezentowany jest przez wskaźnik do typu `CHAR`. Znakiem końca ciągu `PCHAR` jest znak `#0`.

Dopuszczalne jest użycie dodatkowych znaków po końcowym apostrofie, takich jak `*`, `~`.

Znak `*` oznacza ciąg w inwersie, tylda `~` ciąg w kodach **ANTIC-a**.

Innym sposobem modyfikacji wyprowadzanych znaków jest użycie systemowej zmiennej `TextAttr`, każdy znak wyprowadzany na ekran jest zwiększany o wartość `TextAttr` (domyślnie = 0).

```delphi
a: string = 'Atari'*;         // ciąg znaków w inwersie
b: string = 'Spectrum'~;      // ciąg znaków w kodach ANTIC-a
c: char = 'X'~*;              // znak w inwersie, kodach ANTIC-a
```

## [Wskaźnikowe](https://www.freepascal.org/docs-html/ref/refse15.html)

|Type    |Range                    |Size in bytes|
|:-------|:-----------------------:|:-----------:|
|POINTER |0 .. 65535               |2            |

<br/>
Wskaźniki w **MP** mogą być typowane i bez określonego typu, np.:

```delphi
 a: ^word;         // wskaźnik typowany na słowo
 b: pointer;       // wskaźnik bez typu
```

Niezaincjowany wskaźnik najczęściej będzie miał adres `$0000`, należy zadbać aby przed jego wykorzystaniem zaincjować go adresem odpowiedniej zmiennej, np.:

    a := @tmp;         // wskaźnikowi A zostaje przypisany adres zmiennej TMP

Jeśli tego nie zrobimy to w przypadku uruchomienia takiego programu na **PC** spowodujemy błąd ochrony pamięci **Access Violation**.

Zwiększanie wskaźnika przez `INC` zwiększy go o rozmiar typu na jaki wskazuje. Zmniejszenie wskaźnika przez `DEC` zmniejszy go o rozmiar typu na jaki wskazuje. Jeśli typ jest nieokreślony, wówczas domyślną wartością zwiększania/zmniejszanie będzie `1`.


## [Tablice statyczne](https://www.freepascal.org/docs-html/ref/refsu14.html#x38-500003.3.1)

Tablice w **MP** są tylko statyczne, jednowymiarowe lub dwuwymiarowe z początkowym indeksem równym `0`, np.:

```delphi
var tb: array [0..100] of word;
var tb2: array [0..15, 0..31] of Boolean;
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
array [0..255] of byte
array [0..127] of word
array [0..63] of cardinal
```

Gdy liczba bajtów zajmowanych przez tablicę nie przekracza 256 bajtów generowany jest najszybszy kod odwołujący się bezpośrednio do adresu tablicy (przedrostek `ADR.`) z pominięciem wskaźnika. Dla takiej tablicy nie ma możliwości zmiany adresu.

    ldy #118
    lda adr.tb,y

* gdy liczba elementów tablicy wynosi `1`
```delphi
array [0..0] of type
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
array [0..255+1] of byte
array [0..127+1] of word
array [0..63+1] of cardinal
```

Gdy liczba bajtów zajmowanych przez tablicę przekracza 256 bajtów generowany kod odwołuje się do tablicy poprzez wskaźnik. Istnieje możliwość ustalenia nowego adresu takiej tablicy.

    lda TB
    add I
    tay
    lda TB+1
    adc #$00
    sta bp+1
    lda (bp),y


## [Rekordy](https://www.freepascal.org/docs-html/ref/refsu15.html#x39-550003.3.2)

W pamięci rekord reprezentowany jest przez wskaźnik `POINTER`.

    type
        TPoint = record x,y: byte end;
    var px: TPoint;

Domyślnie rekordy w **MP** są typu `PACKED`.
Jeśli zależy nam na zachowania kompatybilności z **FPC** należy dodatkowo poprzedzić słowo `record` słowem `packed`.
Bez tego rozmiar pamięci jaki zajmuje rekord będzie mógł się różnić, będzie mniej zajmował pamięci na **Atari XE/XL**, potencjalnie więcej o kilka bajtów na **PC**.

type
    TPoint = packed record x,y: byte end;

    var px: TPoint;

Dostęp do pól rekordu z poziomu asm:

    mwa px bp2
    ldy #px.x-DATAORIGIN
    lda (bp2),y


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
