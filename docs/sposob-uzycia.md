#

## Przełączniki kompilatora

```bash
Syntax: mp source [switches]

-diag           tryb diagnostyczny
-define:symbol  definiowanie symbolu
-ipath:<x>      dodatkowa ścieżka poszukiwań
-target:<x>     docelowy system: a8 (domyślnie), c64
-code:address   adres uruchomienia programu
-data:address   adres pamięci dla zmiennych, tablic
-stack:address  adres pamięci dla stosu programowego (64 bajty)
-zpage:address  adres na stronie zerowej dla zmiennych (26 bajty)
```

Użycie przełącznika `-diag` powoduje wygenerowanie dodatkowego pliku z informacją o wszystkich użytych zmiennych, procedurach, funkcjach.

Domyślnym rozszerzeniem pliku wynikowego jest `*.A65`, plik taki assemblujemy z użyciem [Mad-Assemblera](https://github.com/tebe6502/Mad-Assembler), dodatkowo ustawiamy `-i:base`, `base` znajduje się w katalogu głównym **MP**), np.:

    mads source.a65 -x -i:base

Przełącznik `-x` **Exclude unreferenced procedures** jest bardzo istotny, pozwoli wygenerować najkrótszy kod wynikowy dla 6502, wszystkie procedury `.PROC`
do których nie wystąpiło odwołanie zostaną pominięte w procesie assemblacji.

## Kody wyjścia

    3 = bad parameters, compiling not started
    2 = error occured
    0 = no errors

Komunikaty ostrzeżenia nie powodują zmiany wartości kodu wyjścia.
