# Mad-Pascal

**Mad-Pascal** jest 32-bitowym kompilatorem **Turbo Pascala** dla **Atari XE/XL**. W założeniu jest kompatybilny z **Free Pascal Compilator** (FPC) (przełącznik `-MDelphi` powinien być aktywny), co oznacza możliwość otrzymania kodu uruchomieniowego dla **XE/XL**, **PC** i każdej innej platformy dla której istnieje **FPC**. **Mad-Pascal** nie jest portem **FPC**, został napisany na podstawie kompilatorów **SUB-Pascal** (2009), **XD-Pascal** (2010), których autorem jest [Vasiliy Tereshkov](https://github.com/vtereshkov).

Źródła dostępne na [GitHub](https://github.com/tebe6502/Mad-Pascal) wraz z [release](https://github.com/tebe6502/Mad-Pascal/releases) dla systemu Windows.

# Historia

## [1.7.2](https://github.com/tebe6502/Mad-Pascal/releases/tag/v1.7.2)
- optymallizacje, bugfixes
- szybszy kod dla tablic ABSOLUTE
- unit SYSTEM dodana tablica 'mem: array [0..0] of byte absolute $0000'
- optymalizacja dla CASE (tail optimize)
- optymalizacja INC/DEC dla tablic [striped]
- nowy unit E80, handler E: w trybie HiRes, 80 kolumn
- nowy unit RC4, szyfrowanie algorytmem RC4

## [1.7.0](https://github.com/tebe6502/Mad-Pascal/releases/tag/v1.7.0)
- modyfikator [STRIPED] dla tablic o maksymalnym zakresie 0..255
- nowy target, neo6502 (-t neo)
- GRAPH.INC: Circle (szybsza wersja)
- poprawki lib/aplib.pas
- poprawki lib/zx0.pas
- poprawiona optymalizacja CASE
- dodana dyrektywa {$UNITPATH filename} jako odpowiednik {$LIBRARYPATH filename}
- dodana możliwość podania ścieżki dla modułu deklarowanego przez USES, np:
```
  uses crt, vector in '..\3d\vector.pas';
```
- dodana obsługa modułów LIBRARY
- dodany modyfikator EXTERNAL dla zmiennych, procedur, funkcji
- dodana możliwość ustawienia adresu kompilacji z poziomu programu, np.:
```
  program name : address;
  library name : address;  
```

## [1.6.9](https://github.com/tebe6502/Mad-Pascal/releases/tag/v1.6.9)
- poprawiona alokacja pamięci dla tablic [0..0], wymuszany jest wstępnie 'ABSOLUTE $0000', oszczędzamy 1 bajt pamięci
- dodana możliwość deklaracji tablic bez podania ich rozmiaru, np.:
```
var tab: array of byte = [1,3,4,3,1];
var tb: array of char = 'abcdefghij';
```
- nowa dyrektywa kompilatora `{$bin2csv filename}` pozwala inicjować tablice np.:
```
var tb: array of byte = [ {$bin2csv filename} ];
```
- nowa dyrektywa kompilatora `{$optimization loopunroll}`, `{$optimization noloopunroll}`, dokonuje rozpętlenia pętli `FOR` przy stałych parametrach
- nowy moduł BLOWFISH pozwalający szyfrować, deszyfrować ciągi znakowe wg podanego klucza
- nowy przykład a8\AES-Rijndeal

## [1.6.7-1.6.8](https://github.com/tebe6502/Mad-Pascal/releases/tag/v1.6.7-1.6.8)
- rezygnacja z rozszerzania typu dla wyrażeń z SHR
- nowy typ zasobu SAPR, SAPRPLAY
- dla RMTPLAY jako drugi parametr można podać adres dla zmiennych na stronie zerowej
- SizeOfResource(variable, name)
- unit SAPLZSS
- unit SHANTI
- unit SHA1
- unit xSFX
- unit SYSTEM: NtoBE, RorByte, RorWord, RorDWord, RolByte, RolWord, RolDWord, SarShortint, SarSmallint, SarLongint
- możliwość zaincjowania tablicy typu CHAR przez STRING (jeśli string jest krótszy zostaną wstawione spacje), np.:
```
    tab: array [0..15] of char = '0123456789ABCDEF';
```
- naprawione przekazywanie wartości funkcji przez tablice
- dodana obsluga przerwania VBLKI (natychmiastowe) przez SetIntVec, GetIntVec (https://mads.atari8.info/doc/pl/przerwania/)
- przepisany kod kompilatora na moddzielne moduły
- przepisany kod obsługi tablic ze wskaźnikami do rekordów `tab: array [0..x] of record^`
- dodana optymalizacja 'Common head/tail Sequence coalescing'

## [1.6.6](https://github.com/tebe6502/Mad-Pascal/releases/tag/1.6.6)
- poprawiona implementacja EXIT
- dodano możliwość generowania kodu dla RAW (-target)
- dodano obsługę modyfikatora INLINE dla procedur i funkcji
- dodano możliwość zadeklarowania zmiennej na stronie zerowej poprzez użycie modyfikatora REGISTER
- dodana obsługa typu TEXTFILE (TEXT)
- unit INIFILES
- unit ZX2
- unit SYSUTILS: CompareMem, TryStrToInt
- unit SYSTEM: CompareByte, Pos, Delete
- poprawiono przekazywanie parametrów do obiektów (OBJECT) bez udziału stosu programu :STACKORIGIN (w większości przypadków)
- dodano możliwość oznaczenia zmiennej jako ulotnej [volatile]
```delphi
    [volatile] vcount: byte absolute $d40b;
```
- dla OBJECT dodano metody CONSTRUCTOR, DESTRUCTOR
- dodano obsługę makr {$define label (parametry) := wyrażenie}
- dodana konstrukcja 'FOR element IN array' dla tablic nie przekraczających 256 bajtów
- więcej wolnej pamięci na stronie zerowej, wskaźniki FXPTR, PSPTR są teraz alokowane w zależności od tego czy są używane
- dodana obsługa typu FLOAT16
- dodana obsługa typu proceduralnego

## [1.6.5](https://github.com/tebe6502/Mad-Pascal/releases/tag/1.6.5)
- przepisana obsługa CASE OF
- przepisany kod optymalizacji dla tablic nie przekraczających 256 bajtów
- optymalizacja dla warunków '>', '<='
- dodana alokacja pamięci dla tablic typu STRING (do tej pory odkładany był tylko wskaźnik, działało to jak 'tablica ^string')
- dodana nowa platforma, Commodore C4 Plus
- unit GRAPHICS: procedura Font(charset: pointer);
- unit STRINGUTILS
- unit MISC, RMT, CMC, MPT: DetectAntic
- unit SYSTEM zmieniony w celu uwzględnienia platform ATARI, C64, C4Plus
- unit ZX0
- unit HCM2
- dodano obsługa $resource (RCDATA, RCASM) dla platform C64, C4Plus, zasoby są sortowane według rosnących adresów
- rozszerzony zasób RCDATA o możliwość określenia ofsetu dla ładowanego pliku
- dodano obsługę składni zgodnej z Pascalem dla bloku ASM, nie są wymagane nawiasy { }
- dodana nowa dyrektywa {$codealign proc = wartość}, {$codealign loop = wartość} pozwalająca na wyrównanie wygenerowanego kodu

## [1.6.4](https://github.com/tebe6502/Mad-Pascal/releases/tag/1.6.4)
- procedury mnożenie u8x8, u16x16 zastąpione kodem z CC65
- dodanie kodu resetujacego POKEY-a na początku uruchomienia programu
- optymalizacje wyrażeń warunkowych IF () and () and ... ; IF () or () or ...
- optymalizacja CMP_SHORTINT, CMP_SMALLINT
- GRAPH, FASTGRAPH: SetActiveBuffer, SetDisplayBuffer, NewDisplayBuffer, SwitchDisplayBuffer, przykład demoeffects\lines2.pas, line3.pas, line4.pas
- GRAPH: procedure Ellipse(X, Y, StAngle, EndAngle, xRadius,yRadius: Word);
- MATH: Hypot
- SYSTEM: Length(string_array[element])
- SYSTEM: GetMem, FreeMem
- SYSTEM: IntToStr(CARDINAL), Str(CARDINAL, STRING)
- SYSUTILS: ByteToStr(BYTE): STRING
- SYSUTILS: poprawione FindFirst, StrToFloat
- SYSTEM: poprawione Val(string, real, code) ; Val(string, single, code)
- nowy unit CIO: Opn, Cls, Get, Put, BGet, BPut, XIO
- nowy unit SIODISK, wydzielony z SYSTEM
- nowy unit S2, zintegrowany z instalatorem handlera VBXE S2: (S_VBXE.SYS)
- nowy unit DEFLATE, realizujący dekompresję DEFLATE (procedure unDEF), przykład compression\undef.pas
- nowy unit LZ4, realizujący dekompresję LZ4 z pamięci lub dysku (procedure unLZ4), przykład compression\unlz4.pas, unlz4_stream.pas
- nowy unit APLIB, realizujący dekompresję apLib z pamięci RAM lub dysku (procedure unApl), przykład compression\unapl.pas, unapl_stream.pas
- nowy unit CRC: function crc32(crc: cardinal; buf: Pbyte; len: word)
- nowy modyfikator procedur/funkcji PASCAL, powodujący przydzielenie/zwolnienie nowego bloku pamięci dla zmiennych przy każdym wywołaniu procedury/funkcji, przykład 'math\evaluate.pas'
- dodana możliwość wyłączenia pamięci ROM z zachowaniem działania systemu, {$DEFINE ROMOFF}
- krótsze nazwy przełacznikow -zpage (-z), -stack (-s), -data (-d), -code (-c), -define (-d), -ipath (-i)
- dodana możliwość generowania kodu dla C64 (-t c64)

## [1.6.3](https://github.com/tebe6502/Mad-Pascal/releases/tag/v1.6.3)
- poprawki, optymalizacje
- SYSUTILS: Trim
- SYSTEM: PByte, PByteArray, PWord
- GRAPH, FASTGRAPH: TFrameBuffer, DisplayBuffer, FrameBuffer, SwitchBuffer, Scanline
- BLIBS: unit GR10PP
- LIB: unit GR4PP

## 1.6.0 - 1.6.2
- nowy unit EFAST dla przyspieszenia wyprowadzania znaków na urządzenie E:
- SYSTEM: function Copy(var S: String; Index: Byte; Count: Byte): String;
- SYSTEM: Palette, HPalette
- dodana obsługa tablic jednowymiarowych typu ^RECORD (wskaznik do rekordu)
- optymalizacja bloków warunkowych, generowany jest możliwie najkrótszy, najszybszy kod wynikowy
- dodany typ PChar, [link]https://www.freepascal.org/docs-html/rtl/system/pchar.html
- dodana możliwość zwracania wartości funkcji przez typ wyliczeniowy
- dodany nowy przełącznik -define:symbol
- dodany nowy przełącznik -ipath:includepath

## 1.5.9 - 1.6.0
- SYSTEM: TDateTime
- GRAPH, FASTGRAPH: Arc, PieSlice, TLastArcCoords, LastArcCoords
- SYSUTILS: Now, Date, DateToStr, DecodeDate, DecodeDateTime, DecodeTime, EncodeDate, EncodeDateTime, EncodeTime, IsLeapYear, BoolToStr, StrToBool
- STRUTILS: AddChar, AddCharR, PadLeft, PadRight
- dodana obsługa {$INCLUDE %DATE%}, {$INCLUDE %TIME%}, [link]https://www.freepascal.org/docs-html/prog/progsu41.html
- przełączniki -CODE:, -DATA:, -STACK:, -ZPAGE: domyślnie wymagają podania wartości HEX bez znaku początkowego '$'

## 1.5.8
- GRAPHICS: Font, FontInitialize, FillRect, TextOut, TextWidth, MoveTo, LineTo
- SYSTEM: FileMode (RESET), [fmOpenRead, fmOpenWrite, fmOpenAppend, fmOpenReadWrite]
- dodana obsługa dyrektyw {$info user_defined}, {$warning user_defined}, {$error user_defined}, {$include filename}, {$resource filename}
- akceptuje zapis PROGRAM (par1, par2 ...)
- poprawione FillFlood, dodane FillFloodH (wypełnianie poziomymi liniami)
- kompilator generuje krótszy kod gdy procedura/funkcja nie ma parametrów
- dostęp do definicji .DEFINE z poziomu ASM, `MAIN.@DEFINES.nazwa`
- procedury/funkcje INTERRUPT niezależnie czy zostaną użyte kompilator zawsze wygeneruje dla nich kod
- poprawiona optymalizacja bloków REPEAT UNTIL
- dodana informacja o czasie kompilacji
- dodana obsługa inicjalizacji tablic dwuwymiarowych stałych i zmiennych

## 1.5.6 - 1.5.7
- SYSUTILS: Click
- SYSTEM: Sqrt(Integer): Single
- MATH: Sign
- dodana możliwość użycia nazwy funkcji dla zwracanej wartości, dotychczas tylko przez RESULT
- dodana obsługa tablic typu STRING
- dodana obsługa dyrektywy {$LIBRARYPATH path1;path2;...}
- nowa biblioteka BLIBS
- nowa dokumentacja dla LIB, BLIBS wygenerowana przez [PASDOC](https://gitlab.com/bocianu/pasdoc)

## 1.5.3 - 1.5.5
- CRT: TextMode
- SYSUTILS: ExtractFilePath
- SYSTEM: EoLn
- dodana obsługa linii poleceń z poziomu DOS II/D dla ParamCount, ParamStr
- OBJECT, można zdefiniować je bez zmiennych, tylko same metody
- OBJECT, można wywołać metodę z poziomu metody
- dodana automatyczna konwersja wyrażeń typu INTEGER na REAL

## 1.5.2
- nowy typ FLOAT jako odpowiednik typu SINGLE
- SYSTEM unti: FileSize (SDX)
- SYSTEM unit: EXP:Single, LN:Single, LN:Real, EXP:Real (wysoka precyzja obliczeń)
- SYSUTILS unit: AnsiUpperCase, ExtractFileExt
- TYPES unit: Bounds, CenterPoint, EqualRect, InflateRect, IntersectRect, IsRectEmpty, NormalizeRect, OffsetRect, Point, PointsEqual, PtInRect, PtInEllipse, Rect, RectWidth, RectHeight, Size, UnionRect
- IMAGE unit: LoadMIC, LoadPIC, LoadBMP, LoadPCX, LoadGIF
- VIMAGE unit: LoadVBMP, LoadVPCX, LoadVGIF
- GRAPH, FASTGRAPH unit: SetClipRect, ClipLine, FillRect(TRect), Rectangle(TRect)

## 1.5.1
- dodane nowe przełączniki -CODE:$address, -DATA:$address -STACK:$address, -ZPAGE:$address
- SYSTEM unit: RandomF (Result as Single), VAL (Integer, Single)
- GRAPH, FASTGRAPH unit: Bar, Bar3D, GetX, GetY, MoveRel, FloodFill
- MATH unit: RandomRange, RandomRangeF, RandG (gaussian distributed random number)
- CRT unit: SOUND (działa identycznie jak SOUND w Atari BASIC)
- VBXE unit: TVBXEMemoryStream
- dodany komunikat ostrzeżenia 'Comparison might be always true/false due to range of constant and expression'
- zasoby RCASM, CMCPLAY, MPTPLAY można teraz ładować pod ROM
- dodana możliwość oznaczenia kodowania ciągu znakowego jako internal ANTIC-a poprzez znak tyldy `~`, np.:

```delphi
txt0: string = 'Atari'~;      // ciąg w kodach ANTIC-a
txt1: string = 'Spectrum'*~;  // ciąg w inwersie w kodach ANTIC-a
```

## 1.5.0
- poprawiona i uzupełniona inicjalizacja tablic typu POINTER
- poprawiona i uzupełniona inicjalizacja zmiennych typu wyliczeniowego
- wprowadzony typ LONGWORD, DWORD, UINT32 jako odpowiednik CARDINAL
- wprowadzony typ LONGINT jako odpowiednik INTEGER
- zreorganizowane typy rzeczywiste, typ ShortReal (fixed point Q8.8), Real (fixed point Q24.8), Single (32bit IEEE-754)
- unit SYSTEM (const SINGLE): NaN, Infinity, NegInfinity
- unit SYSTEM (type SINGLE): SIN, COS, ABS, SQRT, ISQRT, ROUND, TRUNC
- unit MATH (type SINGLE): LOG2, LOG10, LOGN, IsNaN
- dla WRITE/WRITELN akceptowane i ingorowane jest formatowanie wyniku, np.: writeln(f:8:8)

## 1.4.8 / 1.4.9
- zmniejszenie wskaźnika stosu programowego w przypadku wywołania funkcji bez odebrania jej wartości
- lepsza ocena możliwości ustalenia adresu stałej/zmiennej, dodany komunikat "Can't take the address of constant expressions"

## 1.4.7
- optymalizacja dla imulBYTE, imulWORD, imulCARD, mulSHORTINT, mulSMALLINT, mulINTEGER
- dodana obsługa wskaźnika rekordu, np.:

```delphi
type
  TPoint = record x,y: smallint end;
var
  a,b: ^TPoint;
begin
  a^.y := b^.x;
  a^ := b^;
end.
```

- dodana obsługa sekcji INITIALIZATION w modułach UNIT
- dodany nowy typ EXTMEM dla pliku resource $R, pozwalajacy ładować dane do pamięci dodatkowej
- dodany nowy typ OBJECT, czyli RECORD z dodatkowymi metodami

```delphi
type
  TMemoryStream = Object

  Position: cardinal;
  Size: cardinal;

  procedure Create;
  procedure ReadBuffer(var Buffer; Count: word);
  procedure WriteBuffer(var Buffer; Count: word);

  end;
```

- poprawione biblioteki CMC, MPT, RMT

## 1.4.6
- dodana obsługa dyrektyw warunkowych $IFDEF, $IFNDEF, $ELSE, $ENDIF, $DEFINE, $UNDEF przez DMSC

```delphi
{$define test}

const
  {$ifdef test}
  a=1;
  {$else}
  a=2;
  {$endif}
```

- dodana możliwość przekazania wartości funkcji przez EXIT(expression)
- dodana podstawowa implementacja dla typu wyliczeniowego (w pliku wynikowym nie są zapisywane nazwy etykiet)
- dodana obsługa tablic dwuwymiarowych

##  1.4.4/1.4.5
- poprawione porównanie typów tego samego rozmiaru ale o przeciwnych znakach
- dodana możliwość użycia klauzuli USES w blokach modułów UNIT
- dodany wymóg deklaracji funkcji/procedur w sekcji INTERFACE modułów UNIT
- poprawione zauważone błędy optymalizatora

## 1.4.3
- dodana możliwość zwrócenia wartości funkcji przez RECORD, np:

```delphi
  var p: TPoint;
  p:=Point(x,y);
```

- {$f page} FAST_MUL, procedury szybkiego mnożenia dla BYTE, SHORTINT, WORD, SMALLINT, SINGLE
- SYSTEM: ReadConfig, ReadSector, WriteSector
- SYSTEM: Point, Rect, Bounds, PointsEqual [link](https://github.com/graemeg/freepascal/blob/577d99e594f4958bad4f343fb419b8dddda2ebe6/rtl/objpas/classes/classes.inc)
- DOS: CurrentMinuteOfDay, CurrentSec100OfDay, CurrentSecondOfDay, MinuteOfDay, MinutesToTime, SecondOfDay, SecondsToTime [link](https://github.com/graemeg/freepascal/blob/master/packages/fv/src/time.pas#L309)
- MATH: Power, ArcTan2, InRange, EnsureRange, CycleToRad, DegNormalize, DegToGrad, DegToRad, DivMod, GradToDeg, GradToRad, RadToCycle, RadToDeg, RadToGrad [link](https://github.com/graemeg/freepascal/blob/master/rtl/objpas/math.pp)
- nowy typ LABEL
- dodana obsługa GOTO label

##  1.4.1 / 1.4.2
- optymalizacja kodu wynikowego 6502 generowanego dla pętli FOR
- poprawiony odczyt plików include i resource
- MISC: DetectCPUSpeed
- dodatkowy typ danych rzeczywistych SINGLE <-128..127>

## 1.4.0
- w parametrach formalnych procedur i funkcji dodana możliwość podania typu UNTYPED (tylko przez VAR), np.

```delphi
procedure move(var x,y; count: word);
begin
end;

move(tab[100], tab[200], 50);
```

- SYSTEM: poprawki dla FILLCHAR, FILLBYTE, MOVE

```
 fillchar(pointer($bc40+40*8), 100, ord('a'));
 fillchar(scr[40*8], 100, ord('a'));            // teraz takie wywołanie też jest możliwe
```

- funkcja INTTOREAL usunięta, zastąpiona przez wbudowaną konwersję typu REAL, np.

```delphi
var
  f: real;
  i: word;

f:=real(i);
```

- FASTGRAPH: SetClipRect, ClipLine (Cohen-Sutherland algorithm)
- poprawki dla {$i filename}, poprawne liczenie linii kompilowanego programu

## 1.3.8
- SYSTEM: RANDOM(RANGE: BYTE): BYTE; RANDOM(RANGE: INTEGER): SMALLINT;
- CRT: WHEREX, WHEREY
- SYSUTILS: BEEP
- dodatkowy znak `*` po apostrofie `'` oznacza inwers znaków, np.:

 `writeln('inwers'*'bez inwersu');`

## 1.3.7
- dodana możliwość ładowania bloku resource {$R} pod adres tablicy

## 1.3.6
- dodane rozkazy GetIntVec, SetIntVec w zastępstwie rozkazu Intr
- umożliwione zwracanie wartości RESULT dla funkcji poprzez tablice
- poprawki dla MOVE (downwards, upwards)
- optymalizacja ASM dla procedur/funkcji POKE, DPOKE, PEEK, DPEEK, FILLCHAR, MOVE, INTTOREAL
- SYSTEM: ParamCount, ParamStr (SDX, BWDOS)

## 1.3.5

- dodana możliwość odczytu adresu stałej, np.:

  const tb: array [0..0] of byte = ( lo(word(@tb)) );
- dodany odczyt wektorów DLI, VBL przez rozkaz INTR

```delphi
intr(rVBL, label);
intr(rDLI, label);
```

## 1.3.4
- dodana dodatkowa informacja o zajmowanej pamięci przez kolejne kompilowane moduły
- dodany nowy moduł VBXE (tryb OVERLAY, BLITTER)
- dodana obsługa błędów dla operacji I/O {I+} {I-} IOCHECK ON/OFF
- dwie dodatkowe poprawki Greblusa dla $I, $R z myślą o działaniu w środowisku Linuxa

## 1.3.3
- Greblus dodał poprawki umożliwiające działanie kompilatora z Linux-em
- dodana możliwość zaincjowania wskaźnika

```delphi
tmp: byte;
p: pointer = @tmp;
a: ^byte = @tmp;
```

- dodana możliwość przypisania rekordu

```delphi
type a = record x,y: byte end;
var k,w: a;
k:=w;
```

- GRAPH: SetVideoBank, SetColorMapDimensions, SetColorMapEntry
- VBXE: GetXDL, SetXDL, RunBlit, ClrVideoBank

## 1.3.2
- DOS: GetTime, SetTime
- MATH: Ceil
- SYSUTILS: GetTickCount
- FindFirst ... obsługa Attributes
- wbudowanie funkcji w kompilator: INT, FRAC, TRUNC, ROUND, ODD

## 1.3.1
- dodana obsługa rekordów RECORD
- {$i filename}
- {$r filename}, Resource Type: RCDATA, DOSFILE, RELOC, RMT, MPT, CMC
- dodany przełącznik -o Optimize code
- możliwość wymuszenia typu dla CONST

```delphi
  const
      a: word = 5;     // WORD
      b = 5;           // BYTE
```

- dodana obsługa modyfikatorów INTERRUPT, FORWARD, REGISTER dla procedur i funkcji
- dodana obsługa mapy kolorów (40x30x8x8) VBXE (moduł GRAPH, CRT), SetColorMap, SetColorCell, SetVideoBank
- SYSUTILS: TSearchRec, FindFirst, FindNext, FindClose
- GRAPH, FASTGRAPH: FillRect, FillEllipse
- SYSTEM: ODD, INT, LOW, HIGH, SIZEOF, INTR (INTERRUPT_NUMBER), BinStr, OctStr, HexStr, Pause, Sound, NoSound, Concat
- RMT: RMTINIT, RMTPLAY, RMTSTOP
- MPT: MPTINIT, MPTPLAY, MPTSTOP
- CMC: CMCINIT, CMCPLAY, CMCSTOP
