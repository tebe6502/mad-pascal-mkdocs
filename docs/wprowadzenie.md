#

## Wstęp

**Mad-Pascal** (MP) jest 32-bitowym kompilatorem **Turbo Pascala** dla **Atari XE/XL**. W założeniu jest kompatybilny z **Free Pascal Compilator** (FPC) (przełącznik `-MDelphi` powinien być aktywny), co oznacza możliwość otrzymania kodu uruchomieniowego dla **XE/XL**, **PC** i każdej innej platformy dla której istnieje **FPC**. **MP** nie jest portem **FPC**, został napisany na podstawie kompilatorów **SUB-Pascal** (2009), **XD-Pascal** (2010), których autorem jest [Vasiliy Tereshkov](mailto:vtereshkov@mail.ru).

Program, który zadziała na Atari może mieć problem na **PC** jeżeli np. nie zaincjowaliśmy wskaźników adresem zmiennej, będziemy próbowali zapisywać coś pod adresem `$0000` (błąd ochrony pamięci). Mocną stroną **MP** jest szybka i wygodna możliwość dołączania wstawek assemblerowych. Program z wstawkami ASM nie będzie działał na innej platformie niż XE/XL. MP wykorzystuje 64KB pamięci podstawowej, korzystanie z pamięci rozszerzonej udostępnia `TMemoryStream`.

Alokacja zmiennych jest statyczna, nie ma dynamicznego zarządzania pamięcią. Parametry przekazywane do funkcji i procedur są przez wartość, zmienną lub stałą.

Dostępne są:

* `If` `Case` `For` `While` `Repeat` statements
* Compound statements
* `Label` `Goto` statements
* Arithmetic and boolean operators
* Procedures and functions with up to 8 parameters. Returned value of a function is assigned to a predefined `RESULT` variable
* Static local variables
* Primitive data types, all types except the `ShortReal`/`Real` type are compatible. Pointers are dereferenced as pointers to `Word`:
    * `Cardinal` `Word` `Byte` `Boolean`
    * `String` `PChar` `Char`
    * `Integer` `SmallInt` `ShortInt`
    * `Pointer` `File` `Text`
    * `ShortReal` `Real` [`fixed-point`](https://en.wikipedia.org/wiki/Fixed-point_arithmetic)
    * `Float` [`Single`](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)
    * [`Float16`](https://en.wikipedia.org/wiki/Half-precision_floating-point_format)
* One-dimensional and Two-dimensional arrays (with zero lower bound) of any primitive type. Arrays are treated as pointers to their origins (like in C) and can be passed to subroutines as parameters
* Predefined type string `[N]` which is equivalent to `array [0..N] of Char`
* `Type` aliases.
* `Records`
* `Objects`
* Separate program modules
* Recursion

## Układ katalogów

W katalogu z **MP** wymagana jest obecność odpowiednich plików i podkatalogów:

```
  mp\
  mp.exe
     base\
     rtl_default.asm
     rtl6502_a8.asm
     rtl6502_c4p.asm
     rtl6502_c64.asm
     rtl6502_neo.asm
     rtl6502_raw.asm
          atari\
          c4p\
          c64\
          common\
          neo\
          raw\
          runtime\
     lib\
     aplib.pas
     atari.pas
     blowfish.pas
     c64.pas
     ...
     src\
         targets\
         crt.inc
         graph.inc
         system.inc
```

## Kompilacja

Aby skompilować źródło **Mad-Pascala**, można użyć kompilatora z **Delphi**, jeśli ktoś ma akurat zainstalowane środowisko **Delphi 7.0** lub nowsze.

Innym sposobem, bardziej multi platformowym jest użycie kompilatora z pakietu **Free Pascal Compiler** (FPC), który można pobrać ze strony [freepascal.org](http://www.freepascal.org/).

Uruchamiamy instalator, wybieramy katalog w którym zostanie zainstalowany FP. Ważne jest aby nie używać w nazwie katalogu znaku wykrzyknika `!` czy innych nie standardowych znaków. Jeśli nie uda nam się skompilować żadnego pliku, najpewniej winna jest nie standardowa nazwa ścieżki. Linia komend uruchamiająca kompilację może wyglądać następująco (wielkość liter w nazwach parametrów ma znaczenie):

    fpc -Mdelphi -v -O3 mp.pas

* `-Mdelphi` pozwala kompilować plik w formacie Delphi
* `-v` wyświetla wszystkie komunikaty błędów i ostrzeżeń
* `-O3` dokonuje optymalizacji kodu

## Biblioteki

* [LIB](http://mads.atari8.info/library/doc/index.html)
* [BLIBS](https://bocianu.atari.pl/blog/blibs)
* [WLIBS](https://github.com/Ripjetski6502/A8MadPascalLibrary)

## Linki

1. [Free Pascal Reference Guide](http://www.freepascal.org/docs-html/ref/ref.html#refch14.html)
2. [Mad Pascal - AtariArea (PL)](http://www.atari.org.pl/forum/viewtopic.php?id=13373)
3. [Mad Pascal - AtariOnline (PL)](https://atarionline.pl/forum/search.php?PostBackAction=Search&Keywords=mad+pascal&Type=Topics&btnSubmit=Szukaj)
4. [Mad Pascal - AtariAge (ENG)](http://atariage.com/forums/topic/240919-mad-pascal/)
5. [Mad Pascal examples - AtariAge (ENG)](http://atariage.com/forums/topic/243658-mad-pascal-examples/)
6. [Games in Mad Pascal - AtariAge (ENG)](https://forums.atariage.com/topic/249968-games-in-mad-pascal/)
7. [Atari XE/XL Pascal Compilers](https://atariwiki.org/wiki/Wiki.jsp?page=Pascal)s
