#

## ASM

Bloki assemblera nie są przez kompilator weryfikowane pod względem składni, dokonuje tego dopiero **Mad Assembler**.

> UWAGA: Wymagane jest aby zachować stan rejestru `X` `CPU6502`, który używany jest do obsługi stosu programowego **MP**.

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
