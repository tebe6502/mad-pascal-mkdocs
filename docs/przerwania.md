#

## VBL, DLI

Do obsługi przerwań [**VBLD**](https://atariwiki.org/wiki/Wiki.jsp?page=VVBLKD), [**VBLI**](https://atariwiki.org/wiki/Wiki.jsp?page=VVBLKI), [**DLI**](https://atariwiki.org/wiki/Wiki.jsp?page=VDSLST) dedykowane są dwie procedury `GetIntVec` oraz `SetIntVec`. Do prawidłowego działania wymagana jest obecność OS-a (wyłączenie ROM tylko przez [$DEFINE ROMOFF](../skladnia/#romoff))

### GetIntVec

    GetIntVec(iVBLI, pointer);	// pobranie adresu programu obsługi przerwań VBLI ($0222)
    GetIntVec(iVBLD, pointer);	// pobranie adresu programu obsługi przerwań VBLD ($0224)
    GetIntVec(iDLI, pointer);	// pobranie adresu programu obsługi przerwań DLI ($0200)

```delphi
var oldVBL: pointer;

begin

GetIntVec(iVBL, oldVBL);

end.
```

### SetIntVec

    SetIntVec(iVBLI, pointer);	// ustanowienie adresu programu obsługi przerwań VBLI ($0222)
    SetIntVec(iVBLD, pointer);	// ustanowienie adresu programu obsługi przerwań VBLD ($0224)
    SetIntVec(iDLI, pointer);	// ustanowienie adresu programu obsługi przerwań DLI ($0200)

```delphi
procedure newVBL; interrupt; assembler;
asm

 jmp xitvbv

end;


begin

SetIntVec(iVBL, @newVBL);

end.
```

Przerwanie **VBLD** (VBL opóźnione) kończymy skokiem pod adres `XITVBV` ($E462) co spowoduje przywrócenie wartości rejestrów `A` `X` `Y` **CPU6502**.

Jeśli wyłączyliśmy `ROM` przez `{$define romoff}` i korzystamy z procedur umieszczonych w pamięci `$C000..$FFFF` musimy zadbać o odpowiednie ustawienie `PORTB`.

```delphi
procedure newVBL; interrupt; assembler;
asm

 dec portb
 
 jsr user_proc_c000_ffff
 
 inc portb

 jmp xitvbv

end;
```


```delphi
procedure newVBL; interrupt; assembler;
asm

 jmp sysvbv

end;


begin

SetIntVec(iVBLI, @newVBL);

end.
```

Przerwanie **VBLDI** (VBL natychmiastowe) kończymy skokiem pod adres `SYSVBV` ($E45F) co spowoduje kontynuację obsługi przerwania VBL.

Jeśli wyłączyliśmy `ROM` przez `{$define romoff}` i korzystamy z procedur umieszczonych w pamięci `$C000..$FFFF` musimy zadbać o odpowiednie ustawienie `PORTB`.

```delphi
procedure newVBL; interrupt; assembler;
asm

 dec portb
 
 jsr user_proc_c000_ffff
 
 inc portb

 jmp sysvbv

end;
```

## IRQ - TIMER1, TIMER2, TIMER4

Do obsługi przerwań **IRQ** - **TIMER1**, **TIMER2**, **TIMER4** dedykowane są dwie procedury `GetIntVec` oraz `SetIntVec`. Do prawidłowego działania wymagana jest obecność OS-a (wyłączenie ROM tylko przez [$DEFINE ROMOFF](../skladnia/#romoff))

### GetIntVec

    GetIntVec(iTIM1, pointer);	// pobranie adresu programu obsługi przerwań TIMER 1 ($0210)
    GetIntVec(iTIM2, pointer);	// pobranie adresu programu obsługi przerwań TIMER 2 ($0212)
    GetIntVec(iTIM4, pointer);	// pobranie adresu programu obsługi przerwań TIMER 4 ($0214)

```delphi
var oldIRQ: pointer;

begin

GetIntVec(iTIM4, oldIRQ);

end.
```

### SetIntVec

    SetIntVec(iTIM1, pointer);	// ustanowienie adresu programu obsługi przerwań TIMER 1 ($0210)
    SetIntVec(iTIM2, pointer);	// ustanowienie adresu programu obsługi przerwań TIMER 2 ($0212)
    SetIntVec(iTIM4, pointer);	// ustanowienie adresu programu obsługi przerwań TIMER 4 ($0214)

```delphi
procedure irq; assembler; interrupt;
asm

 pla

end;

begin

SetIntVec(iTIM4, @irq, 0, 28);

repeat until keypressed;

SetIntVec(iTIM4, oldIRQ);

end.
```

System wykonując skok do procedury obsługi przerwania odkłada wcześniej zawartość akumulatora na stos, należy o tym pamiętać i kończyć obsługę przerwania przez `PLA`.

Do uruchomienia nowego przerwania **IRQ** wymagane jest podanie dodatkowych parametrów, takich jak wybór zegara bazowego `clock_base = [0,1]` oraz częstotliwość `rate = [6.255]`. Wartości `rate` mniejsze
od `6` spowodują mocne spowolnienie systemu aż do ewentualnego jego zawieszenia.

    SetIntVec(iTIM1, pointer, clock_base, rate);
    SetIntVec(iTIM2, pointer, clock_base, rate);
    SetIntVec(iTIM4, pointer, clock_base, rate);
