#

## [Procedury](https://www.freepascal.org/docs-html/ref/refch14.html#x173-19500014)

**MP** pozwala na przekazanie do procedury maksymalnie **8** parametrów. Dostępne są trzy sposoby przekazywania parametrów: przez wartość, stałą `CONST` i referencję `VAR`.

Parametry procedur odczytywane są i wartościowane od prawej do lewej strony [tests\tests-medium\function_valuation_of_arguments.pas](https://github.com/tebe6502/Mad-Pascal/blob/master/samples/tests/tests-medium/function_valuation_of_arguments.pas)

Możliwe jest zagnieżdżanie procedur.

Możliwa jest rekurencja procedur, pod warunkiem że parametry procedury będą przekazywane przez wartość, będą typu prostego - porządkowego. Typ rekordowy, wskaźnikowy nie będzie właściwie alokowany w pamięci.

Jeśli jest to możliwe kompilator przekazuje parametry do procedury poprzez zmienne z pominięciem stosu programowego.

## [Funkcje](https://www.freepascal.org/docs-html/ref/refch14.html#x173-19500014)

**MP** pozwala na przekazanie do funkcji maksymalnie **8** parametrów. Dostępne są trzy sposoby przekazywania parametrów: przez wartość, stałą `CONST` i referencję `VAR`. 

Wynik funkcji zwracamy przypisując go do nazwy funkcji lub korzystając z automatycznie deklarowanej zmiennej `RESULT`, np.:

```delphi
function add(a,b: word): cardinal;
begin
  Result := a+b;
end;

function mul(a,b: word): cardinal;
begin
  mul := a*b;
end;
```


- Używanie modyfikatora `INTERRUPT` nie jest zalecane.

Parametry funkcji odczytywane są i wartościowane od prawej do lewej strony [tests\tests-medium\function_valuation_of_arguments.pas](https://github.com/tebe6502/Mad-Pascal/blob/master/samples/tests/tests-medium/function_valuation_of_arguments.pas)

Możliwe jest zagnieżdżanie funkcji.

Możliwa jest rekurencja funkcji, pod warunkiem, że parametry funkcji będą przekazywane przez wartość, będą typu prostego - porządkowego. Typ rekordowy, wskaźnikowy nie będzie właściwie alokowany w pamięci.

Jeśli jest to możliwe kompilator przekazuje parametry do funkcji poprzez zmienne z pominięciem stosu programowego.

## Przekazywanie parametrów

Można przekazywać do 8 parametrów do funkcji lub procedury. Oddziela się je średnikami. Każdy z parametrów ma własny typ. 
Zmienne podawane zazwyczaj w parametrach są kopiowane w inny obszar pamięci i właśnie do tego obszaru ma dostęp programista. 
Kompilator `Mad Pascala` wykonuje kopię parametrów, nie należy przekazywać zbyt dużej ilości danych, by nie spowolnić działania programu. Zaleca się przekazywać do czterech bajtów.


### [Przekazywanie przez wartość](https://4programmers.net/Delphi/Procedury_i_funkcje#przekazywanie-parametrow-przez-wartosc)

Przekazywanie parametrów przez wartość jest sposobem najprostszym z możliwych. Deklaracja procedury nie musi zawierać żadnych dodatkowych słów kluczowych - wystarczy poniższa konstrukcja nagłówka procedury bądź funkcji:

```delphi
procedure Foo(S : String);
```

Przekazanie parametru przez wartość wiąże się utworzeniem jego kopii lokalnej do wykorzystania jedynie przez procedurę lub funkcję. Oryginalna wartość zmiennej przekazanej do procedury nie zostaje w żaden sposób naruszona.


### [Przekazywanie przez stałą](https://4programmers.net/Delphi/Procedury_i_funkcje#przekazywanie-parametrow-przez-stala)

 Umieszczenie w deklaracji parametrów słowa kluczowego `CONST` spowoduje przekazywanie parametrów jako stałych.

```delphi
procedure Show(const Message : String);
begin
  Writeln(Message);
end;
```

Procedura nie może w żaden sposób wpływać na zawartość parametru. Próba nadania przez procedurę jakiejś wartości spowoduje komunikat o błędzie:
`Can't assign values to const variable`, nie można więc zapisać tego w ten sposób:

```delphi
procedure Show(const Message : String);
begin
  Message := 'Nowa wartość';
  Writeln(Message);
end;
```

Obecnie kompilator `MP` odkłada takie parametry w pamięci podobnie jak w przypadku przekazywania przez wartość, docelowo powinien tylko przez wskaźnik.


### [Przekazywanie przez referencję](https://4programmers.net/Delphi/Procedury_i_funkcje#przekazywanie-parametrow-przez-referencje)

Przekazywanie parametrów przez referencję polega na umieszczeniu przed parametrami słowa kluczowego `VAR`. Dzięki temu kod znajdujący się wewnątrz procedury może zmienić wartość parametru.

Sposób przekazywania danych przez referencję jest optymalny, gdyż nie jest tworzona kopia zmiennej. W związku z tym jest możliwe przekazywanie danych z procedury na zewnątrz.

```delphi
uses crt;

procedure SetValue(var a,b,c,d: byte);
begin
{ próba nadania nowej wartości dla parametru }
  a := a  + 2;
  b := a  + 3;
  c := b  + 4;
  d := c  + 5;
end;

var a,b,c,d: byte;

begin
 a:=3;

 SetValue(a,b,c,d);

 writeln(a);     // 5
 writeln(b);     // 8
 writeln(c);     // 12
 writeln(d);     // 17

 repeat until keypressed;

end.
```


### [Parametry bez typu](https://www.freepascal.org/docs-html/ref/refsu70.html#x186-21000014.4.7)

`MP` pozwala deklarować parametry procedur i funkcji, które nie posiadają typu. W takim wypadku, nazwy parametrów muszą być poprzedzone słowem kluczowym `VAR`.

Dostęp do wartości takich parametrów musi odbywać się poprzez rzutowanie.

```delphi
var w: cardinal;

procedure doit(var d);

begin
  Writeln('As integer: ',PInteger(@D)^);
  Writeln('As Byte   : ',PByte(@D)^);
end;


begin

w:=31411;

doit(w);

end.

>> 31411
>> 179
```

Przekazywanie parametru przez wskaźnik, zależnie od typu takiego wskaźnika:

```delphi
var pn: pointer;
    tb: array [0..0] of byte;

procedure doit(var a); overload;
begin

 writeln('VAR',',',cardinal(@a));

end;

procedure doit(var a: pointer); overload;
begin

 writeln('VAR POINTER',',',cardinal(a));

end;


begin

 pn:=@tb;

 doit(tb);       // VAR

 doit(pn);       // VAR POINTER

end.
```

Odczyt pliku pod adres wskazany przez wskaźnik:

```delphi
procedure BlockRead(var f: file; var Buf; count: word; var Result: word);
```

```delphi
var pn: pointer;
    f: file;

begin

 InitGraph(15+16);

 pn:=pointer(dpeek(88));

 assign(f, 'FILENAME'); reset(f, 1);

 blockread(f, pn^, 192*40);
 
 close(f);

end.
```

Odczyt pliku pod adres wskazany przez tablicę:

```delphi
var bf: array [0..0] of byte;
    f: file;

begin

 InitGraph(15+16);

 bf:=pointer(dpeek(88));

 assign(f, 'FILENAME'); reset(f, 1);

 blockread(f, bf, 192*40);
 
 close(f);

end.
```

### [Tablice otwarte](https://www.freepascal.org/docs-html/ref/refsu69.html#x185-20900014.4.6)

Obecnie kompilator `MP` nie wspiera `tablic otwartych` ani nie pozwala na przekazywanie `tablic otwartych` jako parametru.

```delphi
function Foo(const A : array of const) : String;
```

```delphi
ShowMessage(Foo(['test', 10, ' ', 'mama', 1000]));
```

```delphi
function Foo(const A : array of const) : String;
var
  i : Integer;
begin
  for I := 0 to High(A) do
  begin
    case A[i].VType of
      vtInteger: Result := Result + IntToStr(A[i].VInteger);
      vtChar: Result := Result + A[i].VChar;
      vtString: Result := Result + A[i].VString^;
      vtAnsiString: Result := Result + String(A[i].VAnsiString);
      vtVariant: Result := Result + String(A[i].VVariant^);
      { itd. }
    end;
  end;
end;
```

## [Procedury zagnieżdżone](https://4programmers.net/Delphi/Procedury_i_funkcje#procedury-zagniezdzone)
Nic nie stoi na przeszkodzie, aby daną procedurę lub funkcję umieścić w innej procedurze lub funkcji.

```delphi
procedure A;

  procedure B;
  begin

  end;
  
begin

end;
```

Z powyższego zapisu wynika, że procedura lub funkcja zagnieżdżona (w tym wypadku procedura `B`) musi zostać umieszczona przed blokiem `BEGIN`.

W takim przypadku nadal obowiązują zasady o zmiennych lokalnych. Oznacza to, że zmienna umieszczona w procedurze `B` nie będzie dostępna dla procedury `A`.

Przy zastosowaniu procedur zagnieżdżonych obowiązują również inne zasady. Procedura zewnętrzna do `A` nie ma dostępu do procedury `B`, procedura `B` ma dostęp do parametrów wywołania procedury `A`. 

```delphi
program nested;

function E(x: byte): byte;

  function F(y: byte): byte;
    begin
    F := x + y
    end;

  begin
  Result := F(3) + F(2)
  end;

begin
  writeln(E(1));

  while true do;
end.
```

W powyższym przykładzie, zmienne `X`, `Y` są dostępne w funkcji `F`, natomiast dla funkcji `E` dostępna jest tylko zmienna `X`.

Procedury i funkcje zagnieżdżone mogą okazać się przydatne, gdy do realizacji jednej funkcji przydatna lub wymagana jest inna procedura lub funkcja, która, z punktu widzenia innych procedur lub funkcji danego modułu, jest zbędna. Jednocześnie warto przypomnieć, że zagnieżdżenie pozwala na wywoływanie bez przekazywania parametrów z funkcji nadrzędnej.



## Modyfikatory

### `assembler`

**Procedury/Funkcje** oznaczona przez `ASSEMBLER` mogą składać się tylko z bloku **ASM**. Kompilator nie dokonuje analizy składni takich bloków, traktuje je jak komentarz, ewentualne błędy zostaną wychwycone dopiero podczas asemblacji.

> **UWAGA:**  
> _Wymagane jest aby zachować stan rejestru `X` `CPU6502`, który używany jest do obsługi stosu programowego **MP**._

Kompilator dopuszcza dwie składnie bloku `ASM`, z klamrami { } jak dla komentarza i standardową bez klamer.

```delphi
ASM
  lda #10
  sta 712
END;
```

```delphi
ASM
{  lda #10
   sta 712
};
```

```delphi
procedure name; assembler;
asm
  lda #10
  sta 712
end;
```

```delphi
procedure name; assembler;
asm
{
  lda #10
  sta 712
};
end;
```

### `overload`

**Procedury/Funkcje** przeciążone rozpoznawane są na podstawie listy parametrów.

```delphi
procedure suma(var i: integer; a,b: integer); overload;
begin
  i := a+b;
end;

procedure suma(var i: integer; a,b,c: integer); overload;
begin
  i := a+b+c;
end;

function fsuma(a,b: word): cardinal; assembler; overload;
asm
{
  adw a b result
};
end;

function fsuma(a,b: real): real; overload;
begin
  Result := a+b;
end;
```


Przy przekazywaniu parametrów do **procedury/funkcji** należy pamiętać że **Mad Pascal** rozszerza typ obliczanych wyrażeń, dlatego jeśli zależy nam na określonym typie powinniśmy dokonać rzutowania.

```delphi
procedure tst(a,b: shortint); overload;
begin
 writeln('shortint');
end;

procedure tst(a,b: smallint); overload;
begin
 writeln('smallint');
end;

procedure tst(a,b: integer); overload;
begin
 writeln('integer');
end;
```

```delphi
dla a,b = shortint

tst(a,b)    -> wybierze shortint

tst(a+1,b)  -> wybierze integer

tst(smallint(a+1),b)  -> wybierze smallint
```


### `forward`

Jeżeli chcemy aby **procedura/funkcja** była zadeklarowana za miejscem jej pierwszego wywołania, należy użyć modyfikator `FORWARD`.

```delphi
procedure nazwa [(lista-parametrów-formalnych)]; forward;

...
...
...

procedure nazwa;
begin
end;
```

### `register`

Użycie modyfikatora `REGISTER` spowoduje, że trzy pierwsze parametry formalne **procedury/funkcji** będą umieszczone na stronie zerowej, w 32-bitowych rejestrach programowych, odpowiednio `EDX`, `ECX`, `EAX`.

    procedure nazwa (a,b,c: cardinal); register;
    // a = edx
    // b = ecx
    // c = eax
    
W przypadku funkcji, zmienna RESULT przechowująca wartość funkcji alokowana jest na stronie zerowej pod adresem `:STACKORIGIN-4` który odpowiada bezpośrednio zmiennej `:TMP`

    function nazwa (a,b,c: cardinal): cardinal; register;
    // a = edx
    // b = ecx
    // c = eax
    // Result = :STACKORIGIN-4 (:TMP)

Jeśli w ciele procedury/funkcji występują operacje mnożenia/dzielenia, albo operacje na tablicach dwu-wymiarowych wówczas rejestry `EDX`, `ECX`, `EAX` mogą ulec zniszczeniu co spowoduje "niespodziewane" rezultaty.


### `interrupt`

**Procedury/Funkcje** oznaczone przez `INTERRUPT` kompilator będzie kończył rozkazem `RTI` (standardowo `RTS`).

Niezależnie czy w programie wystąpi wywołanie takiej **procedury/funkcji** kompilator zawsze wygeneruje dla niej kod.

Na wejściu **procedury/funkcji** oznaczonej przez `INTERRUPT` programista musi zadbać o zachowanie rejestrów **CPU** `A` `X` `Y`, na wyjściu o przywrócenie stanu takich rejestrów, kompilator ogranicza się tylko do wstawienia końcowego rozkazu `RTI`.

Kompilator zgłosi błąd jeśli w takiej procedurze/funkcji wystąpią rozkazy assemblera odwołujące się do zmiennych `:BP`, `:BP2` lub `:STACKORIGIN`.

```delphi
procedure dli; interrupt; assembler;
asm
   pha

    lda #$c8
    sta wsync
    sta $d01a

    pla
end;             // rozkaz RTI zostanie wstawiony automatycznie
```

### `keep`

Użycie modyfikator `KEEP` spowoduje że tak oznaczona **procedura/funkcja** zostanie zawsze skompilowana niezależnie czy wystąpiło, czy nie wystąpiło odwołanie w programie do takiej **procedury/funkcji**.

### `pascal`

Użycie modyfikatora `PASCAL` spowoduje, że **procedura/funkcja** będzie traktowana jako rekurencyjna. Standardowo kompilator automatycznie wykrywa rekurencję, ale mogą zdarzyć się sytuacje dla których będzie to niemożliwe.
Przykład [samples/math/evaluate.pas](https://github.com/tebe6502/Mad-Pascal/blob/master/samples/a8/math/evaluate.pas)

### `stdcall`

Użycie modyfikatora `STDCALL` spowoduje wymuszenie przekazywania parametrów do procedury/funkcji poprzez stos programowy. Domyślnie kompilator stara się przekazywać parametry przez zmienne, bez udziału stosu programowego.

### `inline`

Procedura, funkcja zostaje zamieniona na makro **Mad Assemblera**, pozbywamy się wywołań z udziałem rozkazu `JSR`.

Nie ma możliwości używania rekurencji dla takich **procedur/funkcji**.

### `external`

Modyfikator `EXTERNAL` informuje kompilator że **procedura/funkcja** zostanie dolinkowana

`{$LINK filename}`

na etapie assemblacji.

Przykład kilku procedur, które zostaną dolinkowane do programu w Pascalu (procedura z 1 argumentem typu BYTE jest wyjątkiem, taki argument przekazywany jest przez rejestr akumulatora)

```delphi

.public proc1, proc2, proc3

.reloc

.proc proc1 (.byte a) .reg

 rts
.endp


.proc proc2 (.word tmp) .var

 rts

tmp dta a(0)

.endp


.proc proc3 (.byte x,y,z) .var

 rts

x brk
y brk
z brk
.endp
```

Przykład wykorzystania w Pascalu

```delphi
 procedure proc1 (a: byte); external;
 procedure proc2 (a: word); external;
 procedure proc3 (a,b,c: byte); external;
 
 {$link filename.obx}
```