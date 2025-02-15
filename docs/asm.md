#

## ASM

Bloki assemblera `ASM` nie są przez kompilator weryfikowane pod względem składni, dokonuje tego dopiero **Mad Assembler**.

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

## $LINK

```Delphi
{$link filename}
```

Dyrektywa kompilatora `{$link filename}` pozwala dołączyć plik relokowalny z **Mad Assembler**-a do kompilowanego programu **MP**.

```Delphi
	.reloc

.extrn edx .dword

.extrn	print .proc (.dword edx) .var

.public	prc


.proc	prc (.dword a .dword b .dword c) .var

.var a,b,c .dword

	print a
	print b
	print c

	rts

.endp
```

W powyższym przykładzie korzystamy z procedury `PRINT`, która jest zdefiniowana w **MP**.

```Delphi
uses crt;


procedure prc(a,b,c: integer); external;


procedure print(value: dword); keep; register;
begin

 writeln(value);

end;


{$link test.obx}	// link PRC procedure


begin

 prc(11, 347, 321785);

 repeat until keypressed;

end.
```

Z poziomu assemblera mamy dostęp do procedur **MP** ale tylko oznaczonych modyfikatorem `REGISTER`, czyli takich których parametry przekazywane są przez programowe rejestry `EDX`, `ECX`, `EAX` (jesteśmy ograniczeni do maksymalnie trzech parametrów).

Modyfikator `KEEP` wymusza pozostawienie procedury w kompilowanym kodzie niezależnie od tego czy wystąpiło jej użycie czy nie (normalnie procedury/funkcje które nie są używane są eliminowane).

**MP** natomiast ma dostęp do procedur z linkowanego pliku assemblera, których parametry przekazywane są przez zmienne, modyfikator `.VAR`.

```Delphi
.proc	prc (.dword a .dword b .dword c) .var
```

W programie relokowalnym **Mad Assembler**-a potrzebujemy dodatkowej deklaracji zewnętrzych symboli `EDX`, `ECX`, `EAX`.

```Delphi
.extrn edx, ecx, eax .dword
```

Sama procedura **MP** do której chcemy mieć dostęp z poziomu assemblera jest deklarowana jako procedura zewnętrzna z parametrami oznaczającymi rejestry programowe `EDX`, `ECX`, `EAX`.

Więcej informacji na temat modyfikatora `REGISTER` i kolejności alokacji parametrów w rejestrach programowych w rozdziale [Procedury i funkcje](../procedury-funkcje/#register).

```Delphi
.extrn	print .proc (.dword edx) .var
```

Dostęp do procedury `PRC` z poziomu **MP** zapewni nam jej upublicznienie przez dyrektywę assemblera `.PUBLIC`.

```Delphi
.public	prc
```

Po assemblacji naszego przykładowego relokowalnego programu assemblera `TEST.ASM` otrzymujemy plik `TEST.OBX`, który możemy połączyć z programem **MP** dyrektywą `{$LINK}`.

```Delphi
{$link test.obx}
```