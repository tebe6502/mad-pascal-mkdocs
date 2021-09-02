#

## [Procedury](https://www.freepascal.org/docs-html/ref/refch14.html#x173-19500014)

**MP** pozwala na przekazanie do procedury maksymalnie **8** parametrów. Są dostępne trzy sposoby przekazywania parametrów: przez wartość, stałą `CONST` i zmienną `VAR`.

Dostępne modyfikatory procedur: `OVERLOAD` `ASSEMBLER` `FORWARD` `REGISTER` `INTERRUPT` `PASCAL` `STDCALL` `INLINE`.

Możliwa jest rekurencja procedur, pod warunkiem że parametry procedury będą przekazywane przez wartość, będą typu prostego - porządkowego. Typ rekordowy, wskaźnikowy nie będzie właściwie alokowany w pamięci.

Parametry procedury odczytywane są i wartościowane od lewej do prawej strony (w `FPC` domyślnie od prawej do lewej).
Jeśli jest to możliwe kompilator przekazuje parametry do procedury poprzez zmienne z pominięciem stosu programowego.

## [Funkcje](https://www.freepascal.org/docs-html/ref/refch14.html#x173-19500014)

**MP** pozwala na przekazanie do funkcji maksymalnie **8** parametrów. Są dostępne trzy sposoby przekazywania parametrów: przez wartość, stałą `CONST` i zmienną `VAR`. Wynik funkcji zwracamy przypisując go do nazwy funkcji lub korzystając z automatycznie deklarowanej zmiennej `RESULT`, np.:

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

Dostępne modyfikatory funkcji: `OVERLOAD` `ASSEMBLER` `FORWARD` `REGISTER` `INTERRUPT` `PASCAL` `STDCALL`, `INLINE`

- `INTERRUPT` nie jest zalecane dla funkcji

Możliwa jest rekurencja funkcji, pod warunkiem, że parametry funkcji będą przekazywane przez wartość, będą typu prostego - porządkowego. Typ rekordowy, wskaźnikowy nie będzie właściwie alokowany w pamięci.

Parametry funkcji odczytywane są i wartościowane od lewej do prawej strony (w `FPC` domyślnie od prawej do lewej).
Jeśli jest to możliwe kompilator przekazuje parametry do funkcji poprzez zmienne z pominięciem stosu programowego.

## Modyfikatory

### `assembler`

**Procedury/Funkcje** oznaczona przez `ASSEMBLER` mogą składać się tylko z bloku **ASM**. Kompilator nie dokonuje analizy składni takich bloków, traktuje je jak komentarz, ewentualne błędy zostaną wychwycone dopiero podczas asemblacji.

!!! Wymagane jest aby zachować stan rejestru X, który używany jest do obsługi stosu programowego MP. !!!

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

### `interrupt`

**Procedury/Funkcje** oznaczone przez `INTERRUPT` kompilator będzie kończył rozkazem `RTI` (standardowo `RTS`).
Niezależnie czy w programie wystąpi wywołanie takiej **procedury/funkcji** kompilator zawsze wygeneruje dla niej kod.
Na wejściu **procedury/funkcji** oznaczonej przez `INTERRUPT` programista musi zadbać o zachowanie rejestrów **CPU** `A` `X` `Y`, na wyjściu o przywrócenie stanu takich rejestrów, kompilator ogranicza się tylko do wstawienia końcowego rozkazu `RTI`.
Kompilator zgłosi błąd jeśli w takiej procedurze/funkcji wystąpią rozkazy odwołujące się do zmiennych `:BP`, `:BP2` lub `:STACKORIGIN`.

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

### `pascal`

Użycie modyfikatora `PASCAL` spowoduje, że **procedura/funkcja** będzie traktowana jako rekurencyjna. Standardowo kompilator automatycznie wykrywa rekurencję, ale mogą zdarzyć się sytuacje dla których będzie to niemożliwe.
Przykład [samples/math/evaluate.pas](https://github.com/tebe6502/Mad-Pascal/blob/master/samples/math/evaluate.pas)

### `stdcall`

Użycie modyfikatora `STDCALL` spowoduje wymuszenie przekazywania parametrów do procedury/funkcji poprzez stos programowy. Domyślnie kompilator stara się przekazywać parametry przez zmienne, bez udziału stosu programowego.

### `inline`

Procedura, funkcja zostaje zamieniona na makro.


