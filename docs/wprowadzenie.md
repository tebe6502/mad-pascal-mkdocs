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
    * `Char` `String` `PChar`
    * `Integer` `SmallInt` `ShortInt`
    * `Pointer` `File`
    * `ShortReal` `Real` (fixed-point)
    * `Single` (IEEE-754) [Float]
* One-dimensional and Two-dimensional arrays (with zero lower bound) of any primitive type. Arrays are treated as pointers to their origins (like in C) and can be passed to subroutines as parameters
* Predefined type string `[N]` which is equivalent to `array [0..N] of Char`
* `Type` aliases.
* `Records`
* `Objects`
* Separate program modules
* Recursion

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

## Linki

1. [Free Pascal Reference Guide](http://www.freepascal.org/docs-html/ref/ref.html#refch14.html)
2. [MadPascal AtariArea forum (PL)](http://www.atari.org.pl/forum/viewtopic.php?id=13373)
3. [MadPascal AtariAge forum (ENG)](http://atariage.com/forums/topic/240919-mad-pascal/)
4. [MadPascal examples](http://atariage.com/forums/topic/243658-mad-pascal-examples/)
5. [Atari XE/XL Pascal Compilers](https://atariwiki.org/wiki/Wiki.jsp?page=Pascal)
